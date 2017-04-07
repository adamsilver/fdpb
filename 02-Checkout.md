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

### Address lookup enhancement

I always advise enhancements to be used with careful consideration and only after proving that the enhancement really is an enhancement to the end user. This is because quite often, the so-called degraded experience (normal form fields) work better anyway. And when we enhance we need to think about accesssibility for all.

With that in mind, a possible enhancement we can use here is to use Capture+ or an equivalent service that allows users to enter their address very quickly.

Capture+ has many options, but on a recent project, we enhanced the address form to show just a single text control. Once the user starts typing the first line of their address, they can easily select the right one from the list. Once they do this, the remaining fields display with the populated values.

![Image here](/etc/)

The user can click *Enter manually* in order to bypass the enhancement, in the unlikely event it can't find the address, or if they prefer to type it out fully themselves.

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

### Grouping controls with fieldset and legend

This is the first form we've encountered that needs grouping both semantically and visually. This is because the radio buttons represent delivery options.

Combining the fieldset and legend is inclusive design pattern. Visually the text provides an overarching description for the group. But also, in most screen readers, this will result in the legend's text being combined with each radio button's label.

For these reasons, we will always use a these elements when we need to group related controls together, both visually and semantically.

You might say that all fields could be grouped in some way. For example, we could wrap the registration form from the previous chapter in a fieldset and legend. Whilst this is valid it creates unnecessary noise to fields that are fully understandable anyway.

It's easy to think of patterns as "right" and "wrong" but, as with most design decisions, we should apply solutions based on the context of the problem at hand. This applies to the use of the fieldset and legend.

We should use them when required. We will see other examples of their usage in upcoming chapters.

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

### Revealing the billing address

When a card is validated, it needs an address. Most often, people's billing address is the same as the delivery address, which is why we expose a checkbox asking the user to confirm whether it's the same as their delivery address.

We do this to save users having to retype the same address which is a source of friction. We ask the question to ourselves, do we have to ask for it? and the answer is maybe.

Because the answer is *maybe* we can provide a smart default. Smart defaults is something we'll discuss again in this chapter as well as upcoming chapters. We mark the checkbox by default, because for most people, it will be the same.

This is another important principle: solve the most common problems first. We also offer users to uncheck the checkbox and in doing so, with Javascript we reveal the address form.

Time for some more Javascript and some ARIA to boot to help screen readers understand what's happening:

```javascript
	var checkbox = document.getElementById('TheID');
	var billingAddressContainer = document.getElementById('etc');
	checkbox.addEventListener('click', onCheckboxClick, false);

	function onCheckboxClick(e) {
		if(checkbox.checked) {
			billingAddressContainer.classList.add('hidden');
			// aria
		} else {
			billingAddressContainer.classList.remove('hidden');
			// aria
		}
	}
	// .enhanced .billingAddress-isHidden { display: none }
```

Like is often the case with Progressive Enhancement, we reuse what we have already as UI controls to enhance. That is, we use the checkbox click even to trigger the showing or hiding of the form.

Once again, we can use the same enhancement for the delivery address for the billing address. This is what design patterns are for. So that we can reuse them and so that the familiarity around them helps users.

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

Still, there's more to it. This page, which at first seems like an extra page is the very reason we can eradicate the need to step through all the pages before it. But I'll leave you on a cliff edge to find out more shortly in the section about Smart Defaults.

All in all, it's important to be honest upfront and put users in control.

### Amending their order

Each section in the flow is represented in the check page. They can edit any of those sections easily by clicking "change". When they do, they are taken to the dedicated page.

This is one of the great things about One Thing Per Page. It allows users to jump back and forth easily. Page loads are fast, and each page has zero noise, providing the user with just what they asked for to make the necessary change.

For example, if they want to update their delivery option to "Next Day" then they simply choose it, and the system should update that, and take the user right back to the Check pages for a final review.

This is an important aspect of designing a Checkout flow using One Thing Per Page. As we collect information, we don't have to ask for it again and can send users to the most advanced step at any time reducing friction and in all likeliness, increasing conversion.

## Confirmatin page

Here is the confirmation page:

[!](etc)

There is no form on this page, so is somewhat out of scope for this book dedicated to designing forms. But we'll discuss some of the points briefly here anyway, because confirmation pages come at the end of a form. And we must not dismiss their importance.

They actually serve many purposes and without one, it leaves users confused. In the case of buying something, they may wonder whether their order went through successfully or not.

The GDS service manual states[^4] each service must have a confirmation page with the following:

* a reference number (if there is one)
* details of what happens next and when
* contact details for the service
* links to information or services that users are likely to need next
* a link to your feedback page, where users can tell you what they think of the service
* a way for users to save a record of the transaction (for example, as a PDF)

Whilst GDS isn't selling something to its users, all of this is applicable to us. But GDS don't cover everything our checkout may need in order to create the best experience for our users.

After the user places an order, their experience is, in many respects, just beginning. We need to continue the relationship, provide users with what they need and so forth.

Much like the Check page, a user may have made a mistake so giving them a way to cancel the order is important. If we do this online, we make this quick for the user, and cheap for the business. So if you can offer this in your confirmation screen.

If users aren't signed in, this is a perfect opportunity to have users sign up by explaining the value they may get in doing so. It's up to you and your business to provide value though. Perhaps it's money off their next order, but it can be something more simple, like offering the capability to track orders or enjoying a speedier checkout experience next time.

We'll discuss the merits of being able to checkout anonymously next.

## Guest checkout

Sometimes, online shops force users to sign up before they are able to place an order. As Jared Spool explains in The 300 Million Dollar Button[^], this is just about the worst thing you can do.

It puts unnecessary friction up front, for no gain at this point in time. Remember when we said before that we would offer a faster checkout experience next time? That's because we have the user's details in order to do this.

But the first time they place an order, we don't have those details. This is why, throughout this book we have to constantly ask ourselves, why are we asking for it?

In this case, why are we asking users to register first? The answer to that question is invariably uncompelling.

By all means get users to sign up, by giving them genuine value, but do so on the confirmation page. We should always be looking to lower barriers, and we can only do that by asking ourselves these questions consistently and rigorously.

## Progress indicators

Age old advice would tell you that we should give users knowledge of where they are in the process. But this is not necessarily the case. There is little data that shows including one is valuable in anyway.

![Local Image](./images/Progress_indicators_2.png)

GDS have shown that they:

- are often not noticed
- take up lots of space
- don’t scale well on small screens
- can distract and confuse some people
- make it hard to write good labels for the steps
- make it hard to handle conditional sections

For these reasons, and much lkike the advice with One Thing Per Page, it's better to start without one, and test to see if one is actually needed. It's much easier to add features down the line than it is to remove them. Less costly that way and easier to measure.

If something is questionable, then by including it, it act as visual noise and makes the page slower, not to mention the aformentioned list above.

Also, if we follow the previous guidance, then we'll have already ensured that we're asking the essential questions, with clear labelling and excellent error message handling. In which case, checking out should be so quick and friction-free anyway.

Research might show you need one. If so go with a simple version first and see how it fairs:

![Local Image](./images/Progress_indicators_1.png)

This simple progress indicator may well be more than enough and doesn't suffer from many of the challenges and problems of a more verbose, traditional indicator.

## Smart defaults

Checkboxes and radio buttons allow us to use smart defaults which is exactly what we've done with *delivery options* and specifying that the *billing address is the same as the delivery address*.

But we can do more than this, particulary for users that are ordering for a second time (and have signed up). Remember when I left you on a cliffhanger with the usefulness of the Check Details page? Now we're getting to the exciting end of the movie. I'm thinking of giving up on the analogies now you'll be glad to know.

Assuming we have encouraged users to sign up, our system will already have the user's information. So we can now speed up checkout massively for most people by sending them all the way to the Check Details page, giving users the chance to review and make amendments (as we've already discussed). We've reduced the number of interactions to potentially one.

That's a friction free checkout experience.

## Order summary

When we go shopping in a physical shop we have everything we need through the experience. It's all close to hand. Evene when we line up to pay, we can still see everything we're buying.

The order summary panel provides this functionality digitally. Including it throughout the checkout process keeps users informed and in control. Which in turn builds trust and keeps them moving.

Not including one puts the user at a severe disdavantage and as soon as they forget what they are buying and the choices they've made they are likely to drop out.

On small screens we put the order summary below the form. On bigger screens we have an opportunity put it beside the form as you can see below:

![](Image)

## Summary

We've made it to the end of chapter 2, but we have actually covered the majority of issues we face when designing forms. Even if you stopped here and implemented what you now know, the form experiences you'll design will signficantly improve in quality for your users.

In this chapter we've used one of the best, but quite often unheard of One Thing Per Page pattern to our user's advantage and covered off some of the gaps left over in chapter 1 with regards to managing different types of controls such as checkboxes and radios.

In upcoming chapters we'll build on these patterns by solving different problems that we may face when designing more niche experiences on the web.

## Footnotes

[^autocomplete fields attribute]:(https://www.smashingmagazine.com/2017/03/improve-billing-form-ux/)
[^west]:https://www.smashingmagazine.com/2017/03/world-wide-web-not-wealthy-western-web-part-1/
[^44]:https://baymard.com/checkout-usabilityworld-wide-web-not-wealthy-western-web-part-1/
[^4]:https://www.gov.uk/service-manual/design/confirmation-pageswestern-web-part-1/
