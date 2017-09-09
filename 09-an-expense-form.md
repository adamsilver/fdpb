# An expense form

At first I wanted to fold this chapter into the last one because there is certainly some overlap. Adding multiple files needn't be different from adding multiple of anything else really, it's just that drag and drop is only applicable to files so it made sense to tackle that in isolation.

Unlike file inputs, other form controls don't let you add multiple values. It's up to us to design solutions from scratch that solve this need. There are three main approaches that are applicable to all sorts of data&mdash;files included.

Whether it's adding collaborators on Github, invoices on Freeagent or adding details about each of your children on a survey, the patterns here are useful.

Of course, if you know how many things needed to be added upfront, then simply displaying that amount of fields (and making them required) is all you need to do but that's not really a pattern. These patterns are applicable only when you don't know how many are needed.

## Ever present form pattern

The drag and drop interface from ‘An upload form’ in many respects uses this pattern and so does the infamous ‘Todo list pattern’[^]. Even Github uses the same technique when adding collaborators. What I'm trying to say is that it's likely you've seen this in action before.

The way it works is to have an ever present form on the page (hence the name). Submitting the form adds an item to a list above (or beside) the form. When the user is finished, they can proceed. This is particularly useful for simple forms that can be submitted in one step.

The potential downside is that as the list grows, the form is pushed down (at least on mobile).

Also, there are multiple buttons: one to submit and one to proceed which could cause mild confusion. As mentioned in previous chapters, providing one button is preferrable because it requires less thought.

The other thing is that, each submission creates a request to the server. Not necessarily a big deal, but it could add up depending on frequency.

## One Thing Per Page flow

Imagine you need to add a bunch of expenses. But the type of expense determines the information you need to enter. For example, if you're expensing a car, then you need to enter mileage, but if you're expensing a train ticket, then it's just a monetary amount.

![Show the flow with branching diagram](.)

In this case, the ever present form pattern is less suitable. Instead, using One Thing Per Page as first explained in ‘A checkout flow’ is appropriate because it guides the user to provide the right information at the right time.

But what if the user wants to add another one? Simply add a question asking the user if they'd like to add another (or not). No, finishes the task. Yes, takes the user back through the flow. This way the user is guided down a familiar path. If the user doesn't answer the question, then validation has them covered.

![Do you want to add another, yes no with button](.)

This is useful pattern if you need to branch but it's also a good approach for infrequent tasks and low confident users.

## Add another

This pattern consists of one form, on one page, submitted in single step. It works by using Javascript to create extra form fields instantaneously. This is particularly useful for high confident users who are familiar with performing the task frequently.

### How it might look

![Add another](.)

Clicking add another creates a new row. Pressing remove deletes a row. Users can keep adding rows until they're done, submitting all of the fields in one go which expedites the process.

If you're wondering about the degraded experience, then good. It's exactly the same except the page refreshes each time the buttons are pressed.

### Managing focus

Throughout the book, we've considered how things work for keyboard users. When the add button is pressed, focus should be set to the first newly-created form field. This is useful for screen reader users too, as the announcement of that field naturally prompts the user to continue.

When the user clicks the remove button, the fields in the row, including the button itself are removed. What happens to the focus when you delete the currently focused element? In ‘A todo list’[^], Heydon Pickering explains exactly what happens:

> [...] browsers don’t know where to place focus when it has been destroyed in this way. Some maintain a sort of “ghost” focus where the item used to exist, while others jump to focus the next focusable element. Some flip out completely and default to focusing the outer document — meaning keyboard users have to crawl through the DOM back to where the removed element was.

We could set focus to the previous or next item but this is more confusing than anything else. Alternatively, we could set focus to the add another button, but that's both presumptuous and a little considering they just removed an item.

Instead, we'll set focus to the heading at the beginning of the form and use a live region to provide more explicit feedback.

### Feedback

Only need live region for screen readers because the act of removing the item is feedback itself.

### Cloning explained

- Stuff here
- Stuff here

### Differentiating between labels

TODO: Context for which number item the user is referencing.

## Summary

### Things to avoid

- A
- B
- C

## Footnotes

[^]:
[^]:
[^]:

## Todo notes

- Ignore empty states at your peril (Heydon)