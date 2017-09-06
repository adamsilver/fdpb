# A file upload form

Some forms involve users having to upload files: document, images or anything else really. On the one hand, uploading a file is only marginally more complex than inputting any other type of data. On the other, there are some nuances and opportunites that need to be taken into account.

The problem increases by orders of magnitude as soon as you need to let users upload *multiple* files in one go. Interestingly, we sometimes need an interface that lets users add multiple *anything*, not just files. In this chapter, we're going to start small, but we'll end big.

## A file input

A file input (`type="file"`) is similar to most types of input. But instead of typing into it, you click a button that lets you select a file from your device or computer. Here's how it looks:

![File input](.)

```HTML
<input type="file">
```

Some designers like to restyle the file input because it's quite ugly. We know that *pretty and useless* is far worse than *ugly and useful*, but that doesn't mean beauty and aesthetics aren't important. Where possible we should marry the two together.

Styling file inputs is really tricky because browsers mostly ignore any attempt at doing so in CSS. One approach is to visually hide the input, demarcating it solely via the label. Unlike file inputs, labels are far easier to style.

![File input just a label](.)

Having hidden the input, and styled the label, Javascript should be used to handle the focus states&mdash;similar to what was set out in “Book a flight” for the seat chooser form component. When the user selects a file for upload, the `onchange` event updates the label text accordingly.

![Hidden file input](.)

On the face of it, this implementation is not only valuable, but it's also accessible. Keyboard and mouse users can operate it like normal and screen readers can announce the state of the input and associated label&mdash;because it's only hidden visually.

But operating the interface is not the only thing that needs consideration. Unfortunately, this enhancement crumbles under further scrutiny. Let's take a look why that is now.

First, updating the label text to reflect the state is confusing because the label should describe the field and remain unchanged regardless of state. In this case, screen reader users will hear ‘some-file.pdf selected’ (or similar) as opposed to ‘Attach CV’.

Second, visually this design makes no allowances for a hint or error message which is normally positioned inside the label, as set out in ‘A Registration Form’.

Third, file inputs let mouse users drag and drop files. The input itself acts as a ‘dropzone’, which may be preferable to savvy users. Hiding the input means forgoing this functionality.

Overall, the improvement to aesthetics just isn't worth the degradation in functionality and utility.

## Multiple file input

Some tasks involve having to upload multiple files at once. One way of letting users do this, is by adding a `multiple` attribute onto the file input. It spawns the same file explorer but lets users select multiple files.

![Multiple file input](.)

```HTML
<input type="file" multiple>
```

This inocuous attribute grants a lot of power and seems to solve the multiple file problem entirely, but it's not perfect.

First, users can only select files within a single directory within the file explorer. If they want to upload files residing across different folders they'll be stuck unless they move all the files into a single folder beforehand which puts the onus on the user.

Second, some browsers don't recognise the `multiple` attribute enhancement. People who use one of these browsers will get a single file input. Depending on the design, this will result in a broken experience because for users who want (or are required) to upload multiple files, they won't be able to upload more than one.

This problem may naturally solve itself simply by considering the full journey.

![1. User uploads file(s)](.)
![2. Confirmation of uploaded file, can delete it, add another or continue/finish](.)
![3. Selecting another starts back at #1 again](.)

Notice how this journey also works for those using the enhanced multiple file input. And if they have files residing across multiple directories this caters for that too.

## Drag and drop enhancement

As noted earlier, file inputs (both single or multiple) allow users to drag and drop files onto the control. The problem with the native behaviour is two-fold:

- It's not immediately obvious that a user can drag and drop files onto the input as there's no guidance or affordance to indicate this.
- The hit area of the control is quite small, which makes it hard to operate for motor-impaired users.

By creating our own drag and drop enhancement, we get the opportunity to solve both of these problems. Here's how the enhanced interface might look:

![Drop zone](.)

You'll notice that the interface is geared toward mouse users. Presenting an interface that consists of both the standard file input as well as a drop zone is a cognitive burden. Also, the design goes completely against the earlier advice regarding hiding the file input; using the label; and showing state with Javascript's onchange event.

But that was because the enhancement was purely a visual one. The drag and drop enhancement changes the behaviour significantly, meaning aesthetics and behaviour work in harmony here. Here's why:

- Dropping files onto the dropzone immediately upload them using AJAX. Similarly, using the file input will immediately upload the files using AJAX.
- When the AJAX request is made, the file input is reset, meaning users can continue to upload more files should they wish to.
- There is no need for a hint, but if we needed one, this interface allows for this.
- The label text is fine. So for screen readers, focusing the file input will read ‘Attach files or drag and drop’. Choosing one immediately uploads the file as mentioned above.
- Dealing with errors and loading states is fine, as each of the progress bars will be live regions giving users the chance to delete the items and start over.

Here's the enhanced dropzone HTML:

```HTML
<form action="/upload" method="post" enctype="multipart/form-data">
	<div class="dropzone dropzone--enhanced">
		<div>
			<label for="files">
				Attach a file or drag and drop.
			</label>
			<input type="file" name="files" id="files">
		</div>
	</div>
	<input type="submit" value="Continue">
</form>
```

Notes:

- enctype
- Clicking continue when user is finished

### Javascript

Javascript is primarily used here to listen for `ondragenter`, `ondragleave` and `ondrop` events on the dropzone as well as the `onchange` event on the file input itself.

Whichever method the user chooses, the Javascript will make several AJAX requests, one for each file, allowing the user to see progress of each one in real time and giving users essential feedback along the way.

### Progress

When the files are being uploaded with AJAX we need give users progress, just like the browser normally would. We can do this by using the `progress` event fired by the XMLHttpRequest object.

![Progress](.)

```HTML
<progress></progress>
```

Information about progress element

```JS
(xhr.upload || xhr).addEventListener('progress', function(e) {
    var done = e.position || e.loaded
    var total = e.totalSize || e.total;
    var percentage = Math.round(done/total*100) + '%'
    // update progress bar.
});
```

Then when the request is finished, we simply output the uploaded files, giving users the chance to review them and delete them.

![Review and delete and drag more](.)

Notes:

- Feature detection
- Dragging and dropping with Javascript can only work when using AJAX. That is, you can't populate the file inputs. That means a request occurs for every drop immediately.
- Use Gmail approach. Let users drag and drop files uploaded with AJAX. Then let the user submit the form to save and finish/move onto next step if there is one.
- If users need to drag files across different folders, they'll have to perform this task many times.
- When Javascript is off, or feature detection fails, or if the user chooses not to use Drag and Drop we need to provide an agreeable experience.

One way to give users an agreeable and inclusive experience is by simply exposing the file input all the time, letting users continue to upload again easily. Perhaps in a collapsed state. See sketches.

![Some flow sketches](.)

### The degraded version

When Javascript isn't available or the feature detection doesn't pass, users won't be able to drag and drop or upload via AJAX and onchange. In this case, we simply give users a standard file input with an upload button.

![](.)

When the page reloads, success like normal, error like normal?

### Other considerations

- Onchange is historically problematic but the feature detection caters for this.
- Onchange is also problematic because it goes against accessibility criteria as mentioned in previous chapters.
- Using the label to trigger the input doesn't work

### End note?

An alternative approach means ditching all of this for somethign brand new. Universal Credit, for example, doesn't just ask users to upload multiple documents, but to provide information about ‘multiple’ children too. Let's consider patterns for being able to ‘add another’ now.

## Add another

Patterns are always easier to understand when they are applied to real problems. I don't think there is a one-size fits-all approach for letting users add multiple of something, be it files or plain text.

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
- accept attribute
- capture=camera
## add another js differences

- If you know how many things a user needs to add then show that many fields exactly.
- If you don't know how many things they'll add, but you know the max they are allowed to add, then show the max. Then with JS, show/hide/reveal them.
- If you don't know the max they are allowed to add, then you'll have to offer an add another link. Without js go to a page and back. With js, reveal an extra field OR
- Upload a file, show he file is uploaded, and ask if they want to add another with yes and no. One call to action, guide the user, but long winded for frequent users.

## File `onchange` woes

1. When the user selects the same file or a file with the same name, the event doesn't fire.

Link:
- https://stackoverflow.com/questions/12030686/html-input-file-selection-event-not-firing-upon-selecting-the-same-file
- https://stackoverflow.com/questions/6623310/input-file-onchange-event-not-being-fired-in-chrome
- https://stackoverflow.com/questions/10214947/upload-files-using-input-type-file-field-with-change-event-not-always-firin

Solution:
After the onchange event happens, and you immediatley submit with ajax (or submit with button), then replace the input in Javascript to make it brand new. See links (in full) explaining why you can't do it in other ways.

Thought:
When user changes the file, even if it has same name, it works, but the onchange event won't fire.

2. In some browsers (ie7) the event won't fire until you blur.

Link:
2012 https://stackoverflow.com/questions/2389341/jquery-change-event-to-input-file-on-ie

What can you do: nothing.

Note in this link it says how some version of firefox won't fire the input when clicking a label.

Summary: do it onchange, should be okay especially behind feature detect and automatic ajax like drag and drop. Fall back is two buttons. one to upload, one to continue etc. Think about how nice to make it look with a custom label with js. and then use the label to make sure it works for screen readers too.

## File click() woes

https://stackoverflow.com/questions/210643/in-javascript-can-i-make-a-click-event-fire-programmatically-for-a-file-input

Can dispatch event perhaps.

https://css-tricks.com/examples/DragAndDropFileUploading/

Feature detect: multiple file input???

