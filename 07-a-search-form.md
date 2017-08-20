# Search And Filter

https://github.com/adamsilver/f/issues/40

autofocus bad

Consider dealing with search and filter of the inbox?

TODO: infinite scroll/show more/pagination perhaps

I'm an organised person. Even as I boy I remember being this way. But, I've always been a minimalist too. Organising things when you only own a few things is easy.

As a boy, I rarely lost things, but when I did, I just had to shout in the general direction of the resident search engine 'Where's my...' and the search engine would tell me.

By search engine, I mean mum. Mum knew where everything was. Not just my stuff. Everyone's. This was one of her many qualities. She didn't just know where stuff was, she knew the answer to almost any question I had.

If I could go back in time and accelerate my nerdyness a little bit, I might have nicknamed her Google.

Sometimes when I asked mum questions, she would ask me questions in return. Questions like "what colour is it?" and "is it a football or tennis top?".

I'd tell her and she would give me the answer. These questions are filters on a large set of results.

Now I'm a dad&mdash;as much as I try not to&mdash;I own more things. It's hard to be a minimalist with a wife and two children. Even if I meticulously organise our things it's hard to remember where everything is.

Sometimes I'm left to peruse each cupboard one by one. And of course when I need said thing and can't find it, I become anxious. If I really want it and know it exists I'll persevere. If not, I'll give up.

In this chapter we're going to ensure that finding things is easy. Just like mum we'll want our interface to answer questions accurately, quickly and as humanly as possible.

## Search

The first thing to ask ourselves is *do we need search?*

Kidly is a start-up that houses *the best stuff for your baby all in one place*. They wanted to create something great with a lot of wonderful features. Naturally, they were also eager to launch.

Deciding what to cut from the large backlog of work quickly became an essential skill. Launching an MVP within a reasonable timeframe meant having a critical understanding of the value each feature would give our beloved users.

We knew the product range would be small, at least to start with. This meant we could scrap search and rely on a well-organised navigation system.

This freed up a lot of our time and energy to solve more important problems. Nurturing a do-less mindset is something that benefits everyone because it encourages momentum, keeps the team focussed on the most essential thing for users with a product that maximises the value of the delivery effort.

This wouldn't be a particulary useful chapter on search if we decided to scrap the feature altogether so we'll assume we need it and design accordingly.

### Interface design

Throughout previous chapters we've easily been able to reuse components across different types of forms. This is because the form has been the main feature of the page.

Search is typically located in the header. This is because a user may want to search at any time. The problem is that the header is premium real estate. That is, there isn't much room available and it's highly sought after.

We can't just keep adding features into the header as not only does this push the main content furthe down the page, but it diminishes the value of the header. If everything is important, nothing is. And on mobile, there's even less room.

Designers are often seduced by novel patterns that save space[^hamburgerdeepdesign]. But generally speaking, hiding content behind a toggle, carousel, modal or what have you is a waste of time&mdash;both ours as designers, and for users. It's a waste of time for users because they don't appreciate having to reveal things all the time.

In our case though, the size of the header is something we'll want to say as small as possible. In all likelinesss, we'll have to collapse the search behind some sort of toggle.

Again, for space-saving reasons designers might be tempted to remove the label altogether and replace it with placeholder text but as discussed in the first chapter, relying on the placeholder is a bad usability move.

Seeing as the search form itself is behind a toggle, when it is displayed we can easily accomodate a label, an optional hint, the text box and search button easily.

There is no room within the header to reveal the search form&mdash;that's why we're collapsing it in the first place. The next logical place is immediately below the header:

[!]()

The button in the header will then be responsible for toggling the form's visibility. When the search form is displayed we should automatically move focus to it. Not only does this save users a click, but screen readers will announce that they can immediately start searching.

Lastly, we often use a magnifying glass icon to save more space but the best icon is text[^] so avoid an icon whereever possible. Text is fully inclusive out of the box and removes any ambiguity in meaning.

```HTML
```

### Search should find everything

Having designed the interface, next we should consider the search functionality itself. When I recounted my child-hood interaction with my mum I was able to ask her anything and she would know the answer.

In *Content and Design Are Inseparable Work Partners* Jared Spool explains that *content is the thing the user wants right now*. All too often search is limited to products or articles or videos. That is, the stuff stored in the database.

But users want more than that. As Jared says, they might be looking for a returns policy. But the returns policy doesn't reside in the database so a search yields no results.* Useless.

The label and optional hint should make it obvious to users what they can search for. If you must only show products, then tell users that it's a product search.

### A note about search analytics

We can improve our designs by tracking the most popular searches. Once we know the answer to this question we might change the way search works or we may add content or we may elevate something into the primary navigation bar. Either way, don't miss the opportunity to learn.

## Filter

Depending on the broadness of a search and the volume of content available, results may be in the thousands. Just like Mum, there are times when the system needs a little helping hand to be able to narrow in on what the user is looking for.

Filters, also known as facet navigation or guided navigation lets users refine a set of results by selecting particular attributes. The cool word for these attributes is meta data. This just means information about the information. For example, the information is *trousers* and the meta data is *black in colour*.

The beauty of a filter is that it allows users to find what they want using their own mental model, rather than a predefined one that can't possibly work for all users in all scenarios. Once again, giving users choice is a primary act of designing inclusive interfaces.

### A link filter

Some filters are actually lists of links as follows:

[!https://asset.uie.com/articles/img/faceted_search/BuzzillionsKMWorld.jpg](from UIE)

Links are offered for facets that have results. That's one of the advantages. If there are no Kodak cameras then there would be no link.

In *Resilient Web Design* Jeremy Keith talks about the idea of Material Honesty. He says *one material should not be used as a substitute for another. Otherwise the end result is deceptive.* In this case we want our links to look like links as that's what they are.

The problem is that some designers make links look like radio buttons. They'll use background images to make it appear as though the user is clicking a radio button. The problem is that it's not a radio button and doesn't have the functionality of a radio button. And they due to the custom imagery, the radio buttons always look a little off (or completely different) to the ones provided be browsers natively.

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
- Draings battery on mobile devices

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
[jared search find more than products]:https://medium.com/uie-brain-sparks/content-and-design-are-inseparable-work-partners-5e1450ac5bba

## Todo?

Don't make links look like checkbox/radios (https://medium.com/@z_rose/oh-boy-form-design-df6a71e39d60)
- Not using a label for the search form? (Jeremy and link to my article?).
- select box problem without submit button?