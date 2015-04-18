---
layout: post
title:  "LSAT Curve"
author: MJ Rossetti
categories: open-data-vizualizations
tags: law-school admissions-test score data
published: true
icon_class: book
technologies: javascript rails json d3.js
---

After taking the June 2014 Law School Admissions Test (LSAT), I was interested in learning more about score distributions.

I found a [data table from Cambridge LSAT](http://www.cambridgelsat.com/resources/data/lsat-percentiles-table/),
 manually converted it to [JSON](https://github.com/s2t2/lsat-curve/blob/master/lsat_curve.json),
 then plotted the results on a graph.

View the data vizualizaion live at http://s2t2.info/lsat-curve/ or check out the source code at https://github.com/s2t2/lsat-curve.

<hr>

![A graph plotting the distribution of LSAT scores.](/assets/images/lsat-curve.png "LSAT Curve Graph")

<hr>

<script src="http://gist-it.appspot.com/github/s2t2/lsat-curve/blob/master/index.html"></script>

<script src="http://gist-it.appspot.com/github/s2t2/lsat-curve/blob/master/lsat_curve.json"></script>

<hr>