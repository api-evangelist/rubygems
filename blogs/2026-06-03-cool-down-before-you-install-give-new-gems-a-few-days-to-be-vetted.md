---
title: "Cool down before you install: give new gems a few days to be vetted"
url: "https://blog.rubygems.org/2026/06/03/cooldown-let-new-gems-be-vetted.html"
date: "2026-06-03"
author: "Hiroshi SHIBATA"
feed_url: "https://blog.rubygems.org/atom.xml"
---
Bundler 4.0.13 introduces a cooldown feature that prevents automatic resolution to gem versions published within a specified timeframe, mitigating supply-chain attacks by allowing time for community scrutiny before installation. The opt-in mechanism refuses to resolve to a version until it has been public for at least N days, configurable per-source in the Gemfile or via global settings. Users can override the cooldown policy when security updates require immediate installation.
