# Checkout

I've worked on more than 15 ecommerce websites over the years but one particular story I want to share is from a while back. In 2008, I worked on a website for Boots.com. I remember this story quite vividly because they were fascinated with the idea of having a single-page checkout.

At the time this design pattern was all the rage. The design consisted of one page, with four steps (delivery, payment etc) all represented by an accordion panel. Each step was submitted via AJAX&mdash;on success the panel would close and the next one would open.

It was a disaster and Boots were, let's say, unhappy bunnies.

We redesigned it so that each accordion panel became its own page which removed the need for accordions and AJAX. Fortunately, it converted significantly better and Boots were once again, happy bunnies.

They say history repeats itself and six years later, in 2014, I joined Just Eat. I was once again tasked with helping my fellow UXers redesign the checkout flow, which was functioning in much the same way as the original Boots checkout.

We redesigned it so that each section became its own page etc. This had a massively positive impact to the business. And this time I made a note of the numbers. The result was an extra two million orders a year. And just to be clear that's orders, not revenue.

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

In Forms That Work[^] Caroline and Gerry explain the importance of asking questions in a logical order:

> Asking for information at the wrong time can alienate a user. The same question put at the right moment can be entirely acceptable.

> Think about buying a car. You’re just browsing, getting a sense of what is available. A salesperson comes along and starts to ask you how you’ll pay. Would you answer? Or would you think, “If that person doesn’t stop annoying me, then I’m out to here”?

> Now think about the point where you’ve told the salesperson which car you want to buy. Now it’s appropriate to start negotiating about payment. It would be quite odd if the salesperson did **not** do so.

This applies to the checkout flow. Our checkout will have the following steps:

1. Delivery address
2. Delivery options
3. Payment
4. Check details
5. Confirmation

Just like the salesperson, we'll be asking for the right information at the right time. The *Check details* page acts as a final check of contracts and the confirmation acts as sales receipt and invoice for record keeping.

Much like a human conversation, we should strive to make our digital forms human and conversational too.

We'll next look at designing each of the screens in turn as each one has a set problems that need discussing in their own right.

## Delivery address

Here is our delivery address form:

![Image here](/etc/)

Here is the HTML code:

```html
<form>
  <div>
    <label for="recipientName">Recipient name</label>
    <input type="text" id="recipientName" name="recipientName">
  </div>
  <div>
    <label for="mobile">
    	<span class="label">Your mobile</span>
    	<span class="hint">So we can notify you about delivery</span>
    </label>
    <input type="tel" id="mobile" name="mobile">
  </div>
  <div>
    <label for="address1">Recipient address line 1</label>
    <input type="text" id="address1" name="address1">
  </div>
  <div>
    <label for="address2">Recipient address line 2</label>
    <input type="text" id="address2" name="address2">
  </div>
  <div>
    <label for="city">Recipient city</label>
    <input type="text" id="city" name="city">
  </div>
  <div>
    <label for="postcode">Recipient postcode</label>
    <input type="text" id="postcode" name="poscode">
  </div>
  <input type="submit" value="Next">
</form>
```

### Mobile field

As we know, it's important to tell people why we're asking for certain information. At first glance a user may not realise why we ask for a mobile phone in order to deliver a product and take an online order.

But we're asking for it because our couriers offer real-time notifications on the day of delivery. And so in telling our users, we build trust, reduce friction and make it a feature, all the same time.

Those of you have a keen eye for detail will notice that mobile field has `type=tel`. Much like `type=email` discussed in A Registration Form, this will display a telephone-specific keyboard on various mobile devices, improving usability in the process.

[!](Image)

### Post code field

Often designers like to make things line up perfectly, which helps if you're making a piece of art. But well-designed software is not art and in the case of the postcode field, making it match other fields puts a cognitive burden on the user.

The field width provides a clue as to the length of content required for input. The postcode consists of approximately eight characters, therefore this field should be smaller than other fields in an address form as shown.

You can apply these principles to other form fields where the length of the field is known. For example, you wouldn't want to apply this principle to street and town because those values could be any length.

## Delivery options

Here is our delivery address form:

![Image here](/etc/)

Here is the HTML code:

```html
<form>
	<fieldset>
	    <legend>Delivery options</legend>
      <div>
  	    <input type="radio" name="option" id="option1" value="Standard" checked>
  	    <label for="option1">UK Standard (Free, 2-3 days)</label>
      </div>
      <div>
  	    <input type="radio" name="option" id="option2" value="Premium">
  	    <label for="option2">UK Premium (£6, Next day)</label>
      </div>
	</fieldset>
</form>
```

### Introducing the fieldset, legend and radio control

This our first form that needs a `fieldset` and `legend`. These elements are used to group fields together, which is typically the case for checkboxes and in this case radio buttons. We are using radio buttons because we have two mutually exclusive options.

Without the `fieldset` and `legend`:

* visual users won't be able to understand what the options necessarily pertain to; and
* screen readers won't announce the caption when the user is in forms mode.

For these reasons, we will always use a these elements when we need to group related controls together, both visually and semantically.

### Provide a sensible default

It is good practice to default the selection of the first choice by ensuring we use the `checked` attribute. It is also good practice to put the most commonly used choice first.

When we do this, we remove the need for validation and we require less effort from our users, both of which reduce friction.

Obviously, this makes sense here as we put the most often and most economic option first and check it appropriately. But in some situations defaulting is not always a good idea, which we'll cover later.

### Why not use a `select` box?

Historically, designers like using the `select` box because it is compact and takes up little real estate. But actually it's one of the least friendly form controls[^] at our disposal. Here's why:

1. They have little hierarchy control.
2. Browsers and devices don't enlarge the options.
3. Can't be styled very easily cross-browser. Not a huge problem.
4. They hide information behind an unnecessary extra click.
5. They are not searchable. At least not easily searchable.
6. They are taxing to scroll through and select.

Radio buttons don't suffer from these problems. This doesn't mean we should never use a select box, but we'll look into this more in upcoming chapters.

## Payment

Here is our payment form:

![Image here](/etc/)

Here is the HTML code:

```html
<form>
  TBD
</form>
```

### Field size

Again the size of the fields vary depending on their known lengths. Baymard institute usability study[^44] found that if a field is too long or too short, users start to wonder if they correctly understood the label. This was especially true for fields with uncommon data or a technical label like card verification code.

### We don't need to ask for it

You may have noticed that the payment form does not contain a Valid From field but does contain Name On Card. When we designed the payment form for Kidly we had to take into account a couple things.

Firstly, what did our payment provider need to take payment, and second, what did we want for our own records. I spoke to Oyvind Valland, CTO of Kidly about all this. Here is what he had to say:

> We don't need to ask for Valid From. Only a handful of debit cards show those and it provides more hassle for the customer to enter, than benefit to us in verifying card details. That is, if the card is stolen, having to enter a valid from date isn't going to stop the thief.

> Name on card is something we do ask for but I do not believe stripe uses it for verification. If I remember correctly, only the numerics contained in card details are used for verification. That is, house numbers are used, but not street names.

So I asked Oyvind why we have street name in the billing address field. Here's what he said:

> In order to verify card details I think the answer is no. I do recommend that you ask for it for your own records. Being able to eyeball this stuff is very handy in any situation where you have to query what's happened. Besides, I think people kind of expect that they'll have to provide an address (at least one which is used for both billing and shipping)

Oyvind is not a designer per se, but his input into the design is of vital importance. This goes to show the importance of designign as a team sport[^] and that by following the Question Protocol, mentioned in A Registration Form, we can decide what it is we must ask and what it is we want to ask and why.

Proving assumptions are correct or otherwise, is an essential weapon in a designer's arsenal and it is most certainly applicable to form design too.

## Check details page

Here is the check details page:

[!image]()

You may be wondering why we even have this page. It's an extra page, an extra click and we're trying to get users to buy stuff. But what we already know is that users don't mind clicking as long as each click is meaningful.

There is nothing worse than making an order when it wasn't expected. Or making the wrong order. This Check page is vital in scenarios where users have entered quite a lot of information during a flow.

It allows the user to build trust in a system and double check their information before finally commiting to the order. Even the most sophisticated validation mechanism cannot eradicate the chance of human error, even if everything looks right and is formatted correctly.

Giving users the chance to review, before spending their hard earned money, is a human, and quite frankly respectful thing to do.

Imagine Mary, a mother of two, one young baby. She's tired and stressed and it's late at night. She ran out of nappies. With everything that's going on, she chooses the wrong card. It's still her card. It's still a valid card. But she didn't want to use that one because she's close to her overdraft.

Giving her the chance to review on a dedicated page (again One Thing Per Page), gives her the chance to make the change.

Here's another example. Maybe she chose the wrong nappies completely. Without givine her the chance to review, she ends up with the wrong product which is stressful enough and now she has to return it using time she doesn't have. And she still has no nappies.

And it's also costly and time-consuming to the business, especially if they offer free returns. All of which is so avoidable.

All in all, it's important to be honest upfront and put users in control.

### Amending their order

Each section in the flow is represented in the check page. They can edit any of those sections easily by clicking "change". When they do, they are taken to the dedicated page.

This is one of the great things about One Thing Per Page. It allows users to jump back and forth easily. Page loads are fast, and each page has zero noise, providing the user with just what they asked for to make the necessary change.

For example, if they want to update their delivery option to "Next Day" then they simply choose it, and the system should update that, and take the user right back to the Check pages for a final review.

This is an important aspect of designing a Checkout flow using One Thing Per Page. As we collect information, we don't have to ask for it again and can send users to the most advanced step at any time reducing friction and in all likeliness, increasing conversion.

## Confirmatin page

Once making a purchase, the experience is far from over.

- Much like check page could have made mistake
- Give the chance to cancel order and save phone calls and cost of del/returns
- It's also a chance to have a positive confirmation screen
- and get users to sign up if they haven't already done so.
- Confirmation page (refer to gds confirmation pages perhaps)

## Guest checkout

We've talked already about having to ask for it, or explaining why. But guest checkout is the number one thing. Whatever you do don't ask users to sign up before checking out unless you absolutely have to. Research shows that 300 million dollar button[^].

## Visual step indicator

When we have a flow like this a step indicator is essential. Or is it - read about GDS guidelines.

## Revealing a billing address if different from delivery.

Checkbox is okay because its for UI state as well as value.

Can talk about specifying delivery address first or payment address first. Either way an address is necessary.

Can always use a smart default, so default it to checked on the payment page.

## Smart defaults and one click?

- one click and smart defaults is a benefit, tell the user at the end of guest checkout.
- default delivery option/method
- once information is stored (default del and pay) can jump to conf

## Pressing back

- post redirect get

## Order summary (check answers)

- At the bottom of each screen on mobile etc.

## Summary

We've made it to the end of chapter 2, but we have actually covered the majority of issues we face when designing forms. Even if you stopped here and implemented what you now know, the form experiences you'll design will signficantly improve in quality for your users.

In this chapter we've used one of the best, but quite often unheard of One Thing Per Page pattern to our user's advantage and covered off some of the gaps left over in chapter 1 with regards to managing different types of controls such as checkboxes and radios.

In upcoming chapters we'll build on these patterns by solving different problems that we may face when designing more niche experiences on the web.

## Footnotes

- autocomplete fields attribute - include here, find article??

[^west]:https://www.smashingmagazine.com/2017/03/world-wide-web-not-wealthy-western-web-part-1/
[^44]:https://baymard.com/checkout-usabilityworld-wide-web-not-wealthy-western-web-part-1/
