# Inbox Management

My sister loves lists. Her favourite list is a todo list. In fact she loves lists so much, that one of her favourite things is making new lists out of old ones. Despite her obsession, the world is full of lists. There is even a list of great people[^1]. But lists are a tricky thing to manage. On the web, there are some conventions that have emerged over the years.

In this chapter we're going to make list management is easy, accessible and scalable. My sister loves pen and paper, but I hope that she may one day be converted to digital.

## Using the right list for the job

HTML gives us 4 different elements that we can use to construct a list. Tables (`<table>`) house tabular data a bit like an excel spreadsheet. A description list (`<dl>`) formely known as a definiton list is for key-value pairs. An ordered list (`<ol>`) which is for a list whereby the order matters, like a list of cooking instructions. Finally an unordered list (`<ul>`) is the same but where order doesn't matter.

It's hard to discuss the merits of each list element without first orientating ourselves around a specific problem. So we'll design an inbox, which is really just what we call a list of *emails*. The aim, of course, is to achieve a zen-like state of Inbox Zero[^2]. To get there, the interface must let users delete, archive and mark emails as spam. But not just one at a time; in bulk.

Whilst this chapter is specifically about an inbox, the design patterns herein are transferable to all types of lists that need bulk actions.

### Everything is list

Semantically speaking, everything is a list. The things on the page are a list of things on the page. Pedanticism aside, we need to decide what type of list is best for our inbox.

### Tables versus lists

Tables work when representing two-dimensional data. In our case, rows represent emails and columns describe that data: recipient, subject and date sent, for example. Interestingly, Gmail, for example, omits table headings which suggests that tables are less appropriate for an inbox.

Alternatively we construct each email as a list item. On big screens, we could still style the information into columns. This brings us to the first problem. Tables aren't especially responsive. Tables are semantically tied to the way they look. That is, it's hard to make tables not *look* like tables. There are some ways to make tables work on small screens but they aren't simple and they don't work well cross-browser. However, striving to make tables (or any other element) not look like a table is materially dishonest[^3].

Tables *are* a good choice when data needs contextual information to make it useful or if the data in those rows need comparing or sorting or summarising (as totals). For example, *23* is ambiguous without *goals scored* and *Lionel Messi* as column and row headings respectively. And we might also be interested in comparing Lionel Messi's statistics in comparison with Cristiano Ronaldo's.

[!table](.)

An inbox seems less tabular: it doesn't particularly need to be sorted (beyond the default of most recent first). Emails don't need to be compared, or summarised. It would be quite readable if an email was simply read it as ‘From Heydon, about “Buttons” (19/09/2017)’. It could be argued that including column headings are verbose and unnecessary. Mailchimp, which has a similar interface is an example that employs list items:

![Mailchimp List](./images/mailchimp-list.png)

The advantage over tables is that they are maleable and therefore responsive. On small screens we can stack the information within each list item. On big screens we can position the information into columns. Talking about lists may seem out of place in a book about forms, but they don't exist in a vaccum. They are a major part of an interface, but they rarely form an interface on their own.

On balance, list items are the preferred construct. This is not to say tables are bad and list items are good; we can't classify elements like that. It's a matter of understanding what they're good for and their associated constraints.

Let's get our hands dirty and start constructing the HTML using a list.

```HTML
<ul class="inbox">
	<li>
		<a href="/emails/1/">
			<div class="inbox-recipient">From Heydon Pickering</div>
			<div class="inbox-subject">Subject: Buttons</div>
			<div class="inbox-date">19/09/2017</div>
		</a>
	</li>
	...
</ul>
```

We'll add the form bits later. Interestingly, the act of constructing the HTML exposes another problem. The contents of the `<li>` is wrapped in `<a>` which we simply cannot do with tables. It's semantically invalid and won't work.

### Understanding materials

As designers and makers of things, we should have a good understanding of the materials before creating artifacts. A chair designer should intimately know the properties and constraints of wood for example. Otherwise how are they expected to craft a beautiful, well-functioning and comfortable chair? Similarly, we need to have a deep understanding of HTML, CSS and Javascript.

> ‘If you can solve a problem with a simpler solution lower in the stack, you should.’—Derek Featherstone

As noted before, Gmail uses `<table>`s so how are the rows clickable? They've used Javascript to fix it. But using Javascript to fix a problem in HTML is an act of exclusivity (because not everyone has Javascript) and frankly it's unnecessary. If we can fix a problem lower down the stack, we should. Really, designing inclusively is simply designing to a set of constraints that guide us to design robust and therefore better experiences for everyone.

## Marking email for action

To let users select and action multiple emails at once, we'll need to add a checkbox next to each email.

```
<ul class="inbox">
	<li>
		<input type="checkbox" name="email">
		<a href="/emails/1/">
			<div class="inbox-recipient">From Heydon Pickering</div>
			<div class="inbox-subject">Subject: Buttons</div>
			<div class="inbox-date">19/09/2017</div>
		</a>
	</li>
	...
</ul>
```

Unlike all other fields in the book so far, the checkbox has a label missing. In *almost* all cases, a visible label should be placed beside the checkbox. However, this is a bit of a special case because the interface handles two disperate jobs: Viewing email and managing it.

In this case we can go without having visible labels. You could even argue that having them would interfere with the navigable behaviour of the contents. The problem is that we can't have a `<label>` and a `<a>` occupy the same position in the interface. There are two possible approaches: to use the concept of modes or to simply hide the label.

### Using modes

As we said earlier, the interface is complicated because it's doing two jobs at once. We can split these jobs out and use modes. This means creating two separate views of the inbox: One that is read-only and one that is for bulk action. Read-mode has no forms; clicking an email navigates to the email (like normal). When in bulk-actin mode, the contents is a `<label>` so clicking it checks the checkbox (again like normal).

![Modes use tabs or a link to switch](./images/modes.png)

Modes are best suited when one mode is used more than the other. But when both are used frequently, having to switch all the time is undesirable.

### Visually hiding the label

There are two ways to create a hidden label. One concise way to do this is by using the `aria-labelledby` attribute:

```
<fieldset class="inbox">
	<legend>Inbox</legend>
	<ul>
		<li>
			<input type="checkbox" name="email" aria-labelledby="e1_recipient e1_subject e1_date">
			<a href="/emails/1/">
				<div class="inbox-recipient" id="e1_recipient">From Heydon Pickering</div>
				<div class="inbox-subject" id="e1_subject">Subject: Buttons</div>
				<div class="inbox-date" id="e1_date">19/09/2017</div>
			</a>
		</li>
	</ul>
</fieldset>
```

As I've probably drummed into you by now, ARIA should be used as a last resort. Practically this is because it means having to add and reference several `id` attributes to make it work. Instead we could use a `<label>`:

```
<fieldset class="inbox">
	<legend>Inbox</legend>
		<ul>
			<li>
			<input type="checkbox" name="email" id="email1">
			<label for="email1">From Heydon Pickering about Buttons (19 September 2017)</label>
			<a href="/emails/1/">
				<div class="inbox-recipient">From Heydon Pickering</div>
				<div class="inbox-subject">Subject: Buttons</div>
				<div class="inbox-date">19/09/2017</div>
			</a>
		</li>
	</ul>
</fieldset>
```

This has excellent support but it does mean duplicating the contents, only to hide it with CSS (using the same technique in chapter 3). Even though we shouldn't prematurely optimise for performance, we should be mindful that bloated HTML can diminish the experience for many users causing operations to to take longer. Assistive technology users especially may find their software unresponsive.

Much to our frustration *perfect* rarely exists in the world of design. On balance, exchanging a bit of duplication for a more inclusive experience is preferable. In fact, having a hidden label lets us craft a better message for screen readers:

*From Heydon Pickering about Buttons (19 September 2017)*

### Highlighting marked emails

The deal with human-computer interaction is that when the human does something, the computer should respond. In this case, clicking a checkbox makes a little tick appear (and disappear) accordingly. As with every other checkbox in any other form, this is probably enough feedback.

It is, however, possible to highlight the entire row with CSS and Javascript. As designers, we're tempted to do more than the minimum. We think that more is better. We think that more is a symbol of hard work. It's actually a lot harder to *resist* doing more, than simply *doing* more. Constantly striving for less in a world that rewards you for doing more is very hard work indeed.

Mailchimp, known for their usability prowess do the minimum:

![Mailchimp List](./images/mailchimp-list.png)

If thorough and diverse user research shows that a highlight is really needed then go ahead, but aim to do the minimum which conforms to principle 7, *add value*.

## An action menu

It's all well and good letting users select multiple emails, but so far there is no way to action them. Unlike forms from previous chapters, an inbox is a bit different. First, it's not a matter of placing a button at the end of the form. Second, there are multiple submit buttons.

### Button location

Before now, we've positioned a single submit button directly below the last form field, which we know works best through eye tracking tests. This certainly makes sense as users answer questions from top to bottom and then submit. But here users are actioning individual selected emails. Positioning buttons at the bottom makes discovery harder so we put them at the top.

### Implicit submission and multiple submit buttons

Implicit submission lets the user press <kbd>enter</kbd> while a field is focussed. In doing so the form is submitted as if the user pressed the first button in the HTML. When there is a single submit button this is not an issue. When there are multiple, it's not clear which action will be taken.

Where possible, you should try and split forms so that they have a single action. This is easy when, for example, you have a form that allows a user to update or delete a record, you can just have two separate forms:

![](.)

Our inbox is a special case. As such it's not quite so easy to split the actions into separate forms. One way, would be for users to first select an action (such as ‘Bulk deletion’). Clicking it takes the user to a dedicated interface where they can select the emails and then apply the action.

![Click action link -> present checkboxes (with submit at bottom) ->confirm action](.)

Whilst possible this seems a little long winded for an inbox. At least by positioning the buttons at the top, users (those who use screen readers and those who don't) have an opportunity to discover the available actions.

Also, a form consisting of a single checkbox group, doesn't make implicit submission all that useful. Conversely, a search form&mdash;one that consists of a text box and submit button&mdash;would benefit greatly from implicit submission, as users can take the efficient route pressing <kbd>enter</kbd>.

In any case, if a user did implicitly submit the form, we can mitigate the danger in two ways. First by putting the least invasive action first (such as archive). Secondly by letting users undo their action which we'll discuss shortly.

### Disabling and hiding the submit buttons before selection

You may think it's a good idea to disable the buttons until users selects at least one email. But we already discussed the problems with disabling submit buttons in the first chapter. Similarly, hiding the buttons makes the actions less discoverable. A clean interface is good, but not at the cost of clarity.

### Types of menu

On big screens&mdash;or responsively speaking, when there is available space&mdash;laying out the buttons horizontally is fine. However, an inbox may have other components and there may not be enough space (even on big screens) to present them comfortably.

![Blah](.)

Similarly, on small screens the buttons are likely to stack beneath each other pushing the inbox down the page. Having the buttons dominate the interface like this is problematic. *Dominance* is a quality we should use sparingly. After all, if everything dominates, nothing does. The inbox should take center stage with the menu taking a back-seat role.

![Blah](.)

To keep the interface clean but easy-to-scan we can hide the options in a menu. There are 2 ways to create a menu. The first is by using a `select` box and the second is by building a custom menu component.

### Select box menu

Select boxes are a menu of sorts. They present items for selection (like a menu) and they're an attractive option because browsers supply them for free. Even though select boxes look like menus and behave a little like them, they *aren't* menus.

Select boxes are for input. That's why (forms that contain) select boxes&mdash;like any other input&mdash;must be accompanied by a submit button (to submit the choice). Not only is this by convention, but it's also in the Web Content Accessibility Guidelines (WCAG)[^4]:

> Changing the setting of any user interface component does not automatically cause a change of context.

This compliments principle 4, *give control*. Conversely, select boxes that submit the form `onchange` *takes control away*. Unsuprisingly then, this causes problems for screen reader and keyboard users. On Chrome (Windows), for example, the form is submitted as soon as the user moves to the next option by pressing <kbd>down</kbd>. Moving to the option is either hard or impossible.

This is not a browser bug. It's just that some browsers are more forgiving than others. The forgiving ones only submit the form by pressing <kbd>space</kbd> or <kbd>enter</kbd>. As we know, not all browsers are alike or implement the specification in the same way. Therefore, forgetting about people who use a less forgiving browser is an act of exclusivity.

Also, a select box is always collapsed even when space is available. But we want to make selection more conveninent when possible. We could use Javascript to create vastly different experiences on small and big screens, but this is an adaptive approach to design and goes against the very foundation of responsive design[^5].

[!Show adapative layout differences](.)

Practically speaking, this is more work, and results in computationally heavy interface for the browser to render. It also adds complexity on the server because it has to be ready to handle the way select boxes and submit buttons transmit data to perform the same task. The select box sends `selectName="value"` and the button send `buttonName="value"`.

### A true menu component

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

Arguably, this out-of-the-box input has all the ingredients of an accessible control as it’s screen reader and keyboard accessible. It communicates through its label and change of state. It's label would be *Select all* and it's state would be announced as *checked* or *unchecked*. All this without an ounce of Javascript.

Despite our natural inclination to lean on native technology. And despite the fact that this type of control is accessible by mouse, touch, keyboard and screen readers, it just doesn't quite feel right. Accessibility is only a part of inclusive design. These controls have to look like what they do.

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

When the user submits the form, the emails which have been checked will disppear from view. Like error messages, success messages communicate success. Without a message, the user is left to wonder if what they intended to happen, did so. Here it is:

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

- Plowing ahead without deeply understanding the materials in which to design.
- Using ARIA when you don't have to.
- Using checkboxes and select boxes unconventionally.
- Highlighting rows for the sake it.
- Disabling submit buttons until the form becomes valid.
- Putting friction in the form of *Are you sure?* messages in-front of repeated tasks.

## Footnotes

[^1]: https://en.wikipedia.org/wiki/List_of_people_known_as_%22the_Great%22
[^2]: http://whatis.techtarget.com/definition/inbox-zero
[^3]: https://resilientwebdesign.com/chapter2/#Using%20TABLEs%20for%20layout%20is%20materially%20dishonest.
[^4]: https://www.w3.org/TR/UNDERSTANDING-WCAG20/consistent-behavior-unpredictable-change.html
[^5]: https://www.smashingmagazine.com/2011/01/guidelines-for-responsive-web-design/