---
layout: post
title: "ES6 Classes extending functions"
author: Michael Ringenberg
date: 2018-01-16
---

While trying to clean up some code, I experimented with using the ES6 class
declaration to try to extend a function from a library where the function
is an old ES5 style prototype based "class". This is theoretically possible
and I found that it mostly worked. The only problem seemed to be that any
of the methods defined in my class that were supposed to clobber methods from
the super class were getting ignored.

In my investigations, I discovered that they were not getting ignored, they
were getting clobbered during the call to `super();` in the constructor. The
old library defined methods using the pattern
`this.method = function () {...};` in the constructor as opposed to modifying
the prototype. 

As a work around until the old library gets fixed, I renamed my clobbering
methods and then added `this.method = this._method;` to the constructor
method in my class after the call to `super()` for each of the clobbering
methods.
