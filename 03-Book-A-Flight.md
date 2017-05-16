# Book A Flight

In this chapter, we'll design a flight booking system. At first this might seem like a bit of a *niche* problem, especially when compared to *A Registration Form* and *Checkout*. However, this chapter encourages us to solve several interesting topics that are transferable to other problem domains.

Here are the main steps in our flow:

1. Choose origin/destination
2. Choose departure/return date
3. Choose passengers
4. Confirming flight
5. Choosing a seat

There are other steps, like providing passenger name and passport information, as well as paying out but we'll leave those out as we have covered the majority of those problems in chapter 2, *Checkout*.

## 1. Choose origin/desination

The first thing users need to do is select a destination (and an origin). Without this information we can't search for relevant flights. The destination field could be one of the following:

- radio buttons
- select box
- text box

We'll discuss each in turn.

### Radio Buttons

Radio buttons are great because the choices are exposed, and they are generally well understood and accessible by default. However, they are less suitable when there are many choices to choose from.

We could have hundreds, if not thousands of destinations. Using radio buttons will create a very long and heavy page. Some browsers provide a native search facility which can be activated by `CMD+F` which allows users to *jump* to the option quickly.

But most users don't know about this, and we shouldn't rely on an inconspicuous browser feature to fix issues in our own design. We can do better.

### Select Box

Designers often use `select` boxes because they save space. However, as Luke Wobrelski states: *Dropdowns Should be the UI of Last Resort*[^]. Here's why:

- some users find them hard to close
- some users try to type into them
- some users confuse focused options with selected ones
- users aren't able to pinch-zoom options on devices
- they have limited hierarchy control
- they hide choices behind an unnecessary extra click
- they are not easily searchable

Again, we can do better.

### Text Box

A text box (`input type="search"`) is another option. The search type enhances the functionality of a regular text box (`input type="text"`) by allowing the user to clear the contents of the field&mdash;either by tapping the *X* or pressing the escape key.

![Image here](/etc/)

HTML:

```html
<div>
	<label for="destination">Destination</label>
	<input type="search" name="destination" id="destination">
</div>
```

This design works well when the results of the input are completely dynamic and vast in size and breadth. For example, searching for products on Amazon. But for this service, we have a finite amount of destinations that we know in advance.

If we let users search unassisted, they will likely be met with a screen  displaying a message: *we don't fly to that destination*.

![Image here](/etc/)

Once again, we can do better.

### Autocomplete

What we really need is a text box and select box rolled into one. As the user types a destination, suggestions appear beneath allowing them to autocomplete the field. This saves time scrolling through a plethora of destinations.

Up until recently there has been no such element for us to use. HTML5 gave us `datalist` but unfortuntely, it's too buggy[^caniuse] for us to use, especially for the open web.

If you have fine control over the design and usage of the component, you may still decide to use one. This book is about designing robust, and inclusive experiences, and so we don't have the luxury of this choice.

Our remaining option is to build a custom component ourselves. When we build a custom component there are rules we need to follow[^alice barlett talk bruce lawson?]. A custom component must:

- be focusable with the keyboard
- be operable with the keyboard
- work with assistive devices
- work without Javascript

To solve the last problem we need to decide on what the core experience will be. Will it be a text box or select box? On balance, it seems prudent for us to use the select box. But you may take a different tact depending on the context of your own problem.

Here's how it looks before we enhance it:

![Image here](/etc/)

HTML:

```html
<div>
	<label for="destination">Destination</label>
	<select name="destination" id="destination">
		<option value="">Select</option>
		<option value="france">France</option>
		<option value="germany">Germany</option>
		<option value="spain">Spain</option>
	</select>
</div>
```

Here is the enhanced custom combobox:

![Image here](/etc/)

HTML:

```html
<div class="combobox">
	<input
		type="text"
		name="destination"
		id="destination"
		autocomplete="off"
		role="combobox"
		aria-owns="combobox-options"
		aria-autocomplete="list"
		aria-expanded="true"
		class="combobox-textbox"
	>
	<ul
		id="combobox-options"
		role="listbox"
		class="combobox-options combobox-options-isHidden"
		>
		<li
			id="combobox-option--0"
			role="option">
			France
		</li>
		<li
			id="combobox-option--1"
			role="option"
			aria-selected="true">
			Germany
		</li>
	</ul>
	<div
		aria-live="polite"
		aria-atomic="true"
		role="status"
		class="combobox-status">
	</div>
</div>
```

In Atomic Design[^] speak this *molecule* is made up of three atoms:

- Text box
- Menu
- Status box

This HTML, in combination with CSS and Javascript will display suggestions beneath the text box as the user types. All the attributes are necessary in order to build an inclusive component that users can use with their mouse, (on-screen) keyboard and screen readers.

Here's a run down of the attributes:

The text box has:

- `role="combobox"` so that assistive devices know what it is, as opposed to a regular text box, for example.
- `aria-autocomplete="list"` so that assistive devices may explain that a list of choices will appear from which the user can choose.
- `aria-expanded` so that assistive devices may indicate the menu is showing or not.
- `aria-owns="combobox-options"` connects the text box to the menu by `id`.
- `aria-activedescendant` identifies the active option using the `id` of the menu option. This is because the options in the menu aren't focusable with the keyboard. In a composite widget like this, focus should stay in the text box, so that uses may carry on typing, even if they are 'focussed' on an option within the menu.
- `autocomplete="off"` stops browsers providing their own suggestions and interfering with those offered by our own.

Each option has:

- `role="option"` to explain what it is.
- `id` to tie up with `aria-activedescendant` explained above.
- `aria-selected` to indicate which option is active.

The status box has:

- `aria-role="status" to provide a status such as *2 results are available. France (1 of 2) is selected*
- `aria-live="polite" to announce the status when the user stops typings as opposed to interupting them.
- `aria-atomic="true"` means that assistive devices announce the entirety of the status. The reason for this is so that we can update just the number perhaps, which is perhaps more efficient in Javascript, than injecting a large lump of text, but so that the user doesn't hear "2".

The Javascript specification is as follows:

- listen to keyup events on the text box.
- display options if and when they match.
- if an option matches, hide options
- if user presses up or down, 'focus' the option
- if an option is 'focussed', when the user presses spacebar or enter, put the value in the textbox and close the suggestions, otherwise enter should implicitly submit the form (like normal).
- if the user clicks an option, put value in text box, and move focus back to the text box.

This code is available on the complimentary demo website[^]. If it's not suitable for your needs, you can use the above specification to guide you.

## Choosing dates

Dates are hard[^]. There is no getting away from this fact. Different time zones, formats, delimitters, days in the month, length of a year, day light savings and on and on. It's hard work designing all of this complexity out of a UI.

Traditionally, and sometimes still to this day, websites use three select boxes for a date of birth field; one for day, month and year. We already know select boxes are problematic, but one of their redeeming qualities is that they stop the user entering wrong information.

In the case of picking dates, however, even *this* is not the case. Users can, for example, select *31 February 2017*, which will result in a validation error.

![Select boxes for dates](./images/date-select.png)
[https://www.gov.uk/state-pension-age/y/age]

So why use select boxes instead of a simple textbox? Mostly because it stops the system needing to handle a plethora of different formats. Some dates start with month; others with day. Some delimit dates with slashes; others with dashes. It's really hard to determine, meaning we can't be as forgiving with what we accept as we are with other fields.

Before we can design a date control, we first need to understand what type of date we're asking for. Like GDS says *the way you should ask users for dates depends on the types of date you’re asking for*.

### Dates from documents

We already discussed this in *Checkout* when we designed the expiry date field. If you haven't read it yet, I'll wait here until you have. Back? Let's continue.

### Memorable dates

People find it easy to remember certain dates, such as date of birth. It's often slower and harder to find this date (using a calendar, for example) than it is to type numbers into a text box unassisted.

This is why GDS suggests three *separate* text boxes on their own; one for day, month and year. Why three? To avoid the formatting issues we discussed earlier. Here's what it looks like:

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

Like radio buttons, this uses the fieldset and legend to group the three text boxes together so that users know the month is in relation to date of birth.

The `pattern` attribute is there to trigger the numeric keyboard on iPhones as some versions won't automatically show it even though the *type* should be all we need[^CHECK GDS SERVICE MANUAL FOR DATES].

As there are three separate boxes for one field, some websites automatically advance the user to the next field automically using Javascript. This is a usability problem because it:

- makes mistakes harder to fix. I may make a mistake in the first box, but my cursor is now in the second box.
- Screen readers have trouble http://www.freedomscientific.com/Training/Surfs-Up/Forms.htm

### Calendar control

When booking a flight, which is the case for us, it's helpful to have some context to help users choose a date. People often orientate themselves by day and week when booking flights. For this reason, we'll want to offer users a more convenient solution than three separate text boxes.

Interfaces that try to solve too many problems at once often cause problems for users. For example, if we try and convey price and availability inside a calendar, this could cause information overload. Instead, we'll only let users pick a date. And later, we'll help them pick a flight.

#### Input Type Date

Up until recently, we were left to create our own custom calendar control using Javascript. We already know how hard it is to design and build a custom component because we did just that with the autocomplete component above. Wherever possible, we should let the browser do the work for us.

With the advent of mobile browsers, HTML5 gave us `input type="date"` which enhances a text box into a calendar control. This is good because they are:

- cost effective&mdash;they save us design and development time
- performant (as users don't need to download and execute custom code)
- familiar because native apps/different websites will use the same interface
- accessible by default

Also, when browsers release improvements to native controls, our users get them immediately; they don't have to wait for us to deploy code, freeing us up to solve other problems instead.

The input is well supported on mobile. Here's what it looks like:

![Mobile native date control](./images/mobile-date.png)

Desktop browser support is not as good. Chrome and Edge work well but Firefox, for example, doesn't have any support, although it's on the way. Here's what it looks like in Chrome:

![Desktop native date control](./images/desktop-date.png)

If you're concerned about it looking different across browsers, don't be. User's either don't notice or don't care which Nicholas Zakas beautifully demonstrates in BLAH BLAH[^]. If you're still not convinced you should take a look at Do Websites Needs To Look The Same In Every Browser[^].

Here's the HTML:

```HTML
<div>
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

We can use these pseudo selectors to style these things where we think it helps and tweak the styles and turn things off like "spinners".

So far this works really well. We do have one remaining issue to address. That's browsers that don't support `input type="date"`.

#### Date input unsupported

Browsers that don't support the date input will degrade gracefully into a simple text box, which may be sufficient. This is one of the many beautys of progressive enhancement, we can choose to degrade gracefully, or, we can decide to plug the gap.

For us, choosing dates is integral to booking a flight online. As much as a text box can work, we'll want to do better. We'll once again want to design and build our own custom component.

We'll first need to detect the browser *doesn't* support the native date input. Only then do we want to execute our custom component.

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
- support is drastically improved and that hardly any of our users have an unsupported browser

In both of these cases we may choose to remove our custom component altogether, giving us less to maintain, and give users faster experiences.

To plug the gap, you can choose from a plethora of existing accessibile solutions[^]. Or we can be bold and design our own.

#### Custom Calendar Control Component

What it looks like:

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

For each category we'll want a form field that uses the hint pattern from chapter 1, *Register*. Here's what it looks like:

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

What it looks like:

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
