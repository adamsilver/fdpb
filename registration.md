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

Code wise, the way in which to *connect* an input to a label, is with the `id` and `for` attributes. They must be unique and they must match. Without this the label won't work for visually-impaired and motor-impaired users.

We might be tempted to omit labels for particular forms in order to save space but this is probably the worst thing we can do for users.

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

Now where possible, we should avoid asking users to create complex passwords because they are hard to remember. Instead you may decide to take a look at passphrases[^2]. But for our form, we'll stick to the more common password pattern.

And in this case a hint, in addition to the label is helpful. It should help many users avoid having to to fix problems later when validation kicks in, and only then being told what it takes to meet these complex requirements.

This is how it all looks so far:

![Image here](/etc/)

## Floating labels

When I inform people of the placeholder problem, they often tell me that floating labels are the answer. In actual fact, they come with many of the same problems as placeholders but with a few additions:

1. There is no actual space for a hint, because the hint and the label are one and the same.
2. The labels are often too small. If we made them bigger we would need to leave a bigger space for the label to float into. In which case they don't save space.
3. The animation affect is distracting and disorientating, particularly at a time when we're asking more from our users.

Decluttering a UI is a noble goal. But only when we declutter the superfluous; not the essential. Labels are essential, and in some cases as we've just discussed so are hints.

Employing a pattern that is both problematic and constraining at the same time, is not a recipe we'll be following to design the forms presented in this book.

In fact, this book is about building forms that work. That don't drive users crazy. And that have as little friction as possible. For these reasons, we'll leave floating labels for the creatives out there.

## Do we really need it?

When we ask users to register, they need a reason to do so. Nobody wants to use the forms we put in front of them, they just want the outcome. If we're building an ecommerce form, the outcome might be one or all of the following:

1. Users can buy faster next time, as they don't have to type the same details over and over.
2. Users can track their order, without having to make a phone call.
3. Users will receive ten percent off their next order.

As the first chapter is dedicated to registration, we're going to assume we need it. Which brings us nicely to whether we need to ask for the *fields* within the form.

GDS has the following question protocol:

*Before you start, make a list of all the information you need from your users. Only add a question if you know:*

-*that you need the information to deliver the service*
-*why you need the information*
-*what you’ll do with it*
-*which users need to give you the information*
-*how you’ll check the information is accurate*
-*how to keep the information up to date and secure*

Every time we ask the user for another piece of information, we're asking them for another piece of their time. Time is the most precious resource on earth. We can't get it back and so we need to value user's time like we would value our own.

Does the registration form need to ask for first and last name?

If we don't need the information, or we don't need that information at this point in time then we probably shouldn't be asking for it, right *now*. As with most things in life, it *depends*.

Let's assume we can remove the first and last name fields, this is a significant improvement as it reduces the size of the form by half. But can we do better? Let's find out in the next section.

## No password registration

Medium.com have implemented a no password sign in[^]. They leverage the security of email accounts by sending the user a login link. This would result in our registration form slimming down to a single email field.

However, we may be over-simplifying our registration form. For one, people are familiar with an email address and password (although that is not a reason in itself to avoid improving the experience of course).

For two, people who know their password, or use a password manager, have to flip-flop between the site and their email account, which becomes an unnecessary source of friction.

This goes to show that designing a form in isolation is not a sensible way to go. Which is why following GDS's question protocol is a good place to start. And designing journeys (as opposed to screens) is just as important.

The point of this discussion is not to provide a definitive answer as to how many fields a registration form should have. What's most important is that we need to rigorously ask ourselves why we are asking users for information in the first place and how we may be able to improve an experience by simply not asking at all.

We'll keep the password field in our registration form which will help to keep the experience familiar and straightforward for most users. Moving away from convention is something we should do through testing. Otherwise we may end up exchanging one set of problems for another.

## Email field

Like most sites, our registration form asks users to enter their email address. HTML5 gave us a dedicated input type that improves the experience for people using browsers that support it. Nowadays that's most browsers.

On many touch screen devices, when the user focuses on the email address field, a keyboard will popup. When we use `[type=email]` it contains readily accessible *@* and *.* characters which users need to type in an email address. Here's a screenshot:

![Put image here of email keyboard]()

People using a non-supporting browser will get a standard text field and a standard keyboard. No big deal. We'll be embracing Progressive Enhancement throughout this book.

## Password field

A password input will show circles for each character typed. This is a security measure so that if someone is looking over your should they can't see your password. 

However, this, in reality doesn't happen often. With this being the case, fixing typos when the value is obscured, makes for a poor experience. It's often easier to delete the whole entry and start again, than it is to figure out which circles you're confident are correct.

For this reason we should at least enhance the field with a password reveal[^3]. It's no less secure and it's significantly easier to use because the user can check what they typed.

The password reveal also stops the need to use an additional "confirm password" field which further reduces work on the user's side.

Some browsers provide a password reveal natively. So if you write your own cross-browser solution, then you'll want to supress this functionality as follows:

	input[type=password]::-ms-reveal {
	  display: none;
	}

You might also be thinking that we should just remove the masking altogether. But as we've already discussed moving away from convention and familiarity can breed fear into people that already fear issues of security on the Internet. It's for this reason that we offer the reveal as an enhancement, instead of something that you can *turn off*.

## Label text

Whilst we've discussed the importance of labels, we haven't addressed the text inside the label itself. This is vital. What may seem obvious may not always be.

I've often seen email address fields labelled as "username". But if the username is always their email address we should say so. So our email field should have "Email address" for a label.

What's not quite so obvious is the text we should use for the password field. The label "Password" is potentially missing some clarity. It could suggest that the user needs to type a password they already possess. This is both incorrect and bad for security.

It might also subtly suggest the user is already registered. And they might forget what they're doing and think they're signing in. I don't know what is the case for your form, but it might be better to have "Choose password" as a label. This reinforces the fact they are a) they are signing up and b) that they should create a new password.

Again, the point of this discussion is not to provide the definitive answer on the correct label for your form. The point is to think about the importance of labelling. Friends of mine say "Content is the user experience" and this is hard to argue with.

## Required fields

In *Do we really need it?*, we made sure that only ask users for what is necessary. For this reason, all fields in our registration form our required. Most users in fact expect all fields to be required anyway. And it's visually and audibly noisey to specify required fields. When we explore other forms in later chapters we'll talk about this topic more deeply.

## Positioning

It's easy to find research that shows that top-aligned labels (and legends) convert best[^]. But then if I really had to I could find some research to show something different. With that said, if we're designing inclusive experiences on the web, this implies we're building responsive websites. This in turn means that we'll want our forms to be friendly on small screens.

For this reason alone we'll choose to use labels that sit above their controls.  Not only do they work on small screens, but they make it easier for different sized labels (and legends) and localized versions to fit more easily within the UI.

## Focus styles

Browsers, by default apply a focus style to the active element. This also applies to form fields. This behaviour lets users know where they are, which is particularly helpful for users using a keyboard.

Some designers despise the aesthetic of the browser defaults, because it doesn't match the brand or some such. And in the mist of their hatred, they ask the developer to remove it. That would be okay if they replaced it with something better. But quite often they don't.

There is no reason why we can't replace the outline with our own style and colour. The idea would be to make it clear regardless of colour for those that suffer with vision impairments. To do this, add a border or outline that makes it thicker. Here's an example:

![blah]()

## Submit buttons

You would think that designing a button is far from taxing. And yet, we often forget that the *aggregation of small gains* goes a long way.

The first thing to note about buttons is that they aren't links. Links typically have special positioning (such as a header) and underlines to denote their meaning. And they also have a hand cursor (also known as a pointer).

As buttons aren't links, we shouldn't give them any of this treatment. They should look like a button in order to help users realise they are different. Fortunately buttons have a strong affordance anyway, and so we don't need to mess with them too much.

We should align the button to left, inline with the fields themselves. It doesn't make much sense to position the fields to the left, and the submit button to the right. If labels sit atop of the field, the vertical rhythm goes straight down providing an intuitive expectation that what is needed will appear directly below.

Additionally, keeping the button on the left inline with the fields will help users who zoom&mdash;a right-aligned button would disapppear off screen.

It's vital that the button text is descriptive of the action being taken. In our case "Register" or "Sign up" is meaningful and terse. The exact words might need to align more with your brand and tone of voice but don't exchange clarity for cuteness.

Whilst each of these things seem small in isolation, it's the combination that makes a difference for users. Most sites don't care about these details, but fortunately for us we're not working on our competitor's sites.

## Validation

Up to now, we've done as much as we can to make this registration form easy to use. We have clear, always visible labels. We have an additional password hint that provides must-needed clarification.

We have a layout that works well on small and big screens and a design that caters for people who use keyboard. And we've considered the experience for people suffering from various visual or motor impairments.

But, people don't always read the instructions and sometimes people make mistakes. When this happens it's essential we provide an experience that helps users fix their mistakes.

Validation is the biggest design challenge we've faced so far and whilst it's not that hard, most sites get this wrong by either doing too little, or doing too much. Or oddly, by doing both at the same time.

To ensure users can fix their errors easily, we'll need to tell them what's gone wrong and how to make it right. The first question we need to answer is *when to provide feedback*.

### When to validate

To design an inclusive experience we must consider the experience without Javascript first, which is more common than you may think[^]. Fortunately, when we consider the experience for people without Javascript we're left with one option, which is to validate `onsubmit`. 

That's the beauty of designing within constraints. It means we can strive to excel within those bounds. Afterall, some wise people have said *we can't be creative without constraint*.

I've often found that&mdash;contrary to popular belief&mdash;making things work without Javascript often produces the best experience anyway. And on a recent large-scale project I was involved with, we only performed validation on the server, which worked very well.

This is not to say I advise avoiding client-side validation. I'm simply pointing out that by providing the so-called *degraded* experience may be all that users need. And if we don't need to provide the enhancement, then we don't need to write as much code. This in turn has the following benefits:

1. We have less work to do
2. We have to send less code to the user (meaning a faster experience)
3. It has a wider (read: inclusive) reach by default.

In any case, we can't validate everything on the client. At some point we're going to have to interogate the database. For people to register successfully  they will need to enter an email address that hasn't been used, for example.

By designing without Javascript to begin with, not only do we reach a wider audience, but we also cover scenarios that we may have forgotten about had we jumped straight into the all-singing and all-dancing fancy-pants solution.

So in short, we'll validate forms onsubmit. This is something that forms do by default and is fully accessible out of the box. Validating on submit gives users consistency and familiarity whether Javascript is available or whether the form was sent through to the server.

I'll be providing a Javascript implementation[^] which behaves the same way as the server-side validation (which we'll discuss imminently). The only difference is that it does so without a server-side rount trip (and subsequent page refresh). The benefit to the user is a faster fail and succeed experience.

If you decide to create your own script, be sure to attach the validation routine to the form's `submit` event and not onto the submit button's `click` event. The latter won't work for users who press *enter* when focussed on various form controls, creating a less inclusive experience.

One question still remains though. When Javascript is available, we have an opportunity to provide what's commonly called *live validation*. Live validation enables users to know whether what they type into a text box is valid as they type. 

Heydon Pickering talks about this in Inclusive Design Patterns:

> Some fancy form validation scripts give you live feedback as you type your text entries, letting you know whether what you type is valid as you type it. This can become very difficult to manage. For entries that require a certain number of characters, the first few keystrokes is always going to constitute an invalid entry. So, when do we send feedback to the user and how frequently?

> Not wanting to be the overbearing restaurant waiter continually interrupting customers to check in with them, we didn't flag errors on first run. Instead, only where errors are present after attempted submission do we begin informing the user.

> Once the user is actively engaged in correcting errors, I think it's helpful to reward their efforts as they work.

This hybrid approach is certainly better but Heydon and I agreed that it's still not entirely prevented the problem. That is, when a user is fixing an error they are still interrupted either too early or too late. Too early by validating onkeypress before the user has finished, or too late by validating when they have left the field altogether onblur.

As often as live validation is touted as a must-have design feature, much like the floating label pattern, it's a solution that introduces way more problems than it solves.

Again, we'll stick to robust and simple techniques that help users. We'll validate on submit and leave the clever stuff to our competitors.

### Presenting errors

When the user submits an erroneous form we'll need to do three things:

1. Change the page title
2. Display a summary of the errors
3. Display each error message in-context of the field

Each of these tasks involves using the correct HTML to ensure the experience is inclusive and friendly no matter people's ability and preference (or lackthereof).

#### 1. Change the page title

When a page loads, it's the page's title that is read out first. For this reason updating the title to read "The form has errors" (or words to that affect) will drastically help screen reader users.

Whilst it's arguably less useful for those without vision impairments, it could still be a benefit. For users that multi-task and switch between tabs, having that text on the erroneous tab could provide a vital notification of sorts.

#### 2. Provide an error summary

Next we're going to provide an error summary which looks like this:

![]()

And is coded as follows:

```html
<div id="errorSummary" class="errorSummary" aria-live="assertive" role="alert">
    Stuff
</div>
```

Notes:

1. It's positioned at the top
2. It uses colour in addition to an icon for those with colour blindness.
3. Contains links to each field with an error
4. Uses live region which is important when Javascript kicks in.
5. We need to talk about the error messages themselves.

We'll position this at the top of the page, so that when a page refreshes the error will be shown without the user having to scroll. We can do the same thing, even when Javascript performs the validation (averting the page refresh).

> Conventionally, errors are denoted with a red coloration, so it's advisable to give the message box a red border or background. However, you should be wary of red being the only visual characteristic that classifies the message as an error. To support those who cannot see color and screen reader users at the same time, we can prepend a warning icon containing alternative text.

This icon is vital for those with colour impairments but it's also helpful for those with perfect vision. It's much easier to scan for an icon than it is to read through some text explaining that there is an error. Is it is with inclusive design, what you do for one person often helps everyone else too.

The error summary contains links to each erroneous field.

> When the form's submission event is suppressed, the live region is populated, switching its display state from none implicitly to block. This population of DOM content simultaneously triggers screen readers to announce the content, including the prepended alternative text: "Error: please make sure all your registration information is correct".
> The advantage of declaring the presence of errors using a live region is that we don't have to move the user in order to bring this information to their attention. Commonly, form errors are alerted to the user by focusing the first invalid form field. This unexpected and unsolicited shift of position within the application risks disorientating the user. In our case, the user remains focused on the submit button and is free to move back into the form to fix the errors when ready.

#### Provide inline errors

If the user has more than one error, then having to switch between the summary and the field is some friction that we can remove for users by providing an in-context error message. And which gets read out as the user enters form's mode again.

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

## Summary TODO

We've covered a lot in this chapter. Other chapters will not cover the same ground. Perhaps some of the same topics will be discussed further or more specifically when necessary, but this serves as the foundation to which to design all other forms. What's interesting in the upcoming chapters is the different problems we need to tackle as designers and how we can solve those problems.

---

With regrads to errors, the affect on UX is huge.

> A little improvement of the error messages on an e-commerce website increased completed purchases by 0.5% (a lot compared to the effort) and saved over £250,000 per year for the company. - £250,000 from better error messages.[^]

[^1]: Placeholder article
[^2]: https://www.smashingmagazine.com/2015/12/passphrases-more-user-friendly-passwords/
[^3]: Number 5 in https://uxplanet.org/10-rules-for-efficient-form-design-e13dc1fb0e03#.r3ic58br2
[^3]: https://medium.com/ux-ui-ia-case-studies/masked-passwords-security-questions-captcha-and-other-unusable-security-1f018ad01378#.w8ws8yo23)
[^4]:https://medium.com/@cjforms/the-idea-that-left-aligned-labels-have-slower-completion-times-is-incorrect-e1461f47242b#.dvl3en9g4
[^5]:https://uxdesign.cc/design-better-forms-96fadca0f49c#.iy0c5in6p
[^]:http://www.90percentofeverything.com/2009/02/16/karl-sabino-on-the-roi-of-well-designed-error-messages/
