---
title: "Temporarily disabling language support on rubygems.org"
url: "https://blog.rubygems.org/2026/04/07/temporarily-disabling-language-support.html"
date: "2026-04-07T00:00:00+00:00"
author: "Colby Swandale"
feed_url: "https://blog.rubygems.org/atom.xml"
---
<p>I’m one of the operators of <a href="https://rubygems.org">rubygems.org</a>. Here’s what’s been happening over the past week, and a temporary change we’re making as a result.</p>

<p>For the past seven days, <a href="https://rubygems.org">rubygems.org</a> has been under sustained bot traffic from many different sources scraping data from every published gem. The volume has been large enough to force the site offline while we respond. The bots are deliberately bypassing the Fastly cache, hitting our origin servers directly.</p>

<p>The primary target has been our language locale pages, the translated versions of <a href="https://rubygems.org">rubygems.org</a>. Unfortunately, the locale system wasn’t designed to cache easily through a CDN. To protect site stability, we’re temporarily disabling language support while we rearchitect how locale pages are cached.</p>

<p>We’ll restore language support as soon as we have caching in place that can handle this volume. Thank you for your patience.</p>

<p>P.S. if you need gem and version data for a project, we publish regular database exports at <a href="https://rubygems.org/pages/data">https://rubygems.org/pages/data</a>. We strongly recommend using those instead of scraping <a href="https://rubygems.org">rubygems.org</a> directly.</p>
