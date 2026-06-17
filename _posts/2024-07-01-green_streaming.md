---
title: Green Streaming - reducing the resource cost of streaming services
description: The development of a data analysis tool to study media consumption patterns with the goal of optimizing content delivery and caching, ultimately reducing resource consumption.
date: 2024-07-01 00:00:00 -500
categories: [Internship project]
tags: [TypeScript, SQL, Grafana, CDN, Data Analysis]
image: /assets/green_streaming/logo.png
---

## Overview

Green Streaming is an internship project I was involved in at [Mog Technologies](https://evs.com/na) (now part of EVS Broadcast) during the development of a dissertation for the bachelor's degree in Informatic Engineering. This project had the goal of developing a data analysis tool to study media consumption patterns with the aim of optimizing content delivery and caching, ultimately reducing resource consumption.

## Motivation

Green Streaming surfaced as an European initiative to address the resource consumption challenges of streaming services, which may distribute content to millions of users worldwide inefficiently. The project aimed to analyze media consumption patterns and generate insights that could lead to more efficient content delivery strategies, such as better caching and optimized content distribution networks (CDNs).

## Architecture and Technologies

This project was composed of three separate components: a data collection and processing module, a data analysis module, and a visualization module. My contribution was focused on the collection, processing and visualization of data, while the analysis module, based on AI techniques and predictive models, was developed by another internship colleague. We shared insights with one another, which made developing the project more collaborative and rewarding.

The data collection and processing module was responsible for gathering media consumption information from CDN logs. These logs could be acquired from the CDN provider's API or through an AWS S3 bucket where the logs were batched and stored. This module was developed using TypeScript, and had two modes of operation: a real-time mode that processed logs as they were generated, through the CDN API, and a batch mode that processed historical logs from the S3 bucket.

The objective was to parse the logs and extract relevant information, storing it under a structured format in a relational database. This structured data could then be used for analysis and visualization. Four tables were created in the database: `Client`, `Video`, `Request` and `CDNNode`, they respectively stored information about the users, the media content, the access requests and the CDN server involved in content delivery. 

Lastly the visualization module was developed using Grafana, a popular open-source platform for monitoring and observability. Grafana allowed the creation of interactive dashboards that displayed key metrics and insights derived from the collected data. These dashboards provided a visual representation of media consumption patterns, enabling stakeholders to identify trends and make informed decisions about content delivery strategies.

The following diagram illustrates the architecture of my contribution to the Green Streaming project:

<figure style="display:flex; flex-direction:column; align-items:center; gap:10px;">
  <img src="/assets/green_streaming/media_consumption_analyzer.png" alt="architecture diagram" style="height:500px;">
  <figcaption>
    Distribution of clients worldwide, based on their IP addresses, to identify geographic patterns in media consumption.
  </figcaption>
</figure>

## Dashboard and Insights

The Grafana dashboard created for the Green Streaming project provided a comprehensive view of media consumption patterns. This was the ultimate goal of the project, as it allowed to validate that data was being collected and processed correctly, as well as to generate insights in graphical form, which could be easily interpreted by stakeholders. The dashboard included various panels, some of which are presented in the following images:

<figure style="display:flex; flex-direction:column; align-items:center; gap:10px;">
  <img src="/assets/green_streaming/clients_world.png" alt="Clients Distribution" style="height:350px;">
  <figcaption>
    Distribution of clients worldwide, based on their IP addresses, to identify geographic patterns in media consumption.
  </figcaption>
</figure>

<figure style="display:flex; flex-direction:column; align-items:center; gap:10px;">
  <img src="/assets/green_streaming/requests_world.png" alt="Requests Distribution" style="height:330px;">
  <figcaption>
    Distribution of requests worldwide, based on the location of the CDN nodes, to understand which servers are handling the most traffic.
  </figcaption>
</figure>

<figure style="display:flex; flex-direction:column; align-items:center; gap:10px;">
  <img src="/assets/green_streaming/requests_history.png" alt="Requests History" style="height:200px;">
  <figcaption>
    Requests count over time, aggregated by a configurable time interval (e.g., hourly, daily, etc.).
  </figcaption>
</figure>

<figure style="display:flex; flex-direction:column; align-items:center; gap:10px;">
  <img src="/assets/green_streaming/requests_hourly.png" alt="Hourly Requests" style="height:500px;">
  <figcaption>
    Total request count, aggregated by the hour of the day, to identify peak usage times.
  </figcaption>
</figure>

These and other metrics extracted from the collected data provided the insights necessary to regulate content distribution and cdn configuration, reducing costs and resource consumption. For example, by identifying peak usage times and geographic patterns, the CDN could be optimized to cache content closer to users, reducing latency and bandwidth usage, as well as scaling resources more efficiently.

## Conclusion

The Green Streaming project was a valuable experience that allowed me to apply my skills to develop an extract-transform-load (ETL) workflow, as well as a data visualization solution to help solve a real-world problem. By analyzing media consumption patterns and generating insights, it was possible to contribute to the optimization of content delivery and caching strategies, ultimately reducing resource consumption for streaming services. The first contact with a company's environment, combined with the usage of technologies such as TypeScript, SQL, and Grafana, made it experience that allowed me to grow both professionally and personally, while also contributing to a project with a positive environmental impact.