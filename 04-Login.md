# A Login Form

> As a user I want to login so that
> —Nobody ever

In chapter 3 *Book A Flight*, we *ahem*, navigated our way through the complicated world of custom form controls. I don't know about you, but I could certainly do with a short intermission. And I can't think of a better way to do that, than with the inocuous and ubiquitous login form.

Most sites have login forms but almost as many sites have login forms that cause problems for users. The rise of social login hasn't helped matters either. In this chapter we're going to discuss these issues and look to design solutions accordingly.

A login form:

![Login](./images/login.png)

HTML:

```html
<form>
  <div class="field">
  	<label for="email">
  		<span class="label">Email address</span>
  	</label>
  	<input type="email" id="email" name="email">
  </div>
  <div class="field">
  	<label for="password">
  		<span class="label">Password</span>
  		<span class="hint">Must contain at least 8 characters with 1 uppercase letter and a number.</span>
  	</label>
  	<input type="password" id="password" name="password">
  </div>
  <input type="submit" value="Sign in">
</form>
```

If you're reading this book in order, you'll notice how similar this form is to the form we settled on in chapter 1 *Registration*. We have the same fields with the same field types. And we have the exact same password hint.

In fact everything is pretty much indentical except for the labelling of the password field and the button text. This text is more appropriate in this context.

Too often login forms forgo hint text. That is they don't tell users what password rules they need to abide by. Shamefully, but like many people, I use the same or similar passwords for lots of sites although writing this now has prompted me to sort that out.

In anycase, imagine my password is *password*. On a site that requires a capital letter, I'll probably create a password of *Password*. And if the site forces me to include a number, then it will be *password1*.

The point is, if there is a hint to inform me of the rules, then I won't have to submit the form to find out. Doing so is frustrating, slower and unnecessary.

Often these rules are omitted due to "security reasons". But if a hacker  wants to find out the password rules then all they need to do is sign up for an account themselves.

Don't do this. Instead give users a chance by providing a hint. And if, like some users, they ignore the hint our fully inclusive validation routine will kick in.

## Non standard username and password fields

Some services, like the flight booking service we designed in chapter 3 may not ask users to sign in with their email and password credentials. Instead, they may ask for a booking reference number, for example.

Similarly, some banks ask for a pin number instead of a password. In either case, don't use ambiguous labels. If it's a pin or reference number say so. And also tell users where they might find it.

This is exactly what we did in chapter 2 *Checkout* when we asked users for the security number on the back of their credit card. If something is out of the ordinary or testing shows that people may struggle, provide an obvious hint.

In doing so users have to think less.

## The username and password doesn't match problem

If the user enters an incorrect username or password, many sites will show an error saying *The username and password doesn't match*. Put simply, this is bad and as Jared Spool explains in Design Is Metrically Opposed[^]:

> We know which one doesn't match, we're just not going to tell you, because our security people think that if we told you that it was the password, they would know they had a legal username and they would try every possible password in history.*

Hackers don't do this and even if they did most sites let you sign up for one account. This means if they want to find out, all they have to do is sign up for an account themselves with that username.

Instead, tell users which field is problematic and allow them to fix it easily.

## Contextual login forms

Some sites have all-access areas and login-only areas. Quite often login form pages are designed without considering the context in which they are being used.

To demonstrate the problem, we'll ignore for the moment, that forcing users to login before checking out is an anti-pattern. Imagine a basket (or shopping cart page if you're linguistically american). On there is a *checkout* button.

Clicking *Checkout* takes the user to the beginning of the checkout flow. If the user is logged out they are prompted to login first. A checkout flow, is often designed to streamline the process. To do this checkout flows have a minimalist layout.

If the user went to checkout, then even though the user is presented with a login form, they should still feel as though they are in checkout. This is what we did for Kidly:

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

I think we could have done a better job with this. Again, Medium.com solves this elegantly. In fact it's so elegant that users have no idea. As a user I shouldn't have to remember which one I signed up with.

And Facebook, for example, knows what my email is. If I sign in with my email and have already signed up with Facebook, then Medium.com logs me in automatically. It's seamless. They just merge my accounts without me knowing. The only way I realise this is if I visit the settings page:

![Medium settings page](./images/medium-settings.png)

The "Connections" section shows Facebook and Twitter options. Users can connect or disconnect their social media logins easily here. But only if they're interested in doing so.

If you're going to provide social login capabilities, then first work out why. If the why is compelling, then be sure to make this seamless for users and make the choices clear.

## Forgot Password Placement

One aspect of inclusivity is honouring people's interaction preferences. Some users prefer using the keyboard. Some prefer the mouse. Some use them both interchangeably.

When using forms or more broadly websites, the *tab* allows users to move focus. By default links and form controls gain focus with the tab key. This switching between forms and links is a source of disruption. Unlike forms mode, a web page allows users to move freely between links and form controls.

This means that if you place a forgotten password link between the username and password fields, some users may tab to the link, instead of the expected password field.

Or perhaps the forgotten password link may appear straight after the password field. Both of these may make some sense from a visual design perspective. Offering a solution to a problem in context of the problem is an act of good contextual design.

However, the form is short and so we can place the forgotten password link after the form itself.

![Forgot password link](./images/forgot-password-link.png)

## Don't put two forms together on same page

Some sites put the registration and login forms on the same page. Putting them on the same page is problematic for many of the reasons most of which we discussed in One Thing Per Page.

As we know the registration form is similar to the login form. Putting them together makes it harder to differentiate. It's also confusing for users that have ended up on this page having clicked a specific call to action such as *login*.

Instead keep things separate and offer a small link before or after the login form allowing users to register.

## Summary

In this chapter we have covered the main problems associated with login forms. Most sites have a login form. Don't let unfounded security holes stop you designing a friction-free login experience.

If you're going to provide social login capabilites consider the tradeoffs, implement one at a time, and ensure users aren't penalised if they sign up using different mechanisms.

Finally, put login forms in context of the flow in which they have been triggered. It's intuitive and keeps users focussed on the task at hand.

## Footnotes

[^facebook]:(https://developers.facebook.com/docs/facebook-login/multiple-providers)

https://conversionxl.com/social-login/

https://blog.loginradius.com/2014/01/understanding-benefits-social-login-add-value-website/
