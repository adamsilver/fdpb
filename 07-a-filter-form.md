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

I introduced ARIA landmark roles in chapter 6, “A Serch Form”. We could employ a landmark here to let users jump back to the filter after a page reload. The `<nav>` element has an implicit role of “navigation”. This means, it will be in a list of landmark shortcuts available to most screen readers.

```HTML
<nav>
	<h2>Filter</h2>
	...
</nav>
```

Most sites will have *site* navigation that uses the `<nav>` element which means users would find it hard to differentiate between it and that *filter* navigation. To solve this problem we can label the filter navigation by using the `aria-labelledby` attribute. This way, screen readers will announce “navigation, filter” (or similar).

```HTML
<nav aria-labelledby="filter-heading">
	<h2 id="filter-heading">Filter</h2>
	...
</nav>
```

### Batch Filters

Users who already know what they're looking for will benefit from batch filtering. For example, say a user wants a black, slimline wallet. Once they arrive at wallets they'll filter based on colour and size.

The advantage of this approach is that it's faster, as users make just one request before seeing the results. The disadvantage is that a applying a combination of filters could lead to zero-results.

## Material Dishonesty

Interactive filters are implemented with links. Clicking a link, makes the query immediately. Using a form with checkboxes would be for batch processing, where users would expect to select many filters before submitting them together when ready.

Sometimes, these different interfaces are mixed up between the different types of filter, in such a way that hurts the user experience. Mostly because using the wrong component for the job creates a dishonest interface.

We already discussed material honesty earlier in the book. That's because dishonest interfaces are prevalent on the web. In short, one material shouldn't be used as a substitute for another because in that case the end result is deceptive.

In the case of filters, links are often styled to look like checkboxes — using CSS background images, for example. The problem is that a checkbox is not a link. When a link is clicked, it takes the user to that page. When a checkbox is clicked, users would expect it to become checked but not to be submitted until submission.

The styling of native checkboxes and radio buttons differs greatly across various operating systems and devices[^gdspost]. Therefore, a pseudo, CSS-styled version is always going to look different to the expected native version users are accustomed to on their browser.

> ‘Use checkboxes and radio buttons only to change settings, not as action buttons’ — Jakob Nielsen

By convention, users can select multiple checkboxes before submitting their choices. That's just how they work. It would be confusing if clicking — what looks like — a checkbox, suddenly appeared to submit automatically (because it's actually a link). That's materially dishonest and therefore deceptive. 

![Ebay links with checkboxes](./images/links-as-checkboxes.png)

Similarly, with Javascript, selecting a radio button can be made to submit the form automatically (mimicking the behaviour of a link). This is particularly problematic for keyboard users, because they'll struggle to move through the options by pressing <kbd>Down</kbd>. This is because focusing a radio button also selects it.

If it wasn't already obvious, breaking widely understood conventions can seriously harm the user experience.

## Using AJAX

Filters are often enhanced with AJAX, typically for performance reasons. Most people assume that AJAX is faster but this isn't necessarily the case as we'll discuss shortly.

There are advantages to using AJAX in some cases, but there are also several disadvantages. Let's discuss them next.

### To Keep Focus Still

As mentioned earlier, if users are filtering a lot, having the page refresh and move focus to the top of page is frustrating. A landmark goes someway to mitigate this problem as screen reader users can jump straight back to the filter. But with AJAX, we can keep focus exactly where it is (on the link), while the results are updated on the client.

### Some People Don't Know They Need To Submit

Earlier, we discuss that many sites have created materially dishonest filters. That is, that clicking a checkbox, immediately makes a query. The problem is that their ubiquity has shifted users' understanding of checkboxes, at least in the context of filters.

[Put Dave House bit here?]

In short, some users now *expect* that clicking a checkbox, will make a query, crucially without submitting the form. What this means, is that the form doesn't submit automatically, these users will think the filter is broken.

AJAX can solve this problem because it can be used to make a new query as soon as the checkbox is clicked. And because focus isn't moved (see previous point), users can continue to select other checkboxes without losing their context.

The problem is that this inadvertently converts the batch filter into an interactive filter. In other words, we may as well have provided an interactive filter (made out of links) in the first place, negating the need for AJAX.

Moreover, the main advantage of batch filtering is that users can choose several filters in just one request. Once the bahviour is changed to use AJAX, selecting 4 filters will create 4 AJAX requests. This eats users' data allowance and cause battery drain on device.

### Additional Considerations

- The change in peripheral vision (see NN article) is distracting.
- If you're on desktop using a filter far down the page, if there are only a few results, they'll be off screen. Don't move focus, because moving focus is bad (see above). Could provide an in-context message?
- Not necessarily faster: requires more code and engineers away chunking.
- Need to reimplement the loading indicator. For screen readers, this is headache inducing.
- Will break the back button.

===

- Screen reader feedback: using AJAX requires a certain number of provisions such as a live region (extensively covered in chapters 1, 2, 3 and 5) to indicate loading states. When the user selects an option, the live region would have to be populated with ‘Loading results, please wait’, for example. Then when the request finishes, it would have to be populated with ‘212 results returned’. Having to hear this four times would be like listening to an overzealous disc jockey — headache inducing.
- Feedback: sighted users (long list on desktop, few results top of screen). Don't automatically move the user to the new results. However, if you have a long list of facets on desktop, and not many results, the user may not realise they need to scroll up to see the results.
- Not faster: Despite what you may have heard, AJAX isn't necessarily better or faster than a standard page refresh[^3]. First, it requires more Javascript code to be sent initially. Second, and more importantly, it engineers away progressive rendering (also called chunking)
- removes loading states, both of which the browser provides for free.
- Back button:  https://baymard.com/blog/macys-filtering-experience
- AJAX is more suited and beneficial when making updates to small parts of the page. Filters involve updating the majority of the page, making AJAX somewhat counterproductive.
- All that said, we can only be sure of what's best if we perform research with a diverse group of people, devices and connection speeds.
- With that said, the page refresh isn't a problem if you employ light-weight, well-optimised, single-focused pages.
- “if slow site or slow connection, use AJAX” - bollox. A slow page refresh means a slow ajax request.

## Applied Filters

https://baymard.com/blog/how-to-design-applied-filters

## An Adaptive Approach

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

## TODO: Filter Overload

Beware not to go crazy with filters. Overloading users with hundreds of filters makes their job harder, not easier. This is partially because of the paradox of choice which we discussed in “A Login Form”.

If you're not sure which filters to show, perform user research and check your analytics. Extract the most popularly used filters and include just those — in order too. Then you can either remove the others or reveal them as they become relevant as the user drills down.

Order of filters: most used.

If long list of filters within category, collapse the least common after 7/10

TODO: illustrations for this

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

## Todo

- Speed: how fast you can produce the results. If you expect the queries to be instantaneous then interactive filtering will be less offensive even to users in search mode. If your site can be slow, then batch filtering saves wait time.
- Inferring speed from width