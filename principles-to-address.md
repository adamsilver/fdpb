# Principles to address

## Use the right field for the right keyboard

Good quote on number keypad: http://simplyaccessible.com/article/8-things-parenting-taught-accessibility/

## password tweet

https://twitter.com/azumbrunnen_/status/462584313209696258

## Auto file

https://cloudfour.com/thinks/autofill-what-web-devs-should-know-but-dont/

## Primary principles

- Though shall make label text extra clear. Password vs "Current password" or "New password".

- Though shall put form elements in a single column.

- Placeholders - avoid - help or hint text should be outside field.

- Only ask for absolutely necessary stuff. Ask for it as late as possible. Progressive. If there are optional fields, mark those; not the required fields. 300 million button jared spool. Mr Joe, Boring Forms.

- Explain why users should give us information or sign up. Mobile for text message updates on their delivery. Sign up for x and y and z. If no x and y - why asking for it? Jared Spool video about how got loads of sign ups but no sales. We want the right investment. For example growth labs by whats his chops makes u go thru an entire email funnel before signing up for his course. No link to get there without it. Investment => proper sign up and longevity. Something to consider. https://www.uie.com/jared-live/#design-opposed

## Validation

- Enter should submit the form.

- No live validate. Don't hand hold too early. (keeping forms small means less room for error and more focus etc).

- Summary errors: show error messages as a group at the top.

- Inline errors: show in-context of the erroneous control.

- Forgive mistakes: trim, spaces, uppercase etc. date of birth, postcode fields. Jared spool, 42 mins, Design Is Metrically Opposed: "it takes one line of code to trim brackets and dashes from a telephone number, but it takes 10 lines to tell the user they typed something wrong".

- CCV code disappears once the page refreshes and says "ur card number was wrong" - also in the same video above.

- Success messages: Once shown, don't hide automatically. Let theuser dismiss if at all. Probably want to take them somewhere rather than show a success above the form. Back to account landing, on to the next step. back to home page as last resort etc.

## These require specific examples

- Fieldset and legend for a checkbox or radio group

- Though shall use 1 field instead of 3 i.e. the date field

- Though shall only reveal a form when one is asked for. Progressive disclosure.

- Though shall avoid duplication i.e. for password fields

- Though shall indicate max-length but not stop entry.

- Though shall use smart defaults for radios etc.

- Though shall split up long forms depending on context.

- Though shall always design a form based on context, not on reuse. Kidly onboarding expecting vs has kids.

- Though shall not use unfriendly defaults. Anti pattern. "Uncheck the box if you don't wish to get email"

- Though shall ensure captchas are user-friendly to everyone Christopherson

- Though shall not to try to customise the look and feel too much

- Though shall use AJAX for form submission with extreme caution

- Though shall give the user a sense of positive affirmation. Checkout progress bar, success messages. Tick is only as good as the client side checks unless using ajax which is ill-advised.

## These are rare and low priority

- Though shall not use sliders unless they really have to (maybe a colour picker) (Mr Joe again)

- Though shall avoid multiple select controls

- Though shall use select boxes as a last resort. Select box grouping with opt.

- Though shall not misuse the select box. Use checkbox instead.

- Make sure space below the combined label/field is more. https://uxdesign.cc/design-better-forms-96fadca0f49c#.iy0c5in6p
