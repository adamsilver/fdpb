# A Filter Form

In the introduction to “A Search Form”, you'll recall the type of conversation I used to have with Mum. Sometimes I would ask “Where's my black top?”. But this was so vague that Mum would respond with questions like “Is it a football or tennis top?”. This question is a filter on a large set of results.

On the web, filters, sometimes referred to as facet navigation or guided navigation, lets users refine a large set of search results. Were filters straightforward, I could have folded this pattern into the last one, but there are a number of challenges and concerns to address: the type of element to use; keeping to conventions; potential AJAX enhancements; how to make it work just as well on small screens as it does on large ones. All of these things need to be taken into account.

First of all, though, it needs to be said that if you don't need a filter, don't include one. They're only useful if searching returns a vast amount of results. In the case of Google, for example, most people aren't willing to click beyond the first or second page.

On the web, a search can yield thousands, or even millions of results depending on the content available. But, as humans, we can't juggle more than approximately seven things at once[^1], so being able to narrow them down is crucial. The ability to filter not only offers an additional dimension of control, but it does so in a way that matches each user's own mental model. 

In “Designing for Faceted Search”[^2], Stephanie Lemieux says:

> Think of a cookbook: authors have to organize the recipes in one way only — by course or by main ingredient — and users have to work with whatever choice of organizing principle that has been made, regardless of how that fits their particular style of searching. An online recipe site using faceted search can allow users to decide how they’d like to navigate to a specific recipe [by course type, cuisine or cooking method, for example].

While filters often look similar on many sites, their behaviours vary widely. When to apply the filtering, what interface components should be used, how to inform users of the updated results: it all needs consideration.


## Denoting Selected Filters

Having selected one or more filters, they need to be demarcated on the interface. Baymard's usability study shows that there are two approaches that should be used in combination[^combo]:

### Original Position

The first approach is to show the selected filters in their original position alongisde the other, unselected filters. This is because some users will remember where the filter was originally when they selected it.

![Illustrate](.)

Having it disappear (or moved somewhere) is frustrating. It also gives users context as to what's selected within the category that the user is currently perusing. That is, it might impact their next action.

### Separate List

The second approach is to group all the selected filters in a list at the top.

![Illustrate](.)

This is useful because it gives users confirmation of their selection providing context to the results that are currently being viewed. But it also easy access to remove any applied filters, without having to scroll down the page to find the selected filter amongsts a long list.

## The Small Screen Experience

In chapter 5, “An Inbox”, we discussed the differences between responsive design and adaptive design. In short, a responsive approach is preferred because not only is it less work, but it's more robust and performant for users.

Mobile first, to me at least, just means small screen first. Which really means essential first, which really really means essential only.

Normally speaking, if you cut out the superfluous content and lay out what remains in a small viewport, the experience works well. Of course, this scales up nicely to large viewports too: an increase in font-size and whitespace is usually enough.

In this case, the essential components are both the results and the filter itself. But because both aspects of the interface are important, the filter needs to be almost as prominent as the results.

On large viewports this is easy: you just lay them out side by side. But on small viewports, you'd have to place the results underneath the filter which pushes the main content down. Alternatively, you could place the filter after the results, but then users would have to scroll (or tab) past the results to discover it — many would miss it.

What we're left with is having to collapse the filters behind a toggle button. As long as the button is relatively prominent (and well labelled), it should still be discoverable without pushing the results too far down the page.

This is a good start, but it's not necessarily the end of the story. As noted earlier, Gumtree's user research showed that users expect the form to be submitted as soon as they select a filter. 

However, their research also showed that on mobile, AJAX wasn't desirable because users couldn't see the results refresh. Here's what David House had to say:

> On mobile, AJAX wasn't desirable because users couldn't see any visible refresh. We didn't want to move focus to the results because users wanted to pick more than one filter. We reluctantly had to use an adaptive approach. On mobile, clicking the filter menu button would reveal a batch filtering system that worked without AJAX. On Desktop, users got an AJAX experience where filters were immediately applied.

Because the mobile and desktop users required different experiences, Gumtree reluctantly used an adaptive approach, but there might be a better, more responsive technique.

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
[^scroll]: https://baymard.com/blog/inline-scroll-areas
[^tray]: https://www.nngroup.com/articles/mobile-faceted-search/

## Todo?

- “Refine” vs “Filter”
