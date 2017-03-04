# Checkout

I've worked on more than 15 ecommerce websites over the years but one particular story I want to share is from a while back. In 2008, I worked on a website for Boots.com. I remember this story quite vividly because they were fascinated with the idea of having a single-page checkout flow.

At the time this design pattern was all the rage. And so we designed for them what they wanted. The flow consisted of about 4 steps all represented by an accordion panel. Each step was submitted via AJAX. On success the panel would close and the next one would open.

It was a disaster and Boots were, let's say, unhappy bunnies.

We redesigned it so that each accordion panel became its own page which removed the need for accordions and AJAX. Fortunately, it convertly significantly better and Boots were happy bunnies again.

And history likes to repeat itself and six years later (in 2014), I was once again tasked with helping my fellow UXers redesign their checkout which was functioning in much the same way as the original Boots checkout.

Once again, we redesigned it so that each section became its own page. This had a massively positive impact to the business. And this time I made a note of the numbers. The result wa an extra two million orders annually. And by the way, that's order, not revenue.

Some years later, I found out that putting each thing on a page of its own was a design pattern in its own right. And it had a name and everything. Not that everything needs a label to be good, but it's easier to refer to when it does.

The pattern is known as One Thing Per Page and it is by far and away my favourite design pattern. Apart from the big impact of the statistics there is a lot of rationale behind the pattern which we'll get to shorlty. Before we do that, let's take a brief look at the page refresh, which is rather relevant to all this.

## The page refresh is no bad thing

Browse the Internet. I'll wait here while you do that. Back? Cool. You will have noticed that most websites are cram packed with stuff you just don't need and that your eyes have learnt to glaze over. Even well-known successful websites suffer from this problem.

It's not just the amount of things that is the problem but its the things themselves. Quite often it's form over function. And more literally speaking, we're prone to dumping a lot of high resolution images on a single page.

We do this for several reasons:

1. We have a complex about doing more (Contribution)
2. Departments of successful companies fight for screen real estate and cram shit everywhere.
3. We think people care about our product as much as we do. But they just want the outcome. They don't want to use a website like it's entertainment. We get bored and think we need to put high res images everywhere. Or we're influenced by our competitors
4. Obsessed with some myth about 3 clicks to every outcome.
5. We think having one big thing, is better than having 5 smaller things.
6. We assume people have the same devices and data contracts as we do.

Regardless of the reasons we end up having to come up with "innovative" ways to make our slow pages faster. It's like eating donuts every day, putting on 5 stone, and trying to work out ways to make a fat man run fast. I'm thinking rocket boots, I don't know about you?

When I started writing this section of the book, I didn't think I would be talking about a fat man wearing rocket boots, but hey! "Innovation" is a dirty word and is often seen as a license to spend a lot of time solving problems that are mostly introduced by the same person trying to solve them.

As part of this innovation, somebody announces that *the page refresh is the problem*. But this actually this is rather far from the truth. The real problem is the size of the page itself. AJAX itself doesn't avert the server-side round trip. It just avoids the page refresh.

AJAX still has to repaint/reflow the page. AJAX still has to request stuff from the server. And that's not all. We need to send more code to the client in order to perform AJAX and we need to handle errors and show our own custom loading indicator.

Loading indicators, by the way are a problem because they aren't accurate, like the browser's native implementation. And they aren't familiar.

TODO....

AJAX doesn't avert the server side round trip. It just avoids the page refresh. And that avoidance comes with very real problems. First you need to add *more* code to the page in order to do this. More code does not make websites faster. Second you need more code to recreate the functionality that browsers provide natively for free. That's right spinning indicators.

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