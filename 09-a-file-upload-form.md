# A file upload form

Some forms involve users having to upload files: document, images or anything else really. On the one hand, uploading a file is only marginally more complex than inputting text or selecting options. On the other, there are some nuances and opportunites that need to be taken into account.

The problem increases by orders of magnitude as soon as you need to let users upload *multiple* files in one go. And interfaces also need to let users add multiple of anything&mdash;not just files.

The main focus of this chapter is around file uploads so we'll start there. But we'll also look at the design problem in a more abstract sense when we talk about the ‘Add another’ pattern.

## A file input

A file input is similar to most types of input. But instead of typing into it, clicking the control spawns a dialog window in which to choose a file from your computer. Here's how it looks:

![File input](.)

```HTML
<div class="field">
  <label for="documents">
  	<span class="label">Attach document</span>
  </label>
  <input class="field-file" type="file" id="documents" name="documents">
</div>
```

Everything here should be familiar as it's almost identical to most of the other form components we've dealt with so far. The only difference is the input's type attribute is set to `file`.

### A note on aesthetics

Some designers like to restyle the file input because it's quite ugly. As designers (not artists!), we know that *pretty and useless* is far worse than *ugly and useful*, but that doesn't mean beauty and aesthetics aren't important. Where possible we should marry the two together.

Styling file inputs is really tricky because browsers mostly ignore any attempt at doing so in CSS. One approach is to visually hide the input, demarcating it solely via the label. Unlike file inputs, labels are far easier to style.

![Hidden input, styled label](.)

Having hidden the input, and styled the label, Javascript should be used to handle the focus states: when the input is focused, put a focus ring around the label. When the user selects a file for upload, the `onchange` event updates the label text as shown.

![Label text updated](.)

On the face of it, this implementation is not only visually pleasing, but it's also accessible. Keyboard and mouse users can operate it like normal and screen readers will announce the state of the input and associated label&mdash;remember it's only *visually* hidden.

But operating the interface is not the only thing that needs consideration. Unfortunately, this enhancement crumbles under scrutiny.

First, updating the label to reflect the state is confusing because the label should describe the field and remain unchanged regardless of state. In this case, screen reader users will hear ‘some-file.pdf selected’ (or similar) as opposed to ‘Attach CV’.

Second, the interface makes no allowances for a visual hint or error message which is normally positioned inside the label, as set out in ‘A Registration Form’.

Third, file inputs let mouse users drag and drop files. The input itself acts as a ‘drop zone’, which savvy users may prefer. Hiding the input means forgoing this functionality.

Unfortunately, the improvement to aesthetics isn't worth the degradation in functionality and utility.

## A multiple file input

Some tasks involve having to upload multiple files at once. One way to do this is to add a `multiple` attribute onto the file input. It spawns the same file explorer but lets users select multiple files.

![Multiple file input](.)

This inocuous attribute grants a lot of power and seems to solve the multiple file problem in one fell swoop, but it's not perfect.

First, users can only select files within a single directory within the file explorer. If they want to upload files residing across different folders they'll be stuck. Of course they could move all the files into a single folder beforehand but this puts the onus on the user.

Second, some browsers don't recognise the `multiple` attribute enhancement. People who use one of these browsers will get a single file input. This may result in a broken experience.

![Show a design that means they can't upload more than one file](.)

You could solve this by considering the full journey:

![1. User uploads file(s)](.)
![2. Confirmation of uploaded file, can delete it, add another or continue/finish](.)
![3. Selecting another starts back at (1) again](.)

Note that this journey works with and without multiple file support and may be the right solution in many cases. The only potential downside is that the journey could become a little long winded.

## A drag and drop enhancement

As noted earlier, file inputs (both single or multiple) allow users to drag and drop files onto the control.
 There are two potential problems with this:

- It's not immediately obvious that this is even possible&mdash;there's no guidance or affordance to indicate this.
- The hit area of the control is relatively small, which makes it hard to operate, particularly for motor-impaired users.

Creating our own drag and drop enhancement lets us solve both of these problems.

### How it might look

![Design with progress bar](.)

The design&mdash;slightly biased toward mouse users&mdash;presents a large drop zone making it far easier to use, especially for motor-impaired users. Inside the drop zone is some text that makes the behaviour immediately obvious.

Below the text sits a button. Really, it's a label *styled* as a button using the technique I lambasted earlier. To reiterate, this works because the label is a proxy for the input. That is, clicking the label is like clicking the (hidden) file input, which spawns the dialog as normal.

There's also no submit button because we're going to upload the files immediately as soon as they're dropped onto the drop zone. This is because browser's won't let script programmatically update the file input's value (`ondrop`) due to security reasons[^].

Given this constraint, if the user selects a file using the file input (instead of drag and drop), the selected files will be uploaded immediately (`onchange`). This makes both interactions behave similarly.

The user can keep uploading documents using both interfaces, interchangeably, should they choose. Once finished, clicking continue takes users to the next step, whatever that may be. Gmail users, for example, upload as many files as they want using a similar interface and then click send. It's the exact same pattern with a different veneer.

![Gmail compose?](.)

### Enhanced HTML

The Javascript-enhanced HTML is shown below.

```HTML
<form action="/upload" method="post" enctype="multipart/form-data">
	<div class="dropzone dropzone--enhanced">
		<div>
			<label for="files">
				Attach a file or drag and drop.
			</label>
			<input type="file" name="files" id="files" multiple>
		</div>
	</div>
	<div role="status" aria-live="polite"></div>
	<input type="submit" value="Continue">
</form>
```

The `enctype` attribute is relevant for the degraded experience. Without it, files won't be transmitted to the server.

Keyboard users can tab to the visually hidden input  which will pseudo focus the label&mdash;similar to how we handled state for the seat chooser component as set out in “Book a flight”. At the same time screen readers will announce the label as normal.

As the user uploads files, the live region will be updated to inform screen reader users of progress. More on this shortly.

### Drag and drop behaviour

There are three drag and drop specific events the Javascript needs to use in order to implement this functionality: `ondragover`, `ondragleave` and `ondrop`.

The first two events are simply for adding and removing classes so that we can provide feedback to users that they are successfully over the drop zone.

The last event is where the magic happens. Having dropped the files onto the drop zone, we can iterate over `e.dataTransfer.files` to create an AJAX request for each.

Having separate requests, means we can show a progress bar for each individual file.

### Progress, error and success states

Whether files are dropped or selected via the input, we need to handle progress, errors and success states.

We can only upload files with AJAX using XHR2. Fortunately, this API lets us tap into the progress of the request whilst it's in flight.

```HTML
<progress></progress>
```

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

### Feature detection

Stuff here. The feature detection solves bad browsers we think.

When Javascript isn't available or the feature detection doesn't pass, users won't be able to drag and drop or upload via AJAX and onchange. In this case, we simply give users a standard file input with an upload button.

![](.)

When the page refreshes and shows success/error states as per ajax.

### Other considerations

- Onchange is historically problematic but the feature detection caters for this.
- Onchange is also problematic because it goes against accessibility criteria as mentioned in previous chapters.
- Using the label to trigger the input doesn't work
- Reseting (cloning) the file input for onchange.

### End note?

This is not necessarily needed and perfect etc. There is a lot of complexity here. mkae sure u need drag and drop before going against such conventions etc.

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




This approach is not experientally perfect by any means and I'll address this later on. We also need to consider:

- What happens when browsers can't do this or Javascript is off? In other words, how well does this degrade?
- How does the interface deal with progress, success and error states?
- How is the interface operated by and communicated to screen reader users?
