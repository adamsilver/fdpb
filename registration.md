# A registration form

We're going to begin our foray in to designing forms with a registration form. This seemingly simple form has much for us to discuss. Here it is:

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

The first thing we need to know is that each field needs an associated label. this is because:

- sighted users will be able to see the instructions;
- visually-impaired users will hear the instructions when using a screen reader; and
- motor-impaired users will find it easier to select a field thanks to the larger hit area. This is because clicking a label will move focus to the control.

We might tempt ourselves into omit labels for particular forms in order to save space but this is not something to go minimal on. Every field needs a label.

## Placeholders

Since placeholders came along, we have adopted them as means of storing hints. Their appeal lies in their minimal aesthetic and the fact they save space.

Some designers go one one step further and replace labels with placeholders. Either way, the placeholder is an Inclusive Design anti-pattern which causes problems (I've counted 13) for users. Here are 4:

1. They are easy to forget because they disappears as soon as the user types.
2. When fields are prepopulated, the fields lack clarity.
3. Placeholder text is often mistaken for a value, meaning users skip them and are subsequently shown an error.
4. They have insufficient contrast.

Some people ask me if it’s okay to use a placeholder in addition to a label. I say that if the hint is valuable to the user, we should make it easy-to-read and readily accessible, of which placeholders don’t meet these requirements.

Others say that the placeholder is just an enhancement and not essential to the user. To this I say that if the hint isn’t essential then don’t include it. Content is not an enhancement.

In the case of placeholders, minimal doesn't mean simple. If we do need to provide an additional hint we should provide it outside the field.

TODO: Image needs to go here.

## Floating labels

When I inform people of the placeholder problem, they often tell me that floating labels is the answer to all our problems. In actual fact, they come with many of the same problems as placeholders but with a few additions:

1. There is no actual space for a hint. The hint and the label are one and the same.
2. The labels are often too small. If they were to be big, we would need to leave a space big enough for the label to fly into. In which case they don't save space. They are a novelty. UIs shouldn't be novel.
3. The animation affect is distracting and disorientating.

But actually whilst decluttering the UI is a noble goal, it's only with declutting the unessential, but fixed and familiar labels are essential. I would go as far to say the floating labels are a solution looking for a problem.

## Do we really need ask for that?

Everytime we ask the user another question, we make it harder for them. So we need to be very careful about whether we even should ask it, let alone, how we ask it.

To register, does the user even need to tell us their name? If we don't have to get personal then don't ask them for it. And I mean *have* to. If it is important to have the user's name, then can we ask for it later instead?

For example, we might need their name in order to send them a product. We may need their name in order to know who we're going to call. It all depends on the service they are signing up for.

The point is that we should only ask for information that is critical and critical at this time.

Let's remove the first and last name fields, reducing the size of our form by half. But can we go further?

Medium.com have implemented a no password check in. They leverage the security of email accounts by sending the user a special login link. So technically speaking this registration form could just be a single field.

This might be taking things too far. For one people are familiar with an email address and password (although that is not a reason in itself to avoid improving the experience of course). More importantly, if the user has to sign in via their email account this experience is clunky.

We may have improved the registration process, but for people that know their password or use a password manager, this flip flopping between your site and email account can be an unnecessary source of friction.

The point of this discussion wasn't to get into the validity of email sign in. It's that we must consider whether we really really need to ask for certain information and can we make forms simpler for users by removing the need to ask.

## Label values

In the public sector, we call the epople who write copy, content designers. And I think it's a good name. They are designing content and as we say in the public sector, *content is the user experience*.

As we've seen above, including a label is paramount, but the text we put inside the label is just as vital.

The password field's label is *password* which seems simple enough. It's good to be terse. Less but better and all that. But is it missing some clarity. I would suggest that the label could encourage users to type a password they already have. Or that it might seem as they they are already registered.

Perhaps it would be better if it said "Choose password" to reinforce the fact that a) they are signing up and b) that they should create a new password.

- Labels: though shall make forms human and conversational. Don't use jargon.

## Additional hints

Whilst we discussed earlier that placeholders are not a good design pattern for hints, that does not mean the user can't benefit from a hint itself.

Our registration form could really do with a hint for the password, especially if, like most sites they are asking for a complex ruleset to pass. "Need to type x characters and one upper and lower case latter etc".

If at all possible, we should avoid making our users satisfy a complex set of password rules that are hard for them to remember (even if they are using a password manager). We can do better than this. Pass phrases are more secure and easy to remember.

https://www.smashingmagazine.com/2015/12/passphrases-more-user-friendly-passwords/

The email address doesn't really need an additional hint. Unless you think a user really needs *e.g. yourname@example.com* which is what many designers place in placeholders. A double whammy of hard to use noise.

Just because we can use a hint, doesn't mean we should.

## Why should they provide certain info.

- Reasons to complete the task

- Some forms make it obvious why they should complete the task. Some are less so. With a registration form it might well be important. So we should say as much.

- Why should the user register

## Field types

Talk about email field

Did you notice it this time? No? Look at the keyboard again. That’s right, the keyboard is different. There are dedicated keys for the @ and . characters to help you complete the field more efficiently. As we discussed with type="search", there is no downside to using type="email" right now. If a browser doesn’t support it, it will degrade to type="text". And in some browsers, users will get a helping hand.

Talk about password field

- the keyboards that are displayed on phones

## Typing a password

- Use a password reveal as explained in [this article](https://medium.com/ux-ui-ia-case-studies/masked-passwords-security-questions-captcha-and-other-unusable-security-1f018ad01378#.w8ws8yo23)

## Marking required fields

We've done a really good job already of deciding at least thinking about whether the things we're ask for are truly essential. If they are then there is actually nothing to mark as required or otherwise...

- The first thing is to flip the question. What is optional. Mark that instead.
- Do we really need first and last name.

## Combining first and last name

No. Tis complicated.

## Label and input position

- Mobile first approach, why change it?
- Though shall place labels above each control
- Position of first and last name on one line? NOPE
- Tabbing etc is better.

## Why we don't need a fieldset

Do we really need this?

## Focus styles

When the user focuses a field most browsers will highlight the field which helps the user know where they are. Some designers despise the aesthetic some browsers give by default, because it doesn't match the brand or some such. And in the mist of their hatred, they ask the developer to remove it. That would be okay if they replaced it with something better. But quite often they don't.

There is no reason why we can't replace the outline with our own style and colour. The idea would be to make it clear regardless of colour for those that suffer with vision impairments.

So do this:

TODO: Include example and visual

## Buttons

- Shouldn't have a hand cursor. They aren't links.
- Getting the text right. Not "submit".

## Validation

## When to validate

When the form is submitted or live validation.

Live validation has a bunch of problems.

## Fixing errors

## Implementation

### Server side first.

The summary.

The individual fields.

### Client side progressive enhancement.

Not using HTML 5. novalidate. No required boolean. etc. See Heydon.