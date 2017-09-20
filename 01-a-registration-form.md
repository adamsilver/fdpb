# A Registration Form

We're going to start with a registration form. Many companies want to have long-term relationships with their users. To do that they need users to sign up. And to do *that*, they need to give users value for doing so. Nobody wants to actually sign up. They just want access to your social network, or the tools you offer, or more simply a faster checkout experience.

Whatever it is, a registration form is often the first form a user sees. Despite it's basic appearance, there are many things to consider: the primitive elements that make up a form (labels, buttons and inputs), how to reduce friction (even on small forms like this) all the way through to form validation.

In choosing such a simple form, we can zoom in on the foundational qualities found in well-designed forms.

## How it might look

![Registration Form](.)

Here's the HTML:

```html
<form>
  <label for="firstName">First name</label>
  <input type="text" id="firstName" name="firstName">
  <label for="lastName">Last name</label>
  <input type="text" id="lastName" name="lastName">
  <label for="email">Email address</label>
  <input type="email" id="email" name="email">
  <label for="password">Create password</label>
  <input type="password" id="password" name="password" placeholder="Must contain 8+ characters with at least 1 number and 1 uppercase letter.">
  <input type="submit" value="Register">
</form>
```

This form is made up 4 fields and a submit button. Each field is made up a of a control (the input) and its associated label. Labels are where our discussion begins.

## Labels

Every form control that accepts user input needs an auxiliary `<label>`. The submit button (with `type="submit"`) doesn't need a label because it's `value` attribute acts as one and doesn't take input from the user.

The label describes the form control which is important because without one, sighted users won't understand what they need to type. Visually-impaired people may use screen readers. When a screen reader encounters an input without a label, it can only announce ‘text box’ which is meaningless. Also, browsers will set focus to the input, when clicking its label. This increases the size of the hit area, which is especially useful for motor-impaired users.

![Hit area](.)

To *connect* an input to the label, the input's `id` and label's `for` attribute value must match and be unique to the page. In the case of the email field, the value is “email”:

```html
<label for="email">Email address</label>
<input id="email" name="email" type="email">
```

Failing to include a label means ignoring the needs of (motor and visually-impaired) users. Seeing as we're designing for people, we can use their ability (or lack thereof) as constraints that guide is to design robust experiences. After all, what helps some users often helps others, as is the case with the larger hit area. Even for those with fine motor control, a larger hit area is naturally easier to hit.

## Placeholders

The `placeholder` attribute is intended to store a hint. It gives users extra guidance when filling out a field&mdash;particularly useful for fields that have complex rules such as a password field (more on this shortly). As they are not real values, browsers give them a ‘grayed out’ aesthetic so that they can be differentiated from user-entered values.

![Placeholder example from above](.)

Hints, unlike labels, are optional and shouldn't be used as a matter of course. Just because the placeholder attribute exists doesn't mean we have to use it. For example, you don't need a placeholder of ‘Enter your first name’ as a hint when the label is ‘First name’&mdash;that's needless duplication.

![Placeholder and label with same value](.)

Placeholders appeal to designers because of their minimal, space-saving aesthetic. This is because placeholder text is placed *inside* the field. But, there are many with using the placeholder attribute as a way to show a hint.

First, they disappear when the user types. Disappearing text is naturally hard to remember which could cause errors if for example the user forgets to satisify one of the password rules. Users often mistake placeholder text for a value[^], causing them to skip the field, which again, would cause errors. Gray-on-white text lacks sufficient contrast making it generally hard-to-read[^]. And to top it off, some browsers don't support them, some screen readers don't announce them and long hint text may get cut off.

That's a lot of problems for something that is essentially just text. All content, especially a form hint, shouldn't be considered a ‘nice to have’. So instead of using placeholders, it's better to put hint text inside the label and outside the control:

![Hint pattern](.)

```HTML
<div class="field">
  <label for="password">
    <span class="field-label">Password</span>
    <span class="field-hint">Must contain 8+ characters with at least 1 number and 1 uppercase letter.</span>
  </label>
  <input type="password" id="password" name="password">
</div>
```

The hint is actually placed inside a separate `<span>` so that it can be styled differently. By placing it inside the label it receives all the same benefits. It's read out by screen readers, and this way it actually increases the hit area even more.

This is not the only way to achieve this design. We could use Accessible Rich Internet Application (ARIA) attributes to associate a completely different element with an input:

```HTML
<div class="field">
  <label for="password">Password</label>
  <p class="field-hint" id="passwordhint">Must contain 8+ characters with at least 1 number and 1 uppercase letter.</p>
  <input type="password" id="password" name="password" aria-describedby="passwordhint">
</div>
```

Here, the `aria-describedby` attribute is used to connect the hint via its `id`. Whilst this has visual and audible parity with a standard label, this solution is not the same. Clicking the `<p>` will not set focus to the control, reducing the hit area. Also despite ARIA's ever-improving support, it's never going to be as well supported as native elements.

This is why the first rule of ARIA is not to use ARIA. The specification states:

> If you can use a native HTML element or attribute with the semantics and behaviour you require already built in, instead of re-purposing an element and adding an ARIA role, state or property to make it accessible, then do so.

## Float Labels

The float label[^] pattern by Matt Smith is a technique that uses the label as a placeholder. The label starts *inside* the control, but floats above the control as the user types, hence its name. This technique is often lauded for it's quirky, minimalist and space saving qualities.

![Float label](.)

Unfortunately, there are several pitfalls with this approach. First, there is no space for a hint because the label and hint are one and the same. Second, they're hard-to-read due to their (affordance giving) poor contrast and small text as they're typically designed. Like placeholders, they may be mistaken for a value and could get cropped.  

And, float labels don't actually save space. That's because the label needs space to move into in the first place. Even if they did save space, that's hardly a good reason to diminish usability.

Quirky and minimalist interfaces don't make users feel awesome. Obvious, inclusive and robust interfaces do. Artificially reducing the height of forms like this is both uncompelling and problematic.

Instead, you should priortise making room for an ever-present, readily available label (and hint) at the start of the design process. This way you won't produce a form that drive users crazy.

We'll be discussing several, less artificial techniques to reduce the size of forms shortly.

## The Question Protocol

One powerful and *natural* way to reduce the size of a form is to use The Question Protocol[^] by the Government Digital Service. It urges you to determine what information is needed and why. Only then can you justify the existence of each question (or form field).

Does the registration form need to collect first name, last name, email address and password? Are there better or alternative ways to ask for this information that simplifies the experience?

In all likeliness you don't need to ask for the user's first and last name just to register. If you need that information later, for whatever reason, then ask for it then. By removing these fields, we've reduced the size of the form by 50%&mdash;without resorting to novel and problematic patterns.

### No Password Sign In

One way to avoid asking users for a password is to use the No Password Sign In pattern. It works be leveraging the security of email (that already has a password). Users enter just their email address, and the service sends a special login link to their inbox. Clicking it, logs the user into the service immediately.

![Medium No Password Sign In](.)

Not only does this reduce the size of the form to just one field, but it also saves users having to remember another password. But whilst this certainly reduces friction on the form in isolation, it does add some of its own.

First, users might be less familiar with this approach, and many people are worried about online security. Second, having to move away from the app to your email account is long winded, especially for users who know their password (or use a password manager).

It's not that one technique is always better than the other. It's that the Question Protocol urges us to think about this as a matter of course. Otherwise, you'd mindlessly add a password field on the form and be done with it.

### Passphrases

Passwords are generally short, hard to remember and easy to crack. Users often have to create a password with at least eight characters made up of at least one uppercase and lowercase latter as well as a number. This micro interaction is hardly ideal.

Instead of a password, we could ask users for a passphrase[^5]. A passphrase is a series of words such as ‘monkeysinmygarden’ (sorry, that's the first thing that came to mind). They are generally easier to remember than password and they are more secure due to their length (they must be 16 characters or more).

The downside is that they are unfamiliar, which may cause anxiety for users, that are already worried about their online security.

Whether it's the No Password Sign In pattern or passphrases, we should only move away from convention once we've conducted thorough and diverse user research. You don't want to exchange one set of problems for another.

## Field styles

The way you style your form components will, at least in part, be determined by your product or company's brand. Still, label position and focus styles are important considerations.

Broadly speaking, you should place the label above the form control. In “Label Placement In Forms”[^6], Matteo Penzo shows this is best. But, there are more practical reasons for doing so. On small viewports there's no room beside the control. And on large viewports, zooming increases the chance of the text disappearing off screen[^x1].

Also, some labels contain a lot of text which causes it to wrap onto multiple lines which would disrupt the visual rhythm if placed next to the control. Whilst you should aim to keep labels terse, it's not always possible. So, using a pattern that accomodates varying content&mdash;by positioning labels above the control&mdash;is a sensible strategy.

Focus styles are a simpler prospect. Browsers put an outline around the in-focus element by default so that users, especiallyt hose who use a keyboard, know where they are. You might be tempted to remove this styling for aesthetic reasons but that's hardly a good reason to diminish usability. If you wish to remove it, then make sure you replace it with something more preferable.

![Focus styles, where am i, oh there I am](.)

## The email field

![The email field](.)

```HTML
<div class="field">
  <label for="email">
    <span class="field-label">Email address</span>
  </label>
  <input type="email" id="email" name="email">
</div>
```

Despite it's simple construction there are some important details that have gone into the field's design. As noted earlier, some fields have a hint in addition to the label which is why it's contained in a child `<span>`. It's given a class of `field-label` so that it can be targetted reliably with CSS.

The label itself is “Email address” and is using sentence case. As John Saito explains in “Making a Case of Letter Case”[^7], sentence case (as opposed to title case) is generally easier to read, friendlier and makes it easier to spot nouns. Whether you take heed of this advice is up to you, but whatever you pick, be sure to use that style consistently.

The input's `type` attribute is set to `email` which triggers an email-specific on-screen keyboard on mobile devices. Specifically, it gives users easy access to the ‘@’ and ‘.’ symbols which every email address must contain.

![Email keyboard](.)

People using a non-supporting browser will see a standard text input (`input type="text"`). This is a form of Progressive Enhancement which is a cornerstone of designing inclusive experiences for the web.

TODO: Do I do an aside here about Progressive Enhancement?

## The password field

![The password field](.)

```HTML
<div class="field">
  <label for="password">
    <span class="field-label">Choose password</span>
    <span class="field-hint">Must contain 8+ characters with at least 1 number and 1 uppercase letter.</span>
  </label>
  <input type="password" id="password" name="password">
</div>
```

We're using almost identical mark-up to the email field discussed just now. This way we can ensure all fields are styled consistently throughout the same application which speaks to principle 3, *Be consistent*.

The field contains a hint, because without one, the password requirements are unknown which is likely to cause an error once the user tries to proceed.

The input's `type` attribute is set to `password"` which masks the input's value by replacing what the user types with small black circles. This is a security measure that guards against “over the shoulder” attacks.

### A Password Reveal

Obscuring the value as the user types makes it hard to fix typos. So when one is made, it's often easier to delete the whole entry and start over. This is frustrating as most users aren't using a computer with a person standing behind them.

Due to the increased risk of typos, some registration forms include an additional ‘confirm password’ field. This is a precautionary measure that requires the user to type the same password twice doubling the effort, which is unfortunate.

Instead, it's generally better to let users reveal their password which speaks to principle 5, *Offer choice*. This way users get the security of the mask without increasing the risk of typos.

![Password reveal](.)

To do this, you'll need to use Javascript to inject a button next to the input. When clicked it toggles the `type` between `password` and `text` and switches the text from “Show password” to “Hide password”.

```JS
function PasswordReveal(el) {
    this.el = el;
    this.createButton();
};

PasswordReveal.prototype.createButton = function() {
    this.button = $('<button type="button">'+this.showText+'</button>');
    $(this.el).parent().append(this.button);
    this.button.on('click', $.proxy(this, 'onButtonClicked'));
};

PasswordReveal.prototype.onButtonClicked = function(e) {
    if(this.el.type === 'password') {
        this.hidePassword();
    } else {
        this.showPassword();
    }
};

PasswordReveal.prototype.showPassword = function() {
    this.el.type = 'text';
    this.button.text('Hide password');
};

PasswordReveal.prototype.hidePassword = function() {
    this.el.type = 'password';
    this.button.text('Show password');
};
```

Some browsers provide this functionality natively, so if you're providing your own solution you can supress the browser's implementation as follows:

```css
input[type=password]::-ms-reveal {
  display: none;
}
```

## Button styles

The first thing to know about buttons is that they aren't links. Links are typically underlined or specially placed within the page (navigation bar) so that they are visually identifiable amongst standard text. When hovering a link, the cursor will change to a hand. This is because, unlike buttons, links have weak affordance[^8].

In “Resilient Web Design”[^9], Jeremy Keith discusses the idea of material honesty. He says that *one material should not be used as a substitute for another. Otherwise the end result is deceptive.* Making a link look like a button is materially dishonest. It tells users that links and buttons are the same when they’re not.

Links can do things buttons can't do. For example, links can be opened in a new tab or bookmarked for later. Therefore buttons shouldn't look like links, nor should they have a hand cursor. Instead we should make buttons look like buttons which have naturally strong affordance. Whether they have rounded corners, drop shadows, borders is up to you, but they should look like buttons regardless.

Buttons should still give mouse users feedback on hover by changing the background colour for example.

### Placement

Submit buttons are typically placed at the foot of the form. This is because with most forms, users fill out the fields from top to bottom and then submit. But should the button be aligned left, right or center? To answer this question, we need to think about where users will naturally look for it.

Field labels and form controls are aligned left and run from top to bottom. Users are going to look for the next field below the last one. Naturally then, the submit button should also be positioned in that location: to the left and directly below the last form field. Eye tracking tests prove this to be so. Again, this helps users who zoom in&mdash;a right aligned button could disappear off screen more easily.

### Text

The button's text is just as, if not more important than its styling. The text should explicitly describe the action being taken. And because it's an action, it should be a verb. We should aim to use as fewer words as possible because less words are quicker to read. But we shouldn't remove words at the cost of clarity.

The exact words can match your brand's tone of voice, but don't exchange clarity for quirkiness. Using simple and plain language is easy to understand for everyone. The exact words will depend on the type of service. In this case “Register” or the slighlty softer “Sign up” is fine.

## Validation

Despite our efforts to create an inclusive, simple and friction-free registration experience, we can't eliminate human error. People make mistakes and when they do, we should make remedying them as easy as possible. 

When it comes to form validaton, there are a number of important details to consider. Choosing when to give feedback, through to how to display that feedback, down to the formulation of a good error message&mdash;all of these things need to be taken into account.

### HTML5 Validation

HTML5 validation has been around for a long time now. With a sprinkling of just a few HTML attributes, supporting browsers will mark fields with error messages when the user submits. Normally I would recommend using functionality that the browser provides for free, because it's often performant, robust and accessible by default. Not to mention, that it becomes convention as more and more sites use it.

Whilst browser support is quite good, many browsers, particularly on iOS and Android, have patchy support [^]. To simultaneously avoid these issues and grant oursleves the chance to design for a number of provisions, we'll provide our own solution. In this case, we need to turn off HTML5 validation by adding the `novalidate` boolean attribute.

```HTML
<form novalidate>
```

When the user submits the form, we need to check if there are errors. If there are, we need to prevent the form from submitting the details to the server for processing, which happens as standard.

```JS
function FormValidator(form) {
  form.on('submit', $.proxy(this, 'onSubmit'));
}

FormValidator.prototype.onSubmit = function(e) {
  if(!this.validate()) {
    e.preventDefault();
    // show errors
  }
};
```

*(Note: we are listening to the form's submit event, not the button's click event. The latter will stop users being able to submit the form by pressing <kbd>enter</kbd> when focus is within one of the fields. This is also known as implicit form submission.)*

### Displaying Feedback

It's all very well detecting the presence of errors, but at the moment our user's are none the wiser. There are three disparate parts of the interface that need to be updated. We'll talk about each of those in now.

#### Document Title

The document's `<title>` is the first part of a web page to be read out by screen readers. As such, we can use it to inform users that something has gone wrong with their submission. This is especially useful when errors are caught on the server.

Even though we're enhancing the user experience by catching errors on the client with Javascript, not all errors can be caught this way. For example, checking that an email address hasn't already been taken can only be checked on the server. In any case, Javascript is prone to failure so we can't solely rely on it's availability. 

Where the original page title might read “Register for [service]”, onerror it should read “Retry - Register for [service]” (or similar). Whether it's “Retry - ” or “There's errors - ” is somewhat down to opinion. But, on a recent service we chose the former, because the service is used frequently, and screen reader users appreciated the terser option.

Updating the title with Javascript is as follows:

```JS
document.title = “Retry - ” + document.title;
```

Admittedly this is probably not as crucial for sighted users, but those who multi-task with multiple browser tabs open, the prefixed text may serve as a notification of sorts.

#### Error Summary

The error summary not only needs to tell users that something has gone wrong, but what they can do to remedy the situation quickly. Due to it's importance, we place it prominently at the top of the page. This ensures sighted users don't have to scroll after a page refresh to see it, nor will screen reader users have to wade through as much of the document to hear it. Conventionally speaking we should style the errors in red, but we don't want to rely on colour alone, because this would exclude colour blind users. This is why it's inset with a border.

![Error summary](.)

The panel consists of a heading to summarise the fact there is a problem. Beneath that, there is a list of errors, which are represented as links. Clicking one of those links, will set focus to the erroneous field thanks to the `href` value matching the form control's `id` value.

```HTML
<div class="errorSummary" role="alert">
  <h2 tabindex="-1">There's a problem</h2>
  <ul>
    <li><a href="#emailaddress">Enter an email address</a></li>
    <li><a href="#password">The password must contain an uppercase letter</a></li>
  </ul>
</div>
```

The `div` has a role of `alert` which tells supporting screen readers to announce the region immediately. The heading's `tabindex` attribute is set to `-1`, which allows us to programmatically set focus to it using Javascript. This in turn ensures the error summary panel is brought into view. Without this functionality the interface would appear unresponsive.

```JS
FormValidator.prototype.focusSummary = function() {
  this.summary.focus();
};
```

When there aren't any errors, the summary panel should be hidden. This ensures that there is only ever one summary panel on the page. And, that it appears consistently in the same location whether errors are rendered by the client or the server. To hide the panel we need to add a `hidden` attribute to the containing element.

```HTML
<div class="errorSummary" hidden></div>
```

Some browsers don't support the `hidden` attribute, so we need to add the following rule to the stylesheet:

```CSS
[hidden] {
  display: none;
}
```

#### 3. In-context Errors

We also need to put the relevant error message beside the field. This saves users from having to repeatedly check the summary panel at the top of the page which is likely to include scrolling up and down.

![In-context Errors](.)

Once again, the messages are coloured red by convention and colour blind users are catered for thanks to the inset border on the left hand side. The message is placed above the field and inside the label. Placing it above the field reduces the chance of the message being obscured either by the browser's autocomplete suggestions or by mobile on-screen keyboards which show on focus.

```html
<div class="field">
  <label for="blah">
    <span class="field-label">Email address</span>
    <span class="field-error">Enter an email address</span>
  </label>
</div>
```

Like the hint pattern mentioned earlier, the error message is also injected inside the label. This is ensures that when the field is focused, screen reader users will hear the message in context.

```JS
Injected the message into the label
```

*Note: the registration form only consists of text inputs. In the next chapter we'll look at how to inject errors accessibly for other types of fields such as radio buttons.*

---

### Inline Validation

The first type of instant feedback is inline validation. It works by giving users feedback as they type or as they leave the field (`onblur`). In theory, it's easier to fix errors as soon as they occur and avoids users seeing a large amount of error messages at once. Whilst this makes some sense, inline validation poses several problems.

For entries that require a certain number of characters, the first keystroke will always constitute an invalid entry. This means users will be interrupted causing them to switch mental contexts: entering information and fixing it.

![Instant error](.)

Alternatively, we could wait until the user enters enough characters before showing an error. But this means the only way a user gets feedback is after they have provided the field successfully which is somewhat pointless.

We could wait until the user leaves the field (`onblur`), but this is too late as the user has mentally prepared for (and often started to type in) the next field. Some users switch windows or use a password manager when using a form. Doing so will trigger the blur event and causing an error to show before the user is finished. All very frustrating.

In any case, many users have their eyes fixated on the keyboard as they type, so they may not notice the feedback at all.

If users are making lots of errors, there's probably something fundamentally wrong elsewhere. Focus on shortening the form and provide clear labelling and hint text. This way users shouldn't see more than the odd error. We'll deal with longer forms in upcoming chapters.

### Disabling Submission Until Valid

Another form of instant feedback is to disable the submit button until the form is valid. This feedback is rather ambiguous and suffers from two crucial problems. 

First, disabled buttons are afforded by being ‘grayed out’ making the text hard-to-read. Second, if there is an error, the user won't know why. Instead they're left to guess which (set of) field(s) require their attention.d

### Checklist Affirmation Pattern

The last type of instant feedback is what I call the Checklist Affirmation pattern. Like inline validation, it gives feedback as the user types, but instead of showing *errors* it marks each rule as correct.

![Mailchimp example](.)

This is less invasive than inline validation but it still suffers from several problems. First, it can only check the format. That is, what appears to be successful may turn out to be a problem once submitted to the server.

It also creates an inconsistent experience because some fields aren't appropriate to employ this technique. For example, you wouldn't use this technique on an email address whereby the user is ticking off it's required state and valid formatting for an email address.

Also, the feedback will be obscured on mobile by the on-screen keyboard which may cause the user to stop what they're doing and hide the keyboard to see what's going on.

In the end, like inline validation, this is a distracting experience that takes control away from the user which fails principle 4, *Give control*.

### On Submit

To *give control* and avoid problems with instant feedback, we should validate forms on submit. This way users can focus on filling out the form without being interupted. If the form is short and has well written guidance, then this won't take long. Then, and only when ready, the user can explicitly submit the form at which time the user would expect to receive feedback&mdash;positive or otherwise.

Earlier, I said that we should use people's abilities, disabilities and preferences as constraints to guide us to design better experiences. But they aren't the only constraints. The platform, in this case the Web, also provides a set of constraints which turn into convention.

Validating a form on submit is convention. It's just the way forms have always worked on the Web. Fortunately, conventional interfaces are familiar. And familiar interfaces generally require less cognitive effort. 

In any case, the form needs to be submitted to the server for processing. You can't check, for example, if the user's email address has not already been used to create an account, without hitting the server. So by validating `onsubmit`, the users gets a similar experience regardless.

### Submitting for a second time

If the user does cause errors, then when the user next submits the form, we need to clear the errors before performing the same routine again. Otherwise, the errors will remain.

```JS
clear error message code
```

Creating an instance of the FormValidator is as follows:

```JS
var validator = new FormValidator(form);
```

Then create a validator for each field that needs validating. Here's the email field validator:

```JS
validator.addValidator('email', [{
  method: function(field) {
      return field.value.trim().length > 0;
  },
  message: 'Enter your email address.'
},{
  method: function(field) {
      return (field.value.indexOf('@') > -1);
    },
  message: 'Enter the ‘at’ symbol in the email address.'
}]);
```

Notes:

- Each validator takes the field name as the first parameter and an array of rules as the second.
- Rules are made up of two properties: method and message.
- The message is the error message, which it uses to populate the summary and in context messages.
- The method has a field parameter and we can interrogate the field to test that it passes some logic. If it passes the method should return true, otherwise false.
- It's up to each method to be as forgiving as possible. The email validator, for example, trims the value before checking length.

### How to write errors

Up to now, we've ensured that our approach to validation is robust and inclusive. But this counts for nothing if we were then to neglect the messages themselves. One study showed that *custom* error messages increased conversion by 0.5%, equating to over £250,000 a year in revenue[^15].

> Content is the user experience

A good error message is easy to understand. Whilst it's often backwards to design an interface without first knowing the content, in this case, it's hard to design the messages without understanding the interface.

As we've just designed the interface, we already know that errors  appear in 2 places: the summary and next to the field. This means we need to ensure the content works in context of both locations. That is, ‘Enter an “at” symbol.’ is ambiguous inside the summary, but is perfectly acceptable, better even, next to the field (where the label provides the context.)

[]()

Maintaining 2 versions of the same message is a hard sell for small gain. Instead, we'll design the content to work in both: ‘Your email address needs an “at” symbol.’.

We also need to consider pleasantries. Putting ‘please’ at the start of each message seems noisy and repetitive. But some errors sound blunt without it. For example, ‘Please answer this question’ versus ‘Answer this question’. ‘You need to [answer this question.]’ may be better as it sounds softer but it has more words. Tricky.

We might consider how frequently the system is being used by the same user. For users who use a system every day, removing ‘You need to’ gets straight to the point for someone who has an intimate relationship with a system. Though, it may come across as rude for those using the system infrequently. Without testing it's hard to know.

Regardless of the chosen approach, there's bound to be some repetition. Often when testing validation, we'll submit the form without entering anything which exposes the repetition in the messages in all their glory:

[]()

This, presents a long list of errors. Through this, somewhat artificial scenario, the repetition of words looks like a glaring oversight. As content designers this could freak us out. But how often do users submit a long form without entering a single field? Most users aren't trying to break the interface.

Here's some more practical considerations:

- Use punctuation. Some errors have clauses and contain several sentences.
- Be specific. If the system knows exactly why something went wrong say so. ‘The email is invalid.’ is ambiguous and puts the burden on the user. ‘The email needs an “at” symbol’ is explicit and requires less thinking.
- Use the active voice. For example, ‘Enter your name’ not ‘Your name must have an entry’.
- Don't blame the user.
- Use plain language. Error messages are not an opportunity to promote your quirky brand's tone of voice. Keep it simple and obvious.
- Be human, avoid jargon. Avoid avoid words like *invalid*, *unrecognised* and *mandatory*.
- Makes sense on its own.
- Be terse. Don't be overly chatty, particularly on a frequently used system.
- Be consistent. Use the same tone, the same words and the same punctuation.
- Test your messages with users.

### Be forgiving and restore values

The best problems are the ones users don't have to even know about. We can forgive users for typing extra spaces. If they type ‘John ’ we can trim it to ‘John’. We can also ignore upper or lowercase letters depending on the field.

Jared Spool makes a joke about this in Design is Metrically Opposed[^16], at 42 minutes in. He says ‘it takes 1 line of code to trim brackets and dashes from a telephone number, but it takes 10 to tell the user they typed something wrong’.

When an error occurs and the page is refreshed, we should restore whatever it is the user typed. Making users re-type the same information twice is something some just won't do, nor should they have to.

## Summary

In this chapter we solved most of the fundamental problems we face when designing any form, not just registration. In some respects, this chapter has been as much about what not to do as it has about what it is we should. Here are the main takeaways:

- Always use a clear and readily accessible label. Avoid techniques, however ‘trendy’, that defy this rule.
- Use the Question Protocol to find ways to remove fields, thus reducing the effort needed by the user.
- Use the right type of form control to give mobile users a better on-screen keyboard.
- Show errors on submit to keep users in control.
- Write consistent errors in plain language that is specific to the problem.

In upcoming chapters, we'll build on the foundations we've laid here in order to solve more complex problems. In doing so we'll need to explore a host of other patterns at our disposal.

## Footnotes

[^1]: https://adamsilver.io/articles/placeholders-are-problematic/
[^2]: https://twitter.com/lukew/status/872861520811614208?s=09
[^3]: https://adamsilver.io/articles/floating-labels-are-problematic/
[^4]: https://blog.medium.com/signing-in-to-medium-by-email-aacc21134fcd
[^5]: https://www.smashingmagazine.com/2015/12/passphrases-more-user-friendly-passwords/
[^6]: http://www.uxmatters.com/mt/archives/2006/07/label-placement-in-forms.php
[^7]: https://medium.com/@jsaito/making-a-case-for-letter-case-19d09f653c98
[^8]: https://msdn.microsoft.com/en-us/library/windows/desktop/dn742466%28v=vs.85%29.aspx
[^9]: https://resilientwebdesign.com/
[^10]: http://www.uxmatters.com/mt/archives/2014/09/eye-tracking-in-user-experience-design.php
[^11]: https://medium.com/simple-human/inline-validation-is-problematic-399dd01d436f
[^12]: https://www.smashingmagazine.com/2012/06/form-field-validation-errors-only-approach/
[^13]: https://kryogenix.org/code/browser/everyonehasjs.html
[^14]: http://adrianroselli.com/2017/01/avoid-messages-under-fields.html
[^15]: http://www.90percentofeverything.com/2009/02/16/karl-sabino-on-the-roi-of-well-designed-error-messages/
[^16]: https://vimeo.com/138359368
[^x1]: https://www.w3.org/TR/WCAG20-TECHS/G162.html
[^x2]: http://www.outlinenone.com/

- https://www.tjvantoll.com/speaking/slides/constraint-validation/chicago/#/18