# A long and complex application process

Some forms are really long&mdash;a lot longer than checkout, for example. Some forms involve users adding the same information over and over. Some involve uploading documents. Some applications involve all of these things together.

Where One Thing Per Page helps users easily fill out a form in one sitting, some online applications or case management tasks require various information and documents to be prepared over a number of weeks.

As an example of ‘case management’, people who unfortunately need help from the state can apply for government assistance in the UK. It's called Universal Credit. As part of the application process, users need to declare various information. It's a long and unweildly process that requires information about their finances, living quarters, family members and a whole lot more.

Because the process is long and complex, and because users don't always have everything they need to hand, there are certain techniques we've employed to make that process easier for them. But that's not all. Once they have successfully made their application, there are some ongoing tasks that they have to complete to ensure continued assistance. Both of these things require heavy use of forms.

Theres are a few important things to consider: how to let users know all the tasks they need to complete at a glance and the status of those tasks. And we're going to look at slightly lower-level patterns including uploading multiple documents or in a more abstract sense, multiple of *anything*.

## Task list pattern

At any point in the flow, the user can leave and resume progress later.
The dashboard shows each process and progress and can jump back to any point in the flow, including where they left off.

## Uploading multiple documents

Some form tasks requires users to upload multiple documents. On the one hand, uploading a file is only marginally more complex than inputting any other information. On the other, there are some nuances that need to be taken into account.

### A file input

A file input (`type="file"`) is similar to most types of input. But instead of typing into it, you click a button that lets you browse for a file on your computer. Here's how it looks:

![File input](.)

Some designers like to restyle the file input because it's quite ugly. We know that pretty and useless is far worse than ugly and useful, but that doesn't mean beauty and aesthetics aren't important. Where possible we should marry the two together.

Styling file inputs is really tricky. Browsers mostly ignore any attempt to do so via pure CSS. One approach is to visually hide the input, giving sighted users access via the label.

Unlike file inputs, labels let are far easier to style. After you've hidden the input and styled the label, all you need is a little bit of Javascript to handle the focus and selected states. When the user selects a file, the `onchange` event updates the label text to ‘1 file selected’ or similar.

![Hidden file input](.)

This seems like a sensible enhancement without any downsides&mdash;unless you look beneath the surface. First, updating the label text to reflect the state is confusing. As we well know by now, the label should describe the field. But here, when users focus back onto the field, instead of ‘Attach document’, screen readers will announce ‘1 file selected’.

Another problem is that should if the field needs a hint or has to show an error, then that should be housed in the label, as we set out in ‘A registration form’. The above implementation leaves no room for either.

Lastly, although many users are unaware of this functionality, mouse users can drag and drop files onto the form control, as opposed to using the file explorer. Hiding the input forgoes this functionality.

Together, these pitfalls just aren't worth the effort for such a small improvement to aesthetics.

### Multiple files

HTML5 introduced the capability of uploading multiple files at once. All you have to do is add the boolean `multiple` attribute as shown below. Clicking the input triggers the same file explorer as described earlier but this time users can select multiple files at once.

![Multiple files](.)

```HTML
<input type="file" multiple>
```

Once again, on first glance this seems like a great usability enhancement, but this is not without its problems. First, this is not widely supported which degrades into a single select input. This means that without significant intervention users will be left with a broken interface that doesn't let them achieve their goal.

Second, users can only select multiple files within a single folder. If the users files are across different directories, they'll be stuck unless they realise they need to move files into the same directory. This is a big ask for low confidence users, and hardly a user experience that seems satisfactory.

We could ask users to compress multiple files together into a zip file and just ask for a single field, but that puts the onus on the user instead of ourselves. Really, we ought to do better than this. We need to make it easy for users to add many files. If we think about this problem in more abstract terms, we need a design pattern that lets users add many files, text values or anything else for that matter.

## Add another

Let's imagine

- If you know how many things a user needs to add then show that many fields exactly.
- If you don't know how many things they'll add, but you know the max they are allowed to add, then show the max. Then with JS, show/hide/reveal them.
- If you don't know the max they are allowed to add, then you'll have to offer an add another link. Without js go to a page and back. With js, reveal an extra field OR
- Upload a file, show he file is uploaded, and ask if they want to add another with yes and no. One call to action, guide the user, but long winded for frequent users.

## Summary

### Things to avoid

## Footnotes

## Todo?

- most sites don't need progress bar. If most wizards are small, what's the point, they take up room and haven't been shown to make much of a difference, and with any flow that is dynamic you're going ot have a problem.
- one thing per page works really well, at least as a start
- But what about a really long form. Isn't an indication of progress or ‘where’ am i  and ‘how far along am i’ useful in respecting the user. In a really long form, we can't just have users press next forever and ever, until finally reaching the end.
- But we also don't want users have a one super long form as we know that probably doesn't work. Enter the task list pattern.
- Drag and drop (https://www.youtube.com/watch?v=hqSlVvKvvjQ)