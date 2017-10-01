# An inbox

My sister loves to-do lists. In fact, she loves them so much, that one of her favourite things is making new lists out of old ones. The world is full of lists. There is even a list of great people[^1]. On the web, there are several types of lists, and there are some design patterns that have emerged over the years that help to manage them.

In this chapter, we'll look at an inbox. That is, a list of emails sent from other people. In many respects, an inbox is a list of tasks represented as emails. Besides reading and replying to them, the aim is to achieve a zen-like state of Inbox Zero[^2]. To get there quickly, we will design the interface to let users delete, archive and mark emails as spam. But not just one at a time—in bulk. My sister loves her trusty pen and paper, but if we do this right, perhaps we can convert her to digital.

## List types

First, we're going to look at how best to mark-up a list of emails. Discussing lists may seem out of place in a book about forms, but forms rarely form part of an interface on their own. Ignoring their surroundings can result in disagreeable experiences.

The meaning—or semantics—behind elements should drive their visual design. Put simply, things should look like they function. There are 4 elements we can use to construct lists each with different semantics: description lists, tables, ordered lists and unordered lists. Let's discuss the pros and cons of each now.

### Description lists

A description list (`<dl>`), formerly called a definition list, is for grouping a related group of key-value pairs for *one* record. For example, the key details of a product such as size, price and material. An inbox, however, consists of multiple records (emails), ruling this one out.

```HTML
<dl>
	<dt>Size:</dt>
	<dd>250cm x 135cm x 90cm</dd>
	<dt>Price:</dt>
	<dd>£429.95</dd>
	<dt>Material:</dt>
	<dd>Reclaimed teak</dd>
</dl>
```

### Tables

A table (`<table>`) is an arrangement of data, typically laid out in rows and columns. Like a spreadsheet, they are particularly suited when the data needs to be compared and sorted. And numbers totalled at the bottom too.

Tables unfortunately, are difficult to style on mobile, because there is no room to show more than two or three columns. Even then, it could be a squeeze depending on the data, creating layout issues. Content could wrap prefusly and could force a horizontal scroll bar to appear.

Making tables responsive isn't the most straightforward thing to do. This is because they are inherently tied to the way they look. Put another way, making a table not look like a table, is not only very difficult, but it would be deceptive and counterproductive as we'll see in a moment.

Gmail uses tables and puts recipient, subject and date sent into columns. Interestingly though, there are no headings, which is a clue that tables have been used for layout purposes rather than their semantic qualities. Jeremy Keith talks about the idea of material dishonesty is in his book Resilient Web Design[^]:

> Using TABLEs for layout is materially dishonest. The TABLE element is intended for marking up the structure of tabular data. The end result [...] is a façade. At first glance everything looks fine, but it won’t stand up to scrutiny. As soon as such a website is stress‐tested by actual usage across a range of browsers, the façade crumbles.

See the following table mark-up as an example of this. The `<tr>` is wrapped in an `<a>` to let users read the email. The problem is that browsers ignore this code. It's simply not allowed. Gmail fixes this by using Javascript, but not everyone has Javascript and frankly, it's unnecessary. We'll discuss a better approach next.

```HTML
<table class="inbox">
  <a href="/email/1">
    <tr>
      <td>John Oates</td>
      <td>Your Amazon.co.uk order #123 is out for delivery</td>
      <td>10 August</td>
    </tr>
  </a>
</table>
```

### Ordered and unordered lists

The generic list is useful because it itemises its content accessibly and groups related content into bite-size chunks. But they can be used for more than just bullet points. They come in two flavours: ordered (`<ol>`) and unordered (`<ul>`) lists. And they are far less opiniated than tables.

The difference between the two types lies in their name. If order matters, use an ordered list. For example, following a recipe's instructions requires doing so in order. Not following them in order matters greatly. On the other hand, an inbox doesn't have to be read or actioned in a predefined order. It sounds simple when put like that, but we tend to overthink this sort of thing. Let's lay out the inbox using a `<ul>`.

```HTML
<ul class="inbox">
	<li>
		<a href="/emails/1/">
			<div class="inbox-recipient">John Oates</div>
			<div class="inbox-subject">Your Amazon.co.uk order #123 is out for delivery</div>
			<div class="inbox-date">10 August</div>
		</a>
	</li>
</ul>
```

Unlike tables, unordered lists are stylistically malleable and therefore responsive. That's because we can put whatever structures we want inside each list item. So, on big screens we can lay things out in columns and on small screens we can stack them nicely. Moreover, the entire list item can be made to be clickable without resorting to Javascript hacks. Wrapping a link around the contents is perfectly valid. Less work and fewer problems.

In the case of an inbox, list items are more suited anyway: not only are column headings redundant, but there's no need to compare the items in the list.

## Marking email for action

To let users select emails, we'll need to add checkboxes. Let's do that now.

```
<ul class="inbox">
	<li>
		<input type="checkbox" name="email">
		<a href="/emails/1/">
			<div class="inbox-recipient">John Oates</div>
			<div class="inbox-subject">Your Amazon.co.uk order #123 is out for delivery</div>
			<div class="inbox-date">10 Aug</div>
		</a>
	</li>
	...
</ul>
```

Note the checkbox is missing a label. The problem is that we want the contents of the link to be the contents of the label. In other words, two opposing behaviours need to occupy the same space. Clicking a label should mark the checkbox, clicking a link should take the user to the email. We're faced with a challenge.

![Illustrate the above with a useful caption](.)

It could be argued that the interface doesn't need a visual label. After all, the label would need to mirror, and therefore duplicate, the links content. This would make for a confusing experience for sighted users. Let's see if the concept of modes can solve this for us.

### Using modes

Trying to combine two user needs into a single interface is what's caused this design problem in the first place. One way to avoid the issue would be to split apart the two needs by using the concept of modes. This just means letting users switch between managing email and reading it by offering a link as shown below.

![Mode](.)

Clicking *Organise* puts users into *manage-mode*. When in manage-mode, the link changes to ‘Exit organise’ (or similar) and takes users back.

When in read-mode there are no checkboxes or any other form paraphernalia. The row is a link which takes users to read the email. When in manage-mode, the row turns into a `<label>`. When clicked, it marks (or unmarks) the checkbox like normal.

Modes are best suited when one mode is used more frequently than the other. However, when both are used frequently, as is the case of an inbox, having to switch back and forth all the time is undesirable.

### Visually hide the label

Instead of using modes, we can add a visually hidden label. There are two ways to do this. The first is to use an `aria-labelledby` attribute as shown below which uses the content that already exists. This means adding unique `id`s to make this work. In any case, ARIA shouldn't be used unless we have to&mdash;something we've talked about previously.

Alternatively, a standard `<label>` has better support. The downside, is that the content has to be duplicated. This isn't a huge performance problem, but if we're not careful, bloated HTML can eventually diminish the experience by causing some operations to take longer&mdash;screen reader software can be unresponsive, for example.

Fortunately, there is a silver lining in duplicating the contents. The contents of the label is just for screen readers, meaning we can craft a specific message that reads better audibly than it otherwise would visually.

```HTML
<!-- Using aria-labelledby -->
<input type="checkbox" name="email" aria-labelledby="e1_recipient e1_subject e1_date">
<a href="/emails/1/">
	<div class="inbox-recipient" id="e1_recipient">John Oates</div>
	<div class="inbox-subject" id="e1_subject">Your Amazon.co.uk order #123 is out for delivery</div>
	<div class="inbox-date" id="e1_date">10 Aug</div>
</a>

<!-- Using a standard label -->
<input type="checkbox" name="email" id="email1">
<label for="email1">From John Oates, subject ‘Your Amazon.co.uk order #123 is out for delivery’ (10 August 2017)</label>
<a href="/emails/1/">
	<div class="inbox-recipient">John Oates</div>
	<div class="inbox-subject">Your Amazon.co.uk order #123 is out for delivery</div>
	<div class="inbox-date">10 Aug</div>
</a>
```

### Highlighting marked emails

The deal with human-computer interaction is that when the human does something, the computer should respond. In this case, clicking a checkbox makes a little tick appear (and disappear) accordingly. As with every other checkbox in any other form, this is probably enough feedback.

![Hey I'm checked, oh dear nobody loves me](.)

It is, however, possible to highlight the entire row with CSS and Javascript. As designers, we're tempted to do more than the minimum. We think that more is better. We think that more is a symbol of hard work. It's actually a lot harder to *resist* doing more, than simply *doing* more. Constantly striving for less in a world that rewards you for doing more is very hard work indeed.

Mailchimp, known for their usability prowess, show that you don't necessarily need that extra enhancement. They rely solely on the checked state of the checkbox. One can assume their research showed this to be good enough. My own research aligns with this too. Obviously, if your research shows that highlighting the row is beneficial then by all means do so.

## An action menu

Letting users select multiple emails is all well and good, but we're going to want to facilitate actioning them too. Unlike the forms designed in previous chapters, this form contains multiple submit buttons with a new visual and interactive treatment that needs to be considered.

### Implicit submission and multiple submit buttons

Implicit submission lets you press <kbd>enter</kbd> when a field is focused. This is a convention that lets users submit a form quicker without having to move focus to the submit button. This is particularly useful for a search form&mdash;one that consists of a text box and submit button&mdash;because users can take the efficient route by pressing <kbd>enter</kbd>.

![Stuff](.)

The problem comes when a form has multiple submit buttons. If the user presses <kbd>enter</kbd>, which action will be taken? Browsers simply choose the first button in the document flow. Take a look at the following form. It has two submit buttons: delete and update. If the user implicitly submits the form the record will be deleted instead of updated, as the user intended.

![Update/delete](.)

To mitigate this, split the forms up into separate pages ensuring that there is one action per form. Admittedly, depending on the design this is not always easy, which is the case for the inbox. We could ask users to choose an action *before* selecting and actioning the emails, but this seems a little long winded.

![Click action link -> present checkboxes (with submit at bottom) ->confirm action](.)

Fortunately and by convention, multi-select interfaces typically place the action buttons at the top of the page in vertical alignment to the checkboxes. This gives users a chance to discover the multitude of available actions before making their selection.

It's also worth noting that a form consisting soley of checkboxes doesn't make implicit submission all that useful. If you need multiple submit buttons, put the least invasive action first&mdash;in this case *archive*. That way, in the unlikely that users accidentally submit the form implicitly they'll be in far less of a predicament.

The last thing we can do is offer users a way to *undo* their action. We'll discuss this in more detail later in ‘Success messages’.

### Sticky menus

Todo. Position: sticky enhancement. Pros and cons.

### Disabling and hiding buttons

Some multi-select interfaces choose to hide or disable the buttons until users select at least one item in the list. It could be argued that showing (or enabling) action buttons in response to checking a checkbox may help users take the next step more easily. In the case of hiding them, the interface becomes more streamlined and only shows the action buttons once they become relevant.

On the flipside, this only (and potentially) helps sighted users. And even so, they'd have to be using a large screen whereby the items and the menu are in full view. This is not a particularly inclusive approach to design. We also discussed the full range of problems associated with disabling buttons in ‘A Registration Form’. As a quick reminder, they don't let users know why they're disabled and they are not perceivable to screen readers.

Decluttering an interface is a noble goal, but never at the cost of clarity and inclusivity. Instead make room for the buttons early in the design process. Hiding functionality away from users and requiring them to perform an additional action to reveal that functonality should always be a last resort.

### Menu types

On big screens&mdash;or responsively speaking, when there is enough space to do so&mdash;we should just lay out the submit buttons to make them available to users at all times. In desktop viewports, there's rarely any reason to hide the buttons. However, the inbox could have other components such as a search bar or layout controls such as Gmail's *comfortable*, *cozy* and *compact* menu. This means there may not be enough space to show them comfortably.

Similarly, on small screens, if there are more than two or three buttons, they're likely to stack beneath each other. This in itself is not a huge problem except for the fact it pushes the main content down the page. Having the buttons dominate the interface like this is problematic. *Dominance* is a quality we should use sparingly. After all, if everything dominates, nothing does. The inbox itself should take center stage with the menu taking a back-seat role.

As noted earlier if there's enough room to lay out the submit buttons then do so. But if there isn't, we can keep the interface clean and easy-to-scan by hiding the options in a menu.

There are two ways to create a menu. First by using a standard `select` box and second by creating a custom menu component. Let's discuss the pros and cons of each next.

### A select box menu

Select boxes are a menu of sorts. In fact, select boxes, are also  referred to as drop-down *menus*.

Like a menu, they group similar items together that users can select. And they hide the items behind a click and keep the interface clean. They're an attractive option because, as we know, browsers supply them for free. However, even though select boxes look like menus and behave like them; and even though they are sometimes referred to as menus&mdash;they aren't true menus.

Select boxes are for input. That's why forms that contain select boxes&mdash;like any other input&mdash;must be accompanied by a submit button to submit the choice. Not only is this convention, but it's also in the Web Content Accessibility Guidelines (WCAG)[^4]:

> Changing the setting of any user interface component does not automatically cause a change of context.

The reason I bring this to your attention is because using a select box as a menu, usually means omitting the submit button and causing the form to submit `onchange` using Javascript. This goes against principle 4 by taking control *away* from the user.

Unsurprisingly, this causes problems for screen reader and keyboard users. For example, on Chrome (Windows), the form is submitted as soon as the user presses <kbd>down</kbd> to select the next option. Moving beyond that option is impossible.

![Illustrate this](.)

This is not a browser bug. It's just that some browsers are more forgiving than others. The forgiving ones only submit the form by pressing <kbd>space</kbd> or <kbd>enter</kbd>. Unfortunately, not all browsers are alike or implement the specification consistently. Ignoring people who use a less forgiving browser doesn't make the problem any less real for users.

The other problem with using a select box, is that it's always collapsed, even when there is enough space to lay out the options. One solution is to create a completely different component for big screens using Javascript. This is known as adapative (not responsive!) design, which we'll discuss next.

### Adaptive design versus responsive design

First a little history. When the web came along, we settled on 640px widths. Then a few years later, when larger monitors came to market, we changed to 960px widths. We no longer cared about people with small monitors.

Then more years passed. The mobile web was born. Or more accurately, we could use websites on our phones, which happen to have small screens. A million browsers came out. A million devices came out. And browsers gave us CSS media queries.

Accordingly, we started to design for 320px widths. Why? because many of us had iPhones and this happened to be its portrait width.

Then there was landscape view. Then tablet (is that mobile?). Then desktop. Then really big desktop screens. Then TV. Then all the way back down to wristwatches. If your head is spinning, don't worry, so is mine. This is the problem that responsive design solves and adaptive design exacerbates.

The difference between responsive and adaptive design is both subtle and crucial. Both techniques are based on viewport width. And both use CSS media queries to do so. Let's take a look at each in turn now.

Adaptive design means defining several predefined layouts for specificly chosen viewport widths that correspond to a particular device. All the code for all of the layouts are sent to the browser. Then the layout that matches the predefined media query is applied accordingly. These media queries are known as device breakpoints. That is, a breakpoint which is defined based on a device's width.

```CSS
iphoneLayout {

}

ipadLayout {

}

desktopLayout {

}
```

Using this approach is normally unnecessary and counterproductive. First, there is an endless stream of devices and browsers with different widths and it's only going to get harder. Designing specific layouts for every device width is sysiphean. And the extra code needed to produce such layouts would result in very slow loading pages. Generally speaking adapative design should be a last resort.

Responsive design takes a different approach. It's based on a single fluid layout that should work well at any size regardless of device. Specific browsers and device widths become irrelevant. The difference is that you only add a media query when and if the layout breaks. These media queries are known as content breakpoints.

![](.)

Where adaptive design tries to bend the web to its will, responsive design embraces it. Responsive design understands that you can't possibly design for every device and browser individually. That's just not how the web works. I've come to call the web, a continuum of edgelessness.

With this in mind, let's look at why the select box menu requires an adaptive approach to design. By default, and on small screens, users get a select box. Then, when there is enough space, it's swapped for set of submit buttons, laid out in a row.

![Adaptive select box](.)

In this case the big screen view entirely discards the select box in favour of a completely different design using CSS and Javascript. Not only does this mean more work, but the page will take longer to load. And we either have to change the HTML dynamically with Javascript, or we have to have both layouts in HTML, ready to be enabled and disabled through a CSS device breakpoint.

```CSS
Example {}
```

That's not all. The server now needs to be aware of how both layouts transmit data. The select box sends `selectName="value"` and the submit buttons sends `buttonName="value"` creating yet more work and a maintenance overhead.

### Hover vs click

TODO: Work out where to weave this.

### A responsive menu component

Instead of bending a `select` box to our will, we'll design a true and responsive menu.

![Responsive Menu](.)

```HTML
<div role="menubar">
  <input role="menuitem" type="submit" name="archive" value="Archive">
  <input role="menuitem" type="submit" name="delete" value="Delete">
  <input role="menuitem" type="submit" name="spam" value="Mark as spam">
</div>
```

The menu has a role of `menubar` indicating that it contains menu items. That's why each submit button is given a role of `menuitem`, so screen readers can announce it as a three-item menu. Visually the three buttons are grouped together. So all we've really achieved in using ARIA is to convey the same meaning for screen readers.

When there isn't enough room to display them inline, they'll stack beneath each other. To avoid this, we can tweak the CSS and Javascript to collapse the submit buttons inside a traditional menu.

![Menu closed and open](./images/etc.png)

```HTML
<button aria-haspopup="true" aria-expanded="false">
  Actions
  <span aria-hidden="true">&#x25be;</span>
</button>
<div role="menu">
  <input role="menuitem" type="submit" name="archive" value="Archive">
  <input role="menuitem" type="submit" name="delete" value="Delete">
  <input role="menuitem" type="submit" name="spam" value="Mark as spam">
</div>
```

Notes:

- The `aria-haspopup` attribute indicates that the button shows a menu. It acts as warning that, when pressed, the user will be moved to the “popup” menu.
- The `<span>` contains the unicode character for a down arrow. Conventionally this indicates visually what `aria-haspopup` does non-visually&mdash;that pressing the button reveals something below it. The `aria-hidden="true"` attribute prevents screen readers from announcing “down pointing triangle” or similar. Thanks to `aria-haspopup`, it’s not needed in the non-visual context.
- The `aria-haspopup` attribute is complemented by `aria-expanded` which tells users whether the menu is currently expanded or collapsed by toggling between `true` and `false` values.
- The role is now set to `menu` instead of `menubar` because it now expands and collapses. Where as a `menubar` is always visible.

Here's the Javascript:

```JS
function Menu(container, options) {
	this.container = container;
	this.menu = this.container.find('.menu-items');
	this.setupOptions(options);
	this.setupKeys();
	this.menu.on('keydown', 'input', $.proxy(this, 'onButtonKeydown'));
	this.createToggleButton();
	this.setupResponsiveChecks();
}

Menu.prototype.setupOptions = function(options) {
	options = options || {};
	options.mq = options.mq || '(min-width: 40em)';
	this.options = options;
};

Menu.prototype.setupResponsiveChecks = function() {
	this.mq = window.matchMedia(this.options.mq);
	this.mq.addListener($.proxy(this, 'checkMode'));
	this.checkMode(this.mq);
};

Menu.prototype.createToggleButton = function() {
	this.menuButton = $('<button class="secondaryButton" type="button" aria-haspopup="true" aria-expanded="false">Actions<span aria-hidden="true">&#x25be;</span></button>');
	this.menuButton.on('click', $.proxy(this, 'onMenuButtonclick'));
};

Menu.prototype.checkMode = function(mq) {
	if(mq.matches) {
		this.enableBigMode();
	} else {
		this.enableSmallMode();
	}
};

Menu.prototype.enableSmallMode = function() {
	this.container.prepend(this.menuButton);
	this.hideMenu();
	this.menu[0].setAttribute('role', 'menu');
	this.setupTabIndex();
};

Menu.prototype.enableBigMode = function() {
	this.menuButton.detach();
	this.showMenu();
	this.menu[0].setAttribute('role', 'menubar');
	this.resetTabIndex();
};

Menu.prototype.hideMenu = function() {
	this.menu[0].hidden = true;
	this.menuButton[0].setAttribute('aria-expanded', 'false');
};

Menu.prototype.showMenu = function(first_argument) {
	this.menu[0].hidden = false;
	this.menuButton[0].setAttribute('aria-expanded', 'true');
};

Menu.prototype.onMenuButtonclick = function(e) {
	if(this.menu[0].hidden) {
		this.showMenu();
		this.menu.find('input')[0].focus();
	} else {
		this.hideMenu();
		this.menuButton.focus();
	}
};

Menu.prototype.setupKeys = function() {
	this.keys = {
		enter: 13,
		esc: 27,
		space: 32,
		left: 37,
		up: 38,
		right: 39,
		down: 40,
		tab: 9
   };
};

Menu.prototype.setupTabIndex = function() {
	this.container.find('input').each($.proxy(function(index, el) {
		el.tabIndex = -1;
	}, this));
};

Menu.prototype.resetTabIndex = function() {
	this.container.find('input').each($.proxy(function(index, el) {
		el.tabIndex = 0;
	}, this));
};

Menu.prototype.onButtonKeydown = function(e) {
	switch (e.keyCode) {
		case this.keys.right:
			e.preventDefault();
			this.focusNext(e.currentTarget);
			break;
		case this.keys.up:
			e.preventDefault();
			this.focusPrevious(e.currentTarget);
			break;
		case this.keys.down:
			e.preventDefault();
			this.focusNext(e.currentTarget);
			break;
		case this.keys.left:
			e.preventDefault();
			this.focusPrevious(e.currentTarget);
			break;

		case this.keys.esc:
			if(!this.mq.matches) {
				this.menuButton.focus();
				this.hideMenu();
			}
			break;
		case this.keys.tab:
			if(!this.mq.matches) {
				this.hideMenu();
			}
	}
};

Menu.prototype.focusNext = function(currentButton) {
	var next = $(currentButton).next();
	if(next[0]) {
		next.focus();
	} else {
		this.container.find('input').first().focus();
	}
};

Menu.prototype.focusPrevious = function(currentButton) {
	var prev = $(currentButton).prev();
	if(prev[0]) {
		prev.focus();
	} else {
		this.container.find('input').last().focus();
	}
};
```

Notes:

- Pressing the button shows the menu and moves focus to the first `menuitem`.
- Pressing <kbd>down</kbd> or <kbd>up</kbd> on a `menuitem` moves to the next or previous item (on loop).
- Pressing <kbd>escape</kbd> on a `menuitem` moves focus to the menu button and closes the menu.
- All `menuitems`s have `tabindex="-1"` which means pressing <kbd>tab</kbd> won't move focus to the them. Instead, users can traverse the items with the arrow keys, which saves them having to wade through each of the menu items to get to the next discrete component.

### A note about `matchmedia`

The `matchMedia` API is the Javascript equivalent of a media query. It's a way of keeping behaviour and style in-sync based on the same media query. Where `@media() {}` is for CSS, `matchMedia()` is for Javascript.

In this case, when the viewport has a width less than `45em` the script will construct a toggle menu. When it's more than `45em` it will construct a horizontal menubar. The related CSS is below.

```CSS
/* other styles */

@media(min-width: 45em) {
	/* styles that only apply when the viewport is at least 45ems wide */
}
```

Before `matchMedia` we had to use flakey techniques to get the width of the viewport[^]. Even then, you could only get the value in pixels, not `em`s. It's preferrable to use ems as they honour users' browser CSS preferences or an increase in font size because they are a relative unit. For further reading, see X.

## Select all

Users may want to, for example, archive every email in their inbox. Rather than selecting each one manually, we can provide a more convenient method. One way to service this functionality is through a *special* checkbox, placed at the top and in vertical alignment with the others creating a visual connection. Clicking it checks every checkbox at once.

[!Checkbox mailchimp?](.)

Arguably, this standard input has all the ingredients of an accessible control as it’s screen reader and keyboard accessible. It communicates through its label and change of state. Its label would be *Select all* and it's state would be announced as *checked* or *unchecked*. All this behaviour without an sniff of Javascript.

By now the benefits of using standard elements should be well understood. Depsite this control being accessible by mouse, touch, keyboard and screen readers, it just doesn't quite feel right. Accessibility is only a part of inclusive design. These controls have to look like what they do.

The trouble with using a checkboxes is that they don't signal what they do. Checkboxes, like select boxes, are associated with collecting data for submission. We should match peoples's expectation by using the same interface component for the same job. In doing so the interface becomes familiar and happens to conform to principles 3, *be consistent*.

Let's do this by creating a true toggle button. HTML's `<button>` is perfect and most of the time it should be your go-to element for changing anything with Javascript. That is, except for changing location which is what links are for. The button element, however, doesn't have the concept of *toggling*, so we'll enhance the button with ARIA.

```HTML
<button type="button" aria-pressed="false">Select all</button>
```

The `aria-pressed` attribute will be announced by screen readers as ‘button, select all, pressed’ (or similar). Pressing the button toggles the attribute between `true` (pressed) and `false` (unpressed). We can convey the same meaning for sighted users by styling the button with CSS.

```CSS
button {
	/* styles */
}

button[aria-pressed="true"] {
	/* styles*/
}
```

## Success messages

When the user submits the form, the selected emails will disppear from their inbox. When an action has been completed, telling users is simply the respectful thing to do. Not doing so leaves users to wonder what happened, if at all.

In chapter 1, we designed and constructed an error summary panel that resides at the top of the page. A success message needs a similar treatment with just a few tweaks. First is that instead of having red colouration, it should be green which is universally associated with success. And the content should be ‘You successfully archived 15 emails’ or similar.

![Success](.)

```HTML
<div class="successMessage" role="alert">
  <h2>You've successfully archived 15 emails.</h2>
</div>
```

Both the error and success message panels are placed within the natural flow of the document toward the top of the page to indicate their importance. The `role="alert"` as noted in ‘A Registration Form’ ensures that screen readers will announced it when the page loads.

### Toast messages

Some applications employ what is known as a ‘toast’ message or notification. When the application wants notify users of something, a little (non modal!) dialog will pop-up on top of the page&mdash;a little bit like a piece of toast. Then, after a certain amount of time has elapsed the notification disppears automatically, by fading out.

![Toast message](.)

This is all very interesting from a design perspective, but it's hardly a useful way to communicate. First, the message obscures the content beneath. Second, users have to read the message before it disappears. This makes comprehension a stressful task and takes control *away* from the user.

Really, a success message should be laid out bare and naturally within the flow of the page. There is no need to obscure the content. After all the message is temporary and will naturally disappear when the user leaves the page.

However, if research shows that being able to dismiss a message *adds value* offer the functionality in the form of a button. Clicking it, simply hides the message.

![Success with dismiss](.)

Be careful to inject the `<button>` with Javascript. If we put it in directly in the HTML, then the interface will appear broken when Javascript is available. That is because clicking the button without Javascript will do nothing.

```JS
function MessageDismisser(panel) {
	this.panel = panel;
	this.createButton();
}

MessageDismisser.prototype.createButton = function() {
	var button = $('<button>Dismiss message</button>');
	this.panel.append(button);
	button.on('click', $.proxy(function(e) {
		this.panel.remove();
	}, this);
};
```

### Confirming versus undoing an action

As a safety measure, some roads have speed bumps. They cause drivers to slow down on roads that are more likely to cause accidents. We can achieve the same thing in an interface by asking users to confirm their action:

![Are you sure](./images/etc.png)

This is fine for infrequent tasks but it quickly gets tedious when they need to be performed more often. To continue with the driving analogy, it's a bit like putting speed bumps on motorways: they'd cause more problems than they'd solve.

An alternative approach is to let users perform the action immediately, without warning. Then, along with the success message, give users the choice to undo their action. Clicking *undo* reverses the action by restoring their emails. If only we could *undo* accidents on the road.

![Undo](./images/undo.png)

## Summary

In this chapter we began by choosing the right way to present a collection of emails and the impact of combining two disparate modes&mdash;reading email and actioning it&mdash;into one interface. We looked at how a mult-select form is different to most other types of form and how this caused us to consider several other aspects of design. Finally we looked at ways in which to *add value* and put users firmly in control by designing consistent interfaces that give users feedback and a way to undo their actions.

### Things to avoid

- Ploughing ahead without deeply understanding the materials we design with.
- Fixing bad HTML with Javascript.
- Using ARIA when the functionality can be achieved with HTML.
- Using checkboxes and select boxes for interface components that don't collect data.
- Highlighting rows and doing more work for the sake it.
- Disabling submit buttons until the form becomes valid.
- Putting *Are you sure?* messages in-front of repeated tasks.

## Footnotes

[^1]: https://en.wikipedia.org/wiki/List_of_people_known_as_%22the_Great%22
[^2]: http://whatis.techtarget.com/definition/inbox-zero
[^3]: https://resilientwebdesign.com/chapter2/#Using%20TABLEs%20for%20layout%20is%20materially%20dishonest.
[^4]: https://www.w3.org/TR/UNDERSTANDING-WCAG20/consistent-behavior-unpredictable-change.html
[^5]: https://www.smashingmagazine.com/2011/01/guidelines-for-responsive-web-design/