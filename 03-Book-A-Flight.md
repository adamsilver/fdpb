# Book A Flight

In this chapter, we'll design a service that allows users to book a flight. At first this seems a bit *niche*, especially when compared to the two previous chapters, *A Registration Form* and *Checkout*. However, this chapter encourages the exploration of several more complex patterns. Patterns that are very much transferable to other problem domains such as booking a cinema ticket of hotel room.

Here are the main steps in the flow:

1. Choose origin (and destination)
2. Choose departure (and return) date
3. Choose passengers
4. Choose flight
5. Choose seat

After step 5, the user will go to pay. I've left it out as we covered payment in the previous chapter.

## 1. Choose origin (and destination)

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

There are 3 component parts:

- A text box
- A menu to show suggestions
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

The first thing this does is to replace the select box with a text box, down arrow button, suggestions panel and status box. Then as the user types it shows suggestions to the user.

Notes:

- Pressing <kbd>up</kbd> or <kbd>down</kbd> moves focus to the option.
- Pressing <kbd>enter</kbd>, <kbd>space</kbd> or clicking the option populates the text box and hides the suggestions.
- Pressing <kbd>enter</kbd> when focus is within the text box submits the form (like normal).
- Clicking the down arrow button, reveals all the possible options.
- Pressing <kbd>escape</kbd> hides the options.

## 2. Choose departure (and return) date

Dates are hard[^]. Different time zones, formats, delimitters, days in the month, length of a year, day light savings and on and on. It's hard work designing all of this complexity out of an interface.

Traditionally we've used 3 select boxes to capture dates&mdash;one for day, month and year. One of the redeeming qualities of select boxes is that they stop users entering wrong information. In the case of dates, even *this* doesn't hold up. This is because in combination a user can, for example, choose *31 February 2017* which is, of course, invalid.

![Select boxes for dates](./images/date-select.png)
[https://www.gov.uk/state-pension-age/y/age]

The remaining reason to use select boxes is to avoid the problem of formats. Some dates start with month, others with day. Some delimit dates with slashes, others with dashes. We can't determine the intention accurately. In turn this means we can't be forgiving as we would otherwise like to be. We can do better than select boxes.

But before we get to that, let's take a step back. Before we can design an optimal date field, we'll need to first understand what type of date we're expecting users to enter. Goverment Digital Services (GDS) talks about this in the service manual[^]. It says *the way you should ask for dates depends on the types of date you’re asking for*.

Let's now discuss the 3 types of dates and see which one best suits our use case.

### Dates from documents

GDS says *if you ask for a date exactly as it’s shown on a passport, credit card or similar item, make the fields match the format of the original. This will make it easier for users to copy it across accurately.*

This is exactly the approach we took for expiry date, in chapter 2. We gave users a text box that expects a number matching that found on their card. Users simply copy what they see.

### Memorable dates

Dates that are easy to remember, such as date of birth, need a different tact. The defacto thinking is that date pickers are always better than manually typing numbers. But this is not true. It's often slower and harder to enter a memorable date in this manner, than it would be to type numbers into a text box unassisted.

GDS's research shows that 3 *separate* text boxes works best&mdash;one for day, month and year. Why 3 boxes? Because it solves the formatting issues we discussed before.

How it might look:

![GDS date of birth](./images/gds-dob.png)

HTML:

```HTML
<div class="field">
	<fieldset>
		<legend>
			<span class="field-legend">Date of birth</span>
			<span class="field-hint">DD MM YYYY</span>
		</legend>
		<div class="field-dayWrapper">
			<label for="day">Day</label>
			<input class="field-dayBox" type="number" pattern="[0-9]*" name="day" id="day" min="0" max="31">
		</div>
		<div class="field-monthWrapper">
			<label for="month">Month</label>
			<input class="field-monthBox" type="number" pattern="[0-9]*" name="month" id="month" min="0" max="12">
		</div>
		<div class="field-yearWrapper">
			<label for="year">Year</label>
			<input class="field-yearBox" type="number" pattern="[0-9]*" name="year" id="year" min="0" max="2050">
		</div>
	</fieldset>
</div>
```

This field uses the `fieldset` and `legend` elements to group the 3 inputs together. Like any other input that uses the fieldset, it provides context for the labels within. In this case, without ‘Date of birth’, ‘Day’, ‘Month’ and ‘Year’ would be ambiguous.

Some versions of iOS won't show the numeric keyboard, even if you use `type="number"`. The `pattern` attribute fixes this problem[^filament] which is why we've included it in each of the inputs above.

#### Auto-tabbing

Some websites automatically move focus from one box to the other, once the correct *amount* of characters have been typed. This is a usability failure, which we'll discuss in the next chapter.

### Finding dates in a calendar

In the case of booking a flight users are niether dealing with a memorable date nor one found in a document. When booking flights, we often orientate ourselves around day and week. To mimic this, we'll need a more convenient solution than three text boxes. We need to use a date picker.

Interfaces that try to solve too many problems at once cause problems. If we try to convey, for example, price and availability, inside a calendar, this would result in a busy interface that overloads the user. That is, unless the user has a super-sized screen, but not everyone has one of those.

We'll use the One Thing Per Page technique by first letting users choose a date, without the context of price and availability. Then later, we'll give them the extra context they need to choose the right flight for them.

#### Date input

Before HTML5, we always had to build our own date picker using Javascript. We know this is an complex endeavour  because we had to consider the host of requirements needed to create an inclusive autocomplete component above.

Mobile browsers that support HTML5&mdash;nowadays, that's most&mdash;have `input type="date"`. This is useful because:

- We don't need to spend time designing and developing our own component.
- They are performant because they are provided by the browser.
- They are familiar because every website (and native app for that matter) will use the same interface.
- They are accessible by default.
- When browsers release improvements, users get them quickly.

All of these reasons make time and headspace to solve other more pressing problems. The overused phrase is *don't reinvent the wheel*.

Here's how it looks on mobile:

![Mobile native date control](./images/mobile-date.png)

Desktop browser support is not as good. Chrome and Edge work well but Firefox, for example, doesn't have any support at time of writing.

![Desktop native date control](./images/desktop-date.png)

If you're concerned about it looking different in different browsers you'd needn't be. In Progressive Enhancement 2.0[^], Nicholas Zakas proves that users don't notice the differences between browser implementations. Nobody cares about your website as much as you do and rather officially websites do not need to look the same in every browser[^].

HTML:

```HTML
<div class="field">
	<label for="departureDate">
		<span class="field-label">Departure date</span>
	</label>
	<input class="field-textBox" type="date" id="departureDate" name="departureDate">
</div>

```

#### What about browsers that lacking support?

Browsers that don't support date inputs will degrade into a text box, which may be sufficient. This is one of the many advantages of Progressive Enhancement. We can choose to degrade gracefully. Or, we can decide to provide a more enhanced fallback.

As choosing dates is integral to booking a flight online, the degraded solution isn't really as good as we'd like. Choosing a date should be as easy as possible for as many people as possible who are using the flight booking service. Perhaps this will change in the future as support for the native input gets better. But more on this shortly.

For now, we'll create a date picker component. The first problem though, is that we don't want browsers who support the native input to show users the custom component. In which case, we'll need to detect support:

```Javascript
function supportsDateInput() {
	var el = document.createElement('input');
	try {
		el.type = "date";
	} catch(e) {}
	return el.type == "date";
}

if(!supportsDateInput()) {
	// use custom component
}
```

#### What about the future?

Jeremy Keith often talks about the web being a *continuum*. This means the web is constantly changing. New browsers come out all the time, all of them have different features and capabilities. At any particular moment in time, we need to think about what level of support or optimisation makes sense for a given feature.

As we said earlier, desktop browser support for the native input is lacking. And as choosing a date is so integral to the flow, we've decided to plug the gap. At least for now. In future, things may change, and support is likely to improve drastically at which point we can change tacts.

We may choose not to support the rapidly diminishing number of users that would otherwise experience the degraded text box. Pracitically speaking this means removing all of the Javascript, giving users a faster experience and ourselves less to maintain.

#### Date picker component

Having now detected support, we'll need to design the date picker component itself.

How it might look:

![Date widget](./images/date-widget.png)

Notes:

- Buttons should have large tap targets making it easy to press with a finger or mouse.
- The calendar displays beneath the input. Dialogs obscure the interface and on small screens it takes up the entire screen anyway.

HTML:

```HTML
<div class="field">
	<label for="departureDate">
		<span class="field-label">Departure date</span>
	</label>
	<div class="datepicker">
		<input class="field-textBox" type="text" id="departureDate" name="departureDate" value="">
		<button class="datepicker-toggleButton" type="button" aria-expanded="true">Choose</button>
		<div class="datepicker-wrapper" aria-hidden="false">
			<div class="datepicker-calendar">
				<div class="datepicker-actions">
					<button aria-label="Previous month" type="button" class="datepicker-back">&lt;</button>
					<div role="status" aria-live="polite" aria-atomic="true" class="datepicker-title">July 2017</div>
					<button aria-label="Next month" type="button" class="datepicker-next">&gt;</button>
				</div>
				<table role="grid">
					<thead>
						<tr>
							<th><abbr title="Sunday">Sun</abbr></th>
							<th><abbr title="Monday">Mon</abbr></th>
							<th><abbr title="Tuesday">Tue</abbr></th>
							<th><abbr title="Wednesday">Wed</abbr></th>
							<th><abbr title="Thursday">Thu</abbr></th>
							<th><abbr title="Friday">Fri</abbr></th>
							<th><abbr title="Saturday">Sat</abbr></th>
						</tr>
					</thead>
					<tbody>
						<tr>
							<td class="calendarControl-previousMonthDay">25</td>
							<td class="calendarControl-previousMonthDay">26</td>
							<td class="calendarControl-previousMonthDay">27</td>
							<td class="calendarControl-previousMonthDay">28</td>
							<td class="calendarControl-previousMonthDay">29</td>
							<td class="calendarControl-previousMonthDay">30</td>
							<td tabindex="-1" aria-selected="false" aria-label="1 July, 2017" data-date="Sat Jul 01 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_1" class="datepicker-day">1</td>
						</tr>
						<tr>
							<td tabindex="-1" aria-selected="false" aria-label="2 July, 2017" data-date="Sun Jul 02 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_2" class="datepicker-day">2</td>
							<td tabindex="-1" aria-selected="false" aria-label="3 July, 2017" data-date="Mon Jul 03 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_3" class="datepicker-day">3</td>
							<td tabindex="-1" aria-selected="false" aria-label="4 July, 2017" data-date="Tue Jul 04 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_4" class="datepicker-day">4</td>
							<td tabindex="-1" aria-selected="false" aria-label="5 July, 2017" data-date="Wed Jul 05 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_5" class="datepicker-day">5</td>
							<td tabindex="-1" aria-selected="false" aria-label="6 July, 2017" data-date="Thu Jul 06 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_6" class="datepicker-day">6</td>
							<td tabindex="-1" aria-selected="false" aria-label="7 July, 2017" data-date="Fri Jul 07 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_7" class="datepicker-day">7</td>
							<td tabindex="-1" aria-selected="false" aria-label="8 July, 2017" data-date="Sat Jul 08 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_8" class="datepicker-day">8</td>
						</tr>
						<tr>
							<td tabindex="-1" aria-selected="false" aria-label="9 July, 2017" data-date="Sun Jul 09 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_9" class="datepicker-day">9</td>
							<td tabindex="-1" aria-selected="false" aria-label="10 July, 2017" data-date="Mon Jul 10 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_10" class="datepicker-day">10</td>
							<td tabindex="-1" aria-selected="false" aria-label="11 July, 2017" data-date="Tue Jul 11 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_11" class="datepicker-day">11</td>
							<td tabindex="-1" aria-selected="false" aria-label="12 July, 2017" data-date="Wed Jul 12 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_12" class="datepicker-day">12</td>
							<td tabindex="-1" aria-selected="false" aria-label="13 July, 2017" data-date="Thu Jul 13 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_13" class="datepicker-day">13</td>
							<td tabindex="-1" aria-selected="false" aria-label="14 July, 2017" data-date="Fri Jul 14 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_14" class="datepicker-day">14</td>
							<td tabindex="-1" aria-selected="false" aria-label="15 July, 2017" data-date="Sat Jul 15 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_15" class="datepicker-day">15</td>
						</tr>
						<tr>
							<td tabindex="-1" aria-selected="false" aria-label="16 July, 2017" data-date="Sun Jul 16 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_16" class="datepicker-day">16</td>
							<td tabindex="-1" aria-selected="false" aria-label="17 July, 2017" data-date="Mon Jul 17 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_17" class="datepicker-day">17</td>
							<td tabindex="0" aria-selected="true" aria-label="18 July, 2017" data-date="Tue Jul 18 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_18" class="datepicker-day datepicker-day-isToday datepicker-day-isSelected">18</td>
							<td tabindex="-1" aria-selected="false" aria-label="19 July, 2017" data-date="Wed Jul 19 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_19" class="datepicker-day">19</td>
							<td tabindex="-1" aria-selected="false" aria-label="20 July, 2017" data-date="Thu Jul 20 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_20" class="datepicker-day">20</td>
							<td tabindex="-1" aria-selected="false" aria-label="21 July, 2017" data-date="Fri Jul 21 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_21" class="datepicker-day">21</td>
							<td tabindex="-1" aria-selected="false" aria-label="22 July, 2017" data-date="Sat Jul 22 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_22" class="datepicker-day">22</td>
						</tr>
						<tr>
							<td tabindex="-1" aria-selected="false" aria-label="23 July, 2017" data-date="Sun Jul 23 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_23" class="datepicker-day">23</td>
							<td tabindex="-1" aria-selected="false" aria-label="24 July, 2017" data-date="Mon Jul 24 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_24" class="datepicker-day">24</td>
							<td tabindex="-1" aria-selected="false" aria-label="25 July, 2017" data-date="Tue Jul 25 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_25" class="datepicker-day">25</td>
							<td tabindex="-1" aria-selected="false" aria-label="26 July, 2017" data-date="Wed Jul 26 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_26" class="datepicker-day">26</td>
							<td tabindex="-1" aria-selected="false" aria-label="27 July, 2017" data-date="Thu Jul 27 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_27" class="datepicker-day">27</td>
							<td tabindex="-1" aria-selected="false" aria-label="28 July, 2017" data-date="Fri Jul 28 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_28" class="datepicker-day">28</td>
							<td tabindex="-1" aria-selected="false" aria-label="29 July, 2017" data-date="Sat Jul 29 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_29" class="datepicker-day">29</td>
						</tr>
						<tr>
							<td tabindex="-1" aria-selected="false" aria-label="30 July, 2017" data-date="Sun Jul 30 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_30" class="datepicker-day">30</td>
							<td tabindex="-1" aria-selected="false" aria-label="31 July, 2017" data-date="Mon Jul 31 2017 00:00:00 GMT+0100 (BST)" id="tdate_day_31" class="datepicker-day">31</td>
							<td class="calendarControl-nextMonthDay">1</td>
							<td class="calendarControl-nextMonthDay">2</td>
							<td class="calendarControl-nextMonthDay">3</td>
							<td class="calendarControl-nextMonthDay">4</td>
							<td class="calendarControl-nextMonthDay">5</td>
						</tr>
					</tbody>
				</table>
			</div>
		</div>
	</div>
</div>
```

There are 5 component parts:

- Text box
- Toggle button
- Calendar header
- Calendar grid
- Day cell

Text box notes:

- This is a standard text box. The user can type directly into it if they wish, or they can use the date picker. Using the date picker populates the text box automatically.

Toggle button notes:

- `type="button"` ensures the form is not submitted.
- `aria-expanded="false"` announces to screen reader users if the calendar is showing or not. Clicking the button sets the attribute to `true` and displays the calendar.

Calendar header notes:

- The heading is inside a live region. When the user navigates between months, the new month and year is announced accordingly.
- The header also houses the previous and next month buttons.

Calendar grid notes:

- `aria-hidden="true"` ensures the calendar is not perceivable to screen readers. For visual users, this is achieved by the fact it's not visible.
- The table has `role="grid"` so that the screen reader treats the table as a special grid widget. In JAWS, for example, this means the arrow keys can be used to navigate between days. More on this shortly.
- The thead contains the column headings representing each day of the week. The day is abbreviated and uses the `abbr` element which saves space but gives assistive technology the change to announce the day of the week in full.
- The tbody contains the days organised in weeks which are represented in table rows.

Calendar cell notes:

- `tabindex="-1" allows us to set focus programmatically. More on this shortly. The selected day has `tabindex="0"` so that it appears in the standard tab sequence.
- ????`aria-selected` allows the selected state to be announced. This is set to true when the cell is selected.
- ???? ID
- `aria-label` enables screen readers to announce the full day. Without it, the cell's value is ambiguous.

Javascript:

```JS
Put code here
```

Notes:

- Pressing *choose* toggles the calendar's visibility.
- Pressing *previous* shows the previous month and selects the first day of the preview month.
- Pressing *next* shows the next month and selects the first day of the next month.
- Once focus is on the grid, the arrow keys let the user move freely between days and weeks.
- Only the selected day is in the natural tab sequence. This is because for those relying on the <kbd>tab</kbd> key would have to tab ~30 times in order to leave the calendar and would it make it hard to move focus to the selected day.
- Pressing <kbd>escape</kbd> hides the date picker and moves focus to the button.
- Pressing <kbd>enter</kbd> or <kbd>space</kbd> populates the date into the text box, hides the calendar and moves focus into the text box.

Whilst screen reader users *can* operate the calendar, it's not especially useful to them. Entering a date by typing directly into the text box is probably easier and quicker. In any case, we don't assume they won't use it. Instead, we adhere to inclusive principle number X, by giving users choice.

#### A small trade off

Most designers would stop here. After all that's everyone covered right? Not quite. There are still users that don't use a browser that supports the native input; and that, for some reason Javascript isn't available[^]. Those falling under this category, allbeit an edge case, will have to type a date into a text box unassisted.

The problem is that the field lacks instructions regarding the expected format. We can't explain this with a hint because it will show to those using a browser that *does* support the date input. This is problematic, because the format is different.

![]()

We can mitigate the risk by forgiving users for delimitting the date with slashes, periods and spaces. But typing the year first, for example, *will* cause an error. A well written error message will have to handle this case. Fortunatley, this shouldn't happen too often.

This is one of those occasions whereby a good solution for most, is less than ideal for a small number of people. This is a trade off that we have to make sometimes.

## 3. Choose passengers

Next, users choose how many people are travelling and under which category:

- Adults (aged 16 and over)
- Children (aged between 2 and 15 years old)
- Infants (who are under 2 years old)

How it might look:

![Passengers](./images/choose-passengers.png)

HTML:

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

In chapter 2 we disabled the spinner. Spinners, also known as steppers, let users increase or decrease the value by a constant amount. They are great for making small adjustments. Luke Wobrelkski says, in the same article:

> When testing mobile flight booking forms, we found people preferred steppers for selecting the number of passengers. No dropdown menu required, especially since there's a maximum of 8 travelers allowed and the vast majority select 1-2 travelers.

The problem with the native spinner is that it's small and hard to use. We can enhance the interface by injecting large buttons next to the input.

How it might look:

[!]()

```HTML
<div class="field">
    <label for="adults">
    	<span class="field-label">How many adults, 16 years and over, are flying?</span>
    </label>
    <button type="button" tabindex="-1" aria-label="Decrement">-</button>
    <input type="number" id="adults" name="adults">
    <button type="button" tabindex="-1" aria-label="Increment">+</button>
</div>
<div class="field">
    <label for="children">
    	<span class="field-label">How many children, between 2 and 15 years old, are flying?</span>
    </label>
    <button type="button" tabindex="-1" aria-label="Decrement">-</button>
    <input type="number" id="children" name="children">
    <button type="button" tabindex="-1" aria-label="Increment">+</button>
</div>
<div class="field">
    <label for="infants">
    	<span class="field-label">How many infants, under 2 years old, are flying?</span>
    </label>
    <button type="button" tabindex="-1" aria-label="Decrement">-</button>
    <input type="number" id="infants" name="infants">
    <button type="button" tabindex="-1" aria-label="Increment">+</button>
</div>
```

The buttons are removed from the tab sequence using `tabindex="-1"`. This ensures we don't disturb the natural flow and order of form elements. The buttons use iconography and although the best icon is text, these icons are well understood and keep the interface clean. For those using screen readers, we add `aria-label="Increment"` so the interface makes sense audibly.

Here's the Javascript:

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

The script injects two buttons. The buttons mimic the behaviour of the native spinner by decreasing or increasing the input's value by 1.

## 4. Choose flight

Up to now, we've simply been storing the user's preferences in order to determine which flights to show them. Now we have all the information we need, the user can select a flight from the list of results.

How it might look:

![Image](./images/image.png)

HTML:

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

Notes:

- Each flight is represented as a radio button.
- The user can go back and forth between days using pagination.
- The date of the flight is part of the legend to give context to the radio buttons.
- Each radio button contains departure time, arrival time and price, giving users all the information they need in order to make an informed decision.

## 5. Choose seat

Next, the user needs to choose a seat. If more than one passenger is travelling, they'll be able to select more than one seat. In this case we'll want to use checkboxes to represent each seat.

As a user I want to know where the seat is located so that I can choose an appropriate seat for my wife and I based on our requirements and preferences.

Up to now, we've displayed form fields in a natural stacking manner. In this case, however, we can help sighted users by showing the checkboxes as if they were seats on a plane laid out in rows.

How it might look:

![Image](./images/image.png)

HTML:

```HTML
<fieldset>
	<legend>
		Choose 2 seats (WORDS)
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

ids starting with numbers?

### Breaking the age old rule

Each flight is represented as a radio button. The age old rule that we shouldn't show more than 7 radio buttons at a time is actually nonsense. As with many design problems, the answer is it depends. That rule was born out of putting so much in a single screen, that having a single question take up that much room may cause a problem.

In this case though, the entire screen is about choosing a flight. Hiding the choices behind a menu is both unnecessary and constraining. There is quite a lot of information to store inside the label. This is another benefit of using radio buttons as opposed to select boxes. We can format a number of elements inside the label giving us flexibility to indicate price and time differences.

Showing the options makes the flight times and prices easy to compare against themselves.

### Nested fieldsets

It may not be obvious, but the preferences we have collected from users so far has subtly influenced the interface we have given our users. Specifically, we didn't ask the user whether they wanted to travel first class or economy, which reduced friction upfront.

The downside is that we now have to present both categorisations on a single screen. To expose this categorisation both visually and semantically, we need a nested fieldset.

The outermost fieldset represents the overarching question *What seat do you want?* and the inner fieldset represents the categorisation: either first class or economy.

It would be prudent to ask users this quesion upfront, so that we can reduce the complexity of the experience, and HTML alike. This is particularly useful for screen reader and keyboard users. But really it helps all users, by increasing speed of the page and finally, reducing options for users.

In anycase, most users will choose economy meaning we can use a smart default here. The tiny bit of added friction upfront, is a small price to pay for the reduced complexity during this step. As with everything though, test the experience with users.

Here's how a simplified version might look:

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

If Javascript is available and the browser is capable, we can enhance the experience so that users don't experience an error. That is, *You can only select 2 seats*. We can do this by disabling all seats as soon as the maximum amount have been selected. (Is this good?).

Here's what that script looks like:

```JS
```

## Summary

TODO: Summary

## TODO

- In 200X, somebody wrote [Abbr hatrick](https://alistapart.com/article/hattrick), stating that whilst not many browsers support it, eventually they will. The prediction came true. And it's the same approach we can take more broadly.

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
