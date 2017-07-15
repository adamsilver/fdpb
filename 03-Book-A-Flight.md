# Book A Flight

In this chapter, we'll design a service that allows users to book a flight. At first this seems a bit *niche*, especially when compared to the two previous chapters, *A Registration Form* and *Checkout*. However, this chapter encourages the exploration of several more complex patterns. Patterns that are very much transferable to other problem domains such as booking a cinema ticket of hotel room.

Here are the main steps in the flow:

1. Choose origin/destination
2. Choose departure/return date
3. Choose passengers
4. Choose flight
5. Choose seat

After step 5, the user will go to pay. I've left it out as we covered payment in the previous chapter.

## 1. Choose origin/destination

The first thing users need to do is select an origin and destination (both consist of the same problems and will use the same pattern.) Without knowing this information, we can't search for flights. The question is how are we going present a list of destinations?

One aspect of being a thoughtful designer is considering the materials that are offered natively. This is because, generally speaking, native controls are familiar and fully accessible. It's also far less work to use them than it is to build our own, as we'll see shortly. This is what comes to mind when reading Deiter Rams famous quote ‘Less but better’.

There are 3 native controls in particular that could work well for choosing a destination. We'll explore those now.

### Select box

Designers often use `select` boxes because they save space. But interface design is about far more than just saving space. Select boxes are problematic because:

- some users find them hard to close.
- some users try to type into them.
- some users confuse focused options with selected ones.
- users aren't able to pinch-zoom options on devices.
- they have limited hierarchy control.
- they hide choices behind an unnecessary extra click.
- they are not easily searchable.

Usability expert, Luke Wobrelski, goes as far to state that *Dropdowns Should be the UI of Last Resort*[^]. We'll take heed of his experience.

### Radio buttons

Unlike select boxes, radio buttons present choices clearly and are generally well-understood and accessible by default. However, they are less suitable when they consist of many choices. That is, we could have hundreds, if not thousands of destinations.

Radio buttons, in this case, create pages that are long and unweildly. Some browsers provide a native search facility (activated by <kbd>CMD+F</kbd>) allowing users to *jump* to the option quickly. But most users aren't aware of this. Moreover we shouldn't rely on an inconspicuous browser feature to fix holes in our own design.

### Search box

A text box (`input type="search"`) is another option. The search type enhances the functionality of a regular text box (`input type="text"`) by allowing the user to clear the contents of the field&mdash;either by tapping the <kbd>x</kbd> or pressing <kbd>escape</kbd>.

![Image here](/etc/)

HTML:

```html
<div class="field">
	<label for="destination">
		<span class="field-label">Destination</span>
	</label>
	<input type="search" name="destination" id="destination">
</div>
```

This design works well when the results of the input are completely dynamic and vast in size and breadth. For example, searching for products on Amazon. But for this service, we have a finite amount of destinations that we know in advance.

If we let users search unassisted, they may end up seeing a message: *we don't fly to that destination*, which seems a bit unnecessary.

### Autocomplete

We really need a control with the features of a search box and select box rolled into one. As the user types a destination, suggestions appear allowing them to autocomplete the field. This saves time scrolling through a plethora of destinations.

HTML5 provides `datalist` which does exactly this. Unfortunately, it's so buggy[^] that it's impossible to design a robust solution for use on the open web.

We'll have to design a custom component. To do this, we'll need to follow some important rules[^]:

- be focusable with the keyboard
- be operable with the keyboard
- work with assistive devices
- work without Javascript

To solve the last problem we need to talk about Progressive Enhancement. Progressive Enhancement is about providing a baseline core experience for everyone. Then, where possible, creating a better, enhanced experience for those using a more capable browser.

The enhanced experience will be an autocomplete. But what will the core experience be: a text box or a select box. In this case, it seems prudent to use a select box. At least, a select box removes the chance of seeing a lack of results after submission and a round trip to the server.

How it looks (before enhancement):

![Image here](/etc/)

HTML:

```html
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

How it looks (after enhancement):

![Image here](/etc/)

HTML:

```html
<div class="field">
	<label for="destination">
		<span class="field-label">Destination</span>
	</label>
	<div class="autocomplete">
		<input
			type="text"
			name="destination"
			id="destination"
			autocomplete="off"
			role="combobox"
			aria-owns="combobox-options--destination"
			aria-autocomplete="list"
			aria-expanded="true"
			class="autocomplete-textBox"
		>
		<ul
			id="autocomplete-options--destination"
			role="listbox"
			class="autocomplete-options autocomplete-options-isHidden"
			>
			<li	role="option">
				France
			</li>
			<li role="option" aria-selected="true">
				Germany
			</li>
		</ul>
		<div
			aria-live="polite"
			aria-atomic="true"
			role="status"
			class="autocomplete-status">
		</div>
	</div>
</div>
```

There are three component parts:

- A text box
- A menu of which to choose a suggestion
- A status box to announce changes to screen reader users

The HTML, in combination with CSS and Javascript will display suggestions beneath the text box as the user types. All the attributes are necessary in order to build an inclusive component that users can operate with a mouse, keyboard and screen reader interchangeably.

Text box notes:

- `role="combobox"` to announce that they are interacting with an autocomplete control, not a regular text box.
- `aria-autocomplete="list"` to announce that in using this autocomplete, a list will appear from which the user can choose.
- `aria-expanded` to indicate the menu is showing or not.
- `aria-owns="combobox-options"` connects the text box to the menu by `id`.
- `autocomplete="off"` stops browsers showing their suggestions and interfering with those offered the component itself.

Option notes:

- `role="option"` to announce it as an option in the list.
- `aria-selected` to indicate the selected option.

Status box notes:

- `aria-role="status" announces the status. For example, *2 results available.*
- `aria-live="polite" announces the status when the user stops typing. Ensuring they aren't interrupted.
- `aria-atomic="true"` means the entire status will be announced, even if Javascript was to optimise what's injected.

```Javascript
function Autocomplete(control) {
	this.control = control;
	this.controlId = control.id;
	this.container = $(control).parent();
	this.wrapper = $('<div class="autocomplete"></div>');
	this.container.append(this.wrapper);
	this.createTextBox();
	this.createButton();
	this.createOptionsUl();
	this.removeSelectBox();
	this.createStatusBox();
	this.setupKeys();
	$(document).on('click', $.proxy(this, 'onDocumentClick'));
};

Autocomplete.prototype.onDocumentClick = function(e) {
	if(!$.contains(this.container[0], e.target)) {
        this.hideOptions();
    }
};

Autocomplete.prototype.setupKeys = function() {
	this.keys = {
		enter: 13,
		esc: 27,
		space: 32,
		up: 38,
		down: 40,
		tab: 9
   };
};

Autocomplete.prototype.addTextBoxEvents = function() {
	this.textBox.on('keyup', $.proxy(this, 'onTextBoxKeyUp'));
	this.textBox.on('keydown', $.proxy(function(e) {
		switch (e.keyCode) {
			// this ensures that when users tabs away
			// from textbox that the normal tab sequence
			// is adhered to. We hide the options, which
			// removes the ability to focus the options
			case this.keys.tab:
				this.hideOptions();
				break;
		}
	}, this));
};

Autocomplete.prototype.addSuggestionEvents = function() {
	this.optionsUl.on('click', 'li', $.proxy(this, 'onSuggestionClick'));
	this.optionsUl.on('keydown', $.proxy(this, 'onSuggestionsKeyDown'));
};

Autocomplete.prototype.onTextBoxKeyUp = function(e) {
	switch (e.keyCode) {
		case this.keys.esc:
			// we ignore when users presses escape
			break;
		case this.keys.up:
			// we ignore when the user presses up when on textbox
			break;
		case this.keys.down:
			// we want to handle this one
			this.onTextBoxDownPressed(e);
			break;
		case this.keys.tab:
			this.hideOptions();
			break;
		case this.keys.enter:
			// we ignore when the user presses enter here,
			// otherwise the menu will show briefly before
			// submission completes
			break;
		default:
			// show suggestion
			this.onTextBoxType(e);
	}
};

Autocomplete.prototype.onSuggestionsKeyDown = function(e) {
	switch (e.keyCode) {
		case this.keys.up:
			// want to highlight previous option
			this.onSuggestionUpArrow(e);
			break;
		case this.keys.down:
			// want to highlight next suggestion
			this.onSuggestionDownArrow(e);
			break;
		case this.keys.enter:
			// want to select the suggestion
			this.onSuggestionEnter(e);
			break;
		case this.keys.space:
			// want to select the suggestion
			this.onSuggestionSpace(e);
			break;
		case this.keys.esc:
			// want to hide options
			this.onSuggestionEscape(e);
			break;
		case this.keys.tab:
			this.hideOptions();
			break;
		default:
			this.textBox.focus();
	}
};

Autocomplete.prototype.onTextBoxType = function(e) {
	if(this.textBox.val().trim().length > 0) {
		var options = this.getOptions(this.textBox.val().trim().toLowerCase());
		if(options.length > 0) {
			this.buildOptions(options);
			this.showOptionsPanel();
		} else {
			this.hideOptions();
			this.clearOptions();
		}
		this.updateStatus(options.length);
	}
};

Autocomplete.prototype.onSuggestionEscape = function(e) {
	this.clearOptions();
	this.hideOptions();
	this.focusTextBox();
};

Autocomplete.prototype.isShowingMenu = function() {
	return this.textBox.attr('aria-expanded', 'true');
};

Autocomplete.prototype.focusTextBox = function() {
	this.textBox.focus();
};

Autocomplete.prototype.onSuggestionClick = function(e) {
	this.textBox.val($(e.currentTarget).text());
	this.hideOptions();
	this.focusTextBox();
};

Autocomplete.prototype.onSuggestionEnter = function(e) {
	if(this.isOptionSelected()) {
		this.selectOption();
	}
	e.preventDefault();
};

Autocomplete.prototype.onSuggestionSpace = function(e) {
	if(this.isOptionSelected()) {
		this.selectOption();
		e.preventDefault();
	}
};

Autocomplete.prototype.selectOption = function() {
	this.textBox.val(this.getActiveOption().text());
	this.focusTextBox();
	this.hideOptions();
};

Autocomplete.prototype.onTextBoxDownPressed = function(e) {
	var option;
	var options;
	// No chars typed
	if(this.textBox.val().trim().length === 0) {
		options = this.getAllOptions();
		this.buildOptions(options);
		this.showOptionsPanel();
	// Chars typed
	} else {
		options = this.getOptions(this.textBox.val().trim());
		if(options.length > 0) {
			this.buildOptions(options);
			this.showOptionsPanel();
		}
	}
	option = this.getFirstOption();
	if(option[0]) {
		this.highlightOption(option);
	}
};

Autocomplete.prototype.onSuggestionDownArrow = function(e) {
	var option = this.getNextOption();
	if(option[0]) {
		this.highlightOption(option);
	}
	e.preventDefault();
};

Autocomplete.prototype.onSuggestionUpArrow = function(e) {
	if(this.isOptionSelected()) {
		option = this.getPreviousOption();
		if(option[0]) {
			this.highlightOption(option);

		} else {
			this.focusTextBox();
			this.hideOptions();
		}
	}
	e.preventDefault();
};

Autocomplete.prototype.isFirstOptionSelected = function() {
	var selectedOption = this.getActiveOption();
};

Autocomplete.prototype.isOptionSelected = function() {
	return this.activeOptionId;
};

Autocomplete.prototype.getActiveOption = function() {
	return $('#'+this.activeOptionId);
};

Autocomplete.prototype.getFirstOption = function() {
	return this.optionsUl.find('li').first();
};

Autocomplete.prototype.getPreviousOption = function() {
	return $('#'+this.activeOptionId).prev();
};

Autocomplete.prototype.getNextOption = function() {
	return $('#'+this.activeOptionId).next();
};

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

Autocomplete.prototype.getOptionById = function(id) {
	return $('#'+id);
};

Autocomplete.prototype.showOptionsPanel = function() {
	this.optionsUl.removeClass('autocomplete-options-isHidden');
	this.optionsUl.attr('aria-hidden', 'false');
	this.textBox.attr('aria-expanded', 'true');
	this.textBox.attr('tabindex', '0');
};

Autocomplete.prototype.hideOptions = function() {
	this.optionsUl.addClass('autocomplete-options-isHidden');
	this.optionsUl.attr('aria-hidden', 'true');
	this.textBox.attr('aria-expanded', 'false');
	this.activeOptionId = null;
	this.clearOptions();
	this.textBox.removeAttr('tabindex', '-1');
};

Autocomplete.prototype.clearOptions = function() {
	this.optionsUl.empty();
};

Autocomplete.prototype.getOptions = function(value) {
	var options = [];
	var selectOptions = this.control.options;
	var text;
	for(var i = 0; i < selectOptions.length; i++) {
		text = $(selectOptions[i]).text();
		if(text.toLowerCase().indexOf(value.toLowerCase()) > -1) {
			options.push(text);
		}
	}
	return options;
};

Autocomplete.prototype.getAllOptions = function() {
	var options = [];
	var selectOptions = this.control.options;
	var text;
	for(var i = 0; i < selectOptions.length; i++) {
		options.push($(selectOptions[i]).text());
	}
	return options;
};

Autocomplete.prototype.buildOptions = function(options) {
	this.clearOptions();
	this.activeOptionId = null;
	for(var i = 0; i < options.length; i++) {
		this.optionsUl.append(this.getOptionHtml(i, options[i]));
	}
	this.optionsUl.scrollTop(this.optionsUl.scrollTop());
};

Autocomplete.prototype.buildAllOptions = function() {
	this.clearOptions();
	this.activeOptionId = null;
	var options = this.control.options;
	for(var i = 0; i < options.length; i++) {
		this.optionsUl.append(this.getOptionHtml(i, $(options[i]).text()));
	}
};

Autocomplete.prototype.getOptionHtml = function(i, text) {
	return '<li tabindex="-1" class="autocomplete-option" aria-selected="false" role="option" id="autocomplete-option--' + i + '">' + text + '</li>';
};

Autocomplete.prototype.createStatusBox = function() {
	this.status = $('<div aria-live="polite" role="status" aria-atomic="true" class="autocomplete-status" />');
	this.wrapper.append(this.status);
};

Autocomplete.prototype.updateStatus = function(resultCount) {
	if(resultCount === 0) {
		this.status.text('No results.');
	} else {
		this.status.text(resultCount + ' results available.');
	}
	window.setTimeout(function() {
		this.status.text('');
	}.bind(this), 1000);
};

Autocomplete.prototype.removeSelectBox = function() {
	$(this.control).remove();
};

Autocomplete.prototype.createTextBox = function() {
	this.textBox = $('<input autocapitalize="none" class="autocomplete-textBox" type="text" autocomplete="off">');
	this.textBox.attr('aria-owns', this.getOptionsId());
	this.textBox.attr('aria-autocomplete', 'list');
	this.textBox.attr('role', 'combobox');
	this.textBox.prop('id', this.controlId);
	this.wrapper.append(this.textBox);
	this.addTextBoxEvents();
};

Autocomplete.prototype.getOptionsId = function() {
	return 'autocomplete-options--'+this.controlId;
};

Autocomplete.prototype.createButton = function() {
	this.button = $('<button class="autocomplete-button" type="button" tabindex="-1">&#9662;</button>');
	this.wrapper.append(this.button);
	this.button.on('click', $.proxy(this, 'onButtonClick'));
};

Autocomplete.prototype.onButtonClick = function(e) {
	this.clearOptions();
	var options = this.getAllOptions();
	this.buildOptions(options);
	this.updateStatus(options.length);
	this.showOptionsPanel();
	this.textBox.focus();
};

Autocomplete.prototype.createOptionsUl = function() {
	this.optionsUl = $('<ul id="'+this.getOptionsId()+'" role="listbox" class="autocomplete-options autocomplete-options-isHidden" aria-hidden="true"></ul>');
	this.wrapper.append(this.optionsUl);
	this.addSuggestionEvents();
};

Autocomplete.prototype.isElementVisible = function(container, element) {
	var containerHeight = $(container).height();
	var elementTop = $(element).offset().top;
	var containerTop = $(container).offset().top;
	var elementPaddingTop = parseInt($(element).css('padding-top'), 10);
	var elementPaddingBottom = parseInt($(element).css('padding-bottom'), 10);
	var elementHeight = $(element).height() + elementPaddingTop + elementPaddingBottom;
    var visible;

    if ((elementTop - containerTop < 0) || (elementTop - containerTop + elementHeight > containerHeight)) {
		visible = false;
    }
    else {
		visible = true;
    }
    return visible;
};
```

Notes:

- Replace the select box with a text box, menu and status.
- Listen as the user types. If options match, display them in the menu.
- Pressing <kbd>up</kbd> or <kbd>down</kbd> moves between options
- Pressing <kbd>enter</kbd> or <kbd>space</kbd>, or clicking with the mouse populates the text box with the option and closes the options
- Pressing <kbd>enter</kbd> when focus is within the text box implicitly submits the form, like normal.
- Clicking the button, reveals all the options, like a select box.
- Pressing <kbd>escape</kbd> hides the options.

## Choosing dates

Dates are hard[^]. Different time zones, formats, delimitters, days in the month, length of a year, day light savings and on and on. It's hard work designing all of this complexity out of an interface.

Traditionally we've used 3 select boxes to capture someone's date of birth&mdash;one for day, month and year. One of the select boxes' redeeming qualities is that they stop users entering wrong information. However, even *this* doesn't hold up in the case of dates. Users can, for example, select *31 February 2017*, which will result in a error.

![Select boxes for dates](./images/date-select.png)
[https://www.gov.uk/state-pension-age/y/age]

But why use select boxes? Because they stop the system needing to handle a plethora of different formats. Some dates start with month; others with day. Some delimit dates with slashes, others with dashes.We can't know what the user intended, and so we can't be forgiving like we otherwise would like.

We can certainly do better than 3 select boxes. But, before designing a date field, we first need to understand what type of date it is. Like Goverment Digital Services (GDS) says in the service manual, *the way you should ask users for dates depends on the types of date you’re asking for*.

### Dates from documents

The expiry date, as discussed in chapter 2, is a good example of this. We presented a simple textbox, that expects a number matching the same format as found on the ‘document’, in this case a debit card. These dates are normally easy, because users simply copy what they see.

### Memorable dates

Dates that are easy to remember, such as date of birth, need a different design approach. The defacto thinking is that date pickers are easier for all sorts of dates.

It's often slower and harder to enter a memorable date in this manner, than it would be to type a few numbers into a text box unassisted. Even a well-designed custom date picker requires far more effort to get the job done.

GDS have a excellent guidance here too. They suggest 3 *separate* text boxes&mdash;one for day, month and year. This is mostly to avoid the formatting issues we discussed before.

How it might look:

![GDS date of birth](./images/gds-dob.png)

HTML:

```HTML
<fieldset>
    <legend>Date of birth</legend>
	<div>
		<label for="day">Day (DD)</label>
		<input type="number" name="day" id="day" pattern="[0-9]*">
	</div>
	<div>
		<label for="month">Month (MM)</label>
		<input type="number" name="month" id="month" pattern="[0-9]*">
	</div>
	<div>
		<label for="year">Year (YYYY)</label>
		<input type="number" name="year" id="year" pattern="[0-9]*">
	</div>
</fieldset>
```

This field uses the `fieldset` and `legend` elements to group the 3 inputs together. Without it, ‘Day’, ‘Month’ and ‘Year’ would be ambiguous on their own.

Some versions of iOS ignore `type="number"` and therefore won't show the numeric keyboard. The `pattern` attribute fixes this problem[^filament].

Some websites, design this type of field to automatically tab to the next box once the correct amount of characters have been typed. This is a problem for users, which we'll be discussing in more detail in the next chapter.

### Date picker

In the case of booking a flight users are nither dealing with a memorable date nor one found in a document. When booking flights, we often orientate ourselves around day and week. To mimic this, we'll need a more convenient solution than three text boxes. We need to use a date picker.

Interfaces that try to solve too many problems at once cause problems. If we tried to convey, for example, price and availability, inside a calendar, this would result in a busy interface that doesn't work very well. That is, unless the user has a super-sized screen.

To begin with, we'll let users pick a date, without the context of price and availability. Then later, we'll give them the extra context that they'll need to choose the right flight.

#### Date input

Before HTML5, we had to build our own date picker using Javascript. We know this is hard because we had to consider the host of requirements needed to produce a fully inclusive autocomplete component just earlier.

Mobile browsers that support HTML5&mdash;nowadays, that's most&mdash;have `input type="date"`. This is useful for many reasons:

- We don't need to spend time designing and developing our own.
- They are performant because they are provided by the browser.
- They are familiar because every website (and native app) will use the same interface.
- They are accessible by default.
- When browsers release improvements, users receive them immediately.
- All of which, frees us to solve other problems.

Here's how it looks on mobile:

![Mobile native date control](./images/mobile-date.png)

Desktop browser support is not as good. Chrome and Edge work well but Firefox, for example, doesn't have any support, although it's on the way at the time of writing.

![Desktop native date control](./images/desktop-date.png)

If you're concerned about it looking different across browsers, don't be. User's don't notice, and for those that do, they don't care. In Progressive Enhancement 2.0[^], Nicholas Zakas demonstrates this to the audience at X mins in.

Nobody cares about your website as much as you do. But if you're still not convinced take a look at this dedicated website Do Websites Needs To Look The Same In Every Browser[^] just to be sure.

We'll deal with browsers that don't suppor the date input in a moment. But for now, let's start implementing the field.

HTML:

```HTML
<div class="field">
	<label for="date">Flight date</label>
	<input type="date" name="date" id="date">
</div>
```

```CSS
::-webkit-datetime-edit
::-webkit-datetime-edit-fields-wrapper
::-webkit-datetime-edit-text
::-webkit-datetime-edit-month-field
::-webkit-datetime-edit-day-field
::-webkit-datetime-edit-year-field
::-webkit-inner-spin-button
::-webkit-calendar-picker-indicator
```

We can use these pseudo selectors to style these things where we think it helps and tweak the styles and turn off the "spinners".

This works really well but we do need to think about browsers that lack support.

#### Date input unsupported

Browsers that don't support the date input will degrade gracefully into a simple text box, which may be sufficient. This is one of the advantages of Progressive Enhancement. We can choose to degrade gracefully, or, we can decide to provide a more enhanced fallback.

As choosing dates is integral to booking a flight online, the degraded solution isn't really that acceptable. We'll need to design our own date picker when the date input isn't supported.

First, we need to detect a lack of support:

```Javascript
function supportsDateInput() {
	var el = document.createElement('input')
	el.type = "date"
	return el.type == "date";
}

if(!supportsDateInput()) {
	// code to create and use a custom calendar widget
}
```

In future we may find that:

- research shows that our own widget performs better than the native input
- support is broad enough to worry less about the rapidly diminishing users using unsupported browsers.

In both cases we may choose to remove the custom date picker, giving us less to maintain, and give users faster experiences.

#### Date picker component

How it might look:

![Date widget](./images/date-widget.png)

A run down of the design and behaviour:

- Use big buttons for good tap targets. Fine motor skills.

- When displaying the calendar, display it inline below the text input. Overlays are complicated beasts that obscure parts of the screen, so we'll want to avoid those and keep things simple. Also on mobile, a dialog is a bit of an anti-pattern as the estate available to the overlay is often less than utilising the main page itself.

- Hitting the previous month should show the previous month and select the first day of that month. Same for hitting next month.

- Some date pickers will allow users to jump to the next and previous years. For most airlines though, they only offer flights within the next 12 months. Following the lead here, there is no need to provide this functionality.

- Once focus is set on the grid, the user can use arrow keys to move about to select a date. We avoid making the cells part of the tab sequence as it's too much for users to wade through.

- When focussed within the Date Picker, pressing `escape` should hide it.

- Pressing enter/space should select the date and close the picker.

- Make back/previous buttons to be naturally focusable.

HTML:

```HTML
<div>
	<label for="date">Flight date</label>
	<div class="datepicker">
		<input type="date" name="date" id="date">
		<button type="button">Choose date</button>
		<div class="datepicker-calendar">
			// html here
		</div>
	</div>
</div>
```

In addition to the `input` there is a `button` which toggles the calendar's visibility, and of course the calendar itself. Both are injected with Javascript, as they only work when Javascript is available.

We use a button (with type `button`) as opposed to a link because it's not navigation. The type button ensures the button doesn't submit the form.

Even our calendar widget is screen reader friendly, it's probably not useful to them[^calendarscreenreader]. But we don't assume. Visual users may benefit from it also. One important aspect about inclusive design is giving users choice. We don't assume. Please just a handful of people we may not know about is good practice. Remember we write the component once, and we can reuse it many times over.

Here's a run down of the attributes:

- The button has a type of `button` so that it doesn't submit the form. It's responsible for showing the calendar. It's specifically not a link, as it doesn't navigate.

- `aria-hidden` tells readers whether it's showing or not.

- The table has a `role="grid"` so that screen readers know how to navigate this element i.e. with up, down, left and right arrows.

- The table also has a `aria-labelledby` matching the ID of the element with the visual decription. In the illustration, "April 2017".

- Each week row has a `role="row"` so that screen readers can navigate.

- Each day cell has a `role="gridcell"` for the same reason. They also have `aria-label` to read out the full date, as the number is only enough visually. Much like the autocomplete component each day is like an option and so needs an ID to tie in with `aria-activedescendant` on the table itself. Same goes for `aria-selected`.

- Each column heading has a `role="columnheading"`. Each day is abbreviated for space and convention as we need this to work nicely on small screens. Using the `abbr` element allows the abbreviation to be expanded upon in supporting browsers and assistive technology.

(((In 200X, somebody wrote [Abbr hatrick](https://alistapart.com/article/hattrick), stating that whilst not many browsers support it, eventually they will. The prediction came true. And it's the same approach we can take more broadly.)))

- Live region? Do I need?

Here's the Javascript:

```JS
Do I put all the JS in? It's complex.
```

#### The final trade off

In cases where:

- user hasn't got JS; and
- there is no support for date input

Users are left to type into a textbox lacking instructions. We can't put in a placeholder that says type "dd/mm/yyyy" because it will show when the browser *does* support the native date input and when the user will be assisted.

We are left to rely on error messaging. The best error messages are those that don't appear, but in this case we can't get around it, unless we make the optimisitic experience worse. That is we add a placeholder that makes dealing with the native date input confusing.

We hope that we have endeavoured to provide the best experience where possible with the best degraded experience where necessary. In this case there is nothing more we can do, and I think that's okay.

With design there is always a tradeoff. I consider myself to care about everyone, but when you design for everyone you may end up designing for noone. For example, someone who considers themself an "intellect" may love reading complex high brow paragraphs of text, but we know that hemmingway says we should write for grade 6 or less if possible because it's easy to read for everyone. Can't please em all.

## Choosing passengers

Next, users must choose how many people are travelling. Typically, passengers fall under three categories:

- Adults: aged 16 and over.
- Children, aged between 2 and 15 years old
- Infants, who are under 2 years old

For each category we'll want a form field that uses the hint pattern from chapter 1, *Register*. Here's How it might look:

![Passengers](./images/choose-passengers.png)

HTML:

```HTML
<div>
    <label for="adult-passengers">
    	<span class="label">How many adults are flying?</span>
    	<span class="hint">Aged 16 years and over</span>
    </label>
    <input type="number" id="adult-passengers" name="adult-passengers">
</div>
<div>
    <label for="adult-children">
    	<span class="label">How many children are flying?</span>
    	<span class="hint">Aged between 2 and 15 years old</span>
    </label>
    <input type="number" id="adult-children" name="adult-children">
</div>
<div>
    <label for="adult-infants">
    	<span class="label">How many adults are flying?</span>
    	<span class="hint">Under 2 years old</span>
    </label>
    <input type="number" id="adult-infants" name="adult-infants">
</div>
```

TODO: maxlength=9?

We already discussed the benefits of using a number field in chapter 2, *Checkout*, but we'll want to make additional improvements

It's the type of form field that is the most interesting part of this discussion. Websites once again used the `select` box containing options one through nine. We'll be avoiding that of course and we'll use the number field, which we already discussed at length in *Checkout*.

In *Checkout*, we turned off the native increment and decrement buttons because they look a little odd and they are hard to click. Having increment buttons was unnecessary in that case.

However, in this case, selecting the amount of passengers is a vital aspect of booking flights. It's often easier for many users to tap an increment button a couple times, as opposed to tapping on the field and typing in a digit.

For this reason we'll create our own using Javascript. After the Javascript executes the HTML will look like this:

```HTML
<div>
    <label for="adult-passengers">
    	<span class="label">How many adults are flying?</span>
    	<span class="hint">Aged 16 years and over</span>
    </label>
    <button type="button" tabindex="-1" aria-label="Decrement">-</button>
    <input type="number" id="adult-passengers" name="adult-passengers">
    <button type="button" tabindex="-1" aria-label="Increment">+</button>
</div>
<div>
    <label for="adult-children">
    	<span class="label">How many children are flying?</span>
    	<span class="hint">Aged between 2 and 15 years old</span>
    </label>
    <button type="button" tabindex="-1" aria-label="Decrement">-</button>
    <input type="number" id="adult-children" name="adult-children">
</div>
<div>
    <label for="adult-infants">
    	<span class="label">How many adults are flying?</span>
    	<span class="hint">Under 2 years old</span>
    </label>
    <button type="button" tabindex="-1" aria-label="Decrement">-</button>
    <input type="number" id="adult-infants" name="adult-infants">
    <button type="button" tabindex="-1" aria-label="Increment">+</button>
</div>
```

```JS
function InputNumberButtons(input) {
	this.input = input;
	this.createDecrementButton();
	this.createIncrementButton();
}

InputNumberButtons.prototype.createDecrementButton = function() {
	// create button
	// listen to event
};

InputNumberButtons.prototype.createIncrementButton = function() {
	// create button
	// listen to event
};

InputNumberButtons.prototype.increment = function() {
	// get value
	// parseInt
	// plus one
	// set input value
};

InputNumberButtons.prototype.decrement = function() {
	// get value
	// parseInt
	// minus one
	// set input value
};
```

TODO?: LUKEW steppers: https://www.lukew.com/ff/entry.asp?1950

## Confirming a flight

If there are flights matching the users preferences, the page will display a list of flights represented by radio buttons. Radio buttons are perfectly suited here, because we can fit price, time and any other pertinent information inside the related label.

![Image](./images/image.png)

Clicking *continue*, saves their flight time and sends them to the next step.

## Choosing a seat

Next, the user needs to choose a seat. If more than one passenger is travelling, they'll be able to select more than one seat. In this case we'll want to use checkboxes to represent each seat.

In a similar vein to picking dates, it makes sense to pick a seat&mdash;visually at least&mdash;in the context of where that seat is located. As a user I want to know where the seat is located so that I can choose an appropriate seat for me and my wife, for example.

Up to now, we have just presented form fields in a simple manner. In this case, however, we can help visual users in particular by presenting the checkboxes as if they were seats on a plane. That is, laid in in rows.

How it might look:

![Image](./images/image.png)

HTML:

```HTML
<fieldset>
	<legend>
		Choose 2 seats
	</legend>
	<fieldset>
		<legend>First class</legend>
		<div class="row">
			<div class="seat">
				<input type="checkbox" name="seat" id="1A">
				<label for="1A">1A</label>
			</div>
			<div class="seat">
				<input type="checkbox" name="seat" id="1B">
				<label for="1B">1B</label>
			</div>
			<div class="seat">
				<input type="checkbox" name="seat" id="1C">
				<label for="1C">1C</label>
			</div>
			<div class="seat">
				<input type="checkbox" name="seat" id="1D">
				<label for="1D">1D</label>
			</div>
		</div>
		<div class="row">
			...
		</div>
	</fieldset>
	<fieldset>
		<legend>Economy class</legend>
		<div class="row">
			<div class="seat">
				<input type="checkbox" name="seat" id="9A">
				<label for="9A">9A</label>
			</div>
			<div class="seat">
				<input type="checkbox" name="seat" id="9B">
				<label for="9B">9B</label>
			</div>
			<div class="seat">
				<input type="checkbox" name="seat" id="9C">
				<label for="9C">9C</label>
			</div>
			<div class="seat">
				<input type="checkbox" name="seat" id="9D">
				<label for="9D">9D</label>
			</div>
			<div class="seat">
				<input type="checkbox" name="seat" id="9E">
				<label for="9E">9E</label>
			</div>
			<div class="seat">
				<input type="checkbox" name="seat" id="9F">
				<label for="9F">9F</label>
			</div>
		</div>
		<div class="row">
			...
		</div>
	</fieldset>
</fieldset>
```

Everything here should look familiar as we used the same constructs for radio buttons before. The only difference is the nested fieldset, which we'll discuss next.

## Nested fieldsets

It may not be obvious, but the preferences we have collected from users so far has subtly influenced the interface we have given our users. Specifically, we didn't ask the user whether they wanted to travel first class or economy, which reduced friction upfront.

The downside is that we now have to present both categorisations on a single screen. To expose this categorisation both visually and semantically, we need a nested fieldset.

The outermost fieldset represents the overarching question *What seat do you want?* and the inner fieldset represents the categorisation: either first class or economy.

It would be prudent to ask users this quesion upfront, so that we can reduce the complexity of the experience, and HTML alike. This is particularly useful for screen reader or keyboard users. But really it helps all users, by increasing speed of the page and finally, reducing options for users.

In anycase, most users will choose economy meaning we can use a smart default here. The tiny bit of added friction upfront, is a small price to pay for the reduced complexity during this step. As with everything though, test the experience with users.

Here's what the simplified version looks like:

![Image](./images/image.png)

HTML:

```HTML
<fieldset>
	<legend>
		Choose 2 seats
	</legend>
	<div class="row">
		<div class="seat">
			<input type="checkbox" name="seat" id="1A">
			<label for="1A">1A</label>
		</div>
		<div class="seat">
			<input type="checkbox" name="seat" id="1B">
			<label for="1B">1B</label>
		</div>
		<div class="seat">
			<input type="checkbox" name="seat" id="a3">
			<label for="a3">A3</label>
		</div>
		...
	</div>
	<div class="row">
		...
	</div>
</fieldset>
```

This looks good as is and I would encourage you to test this with users before adding style enhancements. But my spidey sense tells me that we can improve the experience by hiding the checkboxes and styling the labels to look like seats in a plane.

This saves space giving our design a better chance of working on smaller screens, *without* needing a horizontal scroll bar or having the seats wrap. It should also help to show those seats that are in the isle and those that are by the window.

The important thing to note here though, is that we shouldn't hide the labels unless Javascript is available and capable to manage focus. Without this, keyboard users won't know where the cursor is.

To fix this, we can listen to the focus event of each checkbox, and when it fires, we add a highlight to the label. When it blurs, remove the highlight. We don't need to do anything for assistive devices here because they will use the form like normal.

```CSS
https://codepen.io/siiron/pen/MYXZWg
```

```JS
function CheckboxLabelHighlighter(checkboxes) {
	checkboxes.on('focus', $.proxy(this, 'onCheckboxFocus'));
	checkboxes.on('blur', $.proxy(this, 'onCheckboxBlur'));
}

CheckboxLabelHighlighter.prototype.onCheckboxFocus = function(e) {
	$(e.target).addClass('seat-isFocussed');
}

CheckboxLabelHighlighter.prototype.onCheckboxBlur = function(e) {
	$(e.target).removeClass('seat-isFocussed');
}
```

## Limiting seat selection

We purposely asked users how many passengers are flying. Much like the previous point, this has also influenced our interface, and perhaps unnecessarily so.

As we asked how many passengers there are, it seems we are forced to constrain the selection of seats to that value. Otherwise why did we ask for that information in the first place?

Perhaps we don't need to ask it up front. Perhaps we can just let users select as many seats (up to the maximum), and then answer questions about those passengers (like age) later on.

Aha, that's not going to work though. Having talked this through with this sheet of paper, I realise why we're asking for that information upfront. It's because the age of the passenger influences the price of the ticket. The price of the ticket influences the choice the user makes.

Unfortunately, this time we'll have to leave it in. But as always, it's vital we know why.

If there is just one passenger travelling, we should change the inputs to radio buttons. This way, the affordance itself tells users they can only choose one seat.

To crystalise this information for users, we should include this information a hint explaining they can only select X users.

If Javascript is available and the browser is capable, we can enhance the experience so that users don't experience an error. That is, *You can only select 2 seats*. We can do this by disabling all seats as soon as the maximum amount have been selected.

Here's what that script looks like:

```JS
```

## Summary

TODO: Summary

## Footnotes

[^luke]:(http://www.lukew.com/ff/entry.asp?1950)
[^]:(https://www.nngroup.com/articles/drop-down-menus-use-sparingly/)
[^]:(https://www.slideshare.net/cjforms/design-patterns-in-government-2016)
[^buggy]:(http://caniuse.com/#feat=datalist)
[^GDS type]:(https://alphagov.github.io/accessible-typeahead/)
[^leonie]:(http://ljwatson.github.io/design-patterns/autocomplete/)
[^ppk]:(https://medium.com/samsung-internet-dev/making-input-type-date-complicated-a544fd27c45a)
[^exampledatepickers]:(http://www.webaxe.org/accessible-date-pickers/)
[^dateshard]:(http://infiniteundo.com/post/25326999628/falsehoods-programmers-believe-about-time)
[^calendarscreenreader]:(https://ux.stackexchange.com/questions/60884/best-way-for-date-field-for-visually-impaired-users)
[^calendarw3ariaspec]:(https://www.w3.org/TR/2009/WD-wai-aria-practices-20090224/)
[alice bartlett steve faulkner]:https://www.paciellogroup.com/blog/2014/09/web-components-punch-list/
