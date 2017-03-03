# Checkout

In 2008 I worked on a website for Boots.com. They wanted a single-page checkout, which was all the rage at the time. It was designed to use the trendiest of techniques from that era including accordions, AJAX and client-side validation.

Each step—delivery address, delivery options and card details—had an accordion panel. Each panel was submitted via AJAX. On successful submission, the panel collapsed and the next one opened.

It was a disaster and Boots were pissed off.

We redesigned it so that each panel became its own page removing the need for accordions and AJAX. However, we kept the client-side validation to avoid an unnecessary trip to the server.

It converted very well. I can't actually remember the statistics so you'll just have to take my word for it. Fortunately, six years later (2014), when I was at Just Eat, the same thing happened. We redesigned the checkout flow so that each section became its own page which resulted in an extra 2 million orders a year. That’s orders, not revenue.

I found out some years later that this approach to design was a thing in its own right. Now not everything has to be labelled to be good, but I was delighted nonetheless because it’s easier to refer to, when it has a name.

Before we get to the pattern itself, let’s take a brief look at the page refresh which is something that comes hand-in-hand with pages.

## The page refresh is no bad thing

Browse the Internet. It won't take you a second to find a website that is crammed with shit you just don't need. Even well-known and successful websites put way too much on the page.

And it's not just that they put way too much on the page, it's that the put the wrong stuff on the page. It's quite often form over function.

This manifestation stems from several places:

1. We have a complex about doing more (Contribution)
2. Departments of successful companies fight for screen real estate and cram shit everywhere.
3. We think people care about our product as much as we do. But they just want the outcome. They don't want to use a website like it's entertainment. We get bored and think we need to put high res images everywhere. Or we're influenced by our competitors
4. Obsessed with some myth about 3 clicks to every outcome.
5. We think having one big thing, is better than having 5 smaller things.

Now, this is a problem itself for many reasons, but not least of which causes designers and developers to "innovate". Innovate by the way is the dirtiest word in our industry. It's like it's a license to spend time doing BS all day long.

What I mean practically speaking is that some smart designers and developers crack their heads together and announce "the page refresh is the problem". But the page refresh is not a problem. Yes there is a round trip on the network, but you can't eliminate a round trip.

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