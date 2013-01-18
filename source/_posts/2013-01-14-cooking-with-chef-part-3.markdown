---
layout: post
title: "Cooking with Chef, part 3"
date: 2013-01-21 08:00
comments: false
published: false
author: Hart Hoover
categories: 
- Cloud Servers
- Chef
---
![](/a/2013-01-09-cooking-with-chef/chef_logo.png "Chef Logo")

This is the third post in a series on Chef:

* Part one: Introduction to Chef
* Part two: Cookbooks and Deploying with Knife

In those posts I used Opscode's Hosted Chef platform. There are three options for running the Chef Server:

* **Hosted Chef**: Hosted Chef is a version of a Chef server that is hosted by Opscode. Hosted Chef is cloud-based, scalable, and available (24x7/365), with resource-based access control. Hosted Chef has all of the automation capabilities of Chef, but without requiring it to be set up and managed from behind the firewall.
* **Private Chef**: Private Chef is a version of a Chef server that is designed to provide all of the infrastructure automation capabilities of Chef, set up and managed from within the organization.
* **Open Source Chef**: Open Source Chef is an open source version of the Chef server that contains much of the same functionality as Hosted Chef, but requires that each instance be configured and managed locally, including performing data migrations, applying updates to the Open Source Chef server, and ensuring that the Open Source Chef server scales as the local infrastructure it is supporting grows. Open Source Chef includes support from the Chef community, but does not include support directly from Opscode.

This post is focused on running your own Open Source Chef installation.

##Why would I want to run Chef on my own?
You may want to run more than 5 nodes without a monthly fee. You may want to manage how Chef itself gets updated. You may want to host Chef privately, but without the expense of Private Chef. Whatever the reason, installing the Chef server will help you further understand how Chef works.

##Get started
For this example I'll be using Ubuntu 12.10. Opscode provides packages via an APT repository for Debian and Ubuntu servers, which makes this process a lot easier. First, I'll create a server using the `nova` client with my SSH key:

	nova boot --image 8a3a9f96-b997-46fd-b7a8-a9e740796ffd --flavor 3 --file /root/.ssh/authorized_keys=/Users/hhoover/.ssh/id_rsa.pub chefserver

