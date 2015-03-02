---
layout: post
title:  "US Income and Affordability Map"
author: MJ Rossetti
categories: open-data-vizualizations
tags: us-census housing income affordability map
published: true
icon_class: dollar
technologies: mapbox.js d3.js
---

A hackathon I recently attended posed a number of challenges aimed at solving the problem of lack of affordable housing in San Francisco.

I thought it first important to independently verify the challenges' underlying assumptions, specifically to answer the following questions:

1. Just how affordable is housing in San Francisco?
2. How does San Francisco housing affordability compare against other geographies across the United States?

I learned from Wikipedia that [affordability](http://en.wiktionary.org/wiki/affordability) is a measure of cost vs one's ability to pay, and I found data from the US Census which provided exactly this information. 

The result of my work is a map-based tool that facilities exploration of metrics of income and affordability for different kinds of households across the country.

View the tool live at http://bl.ocks.org/s2t2/raw/cd45dfd8007fcf83fef7/ or check out the source code at https://gist.github.com/s2t2/cd45dfd8007fcf83fef7.

![A choropleth map of the United States.](/assets/images/income-and-affordability-map.png "Income and Affordability Map")

{% gist cd45dfd8007fcf83fef7 %}
