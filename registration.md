# A registration form

We're going to begin our forms adventure with a registration form. We'll use this simple form, to ahem, form the foundations on which to design more complex forms. But don't be fooled by its seemingly simple appearance. This registration form has much to be analysed, ripped apart and put back together again.

This chapter is going to cover a lot of ground. As we continue through proceeding chapters we will draw and build on the information covered here.

So here it is, a registration form:

![Image here](/etc/)

Here is the basic HTML code:

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

It contains four fields and a submit button. You'll also notice each text field has a label. Labels are where our analysis begins.

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

Up to now, we've done as much as we can to make our registration form easy to use. We have clear, always visible labels. We have an additional password hint that provides must-needed clarification.

We have a layout that works well on small and big screens and a design that caters for people who use keyboard. And we've considered the experience for people suffering from various visual or motor impairments.

However, sometimes users will make mistakes. And when this happens, it's our job to design an experience which makes fixing these mistakes as quick and as painless as possible.

Validation is the biggest design challenge we've faced so far and whilst it's not too hard, most sites get this wrong&mdash;either by not doing enough, or by doing too much. Or strangely by doing both at the same time.

To ensure users can fix errors easily, we'll need to tell them what's gone wrong and how to make it right. Our first consideration is *when to provide feedback*.

### When to validate

To design an inclusive experience we must first consider the experience without Javascript, which is more common than you may think[^]. Fortunately, when we consider the experience for people without Javascript we're left with one option, which is to validate `onsubmit`.

This is the beauty of designing to constraints. In this case, it means we can focus on designing the best form validation experience through submission to the server.

I've often found that&mdash;contrary to popular belief&mdash;making things work without Javascript often produces the best experience anyway. And on a recent large-scale project I was involved with, we only performed validation on the server, which worked very well.

This is not to say I advise avoiding client-side validation. I'm simply pointing out that the so-called *degraded* experience may be all that users need. And if we don't need to provide the enhancement, then we don't need to write as much code. This in turn has the following benefits:

1. We have less work to do
2. We have to send less code to the user (meaning a faster experience)
3. It has a wider (read: inclusive) reach by default.

In any case, we can't validate everything on the client. At some point we're going to have to interogate the database. For people to register successfully they will need to enter an email address that hasn't been used, for example.

By designing first without Javascript, not only do we reach a wider audience, but we also cover scenarios that we may have forgotten about had we jumped straight into the all-singing and all-dancing fancy-pants solution.

So in short, we'll validate forms onsubmit. This is something that forms do by default and is fully accessible out of the box. Validating on submit gives users consistency and familiarity whether Javascript is available or whether the form was sent through to the server.

I'll be providing a Javascript implementation[^] which behaves the same way as the server-side validation (which we'll discuss imminently). The only difference is that it does so without a server-side round trip (and subsequent page refresh). The benefit to the user is a faster fail and succeed experience.

If you decide to create your own script, be sure to attach the validation routine to the form's `submit` event and not onto the submit button's `click` event. The latter won't work for users who press *enter* when focussed on various form controls, creating a less inclusive experience.

#### Live validation

One question still remains though. When Javascript is available, we have an opportunity to provide what's commonly called *live validation*. The theory is that it's easier to fix errors as soon as they occur which sounds sensible. The thing is, live validation introduces more problems than it solves.

For entries that require a certain number of characters, the first keystroke will always constitute an invalid entry. This means users will be interrupted early and often.

We could wait until the user has entered enough characters before showing an error. But this means the only way in which a user will get feedback is after they have completed the field successfully which is pointless.

Alternatively, we could provide feedback when the user leaves the field (onblur) but this is too late. The user has already started to mentally prepare for (and to fill out) the next field.

Another problem with triggering the feedback onblur is that many people switch windows or use a password manager to assist in filling out forms. But leaving the field will cause an error to show prematurely when the user hadn't finished yet.

This is just a few of the associated problems. I've counted 5 others which you can read about in Live Validation Is Problematic[^] if you're interested.

As is often the case in our industry, live validation is a technique that causes more problems than it solves. Again, we'll stick to robust and simple techniques that help users. We'll validate on submit and leave the clever stuff to our competitors.

#### HTML5 validation

- TODO: Not using HTML5. novalidate. No required boolean. etc. See Heydon.

### Presenting errors

When the user submits a form with errors we'll need to inform the user in three separate ways:

1. Change the page title
2. Display a summary of the errors
3. Display each error message in-context of the field

#### 1. Page title

When a page loads, it's the page's title that is read out first. For this reason updating the title to read "The form has errors" (or words to that affect) will drastically help screen reader users.

I recently worked on a project that was thoroughly accessibility checked by RNIB, and we prepended "Retry - " to the page title. This tested well, but personally think we could have tried to testing something a little clearer.

Whilst it's arguably less useful for those without vision impairments, it could still be a benefit. For users that multi-task and switch between tabs, having that text on the erroneous tab could provide a vital notification of sorts.

For errors caught through client-side Javascript validation, the page title changing is less important (though you can still change the `title`). We'll provide more appropriate ways to inform people who use screen readers.

#### 2. Error summary

Next we're going to provide an error summary which looks like this:

![blah blah](/)

We'll position this at the top of the page, so that when a page refreshes the error will be shown without the user having to scroll. We can do the same thing, even when Javascript performs the validation (averting the page refresh) by bringing the error summary into view.

Conventionally, errors should be styled in some sort of red. So we won't be breaking this convention. To suppor tthose who can't see colour, we provide an icon. As is the case with most inclusive design approaches, this helps people who can see in colour too. A well-designed icon can be quicker to scan and interpret than reading long sentences. And long sentences on their own are often skipped.

Each error message is an internal anchor that sets focus to the field. Here's the HTML code:

```html
<div class="errorSummary">
  <h2 tabindex="-1">Please fix the 4 errors</h2>
  <ul>
    <li><a href="#emailaddress">Email address cannot be empty</a></li>
    <li><a href="#password">Password cannot be empty</a></li>
  </ul>
</div>
```

The `tabindex` attribute allows Javascript to set focus to the element when an error is caught. This means we can bring the error summary into view. When focus is set the heading will be read out prompting the user to take informative action.

Without Javascript, and when there are errors, the summary will be rendered on the server. When the page is loaded without errors, this element should be hidden. To do this the server will need to apply an extra class of `errorSummary-isHidden`.

By doing this, we allow Javascript to reuse the same component (and the same location on the screen). This is important because if the server catches an error, then the Javascript validation will clear it appropriately. Otherwise we risk two error summary components being presented at the same time.

The error messages themselves are of utmost importance. In fact being able to provide custom error messages for one particular e-commerce site increased conversion by 0.5%. This deceivingly small percentage actually equates to over £250,000 per year[^].

This means spending time designing the content of the errors is one of the best investments we can make for our users. If an erroneous field simply said "Email address invalid, please fix", this is obviously unhelpful.

Now this is the extreme of the situation, but subtle versions of this happen all over the Internet. If the Email address is invalid, then we need to explain to the user why that is. Perhaps the email address is already in-use. Or perhaps it contains invalid characters. We need to be specific and design our messages accordingly.

#### 3. Provide in-context error messages

The title tag and error summary panels go a long way to help different users and different use cases but we're not done yet. When the user actively starts to fix errors&mdash;particularly when there are multiple errors on longer forms&mdash;they'll have to flip-flop between the form and the error summary.

We can do better than this. We can place the error messages beside each field. However, we can't just bung the text in a paragraph because people using a screen reader in forms mode won't be aware of such information.

As we know already, labels are read out as the user enters each control. So we can piggyback this well supported functionality by injecting error messages inside the labels themselves. This way when the user focuses on the erroneous email control, for example, they will hear something like "Email address. Your email address is invalid."

We'll be discussing how to handle groups of fields, such as radio buttons, in upcoming chapters. The reason I bring this to your attention, is that when we inject errors to a group of fields, we can't inject the message into the label.

```html
<div class="field">
  <label for="blah">
    <span class="field-label">Email address</span>
    <span class="field-error">Your email address is invalid</span>
  </label>
</div>
```

The classes themselves act as hooks for styling and behaviour. For the coders amongst you, checking the demo page will reveal all.

## Summary

So we did it. From scratch we have navigated through many of the fundamental design challenges that most forms need to consider. In many ways this chapter has explained an equal amount of what not to do, as it has what it is we should do. In short:

1. Always use a clear, readily accessible, human-readable label.
2. Avoid any rule that inhibits rule number one.
3. Strive to reduce the amount of questions we ask of our users.
4. Use the right control for the job as many devices and browsers will enhance the experience for free.
5. Validation should be executed on submit. Upon execution there are 3 parts of the page to update to ensure a fully inclusive, and easy path to remedying any errors.

Whilst we have covered a lot of ground in this chapter, this is lots more to discuss. The foundations have been laid, and with that we will up the ante and face even tougher challenges. In doing so we can explore some of the more interesting techniques at our disposal.

## Footnotes

[^1]: Placeholder article
[^2]: https://www.smashingmagazine.com/2015/12/passphrases-more-user-friendly-passwords/
[^3]: Number 5 in https://uxplanet.org/10-rules-for-efficient-form-design-e13dc1fb0e03#.r3ic58br2
[^3]: https://medium.com/ux-ui-ia-case-studies/masked-passwords-security-questions-captcha-and-other-unusable-security-1f018ad01378#.w8ws8yo23)
[^4]:https://medium.com/@cjforms/the-idea-that-left-aligned-labels-have-slower-completion-times-is-incorrect-e1461f47242b#.dvl3en9g4
[^5]:https://uxdesign.cc/design-better-forms-96fadca0f49c#.iy0c5in6p
[^6]:http://www.90percentofeverything.com/2009/02/16/karl-sabino-on-the-roi-of-well-designed-error-messages/
