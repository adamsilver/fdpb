# Book A Flight

In this chapter, we'll design a flight booking system. At first this might seem like a bit of a *niche* problem, especially when compared to *A Registration Form* and *Checkout*. However, this chapter encourages us to solve several interesting topics that are transferable to other problem domains.

Here are the main steps in our flow:

1. Choose origin/destination
2. Choose departure/return date
3. Choose passengers
4. Confirming flight
5. Choosing where to sit
6. Payment

The last step, *Payment*, is something we already solved in *Checkout*, so you refer to that if you're rebelliously reading this book out of order.

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

Interfaces that try to solve too many problems at once are often detrimental to the user experience. For example, if we were to try and convey price and availability inside a calendar widget, we could end up with too much data for users to process. Instead, we'll let users pick a date preference in which to fly. We can help them decide which date to choose later on.

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

In addition to the `input` there is a `button` which shows the calendar and the calendar itself, both injected with Javascript, as they do nothing without Javascript.

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

The next thing the user needs to do is choose how many passengers are going on the trip. Passengers fall under certain categories. For example, Easyjet[^] has the following categories:

1. Adults, aged 16 and over.
2. Children, aged between 2 and 15 years old.
3. Infants, who are under 2 years old.

For each category we'll want a form field that uses the hint pattern from chapter 1.

It's the type of form field that is the most interesting part of this discussion. Up until recently, I've seen people using a select box with the values ranging from 1-9. As discussed above the select box is not the most friendly control.

Again, radio buttons take up a lot of space which leaves us with a text box. HTML5 gave us a `number` field using `input type="number"`. When supported it has the following benefits:

1. On mobile a keyboard will appear with numbers on it.
2. On desktop, the text box contains up and down arrows that make incrementing and decrementing quicker with the mouse.
3. On desktop, the user can use the up and down arrows on the keyboard to increment and decrement the value.
4. We can constrain the input to be a minimum of 0 and a maximum of whatever the maximum amount of tickets is for our service. Let's say 9.

There is just one small problem with the number field. The up and down arrows are very small. We can enhance those by first hiding them with CSS and then implementing our own increment and decrement functions in Javascript:

```CSS
input[type=number]::-webkit-inner-spin-button,
input[type=number]::-webkit-outer-spin-button {
  -webkit-appearance: none;
}
```

```JS
Add buttons either side
on decrement click, parseint value
minus 1 and set
etc
```

HTML:

```JS
Add buttons either side
on decrement click, parseint value
minus 1 and set
etc
```

What it looks like:

![Image here](/etc/)

## Confirming a flight

Once the user has chosen their requirements, and assuming there are tickets available, they will see a list of flights going out. As the user is booking one flight, we'll present the results as radio buttons.

![Image here](/etc/)

Within the label we can put all the pertinet information. Price and flight times etc. Clicking continue stores their choice and takes the user to the next step.

## Choosing a seat

- Checkboxes
- Nested fieldset and then don't need appointment booking?
- Only being able to select a certain number of seats.

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
