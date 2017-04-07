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

This option is good when the amount of search options is completely dynamic and vast in size and breadth (like Amazon). But for this service, we have a finite amount destinations that we know confidently.

Why let users search, unassisted, in order to arrive at a page that states something along the lines of *we don't fly to that destination*?

In many respects we have regressed from the select box option above. At least users could guarantee a positive result. We can do better.

### Typeahead combobox

What we really need is a textbox and select menu rolled into one. Up until recently there has been no such element for us to use. HTML5 gave us the promising `datalist` but unfortuntely, it's buggy[^caniuse].

Because of this we need to write our own custom form control using Javascript. I will provide solutions to this problem later, but we should first understand the task we face when we embark upon creating our own custom and inclusive form component.

Starting with a baseline experience, for those without Javascript capabilities, we need to offer our users either a text box or select box. As discussed earlier I think on balance it's better to go with a select box but depending on your situation a text box may be fine.

When our script executes successfully we'll need to to the following:

1. Hide the select box.
2. Create a text box with an `id` that matches the select box label.
3. Works with the keyboard.
4. Works with screen readers.

Code:

<div class="typeahead-wrapper">
	<input class="typeahead-hint" readonly="true" tabindex="-1">
	<input
		type="text"
		name="input-typeahead"
		id="typeahead-default"
		autocomplete="off"
		role="combobox"
		aria-owns="typeahead-default-listbox"
		aria-autocomplete="list"
	>
	<ul
		class="typeahead-menu typeahead-menu--hidden"
		id="typeahead-default-listbox"
		role="listbox"
		>
		<li id="typeahead-default-option--0" role="option" tabindex="-1">
			France
		</li>
		<li id="typeahead-default-option--1" role="option" tabindex="-1" aria-selected="true">
			Germany
		</li>
	</ul>
	<div aria-live="polite" role="status" style="hidden stuff">
		<span></span>
	</div>
</div>

1. We give the textbox a role of `combobox` so that assistive devices know that it is a type-ahead control. Not just a plain textbox or select box.

2. We give the suggestions a role of `listbox` and associate the suggestions with the combobox text control using `aria-owns`.

3. Each suggestion has a role of `option`. And each option has an `id` which is used to identify the currently active option with the combobox using `aria-activedescendant`.

4. There is also a dedicated live region with a role of `status` to announce updates to screen readers. For example *2 results are available. France (1 of 2) is selected*.

5. The extra input is used to *perfectly display a grey hint in the input field*.

6. We give the text control an attribute of aria-autocomplete of `list` which means *a list of choices appears from which the user can choose.*

7. We use aria-expanded to indicate whether the element, or another grouping element it controls, is currently expanded or collapsed. For us it's the *grouping element it controls* as it will indicate whether the suggestions are showing or not.

8. In combination with #7 we give the option aria-selected="true".

9. We use autocomplete=off to stop browser interfering with our component.

---

Theo notes and questions:
-The reason it has to empty itself is because I didn’t know about aria-atomic http://pauljadam.com/demos/aria-atomic-relevant.html
-JS OPTION: The autoselect property will select the first option whenever the user types in something that produces results.

1. Why ios check? To prevent closing the menu when users dismiss the on-screen keyboard. I spent a few days trying to come with alternative methods (lots involving checking the size of the browser window) but since iOS 10 I don’t think there is an alternative way, which is very frustrating
2. We can do the space thing. When focused on suggestions (not on text control) pressing space should select the option. So should pressing enter (like u said). When in the text control, space should be a normal space and enter should submit the form entirely.

Bits:
- http://ljwatson.github.io/design-patterns/autocomplete/js/autocomplete.js
- https://alphagov.github.io/accessible-typeahead/examples/
- https://github.com/alphagov/accessible-typeahead/blob/dff68ee25fe0c346f410f353035b23d721949ee3/accessibility-criteria.md

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