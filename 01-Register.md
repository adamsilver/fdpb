# A Registration Form

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

Nobody wants to use the forms we put in front of them, they just want the outcome. If, for example, we're asking users to register for an online shop, the appeal may lie in one or all of the following selling points:

1. Users can buy faster next time, as they don't have to type the same details over and over.
2. Users can track their order, without having to make a phone call.
3. Users will receive ten percent off their next order.

As the first chapter is dedicated to registration, we're going to assume we need it. But the fields *within* the form, are still very much *up for grabs*.

Government Digital Services (GDS) has something called the Question Protocol. It states that *before you start [designing a form], make a list of all the information you need from your users. Only add a question if you know:*

-*that you need the information to deliver the service*
-*why you need the information*
-*what you’ll do with it*
-*which users need to give you the information*
-*how you’ll check the information is accurate*
-*how to keep the information up to date and secure*

Every time we ask for information, we're asking users to spend time doing so. Time is the most precious resource on earth. We can't get it back and so we need to value user's time like we would value our own.

In which case, *do we need to ask for their name?* If *not*, or even if we don't need it at *this* point in time, we shouldn't be asking for it within the registration form.

Let's remove the first and last name fields, which importantly reduces the size of the form by half. Excellent. But we can do better.

## No password registration

Medium.com have implemented a no password sign in[^]. They leverage the security of email accounts by sending the user a login link. This would result in our registration form slimming down to a single email field.

However, we may be over-simplifying things. For one, people are familiar with an email address and password (although that is not a reason in itself to avoid improving the experience of course).

For two, people who know their password, or use a password manager, have to switch between the site and their email account, which becomes an unnecessary source of friction.

This goes to show that designing a form in isolation is not a sensible way to go. Which is why the Question Protocol is something we should lean on. And designing journeys (as opposed to screens) is just as important.

The point of this discussion is not to provide a definitive answer as to how many fields a registration form should have. What's more important is that we need to rigorously ask ourselves why we are asking users for information in the first place and how we may be able to improve an experience by simply not asking users at all.

We'll keep the password field in our registration form which will help to keep the experience familiar and straightforward for most users. Moving away from convention is something we should do through testing. Otherwise we may end up exchanging one set of problems for another.

## Email field

Like most sites, our registration form asks users to enter their email address. HTML5 gave us a dedicated input type that improves the experience for people using supporting browsers&mdash;nowadays that's most of them.

The benefit of using this input type is that on-screen keyboards will include readily accessible *@* and *.* characters which email addresses must contain.

![Put image here of email keyboard]()

People using a non-supporting browser will get a standard text field (`type="text"`) and a standard keyboard. This is the beauty of progressive enhancement, which is an important technique that we'll be using throughout this book.

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

## Required fields

In *Do we really need it?*, we made sure that we only ask users for what is necessary. For this reason, all fields in our registration form are required. Most users in fact expect all fields to be required anyway. And it's visually and audibly noisey to specify required fields. Fortunately, in this case we don't need to tackle this problem here. But we will do in upcoming chapters.

## Positioning

TODO: The Definitive Guide To Form Label Positioning by Jessica Enders

It's easy to find research that shows that top-aligned labels (and legends) convert best[^]. But, if we really had to, we could find research to the contrary. With that said, if we're designing inclusive experiences on the web, this implies we're building responsive websites. This in turn means that we'll want our forms to be friendly on small screens.

For this reason alone we'll choose to use labels that sit above their controls.  Not only do they work on small screens, but they make it easier for different sized labels (and legends) and localized versions to fit more easily within the UI.

## Focus styles

By default, browsers apply a focus style to the active element. This also applies to form fields. This behaviour lets users know where they are, which is particularly helpful for keyboard users.

Some designers dislike the aesthetic of the browser defaults, because it *doesn't match the brand* or some such. In the mist of their dislike, they ask the developer to remove it. This is okay as long as they replace it.

I've found that replacing it with a coloured border or outline (that has a different thickness to the default) makes this beautiful and functional at the same time.

![blah]()

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

- TODO: Not using HTML5. novalidate. No required boolean. etc. See Heydon.

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

## Footnotes

[^1]: Placeholder article
[^2]: https://www.smashingmagazine.com/2015/12/passphrases-more-user-friendly-passwords/
[^3]: Number 5 in https://uxplanet.org/10-rules-for-efficient-form-design-e13dc1fb0e03#.r3ic58br2
[^3]: https://medium.com/ux-ui-ia-case-studies/masked-passwords-security-questions-captcha-and-other-unusable-security-1f018ad01378#.w8ws8yo23)
[^4]:https://medium.com/@cjforms/the-idea-that-left-aligned-labels-have-slower-completion-times-is-incorrect-e1461f47242b#.dvl3en9g4
[^5]:https://uxdesign.cc/design-better-forms-96fadca0f49c#.iy0c5in6p
[^6]:http://www.90percentofeverything.com/2009/02/16/karl-sabino-on-the-roi-of-well-designed-error-messages/
