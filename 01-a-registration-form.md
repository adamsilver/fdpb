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

## A note on styling

The way you style your form components will, at least in part, be determined by your product or companies brand guidelines. With that said, there are some considerations that will universally help your users.

First, the label (and optional hint) is placed above the form controls. In “Label Placement In Forms”[^6], Matteo Penzo's eye tracking tests showed that labels are easier to read this way. Though the time to understand the question and answer it, takes far longer than reading the label. 

More practically on small screen devices there is no space to put the label anywhere else. And for visually-impaired users who use a screen magnifier, there is less chance of the label disppearing off screen this way.

And, labels that contain lots of text will wrap onto multiple lines, disrupting the form's visual rhythm. Whilst we should strive to keep labels and hints terse, this is not always possible and so it's a good move to use a pattern that accomodates the varying length of content found in different forms.

- Font-size
- Padding
- Hit area
- Thick border

## Focus styles

By default, and without any effort on our part, browsers give active form fields a focus style in the form of an outline. This is helpful in its own right, but especially for keyboard users. Some designers dislike the default styling chosen by browsers, so much so that they often ask the developer to remove it. This is an inclusive design anti pattern that we must avoid[^http://www.outlinenone.com/]. If you want to have a focus style that is more in keeping with your brand, then do so. Just don't remove it entirely.

![blah](.)

## The email field

On first glance, this field looks simple and yet, a lot of thought has gone into its construction and design. Firstly the label text explicitly asks for ‘Email address’ where some sites may choose the more ambiguous ‘Username’. That is, unless the site really is asking for a username. Where possible ask users for their email address as it's unique and is normally used for communication.

The text itself is in sentence case because as John Saito explains in “Making a Case of Letter Case”[^7], sentence case, in comparison with title case, is generally easier to read, friendlier and makes it easier to spot nouns.

### Email input

HTML5 gave us `input type="email"` improving the experience for those using supporting browsers. Nowadays that most. On focussing the field on a mobile device, for example, spawns an on-screen keyboard which is specifically designed to help users enter an email address. This is because it displays readily accessible ‘@’ and ‘.’ characters which every email needs.

![Email keyboard](.)

Non-supporting browsers fall back to a standard text field which is Progressive Enhancement in action. One of the main advantages of Progressive Enhancement is that it improves the experience for some without degrading the experience for others. By others I mean those using less capable browsers. Not everyone has the ability to choose their browser. Sometimes it's imposed by the device or corporate computer systems.

Progressive Enhancement itself is an earmark of Inclusive Design. As such, we'll use this design approach to solve many problems in this book.

## The password field

The label for the password field reads ‘Password’ which is okay. But unlike the email field by which the label provided enough clarity, ‘Password’ is a little unclear on its own. That's why we've used the hint pattern to tell users what constitutes a valid password for this particular site.

### Password input

Quite intuitively, the password field uses the `input type="password"`. When the user types it visually replaces each character with a circle. This is a security measure that guards against ‘over the shoulder’ lurking.

The problem with obscuring the value is that it's hard to fix typos. Because of this, it's often easier to delete the whole entry and start over. Another consideration is that most of the time, people don't lurk over your shoulder as you type your password. This fact, however, won't stop users feeling anxious if we were to veer off from convention by showing the password in plain text.

Because this input increases the likeliness of creating a password with a typo, many registration forms have a confirm password field. This extra field is there as a precautionary measure and requires the user to type the same password twice. This is unfortunate because the user has to do even more work.

### A password reveal component

A password reveal component, as they are typically known, gives users the best of both worlds. The field starts of as a regular password input with obscured characters. But gives users a button, that when pressed, converts the input into a regular text box which reveals the password. Clicking it again, converts back into a password field.

This lets users get the benefit of being able to check and fix typos whilst simultaneously giving them a feeling of security and control.
Some browsers provide this behaviour natively so if you're crafting your own solution then you can suppress this with CSS:

```css
input[type=password]::-ms-reveal {
  display: none;
}
```

```JS
function PasswordReveal(el) {
    this.el = el;
    this.passwordControl = $(el);
    this.showingPassword = false;
    this.createButton();
};

PasswordReveal.prototype.createButton = function() {
    this.button = $('<button>'+this.showText+'</button>');
    this.passwordControl.parent().append(this.button);
    this.button.on('click', $.proxy(this, 'onButtonClicked'));
};

PasswordReveal.prototype.onButtonClicked = function(e) {
    e.preventDefault();
    if(this.showingPassword) {
        this.hidePassword();
    } else {
        this.showPassword();
    }
};

PasswordReveal.prototype.showPassword = function() {
    this.el.type = 'text';
    this.showingPassword = true;
    this.button.text('Hide password');
};

PasswordReveal.prototype.hidePassword = function() {
    this.el.type = 'password';
    this.showingPassword = false;
    this.button.text('Show password');
};
```

The script creates a `button` that when clicked toggles the input type between `text` and `password`.

## A submit button

Designing a submit button requires us to consider 3 main aspects: Visual design, placement and text.

### Buttons aren't links

The first thing to know about buttons is that they aren't links. Links are typically underlined or specially positioned to differentiate them amongst other words. When hovering with the mouse cursor will change to a hand. This is because, unlike buttons, links have weak affordance[^8].

In Resilient Web Design[^9], Jeremy Keith discusses the idea of material honesty. He says that *one material should not be used as a substitute for another. Otherwise the end result is deceptive.* Making a link look like a button is materially dishonest. It tells users that links and buttons are the same when they’re not.

Links can do things buttons can't do. For example, links can be opened in a new tab or bookmarked for later which buttons can't do. Therefore buttons shouldn't look like links, nor should they have a hand cursor. Instead we should make buttons look like buttons.

We should still give users feedback when they hover, but we can do so through other style properties such as background colour, border or shadow.

```CSS
input[type=submit] {
	background: red;
}

input[type=submit]:hover {
	background: green;
}
```

### Where to position the submit button

Most forms position the button at the foot of the form. This is because, generally speaking, users fill out the fields first before submitting. At the bottom is certainly a good start but should we align it left, right or center?

To answer this question we need to think about where users are naturally going to look it for it. Think about the form's natural flow and rhythm. The user has been reading from top to bottom with the fields aligned left. Therefore it makes sense that the button is positioned the in the same way: aligned left. Eye tracking tests[^10] show this is exactly the case. This also helps users who zoom-in: a right-aligned label disappears off screen reasily.

### Button text

The word *submit* is an ambiguous way to label a submit button. The word(s) should describe the specific action being taken. And, because it's an action it should be a verb.

We should aim to use as few words as possible because it makes reading faster. But we shouldn't remove words at the cost of clarity. If you need two words to make the action clear then do so and use sentence case like we do for labels.

The exact words may need matching to your brand's tone of voice but don't exchange clarity for cuteness. In this case ‘Register’ is fine.

## Validation

We've put in a lot of effort to create a well-designed registration form. Despite these efforts, we cannot eradicate human error. That is, people make mistakes and it's our job to make sure that fixing them is easy. To design a great validation experience we need to consider:

- When to give feedback
- How to show errors
- How to write errors
- Be forgiving and restore values

### When to give feedback

We can either give users feedback instantly (as they type) or when they submit by pressing submit or through implicit submission. Implicit submission is when the user presses <kbd>enter</kbd> while focus is within a field. There are 3 flavours of instant feedback. We'll discuss each of these now.

#### Inline validation

The first type of instant feedback is inline validation. This informs users whether what they type is valid as they type. The theory is that it’s easier to fix errors as soon as they occur. However, there are several problems with this approach.

For entries that require a certain number of characters, the first keystroke will always constitute an invalid entry. This means users will be interrupted causing them to switch mental contexts. That is, entering information and fixing it.

![Pic](.)

We could wait until the user types enough characters before showing an error. But this means the only way in which a user will get feedback is after they have completed the field successfully which is pointless. Alternatively, we could provide feedback when the user leaves the field (`onblur`) but this is too late. The user has already started to prepare for (and to fill out) the next field.

Another problem with triggering feedback `onblur` is that some users switch windows or use a password manager to help fill out forms. But doing so will cause an error to show prematurely before the user finishes.

![Pic](.)

These are just a few of the many problems with inline validation. The others are documented in Inline Validation is Problematic[^11].

If research shows that users find lots of errors hard to deal with, then we can minimise this by:

- Removing unnecessary fields—we've done this already.
- Ensuring fields have clear guidance—we've done this too.
- Ensuring errors are presented clearly—we'll do this shortly.
- Using One Thing Per Page—we'll discuss this in the next chapter.
- Using Errors-only approach[^12].

In any case, designing a ‘perfect’ inline validation experience is nigh on impossible.

#### Disabling the submit button until the form is valid

Another form of instant feedback is where the submit button is disabled until the form is valid. The user types like normal, and when all the fields becomes valid, the submit button is enabled.

Whilst this feedback is instant, it's also ambiguous and suffers from two  problems. First, disabled buttons are afforded by being ‘grayed out’, but this makes the button hard-to-read.

Secondly, and more crucially, if there is an error, the user won't know why. Instead, they're left to guess which fields are erroneous and need their attention.

#### Checklist affirmation pattern

The last type of instant feedback is what we might call checklist affirmation. Like inline validation, it provides feedback as the user types. Except, instead of showing *errors* it marks each rule as correct. Mailchimp's sign up form uses this technique:

![MC](.)

This is certainly less invasive than inline validation but it still has the following problems:

- It only checks the formatting. That is, it may appear that the user has completed the field successfully but the server might catch another problem.
- It creates an inconsistent experience. This is because other (less complex) fields won't have this behaviour.
- On-screen keyboards, such as those found on mobile may obscure the rules causing the user to scroll to check what's going on.
- It's still a form of distraction. The interface is updating as the user is working on a particular task.
- Low confidence users or those that don't touch-type may not notice the feedback.
- The rules take up a lot of space which may give the perception the task is bigger than it is.

#### Validating `onsubmit`

As we've seen, instant feedback updates the interface without the user taking explicit action. Unsurprisingly then, this creates a jarring, disruptive and confusing experience. Instead, we'll give users feedback when they explicitly expect to get it: on submit. In doing so, our design passes principle 4, *give control*.

There's more to validating onsubmit than it simply being better than the other techniques. Validating onsubmit is a convention. It's just the way forms have been designed to work on the web. We can bypass convention but only if we have a really good reason to do so.

One way of bypassing convention is by using Javascript. In fact, the 3 *instant feedback* techniques above require it to work. Without Javascript, we're left to rely on what browsers give us for free. This is good because constraints like this are important in design.

> There is no creativity without constraint

Without constraint, we can't be creative. As discussed earlier, the needs of users act as constraints, but so does the way in which a platform works. One assumption that designers make is that Javascript is always available and therefore relying on it is okay. This assumption is wrong. Blah designed an excellent poster entitled Everyone Has Javascript, Right[^13] that shows all the points of failure. Here are the main ones:

- If someone interacts with the page before Javascript has loaded
- On a train and connection goes away before the Javascript loads
- The request for Javascript failed
- The corporate firewall or mobile operator blocked the script
- Do they have an extension that inteferes with your script
- Does the browser support the Javascript you've written

Considering these additional problems, we realise that designing for ‘Javascript off’ is crucial. This is afterall an important aspect to Progressive Enhancement because when (not if) the enhancement fails this is the experience they'll get. Having explored the problem from a completely different angle, the answer still, is to validate `onsubmit`.

Interestingly, I've frequently found that good experiences don't need Javascript. For example, on a large-scale Government digital service we only performed validation on the server. Having conducted frequent and thorough user research we never found this to be a problem. This is not to say client-side validation is *bad*, far from it. It's just that something that may appear to be problematic from the point of view of a designer, may not be a problem for users.

Also, Javascript validation tends to only check format. At some point we need to hit the server and run further checks. For example, checking that an email address has not been used to create another account already. By first designing without Javascript, not only do we reach *the widest possible audience*, but we expose scenarios that we may have otherwise missed.

### Displaying errors

Having decided *when* to submit, our next consideration is presenting the errors. Crucially, users need to be alerted that something has not only gone wrong but that they have the right information in the right place to remedy the problem. There are 3 disparate techniques which I've used since 2008 when I worked with the Royal National Institute of Blind People (RNIB) to design the validation experience for Boots.com.

#### Change the title

When a page loads, the `title` element is the first available piece of information. It appears in the browser's tab bar for sighted users and it's the first thing a screen reader will announce, crucial for those who use one. Before submitting the form the title will probably be something like *Register for [awesome service]*.

When there are errors we should prepend the title with *There's a problem -* or a useful alternative *Retry -*.  The latter is useful for a service that is often used and so as the experience gets more familiar the shorter response is informative and terse.

[!](.)

```HTML
<title>Retry - Register for [awesome service]</title>
```

Admittedly, changing the title is mostly for screen readers but that doesn't mean it's presence for sighted users is redundant. For those who multi-task by switching between tabs, the prepended status acts as a psuedo notification of sorts.

#### Show an error summary

Next, we'll provide an error summary at the top of the page, so that when the page refreshes, the error is shown without having to scroll.

![blah blah](/)

We'll apply the same functionality for errors caught on the client. But this time, we'll need to move focus to the error summary to ensure users see it. More on this later.

Conventionally speaking, we should style errors in red. But to support those who can't see (the full range of) colour, we'll make sure the summary is prominent without it by using a short heading and placing it near the top of the screen.

[!](.)

```html
<div class="errorSummary" role="alert">
  <h2 tabindex="-1">There's a problem</h2>
  <ul>
    <li><a href="#emailaddress">Enter an email address</a></li>
    <li><a href="#password">The password must contain an uppercase letter</a></li>
  </ul>
</div>
```

Notes:

- `role="alert"` ensures that a screen reader announces the problem when the page loads.
- The `tabindex` is for Javascript. It lets us move focus to the summary when the user submits the form lower down the page.
- Each error message represented by a link, moves focus to the erroneous field.

When the page loads for the first time *without* errors the summary panel should be empty and hidden. This ensures that we can inject errors from the server or the client to the same location.

```HTML
<div class="errorSummary errorSummary-isHidden">
```

```CSS
.errorSummary-isHidden {
	display: none;
}
```

#### 3. Show in-context errors

As the user moves through an erroneous form we don't want them to have to scroll up and down. *Up* to check the error and *down* to fix it. We also want screen readers to announce the error message as the user enters the erroneous field. This ensures that users have the information they need as they need it. We can achieve all this by simply injecting an error message into the label. This is the same approach we took earlier to give users hints.

[!In context errror](.)

```html
<div class="field">
  <label for="blah">
    <span class="field-label">Email address</span>
    <span class="field-error">Enter an email address</span>
  </label>
</div>
```

Like the hint pattern discussed earlier, we place the error inside the label (above the field) for the same reasons. It gives broad support for screen readers. That is, it will be read out with the label (and hint) when focussed.

As is often the case with inclusive patterns like this, there is yet another benefit for placing the error above the field. In Avoid Messages Under Fields[^], Adrian Roselli explains that doing so is problematic because the browser's auto-complete and on-screen keyboards obscure them.

*Note: The registration form contains text boxes. In the next chapter we'll look at how to handle other form controls such as radio buttons. The spoiler here is that injecting the error into a label doesn't work.*

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

### Validation component

There are concerns about the support and uniformity of HTML5 form validation. Our approach validation requries a custom implementation. To grant us this flexibility, we'll need to turn off HTML5 form validation in browsers that support it:

```HTML
<form novalidate>
```

This enables us to create a reusable and custom validation solution in Javascript:

```JS
function FormValidator(form, options) {
  this.form = form;
  this.errors = [];
  this.validators = [];
  $(this.form).on("submit", $.proxy(this, "onFormSubmit"));
  this.summary = $(".errorSummary");
  this.summary.on('click', 'a', $.proxy(this, 'onErrorClicked'));
};

FormValidator.prototype.onErrorClicked = function(e) {
    e.preventDefault();
    var href = e.target.href;
    href = href.substring(href.indexOf("#")+1, href.length);
    document.getElementById(href).focus();
};

FormValidator.prototype.showSummary = function () {
    this.summary.html(this.getSummaryHtml());
    this.summary.removeClass('errorSummary-isHidden');
    this.summary.focus();
};

FormValidator.prototype.getSummaryHtml = function() {
  var errors = this.getErrors();
    var html = '<h2>You have ' + errors.length + ' errors</h2>';
    html += '<ul>';
    for (var i = 0, j = errors.length; i < j; i++) {
        var error = errors[i];
        html += '<li>';
        html +=   '<a href="#' + error.fieldName + '">';
        html +=     error.message;
        html +=   '</a>';
        html += '</li>';
    }
    html += '</ul>';
    return html;
};

FormValidator.prototype.hideSummary = function() {
    this.summary.addClass('errorSummary-isHidden');
};

FormValidator.prototype.onFormSubmit = function (e) {
  this.removeInlineErrors();
  this.hideSummary();
  if(!this.validate()) {
    e.preventDefault();
    this.showSummary();
    this.showInlineErrors();
  }
};

FormValidator.prototype.showInlineErrors = function() {
  var errors = this.getErrors();
  for (var i = 0, j = errors.length; i < j; i++) {
    this.showInlineError(errors[i]);
  }
};

FormValidator.prototype.showInlineError = function (error) {
  var errorSpan = '<span class="field-error"><span>Error:</span> '+error.message+'</span>';
  var fieldContainer = $("#" + error.fieldName).parents(".field");
  var label = fieldContainer.find('label');
  var legend = fieldContainer.find("legend");
  var errorContainer = fieldContainer.find(".error");
  errorContainer.remove();
  if(legend.length) {
    legend.append(errorSpan);
  } else {
    label.append(errorSpan);
  }
};

FormValidator.prototype.removeInlineErrors = function () {
  $(this.form).find(".field .field-error").remove();
};

FormValidator.prototype.addValidator = function(fieldName, rules) {
  this.validators.push({
    fieldName: fieldName,
    rules: rules,
    field: this.form.elements[fieldName]
  });
};

FormValidator.prototype.validate = function() {
  this.errors = [];
  var validator = null,
    validatorValid = true,
    i,
    j;
  for (i = 0; i < this.validators.length; i++) {
    validator = this.validators[i];
    for (j = 0; j < validator.rules.length; j++) {
      validatorValid = validator.rules[j].method(validator.field,
        validator.rules[j].params);
      if (!validatorValid) {
        this.errors.push({
          fieldName: validator.fieldName,
          message: validator.rules[j].message
        });
        break;
      }
    }
  }
  return this.getErrors().length === 0;
};

FormValidator.prototype.getErrors = function() {
  return this.errors;
};
```

Notes:

- It listens to the form's `submit` event.
- On submit, it clears any errors and validates the form.
- The validate method steps through each validator that has been added (more on this shortly).
- Each validator rule that fails will be added to the `errors` collection.
- If there are errors, it prevents submission to server and shows the errors in the collection as per the above specification.

To create an instance for the registration we'd need something like this:

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