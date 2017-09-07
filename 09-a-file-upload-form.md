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

The design&mdash;slightly biased toward mouse users&mdash;presents a large drop zone making it easier to use, especially for motor-impaired users. Inside the drop zone is some text that makes the behaviour immediately obvious.

Below the text sits a button. Really, it's a label *styled* as a button which is the technique I lambasted earlier (I'll explain the thinking in a moment). To reiterate: this works because the label is a proxy for the input. Clicking the label is like clicking the (hidden) file input, which spawns the dialog as normal.

There's also no submit button because the files are uploaded as soon as they're dropped. This is because browser's won't let you update the file input's value programmatically (`ondrop`) due to security reasons[^]t. And because of this, selecting a file (as opposed to dropping one) also uploads it immediately (`onchange`). This way, both interactions behave similarly and can be used interchangeably seamlessly.

This technical constraint has influenced the direction of the design significantly which is why it's veered away from convention by styling the label as a button and uploading the files immediately `onchange`.

The user can keep uploading documents using both interfaces, interchangeably, should they choose. They can review the uploaded files, and delete ones uploaded in error if they wish.

Once finished, clicking continue takes users to the next step (whatever that is). Gmail users, for example, upload files using a similar interface and clicking send. It's the same pattern with a different veneer.

![Gmail compose?](.)

### The drop zone

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

The `enctype` attribute is only relevant to the degraded experience discussed shortly.

Keyboard users can tab to the visually hidden input  which will pseudo focus the label&mdash;similar to how we handled state for the seat chooser component as set out in “Book a flight”.

There are three events the Javascript uses: `ondragover`, `ondragleave` and `ondrop`. The `ondragover` handler adds a class of `dropzone--dragover` and the `ondragleave` handler removes it. The class is used to provide users feedback so they know they are within the drop zone.

The `ondrop` handler is where the bulk of the functionality happens. The event handler provide and event object (`e.dataTransfer.files`) which can be iterated over in order to create an AJAX request for each file. This way we can provide granular progress which we'll discuss next.

### Providing feedback

Whether files are dropped or selected with the input itself, we need to give users feedback. For each file that's uploading we can use the `<progress>` element.

![Progress](.)

```HTML
<ul>
	<li>
		<span>file.pdf</span>
		<progress max="100" value="80">80% complete</progress>
	</li>
	...
</ul>
```

Listening to the `progress` event on the AJAX request object lets us update the progress bar accordingly. When it's completed, we turn the file.pdf into a link that can be downloaded. We also let users delete the file by providing a delete button.

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

The only thing that's missing is a hidden live region in order to *provide a comparable experience* for screen readers. Here are the 3 types of messages:

- When upload starts ‘3 files are being uploaded.’ is announced.
- When an upload finishes ‘file.pdf has been uploaded.’ is announced.
- When a particular file couldn't be uploaded ‘file.pdf could not be uploaded because it was too big.’ is announced.

### Feature detection

This enhancement uses a lot of advanced Javascript APIs that not all browsers recognise. Before referencing and calling these APIs we have to detect them.

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

### The degraded experience

When Javascript isn't available or the browser fails the feature detection, users won't get the enhanced interface. Instead, they'll see a file input and an upload button.

![Degraded view](.)

Uploading a file this way causes a page refresh with the same feedback component as discussed above.

### The final script

```JS
Put it here
```

### The small print

Even though there is rationale behind hiding the input, using a label and uploading the files `onchange`, that does not mean it's perfectly robust. In fact it even goes against WCAG as mentioned in chapter 6, ‘An inbox’. Here it is again:

> Changing the setting of any user interface component does not automatically cause a change of context.

Here the setting is the chosen file. But this is more than just academic. The `onchange` event is historically problematic, particularly when applied to a file input. In some browsers, if you upload the same file for a second time, the `onchange` event won't fire causing a broken experience[^].

The most robust workaround is to replace the entire file input after the onchange event fires, but this means we need to refocus the input causing it to be announced by screen readers again.

The other problem with `onchange` is that some older browsers, won't fire until blurring the field[^]. Fortunately, our feature detection happens to rule out older browsers, but that's somewhat lucky.

Lastly, some older browsers won't trigger the file input by clicking the label. Fortunately, the detection happens to solve this issue again.

For us here, it seems it's not a problem, but it goes to show that going against standards can often lead to very real problems.

## Add another

This is not necessarily needed and perfect etc. There is a lot of complexity here. mkae sure u need drag and drop before going against such conventions etc.

An alternative approach means ditching all of this for somethign brand new. Universal Credit, for example, doesn't just ask users to upload multiple documents, but to provide information about ‘multiple’ children too. Let's consider patterns for being able to ‘add another’ now.

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

[^onchangesamefile]: https://stackoverflow.com/questions/12030686/html-input-file-selection-event-not-firing-upon-selecting-the-same-file
[^onchangeonblurissue]: https://stackoverflow.com/questions/2389341/jquery-change-event-to-input-file-on-ie
[^labelclickdoesntwork]: https://stackoverflow.com/questions/2389341/jquery-change-event-to-input-file-on-ie

## Todo?

- accept attribute
- capture=camera
- The file input is always wiped due to security

## add another js differences

- If you know how many things a user needs to add then show that many fields exactly.
- If you don't know how many things they'll add, but you know the max they are allowed to add, then show the max. Then with JS, show/hide/reveal them.
- If you don't know the max they are allowed to add, then you'll have to offer an add another link. Without js go to a page and back. With js, reveal an extra field OR
- Upload a file, show he file is uploaded, and ask if they want to add another with yes and no. One call to action, guide the user, but long winded for frequent users.
