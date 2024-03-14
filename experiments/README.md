Dusa Experiments
================

In the process of making the quine, some math was necessary.  Also, before escape sequences were added, I attempted to produce a quine by encoding the entire string as a number; unfortunately, to define multiplication and division, we need (AFAICT) to generate the entire table up to the biggest number we need.  The approach here works for small numbers, but quickly becomes "lifetime of the universe" slow for larger numbers.  This is still kind of neat demonstrating how to do math in Dusa, so here it is without further comment.
