---
layout: page
title: Services
permalink: /services.html
---
<div class="summary" markdown="1">
<br/>
After reading this guide, you will know:

<b>What additional services to install and configure for the full Rails on Services experience.</b>

<br/><br/>
</div>

* TOC
{:toc}

### 1. CI/CD Configuration

NOTE: Here the Postman API key is important and is configurable

### 2. Developer Tooling

Create a Postman team; commit code to repository; CI will auto generate OpenAPI V3 documentation which can be used by Postman as a collection.


#### [2.1 Create a Postman Team](#create-a-postman-team)

As part of the CI/CD process all API documentation will be written and pushed to a Postman team

rails app:ros:doc
output written to app_root/tmp/docs

https://github.com/postmanlabs/openapi-to-postman
postman schema docs: https://schema.getpostman.com/

POST to url 'https://api.getpostman.com/collections'

with API KEY


Before production
[]exception notification
[]prometheus monitoring
[]request logging
[]fluentd logs

