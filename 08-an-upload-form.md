# An Upload Form

The web is more than just text. Whether it's sending a CV to a recruiter by email, or adding photos to an Ebay advert, we need to let users upload files. Forms have this capability baked in.

On one hand, uploading a file is only marginally more complex than say, inputting text or clicking a checkbox. On the other hand, there are number of unique design challenges and opportunities that arise, especially when there's a need to upload multiple files at the same time.

As usual, we'll start by looking at what browsers give us for free. After that, we'll look at designing various enhancements and issues that can arise along the way.

## A File Picker

A file picker (`<input type="file">`) is another type of form control. When clicked, it will spawn a dialog that let's users browse their files on their computer or device. Once a file is selected, the dialog closes and the picker updates to reflect the file has been chosen.

![File picker](./images/08/single-file-picker.png)

If all users need to do is upload a single file, then you can add a file picker to your form and you're pretty much done:

```HTML
<form enctype="multipart/form-data">
  <div class="field">
    <label for="documents">
      <span class="label">Attach document</span>
    </label>
    <input class="field-file" type="file" id="documents" name="documents">
  </div>
  <input type="submit" value="Upload" name="upload">
</form>
```

Notes:

- The form has a `enctype="multipart/form-data"` attribute, which ensures the file is transmitted to the server for uploading
- The file picker uses the same field constructs as first described in chapter 1, “A Registration Form”. Please refer to that chapter if you need to give users a hint or error message.

### Restyling The File Picker Is Dangerous Territory

Some designers like to restyle the file picker to:

- achieve consistency between different browsers and operating systems
- match the company's look and feel
- be able to configure the control's text

Whether you agree with all of these reasons or not, it must be said that *pretty and useless* is considerably worse than *ugly and useful*. But this doesn't mean aesthetics aren't important: where possible we should marry form and function together.

However, it's just as important to make sure that any technique's we employ to achieve these goals doesn't cause any adverse usability issues. That would be a bit like modern medicine. For example, taking a pill often fixes the symptom of a problem only to cause several adverse side effects that require additional pills to fix.

When it comes to file pickers, styling them has always been tricky because browsers ignore any attempt at doing so with CSS. This means we have to resort to a bit of hack, which is never normally a good idea. But, let's walk through how it could work, what can be achieved and the pitfalls in doing so.

#### Hide the input

The most robust way of styling the the file picker is to actually just hide it with a visually hidden:

```HTML
<div class="field">
  <label for="documents">
    <span class="label">Attach document</span>
  </label>
  <input class="visually-hidden" type="file" id="documents" name="documents">
</div>
```

*(Note: The CSS for the vh (visually hidden) class is set out in “A Checkout Flow”.)*

Now that it's hidden, we can style the control's label, which fortunately, is easy to style. As described in “A Registration Form”, this works because a control's label acts as a proxy to the control itself. In this case, this means clicking the label is like clicking the input.

#### Style The Label

Now the input is hidden, we need to style the label so that it looks interactive. To make it look interactive, we need to style it and change the text like this:

![Label as proxy](./images/08/label-as-button.png)

#### Focus States

Now that the label looks clickable and is clickable we need to think about keyboard interaction. 

As the input is visually hidden, when the user Tabs to it, they won't get any feedback that it's actually in focus. To do this, JavaScript should be used so that when the input is focused a class of `focused` is added to the label so that we can style it to look focused:

```CSS
.focused {
  /* focus styles */
}
```

#### Reflecting The Chosen File

When the user selects a file from the dialog, it's the input that will change state (as shown earlier). We'll need to reflect the chosen file within the text of the label.

![Label text updated](./images/08/chosen-file.png)

To do this, well need to listen to the input's `change` event like this:

```JS
$('[type=file]').on('change', function(e) {
  // change label
});
```

#### Pitfalls

On the face of it, this implementation is visually pleasing and still accessible. Keyboard, mouse and touch users can operate it normally and screen readers will announce the value of the input. But these do not cover the exhaustive list of considerations needed to provide a fully inclusive experience.

1. Updating the label to reflect the input's value is confusing because the label should describe the input and remain unchanged. In this case, screen reader users will hear “cv.doc” as opposed to “Attach document”.
2. The interface doesn't fit with the established convention for providing hint and error text, as set out in “A Registration Form”. Not only would we need to think of another way to provide this information, but it would make for an inconsistent and unfamiliar user experience.
3. File inputs are actually drop zones which means they let users drag and drop files (instead of going through the dialog). Hiding the input means forgoing this behaviour which some users may prefer.
4. There's not much room inside the button for a large file name. Remember, inclusive design ensures the interface works under varying lengths of content.

Considering the pitfalls, the improvement to aesthetics doesn't seem to justify the downgrade in usability and utility.

## A Multiple File Picker

Very few tasks on the web, require a user to upload just a single file at a time. Take the two examples from the introduction to this chapter. Both attaching files to an email and uploading photos to an Ebay advert involve uploading multiple files in one go.

The easiest but most problematic way to solve this would be to add the `multiple` boolean attribute to the file input:

```HTML
<input type="file" multiple>
```

This gives users the same method of selecting files as described with the single file input. Except for one difference: the user can select multiple files from the dialog:

![Selecting multiple files](./images/08/multiple-dialog.png)

And that's it, except there are a number of problems:

First, users can only select files within a single folder. If they need to upload files within different folders they can't. Of course, users could move all the files into a single folder beforehand but this puts the onus on the user.

Second, not all browsers support this behaviour. And in these browsers, it will degrade into a single file picker which could result in a broken experience.

For example, take the following form. It asks users to submit receipts. When the `multiple` attribute is supported, users can upload all the relevant receipts and submit them. But without support, users can only upload a single receipt which leaves users with a problem.

One way to solve this problem, is to give users a way to add additional files as part of a flow:

![Degraded solution](./images/08/multiple-degrade-solution.png)

Not only does this design let users upload multiple files in unsupported browsers, but it lets the user review their submission too. Another way to help users upload files is by using a persistent upload form, which we'll discuss next.

## A Persistent Upload Form

Most forms are ephemeral—that is, they only stay on-screen until they've been submitted—at which point, the user is taken to another form or end of the task. For example, when you register, you enter your details, the form disappears and you see a confirmation. Or the user enters their delivery and payment detail, and they get confirmation of their order.

However, the task of uploading multiple files means it's useful to give users a form that persists until the user explicitly decides to move on. We might call this a persistent form.

### How It Might Look

![Illustrate degraded version](./images/08/degraded.png)

The user can choose and upload a file repeatedly until they've uploaded all the desired files. At which point, they can click the continue button.

### The Mark-up

```HTML
<form enctype="multipart/form-data">
  <div class="field">
    <label for="documents">
      <span class="label">Attach file</span>
    </label>
    <input class="field-file" type="file" id="documents" name="documents" multiple>
  </div>
  <input type="submit" value="Upload" name="upload">
</form>
```

Note the file input has the `multiple` attribute. In this case this is a robust enhancement: if the browser has support, the user can choose multiple files (meaning fewer page refreshes). If the user is using an unsupported browser, they can still upload a single file as many times as they need to. 

This also solves the aformentioned problem about not being able to select files across different folders.

## A Drag And Drop Enhancement

As noted earlier, the native file input acts as a drop zone to let users drag and drop files. However, there are two problems:

1. it's not immediately obvious that dragging and dropping is possible—there are no signifiers that makes this behaviour perceivable.
2. the drop zone has a small hit area, which makes it hard to drop files onto, especially if the user has motor-impairments.

To solve these issues, we're going to take our persistent upload form, and progressively enhance it with better drag and drop functionality.

### How It Might Look

![Dropzone](./images/08/drop-zone.png)

The large drop zone is more ergonomic, especially for people with motor-impairments. It's conventionally styled with a dashed border. However, if your users aren't aware this convention, you can add some instructional text.

Inside the drop zone, sits a button. When clicked, it triggers the choose file dialog. The button is actually a label *styled* as a button using the ill-advised technique from earlier. But I haven't gone mad, there's good reason for this.

### Why We're Styling The Label As A Button

The dropzone has two methods of interaction: dropping files onto the drop zone and clicking the button.

Browsers don't let you programmatically update a file input's value due to security reasons[^1]. Because of this, we can't for example update the file input's value, when the user drops files onto the drop zone. Because of this, files will be uploaded immediately with AJAX (which we'll cover shortly).

Remember, the dropzone actually has two methods of interaciton: dropping files onto the drop zone and clicking the button. For consistency reasons we want both approaches to upload files immediately (`onchange`). This way, users don't ever have to think about when (or when not) to submit the form—that interaction is taken out of the equation.

### The Enhanced Mark-up

Here's the Javascript-enhanced mark-up:

```HTML
<form class="dropzone">
	<div class="field">
		<label for="files">Upload file</label>
		<input type="file" name="files" id="files" multiple>
	</div>
</form>
```

Notes:

- The button's been removed because the files will be uploaded with AJAX `onchange`.
- The “dropzone” is exists as a way to target this particular form for CSS and JavaScript enhancement. 

### Dragover And Dragleave Events

Dragging and dropping files onto the dropzone involves three events: `ondragover`, `ondragleave` and `ondrop`.

The `ondragover` event is used to add a class of `dragover`. We can use this class to change the styles of the dropzone so that users know they are within the dropzone. Similarly, the class should be removed when the user leaves the dropzone by using the `ondragleave` event.

![on drag over](./images/08/drag-over.png)

```JS
Dropzone.prototype.onDragOver = function(e) {
  e.preventDefault();
  this.dropzone.addClass('dropzone--dragOver');
};

Dropzone.prototype.onDragLeave = function() {
  this.dropzone.removeClass('dropzone--dragOver');
};
```

Notes:

- We can't just use the `:hover` psuedo class, because we only want to give users feedback when they are dragging a file onto the drop zone. The `:hover` class would apply even if the user wasn't dragging files onto the drop zone. 
- Prevent default?
- bem

### Dropping Files

The `ondrop` handler is where the magic happens. The event handler receives an event object (`e.dataTransfer.files`) that holds data about the files. For each file dropped an AJAX request is made.

```JS
Dropzone.prototype.onDrop = function(e) {
	e.preventDefault();
	this.removeHighlight();
	this.upload(e.originalEvent.dataTransfer.files);
};
```

By default, dragging and dropping a file into the viewport will load the file in the browser which we prevent from happening on the firt line. Next, the highlight is removed. And finally, the `upload()` method is called passing in the dropped files. 

```JS
Dropzone.prototype.upload = function(files) {
    for(var i = 0; i < files.length; i++) {
      var formData = new FormData();
      formData.append('documents', files[i]);
      this.makeRequest(formData);
    }
    $('.fileList').removeClass('hidden');
  };
```

For each file dropped we create the data using the `FormData` object, then pass that data to `makeRequest`. Finally, the fileList component is revealed so that we can inject feedback.

### Feedback

Whether files are dropped or selected with the file picker, we need to give users feedback.

```JS
Dropzone.prototype.makeRequest = function(formData) {
  var li = $('<li>'+ formData.get('documents').name +'<br><progress value="0" max="100">0%</progress></li>');
  $('.fileList ul').append(li);
	$.ajax({
    url: '/ajax-upload',
    type: 'post',
    data: formData,
    xhr: function() {
      var xhr = new XMLHttpRequest();
      xhr.upload.addEventListener('progress', function(evt) {
        if (evt.lengthComputable) {
          // calculate the percentage of upload completed
          var percentComplete = evt.loaded / evt.total;
          percentComplete = parseInt(percentComplete * 100);

          li.find('progress').text(percentComplete + '%');
          li.find('progress').val(percentComplete);
        }
      }, false);
      return xhr;
    }
  });
};
```

This function injects a list item into the file list panel and fires the request. It listens to the `progress` event on the `XMLHttpRequest` object, so that feedback can be given in real time. This is particularly useful if users are uploading large files or using a slow network (or both).

Each file is represented as a list item. Progress is indicated by the `<progress>` element.

![Progress](./images/08/progress.png)

```HTML
<ul>
  <li>
    <span class="file">file.pdf</span>
    <progress max="100" value="80">80% complete</progress>
  </li>
  ...
</ul>
```

The text inside the element is for browsers that lack support for the progress element. They'll just see the text.

The progress bar is updated in response to the AJAX request that has an `onprogress` event.

When the file is finished uploading, the `<span>` is converted into a link so that it can be downloaded. Additionallity, a submit button is added, letting users delete the file if they uploaded it by mistake, for example.

![Success](./images/08/success.png)

```HTML
<ul>
	<li>
		<a href="/path/to/file.pdf">file.pdf</a>
		<progress max="100" value="100">100% complete</progress>
		<input type="submit" name="remove1" value="Remove">
	</li>
	...
</ul>
```

If there's an error, a message is shown in place of the progress bar, letting users dismiss that file to try again by clicking the button.

![An error](./images/08/error.png)

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

The only thing missing is a hidden live region in order to *provide a comparable experience* for screen readers. There are three scenarios that need to be announced:

1. The user starts uploading: “3 files are being uploaded”
2. The upload finishes successfully: “file.pdf has been uploaded”
3. The file couldn't be uploaded: “file.pdf couldn't be uploaded because it was too big”

These messages are only needed for screen reader users, and so they should be placed inside a hidden live region:

```HTML
<div class="vh" role="status" aria-live="polite">
	3 files are being uploaded
</div>
```

### Feature Detection

The drag and drop script uses several Javascript APIs that not all browsers recognise. Before referencing them, they need to be detected to ensure users don't get a broken experience.

```JS
(function() {
	var dragAndDropSupported = (function() {
		var div = document.createElement('div');
		return ('draggable' in div) || ('ondragstart' in div && 'ondrop' in div);
	}());

	var formDataSupported = (typeof Formdata == 'function');

	var fileReaderSupported = (typeof FileReader == 'function');

	if(dragAndDropSupported && formDataSupported && fileReaderSupported) {
		// Define Dropzone code here
	}
}());
```

The calling application detects `Dropzone` before creating an instance. When it's undefined, that indicates the browser either lacks support or there was a network failure. Either way, users get the degraded, but not broken, experience.

```JS
if(typeof Dropzone !== 'undefined') {
  new Dropzone();
}
```

### Considering Older Browsers

Uploading files `onchange` might be unexpected and confusing for users because conventionally speaking, they'd expect to submit the form as a separate action.

However, this isn't just a conventional problem—it's not cross-browser either. Some older browsers suffer with this unconventional enhancement. Here's three examples:

1. Choosing the same file (or a file with the same name) for a second time, won't fire the `onchange` event[^2], which creates a broken interface. The solution involves replacing the entire file input after the `onchange` event fires with a clone of itself. As it would need to be refocused, screen readers will announce it for a second time which is mildly annoying.
2. The `onchange` event won't fire until the field is blurred[^3]. Newer browsers offer the `oninput` event which solves this problem. This is because it fires the event as soon as the value changes (without blurring the field).
3. Clicking the label doesn't trigger the input[^4].

With that said, you may not have any users using these older browsers. And, this enhancement rules the offending browsers out due to the feature detection. That is, that the browsers that suffer from these problems don't support drag and drop.

It's worth noting that these problems have only arisen because of the assumed need to let users drag and drop. If users don't need it, then there's no need to veer away from convention anyway.

## A Note About The `accept` and `capture` Attributes

The file input offers two interesting attributes that affect the experience of uploading files: `accept` and `capture`.

The `accept` attribute takes a hint or mime type. When supported, the browser/device may offer users an optimised interface in which to choose files. 

```HTML
<input type="file" accept="image/*">
```

In Chrome and Safari on iOS and Android, for example, it will give the user a choice of which app to use to capture the image, including the option of taking a photo with the camera or choosing an existing image file.

![iOS and Android accept dialogs](./images/08/accept-attribute.png)

*(Note: On desktop it will prompt the user to upload an image file from the file system disabling files that aren't images, for example. The problem with this is that users won't know why the files are disabled as there's no feedback.)*

The `capture` attribute, when supported, indicates the preference of getting an image from the camera:

```HTML
<input type="file" accept="image/*" capture>
<input type="file" accept="image/*" capture="user">
<input type="file" accept="image/*" capture="environment">
```

Adding the capture attribute without a value let's the browser decide which camera to use (if there's one available), while the "user" and "environment" values tell the browser to prefer the front and rear cameras respectively.

The capture attribute works on Android and iOS, but is ignored on desktop. Beware that on Android this means the user will no longer have the option of choosing an existing picture. The system camera app will be started directly instead, which is probably undesirable.

## Summary

In this chapter, we looked at the intricacies of uploading files - not just one at a time, in bulk. 

We looked at ways to mitigate the file input's ugliness, but soon realised that making it look better is at the cost of usability.

Finally, we looked at ways to enhance the native file input's drag and drop behaviour by creating a large custom drop zone. To do that we used various modern Javascript APIs that meant breaking convention due to security constraints.

### Things To Avoid

- Visually hiding the file input for aesthetic reasons.
- Using multiple file inputs without considering the degraded experience and end-to-end flow.
- Creating a drag and drop solution without first ensuring there's a need for it.

## Demos

TBD

## Footnotes

[^1]: https://css-tricks.com/drag-and-drop-file-uploading/#article-header-id-4
[^2]: https://stackoverflow.com/questions/12030686/html-input-file-selection-event-not-firing-upon-selecting-the-same-file
[^3]: https://stackoverflow.com/questions/2389341/jquery-change-event-to-input-file-on-ie
[^4]: https://stackoverflow.com/questions/2389341/jquery-change-event-to-input-file-on-ie