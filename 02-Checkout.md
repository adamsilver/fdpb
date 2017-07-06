# Checkout

In 2008 I worked on Boots.com. They asked us to design a single-page checkout. This included the trendiest of techniques from that era: accordions, AJAX and client-side validation.

Each step: delivery address, delivery options and payment was an accordion panel. Each panel was submitted via AJAX. On successful submission, the panel collapsed and the next one opened.

It looked like this:

![Single-page accordion](./images/boots1.png)

Users struggled to complete their orders. Errors were hard to fix as users had to scroll up and down. And the the accordion was a distraction. Inevitably, the client asked us to make changes.

We redesigned it so that each panel became its own page removing the need for an accordion and AJAX. However, we kept the client-side validation to avoid an unnecessary trip to the server.

It looked like this:

![Multiple pages, no accordion](./images/boots2.png)

This converted much better. I can’t remember the exact numbers but I know that the client was happy.

Six years later, in 2014, at Just Eat, the same thing happened. We redesigned the single-page checkout flow so that each section became its own page. This time I made a note of the numbers. The result was an extra 2 million orders a year. That’s *orders*, not revenue.

Here are some of the mobile-first designs we used:

![Just Eat checkout](./images/justeat.png)

A couple of years later, in 2016, Robin Whittleton from GDS, told me that putting each thing on a page of its own was a design pattern  known as One Thing Per Page[^]. Apart from the numbers there is a strong rationale behind the pattern, which we’ll get to shortly.

First, we'll take a look at exactly what this pattern is.

## What is One Thing Per Page?

One Thing Per Page is not necessarily about having one element or component on a page (although it could). In all likeliness you’ll still have, for example, a header and footer. Similarly, it’s not about having a single form field on each page either (although, again, it could).

This pattern is about splitting up a complex process into multiple smaller pieces, and placing those smaller pieces on screens of their own. For example, instead of placing the address form on the same page as the delivery options and payment forms, we put it on a separate page.

An address form has multiple fields, but it’s a single, tangible question that is being asked of the user. It makes sense to tackle this question on one screen.

Next we’ll look at why the pattern is so good.

## Why is it so good?

Whilst this pattern often bares wonderful and delicious fruit (or orders and conversions if you hate my analogies) it’s nice to understand the rationale behind it.

### 1. It reduces cognitive load

Like Ryan Holiday describes in The Obstacle Is The Way, remember the first time you saw a complicated algebra equation? It was a jumble of symbols and unknowns. But when you stopped to break it down and isolate the parts, all that was left was the answer.

It’s the same for users trying to complete a form or anything else for that matter. If there is less stuff on screen, and only one choice to make, friction is reduced to a minimum. Therefore users stay on task.

### 2. It's easy to fix errors

When users fill in a small form, errors are caught and presented early. If there’s one thing to fix, it’s easy to fix, which reduces the chance of users giving up.

### 3. Pages load faster

If pages are small by design, they will load faster. Faster pages reduce the risk of users leaving and they build trust in the service.

### 4. It's easy to track progress and return to previous steps

If users submit information frequently, we can save it in a more granular fashion. If a user drops off, we can send them an email, prompting them to complete their order, for example.

### 5. It reduces the chance of losing information

A large form takes longer to complete. If it takes too long, then a page timeout may cause the information to be lost, causing tremendous frustration.

Alternatively, the computer freezes, which was the case for Daniel, the leading character in *I, Daniel Blake*. With declining health and having never used a computer before, his computer freezes and his data lost. In the end, he gives up.

I've counted 16 problems in total, which you can read about in Better Form Design: One Thing Per Page[^].

This inconspicuous and humble UX pattern is flexible, performant and inclusive by design. It truly embraces the web, making things easy for high and low confidence users.

Having lots (or everything) on one page, may give an illusion of simplicity, but like algebraic equations, they are difficult to deal with unless they are broken down.

If we consider a task as a transaction that a user wants to complete, breaking it down into multiple steps makes sense. It’s as if we’re using the very building blocks of the web as a form of progressive disclosure. And the metaphor behind pages provides a subconscious sense of moving forward.

We'll use this approach to design the checkout flow.

## Flow and order

In Forms That Work[^], Caroline Jarett and Gerry Gafney explain the importance of asking questions in a sensible order:

> Asking for information at the wrong time can alienate a user. The same question put at the right moment can be entirely acceptable.

> Think about buying a car. You’re just browsing, getting a sense of what is available. A salesperson comes along and starts to ask you how you’ll pay. Would you answer? Or would you think, “If that person doesn’t stop annoying me, I’m out of here”?

> Now think about the point where you’ve told the salesperson which car you want to buy. Now it’s appropriate to start negotiating about payment. It would be quite odd if the salesperson did **not** do so.

We can apply the same principles to the steps in checkout:

1. Delivery address
2. Delivery options
3. Delivery notes
4. Payment
5. Check and confirm
6. Order confirmation

Just like the car salesperson, we'll ask for the right information at the right time. The *Check and confirm* step acts as a final check of contracts and the confirmation acts as sales receipt for record keeping.

## Delivery address

What it looks like:

![Delivery form](./images/?.png)

HTML:

```html
<form novalidate>
  <div class="field">
    <label for="recipientName">
		<span class="field-label">Recipient name</span>
    </label>
    <input type="text" id="recipientName" name="recipientName">
  </div>
  <div class="field">
    <label for="mobile">
    	<span class="field-label">Your mobile</span>
    	<span class="field-hint">So we can notify you about delivery</span>
    </label>
    <input type="tel" id="mobile" name="mobile">
  </div>
  <div class="field">
    <label for="address1">
    	<span class="field-label">Recipient address line 1</span>
    </label>
    <input type="text" id="address1" name="address1">
  </div>
  <div class="field">
    <label for="address2">
    	<span class="field-label">Recipient address line 2</span>
   	</label>
    <input type="text" id="address2" name="address2">
  </div>
  <div class="field">
    <label for="city">
    	<span class="field-label">Recipient city</span>
    </label>
    <input type="text" id="city" name="city">
  </div>
  <div class="field">
    <label for="postcode">
    	<span class="field-label">Recipient postcode</span>
    </label>
    <input type="text" id="postcode" name="postcode">
  </div>
  <input type="submit" value="Next">
</form>
```

We've used the same field pattern from the first chapter with a label and optional hint for each field. But there are some specific things to note.

### Mobile field

In the first chapter, we discussed just how important it was to tell users, on occasion, why it is we're asking for particular information. Why is it, then, are we asking for the user's phone number, especially when they're ordering online?

The courier who is fulfilling delivery actually offers real-time text messages on the day of delivery. So we tell the users this through by using the hint pattern. This builds trust, reduces friction and promotes the feature, all at the same time.

The mobile input uses `type=tel`. This displays a telephone-specific on-screen keyboard on mobile. This makes it far easier to enter their phone number.

![On-screen device keyboard](./images/?.png)

### Postcode field

Designers are obsessed with clean lines and matching widths. In the case of the delivery form, we might be tempted to give each field the same width. It's hard to argue aginst the beauty of such a design, but we're not installing a minimalist art display.

We're designing a form for people to complete easily. Making the postcode field wide (to match the others) is a cognitive burden on the user. This is because the width of the field gives users a clue as to the length of the content required.

Baymard Institute's study[^44] found that *if a field is too long or too short, users start to wonder if they correctly understood the label. This was especially true for fields with uncommon data or a technical label like card verification code.*

As a postcode consists of approximately 8 characters, field should have width to match as shown above. This rule doesn't apply just to the postcode, we can use the same technique on other fields where the length of field is known.

### Capture+

Capture+[^] is a Javascript plugin that allows users to search for their address quickly and easily. Instead of manually typing each part of the address in 5 separate boxes, it offers users a single text box.

![Capture+ enhancement](./images/?.png)

As the user types the first line of their address, suggestions appear from which to select. This reduces the amount of keystrokes and the chance of typos. If Capture+ doesn't recognise an address, the user can change the interface back to a standard address form giving users choice.

This type of Javascript enhancement comes with many design considerations. We could choose to abdicate responsibility for these by handing then over to the plugin. But in all likeliness they haven't considered the accessibility implications of doing so. We'll look at these implications in the next chapter by designing and building our own inclusive autocomplete component.

## Delivery options

What it looks like:

![Delivery options](./images/?.png)

HTML:

```html
<form novalidate>
	<fieldset>
		<legend>Delivery options</legend>
		<div>
		    <input type="radio" name="option" id="option1" value="Standard" checked>
		    <label for="option1">Standard (Free, 2-3 days)</label>
		</div>
		<div>
		    <input type="radio" name="option" id="option2" value="Premium">
		    <label for="option2">Premium (£6, Next day)</label>
		</div>
	</fieldset>
	<input type="submit" value="Next">
</form>
```

This is the first time we've encountered a field that consists of multiple inputs for the same question in the form of radio buttons. There are some special things to note.

### Using `fieldset` and `legend`

To group multiple inputs together, we need to use the fieldset and legend elements. Each input represents a choice and the grouping is achieved by wrapping it inside a fieldset. The legend describes this grouping, in much the same way as the label describes the input.

Like the label, the legend provides a description both visually and for those using a screen reader. In most screen readers, the legend is read out in combination with the radio button's label. Something like *Delivery options, Standard (Free, 2-3 days)*.

It may be tempting to group all fields in form like this. Chapter one's registration form could, in theory, be wrapped in a fieldset and legend. Whilst this is valid, it creates unnecessary noise to those fields that are perfectly understandable without this treatment.

As Heydon Pickering says in Inclusive Design Patterns[^], *it's easy to think of patterns as ‘right’ and ‘wrong’*. As with most design decisions, we should apply solutions based on the context of the problem, not by blindly following a blanket rule.

### Provide a sensible default

The first radio button has a `checked` attribute. This selects the first delivery option automatically. This has two benefits as there is:

- less work for the user to do
- no chance of causing errors

Where possible the first radio button should be checked. This is because the most common choice should normally come first. In this case, we've sensibly assumed this to be the cheapest option.

## Delivery notes

Imagine you've ordered your thing and it's on the way. You're excited to receive it. But then, it was too big to fit through the letter box and you weren't there to answer the knock at the door. This is where delivery notes can help.

A delivery note tells the delivery person what to do in this event. It might be preferable to leave it with a neighbour for example, or by leaving it in the recycling bin, which works surprisingly well I might add.

What it looks like:

![Delivery notes](./images/?.png)

HTML:

```HTML
<form novalidate>
	<div class="field">
	    <label for="notes">
	    	<span class="field-label">Delivery notes</span>
	    	<span class="field-hint">Tell the delivery person what to do if you're not in. Such as leave it with the next door neighbour. No more than 150 characters.</span>
	    </label>
	    <textarea id="notes" name="notes"></textarea>
  	</div>
  	<input type="submit" value="Next">
</form>
```

The HTML is remarkably similar to most of the other fields we've discussed so far including the hint pattern. But this is the first time we've seen a `textarea`. There are some special things to note.

### Textarea

A `textarea` is like a text box except that it allows many lines of text, and typically takes more content. (Remember, the size of the field should afford its requirements.)

However the problem with a textarea is that it seemingly takes an infinite amount of characters. But the Question Protocol, as discussed in chapter 1 tells us that we need to know what it is we're doing with a piece of information before we can determine how we design it for the interface.

In this case, the device that shows the notes has a limited amount of space to fit the note into. And it doesn't allow scrolling. Even if it did, a lot of text to wade through could cause a lot of wasted time. We need to limit the amount of text that can be entered.

### Limiting characters

The `maxlength` attribute limits the amount of text users can enter. However, support is either lacking or buggy[^caniuse]. Worse though, is that `maxlength` literally prevents the user from entering too many characters.

This is a problem because some users solely focus on the keyboard whilst typing. When they finally look up at the screen, they'll realise the system ignored paragraphs of text they spent ages entering. This causes frustration and a distrust in the service.

Instead, we'll need to create a custom component that indicates to users how many characters they have left. Importantly, the component won't interupt users and it won't stop the user entering too many characters.

For those that don't look up, they'll get feedback then and can take action to reduce the characters. For those that miss the feedback entirely, validation will look after them.

### Characters remaining component

What it looks like:

[]()

The Javascript:

```javascript
function CharactersIndicator(field, options) {
	this.field = $(field);
	this.status = $('<div class="indicator" role="alert" aria-live="polite" />');
	this.setOptions(options);
	this.updateStatus(this.options.maxLength);
	this.field.parent().append(this.status);
	this.field.on("keydown", $.proxy(this, 'onFieldChange'));
};

CharactersIndicator.prototype.setOptions = function(options) {
	this.options = {
		maxLength: 100,
		message: 'You have %count% characters remaining.',
	};
	this.options = $.extend(this.options, options);
};

CharactersIndicator.prototype.onFieldChange = function(e) {
	var remaining = this.options.maxLength - this.field.val().length;
	this.updateStatus(remaining);
};

CharactersIndicator.prototype.updateStatus = function(remaining) {
	var message = this.options.message.replace(/%count%/, remaining);
	this.status.html(message);
};
```

Notes:

- It creates a status which will tell users how many characters remain and place it below the field.
- The status has a `role="status"` and `aria-live="polite"` which ensures the updates are read out by screen readers. Polite just means that the announcement won't happen until the user stops typing, meaning they aren't rudely interrupted.
- As the user types, the status box is updated with a message.

## Payment

What it looks like:

![Payment](./images/?.png)

HTML:

```html
<form novalidate>
	<div class="field">
		<label for="nameoncard">
			<span class="field-label">Name on card</span>
		</label>
		<input type="text" id="nameoncard" name="nameoncard">
	</div>
	<div class="field">
		<label for="cardnumber">
			<span class="field-label">Card number</span>
		</label>
		<input type="number" id="cardnumber" name="cardnumber">
	</div>
	<div class="field">
		<label for="expiry">
			<span class="field-label">Expiry date</span>
		</label>
		<input type="number" id="expiry" name="expiry">
	</div>
	<div class="field">
		<label for="security">
			<span class="field-label">Security code</span>
		</label>
		<input type="number" id="security" name="security">
	</div>
  <div class="field">
    <input type="checkbox" id="sameAsDelivery" name="sameAsDelivery">
    <label for="security">
      <span class="field-label">My billing address is the same as my delivery address</span>
    </label>
  </div>
	<input type="submit" value="Next">
</form>
```

https://about.futurelearn.com/blog/your-courses-my-courses-personal-pronouns

### Field size

- do we say it again or not?

### We don't need to ask for it

You may have noticed that the payment form does not contain a *Valid From* field but does contain *Name On Card*. When we designed the payment form for Kidly we considered the following:

- What did our payment provider require to process payments?
- What did we want to keep for our own records?

Øyvind Valland, CTO of Kidly, explains the thinking behind the decision. He says:

> We don't need to ask for Valid From. Only a handful of debit cards show those and it provides more hassle for the customer to enter, than benefit to us in verifying card details. That is, if the card is stolen, having to enter a valid from date isn't going to stop the thief.

> Name on card is something we do ask for but I do not believe stripe uses it for verification. If I remember correctly, only the numerics contained in card details are used for verification. That is, house numbers are used, but not street names.

But why did we ask for street name as part of the billing address? Here's what he says:

> In order to verify card details I think the answer is no. I do recommend that you ask for it for your own records. Being able to eyeball this stuff is very handy in any situation where you have to query what's happened. Besides, I think people kind of expect that they'll have to provide an address (at least one which is used for both billing and shipping)

Oyvind is not a designer per se, but his input into the design is of vital importance. This goes to show the importance of designing as a team[^] and that by following the Question Protocol, mentioned in A Registration Form, we can decide what it is we must ask and what it is we want to ask and why.

Proving assumptions are correct or otherwise, is an essential weapon in a designer's arsenal.

### Expiry date

Generally speaking, dates are hard. But, fortunately, an expiry date is straightforward to design and easy to use. We offer users a single text box that closely matches the format found on their own physical credit card. This reduces the cognitive burden for users as they can just copy what they see.

> ‘Be conservative in what you send; be liberal in what you accept.’

We can forgive users for entering a slash as we can easily strip that out on the server. We do the hard work so users don't have to. Additionally, we still give users a hint, so that users who are more careful and anxious&mdash;those that read all instructions&mdash;they will feel at ease and empowered at the same time.

You'll also notice the expiry date uses `input type="number"`. For supporting browsers, users cant't type a slash; it will be ignored. On mobile, an on-screen numeric keyboard will be shown making it easier still. This is what it looks like:

![Numeric keyboard](./images/numeric-keyboard.png)

And for people who use the keyboard on desktop, may use the up and down arrows to increment and decrement the field without the pain of selecting/deleting/typing etc.

Some browsers may also show little increment and decrement buttons on these inputs, known rather oddly as spin buttons. They are quite ugly, small and ahrd to use&mdash;we can hide them with CSS:

```CSS
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    appearance: none;
    margin: 0;
}
```

### Security number

You'll notice above that we have once again used the hint pattern for the security number. In this case the label may be ambiguous on it's own. Not everyone knows what a security number is. Sometimes it's referred to as a CVC number.

I've just looked it up and this stands for Card Verification Code. First, avoid acryonyms[^] because users shouldn't have to know what they mean. Second, in providing a hint we have an opportunity to tell users what it is and more importantly where to find it.

Telling users *This is the last 3 digits on the back of your card* keeps things simple.

### Billing address reveal

To validate a card, it needs an associated address. For most users the billing address is the same as the delivery address. The user has already provided this and so we can improve the experience here.

Instead of always exposing a billing address form, we provide users a checkbox asking them to confirm if their billing address is the same as their delivery address.

As this is the most common scenario we mark the checkbox as checked by default, and hide the billing address form. If the billing address *is* different, the user can uncheck the checkbox and fill out the form from there.

To do this we'll need a little Javascript.

```javascript
var checkbox = document.getElementById('TheID');
var billingAddressContainer = document.getElementById('etc');
checkbox.addEventListener('click', onCheckboxClick, false);

function onCheckboxClick(e) {
	if(checkbox.checked) {
		billingAddressContainer.classList.add('billingAddress-isHidden');
		// aria to do
	} else {
		billingAddressContainer.classList.remove('billingAddress-isHidden');
		// aria to do
	}
}
```

```CSS
.enhanced .billingAddress-isHidden {
	display: none;
}
```

When the checkbox is checked we hide the billing address and ensure screen readers know to ignore the fields. When it is unchecked we show the billing address.

You'll also notice that the billing address form looks and functions exactly the same as the delivery address. This is important. In using the same pattern in multiple places, not only do we get to reuse the same solutions in the same way, but users become familiar with them too.

Familiarity is an important UX principle because familiar is easy and requires less effort from the user.

## Check Details Page

What it looks like:

![Check details](./images/?.png)

You may be wondering why we even have this page. It's an extra page, an extra click and afterall we're trying to get users to buy stuff!

TODO: BUTTON TEXT Don't mislead etc.

However, in a checkout flow, there is nothing worse than submitting an order that the user didn't expect. Or equally, submitting the *wrong* order.

Even the most sophisticated validation mechanism cannot eradicate the chance of human error. Even if everything *looks* right and is formatted correctly, it doesn't mean mistakes aren't present.

For example, take Mary (I made her up), a mother of two, one of which is a baby. It's late at night and shes tired and stressed. To make it worse she's ran out of nappies.

She goes online, adds them to her basket, and goes to checkout. But she ordered the wrong nappies and used the wrong card. Both the nappies and the card are valid. But she needed different nappies and she wants to use a different card&mdash;one that is not in the red.

Having spent a lot of time filling out a lot of information to complete the order, it's only human and respectful to give her the chance to review her order and make amends where necessary.

Not only that but it saves the business a lot of time and money too. If she makes the wrong order then she'll need to return them. This is costly and time-consuming, especially if the business offers free returns.

By removing this step, it makes users anxious and therefore drop out. We should strive for clarity over brevity every time. Providing a check page like this employs this principle.

### Amending their order

Each section in the flow is represented in the check page. They can edit any of those sections easily by clicking "change". When they do, they are taken to the dedicated page.

![Reference One Thing Per Page Smashing Screenshot](./images/?.png)

This is one of the great things about One Thing Per Page. It allows users to jump back and forth easily. Page loads are fast, and each page has zero noise, providing the user with just what they asked for to make the necessary change.

For example, if they want to update their delivery option to "Next Day" then they simply choose it, and the system should update that, and take the user right back to the Check pages for a final review.

This is an important aspect of designing a Checkout flow using One Thing Per Page. As we collect information, we don't have to ask for it again and can send users to the most advanced step at any time reducing friction and in all likeliness, increasing conversion.

## Confirmation page

What it looks like:

![Confirm screen](./images/?.png)

Technically, there is no form on this page, but confirmation pages are common at the end of a long form process and they are very much needed.

They serve many purposes and without one, it leaves users confused. In the case of buying something, they may wonder whether their order went through successfully or not.

The GDS service manual states[^4] each service must have a confirmation page with the following:

- a reference number (if there is one)
- details of what happens next and when
- contact details for the service
- links to information or services that users are likely to need next
- a link to your feedback page, where users can tell you what they think of the service
- a way for users to save a record of the transaction (for example, as a PDF)

Whilst GDS isn't selling something to its users, all of this is applicable to us here. But there's more for us to take into consideration...

After the user places an order, their experience is, in many respects, just beginning. We need to continue the relationship, provide users with what they need and so forth.

Much like the Check page, a user may have made a mistake so giving them a way to cancel the order is important. If we do this online, we make this quick for the user, and cheap for the business.

If users aren't signed in, this is a perfect opportunity to have users sign up by explaining the value they may get in doing so. It's up to you and your business to provide value though.

Perhaps it's money off their next order, but it can be something more simple, like offering the capability to track orders or enjoying a speedier checkout experience next time.

We'll discuss the merits of being able to checkout anonymously next.

## Guest checkout

Sometimes, online shops force users to sign up before they are able to place an order. As Jared Spool explains in The 300 Million Dollar Button[^], this is just about the worst thing we can do.

It puts unnecessary friction up front, for no gain at this point in time. Remember when we said earlier that we would offer a faster checkout experience next time? That's because we have the user's details in order to do this.

But the first time they place an order, we don't have those details. This is why we religiously ask ourselves *why are we asking for it?*

In this case, why are we asking users to register first? The answer to that question is invariably uncompelling.

By all means get users to sign up, by giving them genuine value, but do so on the confirmation page. We should always be looking to lower barriers, and we can only do that by asking ourselves these questions on a regular basis.

## Progress indicators

Traditional advice says we should give users knowledge of their progress within a particular process. But this is not necessarily the case. There is little data that shows including one is valuable.

![Progress indicator](./images/Progress_indicators_2.png)

GDS say that they:

- are often not noticed
- take up lots of space
- don’t scale well on small screens
- can distract and confuse some people
- make it hard to write good labels
- make it hard to handle conditional sections

For these reasons, it's better to start without one, and test to see if one is actually needed. It's much easier to add features down the line than it is to remove them. It's cheaper and easier to measure impact.

If something is questionable, then by including it, it acts as visual noise and makes the page slower.

Also, if we follow the previous guidance, then we'll have already ensured that we're asking the essential questions, with clear labels and error messages. In which case, checking out should be so quick and friction-free anyway.

If, however, research shows you need one, go with a simple version first:

![Local Image](./images/Progress_indicators_1.png)

This progress indicator may well be enough and doesn't suffer from many of the challenges and problems of a more verbose, traditional indicator.

## Smart defaults

As we've already discussed, checking checkboxes and radio buttons by default helps users. But we can do more. A lot more. Particularly for those who are order for a second time.

As we have stored their information from the previous order, we can send them all the to the Check Details page again giving the users the chance to review and make amends.

In this case we've reduced the number of interaction points to potentially one. This is the mark of a friction-free checkout experience.

## Order summary

What it looks like:

![Order summary panel](./images/?.png)

When we go shopping in a physical shop we have everything we need through the experience. It's all close to hand. Even when we line up to pay, we can still see everything we're buying.

The order summary panel provides this functionality digitally. Including it throughout the checkout process keeps users informed and in control. Which in turn builds trust and keeps them moving.

Not including one puts the user at a severe disdavantage and as soon as they forget what they are buying and the choices they've made they are likely to drop out.

On small screens we put the order summary below the form. On bigger screens we have an opportunity put it beside the form as follows:

![Order summary panel](./images/?.png)

## Summary

In this chapter we've covered what it takes to design a checkout flow using the One Thing Per Page design pattern. Out of this pattern comes several advantages.

Most designers overcomplicate things by putting too much on a page. But by making each step focused, we make each step simple. In providing what appears to be a longer process, we significantly reduce the burden for *users*.

We'll draw on these techniques further in upcoming chapters.

## TODO

CVC number disappearing due to another error. Means the user fixes the error and resubmits. If you have to clear this for security, first find out why, and second if so, then tell the user to reenter and why so they don't get pissed off.
https://www.uie.com/jared-live/#design-opposed (42 mins)

--- DIFFERENT HEADERS IN CHECKOUT

## Footnotes

[^autocomplete fields attribute]: (https://www.smashingmagazine.com/2017/03/improve-billing-form-ux/)
[^west]: (https://www.smashingmagazine.com/2017/03/world-wide-web-not-wealthy-western-web-part-1/)
[^44]: (http://baymard.com/blog/form-field-usability-matching-user-expectations)
[^4]: (https://www.gov.uk/service-manual/design/confirmation-pageswestern-web-part-1/)
