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

This may look complicated but there are just three main elements:

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

Dates are hard[^]. There is no getting away from this fact. Different time zones, formats, delimitters, days in the month, length of a year, day light savings and on and on. It's hard work designing all of this complexity out of a UI.

Traditionally, and sometimes still to this day, websites use three select boxes; one for day, month and year. We already know select boxes are problematic anyway, but one of their qualities is that they stop the user entering wrong information.

However, this quality doesn't apply to picking dates. This is because users can, for example, select *31 February 2016*, which results in a validation error. This is just one of many problems.

So why use select boxes, instead of a simple textbox? Mostly because it stops the system needing to handle a plethora of different formats. Some dates start with month; others with day. Some delimit dates with slashes; others with dashes. It's actually really hard to know. But I digress.

Before we can design a date control, we first need to understand what type of date we're asking for. Like GDS says *the way you should ask users for dates depends on the types of date you’re asking for*.

### Dates from documents

We already discussed this in *Checkout* when we designed the expiry date field. If you're not reading this book in order, feel free to jump back there now.

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

### Booking dates

A user might want to choose a date having been informed by price, availability and knowing which day of the week it relates to. It's the latter that is of most concern to us. People often orientate themselves by day and week when booking holidays. For this reason, we'll want to offer users a more convenient way of choosing a date.

UIs that try to solve several problems at once are often detrimental to the resulting user experience. For example, if we were to try and convey price and availability inside a calendar widget, from which to choose a date, it would end up being full of information that's hard to process.

Because of this, we'll simply let users pick a date preference in which to fly. We can help them decide which date to choose (based on price for example) in later steps.

### Calendar controls

Up until recently, we were left to create our own custom calendar control using Javascript. We already know how much effort it is to produce a user-friendly custom component that's inclusive. Wherever possible, we should let the browser do the work for us.

HTML5 gave us `input type="date"` which enhances a text box into a calendar control. This is good because they:

- save us time and money in design and implementation costs
- are performant (as users don't need to download and execute custom components)
- are familiar because native apps use the same UI control
- are normally accessible by default

Also, when browser improve the design of the native components, our users get them too without waiting for us to upgrade our own code. This frees us up to solve other problems instead.

So let's talk support. Mobile browser support is good. Here's what it looks like:

![Mobile native date control](./images/mobile-date.png)

Desktop browser support is not as good. Chrome and Edge work well but Firefox, for example, doesn't have any support, although I hear it's on it's way. Here's what it looks like for Chrome:

![Desktop native date control](./images/desktop-date.png)

If you're concerned about differences in appearance across different platforms and browsers. Don't be. User's either don't notice or don't care. And if you're not convinced you should watch Nicholas Zakas's video[^] and have a read of this website [^dowebsitesneedtolookthesame].

Here's the HTML:

```HTML
<div>
	<label for="date">Flight date</label>
	<input type="date" name="date" id="date">
</div>
```

We do have one remaining issue to consider: browsers that don't support `input type="date"`*.

The first thing we're going to want to do is detect support:

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

The great thing about Progressive Enhancement, is that we don't actually have to do anything. The user can still type a date, it's just not as easy as it could be. Or we can use or write some code that creates a custom built calendar widget. There are many to choose from[^exampledatepickers].

And lastly, if testing shows that the custom widget performs better just remove the conditional feature detection and you're component will execute for everyone.

#### Writing your own

If the available widgets aren't suitable, here's a brief run down of things to consider:

- Don't use an overlay. Put it inline. Why make someone click to reveal if most people are going to do that anyway. GDS suggestion. Lot's of advantages. Less complex, less chance of stuff hiding off screen (position: absolute woes) etc.
- Use a button to trigger it's display
- Make the choices easily clickable and tappable for those with fine motor issues.
- Use a table for the grid to organise the days of the month to give context
- Only make one day tabable.
- Make back/previous buttons to be naturally focusable.
- up/down/left/right for dates, and continuation

Visual users may benefit from Cal. Not so much users of screen readers. We can still do something though.https://ux.stackexchange.com/questions/60884/best-way-for-date-field-for-visually-impaired-users

---

SPEC: https://www.w3.org/TR/2009/WD-wai-aria-practices-20090224/

- picking dates natively
  - those that get the native date enhancement
    - style better
    - turn off shit with css
  - those which get degraded text box
    - strict formatting where necessary...
    - good hint
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
