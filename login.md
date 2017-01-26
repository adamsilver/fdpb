# A login form

- https://www.sitepoint.com/3-rules-painless-account-ux-login-screens/
- Forgot password?
- Multiple social logins? That's hard work.
- having it appear on a specific page like login prompt for checkout
- different kinds of login - username/email/booking ref (always need a label and tell the user where to find it)

## The username or password is incorrect problem error message

https://kev.inburke.com/kevin/invalid-username-or-password-useless/

If you tell an attacker the email address is wrong, they'll try a different one. If you tell them the password is wrong, then an attacker knows that the username is correct, and can go on to try a bunch of passwords for that username until they hit the right one. So sites won't tell you which one is wrong, to try and avoid the information disclosure.

Unfortunately this assumes that there's no other way for an attacker to discover whether a username/email address is registered for a service. This assumption is incorrect.

99.9% of websites on the Internet will only let you create one account for each email address. So if you want to see if an email address has an account, try signing up for a new account with the same email address.
