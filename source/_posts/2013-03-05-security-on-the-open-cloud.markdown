---
layout: post
title: "Security on the Open Cloud"
date: 2013-03-05 08:00
comments: false
author: Hart Hoover
published: false
categories: 
- Security
- Five Pillars
---
Security is a major concern for all hosting platforms, but in the cloud security has traditionally been a detractor to cloud adoption. Security concerns include:

* Identity and Access Management
* Configuration and Patch Management
* Endpoint and Network Protection
* Vulnerability and Asset Management
* Data Protection
* Application Security

I'll go through each one of these in depth on how to mitigate these risks on the Rackspace Open Cloud. We may highlight services you didn't know we had, as well as some our partner companies from the [Cloud Tools Marketplace](https://cloudtools.rackspace.com/home).

##Identity and Access Management

When you sign up for a Rackspace Cloud account, you are given one username and API key to manage the entire account. What if you have multiple teams? What if you want to use different account credentials for development and production? You used to have to maintain seperate accounts for these functions. Today you can create additional users that can authenticate against the API with their usernames and passwords. First, authenticate:

    curl -X POST https://identity.api.rackspacecloud.com/v2.0/tokens -d '{ "auth":{ "RAX-KSKEY:apiKeyCredentials":{ "username":"$USERNAME", "apiKey":"$APIKEY" } } }' -H "Content-type: application/json"

With your token, you can then create a user:

	curl –X POST https://identity.api.rackspacecloud.com/v2.0/users -d '{"user": {"username": "$USERNAME", "email":"email@domain.com", "enabled": true, "OS-KSADM:password":"$PASSWORD"}}' -H "Content-type: application/json" -H "X-Auth-Token: $TOKEN”

We also partner with companies that provide account services like [Stormpath](http://www.stormpath.com/).

##Configuration and Patch Management

You can always use a configuration management platform like Puppet to make sure your packages are up to date:

```ruby
package { "mysql-server":
	ensure => latest
}
```

If you haven't moved to a fully automated infrastructure yet, we offer Rackspace Managed Cloud. In a Managed environment, we provide patching services and make sure your servers are up to date.

##Endpoint and Network Protection

Did you know you can have isolated servers with [Cloud Networks](http://devops.rackspace.com/protect-your-infrastructure-servers-with-bastion-hosts-and-isolated-cloud-networks.html)?

Rackspace also manages network security for our customers with RackConnect Firewalls and at the data center level.

##Vulnerability and Asset Management
##Data Protection
##Application Security