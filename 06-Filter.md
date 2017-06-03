# Search And Filter

I'm organised. Even as a boy I was organised. I've always been minimalist too. Organising things when you only have a few things is easy.

I rarely lost things but when I did, all I had to do was shout in the general direction of the resident search engine "Where's my..." and the search engine would tell me.

By search engine, I mean my mum. Mum knew where everything was. Not just my stuff. Everyone's. This was one of her many qualities. She didn't just know where stuff was, she knew the answer to any question I had.

If I could go back in time and accelerate my nerdyness a little bit, I might have nicknamed her Google.

Sometimes when I asked mum questions, she would ask me questions in return. Questions like "what colour is it?" and "is it a football or tennis top?".

I'd tell her and she would give me the answer. These questions are filters on a large set of results.

I'm a dad now, and as much as I try not to, I own more things. It's hard to be a minimalist with a wife and kid. Even if I meticulously organise our things it's hard to remember where everything is.

I'm left to peruse each cupboard one by one. And of course when I need said *thing* and can't find it, I become anxious. If I really want it and know it exists I'll perservere. If not, I'll give up.

In this chapter we're going to design a search and filter which won't make users anxious. Just like mum we'll want these features to be in easy reach and as human as possible.

We'll start with search.

## Search

As ever, the first question is *do we need it*? And, as ever, the answer is *it depends*.

In 2016, I worked at Kidly, a start-up that houses *the best stuff for your baby all in one place*. They had grand visions for all the features they wanted to provide their customers.

But we wanted to launch quickly. Working out what we could cut was an essential skill in forming our MVP and managing to launch in a reasonable time.

As we only had a small product range to begin with our navigation menu was simple. Finding a product in a small range is easy. So we decided to cut search as a feature until the feature is needed.

Nurturing an MVP mindset is something that benefits everyone because it keeps the team focussed on the most essential thing for customers and in turn maximises the value of the delivery.

We'll assume search is a user need otherwise this would be the end of a promising chapter.

### UI

- location
- responsive
- lack of label (Jeremy)
- html and html5 input type and keyboard
- if important show it
- magnifying and iconography last resort. Don't be different with iconography.

### Search should find everything

- TODO

### Measure what users want most

- and make it a nav item (jared)

## Filter

- Don't make links look like checkbox/radios (https://medium.com/@z_rose/oh-boy-form-design-df6a71e39d60)
- Material honesty - check Resilient Web Design book for ref.
- AND = checkboxes
- OR = radios
- select box problem without submit button.
- One submit button, save many ajax requests, is ajax worth it for entire page of results?

## Heydon filter notes

First thing to ask is why don't radios with labels and a separate button work.

One thing I can think of is that a user doesn't know they have to submit.  This is just the way forms work.but because we have fucked them up over the years we may have to reteach them a bit.

This isn't some crazy anti pattern "if u ha e to explain u have failed". Just highlight the button after say 2 seconds for those that seem confused after making their selection.

Everything else is a nonsense approach. Save space. Branding. Less clicks etc.

## Footnotes

[Facet search]: https://articles.uie.com/faceted_search/
[Don't make links look like checkbox/radios]: https://medium.com/@z_rose/oh-boy-form-design-df6a71e39d60)
[jared search find more than products]:https://medium.com/uie-brain-sparks/content-and-design-are-inseparable-work-partners-5e1450ac5bba

