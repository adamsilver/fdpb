# Checkout

I've worked on more than 15 ecommerce websites over the years but one particular story I want to share is from a while back. In 2008, I worked on a website for Boots.com. I remember this story quite vividly because they were fascinated with the idea of having a single-page checkout flow.

At the time this design pattern was all the rage. And so we designed for them what they wanted. The flow consisted of four steps (delivery, payment etc) all represented by an accordion panel. Each step was submitted via AJAX. On success the panel would close and the next one would open.

It was a disaster and Boots were, let's say, unhappy bunnies.

We redesigned it so that each accordion panel became its own page which removed the need for accordions and AJAX. Fortunately, it convertly significantly better and Boots were once again, happy bunnies.

History likes to repeat itself and six years later (in 2014), I was once again tasked with helping my fellow UXers redesign their checkout, which was functioning in much the same way as the original Boots checkout.

Once again, we redesigned it so that each section became its own page etc. This had a massively positive impact to the business. And this time I made a note of the numbers. The result wa an extra two million orders annually. And by the way, that's order, not revenue.

Some years later, Robin Whittleton from GDS, told me that putting each thing on a page of its own was a design pattern in its own right[^]. And it had a name and everything&mdash;not that everything needs a label to be good, but it's easier to refer to when it does.

The pattern is known as One Thing Per Page and it is by far and away my favourite design pattern. Apart from the resulting numbers their is a lot of design rationale behind the pattern which we'll get to shorlty. Before we do that though, let's take a brief look at the page refresh, which is particulary relevant to all this.

## The page refresh

Browse the Internet. I'll wait here while you do that. Back? Cool. You will have noticed that most websites are cram packed with stuff you just don't need and that your eyes have learnt to glaze over. Even well-known successful websites suffer from this problem.

It's not just the *amount* of things that is the problem, but its the things themselves. Quite often it's form over function. And more literally speaking, we're prone to dumping several high resolution images on a single page.

Sometimes we even go one step further and put background videos on the screen; to make an impact; to be different; to separate us from the competition. These sorts of things are not the mark of a positive user experience.

More interestingly, *why* do we do this? I've found there to be quite a few reasons for this:

1. We have a complex about doing more, Mark Jenkins wrote an article about two different designers and how their approach and the resulting contribution sets them apart. In short, we as humans always want to do more. But quite often that is not a smart way to go.

2. Greg McKeown talks about how success often breed failure because with success comes growth. With growth comes more opportunity, more departments, more people trying to contribute (see #1). This manifests itself in different departments (marketing, digital, merchandising) vying for space on different parts of a website. Inevitably stuff gets added.

3. We often think that people care about our product as much as we do. We get way to emotionally invested in the products we design. Most of the time, people use our websites for the outcome. They don't come for entertainment. So we get bored and we add more complexity.

4. More specifically to this topic is that we think all users must achieve every task with 3 clicks[^] which is a gigantic UX myth. It's not about clicks (or pages), it's about users making their way through a process is quickly and as easily as possible.

5. We often confuse simplicity for having *one* of something. But that's not necessarily true. Having one of something that is intertwined with many things is far froms imple.

6. And lastly, we often assume that our users are the same as us, the designers. More specifically, we think they have the same cutting edge devices and data contracts. But, they don't.

Whichever the reason, we end up having to come with clever ways to make slow pages faster. It's like eating donuts ever day, putting on five stone in weight, and then coming upwith innovative ways to make a fat man run fast. I'm thinking rocket boots, do you have any better ideas?

When I began writing this book, I never thought I would write a sentence like that, by it's done now. Let's move on. *Innovation* by the way is a dirty word. Most often, it's seen as a license to spend a lot of time solving problems that are typically introduced by the same person trying to solve them.

Ultimately, somebody announces *the page refresh is the problem*. To get rid of the page refresh we can put everything on a single page and use AJAX. But this is rather obviously not solving the actual problem. AJAX doesn't avert the server-side round trip. It just avoids the page refresh.

Even with AJAX, the page still needs to repaint. It still has to make requests to the server. And that's not all. There are penalties when we use AJAX. We need to send more code to the user to do this fancy stuff and second we need to implement more code to handle errors and show a custom loading indicator.

Loading indicators, by the way are a problem because they aren't accurate, like the browser's native implementation. And they aren't familiar to the user&mdash;that is they are always custom to the site implementing them.

In so many ways we've created a poorer experience. So the problem isn't that the page is slow, the real problem is that you've designed a page that can't be fast.

## What is One Thing Per Page?

One thing per page is not necessarily about having one item on a page. You'd still have the header, the footer etc. And it's not about having one form field on each page either&mdash;that's over thinking things. It's about splitting a process into multiple chunks. just like we did for Boots and Just Eat. But what is the reasoning behind the pattern?

## Why is One Thing Per Page so damn good?

Whilst this pattern often bares wonderful and delicious fruit (or orders and conversions if you hate my analogies), it’s a good idea to understand why.

The most important reason is that it reduces cognitive load.

...

I've actually counted 14 reasons[^] but here are just a few for your convenience:

### 1. It reduces cognitive load

Remember the first time you saw a complicated algebra equation? It was a jumble of symbols and unknowns. But when you stopped to break it down and isolate the parts, all that was left was the answer.

It’s the same for users trying to complete a form or anything else for that matter. If there is less stuff on a single screen, and only one choice to make, friction is reduced to a minimum. Therefore users stay on task.

### 2. It’s easy to handle errors

When users are filling in a small form, errors are caught and presented early. If there’s one thing to fix, it’s easy to fix, which reduces the chance of users dropping off.

### 3. It’s faster to load

If pages are small by design, they will load faster. Faster pages reduce the risk of users leaving and they build trust in the service.

### 4. It’s easier to track behaviour

The more there is on a page the harder it is to determine why a user left the page. Don’t get me wrong, we shouldn’t drive design by the ability to analyse it, but this is a nice by product of this pattern.

### 5. It’s easy to track progress and return to previous steps

If users are submitting information more frequently, we can save their progress in a more granular fashion. For example, if a user exits the payment page (for whatever reason), we can send them back to that specific step by sending them an email.

### 6. It requires less (or no) scrolling

Don’t get me wrong, scrolling is no big deal—users expect web pages to work this way. But if pages are small, users won’t have to. And the call-to-action is more likely to appear above the fold which reinforces the requirements and makes it easier to proceed.

### 7. It’s easier to branch

Sometimes we send users to a different part of the flow based on their previous choice. A simple example would be two drop down menus. What the user chooses in the first, affects the values that are shown in the second.

This pattern makes this easy. The user makes a choice and submits to the server. The server works out what the user should see next. Easy and inclusive by default.

We could use Javascript. But it’s far more complicated to build and ensure the UI is accessible. And when Javascript fails (and it will) the user may suffer a broken experience.

If we use AJAX, the server round trip isn’t eliminated anyway. And now we need to present a loading indicator which isn’t as accurate or familiar as one that is native to the browser.

All of which results in heavier, more complicated pages. This in turn results in an experience, that in all likeliness, doesn’t even have a perceived benefit, let alone a real one.

Alternatively, we could load the page with all the permutations for all branches on the client. But, this could add serious weight to the page. And it could suffer from the same broken experience when Javascript fails.

### 8. It’s easier for screen reader users

If there is less on a page, then screen readers don’t have to wade through lots of superfluous secondary information. Users can navigate to the first heading and enter forms mode quickly.

### 9. It’s easier to amend details

Imagine someone is about confirm their order at the end of the checkout process. At the pertinent moment, they spot they’ve made a mistake with their payment details.

The user will want to amend their details. In this case it is far easier to go back to a dedicated page, than it is to go to a section within a page (using a hash fragment).

A hash fragment has problems. Arriving half way down the page may be disorientating. Remember the user clicked a link, described by the link text, to perform a specific action. Having other things on the page is confusing and distracting.

It also takes more development work in certain circumstances. For example, if you want to show and hide certain panels within the same page, then you need extra logic to do that based on what the user is editing.

Sometimes a user may make a change in the flow that disrupts the “happy path”. That is, they make a change that requires them to answer a different question (see branching above). In this case editing a section that makes a subsequent section redundant makes things complicated.

With a single focus per page all of these problems fade away.

### 11. It gives users control over their data allowance

Users cannot half download a page. It’s all or nothing. Smaller pages save user’s data. If they want more information, they can click a link giving them the ability to choose. Users don’t mind clicking as long as each step takes them closer to their goal.

### 12. Solving performance problems

If everything is one gigantic thing — the extreme of which is an SPA — figuring out performance problems is hard. Is it a runtime issue, a memory leak, an AJAX call etc?

It’s so easy to think that AJAX improves the experience, but adding more code rarely leads to faster experiences.

Having complexity on the client can obscure the root cause of problems on the server. But if pages have one thing, there’s little chance of a performance issue. And if there is, finding the problem should be easier.

### 13. It adds a sense of progression

Because the user is constantly moving to the next step, there is a sense of progression which is a positive feeling to give users when they’re filling out a form.

### 14. It’s easier to design

When we’re working out a complex flow of any kind, breaking it down into atomic screens and components makes it easier to understand the problem and build them back up.

It’s easy to swap screens with each other to change the order. Analysing problem areas is easier when, like users, we’re looking at one thing at time.

All of which reduces the effort to design the feature — which is a nice byproduct of a pattern that benefits users so greatly.






## TODO....


The problem is not that the page is slow. The problem is that you designed a page that cannot be fast. Instead we should solve that problem at its root. Funnily enough this has two wonderful side effects:

1. There is no speed problem anymore
2. As there is less on the page there is far less cognitive burden on the user to know what to do next

Simply put, a single page checkout with a wall of form fields, that takes a while to load is not a smart way of designing responsive, inclusive, performant forms for people. Not one real user has ever stated "oh this checkout is on one page, thank god".

For these reasons and many more we'll use this technique for our checkout flow.

## Flow and order

- reference page 16 and 19 from caroline book.

Let's analyse a typical checkout flow consisting of:

- Delivery options
- Delivery address
- Payment options
- Payment
- Check
- Confirmation

## The order of the forms and fields

We don't ask for payment first. We ask for that last. When we type an address, we ask for it in a sensible order for people to understand etc. Makes things more human and conversational.

## Do we really need to ask for it?

In A Registration Form, we talked about the need to explain to user why they should complete the form, why should they register. Depending on the form and the field we should also be explaining why we are asking for that information specifically.

For example Kidly delivery address, asking for mobile phone, in order to send them notifications. We should tell the user why we are asking for their mobile phone. And if there is no good reason to ask for it, that should drive our design to realise that we shouldn't ask for it, period.

## Guest checkout

This might seem slightly off topic for a book dedicated to the design of forms, but this comes under the *must I ask it for it* rule. 300 million dollar button.

## Valid from date

This isn't really needed. Ask Oyvind. Steven.

## Size of the fields

> Baymard institute usability study found that if a field is too long or too short, users start to wonder if they correctly understood the label. This was especially true for fields with uncommon data or a technical label like CVV (card verification code).

Address the address in this.

http://baymard.com/blog/form-field-usability-matching-user-expectations

The width of a field should provide a clue to the length of content it requires. The length of the content provides a clue to the type of content. Both of which help the user fill in a form field.

For example, a postcode is made up of about 8 characters including an optional space. This field should be smaller than other fields in an address form.

If the width of the postcode field is larger to match the other fields, then there's a cognitive burden on the user. They may have to double check the label for example.

![]Good/bad example of address form.

You can apply these principles to other form fields where the length of the field is known. For example, you wouldn't want to apply this principle to street and town because those values could be any length.

## Visual step indicator

When we have a flow like this a step indicator is essential. Or is it - read about GDS guidelines.

## Address lookup enhancement.

## Revealing a billing address if different from delivery.

Checkbox is okay because its for state as well as value.

Can talk about specifying delivery address first or payment address first. Either way an address is necessary.

Could we put a checkbox USE THIS FOR BILLING ON DELIVERY ADDRESS - probably not its a distraction at the wrong time.

Can always use a smart default, so default it to checked on the payment page.

## Payment card choose automatic js enhancemen

Do I really advocate this? Probably not. Remember it's about fast pages. More code more problems.