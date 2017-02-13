# A registration form

We're going to begin our foray with a registration form. This seemingly simple form has much to discuss. Here it is:

![Image here](/etc/)

```html
<form id="register">
  <label for="firstName">First name</label>
  <input type="text" id="firstName" name="firstName">
  <label for="lastName">Last name</label>
  <input type="text" id="lastName" name="lastName">
  <label for="email">Email address</label>
  <input type="email" id="email" name="email">
  <label for="password">Password</label>
  <input type="password" id="password" name="password">
  <input type="submit" value="Register">
</form>
```

## Labels

The first thing we need to know is that each field needs an associated label. This is because:

- sighted users will be able to see the instructions;
- visually-impaired users will hear the instructions when using a screen reader; and
- motor-impaired users will find it easier to select a field thanks to the larger hit area. This is because clicking a label will move focus to the control.

We might tempt ourselves into omit labels for particular forms in order to save space but this is a case of minimal not equalling simple. Every field needs a label.

## Placeholders

Since placeholders came along, we have adopted them as means of storing hints. Their appeal lies in their minimal aesthetic and the fact they save space.

Some designers go one one step further and replace labels with placeholders. Either way, the placeholder is an Inclusive Design anti-pattern which causes problems (I've counted 13[^1]) for users. I provide 4 here for your convenience:

1. They are easy to forget because they disappears as soon as the user types.
2. When fields are prepopulated, the fields lack clarity.
3. Placeholder text is often mistaken for a value, meaning users skip them and are subsequently shown an error.
4. They have insufficient contrast.

Some people ask me if it’s okay to use a placeholder in addition to a label. I say that if the hint is valuable to the user, we should make it easy-to-read and readily accessible, of which placeholders don’t meet these requirements.

Others say that the placeholder is just an enhancement and not essential to the user. To this I say that if the hint isn’t essential then don’t include it. Content is not an enhancement.

In the case of placeholders, minimal doesn't mean simple. If we do need to provide an additional hint we should provide it outside the field.

TODO: Image needs to go here.

## Floating labels

When I inform people of the placeholder problem, they often tell me that floating labels is the answer. In actual fact, they come with many of the same problems as placeholders but with a few additions:

1. There is no actual space for a hint. The hint and the label are one and the same.
2. The labels are often too small. If we made them bigger we would need to leave a bigger space for the label to float into. In which case they don't save space. They are a novelty. UIs shouldn't be novel.
3. The animation affect is distracting and disorientating.

Decluttering is a noble goal. But only when we declutter the superfluous, not the essential. Labels are essential, and in somecases so are hints.

Emplying a pattern that is problematic and constraining at the same time is not a recipe for a friction-free user experience. I would go as far to say the floating labels are a solution looking for a problem.

## Do we really need ask?

Everytime we ask the user another question, we make it harder for them. So we need to be very careful about whether we even should ask it, let alone, how we ask it.

To register, does the user even need to tell us their name? If we don't have to get personal then don't ask them for it. And I mean *have* to. If it is important to have the user's name, then can we ask for it later instead?

For example, we might need their name in order to send them a product. We may need their name in order to know who we're going to call. It all depends on the service they are signing up for.

The point is that we should only ask for information that is critical and critical at this time.

Let's remove the first and last name fields, reducing the size of our form by half. But can we go further?

Medium.com have implemented a no password check in. They leverage the security of email accounts by sending the user a special login link. So technically speaking this registration form could just be a single field.

This might be taking things too far. For one people are familiar with an email address and password (although that is not a reason in itself to avoid improving the experience of course). More importantly, if the user has to sign in via their email account this experience is clunky.

We may have improved the registration process, but for people that know their password or use a password manager, this flip flopping between your site and email account can be an unnecessary source of friction.

The point of this discussion wasn't to get into the validity of email sign in. It's that we must consider whether we really really need to ask for certain information and can we make forms simpler for users by removing the need to ask.

## Additional hints

Whilst we discussed earlier that placeholders are not a good design pattern for hints, that does not mean the user can't benefit from a hint itself.

For example, if the password field, like most sites is asking for one upper case letter, one lower case and one number, then we should really tell the user in advance, otherwise we can guarantee the user is going to get an error when validating the form.

If possible, we should avoid making users setup a complex password, but we'll discuss that shortly. For now we'll assume the password needs to satisify this complex set of rules.

To help the user we'll want to provide a hint, but instead of putting it inside the field we'll place it outside the field just below the field as follows:

![image](/blah)

A readily accessible hint like this should reduce friction drastically for the user.

But just because we can provide a hant, doesn't mean we should. An email address doesn't really need explaining. Yet, we often see sites that says "Email address" as a hint which is just a repeat of the label anyway.

Or we see "yourname@example.com". Either way we don't need it. We should add a hint outside the field, only when it helps users.

## Complex password

Complex passwords like we just discussed are hard for them to remember. Password managers have their place but they don't solve all problems. We can do better by using a long pass phrase - they are secure and easy to remember.

https://www.smashingmagazine.com/2015/12/passphrases-more-user-friendly-passwords/

## Why should they complete the form

When we decide to put any form in front of users we should first ask ourselves why. If it's compelling to us, the hope is that it's compelling for users. For example, our registration form could help users buy faster next time, or track their order, or get 10% off their next order. Whatever it is, we should be telling users why and explaining the benefits to them.

## An email field

Most sites ask for an email field at some point in time. Our registration also includes one. We should be using the write input type for the job. An email input is not a text input.

An email input will, by default, only accept a valid email address. And on mobile will popup a keyboard that has dedicated and readily accessible keys for @ and . characters, thus making the task of filling in an email address significantly easier:

![Put image here of email keyboard]()

## A password field

A password input will mask what the user types as a security measure. But how often have you been using a computer with someone over your shoulder watching you type your password?

For this reason we should at the very least offer a [password reveal](https://medium.com/ux-ui-ia-case-studies/masked-passwords-security-questions-captcha-and-other-unusable-security-1f018ad01378#.w8ws8yo23). It's no less secure and it's significantly easier to use because the user can check their entries.

Often the user will delete the whole thing rather than counting dots or stars.

> Note that Internet Explorer and Microsoft Edge provide this functionality natively, using an interactive 'eye' icon associated with the <code>::ms-reveal</code> pseudo-class. Since we've provided our own (cross-browser) solution, it would be wise to suppress this feature:

	input[type=password]::-ms-reveal {
	  display: none;
	}

## Required fields

We've done a really good job already of deciding at least thinking about whether the things we're ask for are truly essential. If they are then there is actually nothing to mark as required or otherwise...

- The first thing is to flip the question. What is optional. Mark that instead.
- Do we really need first and last name.

## Combining first and last name

No. Tis complicated.

## Label and input position

Number 5 in https://uxplanet.org/10-rules-for-efficient-form-design-e13dc1fb0e03#.r3ic58br2

Top aligned labels. The biggest advantage to top-aligned labels — they make it easier for different sized labels and localized versions to fit easier within the UI (this is especially good for mobile screens with limited screen space).

- Best completion rates
- easiest to process
- best support for multi language

https://medium.com/@cjforms/the-idea-that-left-aligned-labels-have-slower-completion-times-is-incorrect-e1461f47242b#.dvl3en9g4

- Mobile first approach, why change it?
- Though shall place labels above each control-Multiple columns disrupt a users vertical rythym. Users complete top aligned labeled forms at a much higher rate than left aligned labels. Top aligned labels also translate well on mobile. Can go against it but have a good reason to. This is a good rule of thumb and so will be using this approach through out the book. If in doubt test.
- Position of first and last name on one line? NOPE
- Tabbing etc is better.
- Make sure space below the combined label/field is more. https://uxdesign.cc/design-better-forms-96fadca0f49c#.iy0c5in6p

## Focus styles

When the user focuses a field most browsers will highlight the field which helps the user know where they are. Some designers despise the aesthetic some browsers give by default, because it doesn't match the brand or some such. And in the mist of their hatred, they ask the developer to remove it. That would be okay if they replaced it with something better. But quite often they don't.

There is no reason why we can't replace the outline with our own style and colour. The idea would be to make it clear regardless of colour for those that suffer with vision impairments.

So do this:

TODO: Include example and visual

## Submit buttons

You would think that a button is such a simple thing to design, that getting it right is a no brainer. And yet, the *aggregation of small gains* goes a long way.

The first thing to note about buttons is that they aren't links. Links typically have special positioning (such as a header) and underlines to denote their meaning. And they also have a hand cursor (also known as a pointer).

As buttons aren't links, we shouldn't give them any of this treatment. They should look like a button (affordance) in order to help users realise they are different. Fortunately buttons have a strong affordance anyway, and so we don't need to mess with them too much.

We should align the button to left, inline with the fields themselves. It doesn't make much sense to position the fields to the left, and the submit button to the right.

Keep things inline is easier on the eye, particularly for visually impaired users. Zooming in, for example, could mean a right-most positioned button is off screen.

We'll discuss text in more detail shortly, but emphasising it here is worthwhile too. Whatever we do, the button text should be descriptive of what the form is submitting.

In our case "Register" or "Sign up" is appropriate, friendly and terse. Exactly what you go for, might differe based on your brand. Whichever you go for, don't use jargon and don't be ambiguous. Cutesy text might be errr, cute, but it isn't friendly.

Whilst each of these things seem small in isolation, it's the combination of doing all of these things right that makes a difference for users. It's the little bit details that can have a huge affect.

## Validation

Up to now, we've done as much as we can to avoid user errors. But when the unfortunate thing happens, we need to help our users be on their way.

Validation is the biggest design challenge we've faced so far and whilst it's not that hard, many sites get this wrong by either doing too little, or doing way too much too early.

The user needs to know when the form has errors i.e. something went wrong. And what will make the form valid i.e. what needs fixing and how.

When and how we present this stuff is up to us..

## When to validate

To design an inclusive experience we must consider the experience without Javascript first, which is more common than you may at first think[^2]. Fortunately, when we consider the experience for people without Javascript we find there is only one option which is onsubmit.

I've often found that making things work without Javascript to the best of my ability can quite often provide a great experience as is. And in fact in a recent large-scale government project, we performed validation on the server only. And you know what, because our forms were so friendly, the round-trip caused no issues.

Now don't get me wrong, I'm a fan of client side validation, and we will be looking at this shortly. But the point is when we do the basics first, we can end up with providing the basics only which makes for a faster experience (no loading up extra Javascript for example) and far less for us to maintain as developers.

Also, we can;t fix everything on the client. For example, a user may type something that is formatted correctly but doesn't match up to something in the database. The point being that the user will have to hit the server at some point anyway. So by designing for Javascript off not only do we reach a wider audience but we also cover scenarios that we might not have considered had we jumped straight into the all-singing and all-dancing fancy-pants solution.

Whatever happens we must validate our form on submit anyway so this seems like a sensible place to start. Now it's important to note that our form can be submitted by pressing return/enter as well as pressing the submit button. You see without any developer or designer intervention, forms are accessible to people who use a mouse or a keyboard (or their finger etc).

Validating our form on submit gives users consistency and familiarity whether Javascript kicks in and validates formatting on the client, or whether it was passed through to the server for something more. But when Javascript is available can we do more but should we?

Heydon says:

> Some fancy form validation scripts give you live feedback as you type your text entries, letting you know whether what you type is valid as you type it. This can become very difficult to manage. For entries that require a certain number of characters, the first few keystrokes always going to constitute an invalid entry. So, when do we send feedback to the user and how frequently?

We can provide feedback when the user leaves the field, but at this point the user is finished with the field and has already mentally prepared to fill out the next field.

> Not wanting to be the overbearing restaurant waiter continually interrupting customers to check in with them, we didn't flag errors on first run. Instead, only where errors are present after attempted submission do we begin informing the user.

> Once the user is actively engaged in correcting errors, I think it helpful to reward their efforts as they work. For fields now marked invalid, we could run our validation routine on each input event, switching aria-invalid from false to true where applicable.

I had never thought about this approach until Heydon mentioned it. It's most certainly an improvement, but having spoken to him about this it's still not not ideal. The problem still stands that when the user is fixing an error they are interupted. And validating onblur is too late.

So let's keep it simple and stick with submit only.

## How to present errors

When it comes to actually showing users an error in response to an invalid submission, there are three things to think about.

1. Change the page title
2. Provide an error summary
3. Provide inline errors

### 1. Changing the page title

First we're going to want to change a website's `<title>` so after refresh, a screen reader user will know the form has errors. Most visually enabled users won't notice this but that's just fine.

### 2. Provide an error summary

Next we're going to provide an error summary at the top which looks like this:

![]()

We'll position this at the top of the page, so that when a page refreshes the error will be shown without the user having to scroll. This has become a convention, making things familiar. We won't be breaking that today.

Even with Javascript, we can put the summary here and scroll to it to bring it onscreen again.

> Conventionally, errors are denoted with a red coloration, so it's advisable to give the message box a red border or background. However, you should be wary of red being the only visual characteristic that classifies the message as an error. To support those who cannot see color and screen reader users at the same time, we can prepend a warning icon containing alternative text.

This icon is vital for those with colour impairments but it's also helpful for those with perfect vision. It's much easier to scan for an icon than it is to read through some text explaining that there is an error. Is it is with inclusive design, what you do for one person often helps everyone else too.

- Links to each erroneous field.

Here's the code for the summary:

<div id="errorSummary" class="errorSummary" aria-live="assertive" role="alert">
    Stuff
</div>

> When the form's submission event is suppressed, the live region is populated, switching its display state from none implicitly to block. This population of DOM content simultaneously triggers screen readers to announce the content, including the prepended alternative text: "Error: please make sure all your registration information is correct".

> The advantage of declaring the presence of errors using a live region is that we don't have to move the user in order to bring this information to their attention. Commonly, form errors are alerted to the user by focusing the first invalid form field. This unexpected and unsolicited shift of position within the application risks disorientating the user. In our case, the user remains focused on the submit button and is free to move back into the form to fix the errors when ready.

### Provide inline errors

If the user has more than one error, then having to switch between the summary and the field is some friction that we can remove for users by providing an in-context error message.

And for which gets read out as the user enters form's mode again.

[Placement/position](http://adrianroselli.com/2017/01/avoid-messages-under-fields.html)

- Content

- Code

- ?

## Server side implementation

### Keep populated values

When the page refreshes must populate values as to not make users have to type it in again. Annoying.

### Summary

?

### Inline

?

## Client side progressively enhanced implementation

### HTML5

Not using HTML 5. novalidate. No required boolean. etc. See Heydon.

### Summary

?

### Inline

?

## Words

Up to now we've focussed mostly on the visual and behavioural aspects of form. It's so easy to ignore perhaps the most important aspect of design in general. That would be the content.

Whether we're talking about labels, hints or error messaging, the design of microcopy is essential. The content designers I've worked with say *content is the user experience*. And that's hard to argue with.

With regrads to errors, the affect on UX is huge.

> A little improvement of the error messages on an e-commerce website increased completed purchases by 0.5% (a lot compared to the effort) and saved over £250,000 per year for the company. - £250,000 from better error messages

The same goes for labelling and hints.

The password field's label is *password* which seems simple enough. It's good to be terse. Less but better and all that. But is it missing some clarity. I would suggest that the label could encourage users to type a password they already have. Or that it might seem as they they are already registered.

Perhaps it would be better if it said "Choose password" to reinforce the fact that a) they are signing up and b) that they should create a new password.

- Labels: though shall make forms human and conversational. Don't use jargon.

- Don't use caps?

## Summary

We've covered a lot in this chapter. Other chapters will not cover the same ground. Perhaps some of the same topics will be discussed further or more specifically when necessary, but this serves as the foundation to which to design all other forms. What's interesting in the upcoming chapters is the different problems we need to tackle as designers and how we can solve those problems.

[^1]: Some *crazy* footnote definition.
[^2]: Some *crazy* footnote definition.