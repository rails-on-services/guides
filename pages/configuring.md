---
layout: page
title: Configuring
permalink: /configuring.html
---
<div class="summary" markdown="1">
<br/>

After reading this guide, you will know:

<b>How to configure a new service</b>

<b>How to override default Service settings</b>

<b>How to settings for a Kubernetes deployment</b>
<br/><br/>
</div>

[ros gems](https://github.com/rails-on-services/ros)

Core uses the [config gem](https://github.com/railsconfig/config) to expose Platform configuration

### [Platform Settings](#platform-settings)

In the core gem in config/settings.yml are the default values

{% highlight yaml %}
partition: ros
region: ''
service:
  name: iam
  policy_name: Iam

{% endhighlight %}

### Configuration in a Service

{% highlight ruby %}
Settings.service.name = ['whistler-api.perxtech.org']
{% endhighlight %}

### Override with Environment variables

List of ENVs to export for compose:
{% highlight bash %}
export RAILS_MASTER_KEY=
export PLATFORM__SERVICE__PARTITION_NAME=perx
export PLATFORM__JWT__ISS=perx
export PLATFORM__HOSTS = whistler-api.perxtech.org,another-host-name
{% endhighlight %}

### URN

`URN` is short for universal resource name

[x]confirm that iam, cognito, etc when running use the partition_name perx


<!---
To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.
Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
--->
