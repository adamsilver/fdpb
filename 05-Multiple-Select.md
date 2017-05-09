# Multiple Select

Most online services, products and shops contain lists of things. Whether it's it's an inbox of emails or searching for a list of products to your weekly grocery shop, we all need to manage multiple things at the same time.

Managing lists have several interesting design challenges for our consideration. The first being how to present the list of things to edit.

## Lists Versus Tables

Typically we can represent the same data in more ways than one. We can put data in tables, graphs, lists and we can even describe the data in a paragraph.

Semantically speaking everything is a list. The things on the page are a list of things on the page. Perhaps the first element inside the body tag should be a `ul`? No, don't do that.

Tables are useful when data 

---

Tables are useful when column headings are needed to add context to the values in the cells. For example, the number 23 without a heading tells the user not very much.

Unless of course it had a pound sign in front of it, and it was located within a list of products, with each product given it's own heading.

At Tesco, we did just this, but we also laid out parts of each product in columns as you can see below:

![Tesco Product List](./images/tesco-list.png)

At the time, we weren't designing a fully responsive, mobile-first website but if we had it would have been relatively straightforward to convert these divs/list items.

Tables on the other hand aren't responsive: if there isn't enough room, then the user will get a horizontal scroll bar. This is particulary a problem with tables that have lots of (wide) columns.

If you can design something well that doesn't use tables, I would start there, but regardless, whether you use a table or a list item, or a div for that matter, the design principles in this chapter are still applicable.

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

- Sometimes it's not just a button, sometimes it's a selection, like apply filter. But better to go to other page
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

- Delete should be a post
- aria labelledby - first rule is not to use aria, but here it makes sense.

http://www.enterpriseux.co/how-to-make-gmail-style-user-friendly-tables-part-1/
http://www.enterpriseux.co/gmail-style-data-tables-part-2/
http://www.enterpriseux.co/data-tables-part-3-user-feedback-and-messaging/

