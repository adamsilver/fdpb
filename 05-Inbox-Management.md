# Inbox Management

My sister loves lists. Her favourite list is a todo list. In fact she loves lists so much, that one of her favourite things is making new lists out of old ones.

Despite her obsession, the world is full of lists. There is even a list of great people[^]. But lists are a tricky thing to manage. On the web, there are some conventions that have emerged over the years.

In this chapter, we're going to make sure list management is easy, accessible and scalable. My sister loves pen and paper, but I hope that she may one day be converted to a digitally managed list.

On the web there are several elements that represent lists:

- tabular data (`<table>`)
- description lists (formely known as *definition lists*) (`<dl>`)
- unordered lists (`<ul>`)
- ordered lists (`ol`)

We can't discuss the merit of each of these until we orientate ourselves around a specific problem that needs solving. To do this, we'll design an inbox. The aim of course is to achieve a zen-like state of Inbox Zero[^].

To get to Inbox Zero our interace must enable users to delete, archive and mark emails as spam. But not just one at a time. In bulk.

Whilst this chapter is about email, the principles and design patterns are applicable to most, if not all types of lists that need to be operated on in bulk.

## Everything is list

Semantically speaking, everything is a list. The things on the page are a list of things on the page. Pedanticism aside, we need to decide what type of list is best for our inbox.

Tables work when representing two-dimensional data. In our case, rows represent emails and columns represent recipient, subject and date sent. Interestingly, Gmail omits table headings, suggesting that tables are less appropriate in this case.

We could represent rows as list items and (at least in big screens) visually present them as columns. This brings us to the first problem. Tables aren't especially responsive.

Tables are semantically tied to the way they look. Meaning it's hard to make tables not *look* like tables. There are some solutions, but they are not particularly cross-browser. However, striving to make tables (or any other element) not look like a table is materially dishonest[^].

Tables *are* a good choice when the data needs contextual information to make it useful. For example, *23* is ambiguous without *goals scored* and *Lionel Messi* as column and row headings respectively.

Our inbox, however, is seemingly less tabular. It's a list of emails that if read out as ‘From Heydon, subject: Buttons, 19/09/2017 at 9am’ would be quite readable. You might even argue that the verbosity of having column headings is something that hinders, as opposed to helps the experience. Mailchimp, which is not an inbox, has a similar interface to Gmail but uses list items:

![Mailchimp List](./images/mailchimp-list.png)

It looks the same, but the advantage of list items over table rows is that they are maleable. We can style them differently and more appropriatelty for small and big screens alike. Semantics on the web is hard. Only once we take a step back and critique solutions from several angles does the ‘right’ solution present itself.

This may seem a bit out of place in a book about form patterns but forms aren't something that exist in a vaccum. They are a major part of an interface, but they rarely form an interface on their own.

On balance, an unordered list is preferential. This is not to say tables are bad. We can't classify elements as *good* and *bad*. We simply need to understand their meaning, their behaviour and their constraints. It's better to classify elements as *appropriate* or *inappropriate* for the given problem.

How it might look:

![Inbox](./images/inbox.png)

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

Here's the HTML without any form controls. We'll add those in shortly. Interestingly, the act of writing the HTML exposes another problem. The content is wrapped in a link which is not something we can do with tables&mdash;it's semantically invalid.

As designers, we should have a deep understanding of the materials we use to build artifacts. For example, a chair designer should intimately know how wood can be used to craft chairs. In our case the materials are HTML, CSS, Javascript. And the artefacts are web pages.

> ‘If you can solve a problem with a simpler solution lower in the stack, you should.’—Derek Featherstone

Gmail uses tables and yet the entire row is clickable. Google's developers use Javascript. But, as we've previously discussed, this is an act of exclusivity.

Wherever possible we should avoid solutions that exclude people. Inclusivity is really just a set of constraints that guide us to create robust, and therefore, better experiences.

## Enabling selection

To allow users to select and action multiple emails at once, we'll need to add a checkbox to each item in the list.

How it might look:

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

In this case we can probably get away without having visible labels. In fact having a visible label in this interface interfers with the navigation behaviour of the row itself. We can't have a `label` and a link occupy the same space.

There are 2 solutions:

- Use modes
- Visually hide the label

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

## Highlighting rows

The deal with human-computer interaction is that when the human does something, the computer should respond. In this case, clicking a checkbox makes a little tick appear (and disappear) accordingly. In all likeliness this is enough, though we could highlight the entire row using Javascript.

As designers, we're tempted to do more than the minimum. We think that more is better. We think that more is a symbol of hard work. It's actually a lot easier to do more than it is to do less. Constantly striving for less in a world that pats you on the back for doing more is very hard work indeed.

If thorough and diverse testing shows a highlight is really needed then go ahead. But, by not highlighting the row, we have less work to do and the interface is as performant as possible.

Mailchimp, known for their usability prowess don't bother highlighting the rows. The checkbox is enough.

![Mailchimp List](./images/mailchimp-list.png)

## Submit buttons

It's all well and good allowing users to select multiple emails, but our interface is lacking the ability to action them. Unlike other forms in this book, the inbox is a little bit different. Firstly it's not a simple matter of placing a button at the end of the form. Secondly this form contains multiple submit buttons. We're going to discuss all this and more now.

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

### Disabling buttons before selection

Theoritically the user shouldn't perform an action until they select at least one checkbox. It's this idea that can tempt us to disable the action buttons until the user selects a checkbox.

In chapter one, however, we discussed the associated problems with disabling buttons until a form is valid. Our inbox doesn't look like a traditional form but it suffers from the same problems as any other.

Like all the other forms we've designed so far, we'll ensure the buttons are always enabled.

### Hiding buttons before selection

Our interface is filling up quickly. And we haven't really considered other UI elements that may make up the inbox.

We're not always selecting an email for bulk actioning. For this reason we might consider hiding the buttons until the user selects at least one checkbox.

Both Mailchimp and Gmail do exactly this. Still, whenever we hide piece of UI, we rely on users discovering functionality.

Sometimes this is worth it, sometimes it's not. Like most things in design, it depends.

One good reason to hide the buttons is because users are not always interacting with the inbox. They may simply be browsing and reading their inbox. Why clutter the UI in this case? It may prove distracting.

One bad reason to hide is that it's extra work and requires discovery.

One principle we've followed throughout this book, is doing the simplest thing first and testing. It makes little since to do the hardest thing first and delete. This is far more costly and time-consuming. It's risky.

We'll stick to ever-present buttons and keep in mind to test how this works. We can always hide them later if user testing shows it's a problem.

Having multiple action buttons on small screens may prove tricky. This is something we'll be discussing shortly.

## Menu

Just before we decided that the buttons would always be visible, but we didn't go into any detail as to how they would look.

We could show the buttons like this:

![Buttons in a row above table](./images/etc.png)

Naturally, this works on big screens with plenty of space. On small screens they would stack which is okay excusing the fact they would dominate the screen. Dominance is a quality we need to use sparingly. If everything dominates, nothing does.

Similarly if there are other UI elements, there may not be space to present them comfortably—even on big screens. In this case it's useful to hide the actions behind a menu.

There are two options:

- Select box
- Responsive ARIA menu

### Select box

We know select boxes are problematic because we discussed them in chapter 3 *Book a Flight*. But, we'll discuss again here because they are often used as a replacement for menus.

They are a menu of sorts. They present items for selection (like a menu). And as they are provided by the browser, for free, they require no extra work.

However, select boxes look like menus and act a little like a menu, they aren't menus. Select boxes are for input. Menus are for taking action. This is why a select box like this must always have an accompanying button (to submit the choice).

Moreover, select boxes that submit onchange are a problem for users that use keyboards and screen readers[^]. On Chrome Windows, for example, the form is submitted as soon as the user moves to the first option, making it hard (or impossible) to select the second.

This is not a browser bug, it's just that some browsers are more forgiving and wait until the user presses *space* or *enter* to make the submission. The real problem is that a control that is meant for input has been used to create a menu like thing.

All other forms up to now have kept selection/input and submission separate. This is something that WCAG2.0 also recommends[^]:

> “Changing the setting of any user interface component does not automatically cause a change of context”

Lastly, we want to display the actions on big screens where there is enough room to do so. A select box, however, always starts in a collapsed state. We'd have to write Javascript to change the interface.

The server-side would also have to recognise input from button actions as well as input from the select box.

Instead, we'll construct a true and responsive menu using ARIA next.

### Responsive ARIA Menu

Our menu will be responsive. It will work well on small screens and big screens. Here's what it will look like on big screens:

![Menu](./images/etc.png)

Here's the HTML:

```HTML
<div role="menubar">
	<input role="menuitem" type="submit" name="archive" value="Archive">
	<input role="menuitem" type="submit" name="delete" value="Delete">
	<input role="menuitem" type="submit" name="spam" value="Mark as spam">
</div>
```

#### Notes

- The role menubar and menuitem indicates that the user has entered a menu and will announce how many menuitems there are.
- Submit buttons are focusable using the tab key. On this occasion we'll disable this functionality as a menu should be navigable using the arrow keys. The advantage is that users don't have to move through every menu item to leave the menu. This is useful enhancement, especially where there are many menu items.
- To do this, each button is given a tabindex of -1, except the first. This makes the menu items unfocusable with the tab key but allows us to programmatically set focus when the user presses the arrow keys.
- We don't set the tabindex on the first button so that the user can tab into the menu. Once the user is within the menu we give the first button a tabindex of -1 to match the others.
- Pressing right on a menu item moves to the next item (on loop).
- Pressing left on a menu item moves to the previous item (on loop).

On small screens we'll need to tweak the layout and behaviour. As there is no room to present all the items, we'll hide them behind a tradional menu that expands and collapses.

This is How it might look:

![Menu](./images/etc.png)

HTML:

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

#### Notes

- The aria-haspopup property indicates that the button shows a menu. It acts as warning that, when pressed, the user will be moved to the “popup” menu.
- The <span> inside the button contains the unicode point for a black down-pointing small triangle. This convention indicates visually what aria-haspopup does non-visually — that pressing the button will reveal something below it. The aria-hidden="true" attribution prevents screen readers from announcing “down pointing triangle” or similar. Thanks to aria-haspopup, it’s not needed in the non-visual context.
- The aria-haspopup property is complemented by aria-expanded. This tells the user whether the menu is currently in an open (expanded) or closed (collapsed) state by toggling between true and false values.
The menu itself takes the (aptly named) menu role. It takes descendants with the menuitem role.
- The role is now menu instead of menubar. Menu is used because it expands and collapses. A menubar is ever present.
- Pressing the button, shows the menu and moves focus to the first menu item.
- Pressing down on a menu item moves to the next item (on loop).
- Pressing up on a menu item moves to the previous item (on loop).
- Pressing escape on a menu moves to the menu button and closes the menu.
- All menuitems's have a tabindex of -1. It's the single menu button that is focusable by the tab key making this aspect slightly more straightforward to code.

There final Javascript:

```JS
```

## Select all

Users may want to archive all emails in their inbox for example. Rather than having to endlessly select each checkbox manually, we can offer users the convenience of a button, and let scripting do the heavy lifting.

This would become an additional action but separate to the action menu itself. Or perhaps it should be in the same menu with a separator?

This feature is often provided as an enhancement but we might consider offering it to users without Javascript too. Providing a button that refreshes the page with all checkboxes checked is straightforward and doesn't have to rely on Javascript.

We can then enhance the experience using Javascript, meaning users don't have to wait for the page to refresh.

Often a user interface will have a checkbox to represent *select all*. Even though it's become convention it's not really the right element for the job, semanticall speaking.

Instead, we'll use the already available button and enrich it with a little ARIA.

```HTML
<button aria-pressed="false">Select all</button>
```

And the Javascript:

```JS
Loop through all checkboxes and check them
update aria-pressed to true
```

## Success messages

When the submits the form successfully, the page will refresh and the emails disappear from their inbox. It's wise to notify users of this action as without a clear message the user is left to wonder if it worked. This is particularly the case if they have marked many emails in an inbox full of emails.

Like error messgaes, success messages promote trust and calm. It leaves the user feeling good and in control. Two qualities that make the user feel awesome as they get close to Inbox Zero.

In checkout, for example, the user ends up on a dedicated confirmation screen. We could have a confirmation screen, but it seems a bit long-winded to use this approach for managing an inbox.

Instead, we'll need a success message at the top of the page, much like the error message alert panel we designed in chapter one.

Here's How it might look:

![etc](./images/etc.png)

HTML:

```HTML
<div role="alert" class="success">
	You've successfully archived 5 emails
</div>
```

### Notes

- The role of alert means the panel is announced immediately for those using screen readers
- The message stays on screen. It doesn't automatically hide. This is a poor experience. It either hangs around for too long, or it disappears before the user had a chance to read it.
- Without a Javascript, navigating away will dismiss the message.
- With Javascript, we can enrich the experience by adding a dismiss button. Clicking it will hide the message.

### Confirming versus undo

Roads have speed bumps in order to slow drivers down to keep them safe. In much the same way we can slow users down and keep them safe by asking them to confirm their action:

![Are you sure](./images/etc.png)

This works okay for infrequent tasks. But for tasks that are performed frequently, this is a frustrating source of friction.

Instead we might offer users an undo feature. This is Gmail's:

![Undo](./images/undo.png)

To do this, the success messgae needs to contain a button with the words *Undo*. Clicking it will undo their previous action and restore the emails back into their inbox.

## Summary

TODO

## Footnotes

TODO

## Other considerations

- Selecting all, but spanning several pages (Gmail)
- Hover vs click (for aria menu)
- checkbox indeterminate state.
