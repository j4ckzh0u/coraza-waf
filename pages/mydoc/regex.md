---
title: Regex Support
keywords: regex, scripting, modsecurity, apache, coraza, waf, opensource
last_updated: August 22, 2020
sidebar: mydoc_sidebar
permalink: regex.html
folder: mydoc
---

Many Coraza features like the @rx operator or the Variables engine supports regular expressions.

Coraza WAF supports PCRE expressions, you can check the [documentation here](https://pcre.org/pcre.txt).

We do not use Golang Regex because it supports re2 but it does not support negative lookbacks, negative lookbacks are widely used within the OWASP CRS project.

Most regex in Coraza must be enclosed between slashes, like ``SecRule ARGS:/(.*?)/ "" "id:1"``
