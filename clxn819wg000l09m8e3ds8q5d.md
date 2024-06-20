---
title: "Chef : Configuration Management Tool"
seoTitle: "Chef: Simplifying Configuration Management"
seoDescription: "Learn about Chef, a popular pull-based configuration management tool used to automate administrative tasks."
datePublished: Thu Jun 20 2024 12:11:21 GMT+0000 (Coordinated Universal Time)
cuid: clxn819wg000l09m8e3ds8q5d
slug: chef-configuration-management-tool
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1718881042139/ec924856-d603-4ead-b9c5-f28b489948d2.png
tags: docker, ruby, bash, kubernetes, learning, devops, devops-articles, configuration-management, devopscommunity

---

### Pull Based Configuration Management Tool

Pull configuration nodes checks out with the servers periodically and fetches the configuration about it. In simple language, here PCs automatically check upon themselves whether they need an update or not? So the PCs pull the updates from the servers.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718881672825/2248a654-7616-447d-84b4-9f9dec2153cd.png align="center")

The most famous tool used for pull based configuration is Chef but some people may also use Puppet.

### What is Chef?

Chef is a famous Pull-Based Configuration tool used to automate a lot of work. Some factoids about it are as follows:

* Chef tool was made using Ruby and Erlang Language.
    
* It's used to automate admin tasks.
    
* It's an open source software.
    

### Chef Architecture

Basically it consists of a workstation, a chef-server and a number of nodes(PCs).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718883408946/5a41e8bf-adc8-453c-984c-a4ca02274513.jpeg align="center")

<details data-node-type="hn-details-summary"><summary>Workstation</summary><div data-type="detailsContent">It consists of cookbooks, recipes and some other already installed files. On workstation we write our code in Ruby language on Linux. Recipes are files in which our code is written and is saved as recipe.rb where '.rb' is for ruby. These recipes are stored in a cookbook and multiple cookbook are stored in a folder called cookbooks.</div></details><details data-node-type="hn-details-summary"><summary>Chef-server</summary><div data-type="detailsContent">In order to push updates on to the chef-server from workstation we use a command called as Knife. Our cookbook is also stored in chef servers.</div></details><details data-node-type="hn-details-summary"><summary>Nodes</summary><div data-type="detailsContent">Different nodes consists of chef-client and ohai. Here, Ohai term is used to call a space or a database which has all the information, specifications &amp; minute configuration about the node. Chef-Client is a tool which first checks whether the thing is already present in the node or not by asking Ohai and if not, then chef-client helps to bring the code or the updates from the servers to nodes.</div></details>

### Installation Process

To install Chef on your Linux machines use these following commands:

```bash
sudo su
```

```bash
wget https://packages.chef.io/files/stable/chef-workstation/0.4.2/el/7/chef-workstation-0.4.2-1.el6.x86_64.rpm
```

```bash
 yum install chef-workstation-0.4.2-1.e16.x86 64.rpm -y
```

Keep in mind version may differ.

### Chef Commands

1. Create a directory which store all the cookbook
    
    ```bash
    mkdir cookbooks
    ```
    
2. Create a file/cookbook inside the cookbooks.
    
    ```bash
    chef generate cookbook test-cookbook #test-cookbook is name of a cookbook
    cd test-cookbook
    ```
    
3. Create a recipe.
    
    ```bash
    chef generate recipe test-recipe #test-recipe is name of a recipe
    cd ..
    ```
    
4. Write a code to install & deploy Apache server.
    
    ```bash
    #open vi editor to write the code in ruby language
    vi test-cookbook/recipe/test-recipe.rb
    ```
    
    ```ruby
    package 'httpd' do
    action :install
    end
    
    file '/var/www/html/index.html' do
    content 'Apache server installed'
    action :create
    end
    
    service 'httpd' do
    action [:enable, :start]
    end
    ```
    
5. Run the code.
    
    ```bash
    chef-client -zr "recipe[test-cookbook::test-recipe]"
    ```
    

That's all about today, I guess you and me both learnt something new today. Hope you find my notes easy to understand.