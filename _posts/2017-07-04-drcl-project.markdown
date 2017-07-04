---
layout: post
project: true
title:  "DRCL Project"
date:   2017-07-04 00:00:00 +0100
tags: [dotnetcore]
comments: true
---
I'm starting a new personal project, as Phil Karlton said already, naming things is one of the hardest problems in programming. So I will call it **Drcl**, which is an acronym to my full name.
This project is a set of multiple **dotnet core** projects.
<!--more-->

I've been working with C# and .Net Framework for a while now (about 5 years), and I feel pretty comfortable with that. Unfortunately I haven't used dotnet core yet, therefore starting a project with it will be more challenging.
My main goal is to become fluent with core the same way I'm with the standard one. 
And there is a minor goal, which is to create a set of libraries which I can use constantly for my other projects.

I think that many developers are tired of always write an `ISerializer` interface and some implementations for it for almost all the projects.
So my project will consist initially in packages for (each one will contain interfaces in a different project):

* Serialisers, which will contain the packages for
  * json
  * thrift
  * avro (maybe)
* Messaging, which will contain the packages for
  * azure service bus
  * event hubs
  * rabbitmq (maybe)
* Cloud Storage 
  * Azure Storage Account
  * AWS S3
* ETL, a simple library to speed up the creation of ETLs. Trying to use as much as possible parallel programming.

The projects can be found on [github](https://github.com/diegoluiz?utf8=%E2%9C%93&tab=repositories&q=drcl&type=&language=) and soon I will create a [issues dashboard](https://waffle.io/) for it as well.

I've already configured the pipeline for the package creation and the push to the [nugget official repository](https://www.nuget.org/packages?q=drcl)
It's not in the plan to make them fully ready for production neither LTS. But if you someone wants to open an issue I will consider it a training for me :)
