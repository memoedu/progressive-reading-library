---
title: 'Auto-Smallcaps Filter'
author: Gwern
description: "N/A"
created: 2020-02-19
modified: 2021-06-29
status: finished
importance: N/A
confidence: log
css-extension: dropcaps-de-zs toc-not
backlink: False
...

Description of a Pandoc plugin I wrote for use on [Gwern.net](/static/build/app/hakyll.hs) which automatically rewrites any string text of 3 or more capital letters (eg. "NSA" or "GAN" or "NASA") to rendered with [small caps](https://en.wikipedia.org/wiki/Small_caps) in CSS, which are typographically nicer to read as the small caps make the acronyms less visually overwhelming compared to regular capital letters.

This is trickier to implement than the usual Pandoc plugin because strings must be parsed and broken up, so it's not a straightforward 1-for-1 substitution, and I explain the necessary recursive tricks to make it correct & typecheck.
```{=HTML}
<div class='text-center' id='return-to-blog-index-link'>[<a href='/blog/index' class='link-page link-tag directory-indexes-upwards link-annotated-not' data-link-icon='arrow-up-left' data-link-icon-type='svg' rel='tag' title='Link to blog directory'>Return to blog index</a>]</div>
```
