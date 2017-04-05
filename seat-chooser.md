# Book a flight

In the first two chapters we covered a lot of ground. Even if we finished our exploration here and implemented what we now know we would be in a good position and our users would thank us.

But we certainly haven't covered everything. In this chapter we're going to discuss and analyse some lesser known but vitally important form patterns. And we're going to do this by designing a flight booking system.

Booking a flight is a complex process. Because of this we're going to break it down and use One Thing Per Page once again. But we're not going to cover this aspect again. What's important is what we do on each screen.

For brevity we'll also simplify the flow a little by not offering, for example *one way* tickets.

Our flow will be as follows:

1. Choose origin
2. Choose destination
3. Choose departure date
4. Choose return date
5. Choose passengers
6. Confirming flight
7. Choosing a seat
8. Payment

Let's dive in there is a lot to discuss.

## Choose desination

The first thing users need to do is select a destination (and an origin). Without this information we can't offer the user any flights. There is a few things to discuss.

We could design the destination field to be one of many types of control.

1. Radio button group
2. Select menu
3. Text box (or type=search)

### Radio buttons

We could use radio buttons but there are hundreds, if not thousands of destinations. It would be a huge page. The good thing about radio buttons is that we could use `cmd+f` but:

a) many users won't know about it's existence; 
b) some browsers don't expose this capability particularly on mobile; and 
c) ultimately, we shouldn't really on such an inconspicuous browser feature plug UX holes in our own design. We can surely do better.

### Select menu

In her talk Burn Your Select Tags[^] Alice Bartlett explains that select menus should be avoided wherever possible. This is something that Luke W also states in Dropdown menus are last resort[^lukew]. This is because:

a) They hide choices and require a click just to see them;
b) Some devices suppress the zoom of `option` overlays; and
c) They aren't generally well understood.

This is not about never using them, but use them sparingly. Fortunately, they are native and so they have a lot of accessibility features built in. But as choosing a destination is critical we owe it to ourselves to explore this further.

### Text box (search)

We can use a regular text box, and allow users to search for a destination. We can use `type=search` which is a slightly-enhanced text box where:

- You get a 'X' symbol at the end of the input/search box to clear texts in the box; and
- Pressing 'Esc' key on keyboard also clears texts

The problem with this is that:

a) we don't allow users to select; meaning that they may search and get no results
b) searching, takes relatively a long time to perform requiring a server round trip that again may result in no results (see A)).

Again we can do better, particularly as an enhancement.

### Typeahead

What we really need is a textbox and select menu rolled into one. In HTML5 there is the datalist but it is extremely buggy[^caniuse] otherwise I would most certainly be advising it's use.

Because of that we need to use a relatively hefty amount of Javascript and ensure we don't lose accessibility needs in the process.

More importantly than the code itself is the requirements. GDS have an accessible typeahead in progress.

Before we enhance our field with a typeahead though we first need to determine which of the above is our baseline experience. Either a text box or a select menu. Based on the above and your own situation I leave that as your decision.

Whichever way we go, we'll need to ensure that once we enhance it with script, that it becomes a text box.

When the user types we'll need to reveal the results and expose those appropriately through ARIA attributes. And we'll need to ensure all of this works with the keyboard.

Code:

<div class="typeahead-wrapper">
	<input class="typeahead-hint" readonly="true" tabindex="-1">
	<input 
		aria-owns="typeahead-default-listbox" 
		autocomplete="off" 
		class="typeahead-input" 
		id="typeahead-default" 
		name="input-typeahead" 
		role="combobox" 
		type="text"
	>
	<ul 
		class="typeahead-menu typeahead-menu--hidden" 
		id="typeahead-default-listbox" 
		role="listbox"
		>
		<li id="typeahead-default-option--0" role="option" tabindex="-1">
			France
		</li>
		<li id="typeahead-default-option--1" role="option" tabindex="-1">
			Germany
		</li>
	</ul>
	<div aria-live="polite" role="status" style="border: 0px; clip: rect(0px 0px 0px 0px); height: 1px; margin-bottom: -1px; margin-right: -1px; overflow: hidden; padding: 0px; position: absolute; white-space: nowrap; width: 1px;">
		<span></span>
	</div>
</div>

The reason it has to empty itself is because I didn’t know about aria-atomic http://pauljadam.com/demos/aria-atomic-relevant.html

[3:30]  
The autoselect property will select the first option whenever the user types in something that produces results

[3:31]  
The extra input is used to perfectly display a grey hint in the input field

- role=combobox
- aria-owns
- role=listbox
- role=option
- aria-live

Questions:
1. Why ios check?
2. Why aria atomic?

Bits:
- https://alphagov.github.io/accessible-typeahead/examples/
- https://github.com/alphagov/accessible-typeahead/blob/dff68ee25fe0c346f410f353035b23d721949ee3/accessibility-criteria.md

## Choosing a date

- https://medium.com/samsung-internet-dev/making-input-type-date-complicated-a544fd27c45a

### Different types of date

- Date of birth
- dates in the future
- approximate dates
- etc

### Using native datepicker

### When not to use a native datepicker

### Feature detection

## Choosing passengers

- Number field
- Stepper
- Not using select

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