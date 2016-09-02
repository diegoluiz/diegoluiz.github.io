---
layout: post
title:  "Private Continuous Deployment [almost] free"
date:   2016-04-25 00:00:00 +0000
categories: Code Ops VCS
---

It is already some months my team in the company is using the Continuous Delivery and Continuous Deployment approach, depending on the project. But as we are "mere users", we were responsible to choose a branch strategy, creating the build and deployment flow. But all tools were already there, properly installed and configured. So I decided to start my personal Continuous Deployment solution. These are the following tools I used (all the links are [here](https://diegoluiz.com/2016/04/04/full-stack-from-nothing-to-production/)): GitHub Travis CI Docker A [personalised nodejs app](https://github.com/diegoluiz/dockerhub-listener)  as a simple deployment tool And there are some tools, which I want to integrate, they are Slack StackStorm Opbeat InfluxDB Logstash Sensu As I don't have any real personal project, I built a [simple nodejs app](https://github.com/diegoluiz/node-sample) which I use as a normal application. As the application itself isn't important, I won't go deeper into it. The steps are:

*   Creating repository
*   Linking to the build server
    *   Creating your build file
    *   "Building the code"
    *   Testing
    *   Creating the container
    *   Pushing it to dockerhub
*   Creating your dockerhub repo
*   Configuring a new webhook
*   Installing/configuring the deployment tool

## Creating repository

It's is really easy and straight forward. Repositories in github are free as long as they are public. So just enter [here](https://github.com/new),  and create your own repo. I recommend to choose the right [.gitignore](https://git-scm.com/docs/gitignore) file, and the licensing matters only if you are planning to carry on with your application seriously. ![Screen Shot 2016-05-07 at 11.34.56]({{ site.url }}/assets/2016-04-25-private-continuous-deployment-almost-free/screen-shot-2016-05-07-at-11-34-56.png) Linking to the build server   Creating your build file   "Building the code"   Testing   Creating the container   Pushing it to dockerhub   Creating your dockerhub repo   Configuring a new webhook   Installing/configuring the deployment tool
