---
layout: post
title:  "Creating all your metrics solution"
date:   2016-04-15 00:00:00 +0100
categories:
---

## Overview

Part of being a full-stack-dev (IMO) is knowing how to get metrics from your products and use it to:

*   monitor the health of your products/servers
*   spot hardware wastes (as idle machines)
*   get some insights about how your products work, as peak times, regions, ...
*   alert when something not expected is happening

At work we started using some really cool tools, and looking further on the internet I found a wide variety of tools to get/store/visualise metrics. For my personal project, and this post, I will use the [TICK Stack](https://influxdata.com/time-series-platform/) from [Influxdata](https://influxdata.com/), but with some modifications. In my case I'm using the TIGKS Stack, it stands for:

*   Telegraf - Pulls data from servers/tools/etc
*   Influxdb - Time series database
*   Grafana  - A very good graphs visualisation tool *****
*   Kapacitor - A very flexible alerting tool
*   Statsd - Aggregates the data in the database, so we don't blow our storage. *****

***** Not part of the original TICK Stack from Influxdata

## Installing

## Configuring

## Sending the first data

## Visualising the data

## Building very COOL dashboards

## Creating your own alerts
