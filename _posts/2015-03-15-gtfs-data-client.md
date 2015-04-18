---
layout: post
title:  "GTFS Data Client"
author: MJ Rossetti
categories: open-data-services
tags: gtfs open-transit-data gtfs-data-exchange google-transit-data-feed 
published: false
icon_class: train
technologies: ruby rails relational-database
---

This application relies on data provided by transit agencies according to the [General Transit Feed Specification (GTFS)](https://developers.google.com/transit/gtfs/).

This application consumes GTFS feeds from one or more known source locations.

This application gathers a comprehensive list of known source locations from GTFS community websites, including the GTFS Data Exchange, and a wiki page maintained on Google Transit Data Feeds.

The application provides a command line interface to methods, that when executed daily, ensure the recency of transit data from participating agencies.

Finally, the application displays transit information for participating agencies.

I use it to find out when the next train is leaving from my hometown train station in Branford, CT.

