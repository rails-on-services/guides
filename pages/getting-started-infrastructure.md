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

Rails on Services is currently under heavy development. We have a separate repo for setting up the project.
Please see [Setup](https://github.com/rails-on-services/setup) and complete:

1. [Virtual Mashine Setup](https://github.com/rails-on-services/setup#virtual-machine-setup) section to launch development environment
2. [Verify VM Configuration](https://github.com/rails-on-services/setup#verify-vm-configuration)

Once Vagrant is up and running, the directory from which you ran `vagrant ssh` is mounted in the VM.
You can use a text editor of your choice on Host OS and run ROS CLI commands on the VM to test your changes.

**All subsequent commands are executed in the VM**

Assuming that you ran `vagrant ssh` from your host OS dir of `~/my-project` and you have just exected `vagrant ssh` so in the VM:

{% highlight bash %}
cd my-project
ros new my-platform
cd my-platform
ros be init
{% endhighlight %}

Your folder structure in the VM should look like below and further will be referenced as root folder.
{% highlight bash %}
tree ~ -L 3
.
|-- my-project
|   |-- my-platform
|   |   |-- ansible.cfg
|   |   |-- avp
|   |   |-- config
|   |   |-- Rakefile
|   |   |-- README.md
|   |   |-- ros
|   |   `-- VERSION
|   '-- ros
|   |   |-- cli
|   |   |-- guides
|   |   |-- images
|   |   `-- setup
|   |-- Vagrantfile
{% endhighlight %}

### [2 ROS configuration files explanation](#config-file-explanation)
The `~/my-project/my-platform/ros/config` directory has the following contents:
{% highlight bash %}
.
|-- deployments
|   |-- development-specs.yml
|   |-- development.yml
|   |-- gcp.yml
|   |-- production.yml
|   `-- test.yml
|-- deployment.yml
`-- environment.yml
{% endhighlight %}

`deployment.yml` is a master configuration file which values applies to any deployment managed by ROS CLI.

Deployment configuration files in `deployments` folder will override any values defined in `deployment.yml`.

Deployment configuration file usage determined by `ROS_ENV` environmental variable passed to ROS CLI upon runtime.

Default deployment configuration file is  `development.yml` if no `ROS_ENV` been set.

Setting `ROS_ENV=test` tell ROS CLI to use `test.yml` config.

### [3 Creating a new Provider](#creating-a-new-provider)
<!--
#### [3.1 Terraform](#terraform)

We use terraform for infra providers blah blah

look at the gcp provider. Our goal is to create a new provider based on this one.

-->

We use terraform for infrastructure providers.

In following example we will create an GKE instance with a VPC.

#### [3.1 Start with creating a config file](#start-with-config)

* **Create a profile configuration file**

{% highlight bash %}
cd ~/my-project/my-platform/ros/config/deployments
cp gcp.yml your-provider.yml
{% endhighlight %}

* **Set the values in the config file**

{% highlight yaml %}
components:
  be:
    components:
      infra:
        config:
          cluster:
            type: instance
        components:
          provider_settings:
            config:
              provider: gcp
              region: us-central1
              zone: us-central1-a
              project: 'my-project-name'
              credentials_file: 'path/to/credentials.json'
          vpc:
            config:
              provider: gcp         #other supported types: azure, oracle
              vpc_name: my-test-vpc
              subnet_name: my-test-subnet
              cidr: '10.100.0.0/16'
          instance:
            config:
              provider: gcp
              name: my-test-instance
              machine_type: 'f1-micro'
              disk_image: 'debian-cloud/debian-9'
          dns:
            config:
              provider: gcp
{% endhighlight %}

_Note on empty `dns` module declaration. This is to override default dns settings declared in deployment.yml_

Config values from `your-provider.yml` are exposed as variables in the template `your-provider-main.tf`

Edit this file and add the variables and values that are required to configure your provider's infrastructure.

### [3.2 Create your TF template](#create-tf-template)

We are done with editing ROS project for now. Lets go back to our project root folder and update CLI project.

Terraform temlpates stored at `~/my-project/ros/cli/lib/ros/be/infra/templates/terraform`
{% highlight bash %}
cd ~/my-project/ros/cli/lib/ros/be/infra/templates/terraform
mkdir your-provider   # NOTE: folder name *must* match your provider name set in your-provider.yml
touch your-provider/instance.tf.erb
{% endhighlight %}

Following template is sufficient to generate a correct `main.tf` file.
Note on ERB variables declaration as `tf.component.config.value`

{% highlight terraform %}
provider "google" {
  credentials = "${file("<%= tf.provider_settings.config.credentials_file %>")}"
  project     = "<%= tf.provider_settings.config.project %>"
  region      = "<%= tf.provider_settings.config.region %>"
}

module "vpc" {
  source              = "./gcp/vpc"
  vpc_name            = "<%= tf.vpc.config.vpc_name %>"
  create_subnetworks  = false  
  subnet_name         = "<%= tf.vpc.config.subnet_name %>"
  cidr                = "<%= tf.vpc.config.cidr %>"
}

module "gci" {
  source       = "./gcp/gci"
  name         = "<%= tf.instance.config.name %>"
  machine_type = "<%= tf.instance.config.machine_type %>"
  disk_image   = "<%= tf.instance.config.disk_image %>"
  zone         = "<%= tf.provider_settings.config.zone %>"
  subnetwork   = module.vpc.subnetwork.self_link
}
{% endhighlight %}

### [ 3.3 Create Terraform configuration files](#create-tf-config)

Terraform configuration files stored at `cli/lib/ros/be/infra/files/terraform
{% highlight bash %}
cd ~/my-project/ros/cli/lib/ros/be/infra/files/terraform/
mkdir your-provider   # NOTE: folder name *must* match your provider name set in your-provider.yml
cd your-provider
mkdir instance-type vpc dns
{% endhighlight %}

Now put your terraform resource declaration files, variables and outputs into corresponding folders.
Examples are shown below and can be found in `~/my-project/ros/cli/lib/ros/be/infra/files/terraform/{gcp, aws}`

### [ 3.4 Working examples of Terraform provider files](#working-examples)

For reference, here is what the aws and gcp providers content looks like.
In the `files` directory there is a set of files for each infrastructure component
In the `templates` directory there is a single ERB template file for each infra deployment type: `instance` or `kubernetes`

{% highlight bash %}
~/my-project/ros/cli/lib/ros/be/infra$ tree files templates
files
`-- terraform
    |-- aws
    |   |-- acm
    |   |   |-- main.tf
    |   |   |-- outputs.tf
    |   |   `-- variables.tf
    |   |-- ec2
    |   |   |-- locals.tf
    |   |   |-- main.tf
    |   |   |-- outputs.tf
    |   |   |-- templates
    |   |   |   |-- userdata-debian.tpl
    |   |   |   `-- userdata-ubuntu.tpl
    |   |   `-- variables.tf
    |   |-- eks
    |   |   `-- main.tf
    |   |-- route53
    |   |   |-- main.tf
    |   |   |-- outputs.tf
    |   |   `-- variables.tf
    |   `-- vpc
    |       |-- main.tf
    |       |-- outputs.tf
    |       `-- variables.tf
    `-- gcp
        |-- gci
        |   |-- main.tf
        |   |-- outputs.tf
        |   `-- variables.tf
        `-- vpc
            |-- main.tf
            |-- outputs.tf
            `-- variables.tf
templates
`-- terraform
    |-- aws
    |   |-- instance.tf.erb
    |   `-- kubernetes.tf.erb
    `-- gcp
        `-- instance.tf.erb
{% endhighlight %}

### [4 Set components mapping](#set provider mapping)

As final touch, we set components (declared in gcp.yml) mapping to terraform modules mapping in our template generator.
{% highlight bash %}
vim ~/my-project/ros/cli/lib/ros/be/infra/generator.rb
{% endhighlight %}

Update gcp type with following values:

{% highlight ruby%}
def gcp(type)
  {
    vpc: 'vpc',
    instance: 'gci'
  }[type]
end
{% endhighlight %}

### [5 Stand up the infrastructure](#standup-the-infrastructure)



You should now be able to launch simple infrastructure consisting of a single VPC and GCI:

{% highlight bash %}
cd ~/my-project/my-platform/ros
{% endhighlight %}

Generate Terraform scripts out of your templates
{% highlight bash %}
ros generate:be:infra
{% endhighlight %}

Generate the Terraform plan
{% highlight bash %}
ros be infra plan
{% endhighlight %}

Apply your Terraform infrastructure plan
{% highlight bash %}
ros be infra apply
{% endhighlight %}

Use `-v` and/or `-n` for verbosity and/or dry-run respectively.

Use `ros be infra help` to find out commands description.
