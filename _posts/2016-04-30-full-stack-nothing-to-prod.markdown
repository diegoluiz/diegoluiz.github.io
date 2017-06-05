---
layout: post
title:  "Full Stack? From nothing to production"
date:   2016-04-30 00:00:00 +0000
tags: [code, ops, vcs]
comments: true
---
Well, in this article, I'm expressing my own opinion about what is a full-stack developer, what it means for everybody (who has contacted me for this role). Almost in all the places I read "full-stack", they mean someone who is able to do some Back-end + Front-end development, which is not wrong. 
But recently my responsibilities inside the company and the team expanded a lot, as well my vision of what is the whole process from the requirements to have one application in production.
<!--more-->
Many people, who consider themselves full-stack, whom I talk to consider the "development job" done when you commit to the source control, and then, magically, your code is running in production. What in my opinion is wrong. Nowadays, from my point of view, full-stack, nowadays, means someone who starts working even before the development starts, defining the architecture of the solution and sometimes even helping to define the product itself. You can see the topics I consider a full-stack dev should know, how ever he doesn't need to be a specialist in all of them:

* Pre development
  * Business Requirements
  * Solution Architecture
  * Designing Software / Database / Infrastructure
* During development
  * Developing (Front / back end / Database)
  * Testing
* Post development
  * Deployment (everything before)
  * Monitoring + alerting system
  * Metrics Alert
  * Logging
  * Escalating system
  * Scaling environment
* In parallel
  * Infrastructure / Software Provisioning
  * Additional tools, like ChatOps, Continuous Integration, and so on...

I tried to separate in steps, however many of them can be done together. Each topic of this has an infinite amount of things to learn/do, it's not important to know everything, but to know what you can use and the trade off of using one solution / technology instead of the other.

I can't really share my experiences for all of them, as I haven't participated in the implementation of some of them, however I tried to get as involved as I could to learn about them. Let's try to dive a bit deeper into the topics:

### Business Requirements
in the end, almost all developers have already helped to defined some business requirements. Sometimes about the UX, sometimes about some non-sense or brilliant ideas. It's very common to have developers, specially the oldest of the company, involved in these discussions. Actually, as more they are involved, better. As a solid understanding of the business requirements will make it clearer to defined the User Stories / Tasks and the Acceptance Tests.

### Solution Architecture
before writing any line of code, and after knowing what you need to do, you need to define what is going to be used and how. Usually this step is skipped in many places, as people tend to use what they are already using. It's in this step you choose the technologies you are using. This is where the experience comes.
* C#, Node or Python for a X Scenario?
* Let's go for PaaS ou IaaS?
* Will the target computer be good enough to process this?
* Is this technology easily scalable?
* Do all the chosen technologies support each other?

#### Some real life examples:
* C# is not supported by Storm / Spark (Apache data processing technologies), however if you use Azure, they made C# works with Storm, but not Spark. This you can find one the internet, but knowing the this actually works, and it works really well, only with experience.
* PaaS is easier to deploy, usually cheaper, but until you need to do something out-of-the-box, there is a high probability that it's not supported.
* Sometimes you need to opt for a lighter technology, or an older version because the target computer doesn't have the minimum requirements to run the application.
* You just realise that your message queueing system is not easily scalable when it's about to blow, and everybody goes crazy.
* You can have a brilliant idea to use a cutting edge technology, but when you test the support for the system's language, it's horrible.

Of course you can find all the answer on the internet, but until you need to actually look for them, you will have no clue about them.

### Designing Software / Database
this is where the bias "full-stack" enters. Sometimes defining the software structure, database modelling, and so on.

### Developing Front / back end / Database
here is the main role of the developer. This includes all the good practices / methodologies / principles / patterns / ... of software development. I won't go further because this is an infinite discussion.

### Testing
this is not the unit test (which is inside the developing step), but the tests you run to guarantee your code is doing what it was asked, aka Acceptance Test. Although this step is ignored in many projects (which I've worked), it's extremely important as it says if, in the end, everything works or not.

### Deployment
this is beyond the bias full-stack. This is what happens after your code is committed to the source control. Here you have the strategy and rules to deploy to the environments. Which environment comes first, what to do if the last deployment worked, and what if it didn't work. Synchronising the deployments of dependencies (which is difficult as hell), which includes database, how to deploy a new service without making the previous one unavailable, and so on... In this step comes the terms Continuous Delivery and Continuous Deployment, which many people are talking about recently.

_beyond here comes the responsibilities from operations, which few developers care about. nowadays there is tendency of more sharing those responsibilities to the development team as well, creating the DevOps._

### Monitoring + alerting
it's extremely recommend, if not mandatory, to have something monitoring your system, and alerting when the system doesn't behave in the right way. Monitoring systems evolved a lot, and today they need to be as reliable as the product itself.

### Metrics
sometimes some weird errors happen and they don't turn your application off, but it changes the behaviour of you project, spending more memory, cpu, or giving less or more output. Here comes the metrics system, which monitors the behaviour of your product. It can work closely to the monitoring system, as you can set alerts to be triggered when something is not as expected. Those metrics can be used as well as base for some business / technical decisions.

### Logging
the part of generating logs is already inside the Developing step. However it's normal for a company to have 100+ servers nowadays. What do you do when a website throws a 5xx and there are 10 servers behind a LB? Go for each one and copy the file to look for the exception? Today it's really important to centralise the logs and make them searchable.

### Escalating
this is a very interesting system, which I've never used unfortunately. Let's imagine that your website is offline, what does your monitoring system do? Send email to 100 people and make they crazy? Send email to 1 person and pray for the person to read it? There are some tools you can integrate with your monitoring system to try to reach out a person, if there is no reploy, it calls, sends text message, tries to reach out another person and so on... So you can trust that someone is going to answer the phone when something is wrong.

## Scaling
most of the websites have a predictivish amount of users/sessions/requests, and the peaks are not so extreme. But when you have a completely unpredictive traffic, you need your system to know when it needs to scale up and down. So you don't lose money because you lost users, and you don't spend money with unused infrastructure.

### Infrastructure / Software Provisioning
with the cloud, hardware is being handled just as software. You define it as scripts, commit to source control, sometimes you have a CD implemented and it creates your infrastructure. And then you have some software provisioning running which installs everything the machine needs according to its role. This is taking the infrastructure management to another level. I'm going to leave some links of some nice tools which I've used/heard about.

**I'm not posting anything related to code on purpose, as I will try to do it in other posts. **It's not separated nor it has description, so, if you don't know what the tool is, click on the link.

* [Redis](http://redis.io/)
* [Riak](http://info.basho.com/BOL-Riak-Open-Source_LP.html?TDET=ADW&ADG=riak&gclid=Cj0KEQjwxI24BRDqqN3f-97N6egBEiQAGv37hO3RCkSVTiTc3yaWkorpDHKVRnrj6HX1A39ODYrcX5AaAhfo8P8HAQ)
* [Document DB](https://azure.microsoft.com/en-gb/services/documentdb/?WT.srch=1&WT.mc_ID=SEM_h7COg81O)
* [MongoDB](https://www.mongodb.org/)
* [Cassandra](http://cassandra.apache.org/)
* [Azure Service Bus](https://azure.microsoft.com/en-gb/services/service-bus/)
* [MSMQ](https://msdn.microsoft.com/en-us/library/ms711472(v=vs.85).aspx)
* [RabbitMQ](https://www.rabbitmq.com/)
* [Kafka](http://kafka.apache.org/)
* [Storm](http://storm.apache.org/)
* [Spark](http://spark.apache.org/)
* [VSO Build](https://msdn.microsoft.com/en-us/library/vs/alm/build/vs/define-build)
* [Travis CI](https://travis-ci.org/)
* [AppVeyor](http://www.appveyor.com/)
* [Team City](https://www.jetbrains.com/teamcity/)
* [Octopus Deploy](https://octopus.com/)
* [Redgate](http://www.red-gate.com/)
* [Specflow](http://www.specflow.org/)
* [Selenium](http://www.seleniumhq.org/)
* [Nagios](https://www.nagios.org/)
* [PandoraFMS](http://pandorafms.com/)
* [Sensu](https://sensuapp.org/)
* [Grafana](http://grafana.org/)
* [Graphite](http://graphite.wikidot.com/)
* [InfluxDB](https://influxdata.com/)
* [Whisper](https://github.com/graphite-project/whisper)
* [Ceres](https://github.com/graphite-project/ceres)
* [Statsd](https://github.com/etsy/statsd)
* [Carbon](https://github.com/graphite-project/carbon)
* [Elastic Search](https://www.elastic.co/)
* [Solr](http://lucene.apache.org/solr/)
* [Logstash](https://www.elastic.co/products/logstash)
* [Kibana](https://www.elastic.co/products/kibana)
* [Pagerduty](https://www.pagerduty.com/)
* [Puppet](https://puppetlabs.com/)
* [Chef](https://www.chef.io/chef/)
* [Docker](https://www.docker.com/)
* [Packer](https://www.packer.io/)
* [Terraform](https://www.terraform.io/)
* [Vagrant](https://www.vagrantup.com/)
* [Consul](https://www.consul.io/)
* [ARM](https://azure.microsoft.com/en-gb/documentation/articles/resource-group-overview/)
* [Slack](https://slack.com/)
* [Hipchat](https://hipchat.com/)

Currently, I'm doing my own full-stack process, trying to cover as many areas as I can. So soon, probably, there will be a post with how I did it.

Cheers :)
