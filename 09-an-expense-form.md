# An expense form

At first I wanted to fold this chapter into the last, because there's some cross-over. Adding multiple files needn't be different from adding multiple of anything really, it's just that when it comes to files, there's some widely used patterns that made sense to discuss in isolation.

There are no form controls beside file inputs, that let users add multiple of something in one go, so in this chapter we're going to walk through various patterns that are applicable to all sorts of data types including files.

If the user is tasked with adding lots of something, then the patterns in this chapter will help are certainly applicable to files too.

For example, adding collaborators on Github, invoices on Freeagent or adding information about your family members as part of an online survey. Whatever the case, the patterns in here will serve your users well.

Of course, if you know how many&mdash;let's say expenses&mdash;the user needs to enter, then simply displaying that amount of fields (and making them required) is the way to go. What's far more complicated is when you don't know how many the user needs to add.

## Ever present form pattern

The drag and drop interface designed for uploading files contained an ever present form. Users could use the form over and over to add as many files as they wanted before proceeding.

Github use a similar interface for adding collaborators. This is particularly useful pattern, for simple forms that can be submitted in one step as it's particularly simple and fast.

![Github](.)

If your expense form is simple and requires just a name and an amount for example then this simple approach is simple and inclusive by default. No enhancements needed.

![Expenses](.)

One potential downside is that as the list of items added gets bigger, the form gets pushed down the page, causing the form to be missed. Another one is that there are multiple buttons, one to add, one to leave or continue down the flow.

The last downside to this, is that it requires several requests to the server, one for each thing that's added. This is probably fine for simple forms, but it's worth considering.

## One Thing Per Page again

If adding an expense involves dynamic questions based on previous answers then the ever present form pattern won't work. In this case, using a One Thing Per Page approach may be more appropriate.

Imagine the type of expense you want to add influences the type of information the user needs to enter. For example, if it's a travel expense, then specifying car requires users to enter mileage. Specifiying public transport requires them to enter a monetary amount.

This then, requires users to enter questions in order and to be guided to provide the right information accordingly. Using a One Thing Per Page approach as discussed in ‘A Checkout Flow’ makes sense, but what happens when the user finishes adding an expense, but wants to add more?

Asking users explicitly this question is the simplest approach, because if the user ignores the question and tries to finish, validation will handle this nicely (and that's the worst case scenario).

![Do you want to add another, yes no](.)

Clicking no, finishes the task. Clicking yes, takes the user down the flow again. Simple.

This is necessary approach if the questions are dynamic. But it's particularly useful for infrequent tasks or low confidence users.

## Add another

The last pattern all takes place on a single page but this time users can complete the task with just one request to the server. To do this it'll need to be enhanced with Javascript of course and therefore needs to consider the degraded experience as well as the experience for keyboard and screen reader users.

### How it might look

![Add another](.)

Clicking add another, creates another row instantaneulsly. Pressing remove, deletes the row. This lets users add as many fields as they need before submitting it making the experience fast.

This is a particularly useful technique for high confident users who have to perform the task frequently. Saving time then is important.

### Interaction

- Where do we set focus on add and on delete?
- How do screen readers work

### JS and HTML cloning explained

- Stuff here
- Stuff here

## Summary

You can probably marry frequency of use with level of ability. The user is far more likely to have low confidence if they use the service less frequently or as a one off. Even a high confidence user, who is experiening the service for the first time could benefit from a simpler but slightly more long-winded approach.

### Things to avoid

- A
- B
- C

## Footnotes

[^]:
[^]:
[^]:

## Implementation notes

- If you don't know how many things they'll add, but you know the max they are allowed to add, then show the max. Then with JS, show/hide/reveal them.
- If you don't know the max they are allowed to add, then you'll have to offer an add another button. Without js go to a page and back (or refresh). With js, reveal an extra field OR
- Upload a file, show he file is uploaded, and ask if they want to add another with yes and no. One call to action, guide the user, but long winded for frequent users.
