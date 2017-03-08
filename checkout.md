# Checkout

I've worked on more than 15 ecommerce websites over the years but one particular story I want to share is from a while back. In 2008, I worked on a website for Boots.com. I remember this story quite vividly because they were fascinated with the idea of having a single-page checkout.

At the time this design pattern was all the rage. The design consisted of one page, with four steps (delivery, payment etc) all represented by an accordion panel. Each step was submitted via AJAX&mdash;on success the panel would close and the next one would open.

It was a disaster and Boots were, let's say, unhappy bunnies.

We redesigned it so that each accordion panel became its own page which removed the need for accordions and AJAX. Fortunately, it converted significantly better and Boots were once again, happy bunnies.

They say history repeats itself and six years later, in 2014, I joined Just Eat. I was once again tasked with helping my fellow UXers redesign the checkout flow, which was functioning in much the same way as the original Boots checkout.

Once again, we redesigned it so that each section became its own page etc. This had a massively positive impact to the business. And this time I made a note of the numbers. The result was an extra two million orders a year. And just to be clear that's orders, not revenue.

Some years later, Robin Whittleton from GDS, told me that putting each thing on a page of its own was a design pattern in its own right[^]. And it had a name and everything. Not that things need to be named to be good, but they're easier to refer to when they do.

The pattern is known as One Thing Per Page and it is by far and away my favourite design pattern. Apart from the resulting numbers their is a lot of design rationale behind the pattern which we'll get to shorlty. Before we do that though, let's take a brief look at the page refresh, which is particulary relevant to all this.

## The page refresh

Browse the Internet. I'll wait here while you do that. Back? Cool. You will have noticed that most websites are cram-packed with stuff you just don't need and that your eyes have learnt to glaze over. Even well-known successful websites suffer from this problem.

It's not just the *amount* of things, but its the things themselves. Quite often it's form over function. More literally speaking, we're prone to dumping several high resolution images on a single page.

Sometimes we even go one step further and put background videos on the screen; to make an impact; to be different; to separate us from the competition. These sorts of things are not the mark of a positive user experience.

More interestingly though is *why* do we do this. As I have an unhealthy obsession for simplicity, I've managed to research and consciously observe people practicing the are of complicating things. Here's some notes on the matter:

1. We, people that is, have a complex about doing more. Mark Jenkins wrote an article about two different designers and how their approach and the resulting contribution sets them apart. In short, we as humans always want to do more and we worry about our own contribution. The more we contribute the better we feel. But this is often counterproductive.

2. Greg McKeown talks about how success often breed failure because with success comes growth. With growth comes more opportunity, more departments, more people trying to contribute (see previous point). This manifests itself in different departments (marketing, digital, merchandising) vying for space on different parts of a website. Inevitably stuff gets added and pages get bloated.

3. We often think that people care about our product as much as we do. We get emotionally invested in the products we design. But most people don't care about our site like we do. They don't come for entertainment. They just want the outcome. So *we* get bored and *we* add more stuff. In many respects we come up with problems to solve.

4. More specifically related to this topic is that we think all users must complete tasks within 3 clicks[^] which is a gigantic UX myth. It's not about clicks (or pages), it's about users making their way through a process as quickly and as easily as possible.

5. We often confuse simplicity for having *one* of something[^]. But that's not necessarily true. Having one of something that is intertwined with many things is far from simple.

6. And lastly, we often assume that our users are the same as us. We think they have the same cutting edge devices and the same expensive data contracts. But actually they don't[^West].

Whatever the reason, we end up having to come up with clever ways to make slow pages faster. It's like eating donuts ever day, putting on five stone in weight, and then coming up with innovative ways to make a fat man run fast. I'm thinking rocket boots, do you have any better ideas?

*Innovation* by the way is a dirty word. Most often, it's seen as a license to spend a lot of time solving problems that are typically introduced by the same person trying to solve them.

Ultimately, somebody announces *the page refresh is the problem*. To get rid of the page refresh we can put everything on a single page and use AJAX. But this is rather obviously not solving the actual problem. AJAX doesn't avert the server-side round trip. It just avoids the page refresh.

Even with AJAX, the page still needs to repaint. It still has to make requests to the server. That's not all. There are penalties in using AJAX. We need to send more code to the user to do this fancy stuff and we need to write more code to handle errors and to show a custom loading indicator.

Loading indicators, by the way, are a problem because they aren't accurate, like the browser's native implementation. And they aren't familiar to the user&mdash;that is, they are always custom to the site implementing them. But familiarity is a UX convention that we should only break if we really really have to.

It's clear that in many ways by trying to improve the experience we've created a poorer one. The problem isn't that the page is slow, the real problem is designing a page that cannot be fast.

## What exactly is One Thing Per Page?

*One thing per page* is not necessarily about having one item on a page&mdash;you'd still have the header, the footer etc. And it's not about having one form field on each page either&mdash;that's over thinking things.

It's about splitting a process into multiple digestable chunks, much like we discussed above with Boots and Just Eat. An address form has multiple fields, but it's one tangible thing that can be focussed on at a time.

## Why is One Thing Per Page so damn good?

Whilst this pattern often bares wonderful and delicious fruit (or orders and conversions if you hate my analogies), it’s a good idea to understand why.

**It reduces cognitive load**. In The Obstacle Is The Way, Ryan Holidy puts this eloquently. *Remember the first time you saw a complicated algebra equation? It was a jumble of symbols and unknowns. But when you stopped to break it down and isolate the parts, all that was left was the answer.*

It’s the same for users trying to complete a form or anything else for that matter. If there is less stuff on a single screen, and only one choice to make, friction is reduced to a minimum. Therefore users stay on task.

**It’s easy to handle errors.** When users are filling in a small form, errors are caught and presented early. If there’s one thing to fix, it’s easy to fix, which reduces the chance of users dropping off.

**It’s faster to load.** If pages are small by design, they will load faster. Faster pages reduce the risk of users leaving and they build trust in the service.

**It’s easy to track progress and return to previous steps**. If users  submit information frequently, we can save their progress in a more granular fashion. For example, if a user exits the payment page, we can send them back there later&mdash;by email perhaps.

As if that wasn't enough I've managed to count a further 10 reasons why the pattern is such a good one[^], and so for these reasons we'll be using the approach for our checkout flow and for other problems in upcoming chapters.

MENTION PROGRESS DISC HERE TO FINISH OFF THIS SECTION.

## Flow and order

Caroline Jarett etc, says that blah this and blah that...

- reference page 16 and 19 from caroline book. Amazing stuff.

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

## Smart defaults

- default delivery option/method
- once information is stored (default del and pay) can jump to conf

## Footnotes

[^west]:https://www.smashingmagazine.com/2017/03/world-wide-web-not-wealthy-western-web-part-1/
