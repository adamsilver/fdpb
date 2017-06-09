# Search And Filter

I'm an organised person. Even as I boy I remember being this way. But, I've always been a minimalist too. Organising things when you only own a few things is easy.

As a boy, I rarely lost things, but when I did, I just had to shout in the general direction of the resident search engine 'Where's my...' and the search engine would tell me.

By search engine, I mean my mum. Mum knew where everything was. Not just my stuff. Everyone's. This was one of her many qualities. She didn't just know where stuff was, she knew the answer to almost any question I had.

If I could go back in time and accelerate my nerdyness a little bit, I might have nicknamed her Google.

Sometimes when I asked mum questions, she would ask me questions in return. Questions like "what colour is it?" and "is it a football or tennis top?".

I'd tell her and she would give me the answer. These questions are filters on a large set of results.

I'm a dad now, and as much as I try not to, I own more things. It's hard to be a minimalist with a wife and child. Even if I meticulously organise our things it's hard to remember where everything is.

Sometimes I'm left to peruse each cupboard one by one. And of course when I need said thing and can't find it, I become anxious. If I really want it and know it exists I'll perservere. If not, I'll give up.

In this chapter we're going to ensure that finding things is easy. Just like mum we'll want the interface to answer questions accurately, quickly and as humanly as possible.

## Search

The first thing to ask ourselves is *do we need search?*

Kidly is a start-up that houses *the best stuff for your baby all in one place*. They wanted to create something great with a lot of wonderful features. Naturally, they were also eager to launch.

Deciding what to cut from the large backlog of work quickly became an essential skill. Launching an MVP within a reasonable timeframe meant having a critical understanding on the value each feature would provide our users.

We knew that our product range would be small, at least to start with. This meant we could scrap the ability to search. Instead we could really on a well organised navigation system.

This freed up a lot of our time and attention to solve more important problems. Nurturing a do-less mindset is something that benefits everyone because it encourages momentum, keeps the team focussed on the most essential thing for users with a product that maximises the value of the delivery effort.

This wouldn't be a particulary useful chapter on search if we decided to scrap the feature altogether so we'll assume we need it and design accordingly.

### Interface design

Throughout previous chapters we've easily been able to reuse components across different types of forms. This is because the form has been the primary feature on the page.

Search is typically located in the header. This is because a user may want to search at any time. The problem is that the header is premium real estate. That is there isn't much room available and it's highly sought after. 

We can't just keep adding features into the header as not only does this push  page content down, but it diminishes the value of the header. If everything is important, nothing is. On mobile, there's even less room. This is why designers are often seduced by novel patterns that save space[^hamburgerdeepdesign]. 

Putting something in the header promotes access and importance. To then hide it with Javascript somewhat defeats the point. True to life, design is not always black and white. If we have little space to work with we may well need to collapse it.

Again, for space-saving reasons designed might be tempted to remove the label altogether and replace it with placeholder text. Placeholder text, as we've already discussed in chapter one, suffers from many usability problems.

To accomodate a well-designed search form with an ever-present label we need to consider the content of the header holistically and prioritise. There are four main aspects you might find in a typical website.

- logo
- search
- main navigation (categories)
- account navigation (login, profile, orders)

Account navigation is probably the least used and so is a prime candidate to be collapsed. And we're probably going to want to simplify our main navigation taxonomy to promote a mobile-first and therefore small-screen accomodating design.

Here's Kidly's header as shown for those with small screens:

[!]()

You'll see even with careful thought, we're unable to fit an ever present fully inclusive search form without making the header deeper. Assuming a deeper header is problematic we'll want to have a collapsed state.

Afterall it's better to have something reveal well (with large text and accomodating messaging) than it is to have something ever present and hard to read (due to small text and placeholder text for example). If we're going to reveal the search form then we'll need to do so under the header.

To do this make sure the search button in the header reveals a search form and automatically moves focus to it. Not only does this save users a click, but it means screen readers will know they can immediately start their search.

Iconography in the form of a magnifying glass is also used to save space. Due to its prolific use a magnifying glass is quite well understood. But the best icon is a text icon, so if your tone and design can accomodate it use text. It's fully inclusive out of the box and removes any ambiguity in meaning.

In summary:

- Don't use a placeholder
- Always show a label
- Use text instead of an icon where possible for both the toggle button and the submit button.
- If the form starts hidden, use aria-expanded and focus the search box as soon as the user toggles the display.
- Use `input type="search"` to get the benefits as we discussed in chapter three.

### Search should find everything

Once we have the interface design sorted we need to consider the functionality of the search itself. When I recounted my child-hood interaction with my mum I was able to ask her anything and she would know the answer.

In *Content and Design Are Inseparable Work Partners* Jared Spool explains that *content is the thing the user wants right now*. All too often search is limited to products or articles or videos. That is stuff that is in a database.

But users want what they want. As Jared says, they might be looking for a returns policy. But the returns policy doesn't reside in a database table so a search yields no results. Useless.

It's wise to use the hint pattern to tell users what it is they can and can't search for.

[!]()

### A note about search analytics

We can improve our designs by tracking the most popular searches. Once we know the answer to this question we might change the way search works or we may add content or we may elevate something into the primary navigation bar. Either way, don't miss the opportunity to learn.

## Filter

Depending on the broadness of a search and the volume of content available, results may be in the thousands. Just like Mum, there are times when the system needs a little helping hand to be able to help the user find the one thing they were looking for.

Filters, also known as facet navigation or guided navigation lets users refine a set of results by selecting particular attributes. The cool word for these attributes is meta data. This just means information about the information. For example the information is *trousers* and the meta data is *black in colour*.

The beauty of a filter is that it allows users to find what they want using their own mental model, rather than a predefined one that can't possibly work for all users under all scenarios.

### A link filter

Some filters are actually just organised lists of links as follows:

[!https://asset.uie.com/articles/img/faceted_search/BuzzillionsKMWorld.jpg](from UIE)

If your filter is a list of links then just be sure to be materially honest and make sure the links look like links. I've seen many incarnations that semantically under the hood use links but are made to look like radio buttons or checkboxes. 

Designing honestly is a quality we want to upkeep. Mixing form controls and links is a recipe for confusion and sets the wrong expectation for behaviour. That is, with a form I expect to select something and submit. With a form I don't.

### Radio buttons and checkboxes

Other filters are actual forms and in this case we'll want to make sure our checkboxes and radio buttons look as such. In fact there is no reason to give them differential visual treatment to those which we have used throughout the book.

The only thing that differs is the location of the filter. On big screens we might choose to show it beside the list of results. On small screens we're probably going to want to collpase it to elevate the results themselves, much like the approach we have taken with the global search form.

### Separate submission from selection

We've talked about this before[^?] but we're going to want to stay true to familar conventions by ensuring submission is separate from selection. Like select boxes, we shouldn't submit a form when the user checks a radio button for example.

A good reason for this is that the user may want to choose several facets before submission. For example, if they're searching for a pair of shoes they may want to select the size and colour at the same time. Doign this with two separate interactions is wasteful of their time and the load on the server.

### Checkboxes versus radio buttons

Users may want to select multiple options within the same category. For example, if I'm shopping for shoes I may not be sure of my size and want to search for two different sizes at the same time.

In this case allowing the user to select both options is important. Obviously we should reflect this by using checkboxes. Radio buttons are for selecting one option within a category.

### And or OR results

If the user can choose many options to filter by at the same time then how we treat the search is very different depending on the way we see things. We might say find me products that are red and large. Meaning that red items that aren't large are not returned in the results.

Or we might say find me products that are red or large. Meaning that the results will contain black large things and red small things. This search becomes an OR.

Choosing OR is good because it guarantees that results will be returned but I wonder about the value here. Why would a user search for something that is red OR black, for example?

### Do you need to refresh results with AJAX?

AJAX is often used to patch over websites that have bad performance, normally because we cram so much shit on a page. But AJAX doesn't negate a server round trip.

Also, the search results are what make up the majority of the page. Might as well do a page refresh.

### Don't offer hundreds of facets

### More dos and donts

## Summary

## Footnotes

[Facet search]: https://articles.uie.com/faceted_search/
[Don't make links look like checkbox/radios]: https://medium.com/@z_rose/oh-boy-form-design-df6a71e39d60)
[jared search find more than products]:https://medium.com/uie-brain-sparks/content-and-design-are-inseparable-work-partners-5e1450ac5bba

## Todo?

Don't make links look like checkbox/radios (https://medium.com/@z_rose/oh-boy-form-design-df6a71e39d60)
- Not using a label for the search form? (Jeremy and link to my article?).
- Material honesty - check Resilient Web Design book for ref.
- select box problem without submit button?

## Heydon filter notes?

First thing to ask is why don't radios with labels and a separate button work.

One thing I can think of is that a user doesn't know they have to submit.  This is just the way forms work.but because we have fucked them up over the years we may have to reteach them a bit.
z

This isn't some crazy anti pattern "if u ha e to explain u have failed". Just highlight the button after say 2 seconds for those that seem confused after making their selection.

Everything else is a nonsense approach. Save space. Branding. Less clicks etc.
