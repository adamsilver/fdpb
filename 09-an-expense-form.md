# An Expense Form

As a freelancer I have to submit expenses for my tax return. It’s a pain but if I do it correctly I get tax breaks. The problem is that I have so many expenses to enter and a limited amount of time I'm willing to give up to enter them.

Depending on the system you use, the anatomy of an expense might include a description, company, reference number, currency, amount, proof (receipt, for example) and date. How can we design a pattern that makes entering expenses easy, fast, performant and inclusive?

- Previous chapter looked at ways of adding lots of images.
- This is applicable to more than just expenses
- Of course, if you know how many things that need to be added in advance, then simply display that amount of fields (and make them required). If you don't, keep reading.

## The Ever Present Form Pattern (Persistent Form Pattern)

The drag and drop interface from “An Upload Form” in some respects uses this pattern and so does the infamous “Todo list pattern”[^]. Even Github uses this pattern to let users add collaborators. What I'm trying to say is that you've most probably seen this pattern before even if you didn't have a name for it.

![Github](./images/09/github.png)

The way it works is to have an ever-present form on the page (hence the name). Submitting it adds an item to a list above (or perhaps beside) the form. When the user is finished they can proceed. If this is part of a flow, you'd click a continue button (like the one in “An Upload Form”. On Github, you just leave the page.

This is suitable for simple forms that can be submitted in one step. The potential downside is that as the list grows, the form is pushed down, which could cause trouble, especially on mobile.

Also, there are multiple buttons: one to submit and one to proceed which could be confusing, especially for those with cognitive impairements. One primary action and button is always preferrable anyway as it requires less thinking.

Also, each submission is a server request. While not a huge deal, it could get frustrating if used frequently.

## One Thing Per Page (Again)

Imagine you need to add a bunch of expenses, but the type of expense determines the information you need to enter. For example, if you're expensing a car, then you have to enter mileage, but if you're expensing a train ticket then you have to enter the ticket price.

![Flow](./images/09/expense-branching-flow.png)

In this case, the Ever Present Form pattern is less suitable. Using One Thing Per Page as first discussed in “A Checkout Flow” is more appropriate. This is because it guides the user to provide the right information at the right time, which is especially useful for cognitively impaired users.

If users need to add multiple expenses at a time, then we'd need to ask them if they'd like to add another one at the end of the flow. Selecting *Yes* would take the user down the same flow again. Selecting *No* would complete the task. Not selecting either option will cause a validation error message to show.

![Add another question](./images/09/add-another-radios.png)

This pattern is particularly appropriate for:

- handling branching
- infrequent tasks
- low-confidence users

## The Add Another Pattern

If your interface needs to be used more frequently and doesn't need to branch, you can consider the Add Another pattern. This consists of a single form, on a single page, submitted in a single step.

It works by using Javascript to create extra form fields instantaneously which expedites the process.

### How It Might Look

![Add another](./images/09/add-another-pattern.png)

Clicking the add another button creates a new set of expense fields. Pressing the remove button deletes the field. Users keep adding expenses and when they're finished, they can submit all of the expenses in one go.

The degraded experience is the same, except clicking a button refreshes the page.

### Managing Focus

When the add button is clicked, focus should be set to the first newly-created form field. This is useful for screen reader users too, as the announcement of that field naturally prompts the user to continue.

When the user clicks the remove button, the fields in the row, including the button itself are removed. What happens to the focus when you delete the currently focused element? In “A Todo List”[^2], Heydon Pickering explains exactly what happens:

> [...] browsers don’t know where to place focus when it has been destroyed in this way. Some maintain a sort of “ghost” focus where the item used to exist, while others jump to focus the next focusable element. Some flip out completely and default to focusing the outer document — meaning keyboard users have to crawl through the DOM back to where the removed element was.

So we don't want to leave focus down to the browser but where do we move it to? 

We could set focus to the previous or next expense item but this is confusing. Alternatively, we could set focus to the add another button, but that's both presumptuous and a little odd.

Instead, we should set focus to the heading at the beginning of the form.

### Feedback

For most users, feedback is given implicitly by the new fields appearing in the form. There's no need to show an additional notification at the top of the form, for example. Having two parts of the interface update at the same time would be overbearing. And, as more expenses are added, the notification would be outside the viewport, meaning uses wouldn't see it anyway.

For screen reader users, the act of focusing the new field will announce its label (or legend).

The only remaining consideration would be animation. Perhaps by fading-in the new fields, the chance of the fields being missed is reduced. However, motion is often unnecessary, clunky and harmful to users, especially those suffering from cognitive impairements such as ADHD and Autism.

> ADHD: If there's a “subtle” animation always running, I cannot focus. — @tigt_

> I'm also autistic and can get frustrated with, or repelled by, glitzy mouseover effects/animations - @elementnumber46

### Cloning Explained

Every time the user adds or removes an item, we need to do some important work behind the scenes. Cloning works by taking the first item in the list, and cloning it. But this isn't enough on its own because this clones everything: `id` and `name` attributes included.

If we don't update the label's `for` attribute and the matching input's `id` attribute, when the user clicks the label, focus will be moved to the first field.

![Focus issue](./images/09/add-another-pattern-focus.png)

Crucially, if you don't update the `name` attribute, the server won't be able to recognise and process the submitted data. Remember: the contract between the browser and the server is forged by the `name` of the form controls.

```HTML
<form class="addAnother">
  <div class="addAnother-items">
    <div class="addAnother-item">
  	  <div class="field">
        <label for="items[0][description]">
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

The `id`, `for` and `name` attributes have a particular format. This is because we're sending the server multiple expense items for processing. Many server-side frameworks, such as Express, look for names with array indexes in the request payload. They use this convention to convert the form fields and values into groups.

The important bit is that the index needs to increase by 1 each time. To make this easy to parse in Javascript, the pattern is stored in data attributes. This way, all the script has to do is replace `%index%` with the index number.

```JS
el.name = $(el).attr('data-name').replace(/%index%/, index);
```

The reason there are attributes for `id` and `name` is that in the case of radio buttons and checkboxes, the `name` differs from the `id`. As laid out in, chapter 2, “A Checkout Flow”, the name of each radio button is the same, but the id need to be unique of course.

While this pattern is more complex, it may speed up the task for users who are more confident and familiar with a particular system which is used frequently.

## Summary

In this chapter, we've looked at the different ways to let users add multiple expenses or any other type of information that needs to be included. The patterns are based on frequency of use, digital ability and whether there is a need to branch.

There's no right and wrong way here, it's about picking the most appropriate pattern for your particular problem.

## Demos

TBD

## Footnotes

[^1]: http://todomvc.com/
[^2]: https://inclusive-components.design/a-todo-list/