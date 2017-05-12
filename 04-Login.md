# A Login Form

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

And Facebook, for example, knows what my email is. If I sign in with my email and have already signed up with Facebook, then Medium.com logs me in automtically. It's seamless. They just merge my accounts. The only way I realise this is if I visit the settings page:

![Medium settings page](./images/medium-settings.png)

The "Connections" section shows Facebook and Twitter options. Users can connect or disconnect their social media logins easily here. But only if they're interested in doing so.

If you're going to provide social login capabilities, then first work out why. If the why is compelling, then be sure to make this seamless for users and make the choices clear.

## Forgot Password Placement

This is a minor nitpick but the forgotten password link should come after the form in the document flow. Some cute designers sometimes place the link within the form itself in context of the password field, but this can upset the natural tab order.

When inside a form, users expect the tab to move to form controls. Unlike forms mode in screen readers, normal browsing with the keyboard will focus through form inputs *and* links. Putting a link within a form disrupts the natural flow.

Wherever possible place the link after the form as follows:

![Forgot password link](./images/forgot-password-link.png)

## Sign in/Sign up

Strictly speaking this is not to do with the login form itself, but more to do with how the user arrives at the login form. The advice here is simple. Don't use the same verbs next to each other if you can avoid it.

The problem with using the same verbs next to each other like this is that the difference is minimal, and therefore, hard to notice. Users will make mistake and those that don't will have to exert yet more energy working out which they want.

Instead use *Sign In* and *Register* as they are far easier to distinguish.

## Don't put two forms together on same page

## Summary

Todo

## Footnotes

[^facebook]:(https://developers.facebook.com/docs/facebook-login/multiple-providers)

https://conversionxl.com/social-login/

https://blog.loginradius.com/2014/01/understanding-benefits-social-login-add-value-website/
