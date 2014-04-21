---
layout: post
title:  "Perfecting digital ownership and facilitating team development"
date:   2012-08-06 13:17:50
author: Michael
---
Over at [Ground Six](http://www.groundsix.com/) last week we put [Own My IP](https://www.ownmyip.com) online. This was our first product as a development team. Alongside this we have worked on some other projects, including ShareScribe, our first team Hack-Day and the redevelopment of a high-traffic news service - more on those another day.

## Perfecting Digital Ownership
Own My IP aims to solve the problem of digial ownership with regards to copyright and Intellectual Property.  It provides a system for creators of I.P. to transfer ownership to their customers, and for buyers of to formally request ownership of their I.P. and facilitate the transfer.
Companies can use the digital locker service to keep track of their Intellectual Property, and suppliers can use the service to formally transfer the IP related to their services, creating transparency and building trust.  The service offers pay-per-use pricing in the form of credits for requests and transfers, and subscription services for high use customers, such as design agencies.  A [free account is available](https://www.ownmyip.com/pricing/) with a single credit to try the system out.
The sign-up process is powered by a version of our second project as a development team: ShareScribe.  ShareScribe is a tool which offers discounted subscriptions for users who sign-up using their social network credentials and are happy to share their use of the service at predetermined intervals.  For Own My IP this involves a discreet message either indicating that the user has secured their I.P. or that they have added value to a customer by transferring I.P. to them.  We are using the launch of Own My IP as an opportunity to [eat our own dog food](http://en.wikipedia.org/wiki/Eating_your_own_dog_food), and test and enhance sharescribe, before releasing it as a plugin for a number of CMS platforms soon.

## Facilitating Team Development
Although every project introduces its own host of technical challenges, one of the particularly interesting challenges with this project was that we were a brand new team.  The development team (consisting of [myself](http://www.twitter.com/michaelpeacock), [Lewis](http://www.twitter.com/2bard), [Harry](http://www.twitter.com/harry4_) and [Adam](http://www.twitter.com/gardnio)) and apprentice developers (Sean and Iain) all started at Ground Six at around the same time.  We had to ensure we could all effectively collaborate and develop right from the word go.
### Git &amp; Codebase
We use Git, the distributed version control system, to effectively manage the codebase we work on.  An effective branching strategy of branch-per-feature, being merged into the main branch upon completion has served us well.  [Codebase](http://www.codebasehq.com) is the online project management and repository hosting tool (which I also introduced at Smith Electric) we use to log and allocate tasks, host our remote Git repository and keep track of project progress.
For the first 80% of the project we used a series of nested-submodules to pull together some of the seperate aspects of the project.
### Vagrant
[Vagrant](http://www.vagrantup.com) provides each member of the team with their own virtual development environment, running the same software as our production server.  Combined with Puppet, this allows us to easily track changes we need to make to our server stack, and makes it really easy to get new team members up and running, work from other machines or locations and have completely seperate development environments for different projects.
### LocalTunnel &amp; PageKite
Some aspects of our projects, in particular some of the social and payment integration aspects, required the outside world being able to communicate with our development environment.  This is something which would normally be trivial with static IP addresses or Dynamic DNS services, however with our shared Internet connection provided by people who own our building this wasn't possible.  Enter tools such as [LocalTunnel](http://progrium.com/localtunnel/) and [PageKite](http://pagekite.net/). These tools provide a tunnel over the Internet to your local machine, and through their own servers provide a web address which tunnels to your local web server.  We started off using [LocalTunnel](http://progrium.com/localtunnel/), but found the service to be a little un-reliable, so switched to [PageKite](http://pagekite.net/), which provides static hostnames.  I'm currently looking into setting up our own front-end for [PageKite](http://pagekite.net/) to make things a little easier for us.
### Automated build and Continuous Integration
We used tools and scripts to pull together the code, and create working builds for staging and production environments, making deployments to the live server a process of touching a button.  One of the VMs on our internal [ESXi](http://www.vmware.com/products/vsphere-hypervisor/overview.html) server is setup as a Jenkins Continuous Integration instance.  We setup many aspects of this for the project, although we are looking to use it extensively as part of our development process for our next project - now that we are all more familiar with the tools and principles needed.
## Some of the toys we played with
While these technologies are not anything new or fancy, it was nice to use a range of tools and technologies for our first project.
### GoCardless
A relatively new UK based payment processor, [GoCardless](https://gocardless.com) uses the Direct Debit system.  We use this for processing subscriptions (monthly or annually) and one-off credit purchases.  They provide an excellent sandbox for the development process (this really puts the likes of PayPal to shame), and respond really quickly to requests for help - and they are eaqually quick at responding over the phone or via Twitter.
### PostmarkApp
[PostmarkApp](http://postmarkapp.com/) is a fantastic service for delivering emails.
### Twitter, LinkedIn and Facebook APIs
Through ShareScribe we needed to be able to authenticate users through either Twitter, Facebook or LinkedIn, check the number of followers/friends/connections they have and send a post using a range of oAuth based APIs.
### Twitter Bootstrap
During the initial development process, we made use of Twitter Bootstrap to provide a clean and usable design for the application.  Twitter Bootstrap is a very powerful, flexible, responsive and feature rich front-end framework for creating application designs quickly.  About halfway into the project, Paul stepped in to provide a stunning, functional mobile-first design for the application.  Although Paul is a great designer, that isn't his focus at Ground Six, and there is still time to apply for our [web designer vacancy](http://bdaily.co.uk/jobs/creative-design-media/26-07-2012/designer-developer/), if you are interesting in joining a team that gets to work on interesting projects and play with interesting tech.
## Some other tech we have been using
Just to briefly touch upon some of the interesting tools and technologies we have been using on some of our other projects:

- [Composer](http://getcomposer.org/): a great dependency management tool for PHP projects. I'm currently looking at the best way for us to store a range of components privately for use with Composer.
- [Symfony Components](http://symfony.com/components): a range of standalone components which solve specific web related problems
- [Twig](http://twig.sensiolabs.org/): Lightweight but powerful template engine
- [Silex](http://silex.sensiolabs.org/): A simple easy to use micro-framework. Something we used for our hack-day project
- [Pimple](http://pimple.sensiolabs.org/): Dependency Injection framework


## Too long, didn't read?
We launched [Own My IP](https://www.ownmyip.com) our first project as a new development team. Its designed to make the process of transferring and requesting I.P. rights simple and painless.  We use some interesting tools to work well as a team from the word go, and the project involved some interesting technologies.
