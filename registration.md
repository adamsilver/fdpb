# A registration form

We're going to begin our forms adventure with a registration form. We'll use this simple form, to ahem, form the foundations on which to design more complex forms. But don't be fooled by its seemingly simple appearance. This registration form has much to be analysed, ripped apart and put back together again.

This chapter is going to cover a lot of ground. As we continue through proceeding chapters we will draw and build on the information covered here.

So here it is, a registration form:

![Image here](/etc/)

Here is the HTML code to build it

```html
<form>
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

It contains four fields and a submit button. You'll also notice each text field has a label. Labels are where we will begin.

## Labels

The first thing to know is that each field needs an associated label. This is because:

- sighted users will be able to see the instructions;
- visually-impaired users will hear the instructions when using a screen reader; and
- motor-impaired users will find it easier to select a field thanks to the larger hit area. This is because clicking the label will move focus to its related control.

We might be tempted to omit labels for particular forms in order to save space but this just about the worst thing we can do for users.

Code wise, the way in which to *connect* an input to a label, is with the `id` and `for` attributes. They must be unique and they must match.

We'll talk about words later, but ensuring that every field has a well written, readily accessible, always visible label is perhaps half the battle with simple forms like this one.

## Placeholders and hints

Since placeholders came along, we've adopted them as means of storing hints. Their appeal lies in their minimal aesthetic and the fact they save space.

Some designers go one one step further and replace labels with placeholders. Either way, the placeholder is problematic for many reasons.

1. The placeholder disappears when the user types. Once it’s gone it’s hard to remember.
2. Placeholder text is often mistaken for a value, meaning users skip them and are subsequently shown an error.
3. They lack sufficient contrast, making them hard to read for people lacking perfect vision which is a huge amount of people[^].

I've actually counted 13 problems altogether, so if you're interest check out the reference in Placeholders Are Problematic[^].

Some people ask me if it’s okay to use a placeholder in addition to a label. I say that if the hint is valuable to the user, we should make it easy-to-read and readily accessible. Placeholders don't meet these requirements.

Others say that the placeholder is just an enhancement and not essential to the user. I say that if the hint isn’t essential then don’t include it. Afterall, content is not an enhancement.

In the case of placeholders, minimal doesn't mean simple. If we do need to provide an additional hint we should provide one outside of the field.

Just because we can provide a hint doesn't mean we should. Most of the fields in our registration form are self-explanatory. If in-doubt, test with your users.

However, many sites ask for a complex set of rules for the password. Typically having to conform to the following rules:

- Must be at least 8 character
- Must include at least one uppercase and lowercase letter
- Must include at least one number.

Where possible, we should avoid asking users to create complex passwords because they are hard to remember. Instead you may decide to take a look at passphrases[^2] which is probably a chapters worth of analysis in its own right. For our form, we'll stick to the more common password rules as above.

In this case a hint, in addition to the label is helpful. It should help many users avoid having to to fix errors later on when the validation routine is invoked.

![Image here](/etc/)

## Floating labels

When I inform people of the placeholder problem, they often tell me that floating labels is the answer. In actual fact, they come with many of the same problems as placeholders but with a few additions:

1. There is no actual space for a hint, because the hint and the label are one and the same.
2. The labels are often too small. If we made them bigger we would need to leave a bigger space for the label to float into. In which case they don't save space.
3. The animation affect is distracting and disorientating, particularly at a time when we're asking users to engage.

Decluttering a UI is a noble goal. But only when we declutter the superfluous; not the essential. Labels are essential, and in some cases as we've just discussed so are hints.

Employing a pattern that is both problematic and constraining at the same time, is not a recipe we'll be following to design the forms presented in this book.

In fact, this book is about building forms that work. That don't drive users crazy. And that have as little friction as possible. For these reasons, we'll leave floating labels for the creatives out there.

## Do we really need it?

When we ask users to register, they need a reason to do so. Nobody wants to use the forms we put in front of them, they just want the outcome. If we're building an ecommerce form, the outcome might be one or all of the following:

1. can buy faster next time, as they don't have to type the same details over and over.
2. can track their order, without having to make a phone call.
3. will receive 10 percent off their next order.

As the first chapter is dedicated to registration, we're going to assume we need it. Which brings us nicely to whether we need to ask for the *fields* within it.

GDS has the following question protocol:

*Before you start, make a list of all the information you need from your users. Only add a question if you know:*

-*that you need the information to deliver the service*
-*why you need the information*
-*what you’ll do with it*
-*which users need to give you the information*
-*how you’ll check the information is accurate*
-*how to keep the information up to date and secure*

Every time we ask the user for another piece of information, we're asking them for another piece of their time. Time is the most precious resource on earth. We can't get it back and so we need to value user's time like we would value our own.

Do we really need to ask for their first and last name? If we don't need the information, or we don't need that information at this point in time then we probably shouldn't be asking for it, right *now*. As with most things in life, it *depends*.

Let's assume we can remove the first and last name fields, this is a significant improvement as it reduces the size of the form by half. But can we do better.

## No password registration

Medium.com have implemented a no password sign in[^]. They leverage the security of email accounts by sending the user a special login link. This would result in our registration form slimming down to an email field.

However, we may be over-simplifying our registration form. For one, people are familiar with an email address and password (although that is not a reason in itself to avoid improving the experience of course).

For two, people who know their password, or use a password manager, have to flip-flop between the site and their email account, which becomes an unnecessary source of friction.

This goes to show that designing a form in isolation is not a sensible way to go. Which is why following GDS's question protocol is a good place to start. And designing journeys (as opposed to screens) is just as important.

The point of this discussion is not to provide a definitive answer as to how many fields a registration form should have. What's most important is that we need to rigorously ask ourselves why we are asking users for information in the first place and how we may be able to improve an experience by simply not asking in the first place.

We'll keep the password field in our registration form which will help to keep the experience familiar and straightforward for most users. Moving away from convention is something we should do through testing. Otherwise we may end up exchanging one set of problems for another.

## An email field

Like most sites, our registration form asks users to enter an email address. HTML5 gave us a dedicated input type that improves the experience for people using browsers that support it. Nowadays that's most browsers.

On many touch screen devices, when the user focuses on the email address field, a keyboard will popup. When we use `[type=email]` it contains readily accessible *@* and *.* characters which users need to type in an email address. Here's a screenshot:

![Put image here of email keyboard]()

## A password field

A password input will show circles for each character typed. This is a security measure so that if someone is looking over your should they can't see your password. However, this doesn't happen very often.

With this being the case, fixing typos when the value is obscured, makes usability decline. It's often easier to delete the whole entry and start again, than it is to count the circles you know you typed correctly.

For this reason we should at least enhance the field with a password reveal[^3]. It's no less secure and it's significantly easier to use because the user can check what they typed.

Some browsers provide this functionality out of the box and as Heydon says:

> Note that Internet Explorer and Microsoft Edge provide this functionality natively, using an interactive 'eye' icon associated with the <code>::ms-reveal</code> pseudo-class. Since we've provided our own (cross-browser) solution, it would be wise to suppress this feature:

	input[type=password]::-ms-reveal {
	  display: none;
	}

The password reveal also stops the need to use an additional "confirm password" field which further reduces work on the user's side.

You might also be thinking that we should just remove the masking altogether. But as we've already discussed moving away from convention and familiarity can breed fear into people that already fear issues of security on the Internet. It's for this reason that we offer the reveal as an enhancement, instead of something that you can *turn off*.

## Required fields

In *Do we really need it?*, we made sure that only ask users for what is necessary. For this reason, all fields in our registration form our required. Most users in fact expect all fields to be required anyway. And it's visually and audibly noisey to specify required fields. When we explore other forms in later chapters we'll talk about this topic more deeply.

<!-- highligh, a highlight is to show something different from the norm. Optional fields are different from the norm so mark those. Jessica Enders.-->

## Label and input position

There is no definitive answer as to the best place to position labels (and legends) in relation to their controls. However, if we're designing inclusive experiences on the web, then this implies we're building responsive websites. This in turn means that we'll want our forms to be friendly on small screens. For this reason alone we'll choose to use labels that sit above their controls.

Not only do they work on small screens, but they make it easier for different sized labels (and legends) and localized versions to fit more easily within the UI.

## Focus styles

Browser, by default apply a focus style to the active element. This also applies to form fields. This behaviour lets users know where they are, which is particularly helpful for users using a keyboard.

Some designers despise the aesthetic of the browser defaults, because it doesn't match the brand or some such. And in the mist of their hatred, they ask the developer to remove it. That would be okay if they replaced it with something better. But quite often they don't.

There is no reason why we can't replace the outline with our own style and colour. The idea would be to make it clear regardless of colour for those that suffer with vision impairments. To do this, add a border or outline that makes it thicker. Here's an example:

![blah]()

## Submit buttons

You would think that a button is such a simple thing to design, that getting it right is a no brainer. And yet, we often forget that the *aggregation of marginal gains* goes a long way.

The first thing to note about buttons is that they aren't links. Links typically have special positioning (such as a header) and underlines to denote their meaning. And they also have a hand cursor (also known as a pointer).

As buttons aren't links, we shouldn't give them any of this treatment. They should look like a button in order to help users realise they are different. Fortunately buttons have a strong affordance anyway, and so we don't need to mess with them too much.

We should align the button to left, inline with the fields themselves. It doesn't make much sense to position the fields to the left, and the submit button to the right. If labels sit atop of the field, the vertical rhythm goes straight down providing an intuitive expectation that what is needed will appear directly below.

Additionally, keeping the button on the left inline with the fields will help users who zoom&mdash;a right-aligned button would appear off screen.

We'll discuss copy shortly, but let's take the opportunity to ensure the text on a button is descriptive of the action the user is taking. In our case "Register" or "Sign up" is meaningful and terse. The exact words might need to align more with your brand and tone of voice but don't exchange clarity for cuteness.

Whilst each of these things seem small in isolation, it's the combination that makes a difference for users. Most sites don't care about these details, but we're not designing those sites.

## Validation

Up to now, we've done as much as we can to make this registration form easy to use. We have clear, always visible labels. We have an additional password hint that explains what we expect the user to type.

We have a layout that works well on small and big screens and a design that caters for people who use keyboard and that also may suffer from various visual or motor impairments.

But, sometimes people don't read all the instructions and sometimes people make mistakes. When this happens we need to help the user be on their way. This is essential.

Validation is the biggest design challenge we've faced so far and whilst it's not that hard, many sites get this wrong by either doing too little, or doing too much too early.

To ensure users can fix their errors easily, we'll need to tell them what's gone wrong and how to fix it. But before all that we need to decide when we should validate the form.

### When to validate

To design an inclusive experience we must consider the experience without Javascript first, which is more common than you may at first think[^]. Fortunately, when we consider the experience for people without Javascript we find there is only one option which is onsubmit.

I've often found that making things work without Javascript to the best of my ability can quite often provide a great experience period. And in fact in a recent large-scale project I was involved with, we performed validation on the server only. And it worked really well.

This is not to say client-side validation is bad. It can be really beneficial. The point is when we do the basics first, we can find ourselves needing to provide the basics only. And when we do this, it makes for a faster experience. Less Javascript means we're quicker to market, less going down the wire, and less opportunity for failure.

Additionally, we can't validate everything on the client. At some point we're going to have to interogate the database. For our registration to pass validation it will need to check that the user didn't enter someone elses email address, for example.

By designing with out Javascript to begin with, not only do we reach a wider audience, but we also cover scenarios that we may have forgotten about had we jumped straight into the all-singing and all-dancing fancy-pants solution.

So, we're going to be validating our form on submit. This is something that forms do by default and is fully accessible out of the box. Validating on submit gives users consistency and familiarity whether Javascript kicks in and validates formatting on the client, or whether it was passed through to the server for something more.

When Javascript kicks-in we have an opportunity to provide live validation. Live validation (also known as inline validation or live feedback) enables users to know whether what they type into a text box is valid as they type. Heydon Pickering talks about this in Inclusive Design Patterns:

> Some fancy form validation scripts give you live feedback as you type your text entries, letting you know whether what you type is valid as you type it. This can become very difficult to manage. For entries that require a certain number of characters, the first few keystrokes always going to constitute an invalid entry. So, when do we send feedback to the user and how frequently?

> Not wanting to be the overbearing restaurant waiter continually interrupting customers to check in with them, we didn't flag errors on first run. Instead, only where errors are present after attempted submission do we begin informing the user.

> Once the user is actively engaged in correcting errors, I think it's helpful to reward their efforts as they work.

I had never considered the hybrid approach, but after talking with Heydon we agreed that this is still far from ideal. The problem still stands that when the user is fixing an error they are interupted. We could validate onblur (as they leave the field) but this is too late&mdash;the user has left that field and is mentally preparing for the next one.

As much as live validation is touted to be a great validation pattern, much like floating labels, it's a solution that is nigh on impossible to do well. Any benefits would be outweighed by the problems it introduces.

Again, we'll stick to robust and simple techniques that help users. We'll validate on submit.

### Presenting errors

If the user submits errors, we'll need to show users an error. To do this we'll need to:

1. Change the page title
2. Provide an error summary
3. Provide inline errors

#### 1. Changing the page title

First we're going to want to change a website's `<title>` so after refresh, a screen reader user will know the form has errors. Most visually enabled users won't notice this but that's just fine.

#### 2. Provide an error summary

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

#### Provide inline errors

If the user has more than one error, then having to switch between the summary and the field is some friction that we can remove for users by providing an in-context error message.

And for which gets read out as the user enters form's mode again.

[Placement/position](http://adrianroselli.com/2017/01/avoid-messages-under-fields.html)

- Content

- Code

- ?

### Server side implementation

#### Keep populated values

When the page refreshes must populate values as to not make users have to type it in again. Annoying.

#### Summary

?

#### Inline

?

## Client side progressively enhanced implementation

#### HTML5

Not using HTML 5. novalidate. No required boolean. etc. See Heydon.

#### Summary

?

#### Inline

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

[^1]: Placeholder article
[^2]: https://www.smashingmagazine.com/2015/12/passphrases-more-user-friendly-passwords/
[^3]: https://medium.com/ux-ui-ia-case-studies/masked-passwords-security-questions-captcha-and-other-unusable-security-1f018ad01378#.w8ws8yo23)