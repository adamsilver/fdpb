# A file upload form

Some tasks involve users having to upload documents. On one hand, uploading a file is only marginally more complex than inputting any other type of data. On the other, there are some nuances that need to be taken into account.

Uploading multiple documents increases the design challenge by orders of magnitude and actually we often need to design interfaces that allow users to add multiple of anything, not just files. Let's start small and end big.

## A file input

A file input (`type="file"`) is similar to most types of input. But instead of typing into it, you click a button that lets you select a file from your device or computer. Here's how it looks:

![File input](.)

```HTML
<input type="file">
```

Some designers like to restyle the file input because it's quite ugly. We know that *pretty and useless* is far worse than *ugly and useful*, but that doesn't mean beauty and aesthetics aren't important. Where possible we should marry the two together.

Styling file inputs is really tricky because browsers mostly ignore any attempt at doing so with CSS. One approach is to visually hide the input, demarcating it solely via the label. Unlike file inputs, labels are far easier to style.

![File input just a label](.)

Having hidden the input, and styled the label, you still need a little Javascript to handle the focus states&mdash;similar to what was set out in “Book a flight” for the seat chooser. Lastly, when the user selects a file for upload, the `onchange` event updates the label text accordingly.

![Hidden file input](.)

On the face of it, this implementation is not only valuable, but it's also accessible. Keyboard and mouse users can still operate it like normal and screen readers can announce the state of the input and associated (visually hidden) label.

But operating the interface is not the only thing to consider here and unfortunately, this enhancement crumbles under scrutiny.

First, updating the label text to reflect the state is confusing because the label should describe the field and remain unchanged regardless of state. In this case, screen reader users will hear ‘some-file.pdf selected’ (or similar) as opposed to ‘Attach CV’.

Second, this visual design makes no allowances for a hint or error message which is normally positioned inside the label, as set out in ‘A Registration Form’.

Third, file inputs let mouse users drag and drop files. The input itself acts as a ‘dropzone’, which may be preferable for savvy users. Hiding the input means forgoing this functionality.

Together, the improvement to aesthetics just isn't worth the degradation in functionality.

## Multiple file input

Some tasks involve having to upload multiple files at once. One way of letting users do this, is by adding a `multiple` attribute onto the file input. It spawns the same file explorer but lets users select multiple files.

![Multiple files](.)

```HTML
<input type="file" multiple>
```

This innocuous boolean attribute grants a lot of power and seemingly solves the multiple file problem in one hit, but it's not straightforward.

The first problem is that users can only select files within a single directory. If they want to upload files across several folders they'll be stuck, unless they realise they have to move all the files into a single folder beforehand. This is a big ask for low confidence users, and hardly satisfactory for more confident users.

Alternatively, we could ask users to compress multiple files into a zip file before uploading. In this case, we'd just use the broadly supported single file input. But this puts the onus on the user again. They may not have compression software or even know what it is, let alone how to use it.

Most importantly though, is that this enhancement isn't widely supported&mdash;even if it had good support, we'd still want to consider what happens in  browsers that don't. In this case, it reverts back into a single file input, meaning users won't be able to upload more than one document. This degradation leaves the interface potentially broken.

One option would be to show the single file input afterwards, or give users an opportunity to go back and do it again naturally.

We're going to have think of something better than this if we want to give users something simple and inclusive. Let's keep going.

## Drag and drop enhancement

As pointed out earlier, file inputs already have drag and drop functionality so needing to create our own enhanced interface may seem a little redundant at first. As usual, it's best to determine a user need before embarking on such an enhancement, but there are some issues with the standard drag and drop functionality.

First, it's not immediately obvious that a file can be dragged onto the file input. There's no guidance or affordance that indicates this. Second, the hit area is quite small, which makes it hard to use for motor impaired users. Creating our own enhancement is an opportunity to fix both of these problems.

Let's first remind ourselves of principles 1 and 5:

1. **Provide comparable experience**. Ensure your interface provides a comparable experience for all so people can accomplish tasks in a way that suits their needs without undermining the quality of the content.

5. **Offer choice**. Consider providing different ways for people to complete tasks, especially those that are complex or non standard.

A drag and drop enhancement is inherently tied to mouse users. So the first thing we want to do is to give users a choice to either Drag and Drop or to use an alternative interface: one that is not only suited to keyboard users, but that might also be preferential to mouse users too. In doing so we conform to principles 1 and 5 at the get go.

How it might look:

![Drag and drop choice interface](.)

And the enhanced HTML:

```HTML
<div class="dropzone">
	<p>Drop files here. Yada.</p>
	<button type="button">Browse files</button>
</div>
<div class="addAnotherStuff">
	All of this would have to be visually hidden so that it works for screen reader users. Or not, because they can focus the button and reveal all this stuff?
</div>
```

Then Javascript is used to listen for `ondragenter`, `ondragleave` and `ondrop` events. The first two events are merely for highlighting and unhighlighting the drop zone to give users feedback.

The majority of the work happens when the user drops the files onto the zone. The event object contains the information about the dropped files. The script then has to extract that information and send it to the server for processing using AJAX.

When the files are being uploaded we need to show progress, just like the browser normally would. We can do this by using the `progress` event fired by the XMLHttpRequest object.

![Progress](.)

```HTML
<progress>
```

```JS
(xhr.upload || xhr).addEventListener('progress', function(e) {
    var done = e.position || e.loaded
    var total = e.totalSize || e.total;
    var percentage = Math.round(done/total*100) + '%'
    // update progress bar.
});
```

Then when the request is finished, we simply output the uploaded files, giving users the chance to review them.

![Review and delete and drag more](.)

Notes:

- Feature detection
- Dragging and dropping with Javascript can only work when using AJAX. That is you can't populate the file inputs. That means a request occurs for every drop immediately.
- Use Gmail approach. Let users drag and drop files uploaded with AJAX. Then let the user submit the form to save and finish/move onto next step if there is one.
- If users need to drag files across different folders, they'll have to perform this task many times.
- When Javascript is off, or feature detection fails, or if the user chooses not to use Drag and Drop we need to provide an agreeable experience. Discussed next.

To give users an agreeable and inclusive experience, we need to rethink the entire design approach.
Also, this problem is worth solving in a more abstract fashion. Universal Credit, for example, doesn't just ask users to upload multiple documents, but to provide information about ‘multiple’ children too. Let's consider patterns for being able to ‘add another’.

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

- on mobile, users have to scroll to see success or to see form. OR JUST USE A FLOW again for degraded version.
- multiple call to actions.

## Summary

### Things to avoid

- Forgoing the file input for reasons of aesthetics
- Multiple file inputs

## Footnotes

## Todo?

- https://www.youtube.com/watch?v=hqSlVvKvvjQ

## add another js differences

- If you know how many things a user needs to add then show that many fields exactly.
- If you don't know how many things they'll add, but you know the max they are allowed to add, then show the max. Then with JS, show/hide/reveal them.
- If you don't know the max they are allowed to add, then you'll have to offer an add another link. Without js go to a page and back. With js, reveal an extra field OR
- Upload a file, show he file is uploaded, and ask if they want to add another with yes and no. One call to action, guide the user, but long winded for frequent users.
