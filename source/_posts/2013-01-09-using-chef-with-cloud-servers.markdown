---
layout: post
title: "Using Chef with Cloud Servers"
date: 2013-01-09 10:08
comments: false
author: Hart Hoover
categories: 
- Cloud Servers
- Chef
---
Many of our customers use configuration management packages to manage their cloud infrastructure. These packages include Opscode’s [Chef](http://www.opscode.com/chef/), [CFEngine](http://cfengine.com/), Red Hat’s [Spacewalk](http://spacewalk.redhat.com/), and Puppet Labs’ [Puppet](http://puppetlabs.com/puppet/what-is-puppet/). Here, I’ll dive into Chef to show you how easy it is to manage Cloud Servers using a configuration management solution. We’re going to set up Hosted Chef using Opscode's platform and create two servers: a web server and a database server.

##¿Habla usted Chef?
When using any new technology, it's important to know the terms associated with it. Using Chef you will run into the following terms:

* Nodes - these are servers: physical machines or cloud instances.
* Environments - a group of nodes with different functions.
* Roles - a means of grouping nodes with the same function. Nodes can have multiple roles.
* Cookbooks - a collection of recipes, attributes, and templates that chef uses to configure a node.
* Knife - a command line utility to manage Chef.

Here is an example using these terms:

>An Apache cookbook is used to install Apache on nodes that fall into the "webserver" role in the "Production" environment. A MySQL cookbook is used to install MySQL on nodes that fall into the "database" role in my "Production" environment. The "Production" environment is a WordPress blog that has web nodes with Apache/PHP and database nodes with MySQL.

##Get started with Opscode
You can host and manage your own chef server, but Opscode offers a free trial for 5 nodes or less so it's perfect for our purposes. You can create an account for the free trial [here](http://www.opscode.com/hosted-chef/).

Once your account is created, you will need to [set up your local machine as a workstation](http://wiki.opscode.com/display/chef/Workstation+Setup) to manage Chef. The Chef documentation is very detailed on the steps required, which differ depending on your operating system.

##Use Knife with Rackspace
After you have knife installed and configured, you need to install the knife-rackspace plugin:

    gem install knife-rackspace

You will need to add the following to your `knife.rb`:

    knife[:rackspace_api_username] = "Your Rackspace API username"
	knife[:rackspace_api_key] = "Your Rackspace API Key"

If your `knife.rb` file will be checked into a SCM system (ie readable by others) like GitHub or SVN you may want to read the values from environment variables:

    knife[:rackspace_api_username] = "#{ENV['RACKSPACE_USERNAME']}"
	knife[:rackspace_api_key] = "#{ENV['RACKSPACE_API_KEY']}"

Once this is saved you can provision servers with knife. This is a 1GB Ubuntu 12.10 webserver:

    knife rackspace server create -I 8a3a9f96-b997-46fd-b7a8-a9e740796ffd -f 3 -r 'role[webserver]'

##We still need cookbooks!
