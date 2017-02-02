# A registration form

We're going to begin our foray with a registration form. This seemingly simple form has much for us to discuss. Here it is:

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

We might tempt ourselves into omit labels for particular forms in order to save space but this is not something to go minimal on. Every field needs a label.

## Placeholders

Since placeholders came along, we have adopted them as means of storing hints. Their appeal lies in their minimal aesthetic and the fact they save space.

Some designers go one one step further and replace labels with placeholders. Either way, the placeholder is an Inclusive Design anti-pattern which causes problems (I've counted 13[^1]) for users. Here are 4:

1. They are easy to forget because they disappears as soon as the user types.
2. When fields are prepopulated, the fields lack clarity.
3. Placeholder text is often mistaken for a value, meaning users skip them and are subsequently shown an error.
4. They have insufficient contrast.

Some people ask me if it’s okay to use a placeholder in addition to a label. I say that if the hint is valuable to the user, we should make it easy-to-read and readily accessible, of which placeholders don’t meet these requirements.

Others say that the placeholder is just an enhancement and not essential to the user. To this I say that if the hint isn’t essential then don’t include it. Content is not an enhancement.

In the case of placeholders, minimal doesn't mean simple. If we do need to provide an additional hint we should provide it outside the field.

TODO: Image needs to go here.

## Floating labels

When I inform people of the placeholder problem, they often tell me that floating labels is the answer to all our problems. In actual fact, they come with many of the same problems as placeholders but with a few additions:

1. There is no actual space for a hint. The hint and the label are one and the same.
2. The labels are often too small. If they were to be big, we would need to leave a space big enough for the label to fly into. In which case they don't save space. They are a novelty. UIs shouldn't be novel.
3. The animation affect is distracting and disorientating.

But actually whilst decluttering the UI is a noble goal, it's only with declutting the unessential, but fixed and familiar labels are essential. I would go as far to say the floating labels are a solution looking for a problem.

## Do we really need ask for that?

Everytime we ask the user another question, we make it harder for them. So we need to be very careful about whether we even should ask it, let alone, how we ask it.

To register, does the user even need to tell us their name? If we don't have to get personal then don't ask them for it. And I mean *have* to. If it is important to have the user's name, then can we ask for it later instead?

For example, we might need their name in order to send them a product. We may need their name in order to know who we're going to call. It all depends on the service they are signing up for.

The point is that we should only ask for information that is critical and critical at this time.

Let's remove the first and last name fields, reducing the size of our form by half. But can we go further?

Medium.com have implemented a no password check in. They leverage the security of email accounts by sending the user a special login link. So technically speaking this registration form could just be a single field.

This might be taking things too far. For one people are familiar with an email address and password (although that is not a reason in itself to avoid improving the experience of course). More importantly, if the user has to sign in via their email account this experience is clunky.

We may have improved the registration process, but for people that know their password or use a password manager, this flip flopping between your site and email account can be an unnecessary source of friction.

The point of this discussion wasn't to get into the validity of email sign in. It's that we must consider whether we really really need to ask for certain information and can we make forms simpler for users by removing the need to ask.

## Label values

In the public sector, we call the epople who write copy, content designers. And I think it's a good name. They are designing content and as we say in the public sector, *content is the user experience*.

As we've seen above, including a label is paramount, but the text we put inside the label is just as vital.

The password field's label is *password* which seems simple enough. It's good to be terse. Less but better and all that. But is it missing some clarity. I would suggest that the label could encourage users to type a password they already have. Or that it might seem as they they are already registered.

Perhaps it would be better if it said "Choose password" to reinforce the fact that a) they are signing up and b) that they should create a new password.

- Labels: though shall make forms human and conversational. Don't use jargon.

- Don't use caps?

## Additional hints

Whilst we discussed earlier that placeholders are not a good design pattern for hints, that does not mean the user can't benefit from a hint itself.

Our registration form could really do with a hint for the password, especially if, like most sites they are asking for a complex ruleset to pass. "Need to type x characters and one upper and lower case latter etc".

If at all possible, we should avoid making our users satisfy a complex set of password rules that are hard for them to remember (even if they are using a password manager). We can do better than this. Pass phrases are more secure and easy to remember.

https://www.smashingmagazine.com/2015/12/passphrases-more-user-friendly-passwords/

The email address doesn't really need an additional hint. Unless you think a user really needs *e.g. yourname@example.com* which is what many designers place in placeholders. A double whammy of hard to use noise.

Just because we can use a hint, doesn't mean we should.

Back to password, we should say "your password must contain X, Y and Z".

## Why should they provide certain info.

- Reasons to complete the task

- Some forms make it obvious why they should complete the task. Some are less so. With a registration form it might well be important. So we should say as much.

- Why should the user register

## Field types

Talk about email field

Did you notice it this time? No? Look at the keyboard again. That’s right, the keyboard is different. There are dedicated keys for the @ and . characters to help you complete the field more efficiently. As we discussed with type="search", there is no downside to using type="email" right now. If a browser doesn’t support it, it will degrade to type="text". And in some browsers, users will get a helping hand.

Talk about password field

- the keyboards that are displayed on phones

## Typing a password

- Use a password reveal as explained in [this article](https://medium.com/ux-ui-ia-case-studies/masked-passwords-security-questions-captcha-and-other-unusable-security-1f018ad01378#.w8ws8yo23)

Often the user will delete the whole thing rather than counting dots or stars.

> Note that Internet Explorer and Microsoft Edge provide this functionality natively, using an interactive 'eye' icon associated with the <code>::ms-reveal</code> pseudo-class. Since we've provided our own (cross-browser) solution, it would be wise to suppress this feature:

	input[type=password]::-ms-reveal {
	  display: none;
	}

## Marking required fields

We've done a really good job already of deciding at least thinking about whether the things we're ask for are truly essential. If they are then there is actually nothing to mark as required or otherwise...

- The first thing is to flip the question. What is optional. Mark that instead.
- Do we really need first and last name.

## Combining first and last name

No. Tis complicated.

## Label and input position

Number 5 in https://uxplanet.org/10-rules-for-efficient-form-design-e13dc1fb0e03#.r3ic58br2

Top aligned labels. The biggest advantage to top-aligned labels — they make it easier for different sized labels and localized versions to fit easier within the UI (this is especially good for mobile screens with a limited estate).

- Best completion rates
- easiest to process
- best support for multi language

https://medium.com/@cjforms/the-idea-that-left-aligned-labels-have-slower-completion-times-is-incorrect-e1461f47242b#.dvl3en9g4

- Mobile first approach, why change it?
- Though shall place labels above each control-Multiple columns disrupt a users vertical rythym. Users complete top aligned labeled forms at a much higher rate than left aligned labels. Top aligned labels also translate well on mobile. Can go against it but have a good reason to. This is a good rule of thumb and so will be using this approach through out the book. If in doubt test.
- Position of first and last name on one line? NOPE
- Tabbing etc is better.
- Make sure space below the combined label/field is more. https://uxdesign.cc/design-better-forms-96fadca0f49c#.iy0c5in6p

## Why we don't need a fieldset

Do we really need this?

## Focus styles

When the user focuses a field most browsers will highlight the field which helps the user know where they are. Some designers despise the aesthetic some browsers give by default, because it doesn't match the brand or some such. And in the mist of their hatred, they ask the developer to remove it. That would be okay if they replaced it with something better. But quite often they don't.

There is no reason why we can't replace the outline with our own style and colour. The idea would be to make it clear regardless of colour for those that suffer with vision impairments.

So do this:

TODO: Include example and visual

## Buttons

- Shouldn't have a hand cursor. They aren't links.
- Getting the text right. Not "submit".

## Validation

Up to now, we've done as much as we can to avoid user errors. But when the unfortunate thing happens, we need to help our users be on their way.

Validation is the biggest design challenge we've faced so far and whilst it's not that hard, many sites get this wrong by either doing too little, or doing way too much too early.

The user needs to know when the form has errors i.e. something went wrong. And what will make the form valid i.e. what needs fixing and how.

When and how we present this stuff is up to us..

## When to validate

To design an inclusive experience we must consider the experience without Javascript first, which is more common than you may at first think[^2]. Fortunately, when we consider the experience for people without Javascript we find there is only one option which is onsubmit.

And we cant fix everything on the client. For example, a user may type something that is formatted correctly but doesn't match up to something in the database. The point is, the user will have to hit the server at some point. So by designing for the Javascript off not only do we reach a wider audience but we also cover scenarios that we might not have considered had we jumped straight into the all singing and all dancing Javascript solution.

Whatever happens we must validate our form on submit anyway so this seems like a sensible place to start. Now it's important to note that our form can be submitted by pressing return/enter as well as pressing the submit button. You see without any developer or designer intervention, forms are accessible to people who use a mouse or a keyboard (or their finger etc).

Validating our form on submit gives users consistency and familiarity whether Javascript kicks in and validates formatting on the client, or whether it was passed along to the server for something more. But when Javascript is available can we do more but should we? 

Heydon says:

> Some fancy form validation scripts give you live feedback as you type your text entries, letting you know whether what you type is valid as you type it. This can become very difficult to manage. For entries that require a certain number of characters, the first few keystrokes always going to constitute an invalid entry. So, when do we send feedback to the user and how frequently?

We can provide feedback when the user leaves the field, but at this point the user is finished with the field and has already mentally prepared to fill out the next field.

> Not wanting to be the overbearing restaurant waiter continually interrupting customers to check in with them, we didn't flag errors on first run. Instead, only where errors are present after attempted submission do we begin informing the user.

> Once the user is actively engaged in correcting errors, I think it helpful to reward their efforts as they work. For fields now marked invalid, we could run our validation routine on each input event, switching aria-invalid from false to true where applicable.

I had never thought about this approach until Heydon mentioned it. It's most certainly an improvement, but having spoken to him about this it's certainly not ideal. The problem still stands that when the user is fixing an error they are interupted. And validating onblur is too late.

So keep it simple. Stick to submit.

## How to present errors

### Error summary

#### Position

As discussed before, the degraded experience will need the server to handle the validation. The server will deliver the errors through a page refresh. And when any page is loaded it's the top of the page that is seen first. At this point in time, the fact that the user has errors to fix, it makes sense putting this at the top of the page after the header.

#### Content

You have blah errors

error (link)
error (link)

#### Visual design

> Conventionally, errors are denoted with a red coloration, so it's advisable to give the message box a red border or background. However, you should be wary of red being the only visual characteristic that classifies the message as an error. To support those who cannot see color and screen reader users at the same time, we can prepend a warning icon containing alternative text.

This icon is vital for those with colour impairments but it's also helpful for those with perfect vision. It's much easier to scan for an icon than it is to read through some text explaining that there is an error. Is it is with inclusive design, what you do for one person often helps everyone else too.

#### Code

<div id="errorSummary" class="errorSummary" aria-live="assertive" role="alert">
    Stuff
</div>

> When the form's submission event is suppressed, the live region is populated, switching its display state from none implicitly to block. This population of DOM content simultaneously triggers screen readers to announce the content, including the prepended alternative text: "Error: please make sure all your registration information is correct".

> The advantage of declaring the presence of errors using a live region is that we don't have to move the user in order to bring this information to their attention. Commonly, form errors are alerted to the user by focusing the first invalid form field. This unexpected and unsolicited shift of position within the application risks disorientating the user. In our case, the user remains focused on the submit button and is free to move back into the form to fix the errors when ready.

### In-context inline errors

If the user has more than one error, then having to switch between the summary and the field is some friction that we can remove for users by providing an in-context error message.

#### Placement

#### Content

#### Code

?

## Server side implementation

### Summary

?

### Inline

?

## Error copy

> A little improvement of the error messages on an e-commerce website increased completed purchases by 0.5% (a lot compared to the effort) and saved over £250,000 per year for the company. - £250,000 from better error messages

## Client side progressively enhanced implementation

### HTML5

Not using HTML 5. novalidate. No required boolean. etc. See Heydon.

### Summary

?

### Inline

?

## Summary

We've covered a lot in this chapter. Other chapters will not cover the same ground. Perhaps some of the same topics will be discussed further or more specifically when necessary, but this serves as the foundation to which to design all other forms. What's interesting in the upcoming chapters is the different problems we need to tackle as designers and how we can solve those problems.

[^1]: Some *crazy* footnote definition.
[^2]: Some *crazy* footnote definition.