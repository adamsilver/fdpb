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

I've managed to count a further 10 reasons why the pattern is so beneficial[^]. This inconspicuous and humble UX pattern is flexible, performant and inclusive by design. It truly embraces the web, making things easy for high and low confidence users alike.

Having lots (or everything) on one page, may give an illusion of simplicity, but just like algebraic equations they are difficult to deal with unless they are broken down.

If we consider a task as a transaction that a user wants to complete, breaking it down into multiple steps makes sense. It’s as if we’re using the very building blocks of the web as a form of progressive disclosure. And the metaphor behind pages provides a subconscious sense of moving forward.

I’m not sure there is another pattern that has as many benefits as this one. It’s like compound interest, it just keeps on giving. This is one of those cases where simple is just that.

Needless to say we'll be using this to design our checkout form in this chapter.

## Flow and order

Caroline Jarett etc, says that blah this and blah that...

- reference page 16 and 19 from caroline book. Amazing stuff.

Let's analyse a typical checkout flow consisting of:

- Delivery options
- Delivery address
- Payment
- Check details
- Confirmation page (refer to gds confirmation pages perhaps)

We don't ask for payment first. We ask for that last. When we type an address, we ask for it in a sensible order for people to understand etc. Makes things more human and conversational.

## Delivery options

- Only if you have delivery options obviously
- Radios
- Default radio
- no validation as there is a default

## Delivery address

- explaining to the user why we ask for their mobile number? To send notificaitons. And if there is no good reason to ask for it, that should drive our design to realise that we shouldn't ask for it, period.
- size of fields and affordance
- address lookup enhancement (expensive but v useful)
- why put mobile on this screen tho-weird?

http://baymard.com/blog/form-field-usability-matching-user-expectations

The width of a field should provide a clue to the length of content it requires. The length of the content provides a clue to the type of content. Both of which help the user fill in a form field.

For example, a postcode is made up of about 8 characters including an optional space. This field should be smaller than other fields in an address form.

If the width of the postcode field is larger to match the other fields, then there's a cognitive burden on the user. They may have to double check the label for example.

![]Good/bad example of address form.

You can apply these principles to other form fields where the length of the field is known. For example, you wouldn't want to apply this principle to street and town because those values could be any length.

## Payment

- Size of fields again.
- valid from date. Dont need ask oyvind and steven.

> Baymard institute usability study found that if a field is too long or too short, users start to wonder if they correctly understood the label. This was especially true for fields with uncommon data or a technical label like CVV (card verification code).

## Guest checkout

We've talked already about having to ask for it, or explaining why. But guest checkout is the number one thing. Whatever you do don't ask users to sign up before checking out unless you absolutely have to. Research shows that 300 million dollar button[^].

## Visual step indicator

When we have a flow like this a step indicator is essential. Or is it - read about GDS guidelines.

## Revealing a billing address if different from delivery.

Checkbox is okay because its for state as well as value.

Can talk about specifying delivery address first or payment address first. Either way an address is necessary.

Could we put a checkbox USE THIS FOR BILLING ON DELIVERY ADDRESS - probably not its a distraction at the wrong time.

Can always use a smart default, so default it to checked on the payment page.

## Smart defaults and one click?

- one click and smart defaults is a benefit, tell the user at the end of guest checkout.
- default delivery option/method
- once information is stored (default del and pay) can jump to conf

## Summary

We've made it to the end of chapter 2, but we have actually covered the majority of issues we face when designing forms. Even if you stopped here and implemented what you now know, the form experiences you'll design will signficantly improve in quality for your users.

In this chapter we've used one of the best, but quite often unheard of One Thing Per Page pattern to our user's advantage and covered off some of the gaps left over in chapter 1 with regards to managing different types of controls such as checkboxes and radios.

In upcoming chapters we'll build on these patterns by solving different problems that we may face when designing more niche experiences on the web.

## Footnotes

[^west]:https://www.smashingmagazine.com/2017/03/world-wide-web-not-wealthy-western-web-part-1/
