# A login form

After chapter 3, *Book A Flight*, it would be good to take a break from the technical side of building custom form components. With that in mind let's look at the design challenges surrounding a simple login form.

Most sites have one, and whilst they are simple and stature, many people's sites are found wanting. On top of that, social logins make things more complicated still.

## Social login: choice vs choice paralysis

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

## The username or password is incorrect problem error message

First listen to Jared Spool (https://www.uie.com/jared-live/#design-opposed) at 42 mins.

https://kev.inburke.com/kevin/invalid-username-or-password-useless/

If you tell an attacker the email address is wrong, they'll try a different one. If you tell them the password is wrong, then an attacker knows that the username is correct, and can go on to try a bunch of passwords for that username until they hit the right one. So sites won't tell you which one is wrong, to try and avoid the information disclosure.

Unfortunately this assumes that there's no other way for an attacker to discover whether a username/email address is registered for a service. This assumption is incorrect.

99.9% of websites on the Internet will only let you create one account for each email address. So if you want to see if an email address has an account, try signing up for a new account with the same email address.

## Specific Page Context

- having it appear on a specific page like login prompt for checkout

## One thing per page?

- Like google? Overkill?

## What exactly is a username?

- if email say so.
- Why offering non standard login form, like booking ref?
- If so tell users where to find it and use the hint pattern to explain the format.

## What exactly is a password?

- if it's a pin, say so or at least explain the format

## Captures

- time hacking (https://www.sitepoint.com/3-rules-painless-account-ux-login-screens/)
- Ensure captchas are user-friendly to everyone Christopherson

## Forgot Password handling?

- Not sure I need to cover?
