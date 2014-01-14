# The Learning Virtual Machine (VM)

If you want to learn more about Puppet or are a new user of Puppet or just looking to have fun exploring Puppet, then you've come to the right place. This is our free downloadable Learning VM for you to play around with and explore. The Learning VM is a companion to our Puppet Quests a few sections below, which are a series of tutorials about the Puppet language and using its various tools. Although, please keep in mind that this Learning VM focuses on using Puppet Enterprise at its core, but most of the quests can also apply to Puppet open source.


## Learning Puppet

So what is Puppet? Great question! Puppet is a versatile tool for managing your servers. You simply describe your machine configurations in an easy-to-read declarative language (such as Ruby), and Puppet will ensure that your systems conform to and stay in that desired state.

Associated with this Learning VM are a series of quest adventures that will teach you about about managing your systems with Puppet. These quests start out very basic to accommodate all users no matter your experience level. Essentially, we provide everyone a level starting point to build from. From there, its up to you how much you want to learn and what exactly you want to learn. We hope that this will be the beginning of an interesting journey towards automating your systems management efforts.

## The Quest Structure

The individual quest design is very simple in its structure and consists of three parts.

1. An introduction to the quest as it relates to Puppet
2. A themed storyline of the quest and how Puppet fits in.
3. Going on your quest adventure! (Applying what your knowledge in the Learning VM!)

In the below example quest (you don't need to do anything), in order to understand how Puppet is useful and can be leveraged to simplify configuration management, we will automate a task with Puppet. This is an example quest:

__*Quest*__: 

>Let's assume that you work as an engineer in a web-development firm. You are tasked with configuring Virtual Machines for use by web developers. You are given a list of requirements, such as:  

>* the usernames of the web developers 
* a list of one of more user groups
* a list of files and directories that should be present on the machine  
* a list of software packages that should be installed on the machine 
* a website served by the Apache2 webserver
* a database that should be present on the machine

Each of the above steps are tasks within this quest that you will need to complete.

In the above example you _could_ manually configure the Learning VM by completing a list of tasks each time you need a VM. However, I have a feeling you'll soon realize that this manual process boils down to repeating the same set of tasks over and over again. In addition, you're not even using Puppet, so there really is no point for you to continue.

In relation to this example, we will teach you how to use Puppet to _describe_ your requirements to configure the VM appropriately and automate the process of configuring all the other VM's correctly and quickly.

### Ready to start your [Quest #1](docs.puppetlabs.com/learning) adventure? 


	    
========
<div class="page-break"></div>

### Resources

Imagine a system’s configuration as a collection of many independent atomic units - call them “resources.”

These pieces vary in size, complexity, and lifespan. Any of the following (and more) can be modeled as a single resource:

* A user account
* A specific file
* A directory of files
* A software package
* A running service
* A scheduled cron job

On a given system, you might care about several of each the above types. For example, you may want to ensure that several different users are present on the system. Therefore, it follows that for each type of resource, there are several individual resources, each with a unique __title__.

Also, the various resources may have characteristics that you care to manage - for example, you may want to manage the shell used by users, or their home directories. Each of these details that you care about for the resource is called an __attribute__, and each attribute will have a __value__.

In summary, you can describe resource of various types, and values for the attributes that pertain to the resource. For example, on a certain Unix/Linux machine, you may want to ensure that a user with the username "elmo" is present on the system, with the home directory "/home/elmo" and that his shell is the Bourne Again Shell (bash).

In order to convey your intentions in a manner understandable by Puppet, you use Puppet's Domain-Specific Language (DSL) to describe your requirements. Here is the first example of using Puppet's DSL to describe our user:

        user { 'elmo':
          ensure => 'present',
          shell  => '/bin/bash',
          home   => '/home/elmo',
        }
		
The above is an example of a __resource declaration__, since you are describing how you want the resource to be configured. As you noticed, there is always:

* a __type__, which is "user" in the example above,
* a __title__, which is "elmo" here,
* and one or more __attributes__ with specified __values__ - for example, the attribute "shell" has the value '/bin/bash'.

Resource declarations such as the above are usually placed in a file with the extension ".pp". A file with the extension .pp, that contains puppet code is called a puppet __manifest__.

#### Manifests

Manifests are files containing code written in Puppet's DSL, with the filename extension ".pp". These describe how you want various resources to be configured for a system.

#### Inspecting resources 

Puppet provides you with a tool to inspect resources on your systems.

You are already logged in to the Learning Puppet VM, as user root.

Run the following command:

    # puppet resource service
      ensure => 'stopped',
      enable => 'false',
    }
      ensure => 'stopped', 
      enable => 'true',




    user { 'root':
      ensure           => 'present',
      comment          => 'root',
      gid              => '0',
      home             => '/root',
      password         => '$1$jrm5tnjw$h8JJ9mCZLmJvIxvDLjw1M/',
      password_max_age => '99999',
      password_min_age => '0',
      shell            => '/bin/bash',
      uid              => '0',
    }
    
>In the above, what is the _type_ of the resource?  
What is the _title_ of the resource?
What is the _value_ of the _attribute_ 'home'?

#### Resource Types

Puppet has many built-in resource types. Each type can behave a bit differently, and has a different set of attributes available.  

There are several ways to get information about the resource types available for use in Puppet:











#####  Manage a user, 'gonzo' using puppet.

To complete this task:  

>1. In the Learning Puppet VM, in your home directory (`/root`), create a new file named `user.pp`  
>
2. Edit the file `user.pp` and describe user gonzo using Puppet's DSL.  
    * You may want to refer to the example above that describes user 'elmo'.  
    * Remember that you can use `puppet describe user` and `puppet resource user` for help.  

>3. Once completed, run the following command:  

>		puppet apply /root/user.pp


You will see that puppet does, indeed create a user called gonzo!

#### Puppet Apply

Try the command `puppet help apply` to learn more about the `puppet apply` command. We learn that puppet apply is the standalone puppet execution tool. You can use it to apply individual manifests.
    
This is very handy for learning to write code in Puppet's DSL. However, if you wanted to, you could describe the configuration of an entire system as a list of all the resources you want to manage on the system in a single puppet manifest file. But as you can imagine, that might end up being a _really_ long file! We will see how to make this more of a manageable and coherent process when we learn about Classes in a subsequent lesson.
        
#### Abstracting away complexity

Although the example we described above manages a user on a Unix/Linux system, other operating systems have users too. The _attributes_ for the users might change, along with the values, depending on the operating system - but fundamentally, regardless of the operating system, we care about managing a _type_ of resource called "user".

Puppet abstracts away the complexity of managing resources of various types, such as the user resource, by leveraging different __providers__ to realize the resource on the various operating systems. The implementation of how users are realized on various OS-es might differ - the tools that provide these implementations on various OS-es are the _providers_ for the user resource type.

Puppet has a _Resource Abstraction Layer_ (RAL) that consists of types (high-level models of differnt kinds of resources) and providers (platform-specific implementations for the different types), and Puppet automatically translates your description of how you want the various resources you manage should be configured to the appropriate platform-specific commands required to realize your description.

========
<div class="page-break"></div>

## Classes and Modules

We have seen how there are _resources_ and how we seek to configure systems by means of specifying the values for the attributes of the resources we want to manage. The task of configuring a machine will involve defining the attributes of several different resources, possibly of different types. Classes provide for a layer of abstraction, wherein you can group together resources.

A __Class__ in Puppet is a collection of resources, which, once defined, can be declared as a single unit.  

### Defining Classes

Let's assume that we need to manage users on a system - this time with some additional requirements.  

* Users on the system should have their home directories in the directory `/mnt/home`
* We need to ensure that a group, with the name of `staff` is present
* We also need two users, `elmo` and `gonzo`, who are members of the group `staff`
* elmo's home directory should be `/mnt/home/elmo`. 


We can create a definition for a class called users that does all of the above, as follows:

    class users {
    
		user { 'elmo':
		  ensure => present,
		  gid    => 'staff',
		  home   => '/mnt/home/elmo',	
        }
    
        user { 'gonzo':
		  ensure => present,
		  gid    => 'staff',
		  home   => '/mnt/home/elmo',	
        }
        
        group { 'staff':
          ensure => present,
          gid    => '1001',
        }
    
        file { '/mnt/home/':
          ensure => directory,
          mode   => '0755',
        }
    
    }  

In the above, we __*define*__ a class called `users`, which is a collection of three different resources - a user resource, a group resource, and a file resource. The above description is both elegant, and self-documenting.

Now that we have a class called users, we can include the above class in the configuration of a machine to manage users on the machine.

### Declaring classes

As we learnt earlier, a class is a collection or group of resources. In the previous section, we saw an example of a class definition. The question that needs answering now, is, how can we use the class definition? How can we tell Puppet to _use_ the defintion as part of configuring a system?

You can direct Puppet to apply a class definition on a system by using the __*include*__ keyword. By creating a puppet manifest - we already know that Puppet manifests are files with the extension ".pp" that have code in Puppet DSL - that has the include directive in it, we can use the class.

    include users
    
A manifest with just the single line above will apply the definition of class users to the system.

But when you say, `include users` how does Puppet know where to find the class defintion? We will answer that question in the next section.
























>        class users {
>    
>    		user { 'elmo':
>    		  ensure => present,
>    		  gid    => 'staff',
>    		  home   => '/mnt/home/elmo',	
>          }
>    
>           user { 'gonzo':
>		      ensure => present,
>    		  gid    => 'staff',
>  		      home   => '/mnt/home/elmo',	
>          }
>        
>           group { 'staff':
>             ensure => present,
>             gid    => '1001',
>           }
>    
>           file { '/mnt/home/':
>             ensure => directory,
>             mode   => '0755',
>           }
>      
>        }  











> You should see something similar to the following as the output:

>        Notice: /Stage[main]/Users/File[/mnt/home/]/ensure: created
>        Notice: /Group[staff]/ensure: created
>        Notice: /User[elmo]/ensure: created
>        Notice: /User[gonzo]/ensure: created
>        Notice: Finished catalog run in 0.40 seconds




<div class="page-break"></div>

## The Puppet Forge

The [Puppet Forge](http://forge.puppetlabs.com) is a public repository of modules written by members of the puppet community for Puppet Open Source and Puppet Enterprise IT automation software. Modules available on the forge simplify the process of managing your systems. These modules will provide you with classes and new resource types to manage the various aspects of your infrastructure. So your task is reduced from that of describing the classes using Puppet's DSL to one of _using_ an existing description with the right options.
 
### The puppet module tool

Puppet also provide a module tool to help you download, install, list and manage modules from the forge.

For example:

> Run the command:
> puppet module list

This should list the modules currently installed on the system in the following format:

    /etc/puppetlabs/puppet/modules (no modules installed)
    /opt/puppet/share/puppet/modules
    ├── cprice404-inifile (v0.10.3)
    ├── puppetlabs-apt (v1.1.0)
    ├── puppetlabs-auth_conf (v0.1.6)
    ├── puppetlabs-firewall (v0.3.0)
    ├── puppetlabs-java_ks (v1.1.0)
    ├── puppetlabs-pe_accounts (v2.0.1)
    ├── puppetlabs-pe_common (v0.1.0)
    ├── puppetlabs-pe_mcollective (v0.1.13)
    ├── puppetlabs-pe_postgresql (v0.0.4)
    ├── puppetlabs-pe_puppetdb (v0.0.9)
    ├── puppetlabs-postgresql (v2.3.0)
    ├── puppetlabs-puppet_enterprise (v3.0.1)
    ├── puppetlabs-puppetdb (v1.5.1)
    ├── puppetlabs-request_manager (v0.0.9)
    ├── puppetlabs-stdlib (v3.2.0)
    └── ripienaar-concat (v0.2.0)

You can also search for, and install modules


With regards to things that we need to manage on our current VM, we can accelerate the process of defining the configuration for our machines using modules from the forge.

For example, to configure the test machine for our web-developers, we will need to install, configure, and manage the Apache2 webserver to serve web pages. So let us search for a module to manage the Apache2 webserver.

> Run the command:
> puppet module search apache

This should return something similar to the following:

    Notice: Searching https://forge.puppetlabs.com ...
    NAME                   DESCRIPTION         AUTHOR          KEYWORDS 
    5UbZ3r0-httpd          This module han...  @5UbZ3r0        apache   
    7terminals-ant         The Apache Ant ...  @7terminals     apache   
    7terminals-maven       Puppet module t...  @7terminals     apache  
    puppetlabs-apache      Puppet module f...  @puppetlabs     apache
    vStone-apache          Manage apache a...  @vStone         apache,  
    zeleznypa-xhgui        The XHGui modul...  @zeleznypa      apache 

You can also search at the [Forge website](http://forge.puppetlabs.com). If you do so, you will a list on the results page:
[Results for a search for the apache module.](https://forge.puppetlabs.com/modules?q=apache)
 
Now, we can install the module we need. Let's install puppetlabs-apache.

> Run the command:
> puppet module install puppetlabs-apache.

This should return the following:

    Notice: Preparing to install into /etc/puppetlabs/puppet/modules ...
    Notice: Downloading from https://forge.puppetlabs.com ...
    Notice: Installing -- do not interrupt ...
    /etc/puppetlabs/puppet/modules
    └─┬ puppetlabs-apache (v0.9.0)
      └── puppetlabs-concat (v1.0.0)
      
### Using Modules from the Forge

Now that the apache module is installed in our `modulepath`, let's look at how we might use it!

One way to get started using the module is to inspect the code written in Puppet DSL that is in the module's manifests directory at:
`/etc/puppetlabs/puppet/modules/apache/manifests`

However, there is an easier way to do this for well-written modules that include documentation. Let's begin by visiting the [page for the apache module on the puppet forge](https://forge.puppetlabs.com/puppetlabs/apache).

The documentation on the page provides us insight into how to use the class definitions provided in the module to accomplish several tasks. For example, if we wanted to install apache with the default options, the module documentation suggests we can do it as follows:

    class { 'apache':  }
    
It's as simple as that! So if we wanted our machine to have apache installed on it, all we need to do is ensure that the above _class declaration_ is in some manifest that applies to our node. We can also use the Puppet Enterprise Console to apply the class to our node.

What if we wanted to configure the default website served by Apache? The documentation tells us that we can use the following code to achieve that:

    apache::vhost { 'first.example.com':
          port    => '80',
          docroot => '/var/www/first',
    }

This leverage a _Defined Resource Type_ called `apache::vhost` that helps us create virtual hosts in Apache.

As you can see in the above, you can specify the port Apache listens on by changing the value for the parameter `port` in the above sample, to be the port you need Apache to listen on. 



### Problem Description


> When configured, the system will provide web developers with a base system with the pre-requisite packages installed, with the user accounts and directories created.

> The system needs to have:

>* A user called `webtest` whose primary group (gid) is webdev
* Another user called `apache` whose primary group (gid) is apache
* A group called `webdev`
* The following directories:
	* /var/www - owned by user apache and group webdev
	* /var/www/html - owned by user apache and group webdev
	* /home/webtest - owned by user webtest
* The following packages should be installed:
	* httpd
	* mysql
	* php
	* php-cli
	* php-mysql
* MySQL server should be installed with root password 'foo'
* The mysql and httpd services should be running
* A mysql database called `webdb` should exist

__*Note*__: You can see the number of tasks you have completed as you progress through the quest in the bottom left of the Learning Puppet VM terminals. The quest is complete when all the tasks for the quest are complete.




### Define, Iterate!

Since we want to automate the process of configuring a base system for the web developers, we will want to create a module, with a single class in it.  Let us call this module `testmachine`. This module will have a class called `testmachine` defined in it. We will also need a test manifest to enable us to test the class as we work on our definition of the class. Once we create the test file, we can use the `puppet apply` command to test our definition.

We want to develop the class definition through an iterative process. Starting with a simple definition that defines one, or a few resources, we want to test things as we go. So initially, you may choose to ensure that the  user and the group are created. Then you apply the test to make sure that your definition works as expected. Next, you may try to ensure that the required directories exist - so you add the resources to the class definition, and apply the test.

As you work through the quest, you can refer to puppet's documentation for the various types of resources using:  
          
      puppet describe <type> 

and if you need help with the syntax, refer to previous code samples, or get help from the puppet resource command, for example:

	  puppet resource package 
	  

### Steps towards success

There will be an indicator to the lower right of the terminal window that shows your progress as you complete the tasks needed to configure the machine.

If you want to check on progress, with more details, run the command:

    progress
    
This will list the Incomplete tasks that you have yet to complete.

The steps involved in completing the quest are as follows:

1. Create a module, called `testmachine`, in the correct directory, with the correct directory structure.
	* for this, remember what we learnt about the module directory structure
	* also remember that you will need a directory for your class definition, and another for your test manifest.
2. Define the class called `testmachine`
    * also recall the syntax for class definitions - refer to the class definition in the previous section if needed
    * you may want to start with an empty class definition like the following:
    >        class testmachine {
    >
    >        }
    * and add resource declarations to the definition, one (or a few) at a time
3. Create a test for the class testmachine
	* The test file should help you test the class definition as you work on it.
4. Now, in an iterative manner, build the definition for the class testmachine, until you have completely described all the resources you need to manage on the machine.
5. When it's time to configure apache and mysql, install and use the modules provided by puppetlabs at the Forge
    * After the modules puppetlabs-apache and puppetlabs-mysql are installed, read the documentation for the modules and accomplish the following:
        1. Make sure that the mysql server is installed, with the root user's password being set to 'foo'.
        2. Ensure the httpd service is running, by installing the apache server using the puppetlabs-apache module. 
    * Remember to read the documentation for the module if something isn't clear!
    

The `puppet help` &nbsp; command, and by the other puppet commands we have discussed before may help you as you proceed with the quest. Don't be afraid to experiment! 

========
<div class="page-break"></div>

## The Road Ahead

We hope that completing the quest was a lot of fun. Now that we have a class that can be used to configure a machine as a base system for web developers to test their work on, we can configure a new VM almost instantly, just by applying the class. Your work in defining the class once will pay off many times over in the time you save configuring the new VMs.

Although we learned only about using Puppet to configure a single VM - the one Puppet is installed on, it is possible to create a puppet master server that can be used to configure a lot of machines, each with the puppet agent daemon installed on them. This master-agent setup is the key to leveraging the ease with which you can manage entire networks of machines from a single, central server. The concepts are the same as we covered in this lesson - think of how, instead of just managing your Learning Puppet VM, you would be able to manage any machine with puppet installed on it, that can connect to your machine!

In subsequent lessons, we will learn more about how the Puppet Agent and the Puppet Master interact with each other, how you can dynamically generate content for files etc. For each lesson, we will have a Motivating Example, and a quest to complete!
    