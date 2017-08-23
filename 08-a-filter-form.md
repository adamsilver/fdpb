# A filter form

Filters go with search like ying goes with yang.

Depending on the broadness of a search and the volume of content available, results may be in the thousands. Just like Mum, there are times when the system needs a little helping hand to be able to narrow in on what the user is looking for.

Filters, also known as facet navigation or guided navigation lets users refine a set of results by selecting particular attributes. The cool word for these attributes is meta data. This just means information about the information. For example, the information is *trousers* and the meta data is *black in colour*.

The beauty of a filter is that it allows users to find what they want using their own mental model, rather than a predefined one that can't possibly work for all users in all scenarios. Once again, giving users choice is a primary act of designing inclusive interfaces.

### A link filter

Some filters are actually lists of links as follows:

[!https://asset.uie.com/articles/img/faceted_search/BuzzillionsKMWorld.jpg](from UIE)

Links are offered for facets that have results. That's one of the advantages. If there are no Kodak cameras then there would be no link.

In *Resilient Web Design* Jeremy Keith talks about the idea of Material Honesty. He says *one material should not be used as a substitute for another. Otherwise the end result is deceptive.* In this case we want our links to look like links as that's what they are.

The problem is that some designers make links look like radio buttons. They'll use background images to make it appear as though the user is clicking a radio button. The problem is that it's not a radio button and doesn't have the functionality of a radio button (clicking it immediately navigates, without having to submit). And they due to the custom imagery, the radio buttons always look a little off (or completely different) to the ones provided be browsers natively.

Avoid this wherever possible.

### Radio buttons and checkboxes

Other filters are forms and in this case we'll want to make sure our checkboxes and radio buttons look as such. In fact there is no reason to give them differential visual treatment to those which we have used throughout the book.

The only thing that differs is the location of the filter. On big screens we might choose to show it beside the list of results. On small screens we're probably going to want to collpase it to elevate the results themselves, much like the approach we have taken with the global search form.

### Separate submission from selection

‘Use checkboxes and radio buttons only to change settings, not as action buttons’ https://www.nngroup.com/articles/checkboxes-vs-radio-buttons/

We've talked about this before[^?] but we're going to want to stay true to familar conventions by ensuring submission is separate from selection. Like select boxes, we shouldn't submit a form when the user checks a radio button for example.

A good reason for this is that the user may want to choose several facets before submission. For example, if they're searching for a pair of shoes they may want to select the size and colour at the same time. Doing this with two diseparate interactions is wasteful of their time and puts unnecessary load on the server.

### Checkboxes versus radio buttons

Users may want to select multiple options within the same category. For example, if I'm shopping for shoes I may not be sure of my size and want to search for two different sizes at the same time.

In this case allowing the user to select both options is important. Obviously we should reflect this by using checkboxes. Radio buttons are for selecting one option within a category.

### And or or results

If the user can choose many options to filter by at the same time then how we treat the search is very different depending on the way we see things. We might say find me products that are red and large. Meaning that red items that aren't large are not returned in the results.

Or we might say find me products that are red or large. Meaning that the results will contain black large things and red small things.

Choosing *or* is good because it guarantees that results will be returned but I wonder about the value here. Why would a user search for something that is red or black, for example?

The right answer always ‘depends’.

### Do you need to refresh results with AJAX?

AJAX is often used to patch over websites that have bad performance, normally because we cram so much shit on a page. But AJAX doesn't negate a server round trip and unfortunately it comes with some trade-offs:

- Adds weight to original request
- Engineers away chunking
- Removes the browser's loading indicator
- Needs to design loading and success states
- More code, more testing, more chance of problems
- Increases the chance of memory leaks
- Drains battery on mobile devices

AJAX is useful when you want to make a small update to the screen with the majority of the page staying the same. When filtering, the main feature of the page itself changes. Both the results and the filter itself.

In this case, not only does it defeat the point, but it may well make the page slower.

### Dos and Don'ts

UIE has some high level guidance on facet navigation design:

- DON’T go crazy with facets. This overwhelms the user.
- DO base facets on key use cases & known user access patterns. Use research, and analytics to help decide on included facets.
- DO order facets & values based on importance. Use the same data to decide order.
- DO show additional facets when they become relevant. Start with high level facets, then reveal others as the user drills down.
- DO collapse the least popular facet items. Within a facet show the first 5 and collpase the rest behind a show more.
- DO build your taxonomy with faceted search in mind. A broader shallower taxonomy creates a better experience as users don't end up far down a funnel.

## Summary

TODO

## Footnotes

[Facet search]: https://articles.uie.com/faceted_search/
[Don't make links look like checkbox/radios]: https://medium.com/@z_rose/oh-boy-form-design-df6a71e39d60)
- Don't make links look like checkbox/radios (https://medium.com/@z_rose/oh-boy-form-design-df6a71e39d60)
- select box problem without submit button?
- https://github.com/adamsilver/f/issues/40
- infinite scroll/show more/pagination perhaps