---
layout: page
title: Cognito
permalink: "/cognito.html"
---
<div class="summary" markdown="1">
<br/>
After reading this guide, you will know:

<b>How to use the Cognito Service, create a User Pool and Users, and access Users from your business services</b>
<br/><br/>
</div>

* TOC
{:toc}


### 1. User Login


The middleware is executed before it hits the controller. The user must submit a valid JWT with their new user signup or login request
The JWT already has IAM credentials meaning that the request is already authenticated to the tenant

The use case is this:

User goes to a web site that is specific to the tenant. The user submits login credentials. The endpoint they submit to is an express server.
The express server uses its tenant credentials to make a request to the cognito service on behalf of the user
so there is already a JWT present in the request. The tenant middleware uses that to switch to the correct tenant before processing the login request.
the middleware is in the core gem as it applies to every service. `ros/lib/core/lib/ros/tenant_middleware.rb`

The cognito service always processes a user login/signup that is in the context of a tenant. If there are no valid tenant credentials in the request it will return a 403.
The cognito login's primary function is to authenticate the user and add a claim ognito_subto the JWT that is returned to the requestor

### 3. Background Jobs

workers for importing cognito pools and users
