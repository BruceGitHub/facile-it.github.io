---
authors: ["jean"]
comments: true
date: "2016-05-16"
draft: true
share: true
categories: [English, TDD, PHP]
title: "How PHP 7 & TDD helped me sleep better"

languageCode: "en-US"
type: "post"
---
# The enemies of programming
As many of you will agree with me, **sleep deprivation** is the enemy of programming.   
Maybe we fear only one thing more than that: **being interrupted**.
<p style="text-align: center;">
    [![Why you shouldn't interrupt a programmer (by Jason Heeris)](/images/how-php-7-tdd-helped-me-sleep-better/interruption.png)](http://heeris.id.au/2013/this-is-why-you-shouldnt-interrupt-a-programmer/)
</p>


While writing code we have to think really hard, we use complex abstractions, we go through long business workflows and so on... fatigue and interruptions are the main enemies of those in this line of work.

# My experience 
On my daily job, I do all this mental juggling on a pretty big project, which is based on PHP 5.5, Symfony 2.8, Doctrine and so on; in this project we luckily use a good deal of **good practices**, and **automated sofware testing** is one of those. I actually switched to this job to learn about doing automatic testing, continuous integration and other best practices.

Nearly half a year ago I became a dad. It has been great, and you also get some unexpected perks! For example, my colleagues got me this gift for my son:
<p style="text-align: center;">
    ![A blue elePHPant!](/images/how-php-7-tdd-helped-me-sleep-better/blue-elephpant.jpg)
</p>

So, we can say that his future is pretty clear... But don't say this to my wife! 

During the pregnancy, many of my friends and fellow parents warned me, half jokingly, about one thing: *"sleep now, you'll be deadly tired after!"*. Now I can say that they were a bit exagerating, but I can't deny that, having a child has a toll on your sleep schedule... Even if, as in my case, having a 9 to 6 office job, my wonderful wife does all the parenting heavy lifting (and I consider myself pretty lucky for having her!). 

A few months after my son was born I also had the opportunity to start **a new, fresh project**. To be completly honest, it was not actually fresh, but it was a **complete rewrite** of an internal service that is used to manage invoices for multiple business units inside our company. I knew pretty well the old system that had to be replaced, so I was put in charge of redoing it from scratch.

One of the issue with the old system (and the main reason behind the rewrite) was **maintainability**: we had no tests, we hadn't a proper dev environment, and the design was not great; also, bureaucracy and invoicing are the core domain of the system, so it was inherently complicated. This created the perfect environment to see the [Broken windows theory](https://en.wikipedia.org/wiki/Broken_windows_theory) in action, and the code base got worse over time, one patch, copy paste or fast fix at a time. 

Obviously, as any tech passionate would do, I took the opportunity to use a lot of new shiny tools! I picked **PHP 7**, which was just released, and started the project with something familiar to me but still pretty new and cool, **Symfony 3.0**.

# What I found useful
I rambled and thought about this project a lot in the past months with my colleagues, because the old system was costing us a lot of overhead in usage and maintenance, and we had a pretty clear idea of what its problems were, so I hadn't to study a lot before start writing the first classes.

So, I had to spend *some* time thinking about an object oriented design for my project, but I was rapidly able to dive right into writing code, and be really confident about it. In the end, a lot of this confidence came from a few choices that I pursued during the development of this project.

## TDD and high coverage
The first choice that I am pretty satisfied about is **automatic testing**: I already knew the advantages of doing tests and Test Driven Development, but in the previous project that practice was not introduced from the start, so not all the codebase was covered, and we couldn't (or wouldn't?) do TDD 100% of the times.

Instead, in this case **I wanted to write nearly everything with TDD**, and have a **very high threeshold for the minimum coverage** that I would mantain. Right now I'm proud to have a ~92% test coverage. This wasn't a mere "let's hit 100%!" mindless objective ([since it's pointless]({{< ref "software-testing-coverage-vs-efficacia.md" >}})), but instead it fueled **a positive feedback cycle**: the more I wrote new classes using TDD, the more the coverage rose and stayed high; at the same time, I found myself inspecting the coverage report to find missing spots, and that lead multiple times to highlighting some edge-case that I didn't test, and I really needed to.

Of course, I still leaved some part uncovered or without specific tests, since it's pointless to test them (e.g. Doctrine entities), and I covered some parts multiple times, since they were **critical paths** inside my application.

## Unit tests to the rescue!
Last but not least, the main critical advantage that TDD gave me was **focus even on strained days**: I wrote the classes starting from unit tests, without having to bear in mind all the project in its complex design, and giving all my effort to one piece of code at a time.
 
I then wrote some functional test to assure that the **collaboration between my unit-tested objects** was fine, and this later step was also useful to delay the definition of the classes as services inside the Symfony's DI container. I was also **able to change my mind** on some details of the design a few times without suffering mental confusion or having to rewrite too much code.

## PHP 7: scalar and return types declarations
The second good choice was **PHP 7**: two of the main reasons behind it as the language version for this project were the [two main new features](http://php.net/manual/en/migration70.new-features.php) introduced in that version: **scalar types** and **return type declarations**. 
<p style="text-align: center;">
    ![Return types, return types everywhere!](/images/how-php-7-tdd-helped-me-sleep-better/return-types-everywhere-meme.jpg)
</p>

Before Facile.it, I worked as C++ developer, and oh boy! did I really missed scalars and return types! 

*"I came onboard of the PHP community right in time"*, I thought... So I exploited this situation to start using all this new features. I started to enjoy having again the possibility of typehint string and integers, and I discovered how return types declaration enforces really well the cohesion of your objects, making it **rightly painful returning different types of data from the same method**, or mixing a type with null.

Return types also demonstrated to be a **double edged sword** in some cases, especially on Doctrine entities: they are really useful to enforce consistency in your values, since they trigger a `\TypeError` each time you call a getter method on a erroneously empty property, but **you can't use them on nullable fields**, since it will break your application at any time during execution.

On the other end, having return types declared on your business-logic classes it's pretty useful, and even more when used in conjunction with TDD: every time you define a mock you are forced to declare expectations and predictions with the right types, so it **indirectly helps maintaining the collaboration contract between objects**, without too much hassle. If I changed the signature of a method that was mocked somewhere, the mock would break the test, **highlighting the issue and making the test (and the high coverage) even more valuable**.

# Conclusions
At the end of the day, this (and other) **good practices are helpful** for both your job and your life: you can do a lot of things to be in your best shape and fit when you do your work, but stressful and (good) distracting events are unavoidable, you often will have to fight stress, fatigue or distraction and there will be days where you can't be at the top of your game, for any number of reasons.

Since programming is a mental job, I think that having instruments and good practices is indispensable: they are just some of the **essential tools of our craft**. So, I hope that those little life/programming lessons that I learned in these months will be valuable to other people like me.
