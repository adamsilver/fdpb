# A Login Form

“As a user, I want to log in to [your service] so that I can [do stuff]”, said nobody, ever!

Nobody *wants* to log into your site. They're forced to as security measure. Without this, everyone has access to everyone else's stuff. Not good.

Given how long login forms have been around for and how basic they are in appearance, you'd be surprised at how often they contain usability failures. And the rise of social login hasn't necessarily made things easier for designers.

In this chapter, we'll dissect each potential usability failure and provide a remedy for each of them. By process of elimination, users should be left with a straightforward and pleasant login experience.

## How It Might Look

The login form is remarkably similar to the registration form from chapter 1. It contains the same fields in the same order. In fact the microcopy is also the same. The only difference is the button text.

![Login](./images/login.png)

## Button Text

The button is labelled as ‘Sign in’ as opposed to ‘Log in’. Why *not* ‘Log in’? ‘Log in’ is jargon and relates to something computery, something digital. ‘Sign in’ is the same thing, but more human. When you visit a spa or hotel, you're asked to *sign in*, not *log in*. We should design words to be as human as possible, making the experience friendly and familiar regardless of the medium.

## Provide A Hint

Most login forms don't tell users what the password rules are (like they do when registering). This is usually in the name of security, but more on that shortly.

Many times I've used the same password across different sites. When necessary I'll tweak it to pass the validation rules specific to those sites that deviate from the norm. For example, imagine my password is *password*. If a site requires a capital letter, I'll capitalise the first letter to *Password*. And if the site forces me to include a number, then I'll add the number to the end: *password1*.

Most people have accounts across many sites with many different password rules. This makes passwords hard to remember. Without supplying users with a hint, they're left to guess the rules. To find out if their guess works, they have to submit and hope for a useful error message.

The point is that by providing a hint, users have a greater chance of getting it right without having to submit and wait for feedback.

Often we omit hints due to “security reasons”. But if a hacker wanted to find out the rules, they just register for an account themselves - hardly taxing.

## Use Explicit Labels

When we designed the registration form in chapter 1, we asked users for their email address, not *username* as many sites use. As such, the field is labelled *email address*. Many sites create a problem here, by expecting the user's email address but labelling the field as a username.

## The ‘username and password doesn't match’ Problem

Many users forget their username and password. As we discussed just before, omitting a hint makes matters worse. But worse still, is when a user enters their credentials incorrectly, only to be shown an error later on: *The username and password doesn't match*. Put simply, this is awful and Jared Spool comically explains the issue in “Design Is Metrically Opposed”[^1]:

> We know which one doesn't match, we're just not going to tell you, because our security people think that if we told you that it was the password, they would know they had a legal username and they would try every possible password in history.*

Hackers don't actually hack this way. But even if they did, all they have to do to find out if the username exists, is try and sign up for an account with that username.

The problem for users is that they're often left with no other course of remediation other than to reset their password which is a tedious thing to have to do and is a cause for drop outs.

Like with any other form, we must explicitly tell users what went wrong so they can fix the issue with as little stress as possible.

## Non-standard Username And Password Fields

Instead of asking for *email* and *password*, some sites ask users to sign in using other details. For example, airlines ask users to access their account with their booking reference. Similarly, banks often ask for a pin number. If you must use something that is non-standar then use an explicit label and where they might find it.

## Contextual Login Forms

Some sites have login-only areas. Often login form pages are designed without considering the context in which they are being used. To explain, we'll use checkout as an example. And for purposes of demonstration, we'll ignore that forcing users to login before checkout is an anti-pattern.

Take a shopping basket page. Below the basket is a *checkout* button. Clicking it, takes the user to the beginning of the checkout journey. However, if they're logged out, they're prompted to login.

As discussed in chapter 2, we used a checkout-specific layout&mdash;one with a minimal header to streamline that process. The login form should be given the same treatment. That is the login form should use the checkout-specific layout&mdash;after all, the user clicked *checkout* in the first place.

This reinforces that the user is midway through a process and helps them focus. That's the reason the checkout layout exists in the first place.

Tesco, for example, don't do this. When the user adds a product to their basket, they are prompted to login elsewhere&mdash;somewhere out of context which feels somewhat disorientating.

![Forced to login](./images/tesco-logged-out.png)

![Login form](./images/tesco-login-form.png)

## Social Login

Up until recently, most sites only offered people the standard username and password approach to login. Many sites still do and there's no problem with that. However, more sites are offering the ability to sign in with social networks such as Facebook, Twitter and Google. This saves users typing their login details. If they are logged into Facebook, for example, then they're instantly logged into your site.

Also, they don't have to spend time registering and remembering another set of credentials. And some sites will integrate with your social media account. For example, Medium.com, a social media site for reading and writing articles, will post to Facebook automatically, for example. Those who spend a lot of time socialisiing on Facebook will be pleased to automate this.

Social login, however, is not without it's problems. There are issues of privacy and there is a problem in remembering which mechanism the user chose to sign up with. Lastly, there is the fact that giving users choice can also lead to choice paralysis. That is, the more choice there is, the harder it is for users to make a decision.

### Privacy Concerns

To alleviate people's privacy concerns, it's crucial to tell users exactly how their credentials will or won't be used. Medium.com does a good job off this by telling users *they won't post without asking*.

![Medium Login](./images/medium-login.png)

### Seamless Interchange

Some people may not remember which method they used to sign up. For Kidly, we detected that they had signed up with a different method, we simply told them with an error message:

![Login form](./images/kidly-error.png)

Medium.com solves this without needing to resort to error messages. In fact, it's so seamless that users have no idea anything happened at all. As a user I shouldn't have to remember which method I used to sign up with.

Facebook, for example, knows what my email is. If I sign in with my email and have already signed up with Facebook, then Medium.com logs me in automatically. They just merge my accounts without me knowing. As a user the only way I would know is if I bother to visit the settings page.

![Medium settings page](./images/medium-settings.png)

The ‘Connections’ section shows Facebook and Twitter options. Users can connect or disconnect their social media logins easily here if they want to.

### Choice Versus Choice Paralysis

‘They aren’t many laws in psychology, but here’s one you should know’
-- https://medium.com/@userfocus/10-findings-from-psychology-that-every-user-researcher-should-know-9900973ec186

However, with choice comes choice *paralysis*. If there are many ways to login then users have to spend time and energy *deciding*. To make the choices obvious, lay out the choices clearly:

![Login form](./images/kidly-login.png)

If you're going to provide social login capabilities, then first work out why. If the why is compelling, then be sure to make this seamless for users and make the choices clear.

## ‘Forgotten password’ Placement

As human-beings we're prone to forgetting something just at the moment we most need to remember it. As the saying goes, *we're only human*. I use a password manager[^2] to avoid having to remember, but password managers aren't infallible. If you don't remember to add your credentials into the password manager, then you're in the same position as anyone else. And, not everybody uses a password manager.

Sites offer users a ‘Forgotten password’ link on the login screen which gives users the chance to ‘reset’ their password by following a few simple steps. The feature itself isn't the problem, it's the placement of the link which can cause unnecessary friction for users.

Honouring people's interaction preferences is a cornerstone of inclusive design. Some prefer the keyboard. Some prefer the mouse. Some use both interchangeably.

When using forms, or more broadly, websites, pressing <kbd>tab<kbd> moves focus to the next focusable element. Naturally, and with no  intervention, links and form controls are focusable.

When interacting with a login form, or any form for that matter, the main task is filling out each field and then submitting it. Placing  the ‘Forgotten password’ link between the username and password fields disrupts the natural flow of filling in a form. Even placing the link between the password field and the submit button causes the same level of disruption.

When inside a form, the user expects the <kbd>tab</kbd> to move to the next field, but instead focus moves to the link. And when they do, they start typing, and nothing happens. Eventually they realise they need to tab again. Alternatively, they lose trust, and switch to using the mouse which is a little more explicit. What you click is what becomes focussed.

Whilst placing the link in close proximity to the password field makes some sense from a visual perspective we can a little better. The primary need of the login form is to login. This means the fields and the submit button should come first with the seconday action (reseting the password) shluld come right after it.

It's still in context and works visually without fighting for attention and without disturbing the forms natural flow.

![Forgot password link](./images/forgot-password-link.png)

## Auto-tabbing between fields

Some niche login forms, such as those found on bank sites, ask users for certain characters of a password, memorable word or security pin. They'll then present 3 text boxes or worse, 3 select boxes from which to choose from.

![Image](.)

There is no reason to have 3 separate boxes, much less 3 separate select boxes, but in doing so, they decide to auto-tab between the fields. What this means is focus is moved to the next field automatically as the user enters a pre-determined number of characters.

BBC's UX guidance[^3] says *it can be disorienting and hinder users from verifying information or correcting mistakes if the focus automatically changes when the user is not expecting it.*

And, Leonie Watson, accessibility expert, and someone that uses a screen reader and keyboard says:

> I strongly dislike having auto-tab functionality imposed on me. It is unexpected, and based on a flawed assumption that it is helpful. It's worth noting that it takes me more time and effort to correct mistakes caused by auto-tab, than it does to move focus for myself.

This point of view is unsurprising considering that the solution itself is founded on assumptions. Assumptions that take control away and explicitly moves away from convention. These are two qualities we want to adhere to as priority, not move away from.

## One form per page

Some sites put the registration and login forms on the same page beside each other (or below one another on small screens). In previous chapters, we leveraged One Thing Per Page to put one question on a page. Similarly we can apply this rule to the entire form.

The registration and login forms are remarkably similiar. Putting them together makes them hard to differentiate. It's also confusing for users to be presented with a registration form, having clicking a specific call to action such as *login*.

Give each form a separate and offer a link before or after the login form allowing users to register or vice versa.

![](.)

## Summary

In this chapter we covered the main problems associated with login forms. We looked at some of the usability failures that typically occur in the name of security and how in reality, it doesn't actually stop hackers at all.

We continued by looking at how social login behaviour introduces trade-offs and how we can mitigate some of the inherent problems with it. Finally we ensured that login forms are always surfaced in the context of which the user triggered it ensuring the experience is both intuitive and momentum building.

### Things to avoid

- Using Javascript to auto tab between fields
- Using ambiguous error messages and actively not helping users in the name of security
- Employing social login for the sake it
- Putting more than one form per page
- Messing up keyboad flow by placing other focusable elements (typically links) inbetween form controls.

## Footnotes

[^designopposed]:
[^passwordmanager]:
[^3]:http://www.bbc.co.uk/guidelines/futuremedia/accessibility/mobile/forms/managing-focus

## Other bits?

- autofocus
- https://sessioncam.com/reducing-dropout-during-your-online-checkout-process/
- A note on logout?