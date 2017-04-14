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

- a
- b
- c

I have prototyped a version of this based on GDS's Accessible Typeahead[^]&mdash;theirs is based on Leonie Watson's accessible Autocomplete[^]. Feel free to use and check any of these out or build your own if needed using the above specification to guide you.

## Choosing a date

In his article Making Input Type Date Complicated[^PPK], PPK says that all too often designers and developers make things complicated when it comes to offering users the ability to enter a date. The solution he offers is a simple one. Use HTML5's `input type="date"`.

![What it looks like](/etc/)

This is because:

- It's free
- Doesn't need any extra code (performant)
- Enhances for free
- Matches the device date picker improving familiarity
- Yada
- Bada
- Good for keyboard
- Good for mouse
- etc

I agree with PPK. It has many advantages and it's good place to start. But most design decisions don't adhere to a blanket rule. Context is everything. We can't design a great experience before first asking ourselves *what type of date are we asking users for?*

The answer to this question most definitely influences the solution. For example, if we're asking for a date of birth, a date picker might be a bit clunky. Most people know their date of birth and typing it in directly is faster than scrolling; and then choosing the day, month and year separately.

This is why GDS suggests the following:

![Image here](/etc/)

Each segment has it's own text box and clear labelling, drastically reducing the chance of errors and having to work out the localised format. For example an American date puts the month before the day.

TODO: More reasons

Here's another example. Say the user wants to choose an approximate date. Perhaps they need to take some holiday during the summer holiday perid. In this case the availability and price may well influence the date they pick. GDS's solution maybe less useful here.

The native date picker does have some shortcomings that we should be aware of. It doesn't allow us to disable dates. Such as those that are in the past, or those that are *sold out*. In short, and as is often the case, it depends.

With all that said, the native date field does make sense in our case. If the user chooses a date in the past, we can display an error quickly. And seeing as the user is *searching* for flights, if there are none we have an opportunity to say that and display the nearest dates all within a dedicated page of its own.

Cramming this information inside a date picker is hard to design and hard for users to intepret.

Here's the code:

```HTML
<div>
	<label for="">Flight date</label>
	<input type="date">
</div>
```

Here's what it looks like:

![What it looks like](/etc/)

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

---

## Typeahead outstanding notes

- Why ios check? To prevent closing the menu when users dismiss the on-screen keyboard. I spent a few days trying to come with alternative methods (lots involving checking the size of the browser window) but since iOS 10 I don’t think there is an alternative way, which is very frustrating
- https://github.com/alphagov/accessible-typeahead/blob/dff68ee25fe0c346f410f353035b23d721949ee3/accessibility-criteria.md
- ios check, capture+ blur thinger.
