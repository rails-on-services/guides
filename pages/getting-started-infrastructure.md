---
layout: page
title: Getting Started with Rails On Services
permalink: /getting-started-infrastructure.html
---

Getting Started with Infrastructure
======================================


<div class="summary" markdown="1">
<br/><br/>
After reading this guide, you will know:

<b>How to create Terraform code to stand up an instance and Kubernetes cluster for a Rails on Services project deployment
<p></p>
<b>The required elements of the infrastructure</b>
<p></p>

<b>How to quickly generate the configuration and templates for your provider</b>
<br/><br/>
</div>

* TOC
{:toc}

--------------------------------------------------------------------------------

### [1 Creating a New Rails on Services Project](#creating-a-new-rails-on-services-project)

The best way to read this guide is to follow it step by step. All steps are essential to standup the infrastructure

#### [1.1 Installing Rails on Services](#installing-rails-on-services)

Rails on Services is currently under heavy development. We have a separate repo for setting up the project. Please see [Setup](https://github.com/rails-on-services/setup)

### [2 Creating a new Provider](#creating-a-new-provider)

#### [2.1 Terraform](#terraform)

We use terraform for infra providers blah blah

look at the gcp provider. Our goal is to create a new provider based on this one.


#### [2.2 Start with a copy of existing provider](#start-with-a-copy)

We will create an instance with a VPC, dns, etc

1. Create a profile configuration from an existing configuration:

{% highlight bash %}
cd config/deployments
cp development-gcp.yml development-your-provider.yml
{% endhighlight %}

2. Set the values in the config file

{% highlight yaml %}
components:
  be:
    components:
      infra:
        config:
          cluster:
            type: instance
        components:
          vpc:
            config:
              provider: your-provider
{% endhighlight %}

Note: Values are exposed as variables in the template your-provider-main.tf

3. Create your TF modules



### [5 Stand up the infrastructure](#standup-the-infrastructure)


{% highlight bash %}
ros generate:be:application:infra
ros be infra up
{% endhighlight %}
