# An Expense Form

At first I wanted to fold this chapter into the last one because there's some overlap. Adding multiple files needn't be different from adding multiple of anything else really. It's just that uploading files has some unique considerations (such as drag and drop) that made sense to tackle in isolation.

Unlike file inputs, other form controls don't let you add multiple values. So it's up to us to design a solution from scratch. There are three main approaches that are applicable to all sorts of data&mdash;files included. Whether it's adding collaborators to your Github repo or adding monthly expenses into an accounting service, you'll find the patterns in this chapter useful.

Of course, if you know how many things that need to be added in advance, then simply display that amount of fields (and make them required). If you don't, keep reading.

## The Ever Present Form Pattern

The drag and drop interface from “An Upload Form” in some respects uses this pattern and so does the infamous “Todo list pattern”[^]. Even Github uses this pattern to let users add collaborators. What I'm trying to say is that you've most probably seen it in action many times.

![Github](.)

The way it works is to have an ever-present form on the page (hence the name). Submitting it adds an item to a list above (or perhaps beside) the form. When the user is finished they can proceed. If this is part of a flow, you'd click a continue button. On Github, you just leave the page.

This is suitable for simple forms that can be submitted in one step. The potential downside is that as the list grows, the form is pushed down, which could cause trouble, especially for mobile users.

Also, there are multiple buttons: one to submit and one to proceed which could cause mild confusion. As mentioned in previous chapters, providing one button is preferable because it requires less cognitive effort.

Also, each submission is a server request. While not a huge deal, it coyld become frustrating if that form is used more frequently. Bare this in-mind.

## One Thing Per Page Again

Imagine you need to add a bunch of expenses, but the type of expense determines the information you need to enter. For example, if you're expensing a car, then you have to enter mileage, but if you're expensing a train ticket then you have to enter a monetary amount.

![Show the flow with branching diagram](.)

In this case, the Ever Present Form pattern is less suitable. Using One Thing Per Page as first discussed in “A Checkout Flow” is appropriate because it guides the user to provide the right information at the right time.

But what if the user wants to add another one? Simply add a question to the end of that flow asking the user if they'd like to add another. Selecting no completes the task. Selecting *yes* goes through the same flow again. Of course, not selecting an answer will prompt the user with an error message.

![Do you want to add another, yes no with button](.)

Obviously this pattern is particularly adept at handling branching. But it's also suitable for infrequent tasks and low confidence users.

## Add another

This pattern consists of one form, on one page, submitted in one step. It works by using Javascript to create extra items instantaneously. This is particularly useful for high confidence users who need to perform the task frequently.

### How it might look

![Add another](.)

Clicking the add another button creates new expense fields. Pressing the remove button deletes it. Users can keep adding expenses easily. Once they're finished, they can submit all of the expenses in one go which expedites the process.

The degraded experience is the same, except clicking a button refreshes the page.

### Managing focus

Throughout the book, we've conscientiously thought about how things work for keyboard users. When the add button is clicked, focus should be set to the first newly-created form field. This is useful for screen reader users too, as the announcement of that field naturally prompts the user to continue.

When the user clicks the remove button, the fields in the row, including the button itself are removed. What happens to the focus when you delete the currently focused element? In ‘A Todo List’[^], Heydon Pickering explains exactly what happens:

> [...] browsers don’t know where to place focus when it has been destroyed in this way. Some maintain a sort of “ghost” focus where the item used to exist, while others jump to focus the next focusable element. Some flip out completely and default to focusing the outer document — meaning keyboard users have to crawl through the DOM back to where the removed element was.

We could set focus to the previous or next expense item but this is confusing. Alternatively, we could set focus to the add another button, but that's both presumptuous and a little odd.

Instead, we'll set focus to the heading at the beginning of the form.

### Feedback

For sighted users there is nothing we need to do. The act of adding and removing items is obvious. But to provide a *comparable experience* we can include a live region, as set out in “Book A Flight”, so that screen readers announce the feedback. [Check heydon article for more on this: two feedback panels not good]

### Cloning explained

Every time the user adds or removes an item, we need to do some important work behind the scenes. The cloning works by taking the first item in the list, and cloning it. But this isn't enough on its own because this clones everything: ids and name attributes included.

We first discussed the importance of labels in “A Registration Form”. If we don't update the label's `for` attribute and the matching input `id` attribute, then the interface will break. That is, clicking a label may focus another field entirely.

Perhaps even more importantly, is that if you don't update the name of the input, then the server won't necessarily be able to recognise it and therefore process the submitted data. The contract between the browser and the server is forged by the name of the form controls.

```HTML
<form class="addAnother">
  <div class="addAnother-items">
    <div class="addAnother-item">
  	  <div class="field">
        <label for="items[0][name]">
		      <span class="field-label">Description</span>
	      </label>
        <input class="field-textBox" type="text" id="items[0][description]" name="items[0][description]" value="" data-name="items[%index%][description]" data-id="items[%index%][description]">
      </div>
      <div class="field">
        <label for="items[0][amount]">
          <span class="field-label">Amount</span>
        </label>
        <input class="field-textBox" type="text" id="items[0][amount]" name="items[0][amount]" value="" data-name="items[%index%][amount]" data-id="items[%index%][amount]">
      </div>
      <input type="submit" name="remove" value="Remove expense">
    </div>
    <input type="submit" name="addAnother" value="Add another expense">
  </div>
</form>
```

The `id`, `for` and `name` attributes have a particular format. This is because we're sending the server multiple expense items for processing. Many server side frameworks, such as Ruby on Rails or Express, look for names like this in the request payload and convert them into an array of items.

The crucial part is that the index needs to increase from 0 to 1 and so forth. To make this easy to parse in Javascript, the pattern is stored in data attributes. This way, all the script has to do is replace `%index%` with the index number.

```JS
Put that code here
```

The reason there are attributes for `id` and `name` is that in the case of radio buttons and checkboxes, the `name` differs from the `id`.

```HTML
Example
```

## Summary

In this chapter, we've looked at the different ways to let users add multiple expenses or any other type of information they need to manage. The solutions offered have been based on frequency of use, user ability and the constraints of the collected data (branching).

There's no right and wrong way here, it's about picking the most appropriate technique for your particular problem.

## Footnotes

[^]:
[^]:
[^]:

‘UNIQUE, DESCRIPTIVE LINK TEXT’ heydon