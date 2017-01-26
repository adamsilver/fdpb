# A registration form

We're going to begin our foray with a registration form. This simple form has much to discuss and so starting with a simple example seems like a good thing to do. Here it is:

```html
<form id="register">
  <label for="firstName">First name</label>
  <input type="text" id="firstName" name="firstName">
  <label for="lastName">Last name</label>
  <input type="text" id="lastName" name="lastName">	
  <label for="email">Email address</label>
  <input type="email" id="email" name="email">		
  <label for="password">Password</label>
  <input type="password" id="password" name="password">
  <input type="submit" value="Register">
</form>
```

## Labels

The first thing we need to know is that each interactive form element needs an associated label. The submit button is not a vessel for input and so it doesn't need an explicit label. It's description will be taken from its value.

The `for` attribute associates the label with an input using its `id` value. This is one of the reasons we should ensure `id`s are unique.

But, why exactly are labels so important? They are important because:

- sighted users will be able to see the instructions;
- visually-impaired users will hear the instructions when using a screen reader; and
- motor-impaired users will find it easier to select a field thanks to the larger hit area. This is because clicking a label will move focus to the control.

## Placeholders

## Floating labels

## Label values

- The importance of copy
- Why should the user register
- Why should they provide certain info.

## Field types

- email

Did you notice it this time? No? Look at the keyboard again. That’s right, the keyboard is different. There are dedicated keys for the @ and . characters to help you complete the field more efficiently. As we discussed with type="search", there is no downside to using type="email" right now. If a browser doesn’t support it, it will degrade to type="text". And in some browsers, users will get a helping hand.

- password
- the keyboards that are displayed on phones

## Marking required fields

- The first thing is to flip the question. What is optional. Mark that instead.
- Do we really need first and last name.
- Should those fields be combined - no - complicated

## Label and input position

- Position of first and last name on one line? NOPE

## Size of the fields

## Why we don't need a fieldset

## Focus styles

## Typing a password

- Use a password reveal as explained in [this article](https://medium.com/ux-ui-ia-case-studies/masked-passwords-security-questions-captcha-and-other-unusable-security-1f018ad01378#.w8ws8yo23)

## Validation

- Don't use HTML5 validation, such as required.
- novalidate
- Fixing errors
- server side first.
- The problem with live validation