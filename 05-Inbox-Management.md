# Inbox Management

My sister loves lists. Her favourite list is a todo list. In fact she loves lists so much, that one of her favourite things is making new lists out of old ones.

Despite her obsession, the world is full of lists. There is even a list of great people[^]. But lists are a tricky thing to manage. On the web, there are some conventions that have emerged over the years.

In this chapter, we're going to make sure list management is easy, accessible and scalable. My sister loves pen and paper, but I hope that she may one day be converted to a digitally managed list.

On the web there are several elements that represent lists:

- tabular data (`<table>`)
- description lists (which used to be called definition lists) (`<dl>`)
- unordered lists (`<ul>`)
- ordered lists (`ol`)

We can't discuss the merit of these until we orientate ourselves around a specific problem that needs solving. To do this, we'll design an inbox. The aim of course is to achieve a zen-like state of Inbox Zero[^].

To get to Inbox Zero our UI must enable users to delete, archive and mark spam. But not just one at a time. In bulk.

Whilst this chapter is about email, the principles and design patterns are applicable to most, if not all types of lists that need acting upon.

## Everything is list

Semantically speaking everything is a list. The things on the page are a list of things on the page. Pedanticism aside, we need to decide what type of list is best for our inbox.

Tables work when representing two-dimensional data. In our case, rows represent emails and columns represent the recipient, subject and sent date etc. Interestingly, Gmail omits table headings, suggesting that tables are less appropriate in this case.

We could represent rows as list items and (at least in big screens) make them *look* like columns. This brings us to the first problem. Tables aren't the most responsive of elements.

Tables are semantically tied to the way they look. Meaning it's hard to make tables not *look* like tables. There are some solutions, but they are not particularly cross-browser friendly. However, striving to make tables (or any other element) not look like a table is materially dishonest.

Tables *are* a good choice when the data needs contextual information to make it useful. For example, *23* is useless information without the context of *goals scored* as a column heading and *Lionel Messi* as a row heading.

Our inbox, however, is seemingly less tabular. It's a list of emails that if read out as "From Heydon, subject: Buttons, 19/09/2017 at 9am" would be quite readable. You might even argue that the verbosity of having column headings is something that hinders, as opposed to helps the experience.

Mailchimp, which is not an inbox, has a similar interface to Gmail but *does* use list items:

![Mailchimp List](./images/mailchimp-list.png)

It looks the same, but the advantage of list items over table rows is that they are maleable. We can style them differently and more appropriatelty for small and big screens alike.

Semantics on the web is hard. Only once we take a step back and critique solutions from several angles does the "right" solution present itself.

This may seem a bit out of place in a book about form patterns but forms aren't something that exist in a vaccum. They are a major part of an interface, but they rarely form an interface all on their own.

On balance, an unordered list is preferential. This is not to say tables are bad. We can't classify elements as *good* and *bad*. We simply need to understand their meaning, their behaviour and their constraints. From here we may choose to classify them as *appropriate* or *inappropriate*.

So here's our inbox:

![Inbox](./images/inbox.png)

HTML:

```
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

Here's the HTML without any form controls. We'll add those momentarily.

Interestingly, the act of writing the HTML exposes another design problem. You'll notice the content is wrapped in a link. This can't be done with tables and is semantically invalid.

Designers should have a deep understanding of the materials they use to build artifacts. A chair designer should intimately know how wood works. In our case the materials include HTML, CSS, Javascript. And the artefacts are web pages.

> If you can solve a problem with a simpler solution lower in the stack, you should.
> —Derek Featherstone

Gmail uses tables and yet the entire row is clickable. For this to happen Google's developers have had to use Javascript. As we've previously discussed, this is an act of exclusivity.

Wherever possible we should avoid solutions that exclude people. Inclusivity is really just a set of constraints that guide us to create robust interfaces and therefore better experiences.

## Enabling selection

To allow users to select and action multiple emails at once, we'll need to add a checkbox to each item in the list.

What it looks like:

![Inbox](./images/inbox.png)

HTML:

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

You'll notice that unlike all other fields in the book so far, the checkbox has a label missing. We can add a label, but the contents of the label will duplicate the contents of the link.

We can't wrap the anchor in the label either as their will be two operations occupying the same space. The label should select the checkbox, the link should navigate etc.

We have three options, each with their own set of tradeoffs:

- Use ARIA attributes in order to connect the information to the checkbox.
- Wrap the contents in a `label` and associate it with the checkbox.
- Create a separate `label` and duplicate the contents of the list item.

### Use ARIA attributes

We could use `aria-describedby` or `aria-labelledby` to associate the information in the list item with the label. However, there are two problems.

First, there is less support for ARIA attributes than there is labels. This is because ARIA came along relatively late in the day[^]. Browsers and assistive technology that came before it won't understand the attributes.

The first rule of ARIA is not to use ARIA[^]. Support is intertwined with inclusivity. If we can provide the same functionality natively without ARIA we should.

Secondly, the size of the hit area is smaller. You'll recall from the first chapter that increasing the hit area helps users with fine motor impairments. We don't want to lose this feature if at all possible.

### Duplicate the contents inside a hidden label

We could duplicate the contents inside of a separate label. The problem is that the label needs to be visually hidden. This causes the HTML to be heavier. And the extra noise could cause problems for those using a screen reader. Two problems we want to avoid.

### Wrap the contents in a label

Instead of duplicating the contents inside a hidden label, we could wrap the contents in a label. Labels have excellent support and so there is no need for ARIA. By the same taken, the entire row becomes clickable which maximises the hit area.

So far so good, but this solution introduces some other problems:

A label can't contain a link because you can't have two interactive elements occupying the same space in the interface. We'd have to remove the link, losing the ability to view the email in detail.

We might consider using *modes*.

We could place a separate button on the page that switches the mode between "read" mode and "edit" mode.

Read mode will have the entire row clickable and not show the checkbox. Edit mode will have a checkbox and the entire row being the label (not a link).

![Modes](./images/modes.png)

Modes work well, particulary if one mode is used far less. But if users are using both modes equally, then it might be undesirable to have to switch all the time.

Instead, we could add a *view* link at the end of each "row". The problem is that the most of the row is a label. Clicking it ticks the checkbox, instead of viewing the email. This seems somewhat undesirable.

However, explicit actions are good. That's because dedicated actions are obvious and obvious is something that makes users feel awesome.

We don't have to limit the row to contain just a *view* link. We can put the other actions within the row too.

![Actions in rows](./images/actions-in-rows.png)

This works quite well. For those that want to quickly delete a single email, clicking the button is far quicker than clicking the checkbox and then hitting the action button.

The trade off is that the interface is full of buttons. Only user testing can tell us which is best. I don't have any personal experience to draw on with regards to an inbox with multiple buttons, so we'll have to make a decision.

### Which to choose?

Much to our collective frustration, *perfect* rarely exists in the design world. And in this case, I'm not sure there is a *perfect* answer here either.

On balance, we'll duplicate the label and visually hide it. A bit of duplication never hurt anyone, and more importantly we have rationalised why we've made the choices we have.

HTML:

```HTML
<fieldset class="inbox">
	<legend>Inbox: 26 emails</legend>
	<ul>
		<li>
			<input type="checkbox" name="email" id="email1" value="1">
			<label for="email1">Heydon Pickering, Buttons, 19/09/2017</label>
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

We can hide the visually duplicated label with CSS:

```CSS
.inbox label {
	// visually hidden
}
```

## Highlighting rows

When a user clicks a checkbox it becomes checked. The user knows this because a tick (or check) appears inside the box. We might be tempted to use Javascript to highlight the entire row which is a reasonable feature to progressively enhance.

As designers and developers, or more broadly speaking: humans, we are often tempted to do more. We think more is better.

We also think doing more is a sign of our hardwork. In fact it's so easy to do more. Your peers will cheer you on when you say you want to do *more*.

Try and refrain from this if you can. Because what if you don't need more? If you don't do more you can spend time solving other problems that need you. If you don't do more the developers won't need to do more either. Same goes for everyoen else on your team who is involve in the delivery of that feature.

Mailchimp, who invest heavily into usability don't highlight the rows. The checkbox state is enough:

![Mailchimp List](./images/mailchimp-list.png)

We'll follow their lead and avoid the extra effort. This doesn't mean the enhancement is never needed. It simply means to do the minimum, then test to see if the investment is worth the cost.

## Submit buttons

We have a form that enables the selection of an email, but no actions to apply to it. We need to add buttons for delete, archive, and marking spam. There are a few design considersatons:

- Button location
- The multiple submit problem
- Disabling buttons before selection
- Hiding buttons before selection
- Menu design

### Button location

Traditionally, submit buttons are placed after the form fields. Up to now this is exactly what we've done. This made sense, as users typically have to answer the questions from top to bottom. And once they have finished answering the questions they can submit.

With a form like this, the same logic doesn't necessarily follow. Users who wish to select and act on individual emails will probably not step through each and every checkbox.

Moreover, placing the buttons at the bottom of a long list doesn't aid discoverability. For these reasons, it makes sense to place the buttons at the top.

We might also duplicate the buttons at the bottom of the form. If the list is long, which might be unavoidable, then making users scroll back to the top seems lazy on our part.

There is another option though. We can use CSS to enhance the buttons so that they are *sticky*. That is they will stay on screen even if users scroll. This is not something to do haphazardly though. Using sticky elements take up space and can be visually noisy, particuarly if users are in browsing mode.

If you want to test this, make sure you do so on a wide range of browsers, screen sizes and users, performing different tasks: browsing and editing.

### The multiple submit button problem

All our forms, up to now, have had a single submit button. Having once choice wherever possible makes it easier for users as it requires less thinking. However, our inbox has multiple actions and therefore many submit buttons.

Multiple submit buttons are problematic due to the way *implicit* submission works with the keyboard. You may have realised you can submit a form by pressing *enter*, when the focus is within a field (as opposed to a button).

If the user does press enter, then the form will submit as if the user had **pressed the first button**. This is the *implicit* bit. This is something that wherever possible we should design out of a system. That is explore alternative ways to provide the functionality without combining forms.

For example, consider the following form that combines *save* and *delete* actions:

![Save and delete form combined](./images/save-and-delete-form-combined.png)

In this case we might consider splitting up the forms into two. One for saving and one for deleting. To make this work we would need to consider the end to end experience&mdash;not just the form in isolation.

Our inbox *could* be designed differently. For example, we could have the actions first. Clicking an action takes the user to a dedicated form where the user selects which emails to apply to the action.

I wouldn't mind seeing this approach in user testing to find out, but lets assume that this is long winded and unintuitive in the case of an inbox.

By putting the buttons first in the flow users have an opportunity to see or hear the actions on offer, and it might stop users using the keyboard to submit a form whilst focussed on the checkbox.

The other thing is, the interface doesn't really afford that of implicit submission, because it's a list of checkboxes. Unlike say a search form, where users type and submit without moving to the button.

Despite this, the user could still submit with *enter*. In this case we can ensure the first submit button is the least dangerous and destructive one. That is make sure that the *delete selected emails* action is not first.

We'll shortly discuss the ability to undo actions like this, but mitigation is a fruitful first step regardless.

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

This is what it looks like:

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

Here's what it looks like:

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
