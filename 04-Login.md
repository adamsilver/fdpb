# A Login Form

Having navigated the complex world of custom form controls in Chapter 3, we'll take this opportunity to improve a form that resides on pretty much every website.

Login forms are so , but have become trickier with the rise of social login. There are also common problems that too many login forms out their in the wild suffer from. We we want to do better and so we will.

What it looks like:

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

This is almost identical to the registration form we designed in chapter 1. In fact we can use the exact same pattern for each field. We use the same elements, the same behaviour including the show password enhancement.

The only difference is the password label. Instead of telling users to choose a password, we ask them to enter it. We also keep the hint there so that we don't force users to guess what's needed.

Often this is done in the name of security, but any hacker can sign up themselves to find it the rules if they really want to.

## Non standard username and password fields

If your username isn't an email address say so. For example, a flight booking service, like the one we designed in chapter 3 might ask for a booking reference number, instead of an email address.

In this case tell the user what they need to enter, what form it is in, and where they might find it. Probably in email correspondence or on their ticket.

Similarly, if the password is a pin number, like Paypal, then say so, and tell the user how many numbers they are expected to type.

## Error messages and security

First listen to Jared Spool (https://www.uie.com/jared-live/#design-opposed) at 42 mins.

https://kev.inburke.com/kevin/invalid-username-or-password-useless/

If you tell an attacker the email address is wrong, they'll try a different one. If you tell them the password is wrong, then an attacker knows that the username is correct, and can go on to try a bunch of passwords for that username until they hit the right one. So sites won't tell you which one is wrong, to try and avoid the information disclosure.

Unfortunately this assumes that there's no other way for an attacker to discover whether a username/email address is registered for a service. This assumption is incorrect.

99.9% of websites on the Internet will only let you create one account for each email address. So if you want to see if an email address has an account, try signing up for a new account with the same email address.

- also error doesn't associate itself with field.

## Social Login

https://conversionxl.com/social-login/

https://blog.loginradius.com/2014/01/understanding-benefits-social-login-add-value-website/...........
> To speak to your point, we actually find it reduces confusion due to the Account Linking feature. This allows users to sign in using multiple methods, which means they aren’t required to remember the method they used previously. Also, all methods used will be connected to a single profile of that user.

Up until recently, all login forms consisted of a username and password field. Now, we need to consider giving our users the ability to login with Facebook, Google, Twitter and a slew of others.

The idea is that

One of the great dilemmas we face in design as that of choice versus ease. More choice sounds great, but actually it can cause friction.

The idea behind loggin in with Facebook or Gmail or Twitter etc, is so that users don't have to remember more credentials. Also, some online services will integrate into social media platforms.

For example, Kidly is an online baby store that I worked on in 2016. Before building the product, they were (and still are) active on Facebook, getting to know their customers through online giveaways and quizes.

As Kidly's customers were on Facebook it made sense to offer Facebook social login capabilities. But in integrating Facebook login we had some design challenges. In providing choice we actually have to introduce a little bit of friction up front. The user now has two choices.

Then what if I sign up with Kidly with Facebook, and then later forget, and try logging in with my email? What if I sign up twice, once with email and another time with Facebook?

To handle this situation, we needed to detect whether the user had already signed up with us and offer them an error and a way to rectify things.

For example, if I have signed up with Facebook but I try to sign in by email, and that email is associated with my Facebook account, we show an error message explaining this.

[Image]()

We can perhaps do better, and offer users the choice to override that and vice versa, or perhaps we should allow users to sign in with both?

Of course these problems get worse the more options we give users. The most important thing here, as is the main theme throughout this book, is to understand how important it is to add choices and create more friction.

Pros: Users don’t have to fill out registration form, to create another usernames/passwords and to verify emails, hence can sign up in like 10 seconds instead of 10 minutes. And most important, users don’t have to remember a new usernames/passwords.
Cons: Since the information about the user is loaded automatically it raises a huge privacy concern and not everyone is likely to be happy to share her profile data. For such cases you should have traditional login system running in parallel.

## Specific Page Context

- having it appear on a specific page like login prompt for checkout

## Don't use captcha

- gds

- time hacking (https://www.sitepoint.com/3-rules-painless-account-ux-login-screens/)
- Ensure captchas are user-friendly to everyone Christopherson

## Forgot Password handling?

- Not sure I need to cover?

## sign in and sign UP

https://uxplanet.org/designing-ux-login-form-and-process-8b17167ed5b9

## Don't put two forms together on same page
