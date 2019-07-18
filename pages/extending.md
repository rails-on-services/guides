---
layout: page
title: Extending Rails On Services
permalink: /extending.html
---

Getting Started with Rails On Services
======================================


<div class="summary" markdown="1">
<br/><br/>
After reading this guide, you will know:

<b>How to use the built in generators to create new services and service endpoints.</b>
<p></p>
<b>How to override the built in generators to customize service and infrastructure configuration.</b>
<p></p>


* TOC
{:toc}

--------------------------------------------------------------------------------


Ros generators generate new projects, new services within those projects and endpoints within a service.

## Service Generaotr

When you run `ros g service generator` templates are invoked to create at new rails application

If you want to look at the templates used ot generate artifacts they live in ros/lib/core/lib/generators/be/application/platform/rails/templates


### Override Templates

You can override the template that is used to generate that service attribute

By placing a template in PROJECT_ROOT/lib/generators/be/application/platform/rails/templates

## Infrastructure Generators


