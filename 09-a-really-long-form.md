# A long and complex application process

Some forms are really long&mdash;a lot longer than checkout, for example. Some forms involve users adding the same information over and over. Some involve uploading documents. Some applications involve all of these things together.

Where One Thing Per Page helps users easily fill out a form in one sitting, some online applications or case management tasks require various information and documents to be prepared over a number of weeks.

As an example of ‘case management’, people who unfortunately need help from the state can apply for government assistance in the UK. It's called Universal Credit. As part of the application process, users need to provide various information about themselves. It's a long and unweildly process that requires information about their finances, living arrangements, family members and more.

Because the process is long and complex, and because users don't always have everything they need to hand, there are certain techniques we've used to make that process easier. But that's not all. Once they have successfully made their application, there are some ongoing tasks that they have to complete to ensure continued assistance. All of this requires heavy use of forms.

Making long, unweildly and repetitive tasks easy and inclusive, requires extra design considerations. Whether your building a content management system, long-winded application process or even a case management system that requires an on-going relationship with customers, the patterns in this chapter can help.

## Task list pattern

In chapter 3, ‘A checkout flow’ I introduced you to One Thing Per Page. It's an essential pattern because it breaks longer forms into bit-size chunks and keeps momentum. A good checkout flow should take just minutes to complete for the average user. But what if a transaction takes up to an hour (or even more)?

This is where the task list pattern comes in. Simply put, it breaks down a very long form journey into several little ones that make sense to be sub-grouped. Then, as each sub-journey is completed, it's marked as completed as shown below:

![Task pattern UC](.)

This pattern lets users fill out various sections in their own time without losing progress. It also gives them a feel for how long the process is going to take. In ‘A checkout flow’ we discussed how little value there was in providing a progress indicator which makes sense given a 1 minute task. Here though, giving users an indicator of their time is essential.

If the user wants to check what they've entered they can. If they want to resume later on they can do that too. By marking each task as complete, the user can pick up where they left off easily.

### Once all tasks are complete

TODO

## Uploading multiple documents

Some tasks requires users to upload multiple documents. On the one hand, uploading a file is only marginally more complex than inputting any other information. On the other, there are some nuances that need to be taken into account.

### A file input

A file input (`type="file"`) is similar to most types of input. But instead of typing into it, you click a button that lets you browse for a file on your computer. Here's how it looks:

![File input](.)

Some designers like to restyle the file input because it's quite ugly. We know that *pretty and useless* is far worse than *ugly and useful*, but that doesn't mean beauty and aesthetics aren't important. Where possible we should marry the two together.

Styling file inputs is really tricky. Browsers mostly ignore any attempt at doing so through CSS. One approach is to visually hide the input, demarcating it via the label. Unlike file inputs, labels let are far easier to style.

After you've hidden the input and styled the label, you need a little bit of Javascript to handle the focus and selected states. When the user selects a file, the `onchange` event updates the label text to ‘1 file selected’ (or similar) as shown below.

![Hidden file input](.)

This seems like a sensible enhancement without any downsides&mdash;but it crumbles given a closer look. First, updating the label text to reflect the state is confusing. As we know, the label should describe the field. But here, when users focus back onto the field, instead of ‘Attach document’, screen readers will announce ‘1 file selected’.

Another problem is that if the field needs a hint or has to show an error, then that would normally be positioned in the label, as we set out in ‘A registration form’. The above design leaves no room for either.

The other thing you may not be aware of is that the native file input lets mouser users drag and drop files on the form control, which may be quicker than using the file explorer. Although many users aren't aware of this behaviour, hiding the input forgoes this functionality.

Together, the improvement to aesthetics just isn't worth the degradation in functionality.

### Drag and drop enhancement

- https://www.youtube.com/watch?v=hqSlVvKvvjQ

### Multiple files

HTML5 introduced the capability of uploading multiple files at once. All you have to do is add the boolean `multiple` attribute as shown below. Clicking the input triggers the same file explorer as described earlier but this time users can select multiple files at once.

![Multiple files](.)

```HTML
<input type="file" multiple>
```

Once again, on first glance this seems like a reasonable enhancement, but this also is not without problems. First, this enhancement isn't widely supported and when it degrades, it does so by reverting back to a single file input. This means users won't be able to complete the task of adding multiple documents.

Second, users can only select multiple files within a single folder. If the user's files are across different folders, they'll be stuck, unless they realise they need to move all the files into the same directory beforehand. This is a big ask for low confidence users, and hardly a satisfactory user experience.

Alternatively, we could ask users to compress multiple files into a zip file before uploading. Then we'd just need the broadly supported single file input. But this puts the onus on the user and there's a high chance some users wouldn't know what to do. Heck, some users may not even have compression software. Really, we ought to do the hard work for them.

To give users an agreeable and inclusive experience, we need to rethink the design approach. Also, this problem is worth solving in a more abstract fashion. Universal Credit, for example, doesn't just ask users to upload multiple documents, but to provide information about ‘multiple’ children too. Let's consider patterns for being able to ‘add another’.

## Add another

Patterns are always easier to understand when they are applied to real problems. I don't think there is a one-size fits-all for letting users add multiple of something, be it files or plain text.

Of course, if you know how many of something the user needs to add, then simply displaying those fields and making them required through validation is the way to go. But if you don't, then there are two approaches that broadly-speaking, work well depending on the frequency of usage.

You can probably marry frequency of use with level of ability. The user is far more likely to have low confidence if they use the service less frequently or as a one off. Even a high confidence user, who is experiening the service for the first time could benefit from a simpler but slightly more long-winded approach.

### Infrequent usage (One Thing Per Page)

1. Upload file input, press next.
2. Show success, plus ‘Do you want to add another?’
3. ‘No’ > confirmation. ‘Yes’ > back to #1.

### Frequent usage (One Page For All)

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

#### Keep form there, and add to a list

- on mobile, users have to scroll to see success or to see form.
- multiple call to actions.

## Summary

### Things to avoid

## Footnotes

## Todo?

- most sites don't need progress bar. If most wizards are small, what's the point, they take up room and haven't been shown to make much of a difference, and with any flow that is dynamic you're going ot have a problem.
- one thing per page works really well, at least as a start
- But what about a really long form. Isn't an indication of progress or ‘where’ am i  and ‘how far along am i’ useful in respecting the user. In a really long form, we can't just have users press next forever and ever, until finally reaching the end.
- But we also don't want users have a one super long form as we know that probably doesn't work. Enter the task list pattern.

## add another js differences

- If you know how many things a user needs to add then show that many fields exactly.
- If you don't know how many things they'll add, but you know the max they are allowed to add, then show the max. Then with JS, show/hide/reveal them.
- If you don't know the max they are allowed to add, then you'll have to offer an add another link. Without js go to a page and back. With js, reveal an extra field OR
- Upload a file, show he file is uploaded, and ask if they want to add another with yes and no. One call to action, guide the user, but long winded for frequent users.