# A Filter Form

In the introduction to “A Search Form”, you'll recall the type of conversation I used to have with Mum. Sometimes I would ask “Where's my black top?”. But this was so vague that Mum would respond with questions like “Is it a football or tennis top?”. This question is a filter on a large set of results.

On the web, filters, sometimes referred to as facet navigation or guided navigation, lets users refine a large set of search results. Were filters straightforward, I could have folded this pattern into the last one, but there are a number of challenges and concerns to address: the type of element to use; keeping to conventions; potential AJAX enhancements; how to make it work just as well on small screens as it does on large ones. All of these things need to be taken into account.

First of all, though, it needs to be said that if you don't need a filter, don't include one. They're only useful if searching returns a vast amount of results. In the case of Google, for example, most people aren't willing to click beyond the first or second page.

On the web, a search can yield thousands, or even millions of results depending on the content available. But, as humans, we can't juggle more than approximately seven things at once[^1], so being able to narrow them down is crucial. The ability to filter not only offers an additional dimension of control, but it does so in a way that matches each user's own mental model. 

In “Designing for Faceted Search”[^2], Stephanie Lemieux says:

> Think of a cookbook: authors have to organize the recipes in one way only — by course or by main ingredient — and users have to work with whatever choice of organizing principle that has been made, regardless of how that fits their particular style of searching. An online recipe site using faceted search can allow users to decide how they’d like to navigate to a specific recipe [by course type, cuisine or cooking method, for example].

While filters often look similar on many sites, their behaviours vary widely. When to apply the filtering, what interface components should be used, how to inform users of the updated results: it all needs consideration.

## When To Apply Filters

There are two ways to let users filter: selecting multiple filters at once (batch filters), or one at a time (interactive filters).

Think about how you might order at a restaurant. Say you want to order 2 dishes, but as soon as you say the first one, the waiter walks away to tell the chef to start cooking the dish (interactive). Fortunately, waiters don't do this: they give you time to give the entire order before walking away (batch filtering), even though it might slightly delay the delivery of the first dish.

On the other hand, a waiter might first take your drinks order, to give you more time to decide on the mains. A good waiter knows to adapt to the needs of the customer.

Both batch filters and interactive filters have pros and cons. And which you choose should be based on the data being presented, the user needs and as we'll find out, the size of the screen.

### Interactive Filters

If users don't know exactly what they want, then they'll want to know about the options available to them. These users normally benefit from interactive filters: as soon as they choose a filter, the results are displayed with the new applicable filters. 

For example, once they choose Starters, new filters for temperature such as Cold and Hot will be shown. This helps the user avoid seeing zero-search results.

The disadvantage is that each time a filter is clicked, a request (and page refresh) has to happen to get the new results. Screen reader users will have to tab back to the filter section too. If the site (or connection speed) is slow, and if the user is likely to need many filters, this merry-go-round isn't ideal.

#### ARIA Landmark

I introduced ARIA landmark roles in chapter 6, “A Search Form”. Employing one here, would allow screen reader users to jumo back to the filter quickly, after the page reloads. 

As interactive filters tend to be constructed out of links, a filter component is really just another form of navigation. So it should be wrapped in a `<nav>` element which has an implicit landmark role of “navigation”.

```HTML
<nav>
	<!-- filters here -->
</nav>
```

To differentiate filter navigation from the main *site* navigation, we can give it label of “filter” using `aria-label` like this:

```HTML
<nav aria-label="filter">
	<!-- filters here -->
</nav>
```

### Batch Filters

Users who already know what they're looking for will benefit from batch filtering. For example, say a user wants a black, slimline wallet. Once they arrive at wallets they'll filter based on colour and size.

The advantage of this approach is that it's faster, as users make just one request before seeing the results. And just because users can select multiple facets at once, doesn't mean they have to. Giving users control (principle 4) to select one or multiple facets within the same interface is the inclusive thing to do.

The disadvantage of this approach is that a applying a combination of filters could lead to zero-results.

## Material Dishonesty

We've already discussed material honesty a number of times in the book. That's because dishonest interfaces are prevalent on the web. As a reminder, one material shouldn't be used as a substitute for another because the end result is deceptive.

Interactive filters are implemented with links. Clicking a link makes the request for the new page. Batch filters are implemented as forms made out of checkboxes and radio buttons. Clicking a checkbox marks it as checked. When ready, the user can submit the form as a separate action.

Some sites style links to look like checkboxes, using CSS background images, for example. The problem is that, as noted above, their behaviour is different: they should look like what they do. 

![Ebay links with checkboxes](./images/links-as-checkboxes.png)

By convention, users can select multiple checkboxes before submitting the form. That's just how they work. It would be confusing if clicking — what looks like — a checkbox, suddenly appeared to submit automatically (because it's actually a link). That's materially dishonest and therefore deceptive. 

> ‘Use checkboxes and radio buttons only to change settings, not as action buttons’ — Jakob Nielsen

The styling of native checkboxes (and radio buttons) differs greatly across various operating systems and devices[^gdspost]. Therefore, a pseudo, CSS-styled version is always going to look different to the expected native version users are accustomed to on their browser.

![Native styling differs]()

This also occurs the other way around. By using Javascript, radio buttons can be designed to submit the form automatically on click. This mimicks the behaviour of link, which is particularly problematic for keyboard users, because the act of focusing a radio button by pressing <kbd>Down</kbd>, also selects it.

Breaking widely understood conventions without a very good reason can seriously harm the user experience.

## Using AJAX

Filters are often enhanced with AJAX, typically for performance reasons. Most people assume that AJAX is always faster and better, but this isn't always true as we'll see shortly.

Let's discuss the pros and cons ofusing AJAX in the context of filters now.

### Pros

If users are adding and removing filters a lot, then being constantly interrupted by a page refresh to see the results isn't ideal. As mentioned earlier, screen reader and keyboard users will have to move focus back to the filter component. 

While an ARIA landmark goes someway to mitigate this problem for screen reader users, by using AJAX, we can update the results without interupting users as they continue to play with additional filters. When they're finished, the results will be ready.

The other reason to use AJAX is that materially dishonest interfaces (see above) seem to have shifted users' understanding of how checkboxes work. Some users have actually come to expect that clicking a checkbox, will immediately update the results.

I interviewed David House, a former designer at Gumtree where filters are used extensively. As such, he and his team conducted many usability studies on their search results page. Here's what David had to say:

> “On desktop, Gumtree users expected AJAX. Filters were selected but not submitted. They didn't realise they had to submit their choices. We got a lot of feedback saying “Your filters are broken.” We tried moving the apply button to the top (and the bottom) along with making it sticky; loads of things that didn't really make a difference.”

Whether this is solely down to designing dishonest interfaces in the first place is hard to tell. David thinks that if there weren't so many adverts on Gumtree, they'd probably have better luck drawing attention to the submit button.

But you should be aware of this potential issue and conduct your own research. If need be, you might need to break convention to meet users' new expectations.

### Cons

First, making an AJAX request when the checkbox is clicked, inadvertently converts the batch filter into an interactive filter. That is, selecting 4 filters, for example, will make 4 AJAX requests which will eat into users' data allowances and cause battery drain on device.

Second, if the user is selecting a filter down the page, but there is only a few results, once they load, the user won't see them appear.

![Illustrate the above](.)

*(Note: don't move focus, because this would be confusing for keyboard and screen reader users and it would fail WCAG's “On Input” success criterion[^].)*

Third, without intervention, using AJAX for major page updates breaks the back button. This is because AJAX doesn't actualy navigate in a way that causes the browser to track history. Baymard's usability study[^] found that sites that broke convention like this caused confusion, disappointment, anger and even abandonment.

*(Note: if you're going to use AJAX for filtering, then you should use HTML5's push state APIs[^] which will create a browser history entry.)*

Fourth, you need to gives users a way to know that the results are being loaded or have finished loading. Loading spinners, are a poor replacement for the familiar and accurate loading indicators that browsers provide for free.

*(Note: we'll look at how to implement an accurate and inclusive AJAX loading indicator in the next chapter.)*

Finally, despite what you may have heard, AJAX isn't necessarily faster. If the site (or connection) is slow AJAX is still subjected to much of the same latency. More crucially it can be even slower than a standard page refresh, mostly because it engineers away progressive rendering (also known as chunking) — something the browser provides for free. 

In “Fun Hacks For Faster Content”[^], Jake Archibald explains the concept of progressive rendering:

> When you load a page, the browser takes a network stream and pipes it to the HTML parser, and the HTML parser is piped to the document. This means the page can render progressively as it's downloading. The page may be 100k, but it can render useful content after only 20k is received.

As AJAX has to wait for the “entire 100k” before showing anything, users have to wait a lot longer to see something.

This doesn't mean AJAX is “bad” as such. It just means that AJAX is generally more suited to making smaller changes. 

In the end, we can only be sure of what's best by conducting user research with a diverse group fo people, using a broad range of browsers and devices on varying connection speeds in the context of our own problems.

## Denoting Selected Filters

Having selected one or more filters, they need to be demarcated on the interface. Baymard's usability study shows that there are two approaches that should be used in combination[^combo]:

### Shown In Original Position

The first approach is to show the selected filters in their original position alongisde the other, unselected filters. This is because some users will remember where the filter was originally when they selected it.

![Illustrate](.)

Having it disappear (or moved somewhere) is frustrating. It also gives users context as to what's selected within the category that the user is currently perusing. That is, it might impact their next action.

### Shown In A Separate List At The Top

The second approach is to group all the selected filters in a list at the top.

![Illustrate](.)

This is useful because it gives users confirmation of their selection providing context to the results that are currently being viewed. But it also easy access to remove any applied filters, without having to scroll down the page to find the selected filter amongsts a long list.

## Filter Overload

Depending on the type of data you're dealing with, you could have many different filter categories and many options within those categories. Overloading users with choices can make their job harder if we're not careful. This is otherwise known as the paradox of choice, something we discussed in chapter 4, “A Login Form”.

There are two techniques we can employ to reduce filter overload:

First, we should order the filters based on the most useful and most used category. You can use analytics to check which ones are most used. And of course, if some filters aren't being used, remove them from the interface.

Second, we can collapse the least used categories. And we can also collapse options within a category.

![Illustrate collapsing](.)

## You Might Need An Adaptive Approach (or THERES NO ROOM ON MOBILE)

READ: NN BOOKMARK ABOUT MOBILE

In chapter 5, “An Inbox”, we discussed the differences between responsive design and adaptive design. In short, a responsive approach is preferred because not only is it less work, but it's more robust and performant for users.

Mobile first, to me at least, just means small screen first. Which really means essential first, which to me just really really means essential only. The essential components here are the results and the filter.

Normally speaking, if you cut out the superfluous content and lay out what remains in a small viewport, the experience works well. Of course, this scales up nicely to large viewports too: an increase in font-size and whitespace is usually enough.

But because the filter component is important, it needs to be almost as prominent as the results. On large viewports this is easy: you just lay them out side by side. But on small viewports, you'd have to place the results underneath the filter which pushes the main content down.

If the filter is placed after the results, then users would have to scroll past the results just to discover it. Many users would miss it.

We could collapse the filters and put it above the results. This way the filters are discoverable without pushing the main content too far down. While this is a responsive approach, it's not the end of the story.

I interviewed David House, a former designer at Gumtree where filters are a major part of the experience. As such, he and his team conducted extensive user research around their search results page. Here's what David had to say:

> On desktop, Gumtree users expected AJAX. Filters were selected but not submitted. They didn't realise they had to submit their choices. We got a lot of feedback saying “Your filters are broken.” We tried moving the apply button to the top (and the bottom) along with making it sticky; loads of things that didn't really make a difference.

But what about mobile?

> On mobile, AJAX wasn't desirable because users couldn't see any visible refresh. We didn't want to move focus to the results, because users wanted to pick more than one filter. Not to mention that using AJAX is a waste of bandwidth.

That's two opposing problems which are solved with completely different approaches.

> We reluctantly had to use an adaptive approach. On mobile, clicking the filter menu, would send users down a guided flow without AJAX. On Desktop (when there's enough space to show the filters next to the results), users got an AJAX experience where filters were immediately applied.

I asked Dave if he would use the same approach in future.

> Not necessarily. I wouldn't start with this approach. If a site doesn't have a lot of ads [like Gumtree does], we'd probably have better luck drawing attention to the submit button.

TODO: That is the main reason for which, on mobile devices, we recommend batch filtering — page loads are often slow on the go, and having to wait for four page loads for a complex query involving four filter values increases interaction cost too much for the user.

## Summary

This chapter looked at various interaction design details pertaining to responsive search results page. We looked at how design can shape users — so much so — that occasionally we may have to abandon convention and best practice. To that end, we looked at how adaptive design may be better for users.

### Things To Avoid

- Making links look like radio buttons and checkboxes
- Making checkboxes and radio buttons behave like links
- Assuming AJAX always creates a faster and better experience
- Prioritising best practice above user research

## Demos

TBD

## Footnotes

[^1]: https://en.wikipedia.org/wiki/The_Magical_Number_Seven,_Plus_or_Minus_Two
[^2]: https://articles.uie.com/faceted_search/
[^3]: https://jakearchibald.com/2016/fun-hacks-faster-content/
[^backbutton]: https://baymard.com/blog/macys-filtering-experience
[^combo]: https://baymard.com/blog/how-to-design-applied-filters