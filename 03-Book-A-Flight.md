# Book a flight

In the first two chapters we covered a lot of ground. If we were to finish our exploration here and implemented what we know so far, our users would certainly be happier.

However, we're not going to finish here. We're going to tackle a very interesting, more niche problem. We're going to design a flight booking form.

Booking a flight is a complicated process. For brevity we'll simplify the flow, but this won't take away the primary points. Our flow will be as follows:

1. Choose origin
2. Choose destination
3. Choose departure date
4. Choose return date
5. Choose passengers
6. Confirming flight
7. Choosing a seat
8. Payment

## Choose desination

The first thing users need to do is select a destination (and an origin). Without this information we can't offer the user any flights. The destination field could be a:

1. radio buttons
2. select box
3. text box (`input type="search"`)

### Radio buttons

We could use radio buttons but there are hundreds, if not thousands of destinations. It would be a huge page with a lot of scrolling. The good thing about radio buttons is that we could use `cmd+f` but:

a) many users won't know about it's existence;
b) some browsers don't expose this capability particularly on mobile; and
c) ultimately, we shouldn't rely on such an inconspicuous browser feature to fix issues with our own design. We can do better.

### Select box

We could use a select box but like radio buttons they suffer from lots of scrolling and come with some additional problems.

In her talk Burn Your Select Tags[^] Alice Bartlett explains that select boxes should be avoided. Luke Wobrelski echos this sentiment in his article Dropdown Menus Are A Last Resort[^lukew]. This is because:

a) They hide choices and require a click to see them;
b) Some devices suppress the zoom of `option` overlays; and
c) They aren't generally well understood.

This doesn't mean we should never ever use them, but we should do so consciously and when we have analysed all the options available to us. In this chapter, this is what we're going to do.

### Text box (`input type="search"`)

A regular text box (`input type="text"`) is an option for us but we could enhance the experience by using a search box (`input type="search"`). In doing so the browser allows the user to clear the field more easily, by tapping on the "x" or pressing *escape*.

![Image here](/etc/)

HTML:

```html
	<div>
		<label for="destination">Destination</label>
		<input type="search" name="destination" id="destination">
	</div>
```

This option is good when the amount of search options is completely dynamic and vast in size and breadth (like Amazon for example). But for this service, we have a finite amount of destinations that we know confidently.

Why let users search, unassisted, in order to arrive at a page that states something along the lines of *we don't fly to that destination*?

In many respects we have regressed from the select box option above. At least users could guarantee a positive result. We can do better.

### Typeahead combobox

What we really need is a textbox and select menu rolled into one. As the user types a destination suggestions will appear beneath allowing the user to autocomplete their input. This drastically saves time scrolling through a plethora of destinations.

Up until recently there has been no such element for us to use. HTML5 gave us the promising `datalist` but unfortuntely, it's signifcantly buggy[^caniuse].

So we're left to build our own custom form component using Javascript. I will provide solutions to this problem later, but we should first understand the task we face when we embark upon creating our own custom and inclusive form component.

As discussed in A Registration Form, we'll need to design a core experience for those without Javascript. Our options are those we discussed above: a text box or a select box.

On balance and for our given problem, it seems prudent to use the select box. But you may take a different tact depending on your exact problem domain.

Here is our destination control before we have implemented our custom Javascript combobox:

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

Here is custom combobox after Javascript kicks in:

![Image here](/etc/)

HTML:

```html
<div class="combobox">
	<input class="combobox-hint" readonly="true" tabindex="-1">
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
2. A hidden text box
3. A menu
4. A live region

This HTML, in combination with CSS and Javascript will display suggestions beneath the text box as the user types. All those attributes are necessary in order to build an inclusive component that users can use with their mouse, (on-screen) keyboard and screen readers.

Here's a brief run down:

1. The textbox has a `role` of `combobox` so that assistive devices know that it's not a regular textbox or select box. And `aria-autocomplete` attribute is set to `list` which means *a list of choices appears from which the user can choose*. `aria-expanded` indicates the menu is in an expanded or collapsed state. `autocomplete` is set to `off` to stop the browser providing its own suggestions and interfering with ours.

2. We give the menu a role of `listbox` and associate it with the combobox control using `aria-owns`.

3. Each option within the menu is given a `role` of `option`. And each option has an `id` which is used to identify which is active using `aria-activedescendant`. And `aria-selected` indicates which option is active.

4. The `div` at the bottom is a `live region` with a `role` of `status` to announce changes as the user types. For example, *2 results are available. France (1 of 2) is selected*.

5. The extra input is used to *perfectly display a grey hint in the input field* with CSS.

I built my own version[^] of this drawing on both GDS's *Accessible Typeahead*[^] of which they used Leonie Watson's accessible Autocomplete[^]. Feel free to use and check any of these out or build your own if need be using the above specification to guide you.

## Choosing a date

- https://medium.com/samsung-internet-dev/making-input-type-date-complicated-a544fd27c45a

### Different types of date

- Date of birth
- dates in the future
- approximate dates
- etc
- always in advance, return date after outward date

### Using native datepicker

### When not to use a native datepicker

### Feature detection

## Choosing passengers

- Number field
- Stepper
- Not using select
- making it clear what an adult is. easyjet in brackets.

## Confirming a flight

- Radios again?

## Choosing a seat

- Checkboxes
- Nested fieldset and then don't need appointment booking?
- a11y

## Footnotes

[^luke]:(http://www.lukew.com/ff/entry.asp?1950)
[^]:(https://www.nngroup.com/articles/drop-down-menus-use-sparingly/)
[^]:(https://www.slideshare.net/cjforms/design-patterns-in-government-2016)
[^buggy]:(http://caniuse.com/#feat=datalist)
[^GDS type]:(https://alphagov.github.io/accessible-typeahead/)
[^leonie]:(http://ljwatson.github.io/design-patterns/autocomplete/)

---

## Typeahead outstanding notes

- Why ios check? To prevent closing the menu when users dismiss the on-screen keyboard. I spent a few days trying to come with alternative methods (lots involving checking the size of the browser window) but since iOS 10 I don’t think there is an alternative way, which is very frustrating
- https://github.com/alphagov/accessible-typeahead/blob/dff68ee25fe0c346f410f353035b23d721949ee3/accessibility-criteria.md
