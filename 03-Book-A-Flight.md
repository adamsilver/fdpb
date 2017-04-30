# Book A Flight

In this chapter, we'll design a flight booking system. At first this might seem like a bit of a *niche* problem, especially when compared to *A Registration Form* and *Checkout*. However, this chapter encourages to solve several interesting topics that are very much applicable to other problem domains.

Here are the main steps in our flow:

1. Choose origin/destination
2. Choose departure/return date
3. Choose passengers
4. Confirming flight
5. Choosing where to sit
6. Payment

The last step, *Payment*, is something we already solved in the previous chapter, *Checkout*, so we'll skip this step in this one.

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

Once again: we can do better.

### Autocomplete

What we really need is a text box and select box rolled into one. As the user types a destination, suggestions appear beneath allowing them to autocomplete the field. This saves time scrolling through a plethora of destinations.

Up until recently there has been no such element for us to use. HTML5 gave us `datalist` but unfortuntely, it's too buggy[^caniuse] for the general web.

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
			role="option"
			tabindex="-1">
			France
		</li>
		<li
			id="combobox-option--1"
			role="option"
			tabindex="-1"
			aria-selected="true">
			Germany
		</li>
	</ul>
	<div
		aria-live="polite"
		role="status"
		class="combobox-status">
	</div>
</div>
```

There are three parts to this:

- Text box
- Menu
- Status box

This HTML, in combination with CSS and Javascript will display suggestions beneath the text box as the user types. All the attributes are necessary in order to build an inclusive component that users can use with their mouse, (on-screen) keyboard and screen readers.

Here's a run down of the attributes:

The text box has:

- `role="combobox"` so that assistive devices know what it is.
- `aria-autocomplete="list"` to explain that a list of choices will appear from which the user can choose.
- `aria-expanded` to indicate that the menu is showing or not.
- `aria-owns="combobox-options"` connects the text box to the menu by `id`.
- `aria-activedescendant` identifies the active option using the `id` of the menu option.
- `autocomplete="off"` stops browsers providing their own suggestions and interfering with our component.

Each option has:

- `role="option"` to explain what it is.
- `id` to tie up with `aria-activedescendant` explained above.
- `aria-selected` to indicate which option is active

The "status" div has:

- `aria-role="status" to provide a status such as *2 results are available. France (1 of 2) is selected*
- `aria-live="polite" to announce the status when the user stops typings as opposed to interupting them.

The Javascript specification is as follows:

- listen to keyup events on the text box.
- display options if and when they match
- if an option matches exactly hide options
- ...and when displaying options apply the attributes as mentioned above
- if user presses up or down, highlight the option
- ...and mark attribues as per above
- if an option is highlighted, when the user presses spacebar or enter, put the value in the textbox and close the suggestions, and update the attributes.
- if the user clicks an option, put value in text box, and move focus back to the text box.

This code is available on the complimentary demo website[^]. If it's not suitable for your needs, you can use the above specification to guide you.

## Choosing dates

Dates are hard[^]. There is no getting away from this fact. Different time zones, formats, delimitters, days in the month, length of a year, day light savings and on and on. It's hard work designing all of this complexity out of a UI.

Traditionally, and sometimes still to this day, websites use three select boxes; one for day, month and year. We already know select boxes are problematic, but one of their redeeming qualities is that they stop the user entering wrong information.

In the case of picking dates, however, this is not the case. This is because users can, for example, select *31 February 2017*, which results in a validation error. This is just one of many problems.

![Select boxes for dates](./images/date-select.png)
[https://www.gov.uk/state-pension-age/y/age]

So why use select boxes instead of a simple textbox? Mostly because it stops the system needing to handle a plethora of different formats. Some dates start with month; others with day. Some delimit dates with slashes; others with dashes. It's actually really hard to know. More on this later.

Before we can design a date control, we first need to understand what type of date we're asking for. Like GDS says *the way you should ask users for dates depends on the types of date you’re asking for*.

### Dates from documents

We already discussed this in *Checkout* when we designed the expiry date field. If you're not reading this book in order, feel free to read that now. We'll wait here until you have.

### Memorable dates

People find it easy to remember certain dates, such as date of birth. It's often slower and harder to find this date (using a calendar, for example) than it is to type numbers into a text box unassisted.

This is why GDS suggest using three *separate* text boxes on their own; one for day, month and year. Why three? To avoid the formatting issues we discussed earlier. Here's what it looks like:

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

#### Auto tab

- reference bank/hargreaves to emphasis the point.
- Forms that Automatically Advance to the Next Field
http://www.freedomscientific.com/Training/Surfs-Up/Forms.htm

### Calendar control

When booking a flight, for example, it's helpful to have some context to help us choose a date. People often orientate themselves by day and week when booking flights. For this reason, we'll want to offer users a more convenient solution than three separate text boxes.

UIs that try to solve many problems at once are often detrimental to the resulting user experience. For example, if we were to try and convey price and availability inside a calendar widget, we would end up with too much data to process at once.

Because of this, we'll let users pick a date preference in which to fly. We can help them decide which date to choose later on.

#### Input Type Date

Up until recently, we were left to create our own custom calendar control using Javascript. We already know how hard it is to design and build a custom component because we did just that with the Autocomplete. Wherever possible, we should let the browser do the work for us.

HTML5 gave us `input type="date"` which enhances a text box into a calendar control. This is good because they are:

- cost effective&mdash;they save us design and development time
- performant (as users don't need to download and execute custom components)
- familiar because native apps use the same UI control
- accessible by default

Also, when browsers release improvement to native controls, our users get them without waiting for us to upgrade our own code. This frees us up to solve other problems instead.

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

So far this works really well. We do have one remaining issue to address. That's browsers that don't support `input type="date"`.

#### Date input unsupported

If someone uses a browser that lacks support for the date input, we can build a custom component, much like the Autocomplete we designed earlier.

In addition to that though, we'll want to first detect that the browser doesn't support the native date input. Only then do we want to execute our custom component.

```Javascript
function supportsDateInput() {
	var el = document.createElement('input')
	el.type = "date"
	return el.type == "date";
}

if(!supportsDateInput()) {
	// create and use custom Javascript date picker widget
}
```

This is why Progressive Enhancement is such a useful technique. We don't actually have to write any of this code. We can choose to let it degrade. The user can still type a date, it's just not so easy.

Conversely, we may even find research shows that our custom component works better than the native input. If we do, we can just remove the condition, which will ensure it executes in all browsers&mdash;not just those lacking support for the native control.

Wether we choose to use an existing component[^] or we make the bold step to design our own, it is beneficial to be able to identify the qualities of a well-design and fully inclusive date picker.

#### Writing our own date picker

If we want to create our own date picker, we'll need to know how. I have prototype[^] you can look at to see how it all works.

What it looks like:

![Date widget](./images/date-widget.png)

HTML:

```HTML
<div>
</div>
```

Here's an explanation of the design and code:

- We use a `button` with `type="button"` so that it doesn't submit the form. The button is also responsible for toggling the visibility of the calendar.
- `aria-hidden` tells screen readers whether it's showing or not.
- The table has `role="grid"` so that screen readers know what type of widget this is. It has a label which describes what type of grid it is.
- Each row has role=row
- Each cell has role=gridcell
- Each column heading has role=columnheading
- Live region
- Label by
- activedescendant
- td description
- aria-selected
- Hitting the previous month should show the previous month and select the first day of that month. Same for hitting next month.
- Once focus is set on the grid, the user can use arrow keys to move about to select a date. We avoid making the cells part of the tab sequence as it's too much for users to wade through.

Design wise:

- We want big buttons to make the days easily tapable. Fine motoro skills
- Create a `<button type="button">` that toggles the calendar's visibility.
- When displaying the calendar, display it inline below the text input. Overlays are complicated beasts that obscure parts of the screen, so we'll want to avoid those and keep things simple. Also on mobile, a dialog is a bit of an anti-pattern as the estate available to the overlay is often less than utilising the main page itself.
- When focussed within the Date Picker, pressing `escape` should hide it.
- Pressing enter/space should select the date and close the picker.
- Make back/previous buttons to be naturally focusable.
- up/down/left/right for dates, and continuation
- Event though we are making the control screen reader friendly, it's probably not useful to them. But we don't assume. Visual users may benefit from Cal. Not so much users of screen readers. We can still do something though.https://ux.stackexchange.com/questions/60884/best-way-for-date-field-for-visually-impaired-users

---

SPEC: https://www.w3.org/TR/2009/WD-wai-aria-practices-20090224/

- picking dates natively
  - those that get the native date enhancement
    - style better
    - turn off shit with css
  - those which get degraded text box
    - strict formatting where necessary...
    - good error messaging (good for everyone else too)
	- With design there is always a tradeoff. I consider myself to care about everyone, but when you design for everyone you may end up designing for noone. For example, someone who considers themself an "intellect" may love reading complex high brow paragraphs of text, but we know that hemmingway says we should write for grade 6 or less if possible because it's easy to read for everyone. Can't please em all.
---

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
