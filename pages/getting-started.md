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

#### [3.2 Creating the Blog Project](#creating-the-blog-project)

{% highlight bash %}
ros new blog
{% endhighlight %}

### 4 Hello, Ros!

To begin with, let's create an IAM User and access the list of users from Postman

{% highlight bash %}
ros init iam
{% endhighlight %}

This will seed the database with two tenants each with a root account and a User with `Administrator` policy attached. Each tenant has a nine digit `account_id`.

The two development tenants have `account_id` 111111111 and 222222222

#### 4.1 Starting the IAM Service

You actually have a functional IAM service already. To see it, you need to start a web server on your development machine. You can do this by running the following in the ros-iam directory:

{% highlight bash %}
rails c
{% endhighlight %}

#### 4.2 Navigating the Tenants

When you seeded the IAM database `db/seeds/development/data.seeds.rb` two tenants were created and a User with credentials

Go to rails console

{% highlight ruby %}
[1] [iam][development][public] pry(main)> st
1 111_111_111
2 222_222_222
[2] [iam][development][public] pry(main)> st 2
[3] [iam][development][222_222_222] pry(main)> uf
=> #<User id: 1, console: true, api: true, time_zone: "Asia/Singapore", attached_policies: {"AdministratorAccess"=>1}, attached_actions: {}, username: "Admin_2", created_at: "2019-03-18 18:51:41", updated_at: "2019-03-18 18:51:41">
{% endhighlight %}

In the above console output, the first thing to notice is the prompt. The values in brackets are `[service name][Rails environment][current tenant's account_id]`

Next is the alias command `st` which is short for `switch-tenant`. Invoking this command without a parameter will lists all tenants.
When passed a parameter of the  index value of the tenant as in `st 2` then the tenant with index 2 is selected. In this case it is the tenant
with `account_id` 222_222_222

Next is the command `uf` which is short for `User.first`. The core gem creates shortcut methods for all models in a service using the first letter of the model name and a second letter which can be:
f - first
l - last
a - all
c - create; create will invoke the Factory to create a new object

To access the list of ros console commands available type
{% highlight ruby %}
help ros
{% endhighlight %}


#### 4.2 Viewing the IAM Users with Postman

Create a Postman team; commit code to repository; CI will auto generate OpenAPI V3 documentation which can be used by Postman as a collection.


### [How to Get Started with Postman](getting-started-postman)

rails app:ros:doc
output written to app_root/tmp/docs

https://github.com/postmanlabs/openapi-to-postman
postman schema docs: https://schema.getpostman.com/

POST to url 'https://api.getpostman.com/collections'

with API KEY



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
