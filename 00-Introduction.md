# Introduction

I remember my first experience of forms. Back in 2000, web design was one of the modules for my AVCE ICT course. It mostly consisted of cutting and pasting snippets of HTML, CSS and Javascript. Yep, I came from the view-source school of web development.

My obsession with forms started when&mdash;like with any other HTML&mdash;I tried to cut and paste it. Despite rendering okay, when I tried to submit it, nothing happened. Fast forward 17 years and here I am writing a book about form design patterns.

## Why forms?

Every meaningful interaction that happens on the web is achieved by a form of some sort. That's because without forms the web merely becomes a passive experience, just a way to consume content. Forms are the vessel by which users can create, update and delete things. Whether it's commenting on a blog post, buying a product, banking or working on a fully fledged adminstrative digital system, forms are always front and center.

## Why patterns?

Design patterns serve as guidance and solutions to those who solve similar problems over and over. The reason for design patterns are twofold.

First, instead of solving the same problem from scratch every time, we can instead refer to previously used, available, recognised and well-researched solutions. Ultimately this saves a lot of precious time.

Second, by solving the same problem in the same way, users get a more consistent and coherent experience. The service becomes familiar. Familiar interfaces require less effort to operate. This is because users know how something works based on previous experience. In doing so they have to exert less energy performing the action. Steve Krugs seminal book ‘Don't Make Me Think’ is dedicated to this subject.

Think about it, every time you encounter a door, you intuitively know that it can be opened, closed and sometimes locked. You know all this stuff without thinking. It just happens. It's just how it is. Easy. Leveraging patterns for forms or more broadly, the web, is simply a sensible thing to do.

## Why these particular problems?

I first outlined this book based on principles. I had about 50 or so, and the original plan was that each would become a short chapter. For example, there would be a chapter entitled ‘Every form field needs a label’.

The problem with evaluating problems by principle is that they quite often apply to multiple problems. Discussing one principle often complements or coincides with another. In other respects, principles are too abstract [Explain more].

Instead, it makes more sense to tackle problems the same way anyone would in the workplace. By taking a problem and designing a solution by stepping through various scenarios and constraints. So I changed course, and instead of 50 principles I came up with 7 chapters. Each chapter solves a real life problem that is probably applicable to your project. They've certainly been applicable to the slew of projects I've been involved in over the past 17 years.

Personally, I want to solve common problems in a user-centered way. Seeing as you're here, I'm guessing you do too.

Note that some of the chapters may sound niche, but I assure the patterns that are born out of tackling this seemingly specific problems are very much transferable to other problem domains. [Expand?]

Here are the problems we'll be tackling.

### Chapter 1: A registration form

This chapter forms the foundations for every other chapter in the book. We'll look at labels, placeholders and hints, and the float label pattern. Crucially we'll look at the Question Protocol and how applying it can drastically reduce the friction on any form. Then, we'll consider the content, interaction and visual design of specific form fields and submit buttons. We'll then look at various form validation techniques, including inline validation as well as other related techniques. We'll settle on an approach that makes fixing errors as easy as possible with useful and respectful error messages that work for screen readers too.

### Chapter 2: A checkout flow

This chapter looks at the One Thing Per Page pattern and how it is particularly useful to longer form process like checkout. Next, we'll look at flow and order. Then we'll break down the various steps of checkout (delivery, payment etc) and what the design intracies that together make a huge difference to the user experience. This will be our first look at number inputs and radio buttons. This is followed by several other important concepts such as being able to check your answers, see progress, use smart defaults and enhance the experience for second time users.

### Chapter 3: Book a flight

This chapter is where we'll take a deep dive into the world of custom form components in order to ask users for some tricky information. We'll look at autocomplete components, the crazy world of dates and date pickers, stepper components and seat choosers. The latter of which, we'll be breaking away from convention for good reason.

### Chapter 4: A login form

We'll take a short intermission to look at the ubiquitous login form. Everyone has one, but equally many of them suffer from the same common usability failures. We'll take a quick look at the problems and how to solve them.

### Chapter 5: An inbox

This chapter looks at a more adminstrative interface. Whilst the chapter pertains to an inbox, a bit like Gmail, the patterns that are born out of those problems are applicable to any list that requires management. We'll be looking at multiple-select action menus and a whole lot more.

### Chapter 6: Search and filter

This chapter will look at the best ways to let users find stuff by searching and filtering results. There will be a discussion on affordance, different types of filters (also known as facet navigation) and we'll also look at how AJAX may have a part to play.

### Chapter 7: A really long and complex form

Some transactions are really long. Some transactions are really complicated. Some transactions involve having to upload several files. Some transactions involve all of these things. This chapter will look at some innovate, yet simple ways of breaking down all of this complexity into manageable tasks.

## What about principles?

Whilst I've purposely moved away from principle-orientated chapters, that doesn't mean we should disguard principles altogether. Before we design anything, we need to be upheld to principles. What are we without our principles anyway?

When defining principles we can either steal other peoples or come up with our own. But before we get to that, it's worth taking a look at how we might decide what our principles are in the first place.

Principles stem from a belief system. That something should be the way we believe it to be. This book as about designing interactive experiences for consumption over the web. As such it would be remiss of me to ignore the essence of the web itself. The very power of it, is one of reach and accessibility. Anyone with a browser and an Internet connection has access to it. Whatever principles we decide, aligning with this sentiment is a good start. We don't want to design anything that breaks the web for people.

Whatever we build, we should be thinking about users. On the web, that's everyone. Inclusive design is about designing for everyone, therefore I can think of no better set of principles than those written by Heydon Pickering and Heni Swan of Paciello Group. Interestingly, only one of the seven principles pertains to accessibility. The other ones are just *good* design. Let's lay them out in black and white.

1. Principles
2. Go
3. Here
4. Etc
5. a
6. b
7. c

We'll refer back to these principles throughout the book, pointing out where a solution fails or succeeds. These principles should at least indirectly hold us to account throughout the design process. I'll see you on the other side.

## Todo

- flesh out more about design principles: what makes a good principles?
- more about evaluation methodologies
- Web's grain?