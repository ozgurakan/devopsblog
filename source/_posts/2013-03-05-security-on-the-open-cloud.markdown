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

I'll go through each one of these in depth on how to mitigate these risks on the Rackspace Open Cloud. We may highlight services you didn't know we had, as well as some of our partner companies from the [Cloud Tools Marketplace](https://cloudtools.rackspace.com/home).

##Identity and Access Management

When you sign up for a Rackspace Cloud account, you are given one username and API key to manage the entire account. What if you have multiple teams? What if you want to use different account credentials for development and production? You used to have to maintain seperate accounts for these functions. Today you can create additional users that can authenticate against the API with their usernames and passwords. First, authenticate:

    curl -X POST https://identity.api.rackspacecloud.com/v2.0/tokens -d '{ "auth":{ "RAX-KSKEY:apiKeyCredentials":{ "username":"$USERNAME", "apiKey":"$APIKEY" } } }' -H "Content-type: application/json"

With your token, you can then create a user:

	curl –X POST https://identity.api.rackspacecloud.com/v2.0/users -d '{"user": {"username": "$USERNAME", "email":"email@domain.com", "enabled": true, "OS-KSADM:password":"$PASSWORD"}}' -H "Content-type: application/json" -H "X-Auth-Token: $TOKEN”

More information on the Identity API is available in the [documentation](http://docs.rackspace.com/auth/api/v2.0/auth-client-devguide/content/Overview-d1e65.html). We also partner with companies that provide application account services like [Stormpath](http://www.stormpath.com/).

##Configuration and Patch Management

You can always use a configuration management platform like Puppet to make sure your packages are up to date:

```ruby
package { "mysql-server":
	ensure => latest
}
```

If you haven't moved to a fully automated infrastructure yet, we offer Rackspace [Managed Cloud](http://www.rackspace.com/cloud/managed_cloud/). With a Managed Service Level, we provide patching services and make sure your servers are always up to date, automagically. New vulnerabilities are being discovered all the time, so it is extremely important to make sure the servers you have online are up to date.

##Endpoint and Network Protection

Did you know you can have isolated servers with [Cloud Networks](http://devops.rackspace.com/protect-your-infrastructure-servers-with-bastion-hosts-and-isolated-cloud-networks.html)?

Rackspace also manages network security for our customers' environments with a physical RackConnect Firewall. We of course also have security measures in place at the hypervisor and datacenter level.

##Vulnerability and Asset Management


##Data Protection

Did you know you can [encrypt data at rest](https://community.rackspace.com/products/f/5/t/66.aspx) with [Cloud Block Storage](http://www.rackspace.com/knowledge_center/article/cloud-block-storage-overview)? 

##Application Security