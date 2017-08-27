# Introduction

I remember my first foray into forms. At the turn of the century, web design was one of the modules on the Information Communication and Technology course I took in sixth form college. It mostly consisted of cutting and pasting snippets of HTML, CSS and Javascript. Yep, I came from the view-source school of web development.

My obsession with forms started when&mdash;like with any other HTML&mdash;I tried to cut and paste it. Despite rendering okay, when I tried to submit it, nothing happened. Fast forward 17 years and here I am writing a book about form design patterns.

## Why forms?

Every meaningful interaction that happens on the web is achieved by a form of some sort. That's because without forms the web merely becomes a passive experience, just a way to consume content. Forms are the vessel by which users can create, update and delete things. Whether it's communicating via email, buying a product, online banking or working on a fully fledged adminstrative digital system, forms are always front and center.

The other things about forms, is that on first glance, they're rather easy to get a grasp of. In less than half an hour, you'll have a few types of form controls on screen. But, their low barrier to entry has turned them into, what Heydon Pickering refers to as a *10,000-volt electromagnet for attracting usability problems*[^].

## Why patterns?

Design patterns serve as guidance and solutions to those who solve similar problems over and over. The reason for design patterns are twofold.

First, instead of solving the same problem from scratch every time, we can instead use previously used, available, recognised and well-researched solutions. Ultimately this saves a lot of time, giving us back the headspace to solve newew and bigger problems.

Second, by solving the same problem in the same way, users get a consistent and more coherent experience. The service, app or whatever it is, becomes familiar. Familiar interfaces require less effort to operate. This is because users know how something works based on previous experience. In doing so they have to exert less energy performing the action. Steve Krugs seminal book ‘Don't Make Me Think’[^] is dedicated to this very subject.

Think about it, every time you encounter a door, you intuitively know that it can be opened, closed and sometimes locked. You know all this stuff without thinking. It just happens. It's just how it is. Easy. Leveraging patterns for forms or more broadly, the web, is simply a sensible thing to do.

## Why these particular problems?

I first outlined this book based on principles&mdash;I had about 50 or so. The original plan was that each would become a short chapter. For example, there was a chapter called ‘Always use a label’ and another called ‘Placeholders are problematic’.

One problem with this approach is that sometimes, although it pains me to say, rules need to be broken, something we'll be dicussing shortly. Another problem is that evaluating problems by principle is constraining. Many principles go hand in hand with each other. Talking about related principles together is important because sometimes you have to make trade-offs.

Instead, I eventually realised that I needed to approach the book in much the same way as anyone would in the workplace. By starting with real world problems and designing real solutions to them. I cahnged course: instead of 50 principles, I came up with 8 chapters. Each chapter represents a real problem that is probably relevant to your project in one way or another. They've certainly been applicable to the slew of projects I've worked on in the past 17 years or so.

Personally, I want to solve common problems in a user-centered way. Seeing as you're here, I'm guessing you do too.

Note that some of the chapters may sound niche, but I assure you that the patterns that are born out of tackling this specific problems are very much transferable to other problem domains. That's what makes the solutions patterns. Patterns should be unique and reusable across projects and even organisations.

### Chapter 1: A registration form

We'll start with a basic registration form and the foundational qualities that make up a good form. By applying the Question Protocol we'll look at how to reduce friction drastically without even touching the interface. We'll get into some visual and interaction design specifics. Finally ending by making form validation easy and inclusive.

### Chapter 2: A checkout flow

We'll look at the One Thing Per Page pattern and how and why it's suited to many form processes such as checkout. Considering flow and order we'll break down the various steps and analyse each one in detail, taking a deeper look at several input types and how they affect the user experience on desktop and mobile browsers.

### Chapter 3: Book a flight

We'll take a deep dive into the world of progressively enhanced, custom form components using ARIA. We'll do this by exploring the best way to let users select destinations, pick dates, add passengers and choose seats. We'll analyse native form controls at length and even break away from convention (for good reason).

### Chapter 4: A login form

This short chapter looks at the ubiquitous login form. For all its simplicity there are some common usability failures that we'll look to address. Social media login has both helped and hindered these matters so we'll be looking at this too.

### Chapter 5: An inbox

We'll look at ways to manage and action email in bulk. This is our first look at an adminstrative interface and as such comes with it's own set of challenges and useful patterns. This includes a reponsive ARIA-driven action menu, multiple selection and how we can design better messaging.

### Chapter 6: A search form

In this short chapter, we'll be looking at two important things: how to design a responsive search form that is readily available to users on all pages and the search mechanism that lies behind it. Together, search can be simple and useful.

### Chapter 7: A filter form

This chapter compliments the previous, by looking at the interesting ways in which users can filter a large set of unweildly search results. Without a well-designed filter, users are bound to give up. A filter form, as you'll see, creates another situation where we may have to break away from ‘best practice’ to give users the best experience.

### Chapter 8: A really long form

Some transactions are really long. Some are really complicated. Some transactions involve having to upload several files. Some even involve all of these things. We'll look at some innovate, yet simple ways of breaking down this complexity into simple, manageable tasks that save time and keep users in control.

## What about principles?

Whilst I've purposely moved away from principle-orientated chapters, that doesn't mean we should disguard principles altogether. Before we design anything, we need to be upheld to principles. What are we without our principles anyway?

When defining principles we can either steal other peoples or come up with our own. But before we get to that, it's worth taking a look at how we might decide what our principles are in the first place.

Principles stem from a belief system. That something should be the way we believe or want it to be. This book as about designing interactive experiences for consumption over the web. As such, it would be remiss of me to ignore the essence of the web itself. The very power of it, is one of reach and accessibility. Anyone with a browser and an Internet connection has access to it. Whatever principles we decide, aligning with this sentiment is a good start. We don't want to design anything that breaks the web for a single person.

Whatever we build, we should be thinking about users. On the web, that's everyone. Inclusive design is about designing for everyone, therefore I can think of no better set of principles than those written by Heydon Pickering and Heni Swan of Paciello Group[^]. Interestingly, only one of the seven principles pertains to accessibility. The other ones are just *good* design. Let's lay them out in black and white.

1. Principles
2. Go
3. Here
4. Etc
5. a
6. b
7. c

We'll refer back to these principles throughout the book, pointing out where a solution fails or succeeds. These principles should at least indirectly hold us to account throughout the design process. See you on the other side.

## Footnotes

## Todo

- flesh out more about design principles: what makes a good principles?
- more about evaluation methodologies?
- Web's grain?