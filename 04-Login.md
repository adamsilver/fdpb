# A Login Form

‘As a user I want to sign in so that...’ said nobody, ever.

Nobody *wants* to log (or sign) in to your service. They're forced to for security reasons. Otherwise, everyone could access everyone elses stuff. It would be like nobody having locks on their front door.

Most sites have login forms, but almost as many of them have usability failures in some form or other. Social login, like signing in with Facebook, complicates matters further. For such a ubiquitous and seemingly simple feature, it's surprising how many issues crop up from site to site.

This chapter is dedicated to disecting and remedying each of these issues one by one. By process of elimination, users will be left with a pleasant login form that just works.

How it might look:

![Login](./images/login.png)

```html
<form novalidate>
  <div class="field">
  	<label for="email">
  		<span class="field-label">Email address</span>
  	</label>
  	<input type="email" id="email" name="email">
  </div>
  <div class="field">
  	<label for="password">
  		<span class="field-label">Password</span>
  		<span class="field-hint">Must contain 8+ characters with at least 1 number and 1 uppercase letter.</span>
  	</label>
  	<input type="password" id="password" name="password">
  </div>
  <input type="submit" value="Sign in">
</form>
```

The login form is the counterpart to the registration form we designed in chapter 1. As such, it's remarkably similar. It contains the same fields, and even has the same hint. In fact, the only difference is the button text. Instead of *Register*, we label the button with ‘Sign in’.

Why *not* ‘Log in’? ‘Log in’ is jargon. ‘Sign in’ feels more human. Think about it, when you go to a spa or a hospital, you *sign in*, you're never asked to *log in*. We should design words to be as human as possible, making the experience friendly and familiar at the same time.

## Provide a hint

Most login forms don't tell users what the password rules are, like they do in the registration form. This is in the name of security, but more on that in a moment.

Shamefully, I use the same password for most sites, and I'll tweak it to adhere to the rules of the site in question. For example, imagine my password is *password*. On a site that requires a capital letter, I'll capitalise the first letter: *Password*. And if the site forces me to include a number, then I'll add the number to the end: *password1*.

Most people have accounts to many different sites with many different password rules, making passwords difficult to remember. Without a hint, I'm left to guess the rules of this particular site. Normally, I'd have to submit the form to find out what the error is.

Most error messages are lacking too which we'll talk about shortly. The point is that by providing a hint, users have far more chance in getting their password right without having to submit and wait for feedback. Doing *that* is slower and unnecessary.

Often we omit hints due to ‘security reasons’. But if a hacker wanted to find out the rules then they would simply register themselves in seconds.

## The ‘username and password doesn't match’ problem

It is widely known that most people forget their username and password. And as we just discussed omitting a hint makes matters worse. But worse still, when a user enters their credentials incorrectly, many sites will show an error saying *The username and password doesn't match*.

Put simply, this is bad and as Jared Spool explains in Design Is Metrically Opposed[^]:

> We know which one doesn't match, we're just not going to tell you, because our security people think that if we told you that it was the password, they would know they had a legal username and they would try every possible password in history.*

Hackers don't do this and even if they did most sites let you sign up for one account. This means if they want to find out, all they have to do is sign up for an account themselves with that username.

The problem for users is that they're often left with no other course of remediation other than to reset their password which is a tedious thing to have to do and easily increases drop outs.

Like with any other form, tell users what went wrong so they can fix the issues easily.

## Non-standard username and password fields

Some services, like the flight booking service we designed previously, could ask users to log in with something other than an email and password. A booking reference number, for example.

Similarly, banks often ask for a pin number instead of a password. In both cases, use explicit labels. If it's a pin number say so. And tell users what the format it is and where they might find it (if it is printed on a document, for example.)

All of this allows the user to think less, keeping their energy levels high and ready to do the thing they actually want to do. Remember they have no inclination to log in. They want the feature that lies behind it.

## Contextual login forms

Some sites have all-access areas and login-only areas. Often login form pages are designed without considering the context in which they are being used. To explain, we'll use checkout as an example. And for purposes of demonstration, we'll ignore that forcing users to login beforehand is an anti-pattern.

Take a shopping basket page. Below the basket details, there is a *checkout* button. Clicking it, takes the user to beginning of the checkout flow. However, if they are logged out, they're prompted to login.

As discussed in chapter 2, we provide a checkout specific layout with a minimal header to streamline that process. We should ensure the login form should be given the same treatment, as the user should still feel as though they are in checkout. After all this is what they clicked.

This is what we did for Kidly:

![Forced to login](./images/kidly-login.png)

This reinforces that the user is midway through a process and helps them focus. That's the reason for the dedicated checkout layout in the first place.

Conversely, Tesco don't do this. When the user adds a product to their basket, they are taken somewhere else, that feels more out of context which is disorientating.

![Forced to login](./images/tesco-logged-out.png)

![Login form](./images/tesco-login-form.png)

## Social Login

Up until recently, most sites only offered people the standard username and password approach to login. Many sites today still do this.

However, more sites are offering users the ability to sign in with Facebook or Twitter for example. This saves users typing&mdash;if they are logged into Facebook already then there they are instantly logged into to the site.

Also, they don't have to spend time signing up and remembering yet another set of credentials. And some sites will integrate with your social media account. For example, Medium.com, a social media site for reading and writing articles, will post to Facebook automitically for example.

For those who spend a lot of time socialisiing on Facebook, they'll be pleased to automate some of these things.

However, with choice comes choice *paralysis*. If there are many ways to login then users have to spend time and energy *deciding*. There is also a question and concern over privacy.

To make the choices obvious, you can do this by clearly setting out the options. This is what we did for Kidly:

![Login form](./images/kidly-login.png)

To try and mitigate concerns over privacy, it's important to tell users how their credentials will or won't be used. I think we could have done a better job on this at Kidly.

Medium.com make this clear by telling users that they *won't post without asking*.

![Medium Login](./images/medium-login.png)

The last problem is that some users may not remember which method they chose to sign up. At Kidly, if we detected that they had signed up with a different choice then we told them with an error message.

![Login form](./images/kidly-error.png)

I think we could have done better with this. Again, Medium.com solves this elegantly. In fact it's so elegant that users have no idea. As a user I shouldn't have to remember which one I signed up with.

And Facebook, for example, knows what my email is. If I sign in with my email and have already signed up with Facebook, then Medium.com logs me in automatically. It's seamless. They just merge my accounts without me knowing. The only way I realise this is if I visit the settings page:

![Medium settings page](./images/medium-settings.png)

The "Connections" section shows Facebook and Twitter options. Users can connect or disconnect their social media logins easily here. But only if they're interested in doing so.

If you're going to provide social login capabilities, then first work out why. If the why is compelling, then be sure to make this seamless for users and make the choices clear.

## ‘Forgotten password’ placement

As human-beings we're prone to forgetting something. We, as the saying goes, *only human*. I use a password manager[^] to avoid having to remember, but they aren't infallible. If you don't remember to add the details into the password manager, then you're in the same boat as everyone else. Plus not everybody uses a password manager.

Sites offer users a ‘Forgotten password’ link on the login screen which gives users the chance to ‘reset’ their password by following a few simple steps. The feature itself isn't the problem, it's the placement of the link which can cause unnecessary friction for users.

Honouring people's interaction preferences is a cornerstone of inclusive design. Some prefer the keyboard. Some prefer the mouse. Some use both interchangeably.

When using forms, or more broadly, websites, pressing <kbd>tab<kbd>  moves focus to the next focusable element. Naturally, and with no  intervention, links and form controls are focusable.

When interacting with a login form, or any form for that matter, the main task is filling out each field and then submitting it. Placing  the ‘Forgotten password’ link between the username and password fields disrupts the natural flow of filling in a form. Even placing the link between the password field and the submit button causes the same level of disruption.

When inside a form, the user expects the <kbd>tab</kbd> to move to the next field, but instead focus moves to the link. And when they do, they start typing, and nothing happens. Eventually they realise they need to tab again. Alternatively, they lose trust, and switch to using the mouse which is a little more explicit. What you click is what becomes focussed.

Whilst placing the link in close proximity to the password field makes some sense from a visual perspective we can a little better. The primary need of the login form is to login. This means the fields and the submit button should come first with the seconday action (reseting the password) shluld come right after it.

It's still in context and works visually without fighting for attention and without disturbing the forms natural flow.

![Forgot password link](./images/forgot-password-link.png)

## Auto-tabbing between fields

Some niche login forms, such as those found on bank sites, ask users for certain characters of a password, memorable word or security pin. They'll then present 3 text boxes or worse, 3 select boxes from which to choose from.

![Image]()

There is no reason to have 3 separate boxes, much less 3 separate select boxes, but in doing so, they decide to auto-tab between the fields. What this means is focus is moved to the next field automatically as the user enters a pre-determined number of characters.

BBC's UX guidance[^] says *it can be disorienting and hinder users from verifying information or correcting mistakes if the focus automatically changes when the user is not expecting it.*

And, Leonie Watson, accessibility expert, and someone that uses a screen reader and keyboard says:

> I strongly dislike having auto-tab functionality imposed on me. It is unexpected, and based on a flawed assumption that it is helpful. It's worth noting that it takes me more time and effort to correct mistakes caused by auto-tab, than it does to move focus for myself.

This point of view is unsurprising considering that the solution itself is founded on assumptions. Assumptions that take control away and explicitly moves away from convention. These are two qualities we want to adhere to as priority, not move away from.

## One form per page

Some sites put the registration and login forms on the same page beside each other (or below one another on small screens). In previous chapters, we leveraged One Thing Per Page to put one question on a page. Similarly we can apply this rule to the entire form.

The registration and login forms are remarkably similiar. Putting them together makes them hard to differentiate. It's also confusing for users to be presented with a registration form, having clicking a specific call to action such as *login*.

Give each form a separate and offer a link before or after the login form allowing users to register or vice versa.

[!]()

## Summary

In this chapter we covered the main problems associated with login forms. Most sites have a login form. Don't let unfounded security holes stop you designing a friction-free login experience.

If you're going to provide social login capabilites consider the tradeoffs, implement one at a time, and ensure users aren't penalised for signing up with one or another.

Finally, put login forms in context of the flow in which they have been triggered. It's intuitive and keeps users focussed on the task at hand.

### Things to avoid

- Auto tabbing
- Ambiguous error messages
- Omitting hints

## Footnotes

[^facebook]:(https://developers.facebook.com/docs/facebook-login/multiple-providers)
[^]: https://conversionxl.com/social-login/
[^]: https://blog.loginradius.com/2014/01/understanding-benefits-social-login-add-value-website/
[^]:https://www.usability.gov/what-and-why/glossary/auto-tabbing.html
[^]:http://www.bbc.co.uk/guidelines/futuremedia/accessibility/mobile/forms/managing-focus
[^]: https://lists.w3.org/Archives/Public/w3c-wai-ig/2015AprJun/0168.html
[^]: https://ux.stackexchange.com/questions/22508/auto-advance-to-next-field