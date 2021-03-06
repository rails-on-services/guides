---
layout: page
title: IAM
permalink: '/iam.html'
---
<div class="summary" markdown="1">
<br/>
After reading this guide, you will know:

<b>How to use the IAM Service, create a new Tenant, and create an IAM user with API credentials.</b>

<b>The use of JWTs in the authentication process.</b>

<b>How to implement Fine grained permissions to platform resources in a Rails on Services project.</b>
<br/><br/>
</div>

* TOC
{:toc}


[x]Implement policies and actions in jsonb in IAM and serialize to services
[x]postman requests don’t go through when using JWT
[x]figure out why User access_key_id has a repeating character every time, e.g. AGIEIFIUIKISIPIRIEIS
[x]Have IAM create Postman config also when seeding root and user



### JWT:
iss: is https://iam.perxtech.net
aud: array of valid processors of the token; IAM issues the token; the aud is ["https://perxtech.net”]; so if survey.perxtech.net receives a token from a client, it checks the aud
See: https://stackoverflow.com/questions/28418360/jwt-json-web-token-audience-aud-versus-client-id-whats-the-difference
sub: is the URN; This is always the IAM User; For tenants using a microsite, an IAM User is created with API credentials deployed to the Express server
iat: issued_at
exp: expires_at
cognito_sub: user.id

Who creates: IAM::User, IAM::Root
Who updates: cognito/login
Who reads: core    
