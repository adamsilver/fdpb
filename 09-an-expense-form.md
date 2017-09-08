# An expense form

At first I wanted to fold this chapter into the last one because both problems overlap. Adding multiple files needn't be different from adding multiple of anything really, it's just that drag and drop is only applicable to files so it made sense to tackle that in isolation.

Unlike file inputs, other form controls don't let you add multiple values. It's up to us to design solutions from scratch that solve this need. I've counted three main approaches here that are applicable to all sorts of data&mdash;files included.

Whether it's adding collaborators on Github, invoices on Freeagent or a survery asking for details about each of your family members, the patterns here are more than worthy of your consideration.

Of course, if you know how many things the user needs to enter, then simply displaying that amount of fields (and making them required) is all you need. That's not really a pattern as such. No, these patterns are applicable only when you don't know how many the user will add.

## Ever present form pattern

The drag and drop interface designed for uploading files employed the ever present form pattern. The infamous ‘Todo list pattern’ [^] also uses it. Even Github uses the same technique when adding collaborators.

The way it works is to have an ever present form on the page. Submitting the form adds an item to a list above (or beside) the form. When the user is done, they can proceed. This is particularly useful pattern for simple forms that can be submitted in one step. No enhancements needed.

The potential downside is that as the list grows, the form is pushed down (at least on mobile). Additionally there are multiple buttons: one to add and one to proceed which could cause mild delays. Where possible aim for one primary call to action, reducing choices requires less thinking. Lastly, each submission, requires a trip to the server. Whilst probably not a big deal, it's worth keeping in the back of your mind as we discuss other solutions shortly.

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

Unless you’re careful, the answer is something very annoying for keyboard users, including screen reader users.

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

Ignore empty states at your peril (heydon)
