# An Upload Form

Some forms involve users having to upload files: documents, images or anything else really. On the one hand, uploading a file is only marginally more complex than inputting text or selecting options. On the other, there are some nuances and opportunites that need to be taken into account.

The problem increases by orders of magnitude as soon as you need to let users upload *multiple* files in one go.

## A File Picker

A file picker (`input type="file"`) is similar to most types of input. But instead of typing into it, the control spawns a dialog in which users can select a file from their computer or device.

![File input](.)

```HTML
<div class="field">
  <label for="documents">
  	<span class="label">Attach document</span>
  </label>
  <input class="field-file" type="file" id="documents" name="documents">
</div>
```

There's not much to see here. The component uses the same structure as many other form components. The only difference is the input's `type` attribute.

If all users need to to do is upload a single file, then you can add this field to your form and you're done.

### Aesthetics

Some designers like to restyle the file picker because it looks quite ugly. We know that *pretty and useless* is far worse than *ugly and useful*, but that doesn't mean beauty and aesthetics aren't important. Where possible we should marry the two together.

Styling file inputs is tricky because most browsers ignore any attempt at doing so with CSS. One approach is to visually hide the input, demarcating it solely via its label. Unlike file inputs, labels are far easier to style.

![Hidden input, styled label](.)

Having hidden the input, and styled the label, Javascript should be used to handle focus states: when the input is focused, put a pseudo focus outline around the label. When the user selects a file for upload, the `onchange` event updates the label text as shown.

![Label text updated](.)

On the face of it, this implementation is visually pleasing and it's still accessible. Keyboard and mouse users can operate it like normal and screen readers will announce the state of the input and associated label.

But operating the interface is not the only thing that needs consideration. This enhancement crumbles under further scrutiny.

First, updating the label to reflect the state is confusing because the label should describe the field and remain unchanged regardless of state. In this case, screen reader users will hear ‘some-file.pdf selected’ (or similar) as opposed to “Attach document”.

Second, the interface makes no allowances for a visual hint or error message which is should be positioned inside the label, as explained and set out in “A Registration Form”.

Third, file inputs let mouse users drag and drop files. The input itself acts as a “drop zone”, which savvy users may prefer. Hiding the input means forgoing this functionality.

Any improvement to aesthetics just isn't worth the degradation in functionality and utility.

## Multiple Files

Some tasks involve users needed to upload multiple files at once. One way to do this is to add the `multiple` attribute onto the input. Now, when the user activates the file picker dialog, the user can select multiple files.

![Multiple file input](.)

This inocuous attribute grants a lot of power and seems to solve the multiple file problem in one fell swoop, but it's not perfect.

First, users can only select files within a single directory within the dialog. If they need to upload files withing different folders they can't. Of course, users could move all the files into a single folder beforehand but this puts the onus on the user.

Second, some browsers don't recognise the `multiple` attribute. In this case it will degrade into a standard file picker, which could result in a broken experience.

![Show a design that means they can't upload more than one file](.)

You could solve this by considering the full journey:

- ![1. User uploads file(s)](.)
- ![2. Confirmation of uploaded file, can delete it, add another or continue/finish](.)
- ![3. Selecting another starts back at (1) again](.)

Note that this journey works with and without multiple file support and may be the right solution in many cases. The only downside is the journey could become a little long winded, which isn't useful if users need to use it a lot.

## A Drag And Drop Enhancement

As noted earlier, file pickers let users drag and drop files onto the control. The problem is twofold: 

1. It's not immediately obvious to users that they can do this.
2. The hit area is relatively small, making it hard to use, especially for users with motor impairments.

Implementating our own solution lets us solve both issues.

### How It Might Look

![Design with progress bar](.)

The design&mdash;slightly biased toward mouse users&mdash;presents a large ‘drop zone’ making it easier to use, especially for motor-impaired users. Inside the drop zone is some instructional text that makes the behaviour immediately obvious.

Below the text sits a button. Really, it's a label *styled* as a button which is the technique I lambasted earlier. To reiterate: this works because the label is a proxy for the input. Clicking the label is like clicking the (hidden) file input.

There's no submit button because the files are uploaded as soon as they're dropped. This is because browser's won't let you update the file input's value programmatically (`ondrop`) due to security reasons[^1]t. And because of this, selecting a file (as opposed to dropping one) also uploads it immediately (`onchange`). This way, both interactions behave similarly.

This technical constraint is the reason we've veered away from convention, which is the first time we've done that in the book and it is not without issue. I'll address these issues later.

The user can keep uploading documents using both methods (interchangeably), should they choose. After they've uploaded the files successfully, they can review and delete files uploaded in error if they need to.

When they're done, clicking continue takes users to the next step (whatever that is). Gmail users, for example, upload files using a similar interface and clicking send. That's the same pattern with a different veneer.

![Gmail compose?](.)

### The Drop Zone

Here's the Javascript-enhanced mark-up:

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
</form>
```

The `enctype` attribute is necessary so that the files are transmitted to the server for processing. This is only relevant to the degraded experience which is discussed later. Remember the enhanced experience uses AJAX.

Keyboard users can tab to the visually hidden input which will pseudo focus the label&mdash;similar to how we handled focus states for the seat chooser component set out in “Book a flight”.

To create the drag and drop behaviour there are three javascript events: `ondragover`, `ondragleave` and `ondrop`.

The `ondragover` handler adds a class of `dropzone--dragover` and the `ondragleave` handler removes it. The class is used to give feedback so users know they are within the drop zone.

![on drag over](.)

The `ondrop` handler is where the magic happens. The event handler receives an event object (`e.dataTransfer.files`) that holds data about the files. These are then iterated over in order to upload them via AJAX.

```JS
Some code here
```

### Feedback

Whether files are dropped or selected with the file picker, we need to give users feedback. Each file is represented as an item in a list. Progress is demarcated by the `<progress>` element.

![Progress](.)

```HTML
<ul>
  <li>
    <span class="file">file.pdf</span>
    <progress max="100" value="80">80% complete</progress>
  </li>
  ...
</ul>
```

The text inside the element is for browsers that lack support for it, meaning they'll just see the text.

The progress bar is updated in response to the AJAX request that has an `onprogress` event.

When the file is completely uploaded, the `<span>` is converted into a link so that it can be downloaded. Additionallity, a submit button is added, letting users delete the uploaded file if they uploaded it by mistake.

![Success](.)

```HTML
<ul>
	<li>
		<a href="/path/to/file.pdf">file.pdf</a>
		<progress max="100" value="100">100% complete</progress>
		<input type="submit" name="delete1" value="Delete">
	</li>
	...
</ul>
```

If there's an error, a message is shown in place of the progress bar, letting users dismiss that file to try again by clicking the `<button>`.

![An error](.)

```HTML
<ul>
	<li>
		<a href="/path/to/file.pdf">file.pdf</a>
		<span class="error">File.pdf is too big.</span>
		<button type="button">Dismiss message</button>
	</li>
	...
</ul>
```

The only thing missing is a hidden live region in order to *provide a comparable experience* for screen readers. Here are the 3 types of messages:

- Upload starts: ‘3 files are being uploaded.’
- Upload ends: ‘file.pdf has been uploaded.’
- Upload error: ‘file.pdf could not be uploaded because it was too big.’

### Feature Detection

This enhancement uses several Javascript APIs that not all browsers recognise. Before referencing them, we should detect them first to ensure users don't get a broken experience.

```JS
(function() {
	var dragAndDropSupported = (function() {
		var div = document.createElement('div');
		return ('draggable' in div) || ('ondragstart' in div && 'ondrop' in div);
	}());

	var formDataSupported = typeof Formdata == 'function';

	var fileReaderSupported = typeof FileReader == 'function';

	if(dragAndDropSupported && formDataSupported && fileReaderSupported) {
		function Dropzone() {
			// ...
		}
		// ...
	}
}());
```

The calling application simply detects `Dropzone` before creating an instant.

```JS
if(typeof Dropzone !== 'undefined') {
  new Dropzone();
}
```

### The Degraded Experience

When Javascript isn't available or the browser fails the feature detection, users won't get the enhanced interface. Instead, they'll see a file picker and an upload button. In the absence of script or capability, uploading a file gives users the same feedback view, just via a page refresh.

![Degraded view](.)

### The Small Print

Granted, there's rationale behind moving away from convention, but that doesn't mean it's perfectly
robust. In fact, doing this goes against what the standards say which was discussed in chapter 6, ‘An inbox’. Here it is again:

> Changing the setting of any user interface component does not automatically cause a change of context.

This is more than just an academic endeavour. The `onchange` event is historically problematic, particularly when it's applied to a file input. For example, in some browsers, if you upload the same file for a second time, the `onchange` event won't fire[^2]. This creates a broken interface.

The solution requires the entire file input to be replaced after the `onchange` event fires. This means we need to set focus to the newly created file input , which causes screen readers to announce it for a second time.

The other problem is that some older browsers, won't fire the `onchange` event until blurring the field[^3]. Fortunately, our feature detection happens to rule out those browsers which is fortunate for us in this case, but still worth baring in mind.

Lastly, some older browsers won't trigger the file input by clicking the label[^4]. Fortunately, the feature detection happens to rule out these browsers too because the same browsers don't support the new APIs.

Anything like this needs a healthy amount of diverse testing to ensure what is enhanced for some, doesn't break for others. As you can see, going against standards can lead to very real problems.

It's also worth baring in mind that users may not want, or benefit, from drag and drop. Before embarking on your own drag and drop solution, make sure there is a user need.

## Summary

In this chapter, we looked at the intricacies of uploading files in bulk and one at a time. The file input is necessary but ugly and so looked at ways of making it aesthetically pleasing and functional at the same time.

We then looked at the shortcomings of the native input's drag and drop behaviour and because of this decided to roll our own custom solution using the latest Javascript APIs. In doing so we veered away from convention partly due to browser constraints associated with security.

### Things to avoid

- Visually hiding the file input for solely for aesthetic reasons.
- Using multiple file inputs without considering the degraded experience and end-to-end flow.
- Creating your own drag and drop solution without first ensuring there's a demand for it.

## Footnotes

[^1]: https://css-tricks.com/drag-and-drop-file-uploading/#article-header-id-4
[^2]: https://stackoverflow.com/questions/12030686/html-input-file-selection-event-not-firing-upon-selecting-the-same-file
[^3]: https://stackoverflow.com/questions/2389341/jquery-change-event-to-input-file-on-ie
[^4]: https://stackoverflow.com/questions/2389341/jquery-change-event-to-input-file-on-ie

## Todo

- accept attribute
- capture=camera
- The file input is always wiped due to security

- https://govuk-design-system-prototypes.cloudapps.digital/design-patterns/patterns/components/file-upload/