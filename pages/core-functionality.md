---
layout: page
title: Core
permalink: /core-functionality.html
---

## Getting Started with Cognito

After reading this guide, you will know:

<b>How to install Rails on Services, create a new Rails on Services project, and connect your application to a database.
The general layout of a Rails on Services project.
How to quickly generate the starting pieces of a Rails on Services project.</b>


### Application Controller

[x]add catch to routes when generating app


### Base Classes, Concerns and Inheritance

Concerns:
dummy/app's

{% highlight ruby %}
class ApplicationController < ActionController::API
  include Ros::ApplicationControllerConcern
end
{% endhighlight %}

implemented in: ros-core/app/controllers/concerns/ros/application_controller_concern.rb

dummy/app’s ApplicationRecord < ActiveRecord::Base; include Ros::ApplicationRecordConcern
implemented in: ros-core/app/models/concerns/ros/application_record_concern.rb

Inheritance:
[x]dummy/app’s ApplicationResource < Ros::ApplicationResource; abstract
implemented in: ros-core/app/resources/ros/application_resource.rb
class Ros::ApplicationResource < JSONAPI::Resource; end

[x]dummy/app’s ApplicationPolicy < Ros::ApplicationPolicy
implemented in: ros-core/app/policies/ros/application_policy.rb
class Ros::ApplicationPolicy; end

[x]dummy/app’s ApplicationJob < Ros::ApplicationJob
implemented in: ros-core/app/jobs/ros/application_job.rb
class Ros::ApplicationJob < ActiveJob::Base; end

Engines, e.g. Cognito, have their own name spaced Base Classes
Cognito::ApplicationController < ::ApplicationController; end
CampaignsController < Cognito::ApplicationController

Cognito::ApplicationRecord < ::ApplicationRecord; end
Campaign < Cognito::ApplicationRecord; end

Cognito::ApplicationResource < ::ApplicationResource; end
CampaignResource < Cognito::ApplicationResource; end

Cognito::ApplicationPolicy < ::ApplicationPolicy; end
UserPolicy < Cognito::ApplicationPolicy
