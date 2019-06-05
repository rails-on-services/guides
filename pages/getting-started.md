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

#### [3.2 Creating the Acme Company Platform](#creating-the-acme-company-platform)

Ros comes with a number of scripts called generators that are designed to make your development life easier by creating everything that's necessary to start working on a particular task.
One of these is the new project generator, which will provide you with the foundation of a fresh Ros platform so that you don't have to write it yourself.

To use this generator, open a terminal, navigate to a directory where you have rights to create files, and type:

{% highlight bash %}
ros new acme
{% endhighlight %}

This will create a Ros platform called Acme in the acme directory

After you create the acme platform, switch to its folder:

{% highlight bash %}
cd acme
{% endhighlight %}

The store directory has a number of auto-generated files and folders that make up the structure of a Ros platform.
Here's a basic rundown on the function of each of the files and folders that Ros created by default:


| File/Folder   | Purpose |
| ------------- | ------------- |
| config/       | Configure your platform's deployments, environments and more. This is covered in more detail in Configuring Ros Platforms. |
| devops/       | Compose, Skaffold, terraform and ansible code to manage the platform in various infrastructure environments |
| Dockerfile    | This file contains the instructions for building each of your microservice images |
| lib/          | Extended modules for your platform including core and sdk gems and generator templates |
| ros/          | The core services of the platform, IAM, Cognito, etc |
| Rakefile      | This file locates and loads tasks that can be run from the command line. The task definitions are defined throughout the components of Ros. |
| README.md     | This is a brief instruction manual for your application. You should edit this file to tell others what your application does, how to set it up, and so on. |
| services/     | Contains a Rails application for each microservice you create. You'll focus on this folder for the remainder of this guide. |
| tmp/          | Temporary files (like cache and pid files). |
| .dockerignore | This file tells docker which files (or patterns) it should ignore when building images. |
| .gitignore    | This file tells git which files (or patterns) it should ignore. See GitHub - Ignoring files for more info about ignoring files. |
| .ruby-version | This file contains the default Ruby version. |


### 4 Hello, Ros!

To begin with, let's get the core services up and running quickly.

{% highlight bash %}
cd acme
ros build iam
{% endhighlight %}

This will build the IAM image. Then we need to create the database and seed it with test data:

{% highlight bash %}
ros ros:db:reset:seed iam
{% endhighlight %}

This will seed each service database with two tenants for playing around with.
Each tenant has a nine digit `account_id`. The two development tenants have `account_id` 111111111 and 222222222
In addition, the IAM service also created a root account and an IAM User with `Administrator` policy attached.

Now to start the IAM service and check that it is running

{% highlight bash %}
ros up -d iam
ros ps
{% endhighlight %}

{% highlight bash %}
      Name                     Command                State              Ports
---------------------------------------------------------------------------------------
acme_iam_1          bundle exec rails server - ...   Up         0.0.0.0:32984->3000/tcp
acme_nginx_1        nginx -g daemon off;             Up         0.0.0.0:3000->80/tcp
acme_postgres_1     docker-entrypoint.sh postgres    Up         0.0.0.0:32959->5432/tcp
acme_redis_1        docker-entrypoint.sh redis ...   Up         0.0.0.0:32960->6379/tcp
acme_wait_1         /wait                            Exit 0
{% endhighlight %}


#### 4.1 Setup Credentials

To show the generated test credentials, run

{% highlight bash %}
ros ros:iam:credentials:show
{% endhighlight %}

{% highlight bash %}

Credentials for https://api.ros.rails-on-services.org

[111111111_root]
acme_access_key_id=AICGPTFZDXYNQVSDPVXW
acme_secret_access_key=1B4yv0WT9FqwmjeqNkNOjMjlLhNh690VCatAvNz48OZia_0rzA9pNw

[222222222_root]
acme_access_key_id=ASZEMSMCTVCVOCTBAVBT
acme_secret_access_key=6_1OV7fk4FMAPjMW81I0YuMRZW5Xv77Q-AZLHBCTXQJlzt-yY147Kw

[222222222_Admin_2]
acme_access_key_id=AFJZLEKIOLQKHYHHHROP
acme_secret_access_key=R8ksVUv681NArqe05QaJTGekX6vAHG79gt-LOC4so-PkRlT3MGiv2A


Postman
{"name":"acme-222_222_222-Admin_2","values":[{"key":"authorization","value":"Basic AFJZLEKIOLQKHYHHHROP:R8ksVUv681NArqe05QaJTGekX6vAHG79gt-LOC4so-PkRlT3MGiv2A"},{"key":"username","value":"Admin_2"},{"key":"password","value":"asdfjkl;"}]}
{"name":"acme-111_111_111-root@platform.com","values":[{"key":"authorization","value":"Basic AICGPTFZDXYNQVSDPVXW:1B4yv0WT9FqwmjeqNkNOjMjlLhNh690VCatAvNz48OZia_0rzA9pNw"},{"key":"email","value":"root@platform.com"},{"key":"password","value":"asdfjkl;"}]}
{"name":"acme-222_222_222-root@client2.com","values":[{"key":"authorization","value":"Basic ASZEMSMCTVCVOCTBAVBT:6_1OV7fk4FMAPjMW81I0YuMRZW5Xv77Q-AZLHBCTXQJlzt-yY147Kw"},{"key":"email","value":"root@client2.com"},{"key":"password","value":"asdfjkl;"}]}
{% endhighlight %}

To use Postman to hit the server copy and past one of the lines above as raw text into Postman

#### 4.2 Calling the API with Postman

Viewing the IAM Users

#### 4.3 Calling the API using the SDK

Check the ~/.ros/credentials file

set the ROS_PROFILE env var and use the SDK to Access IAM Users

Notice the attached_permissions hash


### 5
#### 5.1 Access the IAM Service

As you can see from the above output, you actually already have a functional IAM service and a few others.
To see it, you need to start a web server on your development machine. You can do this by running the following in the ros-iam directory:


Exit from the IAM console. We'll now run a rake task to display the generated credentials so you can access the services using Postman
{% highlight bash %}
ros console iam
{% endhighlight %}

#### 5.2 Navigating the Tenants

When you seeded the IAM database `db/seeds/development/data.seeds.rb` two tenants were created and a User with credentials

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


#### 5.3 Notice the Credentials

Note the start with 'acme'. Talk about the URN


### 6. Create a New Service

The Acme company loves to sell widgets. In order to get selling we will first create an inventory service

{% highlight bash %}
ros g service inventory
{% endhighlight %}

Notes about what gets created
Have to update the config/deployment.yml with the new service

The service isn't running yet, so we need to do that

{% highlight bash %}
ros server
{% endhighlight %}

TODO: the server command needs to generate the provision files and run the migrations


### 7. Create an Endpoint for the New Service

{% highlight bash %}
cd services/inventory
ros g endpoint product name description price:integer
{% endhighlight %}

This will create a bunch of stuff. Paste the output here.


#### 7.3 See the model added to the SDK

{% highlight bash %}
cd ../lib/sdk
bin/console
{% endhighlight %}

TODO: add some stuff to get the credential or set the profile

{% highlight bash %}
Acme::Sdk.service_endpoints
Acme::Inventory::Product.create(name: 'Ros', description: 'Amazing Microservice Platform!', price: 0)
{% endhighlight %}


### Console Shortcuts

[x]console shortcuts from gem are not showing up in the app; add initializer


### Development Seeds

The core provides a default seed data for development for each of the services.
By default two tenants are created in every service which schma names 111_111_111 and 222_222_222

[x]Get gem's seeds working in enclosing app

### [10 What's Next?](#whats-next)
Now that you have a basic Ros project, the next step is to setup [services](services.html) to build out the infrastructure

Remember you don't have to do everything without help. As you need assistance getting up and running with Rails, feel free to consult these support resources:

