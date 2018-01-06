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

When the user is dragging files over the dropzone, they should be given feedback so that they know that the files can be dropped. 

![on drag over](./images/08/dragover.png)

We can achieve this by adding a class onto the drop zone when the `ondragover` event is fired. Similarly, we need to remove the class when the user leave the dropzone (when `ondragleave` is fired):

```JS
Dropzone.prototype.onDragOver = function(e) {
  e.preventDefault();
  this.dropzone.addClass('dropzone-dragover');
};

Dropzone.prototype.onDragLeave = function() {
  this.dropzone.removeClass('dropzone-dragOver');
};
```

*(Note, `e.preventDefault()` is called to allow the file to be dropped onto the drop zone. Without doing this, the browser will try and load the dropped file instead.)*

Now we can change the styles of the drop zone with CSS:

```CSS
.dropzone-dragover {
  /* styles here */
}
```

*(Note: we can't just use `:hover` because the feedback should only given when a user is dragging a file over the drop zone—not just that the cursor happens to be over the drop zone.)*

### Dropping Files

Next we need to handle the dropping of the files, which we can do by listening to the `ondrop` event. Here is the handler:

```JS
Dropzone.prototype.onDrop = function(e) {
  e.preventDefault();
  this.dropzone.removeClass('dropzone-dragover');
  $('.fileList').removeClass('hidden');
  this.uploadFiles(e.originalEvent.dataTransfer.files);
};
```

Notes:

- `e.preventDefault()` is called to allow the file to be dropped onto the drop zone. Without this, the browser will attempt to load the file instead.
- The dragover highlight is removed as the file has now been dropped.
- The file list component is revealed ready to give users feedback as the files are uploaded. More on this shortly.
- The event object (`e`) contains information about the files, which is handed over to the `uploadFiles` method (shown below) 

```JS
Dropzone.prototype.uploadFiles = function(files) {
  for(var i = 0; i < files.length; i++) {
    this.uploadFile(files[i]);
  }
};
```

### Uploading The File

Uploading the file involves two steps: creating the data to be sent and and sending the data:

```JS
Dropzone.prototype.uploadFile = function(file) {
    var formData = new FormData();
    formData.append('documents', file);
    $.ajax({
      data: formData
      url: '/ajax-upload',
      type: 'post',
      processData: false,
      contentType: false,
    });
  };
```

The `FormData` API is designed to construct key/value pairs that represent form fields (and their values), which can then be sent with AJAX, including forms that contain files (like ours does). First, we create a new instance, then we append the file data to it.

For convenience, we're using jQuerys `$.ajax` method. Here's a run down of the properties used:

| Property | Description |
|:---|:---|
| data | Set to the data constructed using `FormData`. |
| type | Set to "post" as we're sending data, not retrieving it as "get" is designed for. |
| url | The url for whichthe server will process the request. |
| processData | Set to `false` which tells jQuery not to convert the data into a querystring. This is important as we're sending files, not text. |
| contentType | Set to `false` which tells jQuery not to override the automatically created headers appropriate for sending files[^boundary]. |

### Feedback

It's all well and good having uploaded the files to the server, but the user is none the wiser as the interface hasn't given them any feedback. There are three types of feedback users need: progress, success and error. Let's look at each in turn now.

#### Progress

Unlike text, files may take a long time to upload. As such, it's important to give users feedback during upload—not just on completion. We can provide feedback using a progress bar.

Each file should be represented separately as there's a separate request for each. This way, some small files will upload quickly, while others load more slowly in parallel.

Here's how it might look:

![Progress](./images/08/progress.png)

```HTML
<ul>
  <li>
    <span class="file">file.pdf</span>
    <progress max="100" value="80">80% complete</progress>
  </li>
  <!-- more list items -->
</ul>
```

The files are contained in a `<ul>`. Each file, represented as an `<li>`, contains the file name (`<span>`) and the progress bar (`<progress>`).

The progress element typically displays as a progress bar to show progress. It has two attributes: max and value. The max attribute describes how much work there is to be done. In our case, it's set to 100 as we're working in percentages. The value attribute specifies how much has been completed, which is updated with JavaScript (shown below).

*(Note: we're also setting the inner text of the progress element. This is what users will see in browsers that don't support the progress element.)*

```JS
$.ajax({
  xhr: function() {
    var xhr = new XMLHttpRequest();
    xhr.upload.addEventListener('progress', function(e) {
      if (e.lengthComputable) {
        var percentComplete = e.loaded / e.total;
        percentComplete = parseInt(percentComplete * 100);
        li.find('progress')
          .prop('value', percentComplete)
          .text(percentComplete + '%');
      }
    }, false);
    return xhr;
  }
});
```

As jQuery (at the time of writing) doesn't support the `onprogress` event we have to create our own instance of `XMLHttpRequest` in order to listen to that event. This is done within the `xhr` property.

The handler first checks to see if the server has correctly sent a `Content-Length` header by seeing if `e.lengthComputable` is true. It is has, then we can determine how much of the file has been uploaded, which is done by dividing `e.loaded` by `e.total`. That value is then converted to a percentage before updating the progress bar.

#### Success

Next, we want to show users when a file has been successfully uploaded. First the file name is converted into a link so that users can download and verify the file if they wish. Second, we inject a success message of “File uploaded” and a Remove button which is useful if the file was uploaded by mistake.

![Success](./images/08/success.png)

```HTML
<li>
	<a href="/path/to/file.pdf">file.pdf</a>
	<span class="fileList-success">
    <svg width="1.5em" height="1.5em">
      <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#tick"></use>
    </svg>
    File uploaded
  </span>
	<input type="submit" name="remove1" value="Remove">
</li>
```

```JS
$.ajax({
  success: $.proxy(function(response){
    if(response.file) {
      li.html(this.getSuccessHtml(response.file));
    }
  }, this)
});
```

We're using the success callback which receives the response from the server as an object. The response contains a `file` property which contains the path and name of the file. This is used to create the HTML that is injected into the list item.

*(Note: the demo uses Multer and Express to process the request and generate the appropriate response as designed above.)*

#### Error

If the uploaded file is too big, or in wrong format, we'll need to show users an error message which is going to be similar to the success message. Except instead of showing a green success message with a tick, we'll show a red message with the warning symbol. A Dismiss button is also injected which lets users dismiss the file if they want before attempting to upload another.

![An error](./images/08/error.png)

```HTML
<li>
	<a href="/path/to/file.pdf">file.pdf</a>
	<span class="error">
    <svg width="1.5em" height="1.5em"><use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#warning-icon"></use></svg>
    File.pdf is too big.
  </span>
	<button type="button">Dismiss</button>
</li>
```

```JS
$.ajax({
  success: $.proxy(function(response){
    if(response.error) {
      li.html(this.getErrorHtml(response.error));
    } else if(response.file) {
      li.html(this.getSuccessHtml(response.file));
    }
  }, this)
});
```

When there's an error, the response object will contain an `error` property which contains information about the error so that the HTML can be constructed accordingly.

#### Screen Reader Feedback

While the feedback is useful for sighted users, screen reader users won't hear any feedback. To provide a comparable experience (principle 1) we'll need to add a hidden live region.

```HTML
<div class="vh" role="status" aria-live="polite">
  Uploading files. Please wait.
</div>
```

Notes:

- vh
- attrs

The live region will be updated at various points during the upload process:

| When | Description |
|:---|:---|
| Upload starts | Uploading files. Please wait. |
| Upload completes | File.pdf has been uploaded. |
| Upload fails | File.pdf is too big. The size must be less than 2MB. |

### Feature Detection And Initialisation

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
[^boundary]: yada