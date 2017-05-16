# Inbox Management

My sister loves lists. Her favourite list is a todo list. In fact she loves lists so much, that one of her favourite things is making new lists out of old ones.

Despite her obsession, the world is full of lists. There is even a list of great people[^]. But lists are a tricky thing to manage. On the web, there are some conventions that have emerged over the years.

In this chapter, we're going to make sure list management is easy, accessible and scalable. My sister loves pen and paper, but I hope that she may one day be converted to a digitally managed list.

On the web there are many types of list: tabular data (`<table>`), description lists (which used to be called definition lists) (`<dl>`), unordered lists (`<ul>`) and ordered lists (`ol`).

We can't discuss the merit of each until orientate ourselves around a  specific problem that needs solving. We'll design an inbox. The aim of course is to achieve a zen-like state of Inbox Zero.

To get to Inbox Zero our UI must enable users to delete, archive and mark  spam. But not individually. In bulk.

Whilst this chapter is about email, the principles and design patterns are applicable to most, if not all types of lists.

## Everything is list

Semantically speaking everything is a list. The things on the page are a list of things on the page. Pedantism aside, we need to decide what type of list our inbox will use.

We should use tables to represent two-dimensional data. In our case the rows represent emails and the columns represent the details about the email: recipient, subject and sent date etc. Gmail, omits table headings which might suggest a table is the wrong choice.

We could represent rows as list items and&mdash;at least in big screens&mdash;style them visually as columns. This brings us to the first problem. Tables aren't very responsive.

Tables are semantically tied to the way they look. Meaning it's hard to make tables not *look* like tables. There are some solutions, but they are not particularly cross-browser friendly.

Moreover, tables are a good choice when the data needs contextual information to make it useful. For example, *23* is useless information without the context of *goals scored* as a column heading and *Lionel Messi* as a row heading.

Our inbox is seemingly less tabular. It's a list of emails that if read out as "From Heydon, subject: Buttons, 19/09/2017 at 9am" would be quite readable.

In fact, Mailchimp has a similar looking interface to Gmail but uses list items instead of tables:

![Mailchimp List](./images/mailchimp-list.png)

This shows that semantics on the web is hard. Only once we take a step back and try and critique solutions from many articles does the "right" solution rear its head.

This may seem a bit out of place in a book about form patterns but forms aren't something that exist in a vaccum. It goes to show that we must consider their surrounds just as much as we should consider the elements themselves. If you're interested in wider problems to accessibility you should read Heydon Pickering's *Inclusive Design Patterns*.

At this point, it seems prudent to choose an unordered list to represent our inbox. This certainly makes it easier to design for small screens. Designing for those with small screens is an act of inclusive design.

Here's what our inbox looks like so far:

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

Writing the HTML above shows us another problems that tables have that list items don't. Each email is surrounded by a link, which allows the user to click the row to read the email.

Tables don't allow links to span across multiple cells. Gmail relies on Javascript to do this, which is an act of exclusivity. One that we'll avoid whereever possible. Up to now we've had a 100% record, that I'm keen not to break.

In anycase, list items are working for us. Let's continue.

## Enabling selection

To allow users to select and action multiple emails at once, we'll need to add a checkbox to each item in the list.

Here's what that looks like:

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

The first thing to notice is that the checkbox doesn't have a label. This is one of those annoying situations where every solution on offer has a tradeoff. The trick is find a balance. Let's see if we can find one.

1. use aria described by
2. create a label and duplicate and hide
3. get rid of the link, make the majority/all of the row a label text

1: We can use aria-describedby or labelledby, but the first rule of aria is don't use it. That's the beauty of the label element, it works everywhere. ARIA has less support. ARIA only arrived on the scene in 2010? The good thing about this is that there is no HTML repetition, keeping our mark-up lean and therefore as performant as possible.

2: We create a label, duplicate some/all of the information and visually hide it. Not only is this repetive and slower but there is a smaller target area to click.

3: now the entire row is clickable to mark as selected but the list is multi functional. I can click the row to view the email. Now I would need dedicated "view" links on each row. This is a trade off. First there is less to click for the majority action, second it's repetitive. View this, view that. We might consider modes of operation. Manage mode, and view mode. But modes have their own trade offs in that I have to activate them. It's nice keeping UIs minimal whereever possible. Also, having two modes is slower and time consuming. This for me is not a bad way to go at all, but in this scenario it's my least favourite. If managing emails happened 1% of the time for users then it might be a valid option. But it's more 5050.

--------

- enabling selection
- adding a checkbox and highlighting the item
- multiple submit buttons
- legend?
- buttons on each row, to quickly delete without having to select.
- showing actions (responsively too, select versus menu), misusing select
- location of actions
- (de)select all
- success messages

--------

## Higlighting the item

When a checkbox is clicked, it becomes checked. The user knows this because a little tick appears inside the checkbox. There is no need to write anymore code to highlight the row. Mailchimp does this:

![Mailchimp List](./images/mailchimp-list.png)

If user testing shows, that users don't get it and it's not obvious enough, you may decide to write some Javascript that highlights the entire row. But as with every enhancement in this book, only do that once testing shows it's worth added design effort, and code being downloaded on users machines.

## Showing actions

Another question is do we disable or hide the actions until they can be clicked? Maybe we don't even do that.

Mailchimp hides the actions until the user clicks at least one checkbox. Gmail does the exact same thing.

There is a case hoever, for always showing the actions, but having them in a disable state. This may give the user a clue that they can manage their list, but they first need to do something to enable the buttons.

I have a bit of an aversion to disabled buttons because I have seen users try and click on disabled elements and not understand why they are disabled. A subtle greyed out state doesn't say very must. UIs should be clear always.

So instead, we could allow users to press the button, and show an error if the user hasn't selected any items yet. This matches all the other forms we have designed in the book, and it just feels right.

Once again, consider your options, do the simplest thing first and test.

First time users, or low confidence users may need more than just the enabling or a button. Perhaps a little animation or something might help users notice it.

http://www.enterpriseux.co/gmail-style-data-tables-part-2/

## Where to display action buttons

Conventionally speaking we should display action buttons at the top of a list. This is becauses lists can be long. At least by having actions at the top, the user has a chance to see what is available before making a selection.

You can put the action buttons at the bottom too for convenience. Or you could consider using `position: sticky`. It's not usually something I advise by default, because sticky elements get in the way. And is quite often to the way with lists, we're not always acting upon them in this way. Quite often we're just browsing and clicking in to detail.

## Select all?

With a list of my favourite products, or a list of emails, quite often we might want to select and act upon all options in a list. We may want to, for example archive all the emails. Having a (de)select all button makes sense.

- Without JS? Use buttons to reload the page with all selected.
- With JS piggyback the button/link to not do a post back.
- checkbox indeterminate state.

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

- modes: view and manage modes
- multi select box, checkboxes.
- Delete should be a post
- aria labelledby - first rule is not to use aria, but here it makes sense.

http://www.enterpriseux.co/how-to-make-gmail-style-user-friendly-tables-part-1/
http://www.enterpriseux.co/gmail-style-data-tables-part-2/
http://www.enterpriseux.co/data-tables-part-3-user-feedback-and-messaging/

---

Tables are useful when column headings are needed to add context to the values in the cells. For example, the number 23 without a heading tells the user not very much.

Unless of course it had a pound sign in front of it, and it was located within a list of products, with each product given it's own heading.

At Tesco, we did just this, but we also laid out parts of each product in columns as you can see below:

![Tesco Product List](./images/tesco-list.png)

At the time, we weren't designing a fully responsive, mobile-first website but if we had it would have been relatively straightforward to convert these divs/list items.

Tables on the other hand aren't responsive: if there isn't enough room, then the user will get a horizontal scroll bar. This is particulary a problem with tables that have lots of (wide) columns.

If you can design something well that doesn't use tables, I would start there, but regardless, whether you use a table or a list item, or a div for that matter, the design principles in this chapter are still applicable.
