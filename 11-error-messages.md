# Error Messages

Caroline book: search for "Use error messages that respect the effort the user is making."

Note: I cannot take credit for these principles. They were the work of my immediate teammates and other designers across government. Would need permission too. But they are awesome!

## Principle 1: Context

No matter how good an error message is, try and avoid someone ever seeing it.

Use the [question protocol](https://www.gov.uk/service-manual/design/form-structure#know-why-youre-asking-every-question) so you know why you are collecting every piece of information. This will help you write better:

* questions
* labels
* content
* hint text
* errors

The better all these are, the more likely someone is to know how to answer the question.

The question protocol will help you:

* challenge requests to ask for or store personal data – it is [illegal to store data that we do not need or will not use](https://ico.org.uk/for-organisations/guide-to-data-protection/principle-2-purposes/)
* reduce how likely someone is to make an error
* improve data quality and accuracy
* give a better experience

 You should:

* remove questions that fail the protocol
* accept all characters, numbers and punctuation symbols people may need to enter, like á, é, î, õ, ü, exclamation marks, ampersands, spaces and apostrophes
* change length restrictions
* tag and track errors to find out how often people see them
* allow different formats

Remember to [do the hard work to make it simple](https://www.gov.uk/guidance/government-design-principles#do-the-hard-work-to-make-it-simple). Do not force a particular format, like a postcode in capital letters with a space. Check what they enter, then reformat it to suit your service’s requirements.

## Principle 2: Clear and concise

Describe what has happened and tell them how to fix it. The message must:

* be in plain English
* get to the point
* use positive language
* be specific to the error
* be consistent

Do not use:

* technical jargon like “form post error”, “unspecified error” and “error 0x0000000643”
* words like “forbidden”, “illegal”, “you forgot” and “prohibited”
* “please” – avoid it because it gets in the way and implies a choice
* “sorry” – avoid it gets in the way and we are not actually sorry
* humour and personality

### Valid and invalid

Never use “invalid”.

If you use “valid” make sure people understand what it means. Never use it with people’s names.

### When to display an error message

Only display an error message if it is the right thing to do. The entry may be correct but if it gets an error there is a dead end. It would be better to take them to a screen that:

* explains why we cannot accept the entry
* tells them what to do next
* includes a way to leave the transaction

### Generic errors

Generic errors are not helpful to everyone and do not make sense out of context. Avoid messages like:

* An error occurred
* Answer the question
* Select an option
* Fill in the field
* This field is required

## Principle 3: Consistent

### On the same screen

Show an error message twice on a screen. Once in the error summary box as a link and once next to the field.

Use the same message in both places so they:

* look, sound and mean the same
* make sense out of context
* reduce the cognitive effort needed to understand what has happened
* prevent generic messages

Use the question in the error to provide context, like “Enter how many hours you work a week” for “How many hours do you work a week?”

### Across a service

There should be one way to say “Enter your first name” not 5. Errors can extend a basic message to provide more context, like:

* Enter a valid date
* Enter a valid date of birth
* Enter a valid date when you started paying Tiny Tots

### Instructions and descriptions

Instructions like “Enter your first name” are direct and natural.

But “Enter a first name that is 35 characters or less” is longer, wordier and less natural than a description like “First name must be 35 characters or less”.

A good test is to read the message out loud to see if it sounds like something you would say.

## Error summary box

Use “There is a problem” as the heading in the box. This is simple and plain and describes what has happened.

It does not use any web jargon like “form” or “submit”.

It combines with the error messages to remove the optional description of the errors. This general description rarely adds value to the screen.

For consistency use the same message in the summary and next to the field.

## Form elements and error states

Use the standard messages in your service.

### Text fields

#### Empty

Enter {whatever it is}

For example, “Enter your first name”.

#### Too long

{whatever it is} must be {number} characters or less

For example, “Address line 1 must be 35 characters or less”.

#### Too short

{whatever it is} must be {number} characters or more

For example, “Full name must be 2 characters or more”.

#### Too long or too short

Use if the validation does not know if the entry is too long or too short.

{whatever it is} must be between {number} and {number} characters

For example, “Last name must be between 2 and 35 characters”.

#### Characters

The error message you give will depend on how the field is validated. You may know what characters they typed in, but you may not.

If you do know what invalid characters have been typed in – {whatever it is} must not include {characters}

If you do not know what invalid characters have been typed in – {whatever it is} must only include {characters}

For example:

* First name must not include è and !
* First name must only include letters a to z, hyphens, spaces and apostrophes

#### Formats

This is for things like email addresses, numbers, money and telephone numbers.

Wherever possible, do not force a specific format. Validate what they enter, then reformat the entry to suit requirements.

Only include an example in the error message if there is no hint text or explanatory content on the page.

If the field is empty use the “Empty” message.

#### Email address

Enter a valid email address
Optional example for the end of the message: , like yourname@example.com

This is a general message. You should have different messages for different error states, like missing @ symbols and domain names, spaces and capital letters.

#### National Insurance numbers

Enter a valid National Insurance number
Optional example for the end of the message: , like QQ123456C

#### Telephone numbers

Enter a valid telephone number
Optional example for the end of the message: , like 01642 123 456, 07777 777 777 or +33 1 23 45 67 88

#### Not a number

{whatever it is} must be a number

For example, “Hours worked a week must be a number”.

#### Not a whole number

{whatever it is} must be a whole number

For example, “Hours worked a week must be a whole number”.

#### Number is too low

{whatever it is} must be {lowest} or more

For example, “Hours worked a week must be 16 or more”.

#### Number is too high

{whatever it is} must be {highest} or less

For example, “Hours worked a week must be 99 or less”.

#### Between 2 numbers

{whatever it is} must be between {lowest} and {highest}

For example, “Hours worked a week must be between 16 and 99”.

#### Not an amount of money

{whatever it is} must be an amount of money

For example, “Annual earnings must be an amount of money”.

#### Amount of money that needs decimals

{whatever it is} must include pence, like 123.45 or 156.00

For example, “How much you earn a week must include pence, like 123.45 or 156.00”.

#### Amount of money that cannot have decimals

{whatever it is} must not include pence, like 123 or 156

For example, “How much you earn a week must not include pence, like 123 or 156”.

#### Amount is too low

{whatever it is} must be {lowest} or more

For example, “How much you earn an hour must be 7 or more”.

#### Amount is too high

{whatever it is} must be {highest} or less

For example, “How much you earn a year must be 9,999,999,999 or less”.

### Dates

#### Empty

Enter {whatever it is}

For example, “Enter your date of birth”.

#### Invalid

Enter a valid {whatever it is}

For example, “Enter a valid date of birth”.

This is a basic message. You could have specific errors for:

* incomplete dates
* number of days in the month
* number of months in the year
* leap years

#### Future when it needs to be in the past

{whatever it is} must be in the past
{whatever it is} must be before {today’s date}

For example:

* Date of birth must be in the past
* Date of birth must be before {today’s date}

#### Future when it needs to be today or in the past

{whatever it is} must be today or in the past
{whatever it is} must be before {tomorrow’s date}

For example:

* Date of birth must be today or in the past
* Date of birth must be before {tomorrow’s date}

#### Past when it needs to be in the future

{whatever it is} must be in the future
{whatever it is} must be after {today’s date}

For example:

* Course end date must be in the future
* Course end date must be after {today’s date}

#### Past when it needs to be today or in the future

{whatever it is} must be today or in the future
{whatever it is} must be after {yesterday’s date}

For example:

* Course end date must be today or in the future
* Course end date must be after {yesterday’s date}

#### The same as or after another date

{whatever it is} must be the same as or after {date and optional description}

For example:

* Course end date must be the same as or after 1 September 2017
* Course end date must be the same as or after 1 September 2017 when you started the course

#### After another date

{whatever it is} must be after {date and optional description}

For example:

* Course end date must be after 1 September 2017
* Course end date must be after 1 September 2017 when you started the course

#### The same as or before another date

{whatever it is} must be the same as or before {date and optional description}

For example:

* Date of last exam must be the same as or before 31 August 2017
* Date of last exam must be the same as or before 31 August 2017 when Gordon left school

#### Before another date

{whatever it is} must be before {date and optional description}

For example:

* Date of last exam must be before 31 August 2017
* Date of last exam must be before 31 August 2017 when Gordon left school

#### Between 2 dates

{whatever it is} must be between {date} and {date and optional description}

For example:

* Contract start date must be between 1 September 2017 and 30 September 2017
* Contract start date must be between 1 September 2017 and 30 September 2017 when you were self-employed

### Radio buttons 

#### Yes-no

Select yes if {whatever it is is true}
 
For example. “Select yes if Sarah normally lives with you”.

#### A question with the options in it

Select if {whatever it is}
 
For example, “Select if you are employed or self-employed”.

#### A question without the options in it

Select {whatever it it}

For example, “Select the day of the week you pay your rent”.

### Checkboxes

#### A question with the options in it

Select if {whatever it is}
 
For example, “Select if you are British, Irish or a citizen of a different country”.

#### A question without the options in it

Select {whatever it it}

For example, “Select your nationality or nationalities”.