---
title: "Protecting rubygems.org from the outside in: DoS prevention and compromised passwords"
url: "https://blog.rubygems.org/2026/04/09/protecting-rubygems-from-the-outside-in.html"
date: "2026-04-09T00:00:00+00:00"
author: "Colby Swandale"
feed_url: "https://blog.rubygems.org/atom.xml"
---
<p>Every gem published to <a href="https://rubygems.org">rubygems.org</a> ends up running on someone’s computer. It’s up to <a href="https://rubygems.org">rubygems.org</a> to ensure that each gem contains what it claims, that its metadata is well-formed, and that the person who pushed it is who they say they are.</p>

<p>We’ve been chipping away at that. Over the past few months, we shipped two changes that tighten <a href="https://rubygems.org">rubygems.org</a>’s defences at very different layers: stronger validation of gem contents at push time, and integration with Have I Been Pwned to catch compromised passwords at login.</p>

<h2 id="what-rubygemsorg-checks-when-you-gem-push">What <a href="https://rubygems.org">rubygems.org</a> checks when you gem push</h2>

<p>A RubyGem is actually just a regular tar file, which contains 3 sections: the code, metadata, and checksums, which you can inspect for yourself.</p>

<div class="language-bash highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="nv">$ </span>gem fetch rails
Fetching rails-8.1.3.gem
Downloaded rails-8.1.3

<span class="nv">$ </span><span class="nb">tar</span> <span class="nt">-xvf</span> rails-8.1.3.gem
x metadata.gz
x data.tar.gz
x checksums.yaml.gz
</code></pre></div></div>

<p><a href="https://rubygems.org">rubygems.org</a> closely inspects all 3 of these files when a gem is published, but the ones we’re looking at are the <code class="language-plaintext highlighter-rouge">metadata</code> and <code class="language-plaintext highlighter-rouge">checksums.yaml</code>.</p>

<p>The <code class="language-plaintext highlighter-rouge">checksums.yaml</code> certifies the integrity hash of the <code class="language-plaintext highlighter-rouge">data.tar.gz</code> and <code class="language-plaintext highlighter-rouge">metadata.gz</code> with a sha256 after <code class="language-plaintext highlighter-rouge">gem build</code>. If someone tampers with the code directly, the checksums won’t match and <a href="https://rubygems.org">rubygems.org</a> rejects the push immediately. Checksums are the easy part.</p>

<p><code class="language-plaintext highlighter-rouge">metadata.gz</code> has the serialised YAML of the gem metadata, generated from the gemspec during <code class="language-plaintext highlighter-rouge">gem build</code>.</p>

<div class="language-yaml highlighter-rouge"><div class="highlight"><pre class="highlight"><code> <span class="s">--- !ruby/object:Gem::Specification</span>
<span class="na">name</span><span class="pi">:</span> <span class="s">rails</span>
<span class="na">version</span><span class="pi">:</span> <span class="kt">!ruby/object:Gem::Version</span>
  <span class="na">version</span><span class="pi">:</span> <span class="s">8.1.3</span>
<span class="na">platform</span><span class="pi">:</span> <span class="s">ruby</span>
<span class="na">authors</span><span class="pi">:</span>
<span class="pi">-</span> <span class="s">David Heinemeier Hansson</span>
<span class="na">bindir</span><span class="pi">:</span> <span class="s">bin</span>
<span class="na">cert_chain</span><span class="pi">:</span> <span class="pi">[]</span>
<span class="na">date</span><span class="pi">:</span> <span class="s">1980-01-02 00:00:00.000000000 Z</span>
<span class="na">dependencies</span><span class="pi">:</span>
<span class="pi">-</span> <span class="kt">!ruby/object:Gem::Dependency</span>
  <span class="na">name</span><span class="pi">:</span> <span class="s">activesupport</span>
  <span class="na">requirement</span><span class="pi">:</span> <span class="kt">!ruby/object:Gem::Requirement</span>
    <span class="na">requirements</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="pi">-</span> <span class="s1">'</span><span class="s">='</span>
      <span class="pi">-</span> <span class="kt">!ruby/object:Gem::Version</span>
        <span class="na">version</span><span class="pi">:</span> <span class="s">8.1.3</span>
  <span class="na">type</span><span class="pi">:</span> <span class="s">:runtime</span>
  <span class="na">prerelease</span><span class="pi">:</span> <span class="kc">false</span>
  <span class="na">version_requirements</span><span class="pi">:</span> <span class="kt">!ruby/object:Gem::Requirement</span>
    <span class="na">requirements</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="pi">-</span> <span class="s1">'</span><span class="s">='</span>
      <span class="pi">-</span> <span class="kt">!ruby/object:Gem::Version</span>
        <span class="na">version</span><span class="pi">:</span> <span class="s">8.1.3</span>
<span class="nn">...</span>
</code></pre></div></div>

<p>When a gem is pushed, <a href="https://rubygems.org">rubygems.org</a> deserialises the YAML and reconstructs a <code class="language-plaintext highlighter-rouge">Gem::Specification</code> object from it. It then validates the result, checking that the name and version are well-formed, that the declared dependencies are valid, that the person pushing is authorised. This is where gem validation gets complex.</p>

<h2 id="exploiting-the-validation-process">Exploiting the validation process</h2>

<p>This process of reconstructing the gemspec YAML into a <code class="language-plaintext highlighter-rouge">Gem::Specification</code> object invites a class of exploitation called <a href="https://owasp.org/www-community/vulnerabilities/Insecure_Deserialization">insecure deserialisation</a> that would allow a crafted YAML to attack <a href="https://rubygems.org">rubygems.org</a>.</p>

<p>This isn’t a theoretical concern. In 2017, a <a href="https://blog.rubygems.org/2017/10/09/unsafe-object-deserialization-vulnerability.html">security researcher discovered</a> that rubygems.org was using a bare <code class="language-plaintext highlighter-rouge">YAML.load</code> to deserialise checksums inside gem files, a vulnerability that had potentially been present since 2012. The team patched it within hours by switching to <code class="language-plaintext highlighter-rouge">YAML.safe_load</code>, which restricts which Ruby objects can be instantiated from a document. But that only narrowed the problem. Even with a precise allowlist of classes and objects, malicious gems could still exploit the deserialisation process to exhaust memory or CPU before any validation even ran, causing rubygems.org servers to stop working.</p>

<h2 id="validating-gems-without-gemspecification">Validating gems without <code class="language-plaintext highlighter-rouge">Gem::Specification</code></h2>

<p>The fix was to stop trusting the YAML to tell <a href="https://rubygems.org">rubygems.org</a> what to do with itself.</p>

<p>This was largely <a href="https://bsky.app/profile/tenderlove.dev">Aaron Patterson’s</a> (tenderlove) work. He designed and built the AST-based approach from the ground up. Rather than handing the document to Ruby and letting it materialise objects, we traverse the parsed tree ourselves and extract only the values we expect to find. The YAML never gets to decide what gets instantiated. We also validate the structure against a schema derived from the real thing: Aaron audited all 180,000 gems published on <a href="https://rubygems.org">rubygems.org</a> and built a tool to validate them against it. Some very old gems turned up edge cases we deliberately chose not to handle. If those gems were pushed today, they’d be rejected, but these gems that haven’t seen a new version in years almost certainly never will be. His contribution here is greatly appreciated.</p>

<p>The result is that an entire class of exploitation (using malformed metadata to attack the push endpoint itself) is no longer possible. The attack surface doesn’t exist anymore.</p>

<h2 id="compromised-passwords-and-the-supply-chain">Compromised passwords and the supply chain</h2>

<p>Gem validation protects <a href="https://rubygems.org">rubygems.org</a> from what gets pushed. But there’s a separate persistent threat: who’s doing the pushing.</p>

<p>Package registries are high-value targets for credential stuffing. If an attacker gets hold of a developer’s reused password from an unrelated breach, they can log in as that developer and push a malicious version of a legitimate gem. The code is signed by a trusted account. The checksums match. Everything looks right, because as far as <a href="https://rubygems.org">rubygems.org</a> can tell, it is.</p>

<p><a href="https://haveibeenpwned.com">Have I Been Pwned</a> (HIBP) is a service run by security researcher <a href="https://www.troyhunt.com">Troy Hunt</a> that tracks passwords exposed in known data breaches. At the time of writing, it contains over 10 billion compromised passwords. <a href="https://rubygems.org">rubygems.org</a> now checks against it at login, registration and password resets.</p>

<h2 id="checking-passwords-without-exposing-them">Checking passwords without exposing them</h2>

<p>The obvious concern with checking your password against a third-party service is privacy. <a href="https://rubygems.org">rubygems.org</a> never sends your password, or even a full hash of it, to HIBP.</p>

<p>Instead, it uses <a href="https://www.troyhunt.com/understanding-have-i-been-pwneds-use-of-sha-1-and-k-anonymity/">HIBP’s k-anonymity model</a>. When you log in, <a href="https://rubygems.org">rubygems.org</a> computes a SHA-1 hash of your password and sends only the first 5 characters of that hash to the HIBP API. HIBP returns a list of all hashed passwords in its database that start with those 5 characters. <a href="https://rubygems.org">rubygems.org</a> then checks that list locally. Your full password hash never leaves our servers.</p>

<p>If your password appears in the results, <a href="https://rubygems.org">rubygems.org</a> blocks the session and shows a warning explaining your password has been found in a known breach. You’ll need to reset your password before you can log in again.</p>

<p>Since shipping, it’s detected 1,166 accounts with compromised passwords. Because rubygems.org hashes passwords with bcrypt, we’ve never been able to inspect the strength of passwords in the database directly. This is the first real window into how widespread the problem is, and a way to start course correcting it.</p>

<h2 id="shipping-the-work">Shipping the work</h2>

<p><a href="https://rubygems.org">rubygems.org</a> serves almost a billion gem downloads every single day. Every Ruby application, from side projects to the infrastructure powering large parts of the internet, depends on the integrity of what we distribute.</p>

<p>These two changes address the supply chain at different layers: one at the moment a gem is built and pushed, the other at the moment a person logs in. Neither is glamorous. Validating YAML ASTs and hashing password prefixes don’t ship in a splash announcement. But this is the work: closing specific, real attack vectors before someone finds them for you. If you want to follow along or get involved, everything happens in the open at <a href="https://github.com/rubygems/rubygems.org">github.com/rubygems/rubygems.org</a>.</p>
