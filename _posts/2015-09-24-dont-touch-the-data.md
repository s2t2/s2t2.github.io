---
layout: post
title:  "Don't Touch the Data"
author: MJ Rossetti
categories: best-practices
tags: none
published: true
icon_class: none
technologies: ruby python r
credits:
 - https://www.archaeological.org/pdfs/AIATourismGuidelines.pdf
---

![a photo of one crime scene  investigator putting on white gloves while the other takes pictures](http://www.itsgov.com/wp-content/uploads/2014/01/The-Science-Of-Crime-Scene-Forensics-A-Detailed-Overview.jpg)

Data science, specifically the Extraction, Transformation, and Loading (ETL) of data, requires strict adherence to repeatable, scientific processes.

When encountering raw data, data scientists should assume the mentality of an archeologist or crime scene investigator. Archaeologists avoid improper handling of artifacts, else they risk causing damage and deterioration to the excavation site. Crime scene investigators don't tamper with the evidence, else it may become inadmissible in a court of law.

Likewise, if you perform undocumented, manual transformation processes on a dataset, you diminish its value. Any information findings or interpretations which result from the dataset become unreliable and un-defendable.

This is why many data scientists use scripting languages like Ruby, Python, and R to perform data processing tasks. Data processing scripts automate manual tasks, and facilitate process repetition.

Many organizations have begun to operate open data portals, which allow anyone to download static datasets. This is an important first step toward opening data for use by the general public. And in some cases these open data portals also provide programmatic interface to the data.

As the current trend in downloadable data shifts in the future towards open data APIs and web services, data tampering will become less of a concern. But until then, remember to avoid, or at least minimize and document your manual data processing efforts.
