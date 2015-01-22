---
layout: post
title: Vanagandr
permalink: /projects/vanagandr/
categories: project
---

[[Source]][1]

# Introduction

This is a simple library regrouping different kinds of algorithms seen in class which I wanted to experiment with. It includes numeric methods such as tools for integration and equation solving. It also includes pricing algorithms such as the Binomial Tree and the Trinomial Tree (with different adjustements).

# Implemented

* Binomial Tree (Eu - AM)
  * Cox Ross Rubinstein parametrization with Broadie et Detemple (1996) adjustment
  
* Trinomial Tree (Eu - AM)
  * Tian parametrization with Broadie et Detemple (1996) adjustment
  
* Explicit Finite Difference (Eu - AM)
  * 1 Dimension (matlab must be adapted)

* Numeric Integration
  * Rectangle
  * Trapezoidal
  * Simpson
  * GaussLegendre (3 nodes)
  * Gauss Chebyshev 1-2
  
  
  
[1]: https://github.com/Delaunay/vanagandr
