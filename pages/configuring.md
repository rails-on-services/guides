---
layout: page
title: Configuring
permalink: /configuring.html
# date:   2019-03-21 20:40:06 +0800
# categories: configuration
---
## Configuring Rails On Services


[ros gems](https://github.com/rails-on-services/ros)

### URN

[x]confirm that iam, cognito, etc when running use the partition_name perx


### Service Settings

In the core gems in config/settings.yml are the defaults

{% highlight yaml %}
config:
  hosts: whistler-api.perxtech.org
{% endhighlight %}

### Override with Environment variables

Core uses the [config gem](https://github.com/railsconfig/config) to expose Platform configuration

List of ENVs to export for compose:
{% highlight bash %}
export RAILS_MASTER_KEY=
export PLATFORM__SERVICE__PARTITION_NAME=perx
export PLATFORM__JWT__ISS=perx
{% endhighlight %}

### Configuration in a Service

{% highlight ruby %}
Settings.config.hosts = ['whistler-api.perxtech.org']
{% endhighlight %}

<!---
To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.
Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
--->
