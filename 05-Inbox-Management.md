# Inbox Management

My sister loves lists. Her favourite list is a todo list. In fact she loves lists so much, that one of her favourite things is making new lists out of old ones. Despite her obsession, the world is full of lists. There is even a list of great people[^1]. But lists are a tricky thing to manage. On the web, there are some conventions that have emerged over the years.

In this chapter we're going to make list management is easy, accessible and scalable. My sister loves pen and paper, but I hope that she may one day be converted to digital.

## Using the right list for the job

HTML gives us 4 different elements that we can use to construct a list. Tables (`<table>`) house tabular data a bit like an excel spreadsheet. A description list (`<dl>`) formely known as a definiton list is for key-value pairs. An ordered list (`<ol>`) which is for a list whereby the order matters, like a list of cooking instructions. Finally an unordered list (`<ul>`) is the same but where order doesn't matter.

It's hard to discuss the merits of each list element without first orientating ourselves around a specific problem. So we'll design an inbox, which is really just what we call a list of *emails*. The aim, of course, is to achieve a zen-like state of Inbox Zero[^]. To get there, the interface must let users delete, archive and mark emails as spam. But not just one at a time; in bulk.

Whilst this chapter is specifically about an inbox, the design patterns herein are transferable to all types of lists that need bulk actions.

### Everything is list

Semantically speaking, everything is a list. The things on the page are a list of things on the page. Pedanticism aside, we need to decide what type of list is best for our inbox.

Tables work when representing two-dimensional data. In our case, rows represent emails and columns describe that data: recipient, subject and date sent, for example. Interestingly, Gmail, for example, omits table headings which suggests that tables are less appropriate for an inbox.

Alternatively we construct each email as a list item. On big screens, we could still style the information into columns. This brings us to the first problem. Tables aren't especially responsive. Tables are semantically tied to the way they look. That is, it's hard to make tables not *look* like tables. There are some ways to make tables work on small screens but they aren't simple and they don't work well cross-browser. However, striving to make tables (or any other element) not look like a table is materially dishonest[^].

Tables *are* a good choice when data needs contextual information to make it useful or if the data in those rows need comparing or sorting or summarising (as totals). For example, *23* is ambiguous without *goals scored* and *Lionel Messi* as column and row headings respectively. And we might also be interested in comparing Lionel Messi's statistics in comparison with Cristiano Ronaldo's.

[!table](.)

An inbox seems less tabular: it doesn't particularly need to be sorted (beyond the default of most recent first). Emails don't need to be compared, or summarised. It would be quite readable if an email was simply read it as ‘From Heydon, about “Buttons” (19/09/2017)’. It could be argued that including column headings are verbose and unnecessary. Mailchimp, which has a similar interface is an example that employs list items:

![Mailchimp List](./images/mailchimp-list.png)

The advantage over tables is that they are maleable and therefore responsive. On small screens we can stack the information within each list item. On big screens we can position the information into columns. Talking about lists may seem out of place in a book about forms, but they don't exist in a vaccum. They are a major part of an interface, but they rarely form an interface on their own.

On balance, list items are the preferred construct. This is not to say tables are bad and list items are good; we can't classify elements like that. It's a matter of understanding what they're good for and their associated constraints.

How it might look:

![Inbox with one checkbox selected and the actions exposed](./images/inbox.png)

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

We'll add the form controls shortly. Interestingly, the act of constructing the HTML exposes another problem. The content is wrapped in a link which is not something we can do with tables&mdash;it's semantically invalid.

As designers, we should have a deep understanding of the materials we use to build artifacts. For example, a chair designer should intimately know how wood can be used to craft chairs. In our case the materials are HTML, CSS, Javascript. And the artefacts are web pages.

> ‘If you can solve a problem with a simpler solution lower in the stack, you should.’—Derek Featherstone

Gmail uses tables and yet the entire row is clickable. Google's developers use Javascript to fix this. But, as we've previously discussed, this is an act of exclusivity because not everybody has Javascript. Besides if we can fix this problem without excluding people we should. Inclusivity is really just a set of constraints that guide us to create robust, and therefore, better experiences for all.

## Marking email for action

To allow users to select and action multiple emails at once, we'll need to add a checkbox next to each email. How it might look:

![Inbox](./images/inbox.png)

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

Unlike all other fields in the book so far, the checkbox has a label missing. In *almost* all cases, a visible label should be placed with the checkbox. However, this is a bit of a special case because the interface handles two disperate jobs. That is, to view emails and to *manage* them too.

In this case we can probably get away without having visible labels. In fact having a visible label in this interface interfers with the navigation behaviour of the row itself. We can't have a `label` and a link occupying the same space. Fortunately, there are 2 solutions. The first is to use modes, the second is to hide the label.

### Use modes

As we said before, our interface is a little complicated because it's trying to do 2 jobs. Instead we could split these jobs out and use modes. Affectively this means have two interfaces. One that is read-only and one that is for management.

When in read-mode there are no forms and no checkboxes. Clicking the row takes the user to the email. When in manage-mode, the row becomes the `label` and therefore clicking it activates the checkbox, just like it does in all the other forms we've designed so far.

![Modes](./images/modes.png)

Modes work well, particulary if one is used a lot more than the other. But if both are used equally, then having to switch between them is somewhat undesirable. This seems applicable to our inbox interface. Instead we can visually hide the label.

### Visually hide the label

There are two ways to create a hidden label. One of the simpler and least verbose ways is to use `aria-labelledby`:

```
<ul class="inbox">
	<li>
		<input type="checkbox" name="email" aria-labelledby="e1_recipient e1_subject e1_date">
		<a href="/emails/1/">
			<div class="inbox-recipient" id="e1_recipient">From Heydon Pickering</div>
			<div class="inbox-subject" id="e1_subject">Subject: Buttons</div>
			<div class="inbox-date" id="e1_date">19/09/2017</div>
		</a>
	</li>
	...
</ul>
```

The downside is that there is less support for ARIA than there is if we were to use the native equivalent of a `label`. Using a label  means duplicating the contents and visually hiding the label with CSS.

```
<ul class="inbox">
	<li>
		<input type="checkbox" name="email" aria-labelledby="e1_recipient e1_subject e1_date">
		<a href="/emails/1/">
			<div class="inbox-recipient" id="e1_recipient">From Heydon Pickering</div>
			<div class="inbox-subject" id="e1_subject">Subject: Buttons</div>
			<div class="inbox-date" id="e1_date">19/09/2017</div>
		</a>
	</li>
	...
</ul>
```

Duplicating the contents bloats the HTML which can diminish the experience for many users since many operations will take longer. Assistive technology users especially may find their software unresponsive.

Much to our frustration, *perfect* rarely exists in design. On balance, using a visually hidden label creates a little bit of a bloat in exchange for a more inclusive experience. In fact having a visually hidden label is an opportunity to improve the experience for screen reader users. Note below that the label reads *From Heydon Pickering about Buttons (19 September 2017)* which works better audibly.

```HTML
<fieldset class="inbox">
	<legend>Inbox: 26 emails</legend>
	<ul>
		<li>
			<input type="checkbox" name="email" id="email1" value="1">
			<label for="email1">From Heydon Pickering about Buttons (19 September 2017)</label>
			<a href="/emails/1/">
				<div class="inbox-recipient">Heydon Pickering</div>
				<div class="inbox-subject">Subject: Buttons</div>
				<div class="inbox-date">19/09/2017</div>
			</a>
		</li>
		...
	</ul>
</fieldset>
```

Here's the CSS to visually hide the label.

```CSS
.inbox label {
	position: absolute !important;
    clip: rect(1px, 1px, 1px, 1px);
    padding:0 !important;
    border:0 !important;
    height: 1px !important;
    width: 1px !important;
    overflow: hidden;
}
```

### Highlighting marked emails

The deal with human-computer interaction is that when the human does something, the computer should respond. In this case, clicking a checkbox makes a little tick appear (and disappear) accordingly. In all likeliness this is enough, though we could highlight the entire row using Javascript.

As designers, we're tempted to do more than the minimum. We think that more is better. We think that more is a symbol of hard work. It's actually a lot easier to do more than it is to do less. Constantly striving for less in a world that pats you on the back for doing more is very hard work indeed.

If thorough and diverse testing shows a highlight is really needed then go ahead. But, by not highlighting the row, we have less work to do and the interface is as performant as possible.

Mailchimp, known for their usability prowess don't bother highlighting the rows. The checkbox is enough.

![Mailchimp List](./images/mailchimp-list.png)

## An action menu

It's all well and good allowing users to select multiple emails, but our interface is lacking the ability to action them. Unlike the forms in previous chapters, an inbox is a bit different. Firstly it's not a simple matter of placing a button at the end of the form. Secondly this form contains multiple submit buttons. We're going to discuss all this and more now.

### Button location

Traditionally, submit buttons are placed after form fields. Up to now this is exactly what we've done. This made sense, as users typically have to answer the questions from top to bottom. And they submit once they've filled out the form entirely.

Users who wish to select and act on individual emails won't necessarily step through each and every checkbox. Having buttons at the bottom of a long list doesn't aid discovery of that functionality either. This is why we place them at the top.

### Implicit submission and multiple submit buttons

Implicit submission allows the user to press <kbd>enter</kbd> while a field is focussed. In doing so the form is submitted as if the user pressed the first button in the HTML. When there is a single submit button this is not an issue. When there are multiple, it's not obvious which action will be taken.

Where possible, we should try and split forms so that they have a single, and therefore intuitive action. It's normally easy to design solutions for this. For example, if you have a form that allows a user to update or delete the record, you can just have two separate forms:

![](.)

Our inbox is a special case. As such it's not quite so obvious as to how we might split out the actions into separate forms. One way, would be for users to first select an action (such as ‘Bulk deletion’). Clicking it takes the user to a dedicated interface in which to select the emails and apply the action

![Click action link -> present checkboxes (with submit at bottom) ->confirm action](.)

This could work. On the other this approach seems a little long winded in the case of an inbox. At least in putting the buttons first in the flow, users (those who use screen readers and those more abled) have an opportunity to discover the multitude of available actions.

Also, a form consisting of a single checkbox group, doesn't make implicit submission all that useful. Conversely, a search form&mdash;one that consists of a text box and submit button&mdash;would benefit greatly from implicit submission, as users can take the efficient route pressing <kbd>enter</kbd>.

In any case, if a user did implicitly submit the form, we can mitigate the danger in two ways. First by putting the least invasive action first (such as archive). Secondly by letting users undo their action. This is useful experience regardless and we'll be talking about this more shortly.

### Disabling and hiding the submit buttons before selection

You may think it's a good idea to disable (or hide) the buttons until users selects at least one email. But we already discussed the problems with disabling submit buttons in the first chapter.  similarly, hiding the buttons makes the actions less discoverable. A clean interface is good, but not at the cost of clarity.

### Creating a menu

On big screens, or responsively speaking, when there is space, laying out the buttons horizontally is fine. However, an inbox may have other features and there may not be enough space (even on big screens) to present them comfortably.

[Blah](.)

Similarly, on small screens the buttons are likely to stack beneath each other pushing the inbox itself down the page. Having the buttons dominate the interface like this is problematic. *Dominance* is a quality we should use sparingly. After all, if everything dominates, nothing does. The inbox should take center stage with the menu taking a more subtle role in the interface.

![Blah](.)

To keep the interface clean but easy to scan we hide the options behind a menu button. There are 2 ways to create a menu. The first is by using a `select` box which as we'll find out isn't a true menu. The second is by building a true menu using HTML, CSS and Javascript.

### Select box menu

Select boxes are a menu of sorts. They present items for selection (like a menu) and they're an attractive option because browsers supply them for free. Even though select boxes look like menus and behave a little like them, they *aren't* menus.

Select boxes are for input. That's why (forms that contain) select boxes&mdash;like any other input&mdash;must be accompanied by a submit button (to submit the choice). Not only is this by convention, but it's also in WCAG2.0:

> Changing the setting of any user interface component does not automatically cause a change of context.

This coincides with inclusive design principle 4, *give control*. Select boxes that automatically submit the form `onchange` *takes away control*. Unsuprisingly then, this causes problems. In this case, screen reader and keyboard users. On Chrome (Windows), for example, the form is submitted as soon as the user moves to the next option when pressing <kbd>down</kbd>. Getting to the option after that is either hard or impossible[^].

This is not a browser bug. It's just that some browsers are more forgiving than others. The forgiving ones wait until you press <kbd>space</kbd> or <kbd>enter</kbd> before submission. This lets users move through the options with the arrow keys. As we know, not all browsers are alike or implement the specification in the same way. Therefore, forgetting about people who use a less forgiving browser is an act of exclusivity.

This information easily discredits the use of a select box as a menu. However, there are other problems worth noting. A select box is always collapsed but when there is enough space (on big screens) we want to expose the options making them easier to scan and select. We could use Javascript to create vastly different experiences on small and big screens, but this is an adaptive approach to design and goes against the very foundation of responsive design[^].

[!Show adapative layout differences](.)

Practically speaking, this creates more work for us, and a more computationally heavy interface for the browser to render. It also adds complexity on the server because it has to be ready to handle the way select boxes and submit buttons transmit data to essentially perform the same task. The select box will send `selectName="value"` and the buttons send `buttonName="value"`.

### A true menu

HTML doesn't have a ‘menu’ element so we need to build our own. The basic HTML looks like this:

```HTML
<div role="menubar">
  <input role="menuitem" type="submit" name="archive" value="Archive">
  <input role="menuitem" type="submit" name="delete" value="Delete">
  <input role="menuitem" type="submit" name="spam" value="Mark as spam">
</div>
```

The menu takes the (aptly named) `menubar` role indicating this element contains a menu items. That's why each submit button is given a role of `menuitem`, letting screen readers announce the component as a three-item menu. Visually the three buttons are grouped together. So all we've really achieved in using ARIA is to denote this grouping for those using screen readers.

On small screens, the menu items stack beneath each other as there is no room to present them next to each other. Javascript is used to detect when the screen is small to then enhance the component into a traditional collapsed menu with all the expected interactive qualities which I'll explain shortly.

How it might look:

![Menu](./images/etc.png)

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
- The `<span>` contains the unicode character representing a down arrow. Conventionally this indicates visually what `aria-haspopup` does non-visually&mdash;that pressing the button reveals something below it. The `aria-hidden="true"` attribute prevents screen readers from announcing “down pointing triangle” or similar. Thanks to `aria-haspopup`, it’s not needed in the non-visual context.
- The `aria-haspopup` attribute is complemented by `aria-expanded` which tells users whether the menu is currently expanded or collapsed by toggling between `true` and `false` values.
- The role is now set to `menu` instead of `menubar` because it now expands and collapses. Conversely a `menubar` is always visible.
- Pressing the button shows the menu and moves focus to the first `menuitem`.
- Pressing <kbd>down</kbd> or <kbd>up</kbd> on a `menuitem` moves to the next or previous item (on loop) as before.
- Pressing <kbd>escape</kbd> on a `menuitem` moves focus to the menu button and closes the menu.
- All `menuitems`s have `tabindex="-1"` which means pressing <kbd>tab</kbd> won't move focus to the buttons. We purposely let users use the arrow keys to traverse the menu because this saves the user having to wade through each of the menu items to get to the next interface component. Pressing <kbd>tab</kbd> once achieves this and mimics the behaviour of menus found in standard computer software.

---

## Select all

Users may want to, for example, archive every email in their inbox. Rather than having to select each manually, we can create a more convenient method. One way to service this functionality is through a *special* checkbox&mdash;placed at the top, in vertical alignment with the other checkboxes to create a visual connection. Clicking it marks the checkbox and every other checkbox at the same time.

[!Checkbox mailchimp?](.)

Arguably, this out-of-the-box input has all the ingredients of an accessible control; it’s screen reader and keyboard accessible. It communicates through it's label and change of state. It's label would be *Select all* and it's state would be announced as *checked* or *unchecked*. Without JavaScript, we’ve handled state, and screen readers are able to give feedback to users.

Despite our natural inclination to lean on native technology. And despite the fact that this type of control is accessible by mouse, touch, keyboard and assitive technology, it just doesn't quite feel right. Accessibility is only a part of inclusive design. These controls have to make sense and give clarity as to their affordance.

The trouble with using checkboxes&mdash;much like using a select box as a menu&mdash;is that they associated with collecting data for submission. Ultimately users expect toggle buttons to be buttons and checkboxes as a vessel for input.

Instead we'll create a true toggle button. HTML doesn't have a toggle button, it just has `button`. This generic button of type button (not submit) should be your go-to element for changing anything with Javascript. That is except for changing location which is the responsibility of links.

```HTML
<button type="button" aria-pressed="true">Select all</button>
```

```JS
Put code here
```

Notes:

- The `aria-pressed` attribute tells the user the state of the button by toggling between `true` and `false` values.
- Pressing the button marks all the checkboxes as checked. Pressing it from a second time unchecks them back to their original state.

## Success messages

When the user submits the form, the emails marked for action will disppear from the inbox. Like error messages, success messages communicate success. It's an obvious thing to do that gives users respect. Without a message, the user is left to wonder if what they intended, did indeed happen and gives users a positive anxiety-reducing feeling of calm.

How it might look:

![Success](.)

```HTML
<div role="alert" class="success">
	You've successfully archived 5 emails
</div>
```

Notes:

- `role="alert"` ensures that screen readers announce the message immediately as soon as the page loads.
- Crucially, the message is always visible. Some websites will hide this sort of message after some time has elapsed (a few seconds or what have you), but you shouldn't do this as this takes control away from the user to read it (and perhaps dismiss it) in their own time.
- If you want to let users dismiss the message, then add a button with Javascript so that pressing it hides the message.

### Confirming versus undo

As a safety measure, some roads have speed bumps. They cause drivers to slow down on roads that are more likely to cause accidents. We can achieve the same thing in an interface by asking users to confirm their action:

![Are you sure](./images/etc.png)

This is fine for tasks that are performed infrequently but it quickly gets tedious when they need to be invoked more often. To continue with the driving analogy, it's a bit like putting speed bumps on motorways. The alternative is to perform the action immediately and offer users the ability to undo their action.

![Undo](./images/undo.png)

Clicking *undo* reverses the previous action, in this case restoring the emails back into the inbox. If only we could *undo* road accidents.

## Summary

In this chapter we began by choosing the right way to present a collection of emails and the impact of combining 2 features (reading email and managing it) into 1 interface. We then realised that multi-select forms are very different to the more standard forms discussed in previous chapters. Therefore it caused is to think differently about how to design the interface from a visual and interactive stand point. This first consisted of how to present and interact with an action menu. And second, how to give users feedback after successfully applying one of those actions.

### Things to avoid

- Using checkboxes and select boxes for functionality beside collecting data.
- Highlighting entire rows without first testing with users.
- Disabling and hiding submit buttons until the form becomes valid.

## Footnotes

[^]:Blah