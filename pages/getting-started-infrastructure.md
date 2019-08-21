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

<div class="summary" markdown="1">
<b>In following guide we'll learn to extend ROS CLI with Google Cloud Platform infrastructure provider based on existing AWS provider</b>
</div>

* TOC
{:toc}

--------------------------------------------------------------------------------

### [1 Creating a New Rails on Services Project](#creating-a-new-rails-on-services-project)

The best way to read this guide is to follow it step by step. All steps are essential to standup the infrastructure

#### [1.1 Installing Rails on Services](#installing-rails-on-services)

Rails on Services is currently under heavy development. We have a separate repo for setting up the project. Please see [Setup](https://github.com/rails-on-services/setup) and complete [Virtual Mashine Setup](https://github.com/rails-on-services/setup#virtual-machine-setup) section to launch development environment.

Once Vagrant is up and running your current project folder is mounted into VM. You can use a text editor of your choice on Host OS and run ROS CLI commands on virtual machine to test your changes.

Enter ```your-project-name/ros``` folder and clone existing ROS repo. We'll use it as an example.
{% highlight bash %}
cd your-project-name/ros
git clone https://github.com/rails-on-services/ros.git
{% endhighlight %}

Your folder structure in ```your-project-name/ros``` should look like below and further will be referenced as root folder.
{% highlight bash %}
.
├── cli
├── guides
├── images
├── ros
└── setup
{% endhighlight %}

### [2 ROS configuration files explanation](#config-file-explanation)
Enter ```ros/config``` folder. Which contents looks like that:
{% highlight bash %}
.
├── deployment.yml
├── deployments
│   ├── development.yml
│   ├── production.yml
│   └── test.yml
└── environment.yml
{% endhighlight %}

```deployment.yml``` is a master configuration file which values applies to any deployment managed by ROS CLI.

Deployment configuration files in ```deployments``` folder will override any values defined in ```deployment.yml```.

Deployment configuration file usage determined by ```ROS_ENV``` environmental variable passed to ROS CLI upon runtime.

Default deployment configuration file is  ```development.yml``` if no ```ROS_ENV``` been set.

Setting ```ROS_ENV=test``` tell ROS CLI to use ```test.yml``` config.

### [3 Creating a new Provider](#creating-a-new-provider)
<!--
#### [3.1 Terraform](#terraform)

We use terraform for infra providers blah blah

look at the gcp provider. Our goal is to create a new provider based on this one.

-->
#### [3.1 Start with creating a config file](#start-with-a-copy)

We will create an GKE instance with a VPC

* Create a profile configuration file:

{% highlight bash %}
cd config/deployments
touch gcp.yml
{% endhighlight %}

* Set the values in the config file

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
              provider: gcp   #other supported types: azure, oracle
              name: my-test-vpc
              subnet: '10.100.0.0/16'
          gci:
            config:
              provider: gcp
              name: my-test-instance
              machine_type: 'f1-micro'
{% endhighlight %}

_Note: Config values are exposed as variables in the template your-provider-main.tf_

* Create your TF modules



### [5 Stand up the infrastructure](#standup-the-infrastructure)


{% highlight bash %}
ros generate:be:application:infra
ros be infra up
{% endhighlight %}
