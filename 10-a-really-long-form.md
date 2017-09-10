# A Really Long Form

Different types of tasks take different amounts of time to complete. One Thing Per Page helps users complete small tasks in one sitting, but what about those that take several hours to complete over the course of days, weeks or even months?

In Mailchimp, for example, I'll typically start drafting an email campaign weeks before I send it. And there are a number of discrete steps I'll perform. First, I check the content reads well. Then I need to make sure it looks good in various email clients such as Gmail. Then some time later, I'll run some final checks, decide the subject line and schedule it for release.

Other tasks are performed as a team by different people using the same system. For example, processing a return involves someone at the warehouse receiving the item. Then a decision maker will take a look at the goods to make sure they satisfy the returns policy.

Some Government services take weeks to apply for and users need to provide lots of information about their identity, home, family and financial situation and sometimes they need to gather evidence and send it by post.

Whatever the case, if you're designing a long and unweildly form there's some patterns that we can employ to improve the experience. Of course, if you can simplify the process so that it's short in the first place then you can skip this chapter entirely.

## The Task List Pattern

In “The Psychology of Checklists”[^], Lauren Marchese explains the importance of breaking down big tasks into smaller ones and it's bioligically proven to motivate us.

It's to do with dopamine. In short, when we experience even small amounts of success, our brains release dopamine giving us feelings of pleasure, learning and motivation. I like to call this *momentum*.

Most of us work in teams employing Agile methodologies. One of the main aspects of this is breaking down a large product into epics, stories and tasks. Complete enough tasks, and the story is done. Complete enough stories, and the epic is done.

What's really happening is that tasks seem far easier to achieve when they're broken down. Crucially, if tasks are small enough, then we'll get that hit of dopamine frequently.

This is the basis of the task list pattern, the name of which I've stolen from The Goverment Digital Services[^]. Here's how it looks:

![Task list Pattern](.)

This particular example involves three top level tasks, each containing sub tasks. Clicking the task link takes the user down a flow. Whether that's one screen or several doesn't matter. Once it's complete, they come back to this view with the task marked as complete.

Similarly, Mailchimp, has its own set of tasks but they have no need for sub tasks creating a flatter hierarchy. Both are visually clear.

![Mailchimp](./images/mailchimp_task_list.png)

Instead of copy, Mailchimp uses iconography to mark tasks are complete or otherwise. And instead of standard links, they use call to action buttons. What's nice about Mailchimp is that the status and button is nicely combined because the button is labelled ‘resolve’.

The exact design details will come down to your own design language and user research. But you can read about how GDS iterated their pattern on the design blog[^].

Both use the same pattern as such, and in turn end up solving the same problem for users in a useful way:

1. Each task's status is clearly marked, making it easy to scan what's left at a glance.
2. Users can scan the list to get a feel for how long is left
3. Previous information is saved so that users can return later, even if they log out.

## Additional considerations

1. Explain what users need before completing the overall task or each sub task. Documents, payment types, information.
2. Indicate duration where possible. Estimate will do with a range if necessary. If you can't estimate, that's a sign the task may need to be broken down further.
3. Use verbs for task names. GDS better than Mailchimp here.
4. List the tasks in a sensible order. If order matters use an ordered list (see “An Inbox”).
5. If the tasks need to be completed by separate people, then mark the person or ‘title’ or role against the item.
6. When all tasks are completed, give users a final submit button. Don't let the user not see the completed list. The best part of completing a to-do list is seeing it's all been done. Also doubles up as a check and amend page.

## Summary

### Things to avoid

- Really long forms
- Not saving state

## Footnotes

[^]: https://designnotes.blog.gov.uk/2017/04/04/weve-published-the-task-list-pattern/
[^]: Mailchimp
[^]:

## Todo

- If tasks can't be done out of order