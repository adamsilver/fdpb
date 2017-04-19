# Book A Flight

In this chapter, we'll be designing a flight booking system. On the face of it, this is more of a niche problem compared to Registration and Checkout forms. However, every topic in this chapter is, not only interesting but transferable to other problem domains.

Here are the main steps in our flow:

1. Choose origin/destination
2. Choose departure/return date
3. Choose passengers
4. Confirming flight
5. Choosing where to sit
6. Payment

We discussed making online payments in the previous chapter so we'll skip that part.

## 1. Choose origin/desination

The first thing users need to do is select a destination (and an origin). Without this information we can't offer the user any flights. The destination field could one of the following:

- radio buttons
- select box
- text box

### Radio buttons

The usability of radio buttons is good, generally speaking. But they fall down when there are many choices to choose from. We could have hundreds, if not thousands of destinations to choose from, creating a long page.

The user could use the browsers search feature (`CMD+F`), but a) many users won't know about this feature and we we shouldn't rely on an inconspicuous browser feature to fix issues with our own design.

We can do better.

### Select Box

Designers often use `select` boxes because they save space. However, as Luke Wobrelski states: *Dropdowns Should be the UI of Last Resort*[^]. Here's why:

- some users are unable to close them
- some users try to type into it
- some users confuse focused options with selected ones
- users aren't able to pinch-zoom options on devices
- they have limited hierarchy control
- they hide choices behind an unnecessary extra click
- they are not easily searchable

Again, we can do better.

### Text box

A text box (`input type="search"`) is another option. The search type enhances the functionality of a regular text box (`input type="text"`) by allowing the user to clear the contents of the field&mdash;either by tapping the X or pressing the escape key.

![Image here](/etc/)

HTML:

```html
<div>
	<label for="destination">Destination</label>
	<input type="search" name="destination" id="destination">
</div>
```

This option is good when the amount of search options is completely dynamic and vast in size and breadth, like searching for products on Amazon for example. But for this service, we have a finite amount of destinations that we know in advance.

If we were to let users search, unassisted, they could often be met with no results: *we don't fly to that destination*.

One more time now: we can do better.

### Typeahead combobox

What we really need is a text box and select box rolled into one. As the user types a destination, suggestions appear beneath allowing them to autocomplete the field. This saves time scrolling through a plethora of destinations.

Up until recently there has been no such element for us to use. HTML5 gave us `datalist` but unfortuntely, it's very buggy[^caniuse].

We've done the hard work to analyse the options to us. We're left to build a custom component. When we build a custom component there are rules we need to follow[^alice barlett talk bruce lawson?]. A custom component must:

- be focusable with the keyboard
- be operable with the keyboard
- work with assistive devices
- work without Javascript

To solve the last problem we need to decide on what the core experience will be. Will it be a text box or select box? On balance, it seems prudent for us to use the select box. But you may take a different tact depending on the context of your own problem.

Here's what this looks before it's enhanced:

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

This may look complicated but there are just 3 main elements:

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

The Javascript is quite complicated but here's a run down of what it needs to do:

- listen to keyup events on the text box.
- display options if and when they match
- if an option matches exactly hide options
- ...and when displaying options apply attributes as per above
- if user types down/up arrows highlight the option
- ...and mark attribues as per above
- if highlighted on option, and user presses spacebar/return put the value in the textbox
- if the user taps/clicks an option, put value in text box, and move focus back to the text box.

I have prototyped a version of this based on GDS's Accessible Typeahead[^]&mdash;theirs is based on Leonie Watson's accessible Autocomplete[^]. Feel free to use and check any of these out or build your own if needed using the above specification to guide you.

## Choosing dates

Dates are hard[^]. There is no getting away from this fact. Different time zones, formats, delimitters, days in the month, length of a year. It's hard work to design all of this complexity out of a UI.

Traditionally and sometimes still to this day, websites use three select boxes; one for day, month and year. We already know select boxes are problematic anyway, but one of their qualities is that they stop the user entering wrong information.

With dates this isn't the case as users can select *31 February 2016*, for example resulting in an error, that needs handling. This is just one of many problems.

So why did websites and why do some websites still use select boxes, instead of a simple textbox. Mostly because it stops the system needing to handle a plethora of different formats. Some dates start with month first, some with day. Some delimit dates with slashes; others with dashes. It's actually really hard to know.

But I digress too early. Before we know how to design a date form control, we first need to understand what type of date we're asking for. Fortunately, GDS has done the hard work for us. They say *the way you should ask users for dates depends on the types of date you’re asking for*.

Here are four examples they use to demonstrate:

- memorable dates (for example, date of birth or marriage)
- dates from documents or cards (for example, a passport or credit card)
- approximate dates (like ‘June 1983’)
- relative dates (like ‘4 days from today’)


- types of dates. Must know what, before we know how
  - dob
  - flight
  - other eg
- picking dates natively
  - Why native is best where possible, and that's the approach we have taken so far. Little enhancements, but mostly just beautiful semantic performant html.
  - mobile support
  - desktop support
  - creating our own (feature detect, design, no overlay, button, a11y, size of choices). If need OTHER EG and custom behaviour OR user testing shows that on mobile and desktop our own implementation is better, just remove the IF SUPPORTS BIT. Done.
  - those that get the native date enhancement
    - style better
    - turn off shit with css
  - those which get degraded text box
    - strict formatting where necessary...
    - good hint
    - good error messaging (good for everyone else too)
	- With design there is always a tradeoff. I consider myself to care about everyone, but when you design for everyone you may end up designing for noone. For example, someone who considers themself an "intellect" may love reading complex high brow paragraphs of text, but we know that hemmingway says we should write for grade 6 or less if possible because it's easy to read for everyone. Can't please em all.

---

They just don't work. From our previous analysis, we know that select boxes aren't any good anyway. But the whole point of a select box, is that the user selects from a set of valid options. With dates this is not the case.

This is because some months have 31 days, others 30 and of course February. Oh damn you February. So unique and so painful you are. As I write this memories come flooding back. There is also leap years to contend with.

We didn't want to use a text box because we didn't know what users would type, in what format and with what delimitter: a dash, a period, a slash, or no delimitter at all. Then there is internationalisation. Some people write dates with the month first, some with the day.

HTML5 introduced the date input. This is particularly useful on mobile browsers, where it will show the native date UI control, making it familiar, and quite well suited. Also, if the manufacturers make improvements our forms will get the same treatment, with no effort on our part whatsoever. Lastly, when supported, we don't need any heavy and difficult to create custom components. Native components are typically fast and accessible by default.

Here's the screenshots for what this looks like on mobile:

![Mobile native date control](./images/mobile-date.png)

Desktop browsers are different and have less support. Chrome and Edge work pretty well but Firefox, for example, doesn't have any support. Here's the widget on desktop Chrome:

![Desktop native date control](./images/desktop-date.png)

It all seems a bit messy doesn't it? But with all of these things we need to step through this information slowly.

GDS have done a lot of research with regard to asking for dates&mdash;in particular people's birth date. Most users intuively know this information&mdash;picking a date picker is often slower and more problematic than typing numbers into a text box without any further assistance.

GDS advise three *separate* boxes for day, month and year, to avert the delimitter and internationalisation problems we discussed earlier. Here's what it looks like:

![GDS date of birth](./images/gds-dob.png)

It works well, and the research I've been apart of has backed this position up.

However, as with any other design problem, context is important. Our flight booking system isn't asking for a date of birth. It's asking for a flight  date in the future, exact or approximate, and one that is often informed by the day of the week in which that date lands.

Offering users a date picker seems sensible. Afterall, we already know how hard it is to create a custom component and incurred performance cost. We also know the native date pickers are immune to these particualr issues. So we'll use one.

```HTML
<div>
	<label for="">Flight date</label>
	<input type="date">
</div>
```

Webkit browsers allow us to style native bits of the control using the following psedo selectors:

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

We know that widgets don't need to look the same cross browser[^] so our only remaining concern is what to do with browsers that don't support it.

We can feature detect support for it as follows:

```Javascript
function supportsDateInput() {
	var el = document.createElement('input')
	el.type = "date"
	return typeof el.type == "date";
}

if(!supportsDateInput()) {
	// create and use custom Javascript date picker widget
}
```

If the browser doesn't support the native date input, we can create a custom widget instead. You can either write your own, or use an existing component, for which there are many to choose from[^exampledatepickers]).

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
