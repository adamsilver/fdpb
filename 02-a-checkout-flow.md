# A Checkout Flow

In 2008 I worked on Boots.com where we designed a single-page checkout flow. This involved the trendiest of techniques from that era including accordions, AJAX and client-side validation. Each step: delivery address, delivery options and payment was an accordion panel. Each panel was submitted via AJAX. On successful submission, the panel collapsed and the next one opened.

Users struggled to complete their orders. Errors were hard to fix as users had to scroll up and down. And the accordion was a distraction. Inevitably, Boots asked us to make changes. We redesigned it so that each panel became its own page removing the need for an accordion and AJAX. (We kept the client-side validation to avoid an unnecessary trip to the server.)

![Multiple pages, no accordion](./images/boots2.png)

This converted a lot better. Although I can't remember the exact numbers, the client was happy with the results. Six years later, in 2014, at Just Eat, the same thing happened. We redesigned the single-page checkout flow so that each section became its own page. This time I noted the numbers. The result was a 5% increase in conversion. This is equated to 2 million orders a year. That’s *orders*, not revenue.

![Just Eat checkout](./images/justeat.png)

Two years later, in 2016, Robin Whittleton from GDS, told me that putting each thing on a page of its own was a design pattern called One Thing Per Page[^1]. Apart from the numbers there are many reasons why this pattern drastically improves the user experience.

## One Thing Per Page

One Thing Per Page is about splitting up a complex process into multiple smaller pieces, and placing those smaller pieces on screens of their own. For example, instead of placing Delivery Address on the same page as the Delivery Options and Payment, we put it on a separate page.

It's not necessarily about having one element or component on a page (although it could). In all likeliness you’ll still have, for example, a header and footer. Similarly, it’s not about having a single form field on each page either (although, it absolutely could).

This doesn't mean you'll always end up with one question per page. Caroline Jarrett, who first wrote about the pattern in 2015, explains that user research “will quickly show you that some questions will be best grouped into a longer page.”

However, she also explains that “questions that naturally ‘go together’ from the point of view of designers, don’t need to be on the same page to work for users.” She provides an enlightening example when, for GOV.UK Verify, they tested putting ‘Create a username’ on one page and ‘Create a password’ on the next.

Like most designers, Caroline thought that putting them on separate pages would be overkill. In reality, users weren’t bothered. So start with one thing (field, question) per page, then, through research, find out if grouping fields improves the experience.

That is not to say you’ll always end up combining screens together—in my experience, the best results come from splitting things apart, period.

Whilst this pattern often bares wonderful and delicious fruit (or orders and conversions if you hate my analogies) it’s useful to understand why it works so well.

- Principle 6 says that we should *design interfaces that help users focus on core tasks by prioritising it*. It even goes on to say that *people should be able to focus on one thing at a time*. One Thing Per Page is simply following this principle to the letter, and in doing so it drastically reduces the cognitive burden on users.
- When users fill in a small form, errors are caught and shown early and often. If there’s one thing to fix, it’s easy to fix, which reduces the chance of users giving up on the task.
- If pages have little on them, they'll load quickly. Faster pages reduce the risk of users leaving and they build trust.
- By submitting information frequently, we can save user's information in a more granular fashion. If a user drops out we can, for example, send them an email prompting them to complete their order.
- Conversely, a long form takes a long time to complete. It it takes too long there's a risk that the page times out or the computer freezes causing data loss. This is what happens to Daniel, the lead character in “I, Daniel Blake”[^]. With declining health and having never used a computer, it freezes and he loses his data. In the end, he gives up.
- It adds a sense of progression and increases momentum because the user is constantly moving forwards step by step.

## Flow and order

In “Forms That Work”[^], Caroline Jarett and Gerry Gafney explain the importance of asking questions in a sensible order. They say:

> Asking for information at the wrong time can alienate a user. The same question put at the right moment can be entirely acceptable.

> Think about buying a car. You’re just browsing, getting a sense of what is available. A salesperson comes along and starts to ask you how you’ll pay. Would you answer? Or would you think, “If that person doesn’t stop annoying me, I’m out of here”?

> Now think about the point where you’ve told the salesperson which car you want to buy. Now it’s appropriate to start negotiating about payment. It would be quite odd if the salesperson did not do so.

Just like the car salesperson, we'll ask for the right information at the right time. We'll leave payment until the end and give uses a chance to check their order before finally submitting it. Afterwards, the confirmation screen acts a sales receipt for record keeping. The complete flow is as follows:

1. Email address
2. Mobile phone (optional)
3. Delivery address
4. Delivery options
5. Delivery notes
6. Payment
7. Check your answers
8. Confirmation

## 1. Email Address

In chapter one, “A Registration Form”, we had to ask users for an email address. We can reuse that pattern here too, meaning we don't have to solve the same problem from scratch.

There is, however, an opportunity to adapt the content to fit this context. By that I mean, users may wonder why their being asked for an email address just to buy a product. One of the main takeaways from chapter one, was the need to justify the existence of each and every form field.

Here, it's because we can send users a receipt of purchase, essential when checking out anonymously (guest checkout) whilst simultaneously providing information as to how to return it. In which case, we can tell users through the hint text.

![We need this to send you a receipt](.)

The submit button is also reused. It is position and styled the same way too. The only difference is the button's label, which is set to “Continue”. This verb implies progress which is perfectly suited to the checkout flow.

*(Note: logged-in users won't see this screen. I'll cover the second-time, logged-in experience later on.)*

## 2. Mobile Phone

Like the email field, we should be asking ourselves why we're asking for their phone number. We know that the courier offers real-time text messages on the day of delivery. But the customer doesn't. So we tell them via the hint&mdash;this pattern doesn't just pertain to formatting rules. This transparency builds trust, reduces friction, and promotes the feature all at the same time.

![Mobile screen](.)

```html
<div class="field">
  <label for="mobile">
    <span class="field-label">Your mobile (optional)</span>
    <span class="field-hint">So we can notify you about delivery</span>
  </label>
  <input type="tel" id="mobile" name="mobile">
</div>
```

The input's `type` attribute is set to `tel` which on mobile phones will spawn a telephone-specific keyboard. This makes it easier to enter a phone number thanks to the larger keypad.

![Tel keyboard](.)

### Marking Optional/Required Fields

Whilst real-time notifications *add value*, we shouldn't assume everyone wants to receive them, nor that everyone has a mobile phone. So we follow principle 5, *Offer choice*, by marking the field as optional. This way, users can opt-in if they want.

Traditionally, required fields are marked with an asterisk. A legend, is usually placed above the form to denote its meaning. Luke Wobrelski says “including the phrase ‘optional’ after a label is much clearer than any visual symbol you could use to mean the same thing. Someone may always wonder ‘what does this asterisk mean?’ and have to go hunting for a legend that explains it.”

You might also be wondering why we're marking optional fields, instead of required ones. In “Required Versus Optional Fields”[^], Jessice Enders says “think about what we are doing when we mark something in an interface. We are trying to indicate that it's different.” Thanks to the Question Protocol, most fields should be required, so we mark optional fields instead. And we do that by adding “(optional)” inside the label.

## 3. Delivery Address

![Delivery form](.)

```HTML
<div class="field">
  <label for="address1">
  	<span class="field-label">Address line 1</span>
  </label>
  <input type="text" id="address1" name="address1">
</div>
<div class="field">
  <label for="address2">
  	<span class="field-label">Address line 2 (optional)</span>
 	</label>
  <input type="text" id="address2" name="address2">
</div>
<div class="field">
  <label for="city">
  	<span class="field-label">City</span>
  </label>
  <input type="text" id="city" name="city">
</div>
<div class="field">
  <label for="postcode">
  	<span class="field-label">Postcode</span>
  </label>
  <input type="text" id="postcode" name="postcode">
</div>
```

The delivery address contains 5 fields that together make up an address. Visually there is a slight difference between the fields: field width.

### Field Width

In “Write Less Damn Code”[^4], Heydon Pickering jokingly points out, that the reason some people added XHTML 1.1 Complaint banners to their website was to ensure the height of the menu matches the height of the content. Similarly, you might be tempted to give every address field the same width.

But, giving the postcode field the same width as every other field increases the cognitive burden for users. This is because the the width of the field gives users a clue as to the length of the content it requires.

Baymard Institute's study[^5] found that “if a field is too long or too short, users start to wonder if they correctly understood the label. This was especially true for fields with uncommon data or a technical label like card verification code.”

As postcodes consist of 6-8 characters, the field should have a matching width that is smaller than the other fields. You should apply this rule to every field whereby the length of the content is known.

### Capture+ Enhancement

Capture Plus[^6] is a third party plugin that lets users search for their address quickly and accurately. Instead of manually typing each part of the address in 5 separate boxes, users can type into just one. As the user types the first line of their address, suggestions appear from which they can select. This reduces the amount of keystrokes and therefore the chance of typos. 

![Capture+ Enhancement](.)

If no address is found, users can change the interface back to the original address form. In doing so, we conform to principle 5, *offer choice*.

Capture+ offers a solution out of the box: you include their script and you're away. Except that you're not. Most third-party scripts don't consider the broad range of interaction preferences, usability and acessibility considerations that need to be taken into account. But, we'll look at all of this and more in the next chapter, when we build an accessibile autocomplete component.

## 4. Delivery Options

![Delivery options](./images/?.png)

```html
<div class="field">
	<fieldset>
		<legend>
			<span class="field-legend">Delivery options</span>
		</legend>
		<div class="field-radioButton">
		  <input type="radio" name="option" id="option" value="Standard" checked>
		  <label for="option">Standard (Free, 2-3 days)</label>
		</div>
		<div class="field-radioButton">
		  <input type="radio" name="option" id="option2" value="Premium">
		  <label for="option2">Premium (£6, Next day)</label>
		</div>
	</fieldset>
</div>
```

This is the first field that consists of multiple controls, in this case, radio buttons.

### Grouping

To group multiple controls together, we must wrap them in a `fieldset`. The `legend` describes the group, in the same way a `label` describes the control. The `legend` provides a description for sighted users and screen reader users alike. 

In most screen readers, the `legend` is read out in combination with the radio button's `label`. “Delivery options, Standard (Free, 2-3 days)” (or similar). If we omited the `fieldset` and `legend` then screen reader users would only hear “Standard (Free, 2-3 days)” which is a ambiguous.

You may be tempted to group all fields this way. For example, the address form from earlier, could be wrapped inside a `fieldset` with a `legend` of “Address”. Whilst this is technically valid, it's unnecessary and verbose as the field labels make sense without a `legend`. Users don't need to hear “Address: Address Line 1” as it doesn't *add value*.

### Smart Defaults

The first radio button is selected by default thanks to the `checked` attribute. This has two benefits: first, this stops users from seeing an error negating the need for form validation. Second, there's less work for users to do.

As most users will select free delivery, we put that option first and select it for them. This way users can proceed effortlessly.

## 5. Delivery Notes

Imagine your at work. You get a notification to say your item is on its way to your home address for delivery. When you arrive home, instead of seeing the package, you see a delivery note saying “we couldn't deliver your package as it didn't fit through your letter box”. Frustrating.

A delivery note, which you can provide at your discretion, stops this from happening. The delivery note tells the delivery person what to do in the event that you're not home. Perhaps you'd prefer it to be left with a neighbour, or inside your recycle bin which Amazon[^] refers to as a “safe place”. This, by the way, works surprisingly well.

![Delivery notes](.)

```HTML
<div class="field">
  <label for="notes">
  	<span class="field-label">Delivery notes (optional)</span>
  	<span class="field-hint">Tell us what to do if you're not in. For example, *leave it with the next door neighbour*.</span>
  </label>
	<textarea id="notes" name="notes"></textarea>
</div>
```

This is the first time we've used a `textarea` control. It's remarkably similar to a text box (`input type="text"`) except that tit allows many lines of text. This is appropriate here because a delivery note could span multiple lines. (Remember from earlier, that the size of the field affords its requirements.)

Whilst this question *adds value* and justifies its existence in the checkout flow, we need to understand how it will be used from the delivery person's perspective. It may influence the design of the interface. In this case, the viewport on the device is small and can't be scrolled, so we'll need to limit the amount of text that can be typed.

### Limiting Text

Limiting the amount of a text a user can type can and should be handled by validation as set out in “A Registration Form”. But there are some additional considerations to discuss.

The `maxlength` attribute (which takes a number value) literally limits the amount of a text a user can type. So as soon as the limit is reached, the user's input will be ignored. The support for this attribute on the `textarea` control is both lacking and buggy[^8]. Even if it was well supported, I don't recommend using it, especially in this case.

This is because some users don't look at the screen as they type&mdash;they are focused solely on the keyboard. Where a user enters a lot of text, they'll look up to find half their entry has been truncated causing immense frustration.

### Character Countdown

Instead, we should let users type freely and offer feedback as to how many characters they have left. This way, users can see the feedback when they finally look up at the screen and edit their entry accordingly. If they don't notice the feedback, then an error will show when they submit the form, thanks to our validation routine.

![Character count](.)

To create this component, we need to use Javascript to inject a status box below the field. Then we need to listen to the textarea's `keydown` event.

```JS
function CharacterCountdown(input) {
  this.input = $(input);
  this.status = $('<div role="status" aria-live="polite" />');
  this.setOptions(options);
  this.updateStatus(this.options.maxLength);
  this.input.parent().append(this.status);
  this.input.on("keydown", $.proxy(this, 'onKeydown'));
};
```

As the user types the `keydown` event listener will fire. That method checks the `length` of the field against the configured max length. Then it will update the status box accordingly.

```javascript
CharacterCountdown.prototype.onFieldChange = function(e) {
	var remaining = this.options.maxLength - this.field.val().length;
  this.status.html(this.options.message.replace(/%count%/, remaining));
};

```

The status box has a `role` attribute set to `status` and `aria-live` set to `polite`. When the status box is updated, screen readers will announce it, but only after the user finishes typing. This way, users aren't rudely interupted. 


*(Note: both `role="status"` and `aria-live="polite"` are functionally equivalent, but older versions of JAWS don't support `role`.)*

## 6. Payment

It's hardly surprising that most transactions are abandoned at the payment page. Not only is this screen shown toward the end of the journey (where users have had the most time to reconsider their decision, for example), but they may have to stop and find their credit card.

There are a number of usability provisions we can apply here. By leveraging the browser's autocomplete routine, removing unnecessary fields, using the right input types and crafting the label (and hint) text&mdash;we can drastically reduce friction and keep users on-task.

### Removing Fields

There are a number of details on a credit or debit card: name on card, card number, valid from date, expiry date, issue number, security number; all of these are commonly found on payment forms. However, not all of these details are necessary to process a payment.

When I worked on Kidly's checkout flow, Øyvind Valland, Chief Technology Officer (CTO) at Kidly, carefully picked Stripe as the payment provider. This way, Kidly didnt't have to worry about PCI compliance and the cost of developing its own solution from scratch. Here's the payment form we ended up with:

![Payment](.)

You'll notice *valid from* date is missing so I spoke with Øyvind to find out why that was:

> We don't need to ask for Valid From. Only a handful of debit cards show those and it provides more hassle for the customer to enter, than benefit to us in verifying card details. That is, if the card is stolen, having to enter a valid from date isn't going to stop the thief.

He goes on to talk about the billing address. That is the address to which the card is registered:

> Only the numerics contained in card details are used for verification. That is, house number is used, but not street name. We ask ask for it for our own records. Being able to eyeball this stuff is handy in any situation where you have to query what's happened. Besides, some people expect that they'll have to provide an address (at least one which is used for both billing and shipping).

Øyvind is not a designer per se, but his input into the design process was crucial. Many of us assume that back end developers don't care about the user experience, but tapping into their knowledge is very valuable.

> Design is a team sport

Design is a team sport, and so we should treat it as one. By designing (and researching) with a diverse set of people, we'll normally end up producing a far better experience.

This also shows, that we should constantly be questioning the existence of form fields. If you look at other people's designs and assume something has to be a certain way, we'll never improve micro patterns such as these. Proving assumptions are correct or otherwise, is an essential weapon in a designer's arsenal.

### Autofill

Most modern browsers can automatically fill in form fields, by way of the `autocomplete` attribute. When the user focuses a particular field, the browser checks if it has that information stored&mdash;if it does, the user can select it without having to type. Additionally, since iOS 8, the Safari browser lets users scan their card using the iPhone's camera&mdash;it uses the same mechanism to automatically fill out those fields.

Not only does this drastically reduce the amount of effort to complete the form, but it also negates the chance of typos&mdash;two very helpful usability improvements on a form that has the highest drop off rates in ecommerce.

As mentioned earlier, autofill is enabled with the `autocomplete` attribute. Most modern browsers support, but for those that don't some older browers offer similar functionality by using the `name` attribute instead. For the widest support, you should specify the correct values for both attributes as shown below.

```HTML
<div class="field">
	<label for="ccname">
		<span class="field-label">Name on card</span>
	</label>
	<input type="text" id="ccname" name="ccname" autocomplete="cc-name">
</div>
<div class="field">
	<label for="cardnumber">
		<span class="field-label">Card number</span>
	</label>
	<input type="text" id="cardnumber" name="cardnumber" autocomplete="cc-number">
</div>
<div class="field">
	<label for="expdate">
		<span class="field-label">Expiry date</span>
	</label>
	<input type="text" id="expdate" name="expdate" autocomplete="cc-exp">
</div>
<div class="field">
	<label for="cvc">
		<span class="field-label">Security code</span>
	</label>
	<input type="number" id="cvc" name="cvc" autocomplete="cc-csc">
</div>
<div class="field">
  <fieldset>
  	<legend>
  		<span class="field-legend">Is your billing address the same as delivery?</span>
  	</legend>
  	<div class="field-checkbox">
  		<label for="things">
  			<input type="checkbox" name="things" value="" id="things" checked>
  			Yes, it's the same
  		</label>
  	</div>
  </fieldset>
</div>
```

*(Note: you can refer to the full list of available values in the HTML specification[^autofill])*

### Number Input

The number input (`input type="number"`) lets mobile users more easily type a number via a numeric keypad. On desktop the input will contain increment and decrement buttons called spinners which makes it easy to make small adjustments without having to select and type.

![Numeric keyboard](./images/numeric-keyboard.png)

You might then be thinking that the number input is appropriate for card number, expiry date and CVC number&mdash;after all, they are all made up of numbers. But it's far more complicated than that. By looking at what the spec says, what browsers do and what users want, we can more easily determine when the number input is appropriate.

Let's start with some definitions. Wikipedia says that:

> A number is a mathematical object used to count, measure, and label. [...] numerals are often used for labels (as with telephone numbers), for ordering (as with serial numbers), and for codes (as with ISBNs).

This is how most of us think of numbers. We use them to count and measure. But we also use numbers to represent dates and codes. But the HTML specification only agrees in part with this definition. It says that:

> `type=number` is not appropriate for input that happens to only consist of numbers but isn’t strictly speaking a number. If it doesn’t make much sense to use number with spinners, `type=text` is probably the right choice (possibly with a pattern attribute).

In other words, numbers and digits are different. Numbers are used to represent an amount of something such as:

- my age (announced “thirty four years old”)
- the price of an apple (announced “forty five pence”)
- the time it took me to cook breakfast (announced “ten mins”)

Conversly, digits are sometimes used to represent the other things such as:

- birth date (“nineteenth of June, eineteen eighty three”)
- pin code (“eight, double five, three, two, six”)

Pay close attention to the way the numbers and digits are announced. There's quite a difference. And now that we understand these differences, we can look at how some browsers treat number inputs and why we might perceive these treatments as bugs when they're not.

For example, IE11 and Chrome will ignore non-numeric input such as a letter or a slash. Some older versions of iOS will automatically convert “1000” to “1,000”. Safari 6 strips out leading zeros. Each of these examples seem undersirable, but none of them stop users from entering a real number easily.

Some numbers contain a decimal point such as a price; other numbers are negative which need a minus sign. Unfortunately, some browsers don't provide buttons for these symbols on the keypad. If that wasn't enough some desktop versions of Firefox will round up very large numbers.

In these cases, it's safer to use a regular text box to avoid excluding users unnecessarily. Remember users are still able to type numbers this way it's just that the buttons are a smaller. And to soften the blow a little bit, the numeric keyboard can be triggered for iOS users by using the pattern attribute as shown below.

```HTML
<input type="text" pattern="[0-9]*">
```

In short, only use a number input if:

- incrementing and decrementing makes sense
- the value doesn't start with a zero
- the value doesn't contain letters, slashes, minus signs and decimal points.
- the value isn't a very large number

Let's apply these rules to the expiry date. Incrementing doesn't make sense and the number could start with a zero. And many credit card put a slash in the expiry date which users should be able to copy. Using a number input is not only inappropriate, but it also creates a jarring user experience as the user types a slash which is ignored.

We'll look at appropriate use caes of the number input in the next chapter.

### Forgiving Bad Input

In “A Registration Form” we briefly talked about forgiving little input mistakes. In fact, the success of the Internet is largely down to its robustness, otherwise known as Postel's law which states:

> Be conservative in what you send; be liberal in what you accept.

We can apply this principle to the fields in the payment form. For example, a card number typically appears as 16 digits split into 4 parts by a space. Some users may type the space; others may not. Similarly, for the expiry date some users may type a slash, others may leave it out. 

Whether it's a slash or a space, or a card number or an expirty date, we should be forgiving wherever possible. 

### Card Verification Code (CVC) Field

Every payment provider needs the user's CVC number. It's usually the last 3 digits found on the back of the card.

The first problem is that it's not always referred to as CVC number. Sometimes it's referred to as a security code number or card verficiation value (CVV). Being specified as an acronym doesn't help matters either. And to top it off, the number is never beside a label to demarcate it making the field rather ambiguous.

To fix this, we should emply the hint text pattern to tell users exactly what it is and where to find it. For example, “This is the last 3 digits on the back of the card”.

### Billing Address

The billing address is actually the address to which the card is registered and is needed to process a payment. For most users, their billing address is the same as the delivery address. As the user has already provided this information, we can use it to improve the experience.

First we need to add an extra field, this time a checkbox. The checkbox asks the user if their billing addresss is the same as their delivery address. This way users only have to fill out the billing address on the rare occasion that it's different. As it's the most common scenario, it's checked by default. 

With Javascript we can enhance the experience even more by hiding the billing address until the user unchecks the checkbox. This is a form of progressive disclosure which reduces noise in the interface. That is, we only show the fields when they are relevant to the user.

To do this, we need to listen to the checkbox click event.

```JS
function CheckboxCollapser(checkbox, element) {
  this.checkbox = checkbox;
  this.element = $(element);
  $(this.checkbox).on('click', $.proxy(this, 'onCheckboxClick'));
  this.check();
};
```

When the checkbox is clicked, we need to check whether it's checked or not. If it is then we need to hide the billing address. If it's not we need to show it.

```JS
CheckboxCollapser.prototype.onCheckboxClick = function(e) {
  this.check();
};

CheckboxCollapser.prototype.check = function() {
  if(this.checkbox.checked) {
    this.element.hide();
    this.element.attr('aria-hidden', 'true');
  } else {
    this.element.show();
    this.element.attr('aria-hidden', 'false');
  }
};
```

Note that we hide the element with CSS (by toggling the display property between `block` and `none`). But, we also ensure that screen readers are informed by toggling the `aria-hidden` attribute between `true` and `false`.

## 7. Review Page

At this point in the flow, we have collected all the information we need from users to complete their order. But instead of processing the order after payment, it's wise to let users review their order on another page first. As counterintuitive as this may sound, adding an extra step in the flow actually reduces friction.

Take Mary (I made her up). She's a mother of two young children. It's late, the baby is up, crying inconsolably. Naturally, Mary is tired and stressed. To make things worse, she's ran out of nappies.

She grabs her phone, adds them to her basket, fills out all the checkout details and submits the order. Great, except it isn't. She ordered the wrong size nappies and the she used the wrong card. Even the most sophisticated validation pattern cannot eradicate human error. Even if everything *looks* right and is formatted correctly, there could still be mistakes.

Instead, we can save Mary a lot of frustration by giving her the chance to review her order on a separate page. That way she can focus on the order details. Remember, filling out forms and checking information are two separate mental contexts.

This also happens to save your (client's) business time and money. If Mary wants to cancel the order, then handling calls and processing returns might be rather costly&mdash;especially if the business offers free returns.

This goes to show that solely using completion time as a metric for success is dangerous. Completion time is important, but in conjunction with checking how accurate people's orders are.

### Visual Design

Every piece of information gathered through the checkout flow should be presented on the review page. User's shouldn't have to go back to check any information, otherwise it would defeat the purpose of this page. Users only need to go back to a previous page, if they spot a mistake on this page.

![](.)

Users can click *edit* to make amendments which is another advantage of using One Thing Per Page. As pages are small they will load fast; as each page is dedicated to that one thing, making amendments is an easy task with a maximum signal to noise ratio.

![Flow diagram: Click edit, make change, go back to summary](.)

When the user makes a change, they are taken back to this page again for a final review which puts users firmly in control and is something that should reduce stress and anxiety.

## 8. Confirmation Page

Confirming pages are more important than you might think. It's the first screen a user sees after purchase, so neglecting the user experience here is a sure-fire way to lose out on return business.

We've all experienced this before. If you want to take out some insurance, you call the free sales number and are quickly put through to a helpful agent. Parting with money is one of the easiest things to do in this world. But then, when you need to call up to make a claim, this is usually very painful. Calls aren't free and it takes a while to get through. All very stressful.

We can make the exact same mistakes digitally. Confirmation pages aren't just for confirming orders at the end of a transaction. They are an opportunity to start forging a long term relationship by giving users a better experience.

You probably want to tell users what happens next, such as when delivery should take place. Then you might want to consider telling users what to do if something goes wrong, or telling users how to cancel an order.

This is also a good time to ask users to sign up (if they arrrive here as a guest). As we have most of their information, all users will need to do is provide a password which drastically reduces friction. If the service up to now was good, users may need little reason to do so. Perhaps you'll offer them a faster checkout experience next time. Or you could encourage them to make another purchase by offering a discount on their next order to say thank you.

You might even encourage users to tweet about their purchase in order to receive a gift voucher. It all just depends on the specific service and product you're designing.

We've already talked about the importance of using plain and simple language for labels, hints and errors, but on the confirmation page it's an opportunity to put a bit of personality into it. For example, when you finish sending an email campaign with Mailchimp, they show a high-five image with the text “High five” which brings their branding through.

![Mailchimp high five](.)

Finally, badgering users before a sale with popups that ask for feedback is a no-no, but on the confirmation page, it's a yes-yes. If the experience was really good, some users will be delighted to give feedback so be sure to offer users an easy way to do that.

Here's a checklist of things to include in your confirmation screen:

- a reference number
- what happens next and when
- what to do if something goes wrong
- ask users to sign up in return for something
- ask users to spread the word in return for something
- contact details
- links to further information if they might be useful
- a link to your feedback page

## Guest Checkout

As Jared Spool explains in The 300 Million Dollar Button[^13], forcing users to register, before being able to checkout is just about the worst thing we can do.

Earlier, on the confirmation page we told users that if they signed up they would have a faster checkout experience next time around. This is because we would have most of their details on record. But the first time they place an order, we don't have the details.

This creates an unnecessary burden on the users up front for no gain to the user. Where is the user need? Using the Question Protocol forces us to ask ourselves why. The answer to this question is invariably uncompelling.

By all means ask users to register, by giving genuine value, but do so at the end of a journey. The journey itself should build trust if we design it well.

## Progress Bars

Traditional advice says we should give users knowledge of their progress within a particular process. But this is not necessarily the case. There is little data that shows including one is of much value.

![Progress indicator](./images/Progress_indicators_2.png)

GDS say that they:

- are often not noticed
- take up lots of space
- don’t scale well on small screens
- can distract and confuse some people
- make it hard to write good labels
- make it hard to handle conditional sections

It's better to start without one, and test to see what problems bare out. It's easier and cheaper to add features later than it is to remove them after the fact.

Not having one makes for a cleaner, lighter and more focussed page which should makes things easier for the user. Having meticulously designed every detail the checkout should hopefully be so fast that there is no reason to wonder *how much is left*.

If research shows you need one, consider a simple version first:

![Local Image](./images/Progress_indicators_1.png)

This progress indicator may well be enough and doesn't suffer from many of the challenges and problems of a more verbose, traditional indicator.

In later chapters, we'll look at how to solve a really really long form by showing progress.

## Smart Defaults

We can speed things up drastically for those ordering for a second time. This is because we have their information on record. This means we can bypass all the steps by sending the user directly to the *Check and confirm* page for a final review. For most users, this means reducing interaction points to one.

As we collect information, we don't have to ask for it again and can send users to the most advanced step at any time reducing friction and increasing conversion.

## Order summary

How it might look:

![Order summary panel](./images/?.png)

Picture the experience of shopping in a real world physical shop. No computer in-sight. You walk in, you pick items to buy and place them in your basket. When you decide to go to the till, you can see everything you're buying, even as the assistant scans each item.

To mimic this behaviour, we provide a view of the order and all the details of the order throughout the checkout flow. This builds trust, keeps users in control and upholds the forward momentum.

Not including a summary is akin to dumping a basket of products into a black hole, and the assistant then telling you how much to pay. This is a burden on the user as they are forced to remember what they picked up. This causes anxiety and eventual drop out.

On small screens we put the order summary below the form. On bigger screens we have an opportunity to place it beside the form:

![Order summary panel](./images/?.png)

## Checkout Header

Many checkouts have a specific header that is different to the norm. This is because it reduces the noise, and softly encourages users to complete the process. The suggestion here is to keep the site's logo for familiarity purposes and allow uses to click that to exit the flow.

Also tell users they are in the checkout flow. This is what we did for Kidly:

![Order summary panel](./images/?.png)

## Summary

In this chapter we used One Thing Per Page to reduce the cognitive burden of filling out a relatively long form. We then made our way through a host of smaller details which collectively contribute to a friction-free and respectful experience. Here are the main takeaways:

- Ask questions in a sensible order.
- The width of the field should match the type of input.
- Use `fieldset` and `legend` elements to give checkboxes and radio buttons context for the choices within the group.
- Leverage autocomplete to speed up the form filling process.
- Adding extra questions is not always a bad thing. Time to completion is not the only metric.
- Allow users to check their answers before final submission.
- Use a confirmation page to give users important information and to continue the relationship that has just begun.
- Forcing users to register before checkout is one of the worst things we can do.
- Progress bars don't always improve the experience and may clutter the interface unnecessarily.
- Use smart defaults and stored data to drastically reduce effort and increase conversion.

## Footnotes

[^1]: https://designnotes.blog.gov.uk/2015/07/03/one-thing-per-page/
[^2]: https://www.smashingmagazine.com/2017/05/better-form-design-one-thing-per-page/
[^3]: http://www.formsthatwork.com/
[^4]: https://www.youtube.com/watch?v=tzfHlEFd2Fk
[^5]: http://baymard.com/blog/form-field-usability-matching-user-expectations
[^6]: http://www.pcapredict.com/en-gb/address-capture-software/
[^7]: https://www.smashingmagazine.com/inclusive-design-patterns/
[^8]: http://caniuse.com/#feat=maxlength
[^9]: https://www.smashingmagazine.com/2017/03/improve-billing-form-ux/
[^10]: https://stripe.com/gb
[^11]: https://nordnet.design/design-is-a-team-sport-231a602fc072
[^12]: https://www.gov.uk/service-manual/design/confirmation-pageswestern-web-part-1/
[^13]: https://articles.uie.com/three_hund_million_button/
[^autofillattrs]: https://html.spec.whatwg.org/multipage/form-control-infrastructure.html#autofilling-form-controls:-the-autocomplete-attribute

## Todo

- Back buttons (https://paper.dropbox.com/doc/Back-buttons-and-links-d9DoNzPysaoTFZpJfqGsY)

