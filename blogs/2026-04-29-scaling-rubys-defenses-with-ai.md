---
title: "Scaling Ruby's defenses with AI"
url: "https://blog.rubygems.org/2026/04/29/scaling-rubys-defenses-with-ai.html"
date: "2026-04-29T00:00:00+00:00"
author: "Colby Swandale"
feed_url: "https://blog.rubygems.org/atom.xml"
---
<p>On April 23rd, we submitted a vulnerability report to the <a href="https://github.com/sparklemotion/nokogiri">Nokogiri</a> maintainers. It was the first one our team has filed using AI-assisted scanning. The maintainers accepted the report and published it as <a href="https://github.com/sparklemotion/nokogiri/security/advisories/GHSA-c4rq-3m3g-8wgx">GHSA-c4rq-3m3g-8wgx</a>.</p>

<p>The same week, news broke that Mythos, Anthropic’s most capable security model, had been accessed by unauthorized users through a third-party vendor. According to Anthropic, Mythos has identified thousands of zero-day vulnerabilities across every major operating system and web browser, <a href="https://red.anthropic.com/2026/mythos-preview/">including a 17-year-old remote code execution flaw in FreeBSD and a 27-year-old bug in OpenBSD</a>. Two stories on the same shift, one from each side of it. The capability gap between attackers and defenders just widened, and most open source ecosystems have nothing to close it with.</p>

<p>Anthropic is bringing some open source maintainers into <a href="https://www.anthropic.com/glasswing">Project Glasswing</a>. Ruby is on the list, and agreements signed, but the access is not live yet. We cannot afford to be on the wrong side of that gap.</p>

<p>We have been working on the defender side. <a href="https://rubygems.org">RubyGems</a> hosts roughly 190,000 gems, and you cannot audit them all. The <a href="https://openssf.org/projects/criticality-score/">OpenSSF Criticality Score</a> lets us focus on the gems whose compromise would cascade through the rest of the ecosystem. We’re looking at those first.</p>

<p>We are using Claude Opus 4.7 to surface candidate vulnerabilities. A human reviewer triages, verifies, and writes up every finding before anything reaches a maintainer. None of this work happens without backing. <a href="https://alpha-omega.dev/">Alpha Omega</a>, a project of the <a href="https://openssf.org">OpenSSF</a> at the Linux Foundation, is <a href="https://www.linuxfoundation.org/press/linux-foundation-announces-12.5-million-in-grant-funding-from-leading-organizations-to-advance-open-source-security">sponsoring this work</a>. Anthropic is providing the model access we need to operate at the scale it needs.</p>

<p>The bug we found in Nokogiri is a regex backtracking pathology in the CSS tokenizer. A short, unterminated attribute selector could hang the Ruby process indefinitely because the tokenizer’s regex tries to interpret each escape sequence two different ways and explores an exponential number of possibilities before giving up. Every public Nokogiri CSS entry point routes through this tokenizer. Most large consumers (Rails, Capybara, Loofah) pass developer-written selectors and were unaffected. But any application that lets user input flow into a CSS selector (scrapers, feed readers) was exposed to an unauthenticated denial-of-service via a payload small enough to fit in a request parameter.</p>

<p>ReDoS bugs are a sweet spot for model-assisted finding. They are hard to spot by reading code and easy to verify by running them. Opus 4.7 flagged the ambiguous STRING rule in the CSS tokenizer and proposed a payload to exercise it: an unterminated attribute selector followed by a run of <code class="language-plaintext highlighter-rouge">\a</code> escape sequences. I ran it. Parsing took 6ms at fifteen escape sequences and timed out past five seconds at twenty-four. Each added escape roughly quadrupled the runtime, which is what catastrophic backtracking looks like. I wrote up the report. The Nokogiri maintainers accepted it, patched the bug, and published the advisory. The fix is in.</p>

<p>Open source maintainers are already drowning in AI-generated security reports that don’t hold up. Each one wastes a maintainer’s day and makes the next legitimate report harder to act on. We are not going to be part of that.</p>

<p>Opus 4.7 is the most capable model we have access to right now, and it produced a real advisory in one of the most widely used gems in the ecosystem. We are working with Anthropic to gain access to Mythos through Project Glasswing. We did not need to wait for it to find this bug, and we will not wait to find the next one.</p>

<p><a href="https://rubycentral.org">RubyCentral</a> is hiring a small team of security engineers to scale this work. The job is to run AI-assisted reviews against the most critical gems on rubygems.org, verify findings, and earn the kind of relationship with maintainers where an advisory from us is taken seriously and acted on quickly. If you have done open source security work in any ecosystem and want to do it at scale, we would like to hear from you. Please reach out to <a href="mailto:oss@rubycentral.org">oss@rubycentral.org</a>.</p>

<p>We submitted our first report on April 23rd. There are 190,000 more gems to look at.</p>

<blockquote>
  <p><strong>Update, 30 April 2026:</strong> An earlier version of this post said <em>“Ruby is not in yet”</em> in reference to Project Glasswing. We have been invited into the program, but the access is not live yet. The line has been clarified to reflect that.</p>
</blockquote>
