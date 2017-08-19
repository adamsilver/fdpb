# An inbox

My sister loves to-do lists. In fact she loves lists so much, that one of her favourite things is making new lists out of old ones. Despite her obsession, the world is full of lists. There is even a list of great people[^1]. But lists are tricky to manage. On the web, there are several types of lists and there are some design patterns that have emerged over the years to deal with managing them.

We're going to design an inbox. In other words, a list of emails. Besides reading and replying to emails, the aim is to achieve a zen-like state of Inbox Zero[^2]. To get there quickly, the interface will let users delete, archive and mark emails as spam. But not just one at a time&mdash;in bulk. My sister loves pen and paper, but if we do this right, perhaps she'll be converted to digital.

## List types

First, we're going to look at how best to mark-up a list of emails. Discussing lists may seem out of place in a book about forms, but forms rarely form part of an interface on their own. Ignoring their surrounings can easily result in disagreeable experiences.

The meaning&mdash;or semantics&mdash;behind elements should drive their visual design. That is, something should look like it behaves. There are 4 elements we can use to construct lists each with different semantics:

- Description lists
- Tables
- Ordered lists and unordered lists

Let's discuss the pros and cons of each in relation to our inbox and pick the best one for the job.

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

To let users select emails for action, we'll need to add some checkboxes. That's easy enough but adding a visible label is going to be tricky. There's no space because the interface handles two disparate jobs: viewing email and managing it.

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

To an extent visible labels can be omitted from the interface. Including them interferes with the expected behaviour&mdash;that is, clicking the row should take the user to read the email. In any case, a link and a label can't occupy the same space because they have opposing behaviour. Remember clicking a label should check the checkbox.

### Use modes

Because we're trying to handle two separate jobs, designing the interface is complicated. One way to design this problem out is by splitting apart the two jobs using the concept of modes. To do this, we'll have to give users a way to switch modes. A simple link will do as shown below. Clicking ‘Manage mode’ puts users into manage mode. When in manage mode, the link text will change to ‘Read mode’ and take users back to the initial view.

![Mode](.)

When in read-mode there will be no checkboxes or any other form component. The entire row will be a link taking users to read the email. When in manage-mode, the entire row turns into a label, that when clicked marks the checkbox.

Modes are best suited when one mode is used more frequently than the other. However, when both are used frequently, as is the case of an inbox, having to switch back and forth all the time is undesirable.

### Visually hide the label

Instead of using modes, we can add a visually hidden label. There are two ways to do this. The first is to use `aria-labelledby` attribute as shown below which uses the content that already exists. This does mean we have to add unique `id`s to make this work. In any case, ARIA shouldn't be used unless we have to as we've talked about already.

Alternatively, a standard `<label>` has better support but including one means duplicating content. This isn't a huge performance problem, but if we're not careful, bloated HTML can eventually diminish the experience by causing some operations to take longer&mdash;screen reader software can be unresponsive.

There is an advantage in duplication. As the contents of the label is just screen readers, we can craft a screen reader specific message that reads better audibly than it otherwise would visually.

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

It is, however, possible to highlight the entire row with CSS and Javascript. As designers, we're tempted to do more than the minimum. We think that more is better. We think that more is a symbol of hard work. It's actually a lot harder to *resist* doing more, than simply *doing* more. Constantly striving for less in a world that rewards you for doing more is very hard work indeed.

Mailchimp, known for their usability prowess, set an example here by doing the minimum. One can assume that's enough. My own user research aligns with this too. However, if your own research shows that highlighting the entire row is essential then do so.

## An action menu

It's all well and good letting users select multiple emails, but we haven't given them a way to action them. Unlike the forms we designed in previous chapters, our inbox is different. Let's explore why this is next.

### Button location

Before now, we've positioned a single submit button directly below the last form field, which we know works best through eye tracking tests. This certainly makes sense as users answer questions from top to bottom and then submit. But here users are actioning individually selected emails. Positioning buttons at the bottom makes discovery harder so we put them at the top.

### Implicit submission and multiple submit buttons

Implicit submission lets you press <kbd>enter</kbd> when a field is focused. This is a convention that lets users submit a form quicker without having to move focus to the submit button. This is particularly useful for a search form&mdash;one that consists of a text box and submit button&mdash;because users can take the efficient route pressing <kbd>enter</kbd>.

![Stuff](.)


The problem comes when a form has multiple submit buttons. If the user presses <kbd>enter</kbd>, which action will be taken? Browsers simply choose the first button in the document flow. Take a look at the following form. It has two submit buttons: delete and update. If the user implicitly submits the form the record will be deleted instead of updated, as the user intended.

To mitigate this, you should split the forms up into separate pages ensuring that there is one action per form. Admittedly, depending on the design this is not always easy, which is the case for the inbox. We could ask users to choose an action *before* selecting and actioning the emails, but this seems a little long winded.

![Click action link -> present checkboxes (with submit at bottom) ->confirm action](.)

Fortunately, by convention multi-selection often puts the actions at the top, as opposed to the bottom. This gives users a chance to discover the multitude of available actions before making selection. Also, a form consisting soley of checkboxes doesn't make implicit action all that useful. If you have to use multiple submit buttons, put the least invasive action first&mdash;in this case *archive*. Lastly, let users undo their action (more on this shortly).

### Disabling and hiding the submit buttons before selection

You may think it's a good idea to disable the buttons until users selects at least one email. But we already discussed the problems with disabling submit buttons in the first chapter. Similarly, hiding the buttons makes the actions less discoverable. A clean interface is good, but not at the cost of clarity.

### Types of menu

On big screens&mdash;or responsively speaking, when there is available space&mdash;laying out the buttons horizontally is fine. However, an inbox may have other components and there may not be enough space (even on big screens) to present them comfortably.

![Blah](.)

Similarly, on small screens the buttons are likely to stack beneath each other pushing the inbox down the page. Having the buttons dominate the interface like this is problematic. *Dominance* is a quality we should use sparingly. After all, if everything dominates, nothing does. The inbox should take center stage with the menu taking a back-seat role.

![Blah](.)

To keep the interface clean but easy-to-scan we can hide the options in a menu. There are 2 ways to create a menu. The first is by using a `select` box and the second is by building a custom menu component.

#### Select box menu

Select boxes are a menu of sorts. They present items for selection (like a menu) and they're an attractive option because browsers supply them for free. Even though select boxes look like menus and behave a little like them, they *aren't* menus.

Select boxes are for input. That's why forms that contain select boxes&mdash;like any other input&mdash;must be accompanied by a submit button to submit the choice. Not only is this by convention, but it's also in the Web Content Accessibility Guidelines (WCAG)[^4]:

> Changing the setting of any user interface component does not automatically cause a change of context.

This compliments principle 4, *give control*. Conversely, select boxes that submit the form `onchange` *takes control away*. Unsuprisingly then, this causes problems for screen reader and keyboard users. On Chrome (Windows), for example, the form is submitted as soon as the user moves to the next option by pressing <kbd>down</kbd>. Moving to the option is either hard or impossible.

This is not a browser bug. It's just that some browsers are more forgiving than others. The forgiving ones only submit the form by pressing <kbd>space</kbd> or <kbd>enter</kbd>. As we know, not all browsers are alike or implement the specification in the same way. Therefore, forgetting about people who use a less forgiving browser is an act of exclusivity.

Also, a select box is always collapsed even when space is available. But we want to make selection more convenient when possible. We could use Javascript to create vastly different experiences on small and big screens, but this is an adaptive approach to design and goes against the very foundation of responsive design[^5].

[!Show adapative layout differences](.)

Practically speaking, this is more work, and results in computationally heavy interface for the browser to render. It also adds complexity on the server because it has to be ready to handle the way select boxes and submit buttons transmit data to perform the same task. The select box sends `selectName="value"` and the button send `buttonName="value"`.

#### A true menu component

HTML doesn't have a ‘menu’ element so we need to build our own. The basic HTML looks like this:

```HTML
<div role="menubar">
  <input role="menuitem" type="submit" name="archive" value="Archive">
  <input role="menuitem" type="submit" name="delete" value="Delete">
  <input role="menuitem" type="submit" name="spam" value="Mark as spam">
</div>
```

The menu has a role of `menubar` indicating that it contains menu items. That's why each submit button is given a role of `menuitem`, letting screen readers announce the component as a three-item menu. Visually the three buttons are grouped together. So all we've really achieved in using ARIA is to denote this grouping for those using screen readers.

On small screens, the menu items stack beneath each other as there is no room to present them next to each other. Javascript is used to detect when the screen is small, so that it can collapse the items behind a tradional menu.

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

```JS
Put it here
```

Notes:

- The `aria-haspopup` attribute indicates that the button shows a menu. It acts as warning that, when pressed, the user will be moved to the “popup” menu.
- The `<span>` contains the unicode character for a down arrow. Conventionally this indicates visually what `aria-haspopup` does non-visually&mdash;that pressing the button reveals something below it. The `aria-hidden="true"` attribute prevents screen readers from announcing “down pointing triangle” or similar. Thanks to `aria-haspopup`, it’s not needed in the non-visual context.
- The `aria-haspopup` attribute is complemented by `aria-expanded` which tells users whether the menu is currently expanded or collapsed by toggling between `true` and `false` values.
- The role is now set to `menu` instead of `menubar` because it now expands and collapses. Conversely a `menubar` is always visible.
- Pressing the button shows the menu and moves focus to the first `menuitem`.
- Pressing <kbd>down</kbd> or <kbd>up</kbd> on a `menuitem` moves to the next or previous item (on loop).
- Pressing <kbd>escape</kbd> on a `menuitem` moves focus to the menu button and closes the menu.
- All `menuitems`s have `tabindex="-1"` which means pressing <kbd>tab</kbd> won't move focus to the them. Users can traverse the items with the arrow keys, which saves them having to wade through each of the menu items to get to the next interface component.

## Select all

Users may want to, for example, archive every email in their inbox. Rather than selecting each one manually, we can create a more convenient method. One way to service this functionality is through a *special* checkbox, placed at the top and in vertical alignment with the others creating a visual connection. Clicking it checks every checkbox at once.

[!Checkbox mailchimp?](.)

Arguably, this out-of-the-box input has all the ingredients of an accessible control as it’s screen reader and keyboard accessible. It communicates through its label and change of state. Its label would be *Select all* and it's state would be announced as *checked* or *unchecked*. All this without an ounce of Javascript.

By now the benefits of using native technology are well known. Depsite the fact that this type of control is accessible by mouse, touch, keyboard and screen readers, it just doesn't quite feel right. Accessibility is only a part of inclusive design. These controls have to look like what they do.

The trouble with using a checkbox (like using a select box as a menu), is that these elements don't signal what they do. Checkboxes and select boxes are associated with collecting data for submission. We should match peoples's expectation by conforming to principle 3, *be consistent*.

So let's create a true toggle button. HTML has a generic `<button>` element but it has no concept of *toggling* (so we'll use ARIA for that bit). Most of the time the button should be your go-to element for changing anything with Javascript. That is, except for changing location which is what links do.

```HTML
<button type="button" aria-pressed="true">Select all</button>
```

```JS
Put code here
```

Notes:

- The `aria-pressed` attribute tells the user the state of the button by toggling between `true` and `false` values.
- Pressing the button marks all the checkboxes as checked. Pressing it a second time unchecks them as per their original state.

## Success messages

When the user submits the form, the emails which have been checked will disppear from view. Without letting users know the action was completed successfully, they'll be left to wonder if what they intended to happen, did so.

![Success](.)

It's designed and constructed identically to the error message in chapter 1. The only exception is that it's coloured green, which is associated with *success*.

Some websites choose to hide the message after a certain period of time has passed. But this causes problems because users have to read it quickly, which takes control away from them. If user research shows that being able to dismiss a message *adds value* then append a button that when pressed does just that.

![Success with dismiss](.)

### Confirming versus undoing an action

As a safety measure, some roads have speed bumps. They cause drivers to slow down on roads that are more likely to cause accidents. We can achieve the same thing in an interface by asking users to confirm their action:

![Are you sure](./images/etc.png)

This is fine for tasks that are performed infrequently but it quickly gets tedious when they need to be invoked more often. To continue with the driving analogy, it's a bit like putting speed bumps on motorways.

One alternative approach is to let users perform the action immediately and without warning. Then along with the success message, give users the choice to undo the action. Clicking *undo* reverses the action by restoring their emails. If only we could *undo* accidents on the road.

![Undo](./images/undo.png)

## Summary

In this chapter we began by choosing the right way to present a collection of emails and the impact of combining two disparate modes&mdash;reading email and actioning it&mdash;into one interface. We looked at how a mult-select form is different to the common form and how this made us consider several other aspects of design. Finally we looked at ways in which to *add value* and *give control* by  designing consistent interfaces that give users feedback.

### Things to avoid

- Ploughing ahead without deeply understanding the materials we design with.
- Using ARIA when you don't have to.
- Using checkboxes and select boxes unconventionally.
- Highlighting rows for the sake it.
- Disabling submit buttons until the form becomes valid.
- Putting friction in the form of *Are you sure?* messages in-front of repeated tasks.
- Creating behaviours in Javascript that HTML already offers. (tables links span)

## Footnotes

[^1]: https://en.wikipedia.org/wiki/List_of_people_known_as_%22the_Great%22
[^2]: http://whatis.techtarget.com/definition/inbox-zero
[^3]: https://resilientwebdesign.com/chapter2/#Using%20TABLEs%20for%20layout%20is%20materially%20dishonest.
[^4]: https://www.w3.org/TR/UNDERSTANDING-WCAG20/consistent-behavior-unpredictable-change.html
[^5]: https://www.smashingmagazine.com/2011/01/guidelines-for-responsive-web-design/

## Todo

As designers and makers of things, we should have a good understanding of the materials before creating artifacts. A chair designer should intimately know the properties and constraints of wood for example. Otherwise how are they expected to craft a beautiful, well-functioning and comfortable chair? Similarly, we need to have a deep understanding of HTML, CSS and Javascript.

> ‘If you can solve a problem with a simpler solution lower in the stack, you should.’—Derek Featherstone

- consisder visual design and touch target 44px
