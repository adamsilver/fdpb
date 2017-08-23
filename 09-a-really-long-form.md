# A really long form

## Add another

- If you know how many things a user needs to add then show that many fields exactly.
- If you don't know how many things they'll add, but you know the max they are allowed to add, then show the max. Then with JS, show/hide/reveal them.
- If you don't know the max they are allowed to add, then you'll have to offer an add another link. Without js go to a page and back. With js, reveal an extra field OR
- Upload a file, show he file is uploaded, and ask if they want to add another with yes and no. One call to action, guide the user, but long winded for frequent users.

##


At any point in the flow, the user can leave and resume progress later.
The dashboard shows each process and progress and can jump back to any point in the flow, including where they left off.

‘add another’
‘Task list’
‘Upload’

What we know:

- most sites don't need progress bar. If most wizards are small, what's the point, they take up room and haven't been shown to make much of a difference, and with any flow that is dynamic you're going ot have a problem.
- one thing per page works really well, at least as a start

But what about a really long form. Isn't an indication of progress or ‘where’ am i  and ‘how far along am i’ useful in respecting the user. In a really long form, we can't just have users press next forever and ever, until finally reaching the end.

But we also don't want users have a one super long form as we know that probably doesn't work. Enter the task list pattern.

## Task list pattern

## Upload

- input type file
- offline process, do that async offline, get email to know when finished and security checked.
- allow to select multi files and lack of support
- way to do this without it?
- drag drop (https://www.youtube.com/watch?v=hqSlVvKvvjQ)


Ask user to zip but why ask them and they might not know how to do that.

multiple upload sucks because you can't select multiple across different folders on device or desktop.

Better to handle through intentional design.