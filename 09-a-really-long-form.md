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

Some tasks involve users uploading multiple documents. On the one hand, uploading a file is only marginally more complex than inputting any other information. On the other, there are some nuances that we need to take into account.

### A file input

A file input (`type="file"`) is similar to most types of input. But instead of typing into it, you click a button that lets you browse their computer or device for a file. Here's how it might look:

![File input](.)

Some designers want to restyle the file input because it's quite ugly to look at. We know that pretty and useless is worse than ugly and useful, but that doesn't mean beauty and aesthetics aren't important. Where possible we should marry the two together.

Styling file inputs is really tricky. Browsers mostly ignore any attempt and certainly doesn't give you access to the pieces inside it. It's shadow dom stuff[?terminology adam!?].

One approach is visually hide the input, letting users activate the control with the label. Unlike the input, labels let are stylistically malleable. Then Javascript is used to handle focus states and update the label text to ‘file.pdf is selected’. On first glance this seems like a reasonable enhancement but it's problematic for a few reasons:

- Updating the text of the label is confusing. The label should describe the field, not the state of it. That's what the input is for.
- This approach renders the label, hint and error pattern we formed in ‘A registration form’ useless. There is no room to provide a hint or error message.
- Users can drag and drop files onto the input, rather than selecting from a dialog. Admittedly lots of users aren't aware of this capability but this approach removes it altogether.

These pitfalls just aren't worth the effort and problems they give users such a small improvement to aesthetics.

### Multiple files

HTML5 introduced the capability of uploading multiple files at once. All you have to do is add the boolean `multiple` attribute as shown below. Users trigger the same file explorer dialog but this time they can select multiple files at once.

![Multiple files](.)

```HTML
<input type="file" multiple>
```

On first glance this seems like a really useful usability enhancement. But there are two major problems.

There are several problems:

- doesn't work everywhere and degrades into a single select file input.
- can only select from a single folder (not across folders)





- Ask user to zip but why ask them and they might not know how to do that.

- Drag and drop (https://www.youtube.com/watch?v=hqSlVvKvvjQ)
- removing input inplace of label. Ugly file input.

## Add another notes

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