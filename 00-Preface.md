# Introducion

I remember my first experience of forms. Back in 2000, web design was one of the course modules. It mostly consisted of cutting and pasting snippets of HTML, CSS and Javascript and making them my own. My obsession with forms began around the same time, like with any other HTML element, I tried to cut and paste it. When I did, the form rendered fine. But submitting it did nothing. Dazed and confused I eventually realised the things I was missing. Fast forward 17 years and here I am: Writing a book about form design patterns.

## Why forms?

Every meaningful interaction that happens on the web is achieved by a form of some sort. That's because without forms, the web becomes passive, just a way to read stuff. That's great and all, but we can make the web far more interactive. Forms are the vessel by which users can create, update and delete things. Whether it's commenting on a blog post, buying a product and having it delivered to your door, all the way through a fully fledged adminstrative system, forms are always front and center.

## Why patterns?

Design patterns serve as guidance and solutions to those who solve similar problems over and over. The reason for design patterns are twofold. 

First, instead of solving the same problem from scratch every time, we can instead refer to previously used, available, recognised and well-researched solutions. Ultimately this saves a lot of time and makes use of our time to solve new problems.

Second, by solving the same problem in the same way, users get a more consistent and coherent experience. That is, the service becomes familiar. Familiar interfaces requires users to think less. This is because they know how something works based on previous experience. In doing so they have to exert less energy doing the thing.

Think about it, every time you encounter a door, you intuitively know that it can be opened, closed and sometimes locked. You know all this stuff without thinking. It just happens. It's just how it is. Easy. Leveraging patterns for forms or more broadly, the web, is simply a sensible thing to do.

## Why these particular problems?

When I first outlined the book, I did so based on principles. I came up with about 50 or so. Things like, ‘every field should have a label’. The problem with evaluating problems based on principle is that quite often, the same principles come up for very different problems. What makes more sense is to tackle tangible problems the same way as we would do so in the workplace.

So then I set out to try and encompass the 50 principles naturally and by example. I came up with 7 chapters that solve a bunch of different problems. Those problems are almost definitely applicable to your own project. They've certainly been applicable to the slew of projects I've worked on for the past 15 years.

Personally, I want to solve common problems in a robust and fully inclusive way. Seeing as you're here, I'm guessing you do too.

## Principles

Even though, I've purposely moved away from orientating the chapters around principles, that does not mean principles are disguarded all together. Before we design things, we need to be upheld to some principles. What are we without our principles anyway?

When defining principles we can either steal other peoples or come up with our own. But before we get to that, it's worth a look at how we decide what our principles are in the first place.

This is a book about designing forms for consumption on the web. As such we would be remiss to ignore the essence of the web itself. The very power of it is one of reach and accessibility. Anyone with a connection and a browser gets access to the same thing as anyone else.

Whichever principles we want to aspire to, I think aligning with this sentiment is a bloody good start. Whatever it is we build, we want to have considered and designed for the swath of users that have access to the web. A good approach is to design with an inclusive hat on. This means design for everyone. Therefore, I can think of no better principles than those found at on Inclusive Design Principles Dot Org. Interestingly only one of their principles (out of seven) pertain to accessibility. The others just sound like good design. Let's lay them out in black and white now:

- Dah
- Dah

Throughout the book we'll pick off these principles and point out where our design fails and succeeds in these regards. These principles, should, at least indirectly, guide us during the design process. See you on the other side. 

--

Why by example. Reference bits in evaluation by pattern. Being told what to do is one thing. Being trained through example is an entirely other matter. Much better.

Before we can design solutions, we need to be upheld to some principles. What are we without those. So we need to either create some or steal other people's. But how do we come up with design principles?





---

# Introduction

I remember my first experiences with web design. It was 2000 and one of the modules on my AVCE ICT course was ‘web design’. Class mostly consisted of copying other people's code via *view source* and make small changes.

I built up a fair understanding of most elements. The complex ones were framesets and tables because we used those to design interesting layouts. Links were easy, you just pointed the `href` at the page and everything worked, but forms were different. They were special.

*View source* didn't work. I mean it did work, to an extent. But only in that it rendered the form in the browser. Submitting the form did nothing. I became obsessed with trying to get the form to do *something*. I remember trying to copy Amazon's add-to-basket form in the hope that I could build my own ecommerce store. After an embarassing amount of time I learned that I was missing a little thing called a *server*. This opened up a brand new ‘can of worms’.

In those days as a web developer you were responsible for many facets: databases, servers, HTML and design. Nowadays they have a special term called *full stack*. I realised quickly back then that I was far more concerned with the interaction and experience side of things than I was databases. I quickly specialised as a Front-end Developer.

My obsession with forms never subsided and I've always found myself working on highly interactive websites, not just those that present information. From ecommerce websites to administrative applications, forms have always played a major role. If you're reading this book, it's probably the same for you.

## Evaluation by problem

When I first had the idea to write a book about form design, it started as a list of ~50 principles with a chapter for each one. As I tried to write each one, I found it jarring and I never really knew why. Then I read Heydon Pickering's book Inclusive Design Patterns where he mentioned this exact problem.

In short, when we're in the workplace and we're trying to design something, we don't start with the principles and make the design match those. It's the other way around. We orientate ourselves around a problem and apply various applicable principles to those. Specifically, we often apply the same principles to many different problems. 

Trying to discuss principles in the abstract simply isn't practical. Having discovered Heydon's lightbulb moment, I quickly changed course and sketched out several chapters that would encompass the ~50 principles by solving real world problems. The result is this book.

## Principles

Just because the page isn't designed around principles, doesn't mean we're not designing to them. Principles still have a solid place in this book and we'll refer to many throughout. This book aims to produce and define several reusable patterns. Any design system is usually founded on principles and this book is no different.

- Read Alla book
- lay out the principles

---


When designing solutions to problems, including those revolving around forms we need to have principles and morals. What are we without those anyway?

Every design-led company seem to revolve themselves around their design principles. I've always found most of them a bit abstract. A bit having them just for the sake of having them.

But they don't have to be this way. Refer to Alla book to get fish some good principles out and explain how the teams have been centered by them.

Principles, when practical and applicable, can be tremendously powerful and double up as constraints. That's what design is, solving problems within a set of constraints. For example, why do we have doors? If there weren't any constraints you just wouldn't have a building or an entrance. But we need a building for shelter. We need an entrance to get in and out of the shelter. It needs to be a certain size so that we, humans, of all shapes and sizes can make our way in and out, but we don't neccessarily want insects and water coming in if we can help it.

Coming up with principles isn't an easy task. Fortunately other people have come up with so many principles, that we can just cherry pick the best ones and use them as our own. It's about choosing the right ones. Heydon's Inclusive Design Principles are most appropriate for this book. But why?

I think that's what the web is born on isn't it. Isn't that the webs super power(prog enhancement article js). Naturally then, designing inclusively really means design.

Let's lay out these principles first here.

1
2
3

And as the web is no longer a suckling baby the are now since solid and useful techniques we can use to meet the principles.

Principles ultimately map to the needs of users. As designers, we are overly interested in users, what else is there? This all seems rather sensible and it is.

Ah what do we do with all these thing s? Well keep then in mind, and check ourselves against the principles right the book. If by the end of the book you think we have taken short then you can reach out to me and we can get that solved together.

Frank web's grain

## Taking principles a step further

- Progressive Enhancement
- Responsive Designs

## Why patterns?

Most solutions, including those found in this book, are applicable to so many problems and different domains and websites. That's what patterns help. Don't solve the same problem twice. From micro to macro patterns, this book will help you solve common blah instead of doing the same thing over and over.

---





- Forms are boring. I've never thought that.
- equal amounts common sense, my research and other people's.


Don't be fooled by its simple appearance though. There's a lot of ground to cover and patterns that will emerge. Patterns we'll either use directly or as a foundation to build new patterns when needed.

Refer to inclusive principles in this from heydon that i can refer to.

Refer to the principles from GDS to do the same thing.