# A Login Form

> “As a user, I want to log in to [your service] so that I can [do stuff]” said nobody, ever!

Nobody *wants* to log into your site. They're forced to as a security measure. Without this, everyone has access to everyone else's stuff. Bad.

Given how long login forms have been around for and how basic they are in appearance, you'd be surprised at how often they contain the same usability mistakes that stop users doing something they don't even want to do in the first place. Add social login into the mix and things get even harder.

In this chapter, we'll design a login form and as we bump into each of the problems, we'll look at ways to remedy them. By process of elimination, users should be left with a straightforward and relatively pleasant login experience.

## A Standard Login Form

![Login bad example](./images/04/login-bad.png)

## Username Label And Hint Text

Our login form, like many sites on the web, has an ambiguous label of “Username” even though it expects users to enter their email address. Ultimately, the login form should mirror the site's registration form. In our case, this means the label should be “Email address.”

![Username vs email](./images/04/username-vs-email.png)

Legacy systems sometimes let users enter an email address or a username. In this case, the same rules apply—the label should be “Username or email address”—don't make users guess.

![Username vs username and email](./images/04/username-vs-username-and-email.png)

Some niche sites, such as airlines ask users to enter their booking reference number. In this case, use the hint pattern to tell users where they can find it.

![Hint vs no hint](./images/04/username-hint.png)

## Auto-captalisation, Auto-correct And Spell checking

Some Android and iOS browsers try to help users by auto-capitalising words in text boxes (`<input type="text">`). For example, if I type “adam”, it will be changed to “Adam” which can be helpful depending on the circumstance. 

Prior to iOS 5, this behaviour also applied to the email input. In the case of the username or email address, we certainly don't want users to exert energy fixing mistakes they didn't even make themselves. So we can disable this behaviour like this:

```HTML
<input autocapitalize="none">
```

Similarly, iOS will autocorrect words in a text input that it thinks are a mistake. Continuing with the example above: a username may contain a random string of characters that may look like a mistake but isn't. You can disable this behaviour like this:

```HTML
<input autocorrect="off">
```

By the same token, some browsers will mark misspelled words with an underline. Again, you can disable this like this:

```HTML
<input spellcheck="false">
```

Here's the final HTML for our email address field:

```HTML
<div class="field ">
  <label for="email">
    <span class="field-label">Email address</span>
  </label>
  <input type="email" id="email" name="email" value="" autocapitalize="none" autocorrect="off" spellcheck="false">
</div>
```

## Password Field Design

People often use the same password for different sites and applications. But password rules differ from site to site. For example, some sites ask for a capital letter, others ask for numbers and symbols; other sites ask for a combination of all three.

Many users will tweak their password to match the rules of the site in question. For example, if their password is “password”, and the site requires a capital letter, they'll just capitalise the first letter to “Password.” Obviously, this is not recommended, but shows that users usually take the path of least resistance.

Referring back to the registration form in chapter 1 again, we gave users a hint that explained the password rules. But like many sites, our login form fails to provide the same clarity. Why should users have to guess, or worse, reset their password?

![What are the rules](./images/04/login-hintless.png)

This sort of ambiguity is often in the name of security because providing a hint would make a hacker's job easier. But first, hackers don't hack this way and even if they did, what's to stop the the hacker checking the rules on the registration page? Nothing.

In short, we should leverage the patterns in “A Registration Form”:

1. Give users a hint text. This way, users have a greater chance of success without having to wait for a useful error message. 
2. Let users reveal their password using the password reveal pattern.

![Password field with hint and reveal](./images/04/password-field.png)

## Auto-tabbing

Some login forms, such as those found on bank sites, ask users for certain characters of their password. Or they may ask for certain digits of their security pin. In either case, users are normally given three separate text boxes or select boxes.

![Santander](./images/04/santander.png)

The first problem with this approach is that sites will auto-tab between the fields. That is, focus is moved to the next text box automatically as the user enters a pre-determined number of characters. But as the BBC's UX guidance[^3] says:

> it can be disorienting and hinder users from verifying information or correcting mistakes if the focus automatically changes when the user is not expecting it.

Leonie Watson, accessibility expert, and screen reader user, finds them problematic too:

> I strongly dislike having auto-tab functionality imposed on me. It is unexpected, and based on a flawed assumption that it is helpful. It takes me more time and effort to correct mistakes caused by auto-tab, than it does to move focus for myself.

This point of view shouldn't be surprising given that the technique is founded on assumptions that not only break convention, but also take control away from the user.

In this case, there's just no good reason for it. And splitting up a text box into three is unnecessary. A single, clearly-labelled text box lets users type three characters freely.

![Single field vs three inputs](./images/04/single-field.png)

## Submit Button Text: Log In Versus Sign In

Having ironed out problems with the username and password fields, our login form is almost identical to the registration form. It contains the same fields in the same order with the same microcopy. The only difference is the button's label. Instead of “Register” it's “Sign in.”

“Sign in” is arguably more human than “Log in.” When you visit a spa or office building, signing in grants you entry. And you sign out as you leave. Where possible, we should use the same language for digital experiences. 

It can, however, depend on the industry. Bank sites, for example, tend to use “Log in.” The notion of logging came along with computers in the 80s—the operations that users do are *logged* for security reasons.

![Login](./images/04/button-text.png)

Whichever you choose, be consistent (principle 3). Make URLs, links, headings and buttons match. And if users click “Log in” to log in, then they should click “Log out” to log out.

## The ‘username and password don't match’ Problem

Sometimes, we deliberately make things difficult to use. For example, a door by its very nature isn't easy to use—it would be far easier if there was no door at all. Login forms need to be somewhat difficult to use, otherwise they wouldn't be secure.

Many login forms make the same mistake of showing users an error message that says “The username and password doesn't match.” But as Jared Spool comically explains in In Design Is Metrically Opposed[^]:

> We know which one doesn't match, we're just not going to tell you, because our security people think that if we told you that it was the password, they would know they had a legal username and they would try every possible password in history.

As noted earlier, Hackers don't actually do this. But even if they did, it's easy to check username availability by trying to register an account with that username.

The problem for users, is that they're left to reset their password which is long-winded and may cause abandonment. Even where a lack of usability or understandability is deliberate, there still needs to be a degree of understandability and usability.

In this case, we don't have to tell users what their password is, but we can tell them that it's the password that doesn't correspond to the username (which they have right).

![What are the rules](./images/04/login-error.png)

## The Form In Context

We've ironed out many of the issues surrounding standard login forms, but we've done so while zoomed in on the form itself. We also need to consider the form in context of the page and the overal experience. This includes looking at various journeys through the login form. Let's start with layout.

### Layout

When trying to perform an action anonymously, that requires being logged in, users will be sent to the login page. 

Many sites design the login page with a unique, minimalist layout. For example, when users try to add a product to their basket on Tesco[^], they're taken to a login page with a very different layout.

![Contextless](./images/04/contextless.png)

Giving users a different layout is disorientating, especially for screen reader users and cognitively impaired users as they have to refamiliarise themselves with a new structure.

Where possible, the login form should inherit the layout of the rest of the site.

### One Form Per Page

Some sites put both registration and login forms on one page. Either next to each other on desktop, or below one another on mobile. This is problematic for a number of reasons.

![Link to register](./images/04/two-forms.png)

1. Putting similar forms next to each other makes it hard to decide between them, especially for cognitively impaired users. 
2. Arriving on a page containing two forms (with a heading of “Sign in or register”) is confusing when you consider that many users would have clicked a link labelled “Sign in”.
3. On mobile, one of the forms will be off screen and effectively depriortised.
4. Screen reader and keyboard users are going to have to wade through more of the interface to get to the relevant form.

Instead, put each form on a separate page, and give users a link to each form at the top.

![Link to register](./images/04/link.png)

### Forgotten Password Link Placement

Human beings are forgetful. Password managers[^2] mitigate this problem by storing all your passwords in one place—you just have to remember a single, master password. That's great, but password managers aren't infallible. If you don't rememeber to save your credentials into it, you're in the same position as everyone else. Moreover, not everyone uses one, nor should they have to.

Most sites give users a way to reset their password if they forget it. The feature itself isn't especially problematic. It's the placement of the link within the login form that can cause usability issues. If the link is just above the password field, when users tab from the email field, it's the link that will receive focus, not the password field. Some users will tab and start typing not realising what's happened.

Worst still, is when the link is placed before the submit button. When keyboard and screen reader users tab from the password field and press <kbd>Enter</kbd>, they'll expect the form to submit. But instead, they'll be taken to reset their password. When they realise what's happened, they'll need to go back, re-enter their credentials and be careful not to make the same mistake again.

![Forgot password](./images/04/tab.png)

When the feature is considered in isolation, having the reset password link in close proximity to the password field makes sense. But the primary need is to sign in and the link shouldn't disturb the experience of logging in.

The submit button should be the last interactive element in the form because that's what users expect. Solving this problem is simple: place the forgotten password link before the form, which makes it easy to discover, especially for screen reader and keyboard users.

## Social Login

Sites have recently started to offer users the ability to sign in with social networks such as Facebook, Twitter and Google. This saves users having to type their credentials they may not remember.

Medium.com, for example, lets users login in with Facebook. This is a boon for Medium users because they'll then have the option to post articles directly to Facebook automatically.

Social login is not without its problems though. 

### Privacy

Users are worried about their privacy. They don't know what you'll do automatically, and they want to feel as though they're information is safe and any actions they perform are intended.

Medium.com does this well because on the login page it says “We won't post without asking” which puts users minds at ease.

![Medium login](./images/04/medium-login-usage.png)

### Seamless Interchange

Some users won't remember how they originally created an account, therefore they won't know which login method to choose.

At Kidly, we handled this by showing users an error message. For example, if users had signed up with standard login, then tried to sign up with social login, we'd show an error message saying so. But this puts the burden on the user.

Again, Medium lets users log in interchangeably without users ever knowing what happened. For example, if I log in to Medium with my email but have already registered previously with Facebook, Medium logs me in automatically and merges my accounts. 

Users can visit the settings page to see what accounts are hooked up, which keeps users informed and in control.

![Medium settings](./images/04/medium-settings.png)

### Choice Versus Choice Paralysis

Standard thinking is that choice is good. That it offers freedom, autonomy and personal responsibility. Heck, even one of *the design principles* is “Offer choice”. But *more* choice is not necessarily better when it comes to features or products.

Barry Schwartz, author of “The Paradox of Choice” has a case study where researchers set up two displays of jams at a gourmet food store. Customers could try samples, who were then given a coupon for a dollar off if they bought a jar. One display had 24 jams, the other had just 6. 30% of people exposed to the smaller selection bought a jam, but only 3% of those exposed to the larger selection did.

There's also some interesting data on companies that offer employees pension plans. One of Barry's colleagues got access to the records of Vanguard, a mutual-fund company, and found that for every 10 mutual funds the employer offered, the rate of participation went down 2%. Consider that employees knew that by not participating, they were passing up as much as 5000 dollars a year.

This phenomenon is actually called Hick's Law (named after psychologist William Edmund Hick), which states that the time taken to make a decision increases as the number of choices expand. Funny that William made a law in his own name that's just basic common sense.

The point of course is that we need to be wary that giving users multiple ways to sign in might seem useful, but it may also create a buren on them. We have to balance the value in doing so.

## Summary

In this chapter we started by quashing traditional advice that omiting hint text and explicit error messages improve security on login forms. We then looked at some of the subtle usability issues that can be introduced with social login. Finally, we looked at ways of improving the experience for keyboard and mobile users which meant avoiding auto-tabbing, auto-correcting and auto-capitalising input.

### Things To Avoid

- Using ambiguous microcopy and error messages in the name of security.
- Putting the login form next to the registration form.
- Auto-tabbing between multiple fields.
- Using multiple text boxes for one field.
- Putting the forgot password link inside the form.
- Enabling autocorrect, autocapitalise and spellcheck on fields that may not expect real words: username, for example.

## Demos

- Login form

## Footnotes

[^1]: https://vimeo.com/138359368
[^2]: https://www.lastpass.com/
[^3]: http://www.bbc.co.uk/guidelines/futuremedia/accessibility/mobile/forms/managing-focus

TODO: https://twitter.com/dburka/status/947864214303100929