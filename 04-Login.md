# A Login Form

In chapter 3 *Book A Flight*, we *ahem*, navigated our way through the complicated world of custom form controls. I don't know about you, but I could certainly do with a break from that. And I can't think of a better way to do that, than with the inocuous and ubiquitous login form.

Most sites have login forms, and yet many of them suffer from the same common problems. Coupled with the rise of social login, login forms are an unnecessary source of friction for people. In this chapter, we're going to solve these problems and thus reduce friction.

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


We have used the same fields as we did in chapter 1 *Registration* including the show password enhancement. The only differences are the button text and the label text for the passord.

Too often, login forms forgo useful hint text. Often this is done in the name of security. The thing is, if a hacker really wants to find out the rules that govern a secure password, all they need to do is sign up for the site themselves.

And so, we'll give our users a friendly reminder, in turn giving them the chance to avoid errors. If they forget to read the hint, our fully inclusive validation patterns will kick in anyway.

## Non standard username and password fields

Some online services don't have a standard login form with email address and password fields. The flight service we designed in chapter 3, may ask users to enter their unique bookin reference number in order to login.

Similarly, some websites don't ask for traditional passwords. For example, PayPal asks for a pin number. Whether it's the username, the password or both, we should be explicit about what we expect users to type.

If it's a unique booking reference number, for example, then we should tell users where they might find it, much like we do with the credit card security number on the back of the card.

This gives users less to think about, and avoids an unnecessary validation error that will only explain the same thing as the hint, after submission.

In short, be clear, not ambiguous.

## The username and password doesn't match

If the user enters an incorrect username or password, many sites will show an error saying *The username and password doesn't match*. Put simply, this is bad and as Jared Spool explains in Design Is Metrically Opposed[^]:

> First we know which one doesn't match, we're just not going to tell you, because our security people think that if we told you that it was the password, they would know they had a legal username and they would try every possible password in history.*

In reality hackers don't do this and in anycase most websites only let you sign up for one account. So if they really wanted to find out, they could sign up for an account with that username.

Instead, we should treat humans like human beings. Tell them what is going on so they can login easily.

## Contextual login forms

Another common problem is login forms that are designed out of context. Taking a checkout process for example. Normally a checkout flow has it's own simplified layout with minimalist headers and footers. If the user presses checkout from the basket page, then they should expect to enter the checkout flow.

Now, if the user must login to checkout, then we should display the login form in context of the checkout. That is using the checkout *layout*. This is exactly what we did for Kidly.

It reinforces that the user is midway through a process and helps them focus. That's the reason for the dedicated checkout layout in the first place.

Conversely, Tesco don't do this. When I go to add a product to the basket, they force me to login, but in doing so, I'm taken somewhere else which is disorientating.

![Forced to login](./images/tesco-logged-out.png)

![Login form](./images/tesco-login-form.png)

## Social Login

Up until recently, most sites only offered people one way of logging in. That's with a username and password, or email address and password like the one we have so far designed.

In truth many sites still only offer users one way of logging in. However, more and more sites are offering users another option: social login. Specifically, the ability to sign in to a site without having to setup another username and password. Instead, people can sign in with Facebook, Twitter and a slew of other social platforms.

The idea being that a) most people already have an account for one of those, and b) that it saves them time and hassle. However, with choice comes choice *paralysis*. If there are many ways to login then users have to *decide*, which takes up energy. This energy is limited.

In addition to this, what if I signed up to the site twice, once with Facebook and once with the standard form? Handling this situation is important. Users shouldn't have to remember which one in an ideal world.

Logging in with Facebook, for example is not just about ease but also about integration. It might be that an app will post automatically for you on Facebook for example.

When we were building Kidly, we engaged our potential customers on Facebook. That's where they were so we went to them. We had competitions and giveaways and to this day Kidly have good rapport with their customers on Facebook.

When it came to designing Kidly's login form it made sense to offer this functionality. As with most things in this book, it's so important we take the time to understand why. And reseach with users.

There is a case for providing an MVP without social login, or alternatively social login *only*. It's less work, and it gives you time and space to test one feature change at a time.

If you do end up adding social login, then there are a few considerations:

1. Design the login page clearly
2. Allow users to login with both interchangeably.

The first is quite straightforward. Make it clear there is two ways to sign in. Here's what we did for Kidly:

![Login form](./images/kidly-login.png)

The second is a little more complicated. At Kidly, we helped users by showing them an error message. For example, if I have signed up with Facebook but I try to sign in by email, and that email is associated with my Facebook account, we show an error message explaining this.

![Login form](./images/kidly-error.png)

But when I left the project, there was an item in the backlog to allow users to login interchangebly and connect two different accounts if need be.

---

https://conversionxl.com/social-login/

https://blog.loginradius.com/2014/01/understanding-benefits-social-login-add-value-website/...........
> To speak to your point, we actually find it reduces confusion due to the Account Linking feature. This allows users to sign in using multiple methods, which means they aren’t required to remember the method they used previously. Also, all methods used will be connected to a single profile of that user.

Pros: Users don’t have to fill out registration form, to create another usernames/passwords and to verify emails, hence can sign up in like 10 seconds instead of 10 minutes. And most important, users don’t have to remember a new usernames/passwords.

Cons: Since the information about the user is loaded automatically it raises a huge privacy concern and not everyone is likely to be happy to share their profile data.

## Forgot Password handling?

Put the forgot password link after the form. It's a secondary action and we don't want to disrupt the tab sequence.

## sign in and sign UP

https://uxplanet.org/designing-ux-login-form-and-process-8b17167ed5b9

## Don't put two forms together on same page

## Summary

Todo

## Footnotes

Here
