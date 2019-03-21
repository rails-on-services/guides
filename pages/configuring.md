---
layout: page
title: Configuring
permalink: '/configuring.html'
# date:   2019-03-21 20:40:06 +0800
# categories: configuration
---
## Configuring Rails On Services

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

### URN

[x]confirm that iam, cognito, etc when running use the partition_name perx

List of ENVs to export for compose:
{% highlight bash %}
export RAILS_MASTER_KEY=
export PLATFORM__SERVICE__PARTITION_NAME=perx
export PLATFORM__JWT__ISS=perx
{% endhighlight %}

{% highlight ruby %}
Settings.config.hosts = ['whistler-api.perxtech.org']
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
