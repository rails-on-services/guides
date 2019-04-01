---
layout: page
title: Getting Started with Rails On Services
permalink: /getting-started.html
---
<div class="summary" markdown="1">
<br/><br/>
After reading this guide, you will know:

<b>How to install Rails on Services, create a new Rails on Services project, and run the services in development mode.</b>
<p></p>
<b>The general layout of a Rails on Services project.</b>
<p></p>

<b>How to quickly generate the starting pieces of a Rails on Services project.</b>
<br/><br/>
</div>

* TOC
{:toc}

### 1 Guide Assumptions

This guide is designed for application architects and developers who want to deploy micro service based Rails applications on Kubernetes. It assumes that you have prior experience with Rails, Kubernetes and either AWS, GCP or Azure.

Rails on Services is a project paradigm that provides the typical infrastructure required by a modern business application. If you have no prior experience with Rails, you will find a very steep learning curve diving straight into Rails on Services. There are several curated lists of online resources for learning:

[Ruby on Rails Guides](https://guides.rubyonrails.org)

[Building Microservices](https://www.amazon.com/dp/1491950358?aaxitk=ngaqJOnUNX5y4Wo1Ai39pg&pd_rd_i=1491950358&pf_rd_p=e037c154-e093-48a4-b127-477e5e294e3f&hsa_cr_id=9661114680601&sb-ci-n=asinImage&sb-ci-v=https%3A%2F%2Fimages-na.ssl-images-amazon.com%2Fimages%2FI%2F51m85J4Zi9L.jpg&sb-ci-a=1491950358)

[Domain Driven Design](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/ref=sr_1_1?crid=1DYDEYDM4PYM8&keywords=domain+driven+design&qid=1553189946&s=gateway&sprefix=domain+driven+%2Caps%2C363&sr=8-1)

### 2 What is Rails on Services?

Building on Kubernetes, provides out of the box services, including:

Platform Identity and Access Managment provided by the [IAM Service](iam.html)

Application Users and Groups provided by the [Cognito Service](cognito.html)

Communications Integration with leading providers AWS and [Twilio](https://twilio.com) provided by the [Communication Service](comm.html)

File Managment for browser based uploads and SFTP provided by the [Storage Service](storage.html)

Realtime notification services provided by the [Callback Service](callback.html)

Billing services that allow your business services to provide metered billing provided by the [Billing Service](billing.html) with chargeback services from [Stripe](https://www.stripe.com)

In addition to the core business services, Rails on Services provides the tooling necessary to stand up your project's services in Kubernetes with out of the box support for major [CNCF](https://www.cncf.io) services:

- [Prometheus](https://prometheus.io) monitoring of all services
- [Istio](https://istio.io) and [Envoy Proxy](https://www.envoyproxy.io) to provide a service mesh for reliable communications between your services
- [Grafana](https://grafana.com) for metric reporting and visulization
- [Fluentd](https://www.fluentd.org) for logging

Rails on Services provides Terraform code to deploy all the necessary infrastructure out of the box

- Request logging
- Error reporting to leading providers [sentry.io](https://sentry.io)

Rails on Services leverages the [JSONAPI specification](https://jsonapi.org) for all API endpoints and [JSON Web Tokens](https://jwt.io) for authentication

### [3 Creating a New Rails on Services Project](#creating-a-new-rails-on-services-project)

The best way to read this guide is to follow it step by step. All steps are essential to run this example application and no additional code or steps are needed.

By following along with this guide, you'll create a Rails on Services project called store, a mirco service based store front. Before you can start building the application, you need to make sure that you have Rails on Services itself installed.

#### [3.1 Installing Rails on Services](#installing-rails-on-services)

{% highlight bash %}
gem install ros
{% endhighlight %}

#### [3.2 Creating the Store Project](#creating-the-store-project)

{% highlight bash %}
ros new 'project_name'
{% endhighlight %}

### 4 Hello, Rails!

To begin with, let's get some text up on screen quickly. To do this, you need to get your Rails application server running.

#### 4.1 Starting up the Web Server

You actually have a functional Rails application already. To see it, you need to start a web server on your development machine. You can do this by running the following in the blog directory:

#### 4.2 Say "Hello", Rails

To get Rails saying "Hello", you need to create at minimum a controller and a view.

A controller's purpose is to receive specific requests for the application. Routing decides which controller receives which requests. Often, there is more than one route to each controller, and different routes can be served by different actions. Each action's purpose is to collect information to provide it to a view.

A view's purpose is to display this information in a human readable format. An important distinction to make is that it is the controller, not the view, where information is collected. The view should just display that information. By default, view templates are written in a language called eRuby (Embedded Ruby) which is processed by the request cycle in Rails before being sent to the user.

To create a new controller, you will need to run the "controller" generator and tell it you want a controller called "Welcome" with an action called "index", just like this:

<!---
[x]Dockerfile/compose in gems which mounts the current directory
[x]no puma, just rails s -b 0.0.0.0; puma is in the enclosing app’s Gemfile
[x]nginx is used to so that multiple services are available at the same port number
[x]Files: Dockerfile, docker-compose.yml, nginx.conf, settings.yml?
[x]dependent gems in dockerfile: use git instead of path, commit code to git, use bundle local for development, but dockerfile pulls from GH
[]get this thing deployed into k8s using skaffold
[]add a .env file for the version number of the image, etc. push to perx docker hub
--->

### Endpoint Generator

[x]use the endpoint generator to generate a campaign in survey app

### Console Shortcuts

[x]console shortcuts from gem are not showing up in the app; add initializer


### Development Seeds

The core provides a default seed data for development for each of the services.
By default two tenants are created in every service which schma names 111_111_111 and 222_222_222

[x]Get gem's seeds working in enclosing app
