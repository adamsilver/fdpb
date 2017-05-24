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

To get to Inbox Zero our UI must enable users to delete, archive and mark  spam. But not one at a time. In bulk.

Whilst this chapter is about email, the principles and design patterns are applicable to most, if not all types of lists.

## Everything is list

Semantically speaking everything is a list. The things on the page are a list of things on the page. Pedantism aside, we need to decide what type of list our inbox will use.

We should use tables to represent two-dimensional data. In our case, rows represent emails and columns represent the recipient, subject and sent date etc. Gmail omits table headings. Perhaps this suggests a table is the wrong choice of list.

We could represent rows as list items and&mdash;at least in big screens&mdash;style them visually as columns. This brings us to the first problem. Tables aren't very responsive.

Tables are semantically tied to the way they look. Meaning it's hard to make tables not *look* like tables. There are some solutions, but they are not particularly cross-browser friendly.

Tables *are* a good choice when the data needs contextual information to make it useful. For example, *23* is useless information without the context of *goals scored* as a column heading and *Lionel Messi* as a row heading.

Our inbox, however, is seemingly less tabular. It's a list of emails that if read out as "From Heydon, subject: Buttons, 19/09/2017 at 9am" would be quite readable. You might even argue that the verbosity of having column headings is something that hinders the experience as opposed to helping it.

Mailchimp, which is not an inbox, has a similar interface to Gmail but uses list items instead of tables:

![Mailchimp List](./images/mailchimp-list.png)

It looks the same, but the advantage of list items over table rows is that they are maleable. Semantics on the web is hard. Only once we take a step back and critique solutions from several angles does the "right" solution show itself.

This may seem a bit out of place in a book about form patterns but forms aren't something that exist in a vaccum. They are a major part of an interface, but they rarely form an interface all on their own.

On balance, an unordered list is preferential. This is not to say tables are bad. We can't classify elements this as *good* and *bad*. We simply need to understand their meaning, their behaviour and their constraints. From here we may choose to classify them as *appropriate* or *inappropriate*.

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

This is the HTML without any form controls. We'll add those momentarily. Even so, the very act of creating this HTML exposes another advantage of lists over tables.

You'll notice the contents of each item is wrapped in a link. The link takes the user to read the email *detail* view. Tables don't allow this behaviour. It's invalid and problematic to have a link span across table cells.

> if you can solve a problem with a simpler solution lower in the stack, you should
> —Derek Featherstone

Gmail uses Javascript to make this happen, which unforunately is an act of exclusivity. We want to avoid this wherever possible. We haven't relied on Javascript so far, and there is no reason for us to do this now.

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

You'll notice that unlike all other fields in the book so far, the checkbox has a label missing. We can add a label, but the contents of the label will duplicate the contents of link. We can't wrap the anchor in the label either as their will be two operations using the same space.

We have three options to discuss, each with their own set of tradeoffs:

- Use ARIA attributes in order to connect the values inside the divs to the label.
- Wrap the contents in a label and associate it with the checkbox.
- Create a separate label and duplicate the contents of the list item.

### Use ARIA attributes

We could use aria-describedby or aria-labelledby to associate the already-present information in the list item with the label. There are two problems with this.

The first is that there is less support for ARIA attributes than there is labels. In fact the first rule of ARIA is not to use ARIA if you can do it natively. Support is directly related to inclusivity, so this is not something to take lightly.

The second is that the hit area is reduced. You'll recall from the first chapter that increasing the hit area helps users with fine motor impairments.

TODO: when did ARIA come along

### Wrap the contents in a label

This is my favourite option because it doesn't rely on ARIA meaning it has excellent support. And the hit area is large, solving both of the problems with the previous option.

However, it has several problems:

In order to implement this solution we need to remove the link. But the link is what allows the user to read and reply to the email. We can't have two interactive elements taking up the same space.

We could instead have a *view email* link at the end of each row, but this forces a change in UI. It could be that an explicit buttons works well, but that would need testing and the hit area of the main action is now reduced.

Remember the links are repetitive, bloat that takes up screen real estate. Hardly insurmountable, but worth our consideration as designers.

We could use *modes*. We could have a separate button on the page that changes the mode in which the user is interacting with the list. For example, the list might start off in view mode, but clicking the "edit mode" button converts the list into a set of checkboxes that can be bulk actioned.

Having dedicated modes may work well but both view and edit modes seem desirable. If edit mode was signicantly less desirable then you could default view mode and allow users to switch and avoid the above problems.

This seems a bit over the top for this situation particularly as it slows users down as they have to switch between the views.

### Create a label and duplicate the contents

The final option is to duplicate the contents and put inside a separate regular email. In addition to duplication we must ensure to hide the label visually as it's the same as the already on-screen information.

The problem with this is duplication, bloat and once again a smaller hit area.

### Which to choose?

It's painstakingly obvious that there is no perfect answer. The trick is to find the balance and test. My inclination is to choose the duplicated label as we know it's better supported. A bit of duplication never hurt anyone.

HTML:

```HTML
<fieldset class="inbox">
	<legend>Inbox: 26 emails</legend>
	<ul>
		<li>
			<input type="checkbox" name="email" id="email1" value="1">
			<label for="email1">From: Heydon Pickering, Subject: Buttons, Sent: 19/09/2017</label>
			<a href="/emails/1/">
				<div class="inbox-recipient">From Heydon Pickering</div>
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

When a user clicks a checkbox it becomes checked. The user knows this because a tick (or check) appears inside the box. We might be tempted to use Javascript to highlight the entire list item. Don't fall into temptation to do more until user testing shows that users are struggling.

In software there is special acronym for this approach. MVP stands for Minimial Viable Product. Simply put, do the minimum, test. If it needs more, then do more. Testing proves that *more* is worth the investment.

Mailchimp, who are well-known for their investment into usability testing don't highlight the rows. The checkbox is enough:

![Mailchimp List](./images/mailchimp-list.png)

We'll follow their lead and avoid the extra effort. More code is more opportunity for failure and more stuff to maintain. As designers, we should practice Dieter Ram's famous ethos *less, but better*.

## Submit buttons

We have a form that enables the selection, but no actions to choose from? We need to add buttons for delete, archive, and marking spam.

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

In chapter one, however, we discussed the associated problems with disabling buttons until a form is valid. Our inbox doesn't look like a traditional form but it effectively suffers from the same problem as any other.

Once again, we'll stick to the foundations we've built from earlier chapters and avoid leaving users guessing how an interface works. We'll ensure our buttons are always enabled.

### Hiding buttons before selection

Whilst disabling is clearly problematic, we might consider hiding the actions until the user selects a checkbox. Mailchimp does exactly this. So does Gmail.

--

There is a case hoever, for always showing the actions, but having them in a disable state. This may give the user a clue that they can manage their list, but they first need to do something to enable the buttons.

I have a bit of an aversion to disabled buttons because I have seen users try and click on disabled elements and not understand why they are disabled. A subtle greyed out state doesn't say very must. UIs should be clear always.

So instead, we could allow users to press the button, and show an error if the user hasn't selected any items yet. This matches all the other forms we have designed in the book.

Once again, consider your options, do the simplest thing first and test.

First time users, or low confidence users may need more than just the enabling or a button. Perhaps a little animation or something might help users notice it.

http://www.enterpriseux.co/gmail-style-data-tables-part-2/

### Menu treatment and small screen design (TODO)

TODO:
- small screens need compact menu (responsive aria attributes with JS)
- aria menu vs select box
- just display them

## Select all (TODO)

With a list of my favourite products, or a list of emails, quite often we might want to select and act upon all options in a list. We may want to, for example archive all the emails. Having a (de)select all button makes sense.

- Without JS? Use buttons to reload the page with all selected.
- With JS piggyback the button/link to not do a post back.
- checkbox indeterminate state.
- button aria-pressed?

--------

- buttons on each row, to quickly delete without having to select.
- (de)select all
- success messages

--------

## Action buttons versus select box

- Sometimes it's not just a button, sometimes it's a selection, like apply filter. But better to go to other page probably. Like for Universal Credit, we have a page that selects people to be allocated to another agent. That means the user selects people, then chooses who to allocate it to then presses the button. Better as a flow, at least to start with.
- other thing is, can have modes. Modes that say "browse mode" or "manage mode". Only show checkboxes when users wants to manage stuff. One thing per page principle type thing.

This little gizmo has much to be discussed.

We could use a select box, one of the rare cases where saving space like this makes sense. But always use a submit button, and don't apply the change when the user changes options.

MISUSING THE SELECT BOX

The problem with using a select box despite the general disadvantages as we've discussed at length in Book A Flight is that they don't allow us to enhance the experience for big screens. That is to display the options when there is enough space to do so.

For this reason we might consider a ARIA TOOLBAR. A div with a button that exposes more buttons on small screens, and on big screens, just expose the buttons without showing/hiding (or aria-expand/collapse).

As we know we don't want to hide options if there is room to show them.

## Putting actions within the table too

If users often act upon one list item at a time, then you might want convenient buttons in context too. It saves users having to select an item first and then acting upon it. One click is better than three or four in this case.

If we count the clicks on small screens then it would be:

1. Check the item
2. Scroll up to the top (depending if the actions are off screen)
3. Open the menu/select box
4. Click action

Or with an in-context button

1. Click button

When modifying data, we should always use a form and submit button. Links are for getting/retrievig documents and information. They are not for modifying. This is an HTTP restful principle that I need to look up to get more detail as to why this is a good thing.

If you have an in context delete button, it shouldn't be a link. Unless of course, you're going to link to another page or whatever, that shows a form saying *are you sure you wish to delete* and that button being a form/submit.

But as you will see shortly, if having to confirm these actions all the time, makes the act full of friction and frsutrating. Instead we might be able to use a better way.

## Success messages

When the user submits the form, the page will refresh, and the item will be gone, if the user deleted (or archived). In this case it would be wise to make this clear to the user with a success message.

Like error messages, success messages are of vital importance. They leave users feeling good, and in control. When we built the checkout flow, we had a gigantic success message at the end in the form of an order confirmation.

In this case, we need a message in context. A toast message is named after toast popping up after a toaster has had its way with a piece of bread. The thing to know about toast messages is that they shouldn't disappear without users dismissing them.

Some users may not notice them at first. Some users may be using a screen reader, and not have read it out yet. Some users may notice it, but take a really long time to read the message.

Whatever it is, don't dismiss the message automatically.

Also, within the success message we may provide users with an undo feature, that let's them undo their last action. This often causes less friction, then asking users to confirm their action "Are you sure you want to delte this item?" etc.

In this case this undo feature will be part of the success message like gmail:

![Undo](./images/undo.png)

## Notes

- Delete and other types of modification should be a post

http://www.enterpriseux.co/how-to-make-gmail-style-user-friendly-tables-part-1/
http://www.enterpriseux.co/gmail-style-data-tables-part-2/
http://www.enterpriseux.co/data-tables-part-3-user-feedback-and-messaging/

---

Unless of course it had a pound sign in front of it, and it was located within a list of products, with each product given it's own heading.

At Tesco, we did just this, but we also laid out parts of each product in columns as you can see below:

![Tesco Product List](./images/tesco-list.png)

At the time, we weren't designing a fully responsive, mobile-first website but if we had it would have been relatively straightforward to convert these divs/list items.

Tables on the other hand aren't responsive: if there isn't enough room, then the user will get a horizontal scroll bar. This is particulary a problem with tables that have lots of (wide) columns.

If you can design something well that doesn't use tables, I would start there, but regardless, whether you use a table or a list item, or a div for that matter, the design principles in this chapter are still applicable.
