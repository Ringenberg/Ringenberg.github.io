---
layout: post
title: "Getting MathJax to render CTAT hints"
author: Michael Ringenberg
date: 2017-07-05
---

In one of the courses that I am working on has decided to use
[MathML](https://www.w3.org/Math/) and [MathJax](https://www.mathjax.org/)
for rendering math. Unfortunately, MathJax only processes the page once on
load which is a problem if one's [CTAT](http://ctat.pact.cs.cmu.edu/) hints
also includes MathML as CTAT hints are added dynamically.

Here is a
[gist](https://gist.github.com/Ringenberg/6b2528187623537d558ade938cc0f96d)
that will get MathJax to process hints as they are displayed.

Useful links:
* [The MathJax.Hub Object](http://docs.mathjax.org/en/latest/api/hub.html)
* [Modifying Math on the Page](http://docs.mathjax.org/en/latest/advanced/typeset.html)
* [MutationObserver](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver)


