# An Inbox

My sister loves to-do lists. In fact, she loves them so much, that one of her favourite things is making new lists out of old ones. The world is full of lists. There's even a list of great people[^]. On the web, there are several types of lists, and there are some design patterns that have emerged over the years that help to manage them.

In this chapter, we'll look at an inbox. That is, a list of emails sent from other people. In many respects, an inbox is a list of tasks organized around emails. Besides reading and replying to them, the aim is to achieve a zen-like state of Inbox Zero[^]. To let users get there quickly, we will design the interface so that they can delete, archive and mark emails as spam. But not just one at a time—in bulk. My sister loves pen and paper, but if we get this right, I hope she'll be converted to digital.

## List Types

First, we're going to look at how best to mark-up a list of emails. Discussing lists may seem out of place in a book about forms, but forms rarely form part of an interface on their own. Ignoring their surroundings can result in disagreeable experiences.

The meaning—or semantics—behind elements should influence their appearance. In other words, form should follow function. There are 4 elements we can use to construct lists each with different semantics: description lists, tables, ordered lists and unordered lists. Let's discuss the pros and cons of each now.

### Description Lists

A description list (`<dl>`), formerly called a definition list, is for grouping a list of terms and corresponding definitions. For example, product details such as size, price and material. As a list of emails isn't a collection of terms and definitions, this type of list isn't appropriate.

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

A table (`<table>`) is an arrangement of data, laid out in rows and columns. Like a spreadsheet, tables are well-suited for data that needs to be compared, sorted and totalled.

Tables unfortunately, are difficult to style on small viewports, because there's no room to show more than two or three columns at a time. Even then, it could be a squeeze depending on the data inside the cells, creating layout issues. For example, content could wrap profusly or it could cause users to scroll horizontally to reveal the hidden content.

Making tables responsive isn't the most straightforward thing to do because they are inherently tied to the way they look. Put another way, making a table not look like a table, is not only very difficult, but it would be deceptive, counterproductive and inaccessible.

Gmail[^] uses tables and puts recipient, subject and date sent into columns. Interestingly though, there are no table headings, which is the first clue that tables have been used for layout purposes rather than their semantic qualities which causes various access issues. Jeremy Keith talks about this in his book “Resilient Web Design”[^]:

> Using TABLEs for layout is materially dishonest. The TABLE element is intended for marking up the structure of tabular data. The end result [...] is a façade. At first glance everything looks fine, but it won’t stand up to scrutiny. As soon as such a website is stress‐tested by actual usage across a range of browsers, the façade crumbles.

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

See the mark-up above as an example. The `<tr>` is wrapped in an `<a>` to let users click an email to read it. The problem is that browsers ignore the link. It's simply not allowed and screen reader users will struggle to interpret it. 

Gmail makes the row clickable by using Javascript to listen to click events. But not only is this unclear for screen reader users, not everyone has Javascript and quite frankly, it's unnecessary.

### Ordered And Unordered Lists

The generic list is useful because it itemizes and groups content into related chunks accessibly. But they can be used for more than just bullet points. They come in two flavours: ordered (`<ol>`) and unordered (`<ul>`) lists. And they're far less opiniated than tables.

The difference between the two types, lies in their name. If order matters, use an ordered list. For example, following a recipe's instructions requires users to do so in order. Not doing may produce inedible food. On the other hand, an inbox doesn't have to be read or actioned in a predefined order. It sounds simple when put like that, but we tend to overthink these things. 

*(Note: unfortunately, most screen readers don't differentiate between unordered and ordered lists in any semantic way. But this shouldn't stop us from using the right element. As support improves the benefits will be ready and waiting.)*

Let's lay out the inbox using a `<ul>`.

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

Unlike tables, unordered lists are stylistically malleable and therefore responsive. That's because they can be laid out in different ways without having to affect their structure in a way that makes them less accessible non-visually.

With the mark-up above, on large viewports we can lay the emails out in columns and on small viewports we can avoid layout issues by stacking them vertically. Moreover, the entire list item can be made to be clickable without resorting to Javascript hacks. Wrapping a link around the contents is perfectly valid which is less work and more robust.

In the case of an inbox, list items are more suited anyway: not only are column headings redundant, but there's no need to compare or total items in the list.

## Marking Email For Action

To let users mark emails for action, we need to give each row a checkbox.

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

You'll notice each checkbox is missing a label. The problem is that the contents of the link should also be the contents of the label. In other words, two opposing interactions need to occupy the same space. Remember, clicking the label should mark the checkbox, where as clicking the link should navigate the user to the email.

In this case, you could argue that a visible label is redundant. After all, the label would duplicate the link's content which would make the experience confusing for sighted users.

### Using Modes

Trying to meet two user needs (viewing and managing) in a single interface is partially responsible for the problem in the first place. One way to avoid the issue would be to split these needs apart using the concept of modes. This just means letting users switch between managing email and reading it.

![Left inbox in read mode where each row is a link to read the email. Right inbox in manage mode where each row is a label that toggles the checkbox state.](./images/05/modes.png)

Clicking *Organise* puts users into *manage-mode*. When in manage-mode, the link's label changes to ‘Finished’ (or similar), which when clicked, takes the user back to read mode.

When in read-mode there are no checkboxes or any other form paraphernalia. The row is a link which takes users to read the email. When in manage-mode, the row turns into a `<label>`. When clicked, it marks (or unmarks) the checkbox just like normal.

Modes are best suited when one mode is used more frequently than the other. But, when both are used frequently, like an inbox, having to switch back and forth all the time may be undesirable.

*(Note: we should try and avoid modes, but if they're necessary, the interface must make it obvious which mode is invoked.)*

### Visually Hide The Label

Instead of using modes, we can add a visually hidden label. There are two ways to do this. The first is to use the `aria-labelledby` attribute (shown below) which uses existing content to label the checkbox. The downside is that it means adding `id` attributes. In any case, ARIA shouldn't be used unless there is no better alternative—something first noted in chapter 1, “A Registration Form”.

```HTML
<li>
  <input type="checkbox" name="email" aria-labelledby="inbox_label1">
  <a href="/emails/1/" id="inbox_label1">
    <div class="inbox-recipient">John Oates</div>
    <div class="inbox-subject">Your Amazon.co.uk order #123 is out for delivery</div>
    <div class="inbox-date">10 Aug</div>
  </a>
</li>
```

Alternatively, a standard `<label>` has better support and adheres to ARIA's first rule (not to use it if a native option is available). But the downside with this approach, is that the content has to be duplicated which would create redundancy for sighted users with missing CSS.

```HTML
<li>
  <input type="checkbox" name="email" id="email1">
  <label for="email1" class="visually-hidden">From John Oates, subject ‘Your Amazon.co.uk order #123 is out for delivery’ (10 August 2017)</label>
  <a href="/emails/1/">
    <div class="inbox-recipient">John Oates</div>
    <div class="inbox-subject">Your Amazon.co.uk order #123 is out for delivery</div>
    <div class="inbox-date">10 Aug</div>
  </a>
</li>
```

*(Note: The CSS for the visually hidden class is set out in “Checkout”.)*

While duplication isn't a big performance issue, if we're not careful, bloated HTML can eventually diminish the experience by causing some operations to take longer—screen reader software can be unresponsive, for example.

On the other hand, duplication in this case, can be advantageous. As the label content is just for screen reader users, we can create a specific message just for them. For example, the label has the word “subject” prefixed which is useful in this context. This follows principle 1, *Provide a comparable experience*, which is not about giving users the same experience, but one of comparable value and utility.

### Highlighting Marked Emails

The deal with human-computer interaction is that when the human does something, the computer should respond. In this case, clicking a checkbox makes a little tick appear (and disappear) accordingly. As with every other checkbox in any other form, this is probably enough feedback.

![Left checkbox checked. Right checkbox unchecked.](./images/05/checked.png)

For example, Mailchimp, who have a reputation for their user-centered design philosophy, show that you don't need to highlight the entire row. They rely solely on the checked state of the checkbox. We can assume their research showed this to be enough. My own research aligns with this too.

![Mailchimp's campaign list page with one campaign selected.](./images/05/mailchimp-checkbox.png)

We could highlight the entire row using CSS and Javascript, but we should only do that if user research shows this will *Add value* (principle 7).

## Actioning Emails

Letting users select multiple emails is all well and good, but we're going to want to facilitate actioning them to. This form has three actions and therefore three submit buttons: Archive, Delete and Mark As Spam.

```HTML
<input type="submit" name="archive" value="Archive">
<input type="submit" name="delete" value="Delete">
<input type="submit" name="spam" value="Mark as spam">
<ul class="inbox">...</ul>
```

The nature of this form and the presence of multiple submit buttons creates several new problems that previous chapters haven't had to consider. Let's discuss each of those now.

### The Multiple Submit Button Problem

Implicit submission lets users submit the form by pressing <kbd>Enter</kbd> when focus is within a field. This is convention that speeds up submission without having to move focus to the submit button. This is especially useful for a single field form such as a search form.

Having multiple submit buttons (with differing actions) is problematic because if the user presses <kbd>Enter</kbd>, which action will be taken? The answer is that browsers will choose the first button in the document source.

The best solution to this problem, is avoidance. That is, to have just one action per form. Depending on the design, this may not be easy, which is unfortunately the case with the inbox.

One alternative approach could be to expect users to choose which action they want to perform, before selecting the emails to apply that action to. But this seems somewhat unconventional and long-winded.

Fortunately, multi-select interfaces usually place the submit buttons at the top of form in close alignment to the checkboxes. This gives users a way to discover the available actions before making their selection.

![Gmail's inbox screen showing a selected email with additional menu now available.](./images/05/gmail-menu.png)

It's worth noting that implicit submission is probably less useful on a form consisting solely of checkboxes. In any case, as we have multiple submit buttons, we should put the least critical action first—in this case, Archive. That way, if a user happens to submit the form implicitly, they'll be in less of a predicament.

Also, we can offer users a way to *undo* their last action, which we'll discuss later in the chapter.

### Sticky Menus

The menu is placed above the list of emails so as users scroll it might disappear off screen. A sticky menu, however, would stay on-screen as soon as the menu gets to the top edge of the viewport.

![Sticky menu in three states. Left menu positioned above content as normal before the page is scrolled. Middle the menu (not sticky) off screen after the page has been scrolled. Right a sticky menu still on screen even after the page has been scrolled.](./images/05/sticky-menu.png)

Similarly, Material Design has the floating action button. As users scroll, the action button floats on top of the content. Both of these techniques give users quick and easy access to the menu without having to scroll back up to the top.

![Floating action button layered on top of the screen at the bottom right of the viewport.](./images/05/floating-action.png)

However, sticky menus are problematic for three reasons.

First, they obscure the content beneath which is especially distracting on mobile as the menu impedes access to the primary content. In the case of the inbox, the primary need is to read and respond to email—not to bulk action it.

Second, sticky menus are usually employed to solve symptoms that mask the true underlying problems. That is, that the page is often too long in the first place. An inbox typically shows just 20 emails at a time, which means the menu is, at most, a quick flick away on mobile and always in view on desktop.

Third, the items within the sticky menus are difficult to focus with the keyboard. You might be half way down the page but the menu (which is in close proximity visually) could be a long way away via the keyboard.

For these reasons, it's better to position the menu statically.

*(Note: where sticky menus are useful, you can use `position: sticky` as a progressive enhancement. In the past, we had to resort to complicated techniques that create a jarring and broken experience across a range of mobile and tablet devices[^].)*

### Disabling And Hiding Buttons

Some multi-select interfaces will hide or disable the menu buttons until at least one item's been selected. You could argue that showing (or enabling) the buttons in response to selecting an item helps users take the next step. In the case of hiding the buttons, the interface becomes more streamlined as the buttons are only shown as they become relevant. But this is problematic for three reasons.

First, hiding the buttons means the available actions aren't discoverable. This is why designers opt for disabled buttons. But we discussed the problems with disabled buttons in chapter 1, “A Registration Form”. As a quick reminder, they don't tell users why they're disabled and screen reader users can't focus them.

Second, there needs to be space to reveal the buttons in the first place. Where there isn't, the page can judder as the page reveals the buttons and moves other parts of the interface around to make space. 

Third, having the buttons appear when clicking a checkbox is distracting as users are focused on selecting the right emails. And assuming the change of state is valuable, the buttons would have to be in the viewport for users to see the change anyway.

Just show the buttons at all times.

## A Responsive Menu

When there's enough space, the buttons should just be laid out at all times, making them readily available and interactive. But if you have more than three buttons in the menu, or you need to display additional components along the same row, it's going to be hard to fit them on screen, especially on mobile.

![Left on mobile with menu buttons stacked. Right on desktop with menu buttons laid out in a row.](./images/05/stacked-buttons.png)

The problem is that the buttons will start to stack beneath each other, which pushes the main content downwards and changes the spatial relationship between the menu and the list of emails. Moreover, having the menu dominate the interface is problematic because dominance is a quality we should use sparingly. After all, if everything dominates, nothing does. Really, the inbox itself should take center stage, with the menu taking a back-seat role.

We can handle this problem by hiding the buttons behind a menu. There are two ways to create a menu: first, by using a `select` box and second, by creating a custom menu component. Let's discuss the pros and cons of each next.

### A Select Box Menu

Select boxes are a menu of sorts. In fact, sometimes, they're referred to as drop-down *menus* amongst other names.

Like a menu, they group similar items together that users can select. And they hide the items behind a click, keeping the interface compact. They're an attractive option because, as we know, browsers supply them for free. But, even though select boxes look like menus and behave like them; and even though they are sometimes referred to as menus—they aren't true menus.

Select boxes are for input. That's why forms that contain select boxes—like any other input—must be accompanied by a submit button to submit the choice. Not only is this convention, but it's also in the Web Content Accessibility Guidelines (WCAG)[^]:

> Changing the setting of any user interface component does not automatically cause a change of context.

The reason I bring this up is because using a select box as a menu, often causes designers to omit the submit button from the interface. And then, JavaScript is needed to submit the form when the selected option is changed (`onchange`). But this submits the form, without the user's say-so which fails principle 4, *Give control*.

This also causes problems for screen reader and keyboard users. For example, on Chrome (Windows), the `onchange` event is fired as soon as the user presses <kbd>Down</kbd> to select the next option. But with this approach in place, the form is immediately submitted, making it impossible to move through all the items in the menu.

![Expanded select box with the first option selected. Pressing down immediatelty submits the second option when the user might have wanted to select the third option.](./images/05/select-onchange.png)

Other browsers are more forgiving of such techniques—most won't fire the `onchange` event (and thus submit the form) until the user presses <kbd>Space</kbd> or <kbd>Enter</kbd>. But as not all browsers are alike or implement the specification consistently. Ignoring people who use one of the “bad” browsers, doesn't make the problem any less real for them.

The other problem with using a select box, is that it's always collapsed, even when there's enough space to lay out the options. One solution is to create a completely different component for big screens using Javascript. This is known as adaptive design[^].

### Adaptive Design Versus Responsive Design

First a fun history lesson. 

When the web came along, we settled on 640 pixel widths. Then a few years later, when larger monitors came to market, we increased it to 960 pixels. We no longer cared about people with small monitors. We expected users to maximise their browser; if they didn't they'd get a horizontal scroll bar and that would be their problem.

Then a few more years passed. The mobile web was born. Or more accurately, we could use websites on our phones, which happen to have small screens. A million browsers came out. A million devices came out. And browsers gave us CSS media queries.

Accordingly, we started to design for 320px widths. Why? Because many of us had iPhones and this happened to be its width in portrait mode. The hardcore amongst us started designing for portrait and landscape sizes according to the most popular devices at that time.

Now we have tablets, desktops and really big desktop screens. And we can browse on large screen televisions and tiny watches. If your head is spinning, don't worry, so is mine. This is the problem that responsive design solves and adaptive design exacerbates.

The difference between responsive and adaptive design is both subtle and crucial. Both techniques are often based on viewport width. And both use CSS media queries to change the interface. But they are really quite different.

#### Adaptive Design

Not everyone in the industry agrees on the meaning of adaptive design[^]. Some think it means having different layouts that snap at particular sizes—in other words, not fluid. Others think it's the same as responsive design which is hardly surprising seeing as the words are synonyms.

The other common understanding of adaptive design is about defining several different (parts of) layouts, made up of different HTML that's rendered. Originally, this was based on user agent string[^]. Sometimes JavaScript is used to restructure the arrangement of HTML at certain viewport widths. But more commonly these days, it's done using CSS media queries. We'll focus on this flavour of adaptive design going forward.

This involves delivering all the HTML for the different layouts and hiding and showing these layouts based on CSS media queries (that match a particular device's width—usually Apple devices but not always.

```HTML
<!-- layout 1 -->
<div class="stuff1">...</div>

<!-- layout 2 -->
<div class="stuff2">...</div>
```

```CSS
@media only screen and (min-device-width: 375px) and (max-device-width : 667px) {
  .stuff1 {
    display: none;
  }

  .stuff2 {
    display: block;
  }
}
```

Using this approach is normally unnecessary and counterproductive for a number of reasons.

1. There's an endless stream of devices and browsers with different widths: creating specific designs for every device width is impossible.
2. The extra code needed to produce such designs would result in slow loading pages which are detrimental to the user experience. 
3. Not all components need a breakpoint. Plenty of components can be designed to work well in exactly the same way on both small and large screens.

More importantly, users should get a consistent experience (principle 3) no matter which device they choose to use. Rotating their device from portrait to landscape, for example, shouldn't mean having to relearn an interface because that puts an unnecessary cognitive burden on users.

#### Responsive Design

Responsive design takes a different approach. It's about designing a single, fluid interface that works well at any size regardless of device. Specific browsers and device widths become irrelevant. The difference is that you only add a media query when and if something breaks. These media queries are known as content breakpoints.

```CSS
@media only screen and (min-width: 61.37em) { 
  /* Fix broken layout for a particular thing at a particular width here */
}
```

Where adaptive design tries to bend the web to its will, responsive design embraces it. Responsive design understands that you can't possibly design for every device and browser individually. That's just not how the web works. Instead, responsive design encourages us to design interfaces that work on any size screen.

The select box design I mentioned earlier requires an adaptive approach: on small viewports users get a select box. Then, when there's enough space, it's swapped out for submit buttons.

![Top select box menu for small screens. Bottom menu buttons laid out in a row for large screens.](./images/05/adaptive-design.png)

In this case, the big screen view entirely discards the select box in favour of a different interface using CSS and Javascript. Not only does this mean more work, but the page will take longer to load. And we either have to change the HTML dynamically with JavaScript, or we have to have both layouts in HTML, ready to be enabled and disabled through a CSS device breakpoint.

If that weren't enough, the server needs to be aware of how both menus transmit data. In this case, the select box and submit buttons would be sending different data. 

Not only is all of this more work, but there are now two vastly different variations of the same feature to maintain indefinitely. Adaptive design should always be a last resort.

### Hover Versus Click

On the web, menus are sometimes opened on hover. Designers often assume that this aids discovery and saves users the effort of clicking. The thing is, there are many problems with opening a menu (or anything really) on hover and the effort of clicking a part of the interface is extremely low.

First, hovering is not an intention to open the menu. When a user moves over a hover menu it can obscure the content behind it which disrupts the experience. With the inbox, as the user goes to select the first checkbox, they may accidentally end up clicking one of the items in the menu which fails principles 4, *Give Control*.

Second, users have to be careful to keep the cursor within the bounds of the menu, otherwise it will close. This is known as a hover tunnel, and is especially difficult to operate with motor impairments.

Third, not all users use a mouse (or other types of pointing device) and touch-screen devices are usually operated without one. 

You should note that opening a menu on hover on desktop and on click for mobile isn't recommended either. There are many large touch screen devices and many small screen laptops. Features should never be inferred from screen size.

Needless to say, menus should be triggered on click, which is an explicit intention to activate it, keeping users in control.

### A True Menu

Having explored the pitfalls of adaptive design and hover menus, we can now safely proceed to design a true, responsive menu that opens on click.

![Left a collapsible menu for small screens. Right a menu bar for large screens.](./images/05/true-menu.png)

#### The Basic Mark-up

```HTML
<div class="menu">
  <div role="menu">
    <input type="submit" name="archive" value="Archive" role="menuitem"> 
    <input type="submit" name="delete" value="Delete" role="menuitem"> 
    <input type="submit" name="spam" value="Mark as spam" role="menuitem"> 
  </div>
</div>
```

Notes:

- The menu itself has `role="menu"` indicating that it contains menu items. When a menu item is focused, screen readers will announce it as a three-item menu.
- You should note that the `menu` role isn't commonly used in web applications. Only ones that mimic desktop applications (like ours, which is essentially a web-based Outlook application) apply.
- The wrapping `<div class="menu">` will be needed for enhancement purposes because the toggle button will be prepended to it. 

#### Small Mode Versus Big Mode

When the script initialises it will need to check to see if the viewport is in small or big mode. We're using the words small and big, as opposed to mobile and desktop, because responsive design doesn't think in terms of devices. Moreover, the media query values for small and big don't necessarily correspond to mobile or desktop—they are determined by the place in which the menu would otherwise break.

The constructor function (shown below), takes two arguments: the container element and mq (short for media query) string. The media query string is `(min-width: 45em)` in our case, because that's where the interface starts to break.

```JS
function Menu(container, mq) {
  this.container = container;
  this.menu = this.container.find('[role=menu]');
  this.mq = mq;
  this.keys = { esc: 27, up: 38, down: 40, tab: 9 };
  this.menu.on('keydown', '[role=menuitem]', $.proxy(this, 'onButtonKeydown'));

  // create button and listen to click and down events
  this.createToggleButton();

  // Setup up media query listener and check which applies on initialisation
  this.setupResponsiveChecks();
}
```

Besides assigning properties to `this` to make them available to other methods (shown later), the constructor is responsible for listening to the keydown event on the menu items and creating the toggle button. 

The last line calls the `setupResponsiveChecks()` method which is responsible for collapsing the menu items behind a traditional menu using a combination of CSS media queries and JavaScript's `matchMedia` API.

```JS
Menu.prototype.setupResponsiveChecks = function() {
  this.mql = window.matchMedia(this.mq);
  this.mql.addListener($.proxy(this, 'checkMode'));
  this.checkMode(this.mql);
};

Menu.prototype.checkMode = function(mql) {
  if(mql.matches) {
    this.enableBigMode();
  } else {
    this.enableSmallMode();
  }
};
```

The `matchMedia` API is the JavaScript equivalent of a CSS media query. Where `@media() {}` is for CSS, `matchMedia()` is for JavaScript. It's a way of keeping behaviour and style in-sync based on the same media query. In this case, when the `(min-width: 45em)` media query is matched big mode is enabled. When it doesn't match, this means the viewport width is less than 45em and so the script calls the `enableSmallMode()` method which constructs a toggle menu.

```HTML
<div class="menu">
  <button type="button" aria-haspopup="true" aria-expanded="false">
    Actions
    <span aria-hidden="true">&#x25be;</span>
  </button>
  <div role="menu">
    <input role="menuitem" type="submit" name="archive" value="Archive">
    <input role="menuitem" type="submit" name="delete" value="Delete">
    <input role="menuitem" type="submit" name="spam" value="Mark as spam">
  </div>
</div>
```

Notes:

- The `aria-haspopup` attribute indicates that the button shows a menu. It acts as warning that, when pressed, the user will be moved to the “popup” menu.
- The `<span>` contains the unicode character for a down arrow. Conventionally this indicates visually what `aria-haspopup` does non-visually—that pressing the button reveals something. The `aria-hidden="true"` attribute prevents screen readers from announcing “down pointing triangle” or similar. Thanks to `aria-haspopup`, it’s not needed in the non-visual context.
- The `aria-expanded` attribute tells users whether the menu is currently expanded (open) or collapsed (closed) by toggling between `true` and `false` values.

*(Note: before `matchMedia` we had to use flakey techniques to get the width of the viewport[^] breaking the experience in various browser and device combinations. Even in browsers that returned the correct viewport width, it would only do so in pixels—not `em`s. Using `em`s is preferred because when the user increases the text size, the layout will adapt in proportion.)*

#### Keyboard And Focus Behaviour

When the menu button is clicked, the script checks to see if the menu is currently open by checking whether `aria-expanded` is set to `false`. If it is, the menu is shown and focus is moved to the first item. If it isn't, the menu is hidden and focus moves back to the menu button.

```JS
Menu.prototype.onMenuButtonClick = function() {
  if(this.menuButton.attr('aria-expanded') == 'false') {
    this.showMenu();
    this.menu.find('input').first().focus();
  } else {
    this.hideMenu();
    this.menuButton.focus();
  }
};
```

We can use the `[aria-expanded]` CSS attribute selector to toggle the menu's display.

```CSS
[aria-expanded="true"] + [role=menu] {
  display: block;
}

[aria-expanded="false"] + [role=menu] {
  display: none;
}
```

When focus is on a menu item, pressing <kbd>Down</kbd> or <kbd>Up</kbd> arrows will move to the next or previous item, on loop. Pressing <kbd>Escape</kbd> on a menu item will move focus to the menu button and closes the menu. Sometimes, <kbd>Home</kbd> and <kbd>End</kbd> are used to go to the first and last items directly which is particularly useful if there are lots of menu items.

## Select All

Users may want to, for example, archive every email in their inbox. Rather than selecting each email one by one, we can provide a more convenient method. One way to service this functionality is through a *special* checkbox, placed at the top and in vertical alignment with the other checkboxes creating a visual connection. Clicking it would check every checkbox in one fell swoop.

![Mailchimp's campaign list page showing Select All checkbox positioned top left of the list.](./images/05/mailchimp-select-all.png)

Arguably, this standard checkbox has all the ingredients of an accessible control. It's screen reader and keyboard accessible. It communicates through its label and change of state. Its label would be *Select all* and it's state would be announced as *checked* or *unchecked*. All this behaviour without any JavaScript.

By now the benefits of using standard elements should be well understood. Despite this control being accessible by mouse, touch, keyboard and screen readers, it just doesn't quite feel right. Accessibility is only a part of inclusive design. This control should look like what it does.

The trouble with using a checkbox is that they don't signal what they do. Like select boxes, they are associated with collecting data for submission. We should match peoples's expectation by using the same interface component for the same job. In doing so, the interface becomes familiar and consistent which speaks to principle 3, *Be consistent*.

Instead, we can employ a simple button, labelled “Select all”, that when clicked will check all the checkboxes. At the same time the button's label will change to “Deselect all”. Clicking the button will then uncheck all the checkboxes putting them back into their original state.

```HTML
<!-- When unselected -->
<button type="button">Select all</button>

<!-- when selected -->
<button type="button">Deselect all</button>
```

*(Note: we looked at how to implement an alternative toggle button using the `aria-pressed` attribute in chapter 1 for the Password Reveal pattern.)*

## Success Messages

When the user submits the form, the selected emails will disappear from the inbox. When an action has been completed, telling users that it has been completed is the respectful thing to do. Not doing so leaves users wondering what happened, if anything.

In chapter 1, “A Registration Form”, we designed and constructed an error summary panel that resides at the top of the page. A success message needs a similar treatment with a couple of tweaks.

Instead of having red colouration, it should be green which is universally associated with success. Second, the content should be “You've successfully archived 15 emails” (or similar).

![A green success message panel.](./images/05/success-message.png)

```HTML
<div class="successMessage" role="alert">
  <h2>You've successfully archived 15 emails.</h2>
</div>
```

Both the error and success message panels are placed within the natural flow of the document and toward the top of the page to indicate their importance. The `role="alert"` attribute ensures that screen readers will announced it when the page loads or if it is updated on the client.

### Toast Messages

Some applications employ what is known as a ‘toast’ message or notification. When the application needs to notify users, a little (non modal) dialog will pop-up on top of the page - a bit like a piece of toast. Then, after a certain amount of time the notification disappears automatically, usually with a fading out animation.

![A toast notification on Windows position bottom right but just above the task bar.](./images/05/toast.png)

This is all very interesting from a design perspective, but it's hardly a useful way to communicate. First, the message obscures the content beneath. Second, users have to read the message before it disappears. This makes comprehension a stressful task and takes control *away* from the user.

Really, a success message should be laid out bare and placed within the natural flow of the page. There is no need to obscure parts of the interface. After all, the message is temporary and will naturally disappear when the user leaves the page.

However, if users are likely to stay on the page for a long time after, research might show that dismissing a message is valuable after all. In which case, you can offer that functionality with a button. When clicked, it hides the message.

![A success message panel with Dismiss button.](./images/05/dismiss.png)

Be careful to inject the `<button>` with Javascript. If we put it in directly in the HTML, then the interface will appear broken when Javascript is unavailable. That is, nothing will happen when it's clicked.

### Confirming Versus Undoing

As a safety measure, some roads have speed bumps. They cause drivers to slow down on roads that are more likely to cause accidents. We can create a digital speed bump by asking users to confirm their action by asking them “if they're sure.” 

![An “are you sure” confirmation screen with an option to confirm or cancel the action.](./images/05/are-you-sure.png)

This is fine for infrequent tasks but it quickly becomes tedious when that action needs to be performed more often. Continuing with the driving analogy then: it's a bit like puttings speed bumps on the motorway. They'd probably cause more accidents than they stop.

An alternative approach would be to let users perform the action immediately, without any warning. Then, along with the success message, give users the chance to undo the action. Clicking *undo*, would reverse the action by restoring the emails back to the inbox. If only we could *undo* accidents on the road.

![A success message panel with Undo button.](./images/05/undo.png)

## Summary

In this chapter we began by choosing the right way to present a collection of emails and the impact of combining two disparate modes - reading email and actioning it - into one interface. We then looked at how a mult-select form is different to most other types of form and how this caused us to consider several other visual and interactive design treatments. Finally, we looked at ways in which to *add value* and put users firmly in control by designing consistent interfaces that give users feedback and a way to undo their actions.

### Things To Avoid

- Using the wrong element for the job and fixing it with Javascript.
- Using ARIA when standard HTML can be used instead.
- Using checkboxes and select boxes for buttons and menu components.
- Doing work that doesn't *add value*, such as highlighting selected rows.
- Disabling submit buttons until the form becomes valid.
- Hiding notifications automatically.
- Putting “speed bumps” in front of repetitive tasks.

## Demos

- Inbox: http://nostyle.herokuapp.com/examples/inbox

## Footnotes

[^great people]: https://en.wikipedia.org/wiki/List_of_people_known_as_%22the_Great%22
[^inbox zero]: http://whatis.techtarget.com/definition/inbox-zero
[^gmail]: ?
[^resilient web design]: https://resilientwebdesign.com/chapter2/#Using%20TABLEs%20for%20layout%20is%20materially%20dishonest.
[^fixed positioning]: http://bradfrost.com/blog/mobile/fixed-position/
[^wcag change]: https://www.w3.org/TR/UNDERSTANDING-WCAG20/consistent-behavior-unpredictable-change.html
[^adaptive]: http://mediumwell.com/responsive-adaptive-mobile/
[^brad]: http://bradfrost.com/blog/post/the-many-faces-of-adaptive-design/
[^ress]: https://www.lukew.com/ff/entry.asp?1392
[^flakey]: hhttps://www.smashingmagazine.com/2011/01/guidelines-for-responsive-web-design/#javascript