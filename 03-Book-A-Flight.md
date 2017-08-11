# Book A Flight

In this chapter, we'll design a flight booking service. At first this may seem a bit *niche*, especially when compared to the two previous chapters, *A Registration Form* and *Checkout*. However, this chapter encourages the exploration of several more complex patterns. Patterns that are very much transferable to other problem domains such as booking a cinema ticket or hotel room.

Here are the main steps in the flow:

1. Choose origin (and destination)
2. Choose departure (and return) date
3. Choose passengers
4. Choose flight
5. Choose seat
6. Payment (covered in chapter 2)

## 1. Choose origin (and destination)

First users have to choose an origin and destination. That is a place in which to fly from and to. Without knowing this information the service can't offer any useful flights. What's the best way of asking users for this information?

As designers, we should try and use the features that are native to the browser. This is because, generally speaking, they are are familiar (due to convention) and fully accessible. They also require far less work to design and build. Browsers offer 3 suitable controls. Let's look at each of them next.

### Select box

Designers often use `select` boxes because they save space. But interface design is about far more than just saving space. Select boxes are problematic because:

- some users find them hard to close.
- some users try to type into them.
- some users confuse focused options with selected ones.
- users aren't able to pinch-zoom options on devices.
- they have limited hierarchy control.
- they hide choices behind an unnecessary extra click.
- they are not easily searchable.

Usability expert, Luke Wobrelski, even goes as far to say that select boxes should be the ‘UI of Last Resort’[^1]. Unsurprisngly then, we'll search for a better way.

### Radio buttons

Radio buttons, unlike select boxes, are generally well-understood and easy-to-use. Radio buttons are exposed by defualt making them easier to discover and scan. They are also flexible because we can put formatted content inside the label. This is something we'll be looking at later in the chapter.

The problem with labels, however, is that they're not suitable when there are many of them. An airline could fly to hundreds of destinations. Using radio buttons create a long and unweildly interface, that the user is forced to peruse.

### Search box

A search box (`input type="search"`) is a standard text box with some extras. It lets users clear the contents of the field, either by tapping the x or pressing <kbd>escape</kbd>. With a regular text box you have to select the text and then press <kbd>delete</kbd>. Here's how it looks:

![Image here](/etc/)

This is a useful approach for searching large amounts of dynamic data such as searching for a product on Amazon[^2]. But airlines have a finite amount of destinations which we also know *in advance* of users searching. If we were to let users search unassisted like this, they could end up with no results due to typos.

### Autocomplete

What we really need is a control that let's users filter a long list of destinations quickly. We need the flexibility of a text box combined with the assistance of a select box. What need an autocomplete control. Autocomplete controls may also be known as *type ahead* or *predictive search* but we let's settle on *autocomplete* for communication purposes.

Autocomplete controls work by suggesting, in this case, destinations, as the user starts typing. As suggestions appear, the user selects one to *complete* automatically, hence the name. This saves users from having to scroll a long list and is forgiving if they make small typos.

HTML5 introduced the `datalist` element which when combined with a text box gives us the behaviour we want. Unfortunately, it's not ready for prime time due to its many bugs[^3]. It may be a reasonable option for you, if you're building something only to be consumed in a specific browser that you can control. But otherwise, you'll need to create a custom component.

Now we're about to enter new territory here. Designing custom components is a lot harder than using a native form control. To help guide us, accessibility expert, Steve Faulkner has what he calls a ‘punch list’[^4]. Essentially it's a list of rules which state that a custom component should:

- be focusable with the keyboard.
- be operable with the keyboard.
- work with assistive devices.
- work without Javascript.

To solve the last problem we need to talk about Progressive Enhancement. We briefly skimmed the approach in chapter 1 but it's worth talking about it a little more here. Progressive Enhancement is about giving everyone a core experience. Then, if possible and necessary, creating a better, ‘enhanced’ experience for those using a more capable browser.

In this case, the enhanced experience is the autocomplete itself, but we need to think about users who due to no fault of their own won't be able to get it. The core experience will have to be either a text box or a select box. On balance a select box seems more appropriate here as it stops users having to endure page refresh that delivers no results.

How it might look before enhancement:

![Image here](/etc/)

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

How it might look after enhancement:

![Image here](/etc/)

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

The enhanced autocomplete component is made from 3 parts. The text box lets users type where they want to go. The suggestions menu displays options matching what they type. Selecting one completes the field. The status box ensures that screen reader users know that the options have been filtered.

Text box notes:

- `role="combobox"` tells screen reader users that this is an autocomplete control, not just a regular text box.
- `aria-autocomplete="list"` explains that a list will appear from which the user can choose.
- `aria-expanded` indicates whether the menu is showing or not.
- `aria-owns="combobox-options"` connects the text box to the menu by `id`.
- `autocomplete="off"` stops browsers showing their own suggestions which will interfer with those offered by the component.

Option notes:

- `role="option"` announces the element as an option as the user navigates through the list.
- `aria-selected` indicates whether the item is selected or not.

Status box notes:

- `aria-role="status" announces the status. For example, *2 results available.*
- `aria-live="polite" ensures the announcement doesn't interrupt the user. It waits until the user stops typing.
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

The script replaces the select box with a text box and adds down arrow button, suggestions panel and status box. Then as the user types it shows suggestions to the user.

Notes:

- Pressing <kbd>up</kbd> or <kbd>down</kbd> moves focus to the option.
- Pressing <kbd>enter</kbd>, <kbd>space</kbd> or clicking the option populates the text box and hides the suggestions.
- Pressing <kbd>enter</kbd> when focus is within the text box implicitly submits the form (like normal).
- Clicking the down arrow button, reveals all the possible options.
- Pressing <kbd>escape</kbd> hides the options.

## 2. Choose departure (and return) date

Dates are hard[^5]. Different time zones, formats, delimitters, days in the month, length of a year, day light savings and on and on. It's hard work designing all of this complexity out of an interface.

Traditionally we've used 3 select boxes to capture dates&mdash;one for day, month and year. One of the redeeming qualities of select boxes is that they stop users entering wrong information. In the case of dates, even *this* doesn't hold up. This is because a user can, for example, select *31 February 2017* which is invalid.

![Select boxes for dates](./images/date-select.png)
[https://www.gov.uk/state-pension-age/y/age]

The remaining reason to use select boxes is to avoid the problem of formats. Some dates start with month, others with day. Some delimit dates with slashes, others with dashes. We can't determine the user's intent accurately. In turn this means we can't be forgiving, as we would otherwise like to be. We can do better than select boxes.

But before we get to that, let's take a step back. Before we can design an optimal date field, we'll need to first understand what type of date we're expecting users to enter. Goverment Digital Services (GDS) talks about this in the service manual[^6]. It says *the way you should ask for dates depends on the types of date you’re asking for*.

Let's discuss the 3 types of dates and see which one best fits our need.

### Dates from documents

GDS says *if you ask for a date exactly as it’s shown on a passport, credit card or similar item, make the fields match the format of the original. This will make it easier for users to copy it across accurately.*

We took this exact approach when we designed the expiry date field, in chapter 2. We gave users a text box that expects a number matching the format on their card. Users simply copy what they see.

### Memorable dates

The defacto thinking is that date pickers are always better than manually entering numbers for a date. But for dates that are easy to remember, such as date of birth, this isn't true. It's often slower and harder to enter a memorable date in this manner, than it would be to type numbers into a text box unassisted.

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

Some versions of iOS won't show the numeric keyboard, even if you use `type="number"`. The `pattern` attribute fixes this problem[^7] which is why we've included it in each of the inputs above.

#### Auto-tabbing

Some websites automatically move focus from one field to the other, once the correct *amount* of characters have been entered. This is a usability failure, which we'll discuss in the next chapter.

### Finding dates in a calendar

In the case of booking a flight, users are neither dealing with a memorable date nor one found in a document. When booking flights, we often orientate ourselves around day and week. To mimic this, we'll need a more convenient solution than 3 text boxes. We need a calendar, or what's commonly known as a date picker.

Interfaces that try to solve too many problems at once cause problems. The primary need for the calendar is to select a date. If we try to convey, for example, price and availability inside the calendar too, this would result in a busy interface that overloads the user. Unless the user has a super-sized screen, but not everyone has one of those.

Instead, we'll first let users choose a date unencumbered, without the distraction of price and availability. Then later, we'll give them the extra context they need to choose the right flight for them.

#### Date input

Before HTML5, we always had to build our own date picker using Javascript. We know this is a complex task because we had to consider the breadth of requirements when we creates the autocomplete component earlier.

Mobile browsers that support HTML5&mdash;nowadays, that's most&mdash;have `input type="date"`. This is useful because:

- We don't need to spend time designing and developing our own component.
- They are performant because they are provided by the browser.
- They are familiar because every website (and native app for that matter) will use the same interface.
- They are accessible by default.
- Users receive improvements as soon as the browser makes release them.

All of which give us back time and headspace to solve other, more pressing problems. The overused phrase is *don't reinvent the wheel*.

Here's how it looks on mobile:

![Mobile native date control](./images/mobile-date.png)

Desktop browser support is not as good. Chrome and Edge work well but Firefox, for example, doesn't have any support at time of writing.

![Desktop native date control](./images/desktop-date.png)

If you're concerned about it looking different in different browsers you'd needn't be. In Progressive Enhancement 2.0, at 16 minutes in[^8], Nicholas Zakas proves that users don't notice the differences between browser implementations. Nobody cares about your website as much as you do and rather officially websites do not need to look the same in every browser[^9].

HTML:

```HTML
<div class="field">
	<label for="departureDate">
		<span class="field-label">Departure date</span>
	</label>
	<input class="field-textBox" type="date" id="departureDate" name="departureDate">
</div>

```

#### What about browsers that lack support?

Browsers that don't support date inputs will degrade into a text box, which may be sufficient. This is one of the many advantages of Progressive Enhancement. We can choose to degrade gracefully. Or, we can decide to provide an enhanced fallback.

As choosing dates is integral to booking a flight, the degraded solution isn't as good as we'd like. Choosing a date should be easy for as many people as possible. We may change tacts, in the future, once support for date inputs improve, but more on this shortly. Right now though, we'll create a date picker component.

The first problem we need to solve is to ensure we only give users the our component when their browser doesn't support the native input. That is, we'll need to feature detect this capability:

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

Jeremy Keith often talks about the web being a *continuum*[^10]. What he's means is, web is constantly changing. New browsers are relaeased frequently. All of them have varying features and capabilities. At any particular moment in time, we need to think about what level of support (or optimisation) makes sense for a given feature.

As we said earlier, desktop browser support for the native input is lacking. And as choosing a date is so integral to the flow, we've decided to plug the gap (for now, at least.) In future, things might change&mdash;support is likely to improve. At which point we can change tacts.

We may choose not to support the rapidly diminishing number of users that would otherwise experience the degraded text box. Practically speaking this means removing the Javascript, giving users a faster experience and ourselves less to maintain.

#### Date picker component

Having now detected support, we'll need to design the date picker component itself.

How it might look:

![Date widget](./images/date-widget.png)

Notes:

- Buttons should have large tap[^] targets making it easy to press with a finger or mouse.
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

Most designers would stop here. After all that's everyone covered right? Not quite. Some still use a browser without support for the native date input; and that, Javascript isn't available. Those falling under this category, albeit a minority, will have to type a date into a text box unassisted.

The problem is that there aren't any instructions regarding the expected format. We can't explain this with a hint because it will show to those using a browser that *does* support the date input. This is problematic, because the format is different.

![]()

We can mitigate the risk by forgiving users for delimitting the date with slashes, periods and spaces. But typing the year first, for example, *will* cause an error. A well written error message will have to handle this case. Fortunatley, this shouldn't happen too often.

This is one of those occasions whereby a good solution for most, is less than ideal for a minority. This is a trade off that we have to make sometimes but at least it's still accessible. Interesting this points out a vital difference between accessible interfaces and inclusive ones. This experience is accessible but it's lacking inclusivity.

## 3. Choose passengers

Next, users need to enter the amount of people flying. ‘People’ fall under 3 catgories:

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

In chapter 2 we disabled the spinner. Spinners, also known as steppers, let users increase or decrease the value by a constant amount. They are great for making small adjustments. In the same article we referenced earlier, Luke Wobrelkski says:

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

Adding `tabindex="-1"` removes the buttons from the tab sequence. This ensures we don't disturb the natural flow and order of form elements. The buttons use symbols and although the best icon is text, these icons are well understood and keep the interface clean. For those using screen readers, we add `aria-label="Increment"` so the interface makes sense audibly.

Javascript:

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

Up to now, we've simply been gathering preferences in order to determine which flights to show. Now we have what we need, the user can select a flight from the list of results.

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
- The legend contains the flight date giving context to each of the radio buttons in the group.
- Each radio button contains departure time, arrival time and price, giving users all the information they need in order to make an informed decision.

## 5. Choose seat

Next, the user needs to choose a seat. If more than 1 passenger is travelling, they can select more than 1 seat. So we'll need to represent each seat as a checkbox. Conversely, if 1 passenger is travelling we'd use radio buttons.

How it might look:

![Image](./images/image.png)

Up to now, we've displayed form fields in a stacked manner. This time though, we've laid out the checkboxes in rows, just like they are on the plane. This meets the need that *as a user, I want to know the location of the seat, so that I can choose preferential seats*. Shortly, we'll look at how we can meet the same need for those using screen readers.

### Breaking the rules

There are many seats on a plane, therefore there are many checkboxes or radio buttons we need to show. Traditional advice suggests using radio buttons where there are less than 7 choices, otherwise use a select box. This makes some sense in the context of a long an unweildly form.

Fortunately, we don't have that problem because we have a dedicated screen solely for choosing seats. Using radio buttons makes the seats easy to compare and select. This sort of interface just isn't possible with a select box, proving that as with everything in design, *it depends*.

### Nested fieldsets

The preferences collected up to now have influenced the design of the interface. Specifically, we didn't ask users whether they wanted to travel first class or economy. This reduced the friction upfront in exchange for a more complicated interface now.

We've had to present both first class and economy seats using a nested fieldset. The innermost legends represents the categorisation helping sighted and visually-impaired users equally.

Asking users to specify this preference in the ‘gathering’ process, reduces the complexity at this stage. To offset the added friction upfront we can default the selection to economy and let users change this using the patterns from chapter 2.

How it might look:

![Image](./images/image.png)

HTML:

```HTML
<div class="field field--seats">
	<fieldset>
		<legend>
			<span class="field-legend">Choose seats</span>
			<span class="field-hint">Seat numbers represent the row where 1 is at the front of the plane. Seats run from A-F where A and F are window seats, C and D are isle seats.</span>
		</legend>
		<div class="row">
			<div class="field-checkbox">
				<label for="S1A">
					<input type="checkbox" class="field-seatCheckbox" name="seat" value="1A" id="S1A">
					1A
				</label>
			</div>
			<div class="field-checkbox">
				<label for="S1B">
					<input type="checkbox" class="field-seatCheckbox" name="seat" value="1B" id="S1B">
					1B
				</label>
			</div>
			<div class="field-checkbox">
				<label for="S1C">
					<input type="checkbox" class="field-seatCheckbox" name="seat" value="1C" id="S1C">
					1C
				</label>
			</div>
			<div class="field-checkbox">
				<label for="S1D">
					<input type="checkbox" class="field-seatCheckbox" name="seat" value="1D" id="S1D">
					1D
				</label>
			</div>
			<div class="field-checkbox">
				<label for="S1E">
					<input type="checkbox" class="field-seatCheckbox" name="seat" value="1E" id="S1E">
					1E
				</label>
			</div>
			<div class="field-checkbox">
				<label for="S1F">
					<input type="checkbox" class="field-seatCheckbox" name="seat" value="1F" id="S1F">
					1F
				</label>
			</div>
		</div>
		<div class="row">
			...
		</div>
	</fieldset>
</div>
```

Notes:

- The hint informs screen reader users what the label means in relation to seat location. Without it the fields lack clarity.
- Seats that aren't available are disabled.

### Hiding the checkboxes

On small screens each seat eats into the minimal space available potentially causing a horizontal scroll bar to appear or the seats to wrap over 2 lines. If user feedback proves this to be a problem, we might consider hiding the checkboxes.

Hiding the checkboxes without Javascript is dangerous. This is because pressing <kbd>tab</kbd> moves focus to the checkbox&mdash;not the label. Hiding the checkboxes breaks the interface, because tabbing to a hidden input doesn't provide feedback.

We can fix this by listening to `focus` and `blur` events. When the user moves focus to the visually hidden checkbox, we add a CSS class to the associated label giving users the illusion that it is in focus.

```CSS
.enhanced .field-seatCheckbox {
	position: absolute;
	left: -9999em;
	top: 0;
}

.field-seatCheckbox-isFocussed {
	border: 2px solid #000;
	border-top: none;
}
```

We hide the checkbox with CSS but only when the Javascript is available, indicated by the `enhanced` class which is added in the `head` of the document:

Adding the enhanced class:

```JS
document.documentElement.className = 'enhanced';
```

```JS
function SeatEnhancer() {
	var checkboxes = $('.field-seatCheckbox');
	checkboxes.on('focus', $.proxy(this, 'onCheckboxFocus'));
	checkboxes.on('blur', $.proxy(this, 'onCheckboxBlur'));
}

SeatEnhancer.prototype.onCheckboxFocus = function(e) {
	$(e.target).addClass('field-seatCheckbox-isFocussed');
};

SeatEnhancer.prototype.onCheckboxBlur = function(e) {
	$(e.target).removeClass('field-seatCheckbox-isFocussed');
};
```

### Limiting seat selection

Earlier we said to use radio buttons if 1 passenger is travelling. This naturally stops users from selecting more than 1 seat. In the case of checkboxes, the user can select more than their quota which results in an error once submitted.

User testing may prove this not to be a problem and therefore doesn't need any further consideration. However, if it does, we could limit the amount of seats they can select using Javascript.

To do this we could disable the remaining checkboxes once the user reaches their quota. However, if the user has reaches their quota and clicks another seat nothing happens creating a disagreable experience because their is no feedback.

Savvy users might realise they need to deselect their previously chosen seat before selecting another. Less savvy users might not. We can do the hard work in code by automatically performing this action for them.

Javascript:

```JS
function SeatLimiter(max) {
	this.max = max;
	this.checkboxes = $('');
	this.checkboxes.on('click', $.proxy(this, 'onCheckboxClick'));
}

SeatLimiter.prototype.onCheckboxClick = function(e) {
	this.check(e.target);
};

SeatLimiter.prototype.check = function(checkbox) {
	var checked = this.checkboxes.filter(':checked');
	if(checked.length >= this.max) {
		if(checkbox !== this.checked[0]) {
			this.checked[0].checked = false;
		} else {
			this.checked[1].checked = false;
		}
	}
};
```

## Summary

In this chapter we continued using the One Thing Per Page pattern from chapter 2 which gave us the screen space to create innovative interfaces for booking a flight. We then broke new ground by designing the following progressively-enhanced, custom form components:

- Autocomplete, to make selection from a long list fast.
- Date picker, formatted as a calendar, to make date finding easy.
- Stepper to make small adjustments to the amount of passengers effortless.
- Seat chooser that makes picking seats as close to real life as possible.

All of the components are fully inclusive. We made sure of this by designing them to be focusable and operable with keyboards and screen readers. We used ARIA attributes where necessary and ensured the components work well on small and big screens alike.

Whilst each of these components were designed with a flight booking service in-mind, all of them are transferable to other problem domains such as booking a cinema ticket or a hotel room.

## Footnotes

[^1]: http://www.lukew.com/ff/entry.asp?1950
[^2]: https://www.amazon.co.uk
[^3]: http://caniuse.com/#feat=datalist
[^4]: https://www.paciellogroup.com/blog/2014/09/web-components-punch-list/
[^5]: http://infiniteundo.com/post/25326999628/falsehoods-programmers-believe-about-time
[^6]: https://www.gov.uk/service-manual
[^7]: http://html5doctor.com/html5-forms-input-types/#input-number
[^8]: https://www.youtube.com/watch?v=hdTxeR90_1E
[^9]: http://dowebsitesneedtolookexactlythesameineverybrowser.com/
[^10]: https://adactio.com/journal/6692

## Other stuff?

[^]:(https://www.nngroup.com/articles/drop-down-menus-use-sparingly/)
[^]:(https://www.slideshare.net/cjforms/design-patterns-in-government-2016)

[^GDS type]:(https://alphagov.github.io/accessible-typeahead/)
[^leonie]:(http://ljwatson.github.io/design-patterns/autocomplete/)
[^ppk]:(https://medium.com/samsung-internet-dev/making-input-type-date-complicated-a544fd27c45a)
[^exampledatepickers]:(http://www.webaxe.org/accessible-date-pickers/)
[^calendarscreenreader]:(https://ux.stackexchange.com/questions/60884/best-way-for-date-field-for-visually-impaired-users)
[^calendarw3ariaspec]:(https://www.w3.org/TR/2009/WD-wai-aria-practices-20090224/)


https://tink.uk/screen-reader-support-for-disabled-read-only-form-fields/

https://codepen.io/siiron/pen/MYXZWg
https://codepen.io/anon/pen/NvKJmw

