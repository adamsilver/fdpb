# A Login Form

“As a user, I want to log in to [your service] so that I can [do stuff]”, said nobody, ever!

Nobody *wants* to log into your site. They're forced to as a security measure. Without this, everyone has access to everyone else's stuff. Bad.

Given how prevalent login forms have been around for and how basic they are in appearance, you'd be surprised at how often they contain usability failures. And social login hasn't necessarily made things any easier.

In this chapter, we'll dissect each usability failure and provide a remedy for each of them. By process of elimination, users should be left with a straightforward and pleasant login experience.

## Button Text

The login form is remarkably similar to the registration form in chapter 1. It contains the same fields, in the same order with the same microcopy. In fact, the only difference is the button text - instead of “Register” the button's label is “Sign in”.

![Login](./images/login.png)

“Sign in” is a softer and human version of “Log in”. When you enter a spa, hotel or office building, you sign in which grants you entry. And you sign out as you leave. We should try and use the same principles that people are familiar with digital experienes too.

Whichever you go for consistency is important. Don't use ”Sign in” here and then “Log out” elsewhere. “Log in” goes with “Log out”, and “Sign in” goes with “Sign out”.

## Provide Hint Text

People often use the same password for different sites and applications. But password rules differ from site to site. Some sites ask for capital letter, others don't. When signing up people will tweak their password to match the rules. For example, if my password is “password” but the site requires a capital letter, I'll set it to “Password”.

That's all very well upon registering, but it doesn't help if I'm signing in later. Giving users a refresher of the rules stops users having to repeatedly guess or worse, having to reset their password.

With hint text, users have a greater chance of success without having to wait for feedback. Hint text is often omitted due to security reasons. But if the hacker wanted to find out the rules, they can just register for an account themselves - hardly taxing!

## Use Explicit Labels

Some sites ask for a username but expect an email address. Other sites, such as airlines or banks ask for alternative credentials such as a booking reference number, customer ID or pin number.

If you have to deviate from the standard email address and password fields, then label the fields explicitly. If you expect an email address use “Email address”, if it's a username say “Username”. If it's one or the other say “Username or email address”.

If some of the credentials can be found on a document, such as a booking confirmation email or bank statement, then tell users where to find it. Leaving users to guess is an unnecessary source of friction.

## The ‘username and password doesn't match’ Problem

While omitting hint text increases the chance of an error, what's worse is when users make a mistake and see an error that says “The username and password doesn't match”. Jared Spool comically explains why this is an issue in his talk “Design Is Metrically Opposed”[^1]:

> We know which one doesn't match, we're just not going to tell you, because our security people think that if we told you that it was the password, they would know they had a legal username and they would try every possible password in history.*

As Jared says, Hackers don't actually hack this way. But even if they did, all they have to do to find out if the username exists, is try and sign up for an account with that username.

The problem for users is that they're often left with no other course of remediation other than to reset their password which is long-winded and can cause users to give up.

Like with any other form, we should tell users exactly what went wrong so that they can fix it easily.

## Contextual Login Forms

Some sites have login-only areas. Often login pages are designed without considering the context in which they are being used. To explain, we'll use checkout as an example. And for purposes of demonstration, we'll ignore that forcing users to login before checkout is an anti-pattern.

Take a shopping basket page. Below the basket is a *checkout* button. Clicking it, takes the user to the beginning of the checkout journey. However, if they're logged out, they're prompted to login.

As discussed in chapter 2, we used a checkout-specific layout - one with a minimal header to streamline the process. When the user is taken to the login page, it should be housed inside the checkout layout. After all, the user clicked *checkout* in the first place.

This reinforces that the user is midway through a process and helps them focus. That's the reason the checkout layout exists in the first place.

Tesco, for example, don't do this. When the user adds a product to their basket, they are prompted to login elsewhere - somewhere out of context which feels somewhat disorientating.

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

Standard thinking is that choice is good. That it offers freedom, autonomy and personal responsibility. Heck, even one of *the principles* is “offer choice”. But *more* choice is not necessarily better when it comes to features or products.

Barry Schwartz, author of “The Paradox of Choice” has one case study where researchers set up two displays of jams at a gourmet food store. Customers could try samples, who were then given a coupon for a dollar off if they bought a jar. One display had 24 jams, the other had just 6. 30% of people exposed to the smaller selection bought a jam, but only 3% of those exposed to the larger selection did.

There's some interesting data on companies that offer employees pension plans. One of Barry's colleagues got access to the records of Vanguard, a huge mutual-fund company, and found that for every 10 mutual funds the employer offered, the rate of participation went down 2%. Consider that employees knew that by not participating, they were passing up as much as 5000 dollars a year.

This phenomenon is actually called Hick's Law (named after psychologist William Edmund Hick), which states that the time taken to make a decision increases as the number of choices expand. It predicts that the greater the number of alternative choices, the longer it will take a user to make a decision.

Giving users multiple ways to log in might seem useful but can cause friction for users, so make sure the extra choice is valuable to users. Once you're sure, layout the options clearly.

![Medium login](.)

## Forgotten Password Link Placement

Human beings are forgetful. In fact we're most likely to forget something at the moment we most need it. Some people, myself included, use password managers[^2]. They store all your passwords in one place and all you have to do is remember a single master password.

That's great, but password managers aren't infallible. If you don't rememeber to save your credentials into it, you're in the same position as everyone else. In any case, not everyone uses one, nor should they have to.

Most sites give users a way to reset their password if they forget it. The feature itself isn't especially problematic. It's the placement of the link within a login form that can cause tremendous frustration for users.

If the link is just above the password field, when users tab from the email field, it's the link that will have focus, not the password field. Some users will tab and start typing not realising what's happened. 

Worse still is when the link is placed before the submit button. When keyboard users tab from the password field and press <kbd>Return</kbd>, they will expect the form to submit, but instead, they will be taken reset their password. Once they realise, they'll have to go back, re-enter their credentials and be careful to either tab twice or switch to using their trusty mouse.

![Forgot password link](./images/forgot-password-link.png)

While placing the link in close proximity to the password field makes some sense visually, the primary user need is to log in. Therefore, the link shouldn't disturb the act of loggin in. Instead, you should place it before the form or after the submit button.

## Auto-tabbing Between Fields

Some login forms, such as those found on bank sites, ask users for certain characters of their password. Or they may ask for certain digits of their security pin. In either case they normally give users three text boxes, or even worse, three select boxes from which to choose from.

![Hargreaves](.)

The first problem with this approach is that sites will auto-tab between the fields. That is, focus is moved to the next field automatically as the user enters a pre-determined number of characters. But as the BBC's UX guidance[^3] says:

> it can be disorienting and hinder users from verifying information or correcting mistakes if the focus automatically changes when the user is not expecting it.

Leonie Watson, accessibility expert, and screen reader user backs this point up:

> I strongly dislike having auto-tab functionality imposed on me. It is unexpected, and based on a flawed assumption that it is helpful. It's worth noting that it takes me more time and effort to correct mistakes caused by auto-tab, than it does to move focus for myself.

This point of view should be hardly surprising given that the solution is founded on assumptions. Assumptions that take control away from the user while simultaneously breaking convention. Two qualities we want to move toward, not away from.

The other problem with these interfaces is that they separate what should be one text box into three. There is absolutely no reason to do this. Give users one text box and allow them to type freely.

## One Form Per Page

In previous chapters, we leveraged One Thing Per Page to start with one question on a page but we can also apply this pattern to pages and forms as a whole.

Some sites put the registration and login forms on the same page beside each other (or below one another on small screens). Registration and login forms are remarkably similiar. Putting them together makes them hard to differentiate. It's also confusing for users to be presented with two forms when they clicked a link labelled “Sign in”.

Instead, put each form on a separate page. To let users switch between them, use a link that allows them to switch easily.

![](.)

## Autocapitalisation

https://stackoverflow.com/questions/5171764/how-do-you-turn-off-auto-capitalisation-in-html-form-fields-in-ios

https://bitly.com/a/sign_in

## Autocorrect

If not using the right type, iOS for example will autocorrect the field thinking it's not valid.

Using type=“text” instead of type=“email” or type=“number”. It means iOS autocorrects it because it’s a text field.

https://www.bennadel.com/blog/2307-disabling-auto-correct-and-auto-capitalize-features-on-iphone-inputs.htm

https://stackoverflow.com/questions/35513968/disable-auto-correct-in-safari-text-input

https://uxcellence.com/2014/tweaking-automatic-form-input



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
- https://twitter.com/adambsilver/status/917650810523267073
- When autofill doesn't work?
- autocapitalisation!!!
https://twitter.com/slicknet/status/916704955129413632
