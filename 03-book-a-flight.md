# Book a Flight

In this chapter, we'll design a flight booking service. At first this may seem a bit niche, especially when compared to *A Registration Form* and *A Checkout Flow*. However, we're going to explore several complex problems that in the end, will result in reusable patterns. Patterns that are very much transferable to other problem domains such as booking a cinema ticket or even a hotel room.

Booking a flight consists of many discrete steps. The first few steps simply collect user's preferences: where to fly, when to fly and who's flying. Once we have that we can give users a choice of available flights followed by choosing where to sit. Finally, users will have to make payment, but we covered payment patterns in the previous chapter.

## 1. Where to fly

First, users have to choose an origin and destination. That is, places to fly from and to. Without this information, the service can't offer any flights. What's the best way of asking users for this information?

As designers, we should try and use the features that are native to the browser. That is because, generally speaking, they are familiar (due to convention) and fully accessible out of the box. They also require far less work to implement.

You'd be forgiven for thinking you were spoiled for choice when it comes to form controls: select boxes, radio buttons, text boxes and more recently, datalists. The choice is yours, except it isn't. Not all of these options are suitable. Let's look at some of the pros and cons for each.

### Select Box

Select boxes, also known as drop-down menus, hide options behind a menu. Clicking the select box reveals the options. Once one is selected, the menu collapses back to its original state. Select boxes are often used because of their space-saving qualities. What's particularly interesting though, is why we need to save space in the first place. 

Often an interface is crammed with features, usually to please stakeholders, not users. It's understandable then that learning ways to hide discrete pieces of an interface has become part of a designer's skillset. But design is about so much more than saving space. After all, if an interface really is crammed, then our first job as designers is to declutter it.

Select boxes are hard-to-use. Usability expert Luke Wobrelski even goes as far to say that they should be the “UI of Last Resort”[^1] and suggests some better alternatives, some of which we'll discuss later on in this chapter. 

Besides hiding options behind an unnecessary extra click, users generally don't understand how they work. Some users try to type into them, some confuse focused options with selected ones. And to top it off users can't pinch-zoom the options on mobile.

### Radio Buttons

Radio buttons, unlike select boxes, are generally well-understood and easy-to-use, not least because they don't hide options. They are fully exposed making them easy to compare, scan and select. They're also malleable. That is, they let us put whatever content in whatever format we want, inside the label (more on that shortly).

The problem with radio buttons is that they're less suitable when there are many of them. As an airline could fly to hundreds of destinations, the page would be long and unwieldy, causing users to scroll (and keyboard users to tab) a lot.

Don't get me wrong, users are more than happy to scroll, and we shouldn't use this as a crutch for changing course. But if we can naturally eliminate the need to scroll without introducing new problems we should.

### Search Input

A search box (`input type="search"`) is a standard text box with some added extras. You can clear the contents of the field, by tapping *X* or pressing <kbd>Escape</kbd>. With a text box (`input type="text"`) you have to select the text and press <kbd>delete</kbd> which takes a little longer.

![Image here](.)

Using a search box is useful when searching a large amount of dynamic data, such as searching Amazon's[^2] product catalog. Airlines, however, fly to a finite set of destinations known in advance of the user searching. Letting users search unassisted like this could easily end up with a ‘no results’ page due to typos or a data mismatch.

### Autocomplete

Users need a control that lets them filter a long list of destinations—one that combines the flexibility of a text input with the assurance of a select box. This type of control has many names: *type ahead*, *predictive search*, *combo box*, but we'll refer to it as *autocomplete*.

Autocomplete works by suggesting options (destinations in this case) as the user types. As suggestions appear, users can select one quickly, automatically completing the field—hence the name. This saves users having to scroll (unless they want to) while being able to forgive small typos at the same time. 

HTML5's `datalist` element combines with a text box to create this exact behaviour. Unfortunately, it's particularly buggy[^3]. If your project is locked down to a browser that doesn't contain bugs, then you could use it. But we want to design an experience that works for as many people as possible, no matter their browser and device choices. 

Instead, we'll build a custom autocomplete component from scratch. A word of warning though: we're going to break new ground; designing a robust and fully inclusive autocomplete control is challenging work.

### Building An Autocomplete Component

Accessibility expert Steve Faulkner has what he calls a *punch list*[^4] which is a list of rules anyone should follow to make sure that any custom Javascript component is designed and built to a good standard. The rules state that a component should:

1. work without Javascript
2. be focusable with the keyboard
3. be operable with the keyboard
4. work with assistive devices

To satisfy the first rule we need to choose a native form control to fall back to. There are too many options to use radio buttons, and a search box requires a round-trip to the server and may lead to no results. That leaves us with a select box.

![The core experience](.)

```HTML
<div class="field">
	<label for="destination">
		<span class="field-label">Destination</span>
	</label>
	<select name="destination" id="destination">
		<option value="">Select</option>
		<option value="france">France</option>
		<option value="germany">Germany</option>
		<option value="spain">Spain</option>
	</select>
</div>
```

We'll cover off the other rules as we design the component itself.

First, we need to hide (not remove!) the select box. It shouldn't be removed because it's the select box value that is sent to the server on submission. To hide the select box we need to add:

- a class to make it visually hidden with CSS
- `aria-hidden="true"` so it's not perceivable by screen readers
- `tabindex="-1"` so that it is not focusable by keyboard

```HTML
<select aria-hidden="true" tabindex="-1" class="visuallyhidden">
```

```CSS
.visuallyhidden {
	/*code*/
}
```

Then we need to inject the text box that users will interact with. To make sure the label still works, we transfer the `id` to the text box.

```HTML
<input
	type="text"
	id="destination"
	autocomplete="off"
	role="combobox"
	aria-autocomplete="list"
	aria-expanded="true"
	class="autocomplete-textBox"
>
```

Notes:

- The `name` attribute is not included, because the `select`s value is sent to the server.
- The `role="combobox"` attribute ensures this from control is announced as a combo box instead of a text box. A combo box, according to MDN, is “an edit control with an associated list box that provides a set of predefined choices.”
- The `aria-autocomplete="list"` attribute tells users that a list of options will appear.
- The `aria-expanded` attribute tells users whether the menu is showing or not by toggling it's value between `true` and `false`.
- The `autocomplete="off"` attribute stops browsers making their own suggestions which would interfere with those offered by the component.

Next, we inject a `<ul>` after the text box which will store the suggestions.

```HTML
<ul
	role="listbox"
	class="autocomplete-options autocomplete-options-isHidden"
	>
	<li	role="option" tabindex="-1" aria-selected="false" data-option-value="1" id="autocomplete_1">
		France
	</li>
	<li role="option" tabindex="-1" aria-selected="true" data-option-value="2" id="autocomplete_2">
		Germany
	</li>
</ul>
```

Notes:

- The `role="list"` attribute tells users there is a list of choices from which the user can select. Each `<li>` has `role="option"` to denote it as a choice within the list.
- The `aria-selected="true"` attribute tells users whether the option is selected or not by toggling the value between `true` and `false`.
- The `tabindex="-1"` attribute allows us to set focus to the options programatically. More on this shortly.
- The `data-option-value` attribute is to store the corresponding `select` option value. When the user selects an option, we populate the hidden `select` box accordingly so that the real value will be persisted on submission.

Suggestions appear in the menu giving sighted users feedback. To give screen reader users an equivalent experience we need to inject a live region. We can do this by injecting “13 results are available” into the `div`. By doing this we satisfy principle 1, “provide a comparable experience”.

```HTML
<div aria-live="polite" role="status"></div>
```

The `role="status"` and `aria-live="polite"` attributes tell screen readers to announce the content when it changes, but only after the user stops typing—otherwise it would interupt them. Both attributes are functionally equivalent but we include both as older screen readers don't recognise `role`.

Next we need to enrich the text box with some Javascript events. Let's run through the main interactions now. First we need to listen to the text box `keyup` event.

```JS
AutoComplete.prototype.addTextBoxEvents = function() {
	this.textBox.on('keyup', $.proxy(this, 'onTextBoxKeyUp'));
};

Autocomplete.prototype.onTextBoxKeyUp = function(e) {
	switch (e.keyCode) {
		case this.keys.esc:
			// ignore when users presses escape
			break;
		case this.keys.up:
			// ignore when the user presses up
			break;
		case this.keys.left:
			// ignore when the user presses left
			break;
		case this.keys.right:
			// ignore when the user presses right
			break;
		case this.keys.down:
			// move onto first suggestion
			this.onTextBoxDownPressed(e);
			break;
		case this.keys.space:
			// ignore this, otherwise the
			// the menu will show again.
			break;
		case this.keys.enter:
			// ignore this, otherwise the menu 
			// shows briefly before submission
			break;
		default:
			// show suggestions
			this.onTextBoxType(e);
	}
};
```

We're only really interested when the user presses <kbd>down</kbd> or a character to match on. When the user types a character, we want to show matching options (and update the live region).

```JS
Autocomplete.prototype.onTextBoxType = function(e) {
	if(this.textBox.val().trim().length > 0) {
		var options = this.getOptions(this.textBox.val().trim().toLowerCase());
		this.buildMenu(options);
		this.showMenu();
		this.updateStatus(options.length);
	}
	this.updateSelectBox();
};
```

Let's run through this function. The condition checks to see if the user has typed a value. If they have then it calls `this.getOptions()` which takes that value, and returns any matching options. It then builds and shows the menu before updating the live region. Finally, the hidden select box is updated.

Pressing <kbd>down</kbd> should move focus and highlight the first suggestion. If the user presses <kbd>down</kbd> without typing anything then all the possible options are shown.

```JS
Autocomplete.prototype.onTextBoxDownPressed = function(e) {
	var option;
	var options;
	var value = this.textBox.val().trim();
	// Empty value or exactly matches an option 
	// then show all the options
	if(value.length === 0 || this.isExactMatch(value)) {
		options = this.getAllOptions();
		this.buildOptions(options);
		this.showOptionsPanel();
		option = this.getFirstOption();
		this.highlightOption(option);
	} else {
		options = this.getOptions(this.textBox.val().trim());
		if(options.length > 0) {
			this.buildOptions(options);
			this.showOptionsPanel();
			option = this.getFirstOption();
			this.highlightOption(option);
		}
	}
};
```

This function is quite similar to the one just described. The difference is that, the first option is retrieved before highlighting it. The `highlightOption` (shown below) takes an option to highlight.

```JS
Autocomplete.prototype.highlightOption = function(option) {
	if(this.activeOptionId) {
		var activeOption = this.getOptionById(this.activeOptionId);
		activeOption.removeClass('autocomplete-option-isActive');
		activeOption.attr('aria-selected', 'false');
	}

	option.addClass('autocomplete-option-isActive');
	option.attr('aria-selected', 'true');

	if(!this.isElementVisible(option.parent(), option)) {
		option.parent().scrollTop(option.parent().scrollTop() + option.position().top);
	}

	this.activeOptionId = option[0].id;
	option.focus();
};

```

The function first checks to see if there is highlighted option already. If there is, the highlight is removed by removing the class and setting `aria-selected` to `false`. Then, the new option is given that class and sets the same attribute to `true`. Then it checks to see if the element is visible within the menu, because that option may not be visible. If it's not visible, we bring it into view. Finally, the new option is focused.

Now we need to talk about how users interact with the menu. Mouse (or touch) users can scroll the menu and click an option. First we listen to the menu's click event. the handler then grabs the suggestion (`e.currentTarget`) and hands that off to the `selectSuggestion` method, which we'll discuss next.

```JS
Autocomplete.prototype.addSuggestionEvents = function() {
	this.optionsUl.on('click', '.autocomplete-option', $.proxy(this, 'onSuggestionClick'));
};

Autocomplete.prototype.onSuggestionClick = function(e) {
	var suggestion = $(e.currentTarget);
	this.selectSuggestion(suggestion);
};
```

The `selectionSuggestion` method grabs the corresponding value out of the `data-option-value` attribute. It then uses that value to update the text box and the hidden select box. Finally, it hides the options and sets focus to the text box.

```JS
Autocomplete.prototype.selectSuggestion = function(suggestion) {
	var value = suggestion.attr('data-option-value');
	this.textBox.val(value);
	this.setValue(value);
	this.hideOptions();
	this.focusTextBox();
};
```

For keyboard we end up performing the exact same routine. It's just that this time we perform that routine when the user presses <kbd>space</kbd> or <kbd>enter</kbd>.

The other actions can be summed up briefly. When the user presses:

- <kbd>up</kbd>, the previous option is focused. If it's the first option, focus is set to the text box.
- <kbd>down</kbd>, the next option is focused.
- <kbd>tab</kbd>, the menu is hidden.
- <kbd>escape</kbd>, the menu is hidden and focus is set to the text box.
- a character, then focus is set to the text box so users can continue typing.

## 2. Choosing When To Fly

Dates are notoriously hard[^5]: different time zones, formats, delimiters, days in the month, length of a year, daylight savings and on and on. It's hard work designing all of this complexity out of an interface.

Often three select boxes are used: one for day, month and year. Admittedly, we've just discussed the cons of select boxes, but it must be said that one of their redeeming qualities is that they stop users entering wrong information. But in the case of dates, even *this* quality doesn't hold true. For example, you can select an invalid date such as *31 February 2017*.

![Select boxes for dates](./images/date-select.png)
[https://www.gov.uk/state-pension-age/y/age]

Select boxes are also used to avoid locale and formatting differences. Some dates start with month, others with day. Some delimit dates with slashes, others with dashes or dots. We can't reliably determine the user's intention based on what they enter. It's just one of those things.

![What date is this 10/09/17](.)

### Types Of Date

Many of us assume that using a calendar widget is always better than letting users type freely into a text box unassisted. But this is not always the case. As the Goverment Digital Service (GDS) states:

> “The way you should ask for dates depends on the types of date you’re asking for.”

Let's step through some of the main types of dates and see how we can handle them. Then we can see if any match up to the case of choosing a date to fly on.

#### Dates From Documents

> “If you ask for a date exactly as it’s shown on a passport, credit card or similar item, make the fields match the format of the original. This will make it easier for users to copy it across accurately.*

The expiry date from “A Checkout Flow” falls perfectly under this category. As the expiry date is just 4 characters with an optional slash, we gave users a single text box that matches the expected format. Essentially, users just copy with they see. Easy.

#### Memorable Dates

A memorable date is one that you remember easily such as your date of birth. Typing 6 digits unassisted into a text box is much quicker than scrolling, swiping and clicking through multiple years and months within a calendar.

In this case, you should use three text boxes: one for day, month and year. Why three? Because it solves the locale and formating issues mentioned earlier.

![GDS date of birth](./images/gds-dob.png)

```HTML
<div class="field">
	<fieldset>
		<legend>
			<span class="field-legend">Date of birth</span>
			<span class="field-hint">DD MM YYYY</span>
		</legend>
		<div class="field-dayWrapper">
			<label for="day">Day</label>
			<input class="field-dayBox" type="text" pattern="[0-9]*" name="day" id="day">
		</div>
		<div class="field-monthWrapper">
			<label for="month">Month</label>
			<input class="field-monthBox" type="text" pattern="[0-9]*" name="month" id="month">
		</div>
		<div class="field-yearWrapper">
			<label for="year">Year</label>
			<input class="field-yearBox" type="text" pattern="[0-9]*" name="year" id="year">
		</div>
	</fieldset>
</div>
```

The three fields are wrapped in a `fieldset`. The `legend` (“Date of birth”) gives each text box context and would be read out as “Date of birth, day” (or similar) as the user steps through each field.

*(Note: the pattern attribute is used to trigger the numeric keyboard - a little enhancement for iOS users, as discussed in “A Checkout Flow”.)*

#### Calendar Widgets

When choosing a date to fly, users are neither entering a memorable date, nor one found in a document. They are searching for a date in the future.

We tend to think of time in structured chunks: days, weeks and months etc. And we organise our lives using a calendar which aligns with that notion. It's sensible then, to let users find and pick a date through a familiar and intuitive calendar interface.

The primary user need at this stage of the journey is to select a date — nothing more. So trying to squeeze extra information into it, such as price or availability, is going to result in a busy and overwhelming experience that slows users down.

It's also not practical from a design perspective. Responsive design is about designing interfaces that work well in large and small screens. There's simply not enough room to add more information into each cell of a date picker.

Instead, we'll let users focus on choosing a date unencumbered and later we'll give users more information when it's both useful and practical to do so.

### The Date Input

As usual, our first port of call is to look at what browsers give us for free: HTML5 introduced the date input (`input type="date"`) which offers a special interface for picking dates while enforcing a standard format value that's sent to the server. And mobile browser support is really good too.

![Mobile Date Input](./images/mobile-date.png)

Desktop browser support is patchy. Chrome and Edge have support but Firefox, for example, doesn't have any support (at time of writing).

![Desktop native date control](./images/desktop-date.png)

#### Things Look Different

Don't be too concerned about the difference in appearance. Users either don't notice or don't care. And mose people use the same browser and device daily. Unlike us, they are not agonising over subtle differences during testing.

> Nobody cares about your website as much as you do

If you're not able to conduct your own user research, watch “Progressive Enhancement 2.0”, at 16 minutes in[^8]. Nicholas Zakas shows the audience a photo. He then moves to the next slide which contains the same photo. He then asks the audience if they noticed any differences. Even though the second photo had a border and drop shadow, not one person noticed. Remember the audience was full of designers and developers — people who are trained to notice these things. They didn't notice, because like any user, they were focused on the content.

And if that's not enough proof, may I point you in the direction of “Do Websites Need To Look Exactly The Same In Every Browser”[^].

### A Date Picker

Mobile (and some desktop) users are fine - they get the native date input. But what about people using an unsupported browser? Unless we do something, picking a date is going to be harder than it should be. Really, we should give users a better experience by providing our own date picker.

But we only want to give unsupported browsers the date picker, otherwise users will get two date pickers - the native one and ours. You can check support by using this function:

```JS
function supportsDateInput() {
	var el = document.createElement('input');
	try {
		el.type = "date";
	} catch(e) {}
	return el.type == "date";
}
```

The function first tries to create a date input. Then it checks to see if the browser reports the `type` as `date`. If it there is no support, browsers will show a text box and report the `type` as `text`.

We can use the function like this:

```JS
if(!supportsDateInput()) {
  // define date picker component
}
```

#### The Enhanced Interface

The enhanced interface takes the text box and injects a button beside it. Clicking the button reveals the calendar.

![The Date Picker](.)

Many date pickers are designed as overlays, but they obscure the rest of the page and are prone to disappearing off screen. Instead the calendar is positioned underneath and inline which doesn't suffer from these issues.

There is an inset left border which visually connects the calendat to the field. And the interactive elements within the calendar have large tap targets which are easier to tap with a finger or click with a mouse.

#### Revealing The Calendar

As noted earlier, clicking the toggle button, reveals the calendar.

```HTML
<div class="field ">
  <label for="when">
    <span class="field-label">Date</span>
  </label>
  <div class="datepicker">
  	<input type="text" id="when" name="when">
  	<button type="button" aria-expanded="true" aria-haspopup="true">Choose</button>
  	<div class="datepicker-wrapper" hidden>
  		Calendar widget here
  	</div>
  </div>
```

Notes:

- The `type="button"` attribute stops the button from submitting the form. If it was left undefined or set to “submit” it would submit the form.
- The `aria-haspopup="true"` attribute indicates that the button secretes a calendar. It acts as a warning that, when pressed, the focus will be moved to the calendar. Note: its value is always set to `true`.
- The `aria-haspopup` attribute is complemented by `aria-expanded`. This tells screen reader users whether the calendar is currently in an open (expanded) or closed (collapsed) state by toggling between `true` and `false` values.
- The calendar is hidden using the `hidden` attribute/property as explained in chapter 1, “A Registration Form”.

#### Keyboard And Focus Behaviour

When the button is clicked, focus is set to the calendar widget programmatically by giving the element `tabindex="-1"`. Using `tabindex="0"` means the the calendar will be permanently focusable by way of the tab key which is a 2.4.3 Focus Order WCAG fail. That's because if users can tab to something, they expect that it will actually do something.

```HTML
<div class="datepicker-calendar" tabindex="-1">
</div>
```

When focus is set to the calendar, the user can then <kbd>tab</kbd> between the three calendar controls: the previous month button, the next month button and the currently selected date, which defaults to today.

```HTML
<table role="grid">
  <thead>...</thead>
  <tbody>
    <tr>
      <td tabindex="-1" aria-label="1 October, 2017">
      	<span aria-hidden="true">1</span>
      </td>
      <td tabindex="-1" aria-label="2 October, 2017">
      	<span aria-hidden="true">2</span>
      </td>
      <td tabindex="-1" aria-label="3 October, 2017">
      	<span aria-hidden="true">3</span>
      </td>
      <td tabindex="-1" aria-label="4 October, 2017">
      	<span aria-hidden="true">4</span>
      </td>
      <td tabindex="-1" aria-label="5 October, 2017">
      	<span aria-hidden="true">5</span>
      </td>
      <td tabindex="0" aria-label="6 October, 2017">6</td>
      <td tabindex="-1" aria-lael="7 October, 2017">
      	<span aria-hidden="true">7</span>
      </td>
    </tr>
    <tr>...</tr>
    <tr>...</tr>
    <tr>...</tr>
  </tbody>
</table>
```

Notes:

- The `role="grid"` attribute tells screen readers, such as JAWS, that this is not a regular table and that the arrow keys can be used to navigate it with Javascript.
- Each `<td>` has `tabindex="-1"` except for the selected day, which has `tabindex="0"`. This means that the user can tab straight onto the selected day and navigate from there using the arrow keys.
- When the user presses the arrow keys to select a new day the previously selected day will be set to `tabindex="-1"` and the newly selected day will be set to `tabindex="0"`. This is known as roving tabs, which makes the grid just one tab stop. Otherwise users would have to tab ~30 times to move beyond the calendar with their keyboard.
- Pressing <kbd>left</kbd> moves to the previous day. Pressing <kbd>right</kbd> moves to the next day. Pressing <kbd>up</kbd> moves to the same day in the previous week. Pressing <kbd>down</kbd> moves to the same day in the next week. Users can move freely between months using this approach.
- Pressing <kbd>Enter</kbd> or <kbd>Space</kbd> will confirm selection, populate the text box with the date, move focus back to the text box and finally, close the calendar.
- Pressing <kbd>escape</kbd> hides the calendar and moves focus to the button.
- Pressing <kbd>tab</kbd> within the grid should hide the calendar and move to the next control, whatever that may be.

#### Screen Readers

When the button is pressed, sighted users will see visual feedback because the calendar appears. As clicking the button moves focus to the calendar, we can give screen reader users feedback by using a live region which contains “October 2017”. The same thing is read out as users move between months (more on this shortly).

```HTML
<div role="status" aria-live="polite">October 2017</div>
```

The `thead` contains the column headings which represent each day of the week. The days are abbreviated visually to save space - tables aren't stylistically malleable, something that we'll discuss at length in chapter 5. While abbreviations work here, we can give greater clarity for screen reader users by putting the full version inside the `aria-label` attribute.

```HTML
<thead>
  <tr>
    <th aria-label="Sunday">Sun</th>
    <th aria-label="Monday">Mon</th>
    <th aria-label="Tuesday">Tue</th>
    <th aria-label="Wednesday">Wed</th>
    <th aria-label="Thursday">Thu</th>
    <th aria-label="Friday">Fri</th>
    <th aria-label="Saturday">Sat</th>
  </tr>
</thead>
```

The same technique is used for the day of the month. That is, just the number is shown visually which isn't enough for visually-impaired users. For them, we store the full date inside the `aria-label` attribute, which speaks to principle 1, *provide a comparable experience*.

```HTML
<td tabindex="-1" aria-lael="7 October, 2017">
  <span aria-hidden="true">7</span>
</td>
```

*(Note: the hidden span stops the number being read out twice by some screen readers.)*

While screen reader users *can* operate the calendar, it's not especially useful to them. Entering a date by typing directly into the text box is probably easier and quicker. But we don't make those assumptions. Instead, we adhere to principle 5, *offer choice* by letting them do either.

#### Future Considerations

I love how Jeremy Keith thinks about the web as a *continuum*[^10]. By that he means it's constantly changing. Technology evolves at a rapid pace and browsers and devices are released all the time. Each of which have varying features and capabilities.

At any particular moment in time we need to decide what level of support makes sense for a given feature.

Earlier, we made the decision to create an enhanced experience for (desktop) browsers that don't support the native date input by creating our own date picker. As time goes by browser support will get better and better. At the same time, the number of people who would have experienced the degraded version will diminish.

At which time, we can quietly remove the Javascript code, giving us less to maintain and users a faster experience (as they won't have to load that code). Lovely.

#### Doing Our Best

We have considered people who use a supporting browser. And we have considered those using an unsupported browser. But we haven't considered those using an unsupported browser that experiences a network or Javascript failure as described in “A Registration Form”.

> There is something to be said for design that ignores people, people ignore it.

In this case users will see a text box asking for date. It's not what they see that matters here, it's what they don't see. And they don't see a hint explaining the expected format. We can't just add a hint because browsers that support the date input will use a different format.

![Demo](.)

The only thing we can do is be as forgiving as possible, by letting users type slashes, periods or spaces: whichever they prefer. But typing a two-digit year first, for example, will cause an error. In this case, a well-written error message will have to do.

Design, even inclusive design, is always a question of priorities. Sometimes what is good for the majority creates an experience that is less than ideal for the minority.

Fortunately, in this case, it's okay: users are still able to enter a date. Sometimes that's the difference between inclusivity and accessibility. Inclusive design is about giving everyone a good experience. Accessibility is about giving users a way of doing it, even if it's not ideal.

Designing inclusively is about doing our best and I think we've done that here.

## 3. Choosing passengers

Users need to specify how many adults, children and infants are flying on the trip. Categorisations are based on age and affect the price of the ticket.

![Passengers](./images/choose-passengers.png)

```HTML
<div class="field">
    <label for="adults">
    	<span class="field-label">How many adults, 16 years and over, are flying?</span>
    </label>
    <input type="number" id="adults" name="adults">
</div>
<div class="field">
    <label for="children">
    	<span class="field-label">How many children, aged between 2 and 15 years old, are flying?</span>
    </label>
    <input type="number" id="children" name="children">
</div>
<div class="field">
    <label for="infants">
    	<span class="field-label">How many infants, under 2 years old, are flying?</span>
    </label>
    <input type="number" id="infants" name="infants">
</div>
```

Each category is represented by a separate field. As we're asking users for an *amount* of something&mdash;in this case passengers&mdash;the number input makes sense.

Turn off spinners in webkit:

```CSS
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    appearance: none;
    margin: 0;
}
```

### A stepper component

Spinners, also known as steppers, let users increase or decrease the value by a constant amount. They are great for making small adjustments. In the same article as referenced earlier, Luke Wobrelkski says:

> When testing mobile flight booking forms, we found people preferred steppers for selecting the number of passengers. No dropdown menu required, especially since there's a maximum of 8 travelers allowed and the vast majority select 1-2 travelers.

Having discussed number inputs in chapter 2, we know that some browsers provide their own stepper buttons. The problem is that they are tiny and hard to click. And mobile browsers don't show them at all&mdash;they just show a numeric keyboard. Instead, we can disable the browser spinners and enhance the input with larger ones. Not only are they easier to use on desktop but it saves mobile users having to trigger their on-screen keyboard.

![Thing](.)

```HTML
<div class="field">
    <label for="adults">
    	<span class="field-label">How many adults, 16 years and over, are flying?</span>
    </label>
    <button type="button" aria-label="Decrement">-</button>
    <input type="number" id="adults" name="adults">
    <button type="button" aria-label="Increment">+</button>
</div>
<div class="field">
    <label for="children">
    	<span class="field-label">How many children, between 2 and 15 years old, are flying?</span>
    </label>
    <button type="button" aria-label="Decrement">-</button>
    <input type="number" id="children" name="children">
    <button type="button" aria-label="Increment">+</button>
</div>
<div class="field">
    <label for="infants">
    	<span class="field-label">How many infants, under 2 years old, are flying?</span>
    </label>
    <button type="button" aria-label="Decrement">-</button>
    <input type="number" id="infants" name="infants">
    <button type="button" aria-label="Increment">+</button>
</div>
```

A few notes:

- Whilst the best icons are text[^12] [need to expand this and talk about pros and cons, perhaps it's own section], the button text employs universally understood minus and plus symbols. This keeps the interface clean and in balance.
- The `aria-label` describes the symbol with text for those using screen readers. Instead of hearing ‘Button, minus symbol’ they'll hear ‘Button, increment’ or similar.

```JS
function Stepper(input) {
	this.input = $(input);
	this.container = this.input.parent();
	this.wrapper = $('<div class="stepper"></div>');
	this.wrapper.append(this.input);
	this.container.append(this.wrapper);
	this.createDecrementButton();
	this.createIncrementButton();
	this.wrapper.on('click', '.stepper-decrementButton', $.proxy(this, 'onDecrementClick'));
	this.wrapper.on('click', '.stepper-incrementButton', $.proxy(this, 'onIncrementClick'));
}

Stepper.prototype.createDecrementButton = function() {
	this.decrementButton = $('<button tabindex="-1" aria-label="Decrement" class="stepper-decrementButton" type="button">&#45;</button>');
	this.wrapper.prepend(this.decrementButton);
};

Stepper.prototype.createIncrementButton = function() {
	this.incrementButton = $('<button tabindex="-1" aria-label="Increment" class="stepper-incrementButton" type="button">&#43;</button>');
	this.wrapper.append(this.incrementButton);
};

Stepper.prototype.getInputValue = function() {
	var val = parseInt(this.input.val(), 10);
	if(isNaN(val)) {
		val = 0;
	}
	return val;
};

Stepper.prototype.onDecrementClick = function(e) {
	var val = this.getInputValue();
	this.input.val(val-1);
};

Stepper.prototype.onIncrementClick = function(e) {
	var val = this.getInputValue();
	this.input.val(val+1);
};
```

Notes:

- The script prepends and appends the decrement and increment buttons respectively.
- Clicking *increment* increases the value by 1.
- Clicking *decrement* decreases the value by 1.

## 4. Choosing a flight

Up to now, we've merely been collecting user's information. All with the goal of letting users choose a preferential flight. Now we have what we need, we can display a list of flights from which the user can choose. Here's how it might look:

![Image](./images/image.png)

```HTML
<div class="field">
	<fieldset>
		<legend>
			<span class="field-legend">Flights on Friday 19 June 2019</span>
		</legend>
		<div class="field-radioButton">
			<label for="flight">
				<input type="radio" name="flight" value="" id="flight" >
				Departing at 7:20am. Arriving at 10:30am. £169.
			</label>
		</div>
		<div class="field-radioButton">
			<label for="flight1">
				<input type="radio" name="flight" value="" id="flight1" >
				Departing at 12:20pm. Arriving at 14:30pm. £125.
			</label>
		</div>
		<div class="field-radioButton">
			<label for="flight1">
				<input type="radio" name="flight" value="" id="flight1" >
				Departing at 18:20pm. Arriving at 20:30pm. £99.
			</label>
		</div>
	</fieldset>
</div>
```

Earlier, the user specified a date on which to fly. Therefore we're showing flights that match that date. However, we give users the freedom to move back and forth between days using a standard pagination component. This, by the way, conforms to principle 5, *offer choice*.

Each flight is represented as a radio button. The label contains departure time, arrival time and ticket price which is everything they need to make their selection. This is one of the nice things about radio buttons, they grant you the flexibility to add as much information to the label as necessary, with the flexibility to lay out the information hierarchically.

Earlier I noted that one of the advantages to radio buttons is that we can format the content of the label to suit the needs of users. This is because we can put constructs inside the `<label>` that can be targetted with CSS.

```HTML
<label>
	TODO stuff etc
</label>
```

## 5. Choosing where to sit

In Checkboxes Are Never Round[^13], Daniel De Laney says that ‘interactive things have perceived affordances; the way they look tells us what they do and how to use them. That’s why checkboxes are square and radio buttons are round. Their appearance isn’t just for show&mdash;it signals what to expect from them. Making a checkbox round is like labeling the Push side of a door Pull.’

Essentially, Daniel is saying that radio buttons tell you that just one can be selected; and checkboxes tell you that *more than one* can. Therefore if one person is travelling use radio buttons, otherwise use checkboxes.

![Image](./images/image.png)

### Breaking the rules

There are many seats on a plane, therefore there are many inputs on-screen. Traditional advice says radio buttons should be used when there are less than 7 choices, otherwise use a select box. As designers, rules allow us to think less. In the context of a long form, the rule makes sense. But there are always exceptions to the rule. When blindly accept rules, without thinking about the context, the resulting solution can suffer.

Our context is different. Choosing a seat is a niche interaction and therefore benefits from special treatment. Also, we don't have the same problem as a standard form because we're using One Thing Per Page from chapter 2. This lets us use the majority of the screen to design something better.

### Layout

Before now, we've ‘stacked’ form fields beneath one an other. Here we've laid them out in rows a bit like they are on a plane. Designing the interface based on a user's mental model makes choosing a seat that bit easier, at least for sighted users. To conform to principle 1&mdash;*provide a comparable experience*&mdash;we need to add visually hidden text just for screen readers.

```
<fieldset>
	<legend>
		Choose seats (WORDS)
	</legend>
	<fieldset>
		<legend>First class</legend>
		<div class="plane-row">
			<div class="plane-seat">
				<label for="S1A">
					<input type="checkbox" class="plane-checkbox" name="seat" value="1A" id="S1A">
					1A <span class="plane-seatDescripion">Window</span>
				</label>
			</div>
			<div class="plane-seat">
				<label for="S1B">
					<input type="checkbox" class="plane-checkbox" name="seat" value="1B" id="S1B">
					1B <span class="plane-seatDescripion">Isle</span>
				</label>
			</div>
			<div class="plane-seat">
				<label for="S1C">
					<input type="checkbox" class="plane-checkbox" name="seat" value="1C" id="S1C">
					1C <span class="plane-seatDescripion">Isle</span>
				</label>
			</div>
			<div class="plane-seat">
				<label for="S1D">
					<input type="checkbox" class="plane-checkbox" name="seat" value="1D" id="S1D">
					1D <span class="plane-seatDescripion">Window</span>
				</label>
			</div>
		</div>
		<div class="plane-row">...</div>
	</fieldset>
	<fieldset>
		<legend>Economy class</legend>
		<div class="plane-row">
			<div class="plane-seat">
				<label for="S1A">
					<input type="checkbox" class="plane-checkbox" name="seat" value="1A" id="S1A">
					1A <span class="plane-seatDescripion">Window</span>
				</label>
			</div>
			<div class="plane-seat">
				<label for="S1B">
					<input type="checkbox" class="plane-checkbox" name="seat" value="1B" id="S1B">
					1B
				</label>
			</div>
			<div class="plane-seat">
				<label for="S1C">
					<input type="checkbox" class="plane-checkbox" name="seat" value="1C" id="S1C">
					1C <span class="plane-seatDescripion">Isle</span>
				</label>
			</div>
			<div class="plane-seat">
				<label for="S1D">
					<input type="checkbox" class="plane-checkbox" name="seat" value="1D" id="S1D">
					1D <span class="plane-seatDescripion">Isle</span>
				</label>
			</div>
			<div class="plane-seat">
				<label for="S1E">
					<input type="checkbox" class="plane-checkbox" name="seat" value="1E" id="S1E">
					1E
				</label>
			</div>
			<div class="plane-seat">
				<label for="S1F">
					<input type="checkbox" class="plane-checkbox" name="seat" value="1F" id="S1F">
					1F <span class="plane-seatDescripion">Window</span>
				</label>
			</div>
		</div>
		<div class="plane-row">...</div>
	</fieldset>
</fieldset>
```

Screen readers will announce the seat description text as it's part of the label. Sighted users don't need it so we hide this with CSS:

```CSS
.plane-seatDescription {
	position: absolute;
	left: -9999em;
}
```

You'll notice the use of nested fieldsets. They're needed to indicate which seat belongs to which class (first class or economy). The manner in which we've asked for information before now has increased the complexity here. That is, we didn't ask users how they want to travel, and therefore we need to capture that here.

If, for example, we asked users how they wanted to travel beforehand, we could simplify the experience by showing less seats and not needing the extra level of grouping. Adding a little friction upfront is a worthwhile trade-off.

To mitigate this extra, albeit, tiny amount of friction we can use smart defaults (as discussed in chapter 2) by simply marking *economy* by default. After all, this is the most common choice. Here's how the simplified experience looks:

![Image](./images/image.png)

```HTML
<div class="field field--seats">
	<fieldset>
		<legend>
			<span class="field-legend">Choose seats</span>
		</legend>
		<div class="plane-row">
			<div class="plane-seat">
				<label for="S1A">
					<input type="checkbox" class="plane-checkbox" name="seat" value="1A" id="S1A">
					1A <span class="plane-seatDescripion">Window</span>
				</label>
			</div>
			<div class="plane-seat">
				<label for="S1B">
					<input type="checkbox" class="plane-checkbox" name="seat" value="1B" id="S1B">
					1B
				</label>
			</div>
			<div class="plane-seat">
				<label for="S1C">
					<input type="checkbox" class="plane-checkbox" name="seat" value="1C" id="S1C">
					1C <span class="plane-seatDescripion">Isle</span>
				</label>
			</div>
			<div class="plane-seat">
				<label for="S1D">
					<input type="checkbox" class="plane-checkbox" name="seat" value="1D" id="S1D">
					1D <span class="plane-seatDescripion">Isle</span>
				</label>
			</div>
			<div class="plane-seat">
				<label for="S1E">
					<input type="checkbox" class="plane-checkbox" name="seat" value="1E" id="S1E">
					1E
				</label>
			</div>
			<div class="plane-seat">
				<label for="S1F">
					<input type="checkbox" class="plane-checkbox" name="seat" value="1F" id="S1F">
					1F <span class="plane-seatDescripion">Window</span>
				</label>
			</div>
		</div>
		<div class="plane-row">
			...
		</div>
	</fieldset>
</div>
```

Note: Unavailable seats are marked as disabled (by setting a disabled attribute on the checkbox). Disabled controls aren't perceivable meaning you can't tab to them and screen readers won't announce them.

### Enhancing the interface

Visually presenting seats by row may cause seats to wrap (or cause a horizontal scroll bar) on small screens. Whilst this is certainly accessible, it's not ideal. If user research shows this to be a problem, we can look at ways to save space.

One way to do that is by hiding the checkboxes and making the seats look clickable. Hiding the checkboxes through CSS alone is dangerous because pressing <kbd>tab</kbd> moves focus to the checkbox, not the label. This breaks the interface because as a user tabs to the hidden checkbox, there is no feedback to know which, if any, checkbox is focussed.

To fix this, we need to use Javascript by listening to `focus` and `blur` events. As the user moves focus to the visually hidden checkbox, a class is added to the *visible* label giving users the illusion that it is focused.

![Focus](.)

```CSS
.enhanced .plane-seat input {
	position: absolute;
	left: -9999em;
	top: 0;
}

.enhanced .plane-seat-isFocussed label {
	/* etc */
}
```

The CSS is applied only once we know Javascript is available. This is achieved by adding a class of `enhanced` to the document element in the `<head>` of the document:

```JS
document.documentElement.className = 'enhanced';
```

While we're adding these enhancements, we should consider limiting how many seats users can select based on how many passengers they specified earlier. Radio buttons don't need this enhancement as only one is selectable. In the case of checkboxes, however, the user can select more than their quota. When they do, they'll get an error.

Without user research it's hard to know if this is a problem or not. If it proves to be, we can limit selection with Javascript. One way to do this is to disable the remaining seats as soon as the limit is reached. But what if the user wants to choose another seat as a replacement? If we disable the remaining seats the interface won't respond.

![Showing no feedback](.)

Savvy users may realise they have to deselect currently-selected seats first. Less savvy users probably won't. Instead, let's do the hard work for them. As the user surpasses their quota the script should uncheck the previous selection automatically.

```JS
function SeatEnhancer(max) {
	this.max = max;
	this.checkboxes = $('.plane-seat input');
	this.checkboxes.on('focus', $.proxy(this, 'onCheckboxFocus'));
	this.checkboxes.on('blur', $.proxy(this, 'onCheckboxBlur'));
	this.checkboxes.on('click', $.proxy(this, 'onCheckboxClick'));
}

SeatEnhancer.prototype.onCheckboxFocus = function(e) {
	$(e.target).parents('.plane-seat').addClass('plane-seat-isFocussed');
};

SeatEnhancer.prototype.onCheckboxBlur = function(e) {
	$(e.target).parents('.plane-seat').removeClass('plane-seat-isFocussed');
};

SeatEnhancer.prototype.onCheckboxClick = function(e) {
	if(this.getCheckedSeats().length > this.max) {
		var last = this.getLastCheckedSeat();
		this.markAsUnchecked(last);
		last[0].checked = false;
	}

	var checkbox = $(e.target);
	if(checkbox[0].checked) {
		this.markAsChecked(checkbox);
		this.lastChecked = checkbox;
	} else {
		this.markAsUnchecked(checkbox);
		if(this.lastChecked) {
			if(checkbox[0] == this.lastChecked[0]) {
				this.lastChecked = null;
			}
		}
	}

};

SeatEnhancer.prototype.markAsChecked = function(checkbox) {
	checkbox.parents('.plane-seat').addClass('plane-seat-isSelected');
};

SeatEnhancer.prototype.markAsUnchecked = function(checkbox) {
	checkbox.parents('.plane-seat').removeClass('plane-seat-isSelected');
};

SeatEnhancer.prototype.getLastCheckedSeat = function() {
	if(this.lastChecked) {
		return this.lastChecked;
	} else {
		var checked = this.getCheckedSeats();
		if(checked.length) {
			return $(checked[checked.length-1]);
		} else {
			return null;
		}
	}
};

SeatEnhancer.prototype.getCheckedSeats = function() {
	return this.checkboxes.filter(':checked');
};

```

Notes:

- The script listens to the checkbox `focus` and `blur` events.
- When the user focuses a checkbox, a class is added to the container enabling the seat to be styled to mimic a focus state. On `blur` the class is removed.
- If the user selects an extra seat, the previously chosen seat is automatically unchecked removing the need for error messages.

## Summary

This chapter continued where we left off from the previous: by leveraging One Thing Per Page, we could design simple but innovative interfaces that fully utilise the available screen estate. As much as we tried to use native form controls, it became necessary to break new ground by designing 4 custom components from scratch.

- An autocomplete component which lets users search through a long list of destinations quickly and accurately.
- A date picker which lets users find a date in the future matching people's concept of time.
- A stepper component which lets users make small adjustments to an amount of passengers effortlessly.
- A seat chooser which makes seat selection simple, even on mobile.

### Things to avoid

- Using radio buttons instead of checkboxes and vice versa.
- Using select boxes when better alternatives exist.
- Letting users do the hard work when the interface can be designed to do it for them.
- Adding complexity later on in a flow, when a little friction upfront drastically reduces it.
- Blindly following age-old advice without consideration of the specific problem.

## Footnotes

[^1]: http://www.lukew.com/ff/entry.asp?1950
[^2]: https://www.amazon.co.uk
[^3]: http://caniuse.com/#feat=datalist
[^4]: https://www.paciellogroup.com/blog/2014/09/web-components-punch-list/
[^5]: http://infiniteundo.com/post/25326999628/falsehoods-programmers-believe-about-time
[^6]: https://www.gov.uk/service-manual/design/dates
[^7]: http://html5doctor.com/html5-forms-input-types/#input-number
[^8]: https://www.youtube.com/watch?v=hdTxeR90_1E
[^9]: http://dowebsitesneedtolookexactlythesameineverybrowser.com/
[^10]: https://adactio.com/journal/6692
[^11]: https://developers.google.com/speed/docs/insights/SizeTapTargetsAppropriately
[^12]: https://thomasbyttebier.be/blog/the-best-icon-is-a-text-label
[^13]: http://danieldelaney.net/checkboxes/?utm_source=designernews