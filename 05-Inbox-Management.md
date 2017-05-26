# Inbox Management

My sister loves lists. Her favourite list is a todo list. In fact she loves lists so much, that one of her favourite things is making new lists out of old ones.

Despite her obsession, the world is full of lists. There is even a list of great people[^]. But lists are a tricky thing to manage. On the web, there are some conventions that have emerged over the years.

In this chapter, we're going to make sure list management is easy, accessible and scalable. My sister loves pen and paper, but I hope that she may one day be converted to a digitally managed list.

On the web there are several elements that represent lists:

- tabular data (`<table>`)
- description lists (which used to be called definition lists) (`<dl>`)
- unordered lists (`<ul>`)
- ordered lists (`ol`).

We can't discuss the merit of these until we orientate ourselves around a  specific problem that needs solving. To do this, we'll design an inbox. The aim of course is to achieve a zen-like state of Inbox Zero[^].

To get to Inbox Zero our UI must enable users to delete, archive and mark spam. But not just one at a time. In bulk.

Whilst this chapter is about email, the principles and design patterns are applicable to most, if not all types of lists.

## Everything is list

Semantically speaking everything is a list. The things on the page are a list of things on the page. Pedantism aside, we need to decide what type of list is best for our inbox.

Tables work when representing two-dimensional data. In our case, rows represent emails and columns represent the recipient, subject and sent date etc. Interestingly, Gmail omits table headings, suggesting that a table is the wrong choice in this case.

We could represent rows as list items and (at least in big screens) make them *look* like columns. This brings us to the first problem. Tables aren't the most responsive component.

Tables are semantically tied to the way they look. Meaning it's hard to make tables not *look* like tables. There are some solutions, but they are not particularly cross-browser friendly.

Tables *are* a good choice when the data needs contextual information to make it useful. For example, *23* is useless information without the context of *goals scored* as a column heading and *Lionel Messi* as a row heading.

Our inbox, however, is seemingly less tabular. It's a list of emails that if read out as "From Heydon, subject: Buttons, 19/09/2017 at 9am" would be quite readable. You might even argue that the verbosity of having column headings is something that hinders, as opposed to helps the experience.

Mailchimp, which is not an inbox, has a similar interface to Gmail but *does* use list items instead of tables:

![Mailchimp List](./images/mailchimp-list.png)

It looks the same, but the advantage of list items over table rows is that they are maleable. We can style them differently and more appropriatelty for small and big screens alike.

Semantics on the web is hard. Only once we take a step back and critique solutions from several angles does the "right" solution show itself.

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

Designers should have a deep understanding of the materials they use to build artifacts. In our case the materials are HTML, CSS, Javascript. And the artefacts are web pages.

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

### Menu design

Just before we decided that the buttons would always be visible, but we didn't go into any detail as to how they would look.

We could show the buttons like this:

![Buttons in a row above table](./images/etc.png)

Naturally, this works on big screen as there's plenty of space. On small screens they would stack which is okay but they would dominate the screen. Dominance is a quality we need to use sparingly. If everything dominates, nothing does.

Similarly if there is other UI elements, there may not be enough room, even on big screens. In this case we're going to want to apply some *show hide* magic. But how?

There are two options that come to mind:

- Select box
- Responsive ARIA menu

#### Select box

#### Responsive ARIA menu

- On small screens, hidden menu and aria
- On big screen, exposed menu, no aria?

---

### Action buttons versus select box

- Sometimes it's not just a button, sometimes it's a selection, like apply filter. But better to go to other page probably. Like for Universal Credit, we have a page that selects people to be allocated to another agent. That means the user selects people, then chooses who to allocate it to then presses the button. Better as a flow, at least to start with.
- other thing is, can have modes. Modes that say "browse mode" or "manage mode". Only show checkboxes when users wants to manage stuff. One thing per page principle type thing.

This little gizmo has much to be discussed.

We could use a select box, one of the rare cases where saving space like this makes sense. But always use a submit button, and don't apply the change when the user changes options.

MISUSING THE SELECT BOX

The problem with using a select box despite the general disadvantages as we've discussed at length in Book A Flight is that they don't allow us to enhance the experience for big screens. That is to display the options when there is enough space to do so.

For this reason we might consider a ARIA TOOLBAR. A div with a button that exposes more buttons on small screens, and on big screens, just expose the buttons without showing/hiding (or aria-expand/collapse).

As we know we don't want to hide options if there is room to show them.

## Success messages

When the user submits the form, the page will refresh, and the item will be gone, if the user deleted (or archived). In this case it would be wise to make this clear to the user with a success message.

Like error messages, success messages are of vital importance. They leave users feeling good, and in control. When we built the checkout flow, we had a gigantic success message at the end in the form of an order confirmation.

In this case, we need a message in context. A toast message is named after toast popping up after a toaster has had its way with a piece of bread. The thing to know about toast messages is that they shouldn't disappear without users dismissing them.

Some users may not notice them at first. Some users may be using a screen reader, and not have read it out yet. Some users may notice it, but take a really long time to read the message.

Whatever it is, don't dismiss the message automatically.

Also, within the success message we may provide users with an undo feature, that let's them undo their last action. This often causes less friction, then asking users to confirm their action "Are you sure you want to delte this item?" etc.

In this case this undo feature will be part of the success message like gmail:

![Undo](./images/undo.png)

## (De)Select all (TODO)

With a list of my favourite products, or a list of emails, quite often we might want to select and act upon all options in a list. We may want to, for example archive all the emails. Having a (de)select all button makes sense.

- Without JS? Use buttons to reload the page with all selected.
- With JS piggyback the button/link to not do a post back.
- checkbox indeterminate state.
- button aria-pressed?
- Could just have a button that doesn't rely on checking boxes. "Delete all" for example.

## Summary

TODO
