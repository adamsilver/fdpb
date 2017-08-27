# A long and complex application process

Some forms are really long&mdash;a lot longer than checkout. Some forms a complicated and involve users adding lots of the same information over and over. Some involve uploading support documents. Some applications involve all of these things together.

Where One Thing Per Page helps lets users easily fill out a form in one sitting, some online applications or case management tasks require various information and documents to be prepared over a number of weeks.

As an example of ‘case management’, people who unfortunately need help from the state can apply for government assistance in the UK. It's called Universal Credit. As part of the application process, users need to declare various information. It's a long and unweildly process that requires information about their finances, living quarters, family members and a whole lot more.

Because the process is long and complex, and because users don't always have everything to hand the first time they apply, there are certain techniques we've used to make that process easier for them. Furthermore, once they have successfully made their application, there are some ongoing tasks that they have to do to ensure continued assistance. Both of these things require heavy use of forms.

In this chapter we're going to look at ways of ensuring that users know what tasks a certain transaction involves at a glance and the status of all of those tasks. And we're also going to look at patterns that make uploading multiple documents easy for low and high confidence users alike.

## Task list pattern

At any point in the flow, the user can leave and resume progress later.
The dashboard shows each process and progress and can jump back to any point in the flow, including where they left off.

## Uploading multiple files

- Ask user to zip but why ask them and they might not know how to do that.
- Input type file
- Multi-select file doesn't work. multiple upload sucks because you can't select multiple across different folders on device or desktop.
- Asking for lots of something
- offline process, do that async offline, get email to know when finished and security checked.
- Drag and drop (https://www.youtube.com/watch?v=hqSlVvKvvjQ)

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