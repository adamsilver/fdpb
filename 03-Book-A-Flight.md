# Book A Flight

In only two chapters you'd be forgiven for thinking *there can't be too much more to know*. You'd be forgiven but you wouldn't be right. We're going to continue our adventure by analysing and designing a flight booking system.

On the face of it, this is a niche problem&mdash;certainly less common than a Registration and Checkout forms. However, every topic within this chapter is an interesting one with tips that we can use in other forms.

Booking a flight is a relatively long and complicated process. For brevity we'll simplify the flow a little bit. Our flow will be as follows:

1. Choose origin/destination
3. Choose departure/return date
5. Choose passengers
6. Confirming flight
7. Choosing where to sit
8. Payment

## Choose desination

The first thing users need to do is select a destination (and an origin). Without this information we can't offer the user any flights. The destination field could be a:

- radio button group
- select box
- text box (`input type="search"`)

### Radio Button Group

We could use radio buttons but there are hundreds, if not thousands of destinations. It would be a huge page with a lot of scrolling. One additional advantage that radio buttons possess is the ability to use use `cmd+f` to search the page for text strings.

However, most users won't know about this feature, not all browsers support it and ultimately, we shouldn't rely on an inconspicuous browser feature to fix issues in our design.

We can do better.

### Select Box

Designers often use `select` boxes because they save space. However, as Alice Bartlett explains in her talk Burn Your Select Tags, they should be avoided where possible.

This is because:

- some users are unable to close the select
- some users try to type into a select
- some users confused focused options with selected ones
- users aren't able to pinch-zoom options on devices
- they have limited hierarchy control
- they hide choices behind an unnecessary extra click
- they are not easily searchable

### Text box

A text box (`input type="search"`) is another option. The search type enhances the functionality of a regular text box (`input type="text"`) by allowing the user to clear the contents of the field&mdash;either by tapping the X or pressing escape.

![Image here](/etc/)

HTML:

```html
	<div>
		<label for="destination">Destination</label>
		<input type="search" name="destination" id="destination">
	</div>
```

This option is good when the amount of search options is completely dynamic and vast in size and breadth (like Amazon for example). But for this service, we have a finite amount of destinations that we know in advance.

If we were to let users search, unassisted, they would arrive at a page with a message of *we don't fly to that destination*. In doing this we have regressed the experience form a native select box&mdash;at least users could guarantee a positive result.

Let's keep going.

### Typeahead combobox

What we really need is a text box and select menu rolled into one. As the user types a destination, suggestions appear beneath allowing them to autocomplete the field. This drastically saves time scrolling through a plethora of destinations.

Up until recently there has been no such element for us to use. HTML5 gave us `datalist` but unfortuntely, it's signifcantly buggy[^caniuse].

As we've done the hard work in analysing what is best for users, we're left to build a custom component using Javascript. In doing so we need to adhere to a few important rules:

- must be focusable with keyboard
- must be operable with keyboard
- must expose it's values and state to acessibility APIs
- must work without Javascript

To solve the last problem we need to choose from the aformentioned text box or select box. On balance, it seems prudent for us to use the select box. But you may take a different tact depending on your exact problem domain.

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
		aria-owns="destination-listbox"
		aria-autocomplete="list"
		aria-expanded="true"
		class="combobox-textbox"
	>
	<ul
		id="destination-listbox"
		role="listbox"
		class="combobox-menu combobox-menu-isHidden"
		>
		<li
			id="combobox-menuOption--0"
			role="option"
			tabindex="-1">
			France
		</li>
		<li
			id="combobox-menuOption--1"
			role="option"
			tabindex="-1"
			aria-selected="true">
			Germany
		</li>
	</ul>
	<div
		aria-live="polite"
		role="status"
		class="combobox-liveRegion">
	</div>
</div>
```

This may look complicated but let's break it down and explain what's going on. There are four major HTML parts:

1. The text box
3. A menu
4. A live region

This HTML, in combination with CSS and Javascript will display suggestions beneath the text box as the user types. All the attributes are necessary in order to build an inclusive component that users can use with their mouse, (on-screen) keyboard and screen readers.

Here's a brief run down:

- The text box has a `role` of `combobox` so that assistive devices know that it's not a regular text box or select box. And `aria-autocomplete` attribute is set to `list` which means *a list of choices appears from which the user can choose*. `aria-expanded` indicates the menu is in an expanded or collapsed state. `autocomplete` is set to `off` to stop the browser providing its own suggestions and interfering with ours.

- We give the menu a role of `listbox` and associate it with the combobox control using `aria-owns`.

- Each option within the menu is given a `role` of `option`. And each option has an `id` which is used to identify which is active using `aria-activedescendant`. And `aria-selected` indicates which option is active.

- The `div` at the bottom is a `live region` with a `role` of `status` to announce changes as the user types. For example, *2 results are available. France (1 of 2) is selected*.

TODO: JS?

I have prototyped my own version of this which is based on GDS's Accessible Typeahead[^]&mdash;theirs is based on Leonie Watson's accessible Autocomplete[^]. Feel free to use and check any of these out or build your own if need be using the above specification to guide you.

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
