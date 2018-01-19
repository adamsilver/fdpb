# An Inbox

My sister loves to-do lists. In fact, she loves them so much, that one of her favourite things is making new lists out of old ones. The world is full of lists. There's even a list of great people[^1]. On the web, there are several types of lists, and there are some design patterns that have emerged over the years that help to manage them.

In this chapter, we'll look at an inbox. That is, a list of emails sent from other people. In many respects, an inbox is a list of tasks organized around emails. Besides reading and replying to them, the aim is to achieve a zen-like state of Inbox Zero[^2]. To let users get there quickly, we will design the interface so that they can delete, archive and mark emails as spam. But not just one at a time—in bulk. My sister loves pen and paper, but if we get this right, I hope she'll be converted to digital.

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

![Label and link](./images/05/link-label.png)

In this case, you could argue that a visible label is redundant. After all, the label would duplicate the link's content which would make the experience confusing for sighted users.

### Using Modes

Trying to meet two user needs (viewing and managing) in a single interface is partially responsible for the problem in the first place. One way to avoid the issue would be to split these needs apart using the concept of modes. This just means letting users switch between managing email and reading it.

![Modes](./images/05/modes.png)

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

*(Note: The CSS for the visually hidden class is set out in “A Checkout Flow”.)*

While duplication isn't a big performance issue, if we're not careful, bloated HTML can eventually diminish the experience by causing some operations to take longer—screen reader software can be unresponsive, for example.

On the other hand, duplication in this case, can be advantageous. As the label content is just for screen reader users, we can create a specific message just for them. For example, the label has the word “subject” prefixed which is useful in this context. This follows principle 1, *Provide a comparable experience*, which is not about giving users the same experience, but one of comparable value and utility.

### Highlighting Marked Emails

The deal with human-computer interaction is that when the human does something, the computer should respond. In this case, clicking a checkbox makes a little tick appear (and disappear) accordingly. As with every other checkbox in any other form, this is probably enough feedback.

![Checked](./images/05/checked.png)

For example, Mailchimp, who have a reputation for their user-centered design philosophy, show that you don't need to highlight the entire row. They rely solely on the checked state of the checkbox. We can assume their research showed this to be enough. My own research aligns with this too.

![Mailchimp](./images/05/mailchimp-checkbox.png)

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

![Gmail menu proximity](./images/05/gmail-menu.png)

It's worth noting that implicit submission is probably less useful on a form consisting solely of checkboxes. In any case, as we have multiple submit buttons, we should put the least critical action first—in this case, Archive. That way, if a user happens to submit the form implicitly, they'll be in less of a predicament.

Also, we can offer users a way to *undo* their last action, which we'll discuss later in the chapter.

### Sticky Menus

The menu is placed above the list of emails so as users scroll it might disappear off screen. A sticky menu, however, would stay on-screen as soon as the menu gets to the top edge of the viewport.

![Sticky menu](./images/05/sticky-menu.png)

Similarly, Material Design has the floating action button. As users scroll, the action button floats on top of the content. Both of these techniques give users quick and easy access to the menu without having to scroll back up to the top.

![Floating action button](./images/05/floating-action.png)

However, sticky menus are problematic for three reasons.

First, they obscure the content beneath which is especially distracting on mobile as the menu impedes access to the primary content. In the case of the inbox, the primary need is to read and respond to email—not to bulk action it.

Second, sticky menus are usually employed to solve symptoms that mask the true underlying problems. That is, that the page is often too long in the first place. An inbox typically shows just 20 emails at a time, which means the menu is, at most, a quick flick away on mobile and always in view on desktop.

Third, the items within the sticky menus are difficult to focus with the keyboard. You might be half way down the page but the menu (which is in close proximity visually) could be a long way away via the keyboard.

For these reasons, it's better to position the menu statically.

*(Note: where sticky menus are useful, you can use `position: sticky` as a progressive enhancement. In the past, we had to resort to complicated techniques that create a jarring and broken experience across a range of mobile and tablet devices[^6].)*

### Disabling And Hiding Buttons

Some multi-select interfaces will hide or disable the menu buttons until at least one item's been selected. You could argue that showing (or enabling) the buttons in response to selecting an item helps users take the next step. In the case of hiding the buttons, the interface becomes more streamlined as the buttons are only shown as they become relevant. But this is problematic for three reasons.

First, hiding the buttons means the available actions aren't discoverable. This is why designers opt for disabled buttons. But we discussed the problems with disabled buttons in chapter 1, “A Registration Form”. As a quick reminder, they don't tell users why they're disabled and screen reader users can't focus them.

Second, there needs to be space to reveal the buttons in the first place. Where there isn't, the page can judder as the page reveals the buttons and moves other parts of the interface around to make space. 

Third, having the buttons appear when clicking a checkbox is distracting as users are focused on selecting the right emails. And assuming the change of state is valuable, the buttons would have to be in the viewport for users to see the change anyway.

Just show the buttons at all times.

## A Responsive Menu

When there's enough space, the buttons should just be laid out at all times, making them readily available and interactive. But if you have more than three buttons in the menu, or you need to display additional components along the same row, it's going to be hard to fit them on screen, especially on mobile.

![Stacked buttons](./images/05/stacked-buttons.png)

The problem is that the buttons will start to stack beneath each other, which pushes the main content downwards and changes the spatial relationship between the menu and the list of emails. Moreover, having the menu dominate the interface is problematic because dominance is a quality we should use sparingly. After all, if everything dominates, nothing does. Really, the inbox itself should take center stage, with the menu taking a back-seat role.

We can handle this problem by hiding the buttons behind a menu. There are two ways to create a menu: first, by using a `select` box and second, by creating a custom menu component. Let's discuss the pros and cons of each next.

### A Select Box Menu

Select boxes are a menu of sorts. In fact, sometimes, they're referred to as drop-down *menus* amongst other names.

Like a menu, they group similar items together that users can select. And they hide the items behind a click, keeping the interface compact. They're an attractive option because, as we know, browsers supply them for free. But, even though select boxes look like menus and behave like them; and even though they are sometimes referred to as menus—they aren't true menus.

Select boxes are for input. That's why forms that contain select boxes—like any other input—must be accompanied by a submit button to submit the choice. Not only is this convention, but it's also in the Web Content Accessibility Guidelines (WCAG)[^4]:

> Changing the setting of any user interface component does not automatically cause a change of context.

The reason I bring this up is because using a select box as a menu, often causes designers to omit the submit button from the interface. And then, JavaScript is needed to submit the form when the selected option is changed (`onchange`). But this submits the form, without the user's say-so which fails principle 4, *Give control*.

This also causes problems for screen reader and keyboard users. For example, on Chrome (Windows), the `onchange` event is fired as soon as the user presses <kbd>Down</kbd> to select the next option. But with this approach in place, the form is immediately submitted, making it impossible to move through all the items in the menu.

![Select submit onchange](./images/05/select-onchange.png)

Other browsers are more forgiving of such techniques—most won't fire the `onchange` event (and thus submit the form) until the user presses <kbd>Space</kbd> or <kbd>Enter</kbd>. But as not all browsers are alike or implement the specification consistently. Ignoring people who use one of the “bad” browsers, doesn't make the problem any less real for them.

The other problem with using a select box, is that it's always collapsed, even when there's enough space to lay out the options. One solution is to create a completely different component for big screens using Javascript. This is known as adaptive design[^adaptive].

### Adaptive Design Versus Responsive Design

First a fun history lesson. 

When the web came along, we settled on 640 pixel widths. Then a few years later, when larger monitors came to market, we changed to 960 pixels. We no longer cared about people with small monitors. We expected users to maximise their browser. If they didn't they'd get a horizontal scroll bar and that would be their problem.

Then some more years passed. The mobile web was born. Or more accurately, we could use websites on our phones, which happen to have small screens. A million browsers came out. A million devices came out. And browsers gave us CSS media queries.

Accordingly, we started to design for 320px widths. Why? Because many of us had iPhones and this happened to be its width in portrait mode. The hardcore amongst us starting designing for portrait and landscape sizes according to the most popular devices at that time.

Now we have tablets, desktops and really big desktop screens. And we can browse on large screen televisions. Then all the way back down to watches with tiny viewports. If your head is spinning, don't worry, so is mine. This is the problem that responsive design solves and adaptive design exacerbates.

The difference between responsive and adaptive design is both subtle and crucial. Both techniques are based on viewport width. And both use CSS media queries to change the interface. But they are rather different.

#### Adaptive Design

Adaptive design means defining several predefined layouts for specifically chosen viewport widths that correspond to a particular device. All the code for the different layouts are sent to the browser. 

The interface that matches the predefined media query is applied accordingly. These media queries are known as device breakpoints because they are defined based on a device's width.

```CSS
@media only screen and (min-device-width : 375px) and (max-device-width : 667px) { /* styles */}
```

Using this approach is normally unnecessary and counterproductive. 

First, there's an endless stream of devices and browsers with different widths: creating specific designs for every device width is impossible. Second, the extra code needed to produce such designs would result in slow loading pages which are detrimental to the user experience. Third, not all components need a breakpoint. Plenty of components can be designed to work well in exactly the same way on both small and large screens.

Moreover, this isn't just a technical issue. Users should receive a consistent experience (principle 3) no matter which device they choose to use. And if they rotate their device from portrait to landscape, they shouldn't have to relearn the interface as this is unnecessary cognitive load on users. 

#### Responsive Design

Responsive design takes a different approach. It's about designing a single, fluid interface that works well at any size regardless of device. Specific browsers and device widths become irrelevant. The difference is that you only add a media query when and if something breaks. These media queries are known as content breakpoints.

```CSS
@media only screen and (min-device-width : 61.37em) { /* Fix broken layout for a particular thing at a particular width here */}
```

Where adaptive design tries to bend the web to its will, responsive design embraces it. Responsive design understands that you can't possibly design for every device and browser individually. That's just not how the web works. Instead, responsive design encourages us to design interfaces that work on any size screen.

The select box design I suggested before requires an adaptive approach: on small viewports users get a select box. Then, when there's enough space, it's swapped out for set of submit buttons, laid out in a row.

![Adaptive select box](./images/05/adaptive-design.png)

In this case, the big screen view entirely discards the select box in favour of a different interface using CSS and Javascript. Not only does this mean more work, but the page will take longer to load. And we either have to change the HTML dynamically with Javascript, or we have to have both layouts in HTML, ready to be enabled and disabled through a CSS device breakpoint.

```CSS
@media only screen and (min-device-width : 30em) {
  .smallScreenMenu {
    display: none;
  }

  .largeScreenMenu {
    display: block;
  }
}
```

If that weren't enough, the server needs to be aware of how both menus transmit data. That is, the select box sends different data to the submit buttons. Adaptive design can be even more complicated if you had to listen to JavaScript events to restructure the arrangement of HTML.

Not only is all of this more work, but there are now two vastly different variations of the same feature to maintain indefinitely.

### Hover Versus Click

On the web, menus are often opened on hover. There are many problems with opening a menu or revealing anything else for that matter this way.

First, hovering is not an intention to open the menu. When a user moves over a menu that opens on hover it can obscure the content behind which disrupts the experience. With the inbox, as the user goes to select the first checkbox, they may accidentally end up clicking one of the items in the menu which fails principles 4, *Give Control*.

Second, users have to be careful to keep the cursor within the bounds of the menu, otherwise it will close. This is known as a hover tunnel, and is especially difficult to operate with motor impairments.

Third, not all users use a mouse (or other pointing device) and touch-screen devices are usually operated without one. So you need to show the menu on click anyway. If you wanted to open the menu on hover for large viewports and on click for small ones, think again. There are many large touch screen devices.

### A True Menu

Having nimbley navigated through the pitfalls of adaptive design and activating a menu on hover, we can now safely proceed to design a true, responsive menu that opens on click.

![True menu](./images/05/true-menu.png)

#### The Basic Mark-up

```HTML
<div role="menu">
  <input role="menuitem" type="submit" name="archive" value="Archive">
  <input role="menuitem" type="submit" name="delete" value="Delete">
  <input role="menuitem" type="submit" name="spam" value="Mark as spam">
</div>
```

The menu has a role of `menu` indicating that it contains menu items. When the menu is focused, screen readers will announce it as a three-item menu. You should note that the `menu` role isn't commonly used in web applications. Only ones that mimic desktop applications (like this one, which is essentially a web-based Outlook application) apply.

#### The Enhanced Mark-up

```HTML
Put here
```

When there isn't enough room to display the menu items inline, we'll collapse them behind a traditional menu using media queries and JavaScript's `matchMedia` API:

```JS
Menu.prototype.setupResponsiveChecks = function() {
  this.mq = window.matchMedia('(min-width: 40em)');
  this.mq.addListener($.proxy(this, 'checkMode'));
  this.checkMode(this.mq);
};

Menu.prototype.checkMode = function(mq) {
  if(mq.matches) {
    this.enableBigMode();
  } else {
    this.enableSmallMode();
  }
};
```

The `matchMedia` API is the Javascript equivalent of a media query. Where `@media() {}` is for CSS, `matchMedia()` is for Javascript. It's a way of keeping behaviour and style in-sync based on the same media query. In this case, when the viewport's width is less than `40em` the script will construct a toggle menu (small mode). When it's more than `40em` it will construct a horizontal menu (big mode).

Before `matchMedia` we had to use flakey techniques to get the width of the viewport[^]. Even then, you could only get the value in pixels, not `em`s. Using `em`s is preferred because if the user increases the text size, the layout will adapt in proportion.

When small mode is enabled, the HTML is updated:

```HTML
<button type="button" aria-haspopup="true" aria-expanded="false">
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
- The `<span>` contains the unicode character for a down arrow. Conventionally this indicates visually what `aria-haspopup` does non-visually — that pressing the button reveals something. The `aria-hidden="true"` attribute prevents screen readers from announcing “down pointing triangle” or similar. Thanks to `aria-haspopup`, it’s not needed in the non-visual context.
- The `aria-haspopup` attribute is complemented by `aria-expanded` which tells users whether the menu is currently expanded (open) or collapsed (closed) by toggling between `true` and `false` values.

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

We can use the `[aria-expanded]` CSS attribute selector to toggle the menu's display:

```CSS
[aria-expanded="true"] + .menu-items {
  display: block;
}

[aria-expanded="false"] + .menu-items {
  display: none;
}
```

When focus is on a menu item, pressing <kbd>Down</kbd> or <kbd>Up</kbd> arrows will move to the next or previous item, on loop. Pressing <kbd>Escape</kbd> on a menu item will move focus to the menu button and closes the menu. Sometimes, <kbd>Home</kbd> and <kbd>End</kbd> are used to go to the first and last items directly which is particularly useful if there are lots of menu items.

## Select All

Users may want to, for example, archive every email in their inbox. Rather than selecting each email, one by one, we can provide a more convenient method. One way to service this functionality is through a *special* checkbox, placed at the top and in vertical alignment with the other checkboxes creating a visual connection. Clicking it would check every checkbox in one fell swoop.

![Select all mailchimp](./images/05/mailchimp-select-all.png)

Arguably, this standard input has all the ingredients of an accessible control as it is screen reader and keyboard accessible. It communicates through its label and change of state. Its label would be *Select all* and it's state would be announced as *checked* or *unchecked*. All this behaviour without any Javascript.

By now the benefits of using standard elements should be well understood. Despite this control being accessible by mouse, touch, keyboard and screen readers, it just doesn't quite feel right. Accessibility is only a part of inclusive design. This control should look like what it does.

The trouble with using a checkbox is that they don't signal what they do. Like select boxes, they are associated with collecting data for submission. We should match peoples's expectation by using the same interface component for the same job. In doing so, the interface becomes familiar and consistent which speaks to principle 3, *Be consistent*.

Like the menu button, we can use the `<button>` element again to create a true toggle button. What's missing is that buttons don't have the concept of *toggling*, but we can enrich their semantics with ARIA.

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

## Success Messages

When the user submits the form, the selected emails will disappear from the inbox. When an action has been completed, telling users that it has been completed is the respectful thing to do. Not doing so leaves users wondering what happened, if anything.

In chapter 1, “A Registration Form”, we designed and constructed an error summary panel that resides at the top of the page. A success message needs a similar treatment with a couple of tweaks.

Instead of having red colouration, it should be green which is universally associated with success. Second, the content should be “You've successfully archived 15 emails” (or similar).

![Success](./images/05/success-message.png)

```HTML
<div class="successMessage" role="alert">
  <h2>You've successfully archived 15 emails.</h2>
</div>
```

Both the error and success message panels are placed within the natural flow of the document and toward the top of the page to indicate their importance. The `role="alert"` attribute ensures that screen readers will announced it when the page loads or if it is updated on the client.

### Toast Messages

Some applications employ what is known as a ‘toast’ message or notification. When the application needs to notify users, a little (non modal) dialog will pop-up on top of the page - a bit like a piece of toast. Then, after a certain amount of time the notification disappears automatically, usually with a fading out animation.

![Toast message](./images/05/toast.png)

This is all very interesting from a design perspective, but it's hardly a useful way to communicate. First, the message obscures the content beneath. Second, users have to read the message before it disappears. This makes comprehension a stressful task and takes control *away* from the user.

Really, a success message should be laid out bare and placed within the natural flow of the page. There is no need to obscure parts of the interface. After all, the message is temporary and will naturally disappear when the user leaves the page.

However, if research shows that being able to dismiss a message *adds value*, you can offer the functionality in the form of a button. When clicked, it hides the message.

![Success With Dismiss](./images/05/dismiss.png)

Be careful to inject the `<button>` with Javascript. If we put it in directly in the HTML, then the interface will appear broken when Javascript is unavailable. That is, nothing will happen when it's clicked.

### Confirming Versus Undoing

As a safety measure, some roads have speed bumps. They cause drivers to slow down on roads that are more likely to cause accidents. We can create a digital speed bump by asking users to confirm their action by asking them “if they're sure.” 

![Are you sure](./images/05/are-you-sure.png)

This is fine for infrequent tasks but it quickly becomes tedious when that action needs to be performed more often. Continuing with the driving analogy then: it's a bit like puttings speed bumps on the motorway. They'd probably cause more accidents than they stop.

An alternative approach would be to let users perform the action immediately, without any warning. Then, along with the success message, give users the chance to undo the action. Clicking *undo*, would reverse the action by restoring the emails back to the inbox. If only we could *undo* accidents on the road.

![Undo](./images/05/undo.png)

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

- Inbox
- Menu
- Select all

## Footnotes

[^1]: https://en.wikipedia.org/wiki/List_of_people_known_as_%22the_Great%22
[^2]: http://whatis.techtarget.com/definition/inbox-zero
[^3]: https://resilientwebdesign.com/chapter2/#Using%20TABLEs%20for%20layout%20is%20materially%20dishonest.
[^4]: https://www.w3.org/TR/UNDERSTANDING-WCAG20/consistent-behavior-unpredictable-change.html
[^5]: https://www.smashingmagazine.com/2011/01/guidelines-for-responsive-web-design/
[^6]: http://bradfrost.com/blog/mobile/fixed-position/
[^adaptive]: http://mediumwell.com/responsive-adaptive-mobile/