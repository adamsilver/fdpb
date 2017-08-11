# A Registration Form

We'll begin our adventure into form design with a humble registration form. We'll use this seemingly simple form, to well, form the foundation on which to solve more complex forms later. Choosing such a simple form to start with makes it easier to tease out some of the most important qualities found in well-designed forms.

## How it might look

![Registration Form](./images/.png)

HTML:

```html
<form>
  <label for="firstName">First name</label>
  <input type="text" id="firstName" name="firstName">
  <label for="lastName">Last name</label>
  <input type="text" id="lastName" name="lastName">
  <label for="email">Email address</label>
  <input type="email" id="email" name="email">
  <label for="password">Password</label>
  <input type="password" id="password" name="password" placeholder="Must be 8 characters">
  <input type="submit" value="Register">
</form>
```

The form contains just 4 fields and a submit button with each field having an associated label. Labels are where our analysis begins.

## Labels

Labels describe the requirements for the form control. Each field needs an associated label because:

- sighted users can see the instructions.
- visually-impaired users can hear the instructions using a screen reader.
- motor-impaired users can more easily move focus to the field thanks to the larger hit area. This is because clicking the label moves focus to the field.

To *connect* an input to a label, the `id` and `for` attributes must match and contain a unique value. Forgetting to do so, means ignoring the needs of those with visual and motor impairments.

Seeing as we're designing for people, we should use the *diversity* of people as constraints. These constraints ultimately lead to the creation of form patterns that work well for a diverse set of user needs.

Many designers omit labels in order to save space but as the budding form designer that you are, you should be aware that this is just about the worst thing you can do. In later chapters we'll discuss scenarios where we'll need to resist the natural temptation to exclude them&mdash;something that is normall done in the name of minimalist design.

## Placeholders and hints

The `placeholder` attribute is used to store additional content that provides extra clarification for the field beyond its label. This is particularly useful for fields that have complex rules such as the password field (discussed shortly). Hints, unlike labels, are optional and should be used only when necessary, not as a mattter of course.

Placeholders appeal to designers because of their minimal aesthetic and the fact they save space. This is because placeholder text is placed *inside* the field. It's given a ‘greyed out’ aesthetic so that it looks different to real values.

![Hint pattern](.)

Sometimes, designers use placeholders in place of labels which we know is a usability failure. But even if they are used in addition to labels, they are still problematic. Here's why:

- They disappear when the user types making the hint easy to forget.
- Users often mistake the text for a value causing them to skip the field, only to be shown an error after submission.
- The grey text lacks sufficient contrast making them hard-to-read.
- Some browsers don't support them.
- Some screen readers don't announce them.
- Some browsers don't translate them.
- Long placeholder text may be cut off.

That's a lot of problems for something that looks so innocent. The majority of the time browser vendors make the right design decisions for native behaviour, but not always. Sadly, placeholders are an example of this. The thing is, content&mdash;even a hint&mdash;should not be considered an enhancement. If a hint helps the user answer the question, then it should be readily available.

Instead of using a placeholder to store the hint, we can simply add the hint as an extra element inside the `label`. Something like this:

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

The hint is placed inside a `span` so that we can style it differently to the label. It's placed inside the `label` to so that screen readers will announce it. As is often the case, there are many ways to achieve the same design. One alternative approach is to use a separate element (such as a `p`) as follows:

```HTML
<div class="field">
  <label for="password">Password</label>
  <p class="field-hint" id="passwordhint">Must contain 8+ characters with at least 1 number and 1 uppercase letter.</p>
  <input type="password" id="password" name="password" aria-describedby="passwordhint">
</div>
```

Here we've used the `aria-describedby` attribute to connect the hint to the field by referencing the `id`. Whilst this has visual and audibe parity with a label, this solution is not created equal. Crucially, the first rule of ARIA is *Don't use ARIA*. The specification states:

> If you can use a native HTML element or attribute with the semantics and behaviour you require already built in, instead of re-purposing an element and adding an ARIA role, state or property to make it accessible, then do so.

This is important and it's not a merely an academic endeavour. In this case, a `label` gives users a better experience with less effort. It's better because  placing the hint inside the label further increases the hit area. ARIA is for assistive technology, not for browser behaviour. It's less effort because the semantics the label provides the behaviour automatically.

## Float labels

Float labels are a bit of a hybrid between a label and a placeholder. The label is placed inside the field, and then when the user starts typing, the label floats above the field, hence the name. Designes like this approach because because supposedly they reduce the height of the form and therefore make the forms easier to scan. There is also the added bonus that the animation is fancy.

![Float label](.)

The thing is, fancy interfaces don't make users feel awesome. Obvious, inclusive and robust interfaces do. Reducing the height of forms is a noble goal if done right. But using float labels introduces problems for an unconvincing gain. Facetiously, we could remove all the whitespace and decrease the font size to 5px which also reduces the height. But rather obviously, this doesn't create a better experience.

Forms expert, Luke Wobrelski[^2] hasn't got any data at scale that shows shaving pixels in forms increases conversion or usability. In anycase float labels don't actually save space because the label needs space to move into. Besides, scanning a form is not the only user need. Users have to read and understand instructions, fill them in and fix errors. Float labels are problematic because:

- There is no space for a hint. The label and hint are one and the same.
- They are hard to read due to poor contrast and small text.
- Like placeholders, they may be mistaken for a value.

Decluttering an interface is a noble goal. But only when we declutter the superfluous; not the essential. Labels are essential, and in some cases so are hints. Employing a pattern that moves away from convention, but that is both problematic and constraining too is not a recipe we'll use to cook up delicious forms. More importantly that's the start and end of the cooking analogy.

Ultimately, this book is about designing forms that work. Forms that don't drive users crazy. And forms that have as little friction as possible. So we'll use labels (and hints) that are readily available, high-contrast and easy-to-read. In any case, there are far more practical techniques to reduce the size of forms which we'll be exploring throughout the book. One way of doing this is by using the Question Protocol.

## The Question Protocol

Nobody wants to use forms. They are not a source of entertainment. People just want the end result. Paying off their debts, receiving some new shoes, or receving a new tax disc so they avoid fines and save money or some of the reasons why people will use a form.

When we put a form in front of someone we have to make sure that there are good reasons for doing so. Why then, are we suggesting people register? As we're looking at patterns as opposed to building a service as a whole, we'll have to consider some reasonably common reasons. Perhaps it's because the user will get:

- A faster checkout experience next time.
- Order tracking without having to make a phone call.
- A 10% discount as a reward for handing over details.

It's not just the form itself that needs justification. The questions within the form have just as much to answer for. Government Digital Services’ (GDS) Question Protocol suggests that before starting the design process, to make a list of the information we need from users. Then, only add a question if you know:

- that you need the information to deliver the service
- why you need the information
- what you’ll do with it
- which users need to give you the information
- how you’ll check the information is accurate
- how to keep the information up to date and secure

Asking these questions urges us to justify the existence of each question. And in doing so nudges us to explore more simpler ways of getting users to fill out forms. For the registration form, we might want to ask ourselves:

- Do we need to ask for their first and last name?
- Do we need to ask for a password? And, assuming we do, are there better ways of asking for it?

### Do we need to ask for their first and last name?

Users don't need to tell us their name to register for an account. The minimum they need to do is provide an email and password. And maybe not even that but more on this shortly. If at some stage we do need their name, we should ask for it then, in context. For example, asking for their name during checkout is required because the product needs to be addressed to a human being.

By removing the first and last name fields, we halve the size of the form. A simple question, a simple answer and a better experience all because we followed the Question Protocol. This, by the way, *naturally* reduces the height of the form without resorting to novel patterns that introduce usability problems.

### Do we need to ask for a password?

Medium.com implemented a ‘no password’ sign in[^4]. A no password sign-in works by leveraging the security of email accounts (that do have a password) by sending the user a login link. If we were to use this technique, this would reduce the registration form down just 1 field. But maybe this an over simplification.

Users are less familiar with this approach, although that is not a reason in itself to avoid improving the experience. More importantly when people login, they have to switch to their email account (which also requires logging in to). For those that know their password, or use a password manager, this switching is actually a slower than a more traditional login experience.

This shows that designing a form in isolation is not a sensible approach to design. The Question Protocol makes us think about the journey as a whole. This discussion isn't about proving that one technique is better than the other. It simply shows that merely having to tink about it probably causes us to design better experiences in the end.

For now, we'll stick with the password field which the safer option for most users thanks to it's more familiar appearance. And after all, moving away from convention is something we should do confidently through user testing and research. Without doing that, the we might just be exchanging one set of problems for another.

### Are there better ways to ask for a password?

Passwords are generally short, hard to remember and easy to crack, even if they are forced to meet the following rules:

- eight characters
- one uppercase letter
- one lowercase letter
- one number

Where possible we want to ask users something that is easy to remember and more secure. Passphrases[^5] are easier to remember. They are considered more secure due to their length and the fact you don't need to write them down. But what is a passphrase?

A passphrase is a series of words such as ‘monkeysinmygarden’ (I'm sorry but that was the first thing that came to mind). The only downside with passphrases is that like the ‘no password’ sign in, it's unfamiliar. Unfamiliarity, particularly when it comes to passwords can cause anxiety for users, that are already worried about online privacy and security.

Again, we shouldn't discount this technique because it's new or unfamiliar, but we should explore the validity of the approach with users before making the call. We'll stick to a more traditional password field that requires a set of complex rules. In doing so we'll look at ways to make this common approach better for users.

## Marking required fields

Traditionally, we've marked required fields using an asterisk. A legend, usually placed above the form explains its meaning. I'm not entirely sure how this came to be, but having a layer of abstraction (between the symbol and the meaning) puts cognitive strain on the user. Luke Wobrelski says *including the phrase “optional” after a label is much clearer than any visual symbol you could use to mean the same thing. Someone may always wonder “what does this asterisk mean?” and have to go hunting for a legend that explains it.*

The Question Protocol encourages us to only include questions that are essential. If everything is required, we don't need to mark anything. In Required Versus Optional Fields[^] Jessica Enders says *think about what we are doing when we mark something in an interface. We are trying to indicate that it's different.*

If required fields are the norm, and optional fields aren't, then it's the optional fields we should think about marking in some way. In this case put the word *optional*, in brackets, inside the label to ensure it is read out as part of the label. This way the information is obvious in context of the field.

## Label position

The label and hint have purposely been positioned above the field. The alternative (for those that read left to right at least) is left-aligned labels. The supposed advantage is that it reduces the overall height of the form. We already know that it's unwise to focus soley on reducing the height of a form through interface jiggery, but let's take a look at some research anyway.

In Label Placement in Forms[^6], Matteo Penzo's eye tracking research showed that labels above the field are easier to read and faster to complete. Though it must be said it's probably not a big deal. After all, the time to understand the question and enter the answer, takes far longer than reading the label.

Moreover, there are practical reasons to avoid left-aligned labels. On small screens, oriented in portrait (such as mobile phones) there is no space anyway. And for those using screen magnifiers there is more chance of the label disappearing off screen.

And, labels that contain lots of text will wrap onto multiple lines, disrupting the form's visual rhythm. Whilst we should strive to keep labels and hints terse, this is not always possible and so it's a good move to use a pattern that accomodates the varying length of content found in different forms.

## Focus styles

By default, and without any effort on our part, browsers give active form fields a focus style in the form of an outline. This is helpful in its own right, but especially for keyboard users. Some designers dislike the default styling chosen by browsers, so much so that they often ask the developer to remove it. This is an inclusive design anti pattern that we must avoid[^http://www.outlinenone.com/]. If you want to have a focus style that is more in keeping with your brand, then do so. Just don't remove it entirely.

![blah](.)

## The email field

On first glance, this field looks simple and yet, a lot of thought has gone into its construction and design. Firstly the label text explicitly asks for ‘Email address’ where some sites may choose the more ambiguous ‘Username’. That is, unless the site really is asking for a username. Where possible ask users for their email address as it's unique and is normally used for communication.

The text itself is in sentence case because as John Saito explains in *Making a Case of Letter Case[^7], sentence case, in comparison with title case, is generally easier to read, friendlier and makes it easier to spot nouns.

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

## Submit buttons

Designing a submit button requires us to consider 3 main aspects: Visual design, placement and text.

### Visual design

The first thing to know about buttons is that they aren't links. Links are typically underlined or specially positioned to differentiate them amongst other words. When hovering with the mouse cursor will change to a hand. This is because, unlike buttons, links have weak affordance[^8].

In Resilient Web Design[^9], Jeremy Keith discusses the idea of material honesty. He says that *one material should not be used as a substitute for another. Otherwise the end result is deceptive.* Making a link look like a button is materially dishonest. It tells users that links and buttons are the same when they’re not.

Links can do things buttons can't do. For example, links can be opened in a new tab or bookmarked for later which buttons can't do. Therefore buttons shouldn't look like links, nor should they have a hand cursor. Instead we should make buttons look like buttons.

### Placement

The primary action should be placed where users are naturally going to look for it. Eye tracking tests[^10] show that users naturally and intuitively look directly below the last field. This makes sense when we consider the form's flow and rythm. This also helps users who zoom in, as a right-aligned button would disappear off-screen more easily.

### Label

The word *submit* is probably a bad name for a submit button. The word(s) should describe the specific action being taken. And, because it's an action it should be a verb.

We should aim to use as few words as possible because it makes reading faster. But we shouldn't remove words at the cost of clarity. If you need two words to make the action clear then do so and use sentence case like we do for the field labels.

The exact words may need matching to your brand's tone of voice but don't exchange clarity for cuteness. In our case ‘Register’ is fine.

## Validation

We've put in a lot of effort to create a well-designed registration form. Despite these efforts, we cannot eradicate human error. That is, people make mistakes and it's our job to make sure that fixing them is easy. To design a great validation experience we need to consider:

- When to give feedback
- How to show errors
- How to write errors
- How to be forgiving
- Restoring values

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

Whilst this feedback is instant, it's also ambiguous and suffers from two  problems. First, disabled buttons are afforded by being ‘greyed out’, but this makes the button hard-to-read.

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

Considering these additional problems, we realise that designing for ‘Javascript off’ is crucial. This is afterall an important aspect to Progressive Enhancement because when (not if) the enhancement fails this is the experience they'll get. Having explored the problem from a completely different angle, the answer still, is to validate onsubmit`.

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