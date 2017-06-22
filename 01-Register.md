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

The `placeholder` attribute is used to store additional text that acts as a hint for the field. This is particularly useful for fields that have complex rules such as our password field.

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

This is how it looks in our registration form, but we'll use this pattern throughout the book.

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

For our registration form, we might want to ask ourselves:

- Do we need to ask for their first and last name?
- Do we need to ask for a password?
- Assuming we do, are there better ways of asking for it?

### Do we need to ask for their first and last name?

Users don't need to tell us their name to register for an account. The minimum they need to do is provide an email and password. And maybe not even a password is needed (more on this shortly).

If at some stage we do need their name, we should ask for it then, in context. For example, asking for their name during checkout is required in order to deliver the address to the recipient.

We'll remove these tewo fields in our form. In doing so it halves the size of our form. A simple question, a simple answer and a better experience. This naturally reduces the height of the form, but without resorting to novel and problematic design patterns.

### Do we need to ask for a password?

Medium.com have implemented a no password sign in[^]. A no password sign-in works by leveraging the security of email accounts (that do have a password) by sending the user a login link. 

If we were to use this technique, this would reduce our registration form down to a single field. But this may be an over simplification. Users are less familiar with this approach, although that is not a reason in itself to avoid improving the experience.

More importantly when people login, they have to switch to their email account (which also requires logging in to). For those that know their password, or use a password manager, this switching is actually a lot slower.

This shows that designing a form in isolation is not a sensible approach to design. The Question Protocol helps us think about the journey as a whole.

This discussion doesn't show that one approach is better than the other necessarily. It simply proves that discussion and analysis is a good thing and makes us conscious of our decisions and the effect they have on users. 

We'll keep the password field making the experience familar and straightforward for most users. Moving away from convention is something we should do through testing. Otherwise we may end up exchanging one set of problems for another.

### Are there better ways of asking for a password?

Passwords are typically designed to conform to a complex set of rules. They must have at least:

- eight characters
- one uppercase letter
- one lowercase letter
- one number

Where possible, we should avoid asking users to create complex passwords because they are hard to remember. In the case of simplifying the password field, we might consider passphrases[^2].

Like the No Password technique, a passphrase has many benefits but they are uncommon and unfamilar. Once again this is something we should explore through testing.

We'll stick to a more traditional password field (that has complex rules). In doing so we'll look at ways to make such a field easy to use.

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

The first field asks users for their email. As this is self explanatory we don't need a hint. And the text of the label should be *email address*. As obvious as this may sound to us here, some sites actually label the field username. Where possible we should be specific and explicit.

As we're asking users for their email address we can use HTML5’s `input[type="email"]`. In doing so it improves the experience for those using supporting browsers. Nowadays that's most. The benefit is that mobile devices will display dedicated on-screen keyboards that expose a readily accessible @ and period characters which every email address contains.

![Put image here of email keyboard]()

Non-supporting browsers will fall back to a standard text field and thus a standard keyboard. No biggie. This is one of the advantages of progressive enhancement, a principle we'll be using throughout this book. A principle that is one of the cornerstones of designing inclusive experiences.

## Password field

A password field (`input type="password"`) shows circles for each character typed. This is a security measure that protects users from people looking *over the shoulder*.

The problem with circles, is that it's much harder for users to fix typos because the value is obscured. And because of this, it's often easier to delete the whole entry and start over, than it is to work out which circles you're confident are correct.

In reality, it's not often that people are standing behind you as you type your password. For this reason it's a good idea to enhance the field with a password reveal[^3]. It's no less secure and it's significantly easier to use because the user can check what they type.

The password reveal also stops the need to use an additional "confirm password" field which further reduces user's workload.

Some browsers provide a password reveal natively. So if you write your own cross-browser solution, then you'll want to supress this functionality:

```css
input[type=password]::-ms-reveal {
  display: none;
}
```

Removing the masking altogether is an option, but as we've already discussed, moving away from conventions that are familar can breed fear into people that already have concerns with security and privacy on the Internet.

## Label text

Whilst we've discussed the importance of labels, we haven't addressed the text itself. This is so important. What may seem obvious may not always be.

I've often seen email addresses labelled as "username". But if the username always requires an email address we should make that clear. So our email field has a label value of "Email address", accordingly.

What's not as obvious is the text we should use for the password field. The label "Password" is potentially missing some clarity. It could suggest that the user needs to type a password they already possess. This is both incorrect and bad for security.

It might also subtly suggest the user is already registered. And they might forget what they're doing and think they're signing in. In which case  "Choose password" may be an improvement that provides clarity from all perspectives. This reinforces the fact a) they are signing up and b) that they should create a new password.

Once again, the point of this discussion is not to provide the definitive answer on the correct label for your form. The point is to think about the importance of labelling in general. Friends of mine say *Content is the user experience* and this is a good way to reinforce this principle.

## Submit buttons

Designing a button is straightforward, and yet the aggregration of small details goes a long way.

The first thing to note about buttons is that they aren't links. Links typically have special positioning (such as those placed within a header) and underlines to denote their meaning. And they also have a hand cursor (also known as a pointer).

As buttons aren't links, we shouldn't give them any of this treatment. They should look like a button in order to help users realise they are different. Fortunately buttons have a strong affordance[^] anyway, and so we don't need to mess with them.

We should align the button to left, inline with the fields themselves. It doesn't make much sense to position the fields to the left, and the submit button to the right. If labels sit atop of the field, the vertical rhythm is straight down. This provides an intuitive expectation that what comes next will appear directly below.

Moreover, aligning buttons to the left will help users who zoom. Conversely a right-aligned button would disappear off-screen.

The button's text must be descriptive of the action being taken. In our case "Register" or "Sign up" is both meaningful and terse. The exact words might need to align more with your brand and tone of voice but don't exchange clarity for cuteness.

Whilst each of these things seem small in isolation, it's the combination that makes the difference to users. Most sites don't care about these details, but we do.

## Validation

Up to now, we've done as much as we can to make our registration form easy to use. We have clear, always visible labels. And we have readily accessible hints for added clarity where necessary.

We also have a layout that works well on small and big screens. And a design that caters for people who use a keyboard. And lastly we've considered the experience for people suffering from various visual or motor impairments.

However, people make mistakes. And when they do, it's our job to design an experience which makes fixing them as quick and as painless as possible.

Validation is the biggest design challenge we've faced so far and whilst it's not a huge design challenge, most sites get this wrong&mdash;either by not doing enough, or by doing too much. Or strangely by doing both at the same time. Don't worry, I'll explain.

To ensure users can fix errors easily, we'll need to tell them what's gone wrong and how to make it right. Our first discussion focuses on *when* to provide feedback.

### When to validate

To design an inclusive experience we must first consider the experience without Javascript, which is more common than most think[^]. Fortunately, when we consider the experience for people without Javascript we're left with one option, which is to validate `onsubmit`.

This is the beauty of designing to constraints. In this case, it means we can focus on designing the best form validation experience through submission to the server.

I've often found that&mdash;contrary to popular belief&mdash;making things work without Javascript often produces the best experience anyway. And in a recent large-scale project I was involved with, we *only* performed validation on the server, which worked very well.

This is not to say I advise avoiding client-side validation. I'm simply pointing out that the so-called *degraded* experience may be all that users need. And if we don't need to provide the enhancement, then we don't need to write as much code. This in turn has the following benefits:

1. We have less work to do
2. We have to send less code to the user (meaning a faster experience)
3. It has a wider (read: inclusive) reach by default.

In any case, we can't validate everything on the client. At some point we're going to have to interogate the database. For people to register successfully they will need to enter an email address that hasn't been used, for example.

By designing first without Javascript, not only do we reach a wider audience, but we also cover scenarios that we may have forgotten about had we jumped straight into the all-singing and all-dancing fancy-pants solution.

In short, we'll validate forms onsubmit. This is something that forms do by default and is fully accessible out of the box. Validating on submit gives users consistency and familiarity whether Javascript is available or whether the form was sent through to the server.

I'll be providing a Javascript implementation[^] which behaves the same way as the server-side validation. The only difference is that it does so without a server-side round trip (and subsequent page refresh). The benefit to the user is a faster fail and succeed experience.

If you decide to create your own script, be sure to attach the validation routine to the form's `submit` event and not onto the submit button's `click` event. The latter won't work for users[double check your memory on this] who press *enter* when focussed on various form controls, creating a less inclusive experience.

#### Live validation

One question still remains though. When Javascript is available, we have an opportunity to provide what's commonly called *live validation*. The theory is that it's easier to fix errors as soon as they occur which sounds sensible. The thing is, live validation introduces more problems than it solves.

For entries that require a certain number of characters, the first keystroke will always constitute an invalid entry. This means users will be interrupted early and often.

We could wait until the user has entered enough characters before showing an error. But this means the only way in which a user will get feedback is after they have completed the field successfully which is pointless.

Alternatively, we could provide feedback when the user leaves the field (on `blur`) but this is too late. The user has already started to mentally prepare for (and to fill out) the next field.

Another problem with triggering the feedback on `blur` is that many people switch windows or use a password manager to assist in filling out forms. But leaving the field will cause an error to show prematurely when the user hadn't finished yet.

This is just a few of the associated problems. I've counted 5 others which you can read about in Live Validation Is Problematic[^] if you're interested.

As is often the case in our industry, live validation is a technique that causes more problems than it solves. We'll stick to robust and simple techniques that help users. We'll validate on `submit` and leave the clever stuff to our competitors.

#### Disabling buttons until valid

I've seen some forms that will start off as disabled until a user fills out the fields successfully. As you will probably have guessed by now, this presents several difficulties.

First, as we said earlier, valid formats does not make valid forms. So as the form becomes valid, and enables the button only to be a shown an error later is frustrating.

Second, disabled buttons are greyed out making them hard to read for those with poor eyesight.

Third, they don't convey *why* they are disabled, or what it takes for it to enable. This is why we validate on submit, because allowing the user to submit, allows the system to respond with errors, helping the user to progress.

#### HTML5 validation

As Heydon Pickering says in Inclusive Design Patterns, *there are concerns about the support and the uniformity of HTML5 form validation based on the browser's interpretation.

For these reasons, we'll implement our own implementation. To stop HTML5 compliant browsers from executing their own validation, we'll disable it by using `novalidate` as follows:

HTML:

```html
<form novalidate>
</form>
```

### Be forgiving

TODO: Be flexible, allow spaces, uppercase, lowercase trim.
Jared 42mins, Design Is Metrically Opposed: "it takes one line of code to trim brackets and dashes from a telephone number, but it takes 10 lines to tell the user they typed something wrong".

### Presenting errors

When the user submits a form with errors we'll need to inform the user in three separate ways:

1. Change the page title
2. Display a summary of the errors
3. Display each error message in-context of the field

#### 1. Page title

When a page loads, it's the page's title that is read out first by screen readers. For this reason updating the title to read "The form has errors" (or words to that affect) will drastically help screen reader users.

I recently worked on a project that was accessibility checked by RNIB, and we prepended "Retry - " to the page title. This tested well because as the experience became familiar, a short prompt proved to be both informative and terse.

Whilst it's arguably less useful for those without vision impairments, it could still be a benefit. For users that multi-task and switch between tabs, having that text on the erroneous tab acts as a notification of sorts.

For errors caught through client-side Javascript validation, the change of page title is less important (though you still can). We'll provide more appropriate ways to inform those using use screen readers.

#### 2. Error summary

Next we're going to provide an error summary which looks like this:

![blah blah](/)

We'll position this at the top of the page, so that when it refreshes the error will be shown without the user having to scroll. We can do the same thing, even when Javascript performs validation (averting the page refresh) by bringing the error summary into view.

Conventionally, errors should be styled in red. We'll embrace this convention. To support those who can't see colour, we also provide an icon. As is the case with most inclusive design approaches, this helps people who can see in colour too. A well-designed icon is quicker to scan and interpret than reading long sentences.

Each error message is an internal anchor that sets focus to the field.

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

As Aaron Gustafsson says in The Features Of Highly Effective Forms (39 mins), `role=alert` this will get read out as soon as the page loads, when Javascript didn't catch the error on the client.

The `tabindex` attribute allows Javascript to set focus to the element when an error is caught. This means we can bring the error summary into view. When focus is set, the heading will be read out prompting the user to take informative action.

Without Javascript, and when there are errors, the summary will be rendered on the server. When the page is loaded without errors, this element should be hidden. To do this, the server will need to apply an extra class of `errorSummary-isHidden`.

By doing this, we allow Javascript to reuse the same component (and the same location on the screen). This is important because if the server catches an error, the Javascript validation will clear it appropriately. Otherwise we risk two error summary components being presented at the same time.

The error message text itself is also important. One study showed that *being able to provide custom error messages for one particular e-commerce site increased conversion by 0.5%*. This tiny increase equates to over £250,000 per year[^].

Spending time designing error messages is one of the best investments we can make for users. If an erroneous field simply said "Email address invalid, please fix", this is lazy and unhelpful.

Subtle versions of this happen all the time. If the Email address is invalid, then we need to explain to the user why that is. Perhaps the email address is already in-use. Or perhaps it contains invalid characters. We need to be specific and design our messages accordingly.

#### 3. Provide in-context error messages

The title tag and error summary panels go a long way to help different users and different use cases but we're not done yet. When the user actively starts to fix errors&mdash;particularly when there are multiple errors on longer forms&mdash;they'll have to switch between the form and the error summary.

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

## Footnotes

[^1]: Placeholder article
[^2]: https://www.smashingmagazine.com/2015/12/passphrases-more-user-friendly-passwords/
[^3]: Number 5 in https://uxplanet.org/10-rules-for-efficient-form-design-e13dc1fb0e03#.r3ic58br2
[^3]: https://medium.com/ux-ui-ia-case-studies/masked-passwords-security-questions-captcha-and-other-unusable-security-1f018ad01378#.w8ws8yo23)
[^4]:https://medium.com/@cjforms/the-idea-that-left-aligned-labels-have-slower-completion-times-is-incorrect-e1461f47242b#.dvl3en9g4
[^5]:https://uxdesign.cc/design-better-forms-96fadca0f49c#.iy0c5in6p
[^6]:http://www.90percentofeverything.com/2009/02/16/karl-sabino-on-the-roi-of-well-designed-error-messages/
[^lukesays]: https://twitter.com/lukew/status/872861520811614208?s=09
