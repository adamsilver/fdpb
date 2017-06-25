# A Registration Form

We're going to begin our adventure with registration. We'll use this simple form, to ahem, form the foundations on which to solve more complex forms later.

Don't be fooled by its simple appearance though. There's a lot of ground to cover and patterns that will emerge. Patterns we'll either use directly or as a foundation to build new patterns when needed.

## What it looks like

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

It contains four fields and a submit button. You'll  notice each field has a label. Labels are where our analysis begins.

## Labels

Each field needs an associated label because:

- sighted users can see the instructions.
- visually-impaired users can hear the instructions  using a screen reader
- motor-impaired users can more easily move focus to the field due to the larger hit area. (Clicking the label moves focus to the field.)

To *connect* an input to a label, the `id` and `for` attributes must match and contain a unique value. Forgetting to do so, means ignoring the needs of those with visual and motor impairments.

As we're designing for people, we'll use the diversity of people as constraints. Constraints that ultimately lead to the creation of inclusive form patterns that work.

Many forms omit labels in order to save space but this is the worst thing we can do. In upcoming chapters we'll discuss situations where we'll have to resist the natural temptation to exclude them.

## Placeholders and hints

The `placeholder` attribute is used to store additional text that acts as a hint for the field. This is particularly useful for fields that have complex rules such as the password field.

Unlike labels, placeholders are optional. Designers find them appeaing due to their minimal aesthetic and the fact they save space.

Some go one one step further and replace labels with placeholders. As we know already this is problematic. But actually, placeholders are problematic in their own right. Here's some reasons why:

- It disappears when the user types making it easy to forget.
- It's often mistaken for a value, meaning users skip the field causing an error.
- They lack sufficient contrast making them hard to read.
- Some browsers don't support them.
- Some screen readers don't announce them.
- Some browsers don't translate them.

I've counted 7 additional problems which you can read about more deeply in Placeholders Are Problematic[^].

Content isn't an enhancement[^]. Therefore, if a hint helps users we should make sure it's readily accessible. We can do this by placing it outside the field below the label.

This is how it looks in the registration form, but we'll use this pattern throughout the book.

![]()

HTML:

```HTML
<div class="field">
  <label for="password">
    <span class="field-label">Password</span>
    <span class="field-hint">Your password must be at least 8 characters including at least one number and one uppercase letter.</span>
  </label>
  <input type="password" id="password" name="password">
</div>
```

You'll notice the hint is actually part of the label and we wrap a `span` around it so that we can style it as per design. We put it inside the label to make sure it's read out by screen readers.

## Floating labels

Floating labels work by putting the label inside the field to begin with—like a placeholder. But, when the user starts typing the label moves out of the box to ‘float’ above the field.

Designers like this approach because they supposedly:

- make forms cool
- reduce the height of forms
- make forms easier to scan

Cool interfaces don't make users feel awesome. Obvious, inclusive and robust interfaces do that. So we'll forget that one.

Reducing the height of forms is a noble goal if done right. But using float labels creates problems for an unconvincing gain. We could remove all the whitespace and decrease font size to 5px which also reduces the height but creates an awful experience.

Forms expert, Luke Wobrelski[^lukew] hasn't got any data at scale that shows shaving pixels in forms increases conversion or usability. In anycase they don't actually save space because floating labels need space to move into.

Besides, scanning a form is not the only user need. Users have to read instructions, fill them in and fix errors too. Floating labels are problem because:

- There is no space for hint. The label and hint are one and the same
- They are hard to read due to poor contrast and small text
- Like placeholders, they may be mistaken for a value

I've counted nine problems in total, which you can read about in Floating Labels Are Problematic[^].

Decluttering a UI is a noble goal. But only when we declutter the superfluous; not the essential. Labels are essential, and in some cases so are hints.

Employing a pattern that is both problematic and constraining at the same time, is not a recipe we'll use to cook up delicious forms. More importantly I won't be using this analogy going forward.

Ultimately, this book is about designing forms that work. Forms that don't drive users crazy. Forms that have as little friction as possible.

And so, we'll use a pattern that has an ever present, high-contrast and easy to read label and hint. We'll leave placeholders and floating labels to our competitors and use better techniques to reduce the size of forms.

## The Question Protocol

Nobody wants to use forms. They are not a source of entertainment. People just want the end result. Paying off their debts, receiving some new shoes, or receving a new tax disc so they avoid fines and save money.

When we put a form in front of someone we have to make sure that there are good reasons for doing so. Why then, are we suggesting people register? Perhaps it's because in doing so the user will get:

- a faster checkout
- order tracking
- a 10% discount

It's not just the form itself though. It's the questions within the form that need justification too. Government Digital Services’ (GDS) Question Protocol suggests that before starting design, make a list of the information you need from users. Then, only add a question if you know:

- that you need the information to deliver the service
- why you need the information
- what you’ll do with it
- which users need to give you the information
- how you’ll check the information is accurate
- how to keep the information up to date and secure

Asking these questions ensures we justify the existence of each question. And in doing so urges us to explore more simpler ways of getting users to fill out forms.

For the registration form, we might want to ask ourselves:

- Do we need to ask for their first and last name?
- Do we need to ask for a password?
- Assuming we do, are there better ways of asking for it?

### Do we need to ask for their first and last name?

Users don't need to tell us their name to register for an account. The minimum they need to do is provide an email and password. And maybe not even a password is needed (more on this shortly).

If at some stage we do need their name, we should ask for it then, in context. For example, asking for their name during checkout is required as part of a delivery address form.

By removing the first and last name fields, we've halved the size of the form. A simple question, a simple answer and a better experience. This *naturally* reduces the height of the form without resorting to problematic design patterns such as those discussed earlier.

### Do we need to ask for a password?

Medium.com have implemented a no password sign in[^]. A no password sign-in works by leveraging the security of email accounts (that do have a password) by sending the user a login link.

If we were to use this technique, this would reduce our registration form down to a single field. But this may be an over simplification. Users are less familiar with this approach, although that is not a reason in itself to avoid improving the experience.

More importantly when people login, they have to switch to their email account (which also requires logging in to). For those that know their password, or use a password manager, this switching is actually a lot slower.

This shows that designing a form in isolation is not a sensible approach to design. The Question Protocol helps us think about the journey as a whole.

This discussion doesn't show that one approach is better than the other necessarily. It simply proves that discussion and analysis is a good thing and makes us conscious of our decisions and the effect they have on users.

We'll keep the password field making the experience familar and straightforward for most users. Moving away from convention is something we should do through testing. Otherwise we may end up exchanging one set of problems for another.

### Are there better ways of asking for a password?

Passwords are generally short, hard to remember and easy to crack, even if they adhere to the following rules:

- eight characters
- one uppercase letter
- one lowercase letter
- one number

Where possible we want to ask users something that is easy to remember and more secure. Passphrases are easier to remember. They are considered more secure due to their length and the fact you don't need to write them down.

A passphrase is a series of words such as ‘monkeysinmygarden’. The only downside is that this approach is unfamiliar and unfamiliarity, especially with passwords causes anxiety.

Again, we shouldn't discount this technique because it's new, but we should explore the validity of the approach with users before making the decision either way.

We'll stick to a more traditional password field that requires a set of complex rules. In doing so we'll look at ways to make this approach easy to use.

## Marking required fields

Traditionally, we've marked required fields using an asterisk. A legend, above the form would explain the meaning behind it. I'm not entirely sure how this came to be, but having a layer of abstraction puts cognitive strain on the user.

Luke Wobrelski says *including the phrase “optional” after a label is much clearer than any visual symbol you could use to mean the same thing. Someone may always wonder “what does this asterisk mean?” and have to go hunting for a legend that explains things.*

As we've seen, the Question Protocol encourages us to only include questions that are essential. If everything is required, we don't need to mark anything. In *Required versus optional fields* Jessica Enders says *think about what we are doing when we mark something in an interface. We are trying to indicate that it's different.*

If required fields are the norm, and optional fields aren't then it's the optional fields we should think about marking. In this case put the words optional (in brackets) and inside the label to ensure it is fully accessible.

## Label position

You'll notice that we've placed the label and hint above the field. The alternative is to place labels to the left of the field. The only so-called advantage of this is that it reduces the height. However, we already that mindlessly striving to reduce the height of a form is unwise.

Regardless, there is no space on small screens to put the label here anyway. And even on big screens placing labels beside the field is unnecessarily constraining. This is because if the label wraps onto multiple lines, the flow of the form is disrupted and thus hinders scanability.

You might be thinking that as long as we make labels terse we need not worry about the wrapping and thus the problem goes away. But some questions require longer labels and hints. Putting the label above the field enables us to use a pattern that accomodates to the needs of different questions and their content.

## Focus styles

By default, and without any effort on our part, browsers give active form fields a focus style in the form of an outline. This is helpful in its own right, but especially so for those using keyboards.

Some designers dislike the default styling that the browser vendors chose. So much so that they often ask their developer to remove it altogether. This is an inclusive design anti pattern that we must avoid.

If you want to have a focus style that is more in keeping with your design system, then be sure to replace it with something more suitable, but do not remove it.

![blah]()

## Email field

There's a few things to notice about the label:

- The label is specific. It says ‘Email address’. Some sites will label this field as ‘Username’ which is ambiguous. We should design specific labels where possible.
- The label uses sentence case because as John Saito explains in Making a Case for Letter Case[^], it's easier to read, friendlier and easier to spot Nouns.
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

The first thing to know about buttons is that they aren't links. Links are typically underlined or specially positioned to differentiate them amongst other words. For those using a mouse the cursor will change to a hand on hover. This is because, unlike buttons, links have weak affordance[^].

In *Resilient Web Design*, Jeremy Keith discusses the idea of material honesty. He says:

> One material should not be used as a substitute for another. Otherwise the end result is deceptive.

Making a link look like a button is materially dishonest. It tells users that links and buttons are the same when they’re not. Links can do things buttons can't do. For example, links can be opened in a new tab or bookmarked.

So it follows, buttons shouldn't look like links and they shouldn't use the hand cursor. Instead we should make them look like buttons.

### Placement

The primary action should be placed where users are naturally going to look for it. Eye tracking testing[^] shows that users naturally and intuitively look directly below the last field which makes sense from a flow perspective. This also helps users who zoom in as a right-aligned button would disappear off-screen easily.

### Label

The word *submit* is probably a bad name for a submit button. The word(s) should describe the specific action being taken and because it's an action it should be a verb.

We should aim to use as few words as possible because they are quicker to read. But not at the cost of clarity. If you need two words to make the action clear then use sentence case.

The exact words may need matching to your brand's tone of voice but don't exchange clarity for cuteness. In our case ‘Register’ works well.

## Validation

We've put in great effort to create a well designed registration form. Despite these efforts, we cannot eradicate human error. People make mistakes and it's our job to make sure they know exactly how to fix them.

Ensuring errors are easily remedied relies on a few things. The first being *when* to provide feedback.

### When to validate

To design inclusively we must first consider the experience without Javascript, which is far more common than you may think[^]. Without Javascript we are left to validate forms `onsubmit`.

> ‘There is no creativity without constraint’

Constraints help us solve problems. When we accept the constraints of a system and the users of that system, we narrow our focus, often to the user's advantage.

I've found that, generally speaking, the best experiences are those that don't rely on Javascript anyway. On a recent project, we only performed validation on the server side, and nobody noticed.

This is not to say client-side validation is *bad*. It's just that something that may appear to be  problematic from a designer's point of view, may not be a problem at all for users. Without testing with users, we can't tell.

Not everything can be validated with script anyway. At some point we're going to need to check information in a database. To register, for example, we'll need to ensure nobody else has an account with  their email address.

By designing first without Javascript we reach a wider audience, and expose scenarios that we may have otherwise forgotten, had we jumped head first into the all-singing and all-dancing fancy-pants solution.

For example, if some errors can only be caught on the server, then if we're not careful we could end up with two error summary boxes—one for the server error and one for the client side error. This makes for a poor experience.

[]()

#### Inline validation

Inline validation informs users whether what they type is valid as they type. The theory is that it’s easier to fix errors as soon as they occur instead of waiting until submission. The thing is, inline validation causes several problems:

For entries that require a certain number of characters, the first keystroke will always constitute an invalid entry. This means users will be interrupted causing them to switch mental contexts—entering information and fixing it.

[]()

We could wait until the user has entered enough characters before showing an error. But this means the only way in which a user will get feedback is after they have completed the field successfully which is pointless.

Alternatively, we could provide feedback when the user leaves the field (`onblur`) but this is too late. The user has already started to prepare for (and to fill out) the next field.

Another problem with triggering feedback `onblur` is that many people switch windows or use a password manager to assist in filling out forms. But leaving the field will cause an error to show prematurely before the user finishes.

[]()

These are just a few of the problems with inline validation. I've documented the others in Inline Validation is Problematic[^] if you'd like to know more.

Designers like inline validation because it avoids users seeing lots of errors after filling a long form. Assuming this is a true we can solve this by:

- Removing unnecessary fields (we've done this).
- Ensuring fields are well-understood with clear guidance (again we've done this).
- Using One Thing Per Page which we'll discuss in the next chapter.

In any case, designing the perfect inline validation experience is nigh on impossible. Where possible, we'll focus on solutions that don't come with problems. We'll simply validate forms `onsubmit`.

#### Checklist affirmation pattern

Some fields such as the password field which we've already discussed have a specific set of rules that must be met. In this case we might consider a Javascript enhancement as the user types—but this is not inline validation.

Inline validation works by showing errors as the user types. Conversely this works by marking each rule as correct as the user types. Mailchimp does this really well.

[]()

This is most certainly better than inline validation, but it still suffers from the following problems:

- It only checks the formatting, so it may appear to the user they have done everything right, only to receive an error after submission.
- It creates an inconsistent experience, because other, less complex fields won't have this treatment.
- The on-screen keyboard on mobile phones, for example, may obscure some of the rules causing the user to stop filling out the form and scroll to check.
- It could be distracting to be typing a password and see the interface changing.
- Low confidence users or those that look at the keyboard while typing may not notice the feedback anyway.
- The rules take up a lot of space.

With those problems in the open, this pattern may still help some users. But considering the problems and that the validation provides clear feedback without distracting users we won't bother.

#### Disabling the submit button until valid

Some forms start off with a disabled submit button. When all fields become valid, the button will be enabled, giving users a clue that the form is now valid. This presents several problems for users:

- Validating formats doesn't prove everything is valid, creating an untrustworthy experience. That is, they submit a correct looking form, only to see other errors that have been caught on the server.
- If there is something wrong, the user won't know why. As there is no feedback, the user is left to guess which field is wrong and why.
- Disabled buttons typically have a greyed out treatment that has poor contrast making the button hard to read.

Instead, we'll allow users to submit a form when they want and provide feedback accordingly. This ensures that the system can respond and help users progress.

#### HTML5 validation

As Heydon Pickering says in Inclusive Design Patterns, *there are concerns about the support and the uniformity of HTML5 form validation based on the browser's interpretation.

For these reasons, we'll implement our own implementation. To stop HTML5 compliant browsers from executing their own validation, we'll need to disable it by using `novalidate`:

HTML:

```html
<form novalidate>
</form>
```

### Showing errors

When the user submits an erroneous form we'll need to inform the user by:

- Changing the page title.
- Displaying an error summary.
- Displaying an in-context error.

#### Changing the page title

When a page loads, the `title` is read out first by screen readers. So we'll update the title to read ‘The form has errors’ (or words to that affect).

On one project, the Royal National Institute of Blind People (RNIB) suggested we prepend ‘Retry — ’ which tested well. As the experience became familiar, this shorter prompt proved informative and terse.

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

Next, we'll provide an error summary at the top of the page, so that when the page refreshes, the error will be shown without having to scroll.

![blah blah](/)

We'll apply the same functionality for errors caught on the client-side. But this time, we'll need to focus the error summary into the viewport. More on this later.

Conventionally speaking, we should style errors in red. To support those who can't see colour, we'll need to ensure the summary is prominent without it. We could use an icon, or a short heading for the summary.

As is often the case with inclusive design patterns, this helps everyone—not just those who can't see colour. Here's what it looks like:

[]()

HTML:

```html
<div class="errorSummary" role="alert">
  <h2 tabindex="-1">Please fix the 4 errors</h2>
  <ul>
    <li><a href="#emailaddress">Email address cannot be empty</a></li>
    <li><a href="#password">Password cannot be empty</a></li>
  </ul>
</div>
```

Here's a few notes on the HTML:

- Each error message is an internal anchor that sets focus to the field.
- As Aaron Gustafsson says in The Features Of Highly Effective Forms (39 mins), `role="alert"` this will be read out when the page finishes loading, when Javascript didn't catch the error on the client.
- The `tabindex` allows us to programmatically set focus to the element when an error is caught by script. This means we can bring the summary into view. When focus is set, the heading will be read out prompting the user to take action.

Without Javascript, the server will render the summary. When the page is loaded without errors, this element should be hidden. To do this, the server will need to apply a `hidden` attribute. For browsers that don't support it, just add `hidden { display: none; }` to your stylesheet.

This allows us to reuse the same component (and the same location on the screen). This is useful because if the server catches an error, the Javascript validation will clear it appropriately. Otherwise we risk two error summaries being shown.

The error message text itself is also important. One study showed that *being able to provide custom error messages for one particular e-commerce site increased conversion by 0.5%*. This tiny increase equated to over £250,000 per year[^].

Spending time designing error messages is one of the best investments we can make for users. An error message of ‘Email address invalid, please fix’ is lazy and unhelpful.

Instead, we need to be specific. Did the user miss the @ symbol, or has the address been taken?

#### 3. Display in-context error messages

Placing errors in-context of the field is helpful on many counts.

What it looks like:

HTML:

```HTML

```

We can do better. We can place error messages beside each field. However, we can't just place the text in a paragraph because people using screen readers won't be aware of such information.

As we know already, labels are read out as the user focuses each control. So we can piggyback this well supported functionality by injecting error messages inside the labels themselves. This way, on focussing on the erroneous email control, for example, they will hear something like "Email address. Your email address is missing the @ symbol."

We'll be discussing how to handle groups of fields, such as radio buttons, in upcoming chapters. The reason I bring this to your attention, is that when we inject errors into a group of fields, injecting it into a label doesn't work.

```html
<div class="field">
  <label for="blah">
    <span class="field-label">Email address</span>
    <span class="field-error">Your email address is invalid</span>
  </label>
</div>
```

The classes themselves act as hooks for styling and behaviour. You can view a demo of the code[^] to see how these things apply, and customise them to your needs.

## Summary

We have navigated through many of the fundamental design challenges that most forms need to accommodate. In many ways this chapter has explained an equal amount of what not to do, as it has what it is we should do. In short:

1. Always use a clear, readily accessible, human-readable label.
2. Avoid any rule that inhibits rule number one.
3. Strive to reduce the amount of questions we ask of our users.
4. Use the right form control so that browsers can enhance the experience.
5. Validation should be executed on submit. Upon execution there are 3 parts of the page to update to ensure a fully inclusive, and easy path to remedying any errors.

Whilst we have covered a lot of ground in this chapter, this is lots more to discuss. The foundations have been laid, and with that we will up the ante and face even tougher challenges. In doing so we can explore some of the more interesting techniques at our disposal.

## TODO

- ARIA - Put the first rule of aria in chapter 1, why we use labels, and put errors inside those.

- When the page refreshes must populate values as to not make users have to type it in again. Annoying.

Jared: That stupid credit card security code, that is erased as long as there’s some other error on the page. Then you correct the other error and submit it, [?] and it says, “But you didn’t put in your security code.” I said, “I did put in my security code. You have the memory of a goldfish.”

### Be forgiving

TODO: Be flexible, allow spaces, uppercase, lowercase trim.
Jared 42mins, Design Is Metrically Opposed: "it takes one line of code to trim brackets and dashes from a telephone number, but it takes 10 lines to tell the user they typed something wrong".


## Footnotes

[^1]: Placeholder article
[^2]: https://www.smashingmagazine.com/2015/12/passphrases-more-user-friendly-passwords/
[^3]: Number 5 in https://uxplanet.org/10-rules-for-efficient-form-design-e13dc1fb0e03#.r3ic58br2
[^3]: https://medium.com/ux-ui-ia-case-studies/masked-passwords-security-questions-captcha-and-other-unusable-security-1f018ad01378#.w8ws8yo23)
[^4]:https://medium.com/@cjforms/the-idea-that-left-aligned-labels-have-slower-completion-times-is-incorrect-e1461f47242b#.dvl3en9g4
[^5]:https://uxdesign.cc/design-better-forms-96fadca0f49c#.iy0c5in6p
[^6]:http://www.90percentofeverything.com/2009/02/16/karl-sabino-on-the-roi-of-well-designed-error-messages/
[^lukesays]: https://twitter.com/lukew/status/872861520811614208?s=09
[^position]: http://www.uxmatters.com/mt/archives/2014/09/eye-tracking-in-user-experience-design.php
