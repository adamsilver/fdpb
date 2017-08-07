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

The form contains just 4 fields and a submit button. Each field has a label, which is where our analysis begins.

## Labels

Each field needs an associated label because:

- sighted users can see the instructions.
- visually-impaired users can hear the instructions using a screen reader.
- motor-impaired users can more easily move focus to the field thanks to the larger hit area. This is because clicking the label moves focus to the field.

To *connect* an input to a label, the `id` and `for` attributes must match and contain a unique value. Forgetting to do so, means ignoring the needs of those with visual and motor impairments.

As we're designing for people, we'll use the diversity of people as constraints. Constraints that ultimately lead to the creation of inclusive form patterns.

Many forms omit labels in order to save space but as the budding form designers that we are, this is just about the worst thing we can do. In later chapters we'll consider scenarios where we'll have to resist the natural temptation to exclude them normally in the name of minimalist design.

## Placeholders and hints

The `placeholder` attribute is used to store additional text that acts as a hint for the field. This is particularly useful for fields that have complex rules such as a password field. Unlike labels, placeholders are optional.

Placeholders appeal to designers because of their minimal aesthetic and the fact they save space. Some go one one step further and replace labels with placeholders. As we know already, excluding labels is problematic. But actually, placeholders are problematic in their own right. Here's why:

- They disappear when the user types making the hint easy to forget.
- Users often mistake placeholder text for a value causing them to  skip the field which causes an error later on.
- They lack sufficient contrast making them hard-to-read.
- Some browsers don't support them.
- Some screen readers don't announce them.
- Some browsers don't translate them.

Content, even that which is considered to be a hint is not an enhancement. If a hint helps the user fill out the form we should make sure it's readily accessible. Instead of using the placeholder attribute, place a hint below the label. This is how it might look:

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

The hint is inside a `span` so that we can style it differently to the label itself. It's placed inside the `label` which ensures that, like a hint-less label, it's read out by screen readers. We could have placed the hint outside of the `label` using ARIA:

```HTML
<div class="field">
  <label for="password">Password</label>
  <p class="field-hint" id="passwordhint">Must contain 8+ characters with at least 1 number and 1 uppercase letter.</p>
  <input type="password" id="password" name="password" aria-describedby="passwordhint">
</div>
```

Even though we can use ARIA to achieve the same behaviour, the first rule of ARIA is *Don't use ARIA*. Here's how the specification puts it:

> ‘If you can use a native HTML element or attribute with the semantics and behaviour you require already built in, instead of re-purposing an element and adding an ARIA role, state or property to make it accessible, then do so.’

In this case, the native `label` lets us provide the same behaviour without having to sprinkle extra semantic attributes just for screen readers. Even though ARIA support continues to improve, it's never as good as using native elements. And in this particular case, adding a hint inside the label further increases the hit area. Adding an ARIA attribute doesn't mimick this behaviour.

## Floating labels

Floating labels work by putting the label inside the field to begin with—like a placeholder. But, when the user starts typing the label moves out of the box to ‘float’ above the field. Designers like this approach because they supposedly:

- make forms cool
- reduce the height of forms
- make forms easier to scan

Cool interfaces don't make users feel awesome. Obvious, inclusive and robust interfaces do. Reducing the height of forms is a noble goal if done right. But using float labels introduces problems for an unconvincing gain. Facetiously, we could remove all the whitespace and decrease the font size to 5px which also reduces the height. But in a similar vein this doesn't create a better experience.

Forms expert, Luke Wobrelski[^2] hasn't got any data at scale that shows shaving pixels in forms increases conversion or usability. In anycase they don't actually save space because floating labels need space to move into. And besides, scanning a form is not the only user need. Users have to read and understand instructions, fill them in and fix errors. Floating labels are problem because:

- There is no space for a hint. The label and hint are one and the same.
- They are hard to read due to poor contrast and small text.
- And like placeholders, they may be mistaken for a value.

Decluttering an interface is a noble goal. But only when we declutter the superfluous; not the essential. Labels are essential, and in some cases so are hints. Employing a pattern that is both problematic and constraining at the same time is not a recipe we'll use to cook up delicious forms. More importantly I won't be using this cooking analogy anywhere else in the book.

Ultimately, this book is about designing forms that work. Forms that don't drive users crazy. Forms that have as little friction as possible. So we'll use a pattern that has an ever present, high-contrast and easy-to-read label and hint. We'll explore more practical techniques to reduce the size of forms. One way of doing this is by using the Question Protocol.

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

### Are there better ways of asking for a password?

Passwords are generally short, hard to remember and easy to crack, even if they are forced to meet the following rules:

- eight characters
- one uppercase letter
- one lowercase letter
- one number

Where possible we want to ask users something that is easy to remember and more secure. Passphrases[^5] are easier to remember. They are considered more secure due to their length and the fact you don't need to write them down. But what is a passphrase?

A passphrase is a series of words such as ‘monkeysinmygarden’ (I'm sorry but that was the first thing that came to mind). The only downside with passphrases is that like the ‘no password’ sign in, it's unfamiliar. Unfamiliarity, particularly when it comes to passwords may cause anxiety for users, that are already worried about interntet security.

Again, we shouldn't discount this technique because it's new or unfamiliar, but we should explore the validity of the approach with users before making the call. We'll stick to a more traditional password field that requires a set of complex rules. In doing so we'll look at ways to make this common approach better for users.

## Marking required fields

Traditionally, we've marked required fields using an asterisk. A legend, above the form explains its meaning. I'm not entirely sure how this came to be, but having a layer of abstraction puts cognitive strain on the user. Luke Wobrelski says *including the phrase “optional” after a label is much clearer than any visual symbol you could use to mean the same thing. Someone may always wonder “what does this asterisk mean?” and have to go hunting for a legend that explains it.*

The Question Protocol encourages us to only include questions that are essential. If everything is required, we don't need to mark anything. In *Required versus optional fields* Jessica Enders says *think about what we are doing when we mark something in an interface. We are trying to indicate that it's different.*

If required fields are the norm, and optional fields aren't, then it's the optional fields we should think about marking. In this case put the word *optional*, in brackets, inside the label to ensure it is read out as part of the label. This way the meaning is in context of the field in question.

## Label position

The label and hint is positioned above the field. The alternative (for those that read left to right at least) is left-aligned labels. The supposed advantage is that it reduces the overall height of the form. We already know that it's unwise to focus soley on reducing the height of a form through interface jiggery, but let's take a look at some research anyway.

In Label Placement in Forms[^6], Matteo Penzo's eye tracking research showed that labels above the field are easier to read and faster to complete. Though it must be said it's probably not a big a deal. The time to understand the question and type, takes far longer than reading the label.

Moreover, there are practical reasons to avoid left-aligned labels. On small screens that are oriented in portrait (such as mobile phones) there is no space anyway. And for those using screen magnifiers there is more chance of the label disappearing off screen.

Also, labels that contain a lot of text will wrap onto multiple lines, disrupting the form's visual rhythm. Whilst we should strive to keep labels and hints terse, this is not always possible and so it's a good move to use a pattern that accomodates the varying length of content found in different forms.

## Focus styles

By default, and without any effort on our part, browsers give active form fields a focus style in the form of an outline. This is helpful in its own right, but especially so for those using keyboards. Some designers dislike the default styling chosen by browsers, so much so that they often ask the resident developer to remove it. This is an inclusive design anti pattern that we must avoid[^http://www.outlinenone.com/]. If you want to have a focus style that is more in keeping with your brand, then you can, but whatever you do, don't just remove it.

![blah](.)

## Email field

There's a few things to notice about the label:

- The label is specific. It says ‘Email address’. Some sites will label this field as ‘Username’ which is ambiguous. We should design specific labels where possible.
- The label uses sentence case because as John Saito explains in Making a Case for Letter Case[^7], it's easier to read, friendlier and easier to spot Nouns.
- There is no need for a hint as the label is enough.

### Input type email

HTML5 gave us the `email` input which improves the experience for those using a supporting browser. Nowadays that most.

Using this field on a mobile phone, for example, will show a *dedicated* on-screen keyboard with readily accessible ‘@’ and ‘.’ symbols which every email address needs.

![Put image here of email keyboard]()

Non-supporting browsers will fall back to a standard text field. This is one of the many advantages of progressive enhancement. Progressive enhancement improves the experience where possible without hurting others.

Progressive Enhancement is a cornerstone of Inclusive Design, a technique that we'll be using throughout this book.

## Password field

The password label is ‘Password’. However, the label is not enough information on its own. So we can use the hint pattern to explain the rules that constitute a valid password.

### Input type password

A password should use the `password` input type. It will show a circle for each character the user types. This is a security measure that guards against ‘over the shoulder spying’.

The problem is that it's hard to fix typos as the value is obscured. Because of this, it's often easier to delete the whole entry and start over.

Also, most of the time, people aren't lurking over our shoulder. Regardless, people would feel anxious if we were to show the password in plain text.

For all these reasons it's a good idea to have a password reveal. It's no less secure, and it gives users choice to see what it is they've typed.

Being able to check their password, also means we don't have to add an extra ‘Confirm password’ field which reduces effort.

### Password reveal component

Some browsers provide this behaviour natively. If you're crafting your own solution then you'll want to supress this:

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

The script creates a `button` that when clicked changes the input type to `text`. And when clicked again, back to `password`.

## Submit buttons

When it comes to submit buttons there are three design considerations:

- Visual design
- Placement
- Label

### Visual design

The first thing to know about buttons is that they aren't links. Links are typically underlined or specially positioned to differentiate them amongst other words. For those using a mouse the cursor will change to a hand on hover. This is because, unlike buttons, links have weak affordance[^8].

In Resilient Web Design[^9], Jeremy Keith discusses the idea of material honesty. He says:

> One material should not be used as a substitute for another. Otherwise the end result is deceptive.

Making a link look like a button is materially dishonest. It tells users that links and buttons are the same when they’re not. Links can do things buttons can't do. For example, links can be opened in a new tab or bookmarked.

So it follows, buttons shouldn't look like links and they shouldn't use the hand cursor. Instead we should make them look like buttons.

### Placement

The primary action should be placed where users are naturally going to look for it. Eye tracking testing[^10] shows that users naturally and intuitively look directly below the last field which makes sense from a flow perspective. This also helps users who zoom in as a right-aligned button would disappear off-screen easily.

### Label

The word *submit* is probably a bad name for a submit button. The word(s) should describe the specific action being taken and because it's an action it should be a verb.

We should aim to use as few words as possible because they are quicker to read. But not at the cost of clarity. If you need two words to make the action clear then use sentence case.

The exact words may need matching to your brand's tone of voice but don't exchange clarity for cuteness. In our case ‘Register’ works well.

## Validation

We've put in a lot of effort to create a well designed registration form. Despite these efforts, we cannot eradicate human error. People make mistakes and it's our job to make sure that fixing them is easy.

To design a great validation experience we need to consider:

- When to give feedback
- How to show errors
- How to write errors
- How to be forgiving
- Restoring values

### When to give feedback

We can either give users feedback instantly or when they submit by pressing submit or through implicit submission. Implicit submission is when the user presses <kbd>enter</kbd> while focus is within a field. There are 3 flavours of instant feedback. We'll discuss each of these first.

#### Inline validation

The first type of instant feedback is inline validation. This informs users whether what they type is valid as they type. The theory is that it’s easier to fix errors as soon as they occur. However, there are several problems with this approach.

For entries that require a certain number of characters, the first keystroke will always constitute an invalid entry. This means users will be interrupted causing them to switch mental contexts—entering information and fixing it.

[]()

We could wait until the user types enough characters before showing an error. But this means the only way in which a user will get feedback is after they have completed the field successfully which is pointless.

We could provide feedback when the user leaves the field (`onblur`) but this is too late. The user has already started to prepare for (and to fill out) the next field.

Another problem with triggering feedback `onblur` is that some users switch windows or use a password manager to help fill out forms. But doing so will cause an error to show prematurely before the user finishes.

[]()

These are just a few of the problems with inline validation. The others are documented in Inline Validation is Problematic[^11].

If users really do find lots of error hard to deal with, then we can minimise this by:

- Removing unnecessary fields—we've done this already.
- Ensuring fields have clear guidance—we've done this too.
- Ensuring errors are presented clearly—we'll do this shortly.
- Using One Thing Per Page—we'll discuss this in the next chapter.
- Using Errors-only approach[^12].

In any case, designing a ‘perfect’ inline validation experience is nigh on impossible.

#### Disabling the submit button until valid

Another form of instant feedback is by disabling the submit button until the form is valid. The user types like normal, and when all the fields becomes valid, the submit button is enabled.

Whilst instant, this feedback is ambiguous and suffers from two main problems. Firstly, disabled buttons are afforded by being ‘greyed out’, but this is hard to read.

Secondly, and more importantly, if there is an error, the user won't be told why. They're left to guess which fields are erroneous, and even worse they won't know how to fix it.

#### Checklist affirmation pattern

The last type of instant feedback is what we might call checklist affirmation. Like inline validation, it provides feedback as the user types. Except, instead of showing *errors* it marks each rule as correct.

Mailchimp's sign up form uses this technique:

[]()

This is less invasive than inline validation but it still has the following problems:

- It only checks the formatting. That is, it may appear that the user has completed the field successfully but the server might catch another problem.
- It creates an inconsistent experience. This is because other (less complex) fields won't have this behaviour.
- On-screen keyboards, such as those found on mobile may obscure the rules causing the user to scroll to check what's going on.
- It's still a form of distraction. The interface is updating as the user is working on a particular task.
- Low confidence users or those that don't touch type won't notice the feedback.
- The rules take up a lot of space which may give the perception the task is bigger than it is.

#### On submit

Instant feedback causes the interface to update even though the user didn't take explicit action. This creates a jarring, disruptive and ambiguous experience. Instead, we'll give users respect by putting them firmly in control. To do this, we'll provide feedback when they explicitly submit the form.

To design inclusively, we must first consider the experience without Javascript, which is far more common than most people think[^13].

> ‘There is no creativity without constraint’

Constraints help us solve problems. When we accept constraints, we narrow our focus, often to the user's advantage. In this case, we can only validate forms on submit anyway.

Interestingly, I've found that, generally speaking, the best experiences are those that don't rely on Javascript. On a recent project, we only performed validation on the server side, and nobody noticed.

This is not to say client-side validation is *bad*. It's just that something that may appear to be problematic from a designer's point of view, may not be a problem at all for users. But, without testing we can't be sure.

Client-side validation typically only checks the format. At some point we're going to need to check information in a database. To register, for example, we'll need to ensure nobody enters an email address that already exists in the database.

By designing first without Javascript we reach a wider audience, and expose scenarios that we may have otherwise forgotten, had we jumped head first into the all-singing and all-dancing fancy-pants solution.

If some errors can only be caught on the server, if we're not careful we could end up with two error summaries—one generated on the server and one generated on the client.

[]()

As already noted, by validating on submit we create a consistent experience whether errors are caught on the server or the client. And it puts users in control and saves us having to second guess when users expect feedback.

### How to show errors

When the user submits an erroneous form we'll need to inform the user by:

- Changing the page title.
- Displaying an error summary.
- Displaying an in-context error.

#### Changing the page title

When a page loads, the `title` is read out first by screen readers. So we'll update the title to read ‘Errors in - ’ or similar. On one project, the Royal National Institute of Blind People (RNIB) suggested we prepend ‘Retry — ’ which tested well. As the experience became familiar, this shorter prompt proved informative and terse.

Changing the `title` is mostly for those using screen readers. However, for those multi-tasking and switching between tabs, the title acts as a notification of sorts.

[]()

To change the title on the server, we'll need to update the `title` element:

```HTML
<title>Retry - Title</title>
```

To apply the same behaviour in Javascript we need to prepend the text to the documents `title` property:

```Javascript
document.title = 'Retry - ' + document.title;
```

#### 2. Displaying an error summary

Next, we'll provide an error summary at the top of the page, so that when the page refreshes, the error is shown without having to scroll.

![blah blah](/)

We'll apply the same functionality for errors caught on the client. But this time, we'll need to move focus to the error summary to ensure users see it. More on this later.

Conventionally speaking, we should style errors in red. But to support those who can't see (the full range of) colour, we'll need to ensure the summary is prominent without it. We'll use a short but prominent heading to do this.

As is often the case with inclusive design patterns, what helps a minority of users often helps everyone else too. In this case a prominent heading helps everyone, not just those with poor vision.

Here's How it might look:

[]()

HTML:

```html
<div class="errorSummary" role="alert">
  <h2 tabindex="-1">Fix the following errors</h2>
  <ul>
    <li><a href="#emailaddress">Provide an email address.</a></li>
    <li><a href="#password">The password must contain an uppercase letter.</a></li>
  </ul>
</div>
```

Notes:

- `role="alert"` ensures the summary is read out first when the page loads.
- The `tabindex` allows us to programmatically set focus to the element when an error is caught on the client. This means we can bring the summary into view. When focus is set, the heading will be read out prompting the user to take action.
- Each error message is an internal anchor that sets focus to the field.

For those without Javascript support, or for errors that can only be handled on the server, the server will render the summary. When the page loads without errors the summary should be hidden. To do this, the server will need to apply an extra class:

```HTML
<div class="errorSummary errorSummary-isHidden">
```

```CSS
.errorSummary-isHidden {
	display: none;
}
```

This allows us to reuse the same component and the same location on the screen regardless and ensures only one summary will ever be shown.

#### 3. Show in-context error messages

How it might look:

HTML:

```html
<div class="field">
  <label for="blah">
    <span class="field-label">Email address</span>
    <span class="field-error">Provide an email address</span>
  </label>
</div>
```

Like the hint pattern discussed earlier, we place the error inside the label (above the field) for the same reasons. It gives broad support for screen readers. That is, it will be read out with the label (and hint) when focussed.

As is often the case with inclusive patterns like this, there is yet another benefit for placing the error above the field. In Avoid Messages Under Fields[^], Adrian Roselli explains that doing so is problematic because the browser's auto-complete feature obscures them and on-screen keyboards may also obscure them.

Quick note: The registration form contains two text boxes. In the next chapter we'll look at how to inject errors for groups of fields such as radio buttons. Spoiler alert: injecting the error into the label doesn't work.

### How to write errors

Up to now, we've ensured that our approach to validation is robust and inclusive. But this counts for nothing if we were then to neglect the messages themselves. One study showed that *custom* error messages increased conversion by 0.5%, equating to over £250,000 a year in revenue[^15].

A good error message is easy to understand and easy to fix. Whilst it's often backwards to design an interface without first knowing the content, in this case, it's hard to design the messages without understanding the interface.

As we've just designed the interface, we already know that errors might appear in two places: the summary and next to the field. What this means is we need to ensure the error message works well regardless of these two locations. That is, ‘You need to enter the “at” symbol.’ is ambiguous when it appears in the summary but works when next to the field.

[]()

Maintaining two different messages for each error is a lot more work and for little gain. To solve this, we'll design messages that works in both cases. For example, ‘You need to enter the “at” symbol in the email address.’.

We also need to consider pleasantries. Putting ‘please’ at the start of each message seems noisey and repetitive. But some errors sound blunt without it. For example, ‘Please answer this question’ versus ‘Answer this question’. ‘You need to [answer this question.]’ may be better as it sounds softer but it has more words. Tricky.

To help answer the question of pleasantries, we might consider how frequently the system is being used by the same user. For users who use a system every day, removing ‘You need to’ gets straight to the point and may be better. For less frequent users it may come across rude. Without testing it's hard to know.

Regardless of the chosen approach, there's bound to be some repetition. Often when testing validation, we'll submit the form without entering anything. This, of course, presents a long list of errors:

[]()

The repetition is obvious in this worst case scenario. And, as content designers, we might freak out and try fix this cardinal sin. But in reality how often do users submit a long form that is full of errors? Most users aren't trying to break the interface.

With these considerations out of the way, here's a list of tips for designing error messages:

- Use punctuation. Some errors have clauses and contain several sentences.
- Be specific. If the system knows why something is wrong, then it should say so. ‘The email is invalid.’ is ambiguous and puts the burden on the user to find out why.
- Use the active voice. For example, ‘Enter your name’ not ‘Your name must have an entry’.
- Don't blame the user.
- Use plain language. Error messages are not an opportunity to promote your quirky brand's tone of voice. Keep it simple and obvious.
- Be human, avoid jargon. Avoid being cutesy and avoid words like *invalid*, *unrecognised* (which is passive) and *mandatory*.
- Understandable out of context. A message of ‘You need to answer this.’ works when it's next to the field, but it's useless when inside the summary.
- Be terse. Don't be overly chatty, particularly on a system that is used frequently.
- Be consistent. Use the same tone, the same words and the same punctuation.
- Test your messages with users.

### Be forgiving

There are some things we can do to stop users seeing errors unnecessarily. We can forgive users for typing extra spaces. If they type ‘John ’ we can trim automatically trim that to ‘John’. We can also ignore uppercase and lowercase letters and other characters depending on the field.

Jared Spool makes a joke about this in Design is Metrically Opposed[^16], at 42 minutes in. He says ‘it takes 1 line of code to trim brackets and dashes from a telephone number, but it takes 10 to tell the user they typed something wrong’.

### Restoring values

If an error is caught on the server—therefore causing a page refresh—we should ensure whatever the user typed is restored. Otherwise users have to type the same thing again which many just won't do. Avoid unnecessary drop outs by making this part of your workflow.

### Validation component

There are concerns about the support and uniformity of HTML5 form validation. To achieve all of the functionality we've discussed so far and to grant ourselves the flexibility to design *whatever it is we need* we'll build our own implementation.

To do this, we'll first need to ‘turn off’ HTML5 form validation for browsers that support it. We can do this by adding `novalidate` to the form element:

```HTML
<form novalidate>
```

Then we'll need to create a reusable component using Javascript:

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
- Use the right type of form control to provide field-specific keyboards to speed up the process.
- Show errors on submit, keeping users in control and avoiding the problem with instant validation.
- Write errors in the active voice and ensure they are specific, consistent and helpful.

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