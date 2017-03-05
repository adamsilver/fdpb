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

**It reduces cognitive load**. Remember the first time you saw a complicated algebra equation? It was a jumble of symbols and unknowns. But when you stopped to break it down and isolate the parts, all that was left was the answer.

It’s the same for users trying to complete a form or anything else for that matter. If there is less stuff on a single screen, and only one choice to make, friction is reduced to a minimum. Therefore users stay on task.

**It’s easy to handle errors.** When users are filling in a small form, errors are caught and presented early. If there’s one thing to fix, it’s easy to fix, which reduces the chance of users dropping off.

**It’s faster to load.** If pages are small by design, they will load faster. Faster pages reduce the risk of users leaving and they build trust in the service.

**It’s easy to track progress and return to previous steps**. If users are submitting information more frequently, we can save their progress in a more granular fashion. For example, if a user exits the payment page (for whatever reason), we can send them back to that specific step by sending them an email.

As if that wasn't enough I've managed to count a further 10 reasons why the pattern is such a good one[^], and so for these reasons we'll most certainly be using the approach for our checkout flow and for other problems in upcoming chapters.

## Flow and order

Caroline Jarett etc, says that blah this and blah that...

- reference page 16 and 19 from caroline book.

Let's analyse a typical checkout flow consisting of:

- Delivery options
- Delivery address
- Payment options
- Payment
- Check
- Confirmation

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