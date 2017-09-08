# An expense form

## Add another

The patterns discussed so far have revolved around uploading multiple *files*. But what if the user needed to add multiple collaborators, expenses or invoices into a system?

There is no form control that provides this functionality, so we either need to create one or to design something that works as part of a journey. This is where the ‘Add another’ pattern can help and there are many variations of this pattern. Really, it's a catch-all term for being able to add lots of something.

Of course, if you know how many&mdash;let's say expenses&mdash;the user needs to enter, then simply displaying that amount of fields (and making them required) is the way to go. What's far more complicated is when you don't know how many the user needs to add.

### Add another flow

> Good for infrequent usage/low confident users/dynamic questions.

If the user wants to add an expense they might be asked first the type, and based on the type might see other questions. For example if the user is adding an expense for a car then perhaps they provide mileage. When adding a lunch expense, it's simply an amount.

This requires the user to answer questions in order. First type, then the amount of pounds or mileage.

Once the user has added one expense, they're asked if they want to add another. Clicking yes, goes round again, clicking no ends the task.

### Add another: keep form there

> Good for speeding a simple process up that doesn't consist of dynamic questions

In short, show a form, and submitting it adds an item to the list. The user can then just keep submitting the form. This is a bit like the drag and drop interface. It stays there until users are done.

- on mobile, users have to scroll to see success or to see form. OR JUST USE A FLOW again for degraded version.
- multiple call to actions.

### Add another: Dyanmic JS injection

> Good for frequent high confidence users who need to get the job done quickly.

#### Add another button

1. Upload a file with Next and Add Another buttons.
2. ‘Add another’ reveals and focuses onto new field(s). ‘Next’ completes the task.
3. When another field is cloned, a Remove button is added. Clicking it removes the field and sets focus to where?

- users could miss secondary action and do the minimum. It doesn't guide the user.

#### Additional question

1. Upload file input, ‘Do you want to add another YES NO’ and Next button.
2. Clicking yes reveals and focuses onto new field(s).
3. Clicking Next without answering YES or NO generate validation error. Otherwise shoud confirmation.

- This forces the user to answer the question solving previous problem but adds another problem. That we're moving focus via selecting an input so user can't switch and breaks convention.

## add another js differences

- If you know how many things a user needs to add then show that many fields exactly.
- If you don't know how many things they'll add, but you know the max they are allowed to add, then show the max. Then with JS, show/hide/reveal them.
- If you don't know the max they are allowed to add, then you'll have to offer an add another link. Without js go to a page and back. With js, reveal an extra field OR
- Upload a file, show he file is uploaded, and ask if they want to add another with yes and no. One call to action, guide the user, but long winded for frequent users.

## Nice sentence perhaps

You can probably marry frequency of use with level of ability. The user is far more likely to have low confidence if they use the service less frequently or as a one off. Even a high confidence user, who is experiening the service for the first time could benefit from a simpler but slightly more long-winded approach.


More abstractly, users don't just need to add multiple *files*. They may need to add multiple of anything other type of data too.

So whilst the main focus of this chapter is around file uploads, we'll also be looking at the design pattern in a more abstract sense too.
