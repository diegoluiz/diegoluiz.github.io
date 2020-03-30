---
layout: post
title:  "Full Stack? From nothing to production"
date:   2016-04-30 00:00:00 +0000
tags: [code, ops, vcs]
comments: true
---

In this article, I'm expressing my own opinion about what is a full-stack developer, what it means for everybody (who has contacted me for this role). Almost in all the places, I read "full-stack", they mean someone who is able to do some Back-end + Front-end development, which is not wrong. 
But recently my responsibilities inside the company and the team expanded a lot, as well my vision of what is the whole process from the requirements to have one application in production.
<!--more-->
Many people, who consider themselves full-stack, whom I talk to consider the "development job" done when you commit to the source control, and then, magically, your code is running in production. What is wrong. Full-stack means someone who starts working even before the development starts, defining the architecture of the solution and even helping to define the product itself. Full-stack developers should know at least the following:

* Pre-development
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

I tried to separate in steps, however many of them can be done together. Each topic of this has an infinite amount of things to learn/do, it's important to know trade-off of using one solution/technology instead of another.

I haven't implemented all of them, however, I strive to get as involved on their implementations. Let's dive a bit deeper into the topics:

### Business Requirements
In the end, almost all developers have contributed to defining some business requirements. Sometimes about the UX, others in some non-sense or brilliant ideas. It's very common to have developers, especially the oldest in the company, involved in these discussions. Actually, as more they are involved, then better. As long the understanding of the business requirements become clearer, easier is to define the User Stories / Tasks and the Acceptance Tests.

### Solution Architecture
Before writing any line of code, and after knowing what you need to do, you need to define what is going to be used and how. It's in this step you choose the technologies you are using. This is where the experience comes.
In many places, this step is skipped. People tend to use what they are already using.
Some of the questions you should ask:
* C#, Node or Python for an X Scenario?
* Let's go for PaaS ou IaaS?
* Will the target computer be good enough to process this?
* Is this technology easily scalable?
* Do all the chosen technologies support each other?

#### Some real-life examples:
* C# is not supported by Storm / Spark (Apache data processing technologies). However, if you use Azure, they made C# works with Storm, but not Spark. This can be found on the internet. You can only know it actually works, and it works really well, only with experience.
* PaaS is easier to deploy, usually cheaper, but until you need to do something out-of-the-box, there is a high probability that it's not supported.
* Sometimes you need to choose a lighter technology, or an older version because the target computer doesn't have the minimum requirements to run the application.
* You just realize that your message queueing system is not easily scalable when it's about to blow, and everybody goes crazy.
* You can have a brilliant idea to use a cutting-edge technology, but when you test the support for the system's language, it's horrible.
Of course, you can find all the answer on the internet, but until you have the need, you will have no clue about them.

### Designing Software / Database
This is where the bias "full-stack" comes. Sometimes defining the software structure, database modeling, and so on.

### Developing front-end/ back-end / database
This is what developers do. This includes all the good practices / methodologies / principles / patterns / ... of software development. I won't go further because this is an infinite discussion.

### Testing
Not unit testing (which is inside the developing step), but the tests you run to guarantee your code is doing what it was asked for, these tests are called Acceptance Test. Although this step is ignored in many projects (which I've worked on), it's extremely important to know whether everything works or not.

### Deployment
This is beyond the bias of full-stack. This is what happens after your code is committed to the source control. Here you have strategies and rules to deploy to the environments. Which environment comes first, what to do if the last deployment was successful or not.
Deployment includes synchronizing deployment of dependencies (which is difficult as hell), including databases, how to deploy a new service without making a previous one unavailable, and so on... In this step we care about Continuous Delivery and Continuous Deployment, recently many people are talking about it.
_What is operations? Nowadays there is tendency of sharing operations responsibilities with the development team, creating the DevOps._

### Monitoring + alerting
It's extremely recommended, if not mandatory, to have something monitoring your system, and to alert when the system doesn't behave in the right way. Monitoring systems evolved a lot, and today they need to be as reliable as the product itself.

### Metrics
Sometimes some weird errors happen and they don't turn your application off, but it changes the behavior of your application, spending more memory, CPU, or giving less or more output. For this, we use metric systems, which monitors the behavior of your product. They can work closely with the monitoring system, as you can set alerts to be triggered when something is not as expected. Those metrics can be used as well as the base for some business or technical decisions.

### Logging
The part of generating logs is already inside the Developing step. However, it's normal for a company to have more than 100 servers. What do you do when a website throws a 5xx and there are 10 servers behind a load balancer? Go for each one and copy the file to look for the exception? It's really important to centralize the logs and make them searchable.

### Escalating alerts
This is a very interesting system. Let's imagine that your website is offline, what does your monitoring system do? Send emails to 100 people and make them crazy? Send emails to 1 person and pray for the person to read it? There are some tools you can integrate with your monitoring system to try to reach out a person, if there is no reply, it calls, sends text messages, tries to reach out another person and so on... So you can trust that someone is going to answer the phone when something is wrong.

### Scaling
Most of the websites have a certain amount of users/sessions/requests, and the peaks are not so extreme. But when you have a completely unpredictable traffic, you need your system to know when it needs to scale up and down. So you don't lose money because you lost users, and you don't spend money on inactive infrastructure.

### Infrastructure / Software Provisioning
With the cloud, the hardware is being handled just as software. You define it as scripts, commit to source control, sometimes you have a CD implemented and it creates your infrastructure. And then you have some software provisioning running which installs everything the machine needs according to its role. This is taking the infrastructure management to another level. I'm posting some links to some nice tools which I've used/heard about.

**I'm not posting anything related to code on purpose, as I will do in other posts. **It's not separated nor it has a description, so, if you don't know what the tool is, click on the link.
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
