---
title: "Power of Chef Tool Automation - 1"
seoTitle: "Chef Tool Automation Unleashed"
seoDescription: "Learn how to automate recipes using Chef, set up servers, bootstrap nodes, and efficiently manage cookbooks with this comprehensive guide"
datePublished: Thu Jun 27 2024 03:40:35 GMT+0000 (Coordinated Universal Time)
cuid: clxwpvdn900000al96ykb3gxq
slug: power-of-chef-tool-automation-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1719456421077/4009d6ec-f81f-4872-ac73-cea79145ffa6.jpeg
tags: cpp, blogging, bootstrap, devops, devops-devopscommunity-sreengineer-sre-knowledge-pe-platformengineering-terraform-ansible-docker-kubernetes-jenkins-jfrog-nexus-prometheus-google-gcp-aws-aws-cloudformation-iac-selenium-maven-jira-chef-puppet-datadog-splunk-spinnaker-openshift

---

Before we start automating the execution, let's explore the Runlist and how to execute different recipes from various cookbooks simultaneously.

A Runlist is a space where the recipes we want to run are kept in order. It contains the commands used to run different recipes.

1. To run one recipe from one cookbook
    
    ```bash
    chef-client -zr "recipe[cookbook_name1::recipe_name1], recipe[cookbook_name2::recipe_name2]"
    #recipe1 belongs to cookbook1, recipe2 belongs to cookbook2
    ```
    
2. To run multiple recipes from a single cookbook
    
    ```ruby
    #Here we use the default.rb file to include all the recipes, then run the default file
    include-recipe "cookbook_name1::recipe_name1"
    include-recipe "cookbook_name1::recipe_name2"
    ```
    
    ```bash
    chef-client -zr "recipe[cookbook_name1::default]"
    ```
    
3. To run multiple recipes from multiple cookbook
    
    ```bash
    #Here, also add all the recipes from different cookbooks in their specific default files, and then execute them
    chef-client -zr "recipe[cookbook_name1::default], recipe[cookbook_name2::default]"
    ```
    

## Connect Workstation with Chef-Server

Steps to set up a server online so it can connect to Linux:

1. Open your browser, go to [manage.chef.io](http://manage.chef.io), and create an account.
    
2. Download the Chef-Starter kit after creating and naming an organization.
    
3. Install *WinSCP* software, which helps transfer files between different operating systems. We will use it to transfer files from Windows to Linux.
    
4. Unzip the file and transfer the chef-repo folder from Windows to Linux using *WinSCP*. Use the login credentials of your EC2 user machine from the Amazon console.
    
5. Open the Linux machine and start working inside the chef-repo. You can check the config.rb file to see that the server is connected to the workstation.
    
6. To verify the connection, run this command:
    

1. ```bash
    knife ssl check
    ```
    

## Bootstrap a Node

Connecting a node to the Chef server is called bootstrapping. Create a new Linux machine and consider it as a node, for example, Node1. Now, from the workstation, run this command to connect the Chef server with Node1:

```bash
#Write your own public ip address of the node in the command
knife bootstrap 172.31.21.88 --ssh-user ec2-user --sudo -i node-key.pem -N Node1
```

To check if server is connected with the node:

```bash
knife node list
```

From now on, create all cookbooks in the default folder of the chef-repo. This is a good practice. Now, let's upload a cookbook from the workstation to the chef-server so that the chef-client can pull the content into the node.

Write the first three commands in the workstation and the fourth command in the node.

1. To upload cookbook to chef-server
    
    ```bash
    knife cookbook upload cookbook_name1
    ```
    
2. To check whether it's uploaded or not
    
    ```bash
    knife cookbook list
    ```
    
3. To attach recipe which we would like to run on node
    
    ```bash
    knife node run_list set Node1 "recipe[cookbook_name1::recipe_name1]"
    ```
    
    Whenever the recipe is updated, the changes will occur on the node too, automatically.
    

To run this recipe manually from the Node1, we haven't automated it fully yet

```bash
#As you can see, now we don't write the full command, which included -zr & recipe & cookbook name
chef-client
```

If you don't want to keep writing the `chef-client` command repeatedly, you can automate this process. Whenever you update the Chef server, the node will automatically check the server and update itself at specific intervals.

1. Open Node1, use vi editor to open this folder
    
    ```bash
    #Crontab is a place, where it lets user schedule a process
    vi /etc/crontab
    ```
    
2. Write this command inside vi editor to automate the process
    
    ```bash
    # * * * * *
    # | | | | |------- day of week
    # | | | ---------- month
    # | | ------------ day of month
    # | -------------- hour
    # ---------------- minute
    * * * * * root chef-client
    #It will run chef-client command in for root user, at the ***** interval of time
    ```
    
    That's how we automate the process of pulling the content and running it from the node. Soon, we might also try to automate bootstrapping.