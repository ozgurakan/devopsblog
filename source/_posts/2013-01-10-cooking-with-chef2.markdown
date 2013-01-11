---
layout: post
title: "Cooking with Chef, part 2"
date: 2013-01-14 08:00
comments: false
published: false
author: Hart Hoover
categories: 
- Cloud Servers
- Chef
---
![](/a/2013-01-09-cooking-with-chef/chef_logo.png "Chef Logo")

Continuing the series on Chef that I started in [part one](), you should now have an account with Opscode and a working knife install. In this post, I will explain how cookbooks and roles work and we will deploy our first application with Chef.

##Cookbooks?
That's right, Emeril Lagasse, managing infrastructure with Chef means you are using cookbooks. Cookbooks are the way Chef users package, distribute, and share configuration details. A cookbook usually configures only one service. Let's look at the structure of a common cookbook like [Apache](https://github.com/opscode-cookbooks/apache2):

```bash
knife cookbook site install apache2
```

    apache2
		attributes
		definitions
		files
		recipes
		templates
		test
		CHANGELOG.md
		CONTRIBUTING.md
		Gemfile
		LICENSE
		README.md
		metadata.rb

The first directory to look into is `attributes`, specifically `default.rb`. Here you can see all the things you can set for your Apache installation. Your recipes will read the attributes files for configuration details. You can see that there are different settings for different operating systems, meaning you can run this cookbook against Ubuntu, Red Hat, and other servers. This means the underlying operating system doesn't really matter - your Apache settings matter.

The next directory to look into is `recipes`. Recipes are Ruby files in which you use Chef's Domain Specific Language (DSL) to define how particular parts of a node should be configured. The default.rb file will be run through first to install the service, followed by module recipes. Inside a recipe you will see something similar to this:

```ruby
when "debian"

  package "libapache2-mod-php5"

when "rhel"

  package "php package" do
    if node['platform_version'].to_f < 6.0
      package_name "php53"
    else
      package_name "php"
    end
    notifies :run, "execute[generate-module-list]", :immediately
    not_if "which php"
  end
```

As you can see again, the cookbook is making decisions on what to do based on the operating system of the instance. In the case of Red Hat, it makes a decision on which version of PHP to install packages based on the version of RHEL you are running. There are hundreds of [community cookbooks](http://community.opscode.com/cookbooks) available to choose from, or you can [write your own](http://wiki.opscode.com/display/chef/Guide+to+Creating+A+Cookbook+and+Writing+A+Recipe).

Last but not least, look into the `templates` directory. This directory holds templates for your Apache configuration files. The template is filled in with information from your recipes and attributes files. Here is a snippet from the `apache2.conf` template, called `apache2.conf.erb`:

    ServerRoot "<%= node['apache']['dir'] %>"

This is typically set to /var/www but you can set it to whatever directory you like programmatically. Now that you have a very general idea of how cookbooks and recipes work, let's figure out how to apply these cookbooks to servers.

#Roles
We need to know how to define which servers get what cookbooks. Roles allow us to do that in Chef. Each role you specify would have a list of recipes it requires and in what order. In the Chef repository you should have from the first post in this series you have a roles directory:

	chef-repo/
		.chef/
		certificates/
		config/
		cookbooks/
		data_bags
		environments/
		roles/   <=====


#Deploying with Knife
