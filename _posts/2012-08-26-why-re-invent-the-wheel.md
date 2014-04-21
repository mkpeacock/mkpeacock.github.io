---
layout: post
title:  "Why re-invent the wheel?"
date:   2012-08-26 18:49:13
author: Michael
---

As developers, it's sometimes tempting to reinvent the wheel. You need some code, a tool, an API, a CMS or framework that works in a specific way. You find tools out there that do most of what you want, but not quite everything.  And so, we give in to temptation.  Before you know it, you have spent more time reinventing things that you need just so that you can start developing your product - but you haven't yet started on the product!

Sometimes there is a genuine case for reinventing the wheel.  If you rely too heavily on external frameworks, or APIs, you can find that your product can only be extended in ways that the external code allows.  You might not want to be tied to the development cycle of someone else ([Facebook's ever shifting sands](http://techcrunch.com/2012/08/10/branchout-ceo-rick-marini-on-building-a-company-atop-facebooks-shifting-sands-tctv/) anyone?). These particular risks come down to your business needs, and in many cases, are an acceptable risk, especially over building things entirely from scratch.

Even if you find the risk of existing frameworks or platforms too great, or you just want to retain architectural control of your product, there is an alternative: Components.

## Components
Components are stand alone pieces of code which focus on a specific problem or aspect of an application.  Perhaps its dealing with authentication, security, template generation or sending emails.

### Symfony Components

Symfony is a very popular PHP framework.  The team behind it built it in a very component based fashion. So much so, that they have released many key aspects of their framework as standalone components. While many might be skeptical as to how well these components work on their own, outside of the structure of the framework they were built in, a true testament to their independance is that the next version of Drupal (a 'competing' framework and content management system) is being built using many of these components.
The suite of [Symfony Components](http://symfony.com/components) provides tools for:

- Application routing
- Event dispatching
- Form processing and validation
- Security
- HTTP abstraction
- Dependency Injection
- Templating
- And many more

### Friends of Symfony
The makers of Symfony have also released some separate components, these include:

- <a href="http://pimple.sensiolabs.org/">Pimple</a>: A dependency injection container
- <a href="http://twig.sensiolabs.org/">Twig</a>: Template engine
- <a href="http://swiftmailer.org/">Swift mailer</a>: Mailing Library

### Fuel PHP
Symfony are not the only framework to release parts of their code as components, Fuel PHP (and a number of other frameworks) are also following this trend. In particular, I'm a fan of their [Validation Component](https://github.com/fuelphp/validation).
## Using Components
Over the past couple of years there has been a growing trend in the industry to move towards components. More people are building them, and more people are using them.  Thanks to the likes of Composer, a package and external dependency manager, and the PRS-0 standard components are a great option for your PHP project.
### PSR-0
The [PSR-0 Autoloading standard](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md) for PHP provides specific guidelines for how code should be namespaced and autoloaded.  The primary benefit in terms of components, is that you can use lots of external components without fear of conflicts in terms of class names, both between components and with your own products code.
### Composer
[Composer](http://getcomposer.org/) is a great tool for managing these dependencies. Updating external dependencies, adding new ones to a project, storing them in harmony with your codebase and removing them before distribution (depending on their licence) is a serious headache for any developer.  Composer lets you simply create a configuration file listing your dependencies and if appropriate, the version numbers. Composer then manages these dependencies, updating (or sticking to a specific version), installing and removing them as appropriate.  This tool really lowers the barrier to entry to try out these components, as you can quickly import them to a project, try them and throw them away if they are not suitable, with minimal effort.
## PHP North East Talk
Last week, I [presented a talk](http://www.slideshare.net/michaelpeacock/phpne-august2012symfonycomponentsfriends) to around 26 developers from the North East at the monthly [PHPNE](http://phpne.org.uk/), discussing the Symfony Components, their friends and how they can quickly be dropped into a project.
## Summary
While there is a risk in using external code in your products, making use of the wealth of quality components that solve common web related problems you can retain architectural control over your product, while using solid stand alone components to solve specific tasks.  Symfony components along with Twig, Pimple and some components from the Fuel PHP framework provide some of these solutions.  There are a whole host of other tools available on Packagist too.
At [Ground Six](http://www.groundsix.com) we use components to help build our products and to help refactor legacy code we work on.  It is one of the many tools and techniques that we use to build our products quickly, and get them to market so that we can evaluate the [MVP](http://www.groundsix.com/).

[Symfony Components & Friends: PHPNE Talk](http://www.slideshare.net/slideshow/embed_code/14023481).
