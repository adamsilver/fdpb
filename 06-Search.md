# Search And Filter

I'm organised. I remember I've always been organised, even as a boy. I've always been a minimalist too. Organising things when you only own a few things is easy.

I rarely lost things but when I did, all I had to do was shout in the general direction of the resident search engine "Where's my..." and the search engine would tell me.

By search engine, I mean my mum. Mum knew where everything was. Not just my stuff. Everyone's. This was one of her many qualities. She didn't just know where stuff was, she knew the answer to any question I had.

If I could go back in time and accelerate my nerdyness a little bit, I might have nicknamed her Google.

Sometimes when I asked mum questions, she would ask me questions in return. Questions like "what colour is it?" and "is it a football or tennis top?".

I'd tell her and she would give me the answer. These questions are filters on a large set of results.

I'm a dad now, and as much as I try not to, I own more things. It's hard to be a minimalist with a wife and kid. Even if I meticulously organise our things it's hard to remember where everything is.

I'm left to peruse each cupboard one by one. And of course when I need said *thing* and can't find it, I become anxious. If I really want it and know it exists I'll perservere. If not, I'll give up.

In this chapter we're going to design a search and filter which won't make users anxious. Just like mum we'll want the interface to answer our questions accurately and as quickly as possible.

## Search

As ever, the first question is *do we need it*? And, as ever, the answer is *it depends*.

In 2016, I worked at Kidly, a start-up that houses *the best stuff for your baby all in one place*. They had grand visions for all the features they wanted to provide their customers.

But naturally, Kidly were eager to launch. Deciding what to cut was essential in forming our MVP and managing to launch in a reasonable time frame.

As we only had a small product range to begin with our navigation menu was simple. Browsing a small range of products is easy. For this reason we cut search from the backlog.

Nurturing an MVP mindset is something that benefits everyone because it keeps the team focussed on the most essential thing for customers and in turn maximises the value of the delivery.

We'll assume our users need search. Otherwise that would be the end of an interesting chapter.

### Interface design

Throughout previous chapters we've been able to reuse form patterns because they have been the main feature of the page or flow in question.

Search is typically located in the header. This is because just like navigation, a user may want to search at any time.

The problem is that the header is like premium real estate. There isn't much room and it's highly sought after.

We can't just keep adding features into the header as not only does it diminish the value of the header but it pushes the content down. I won't dare mention the elusive fold.

On mobile, there's even less room and so the design challenge is even greater.

This is why designers are often seduced by novel patterns that save space[^hamburgerdeepdesign]. But putting something in the header to make it easy, only to hide it somewhat defeats the point.

If it's going to be frequently used, then we should do our very best not to hide it. If we don't think it will be used often, we might want to question it's existence.

Medium even on large viewports hides the search form behind a magnifying glass icon meaning I have to click before typing. Is this wrong? Not necessarily but it's something to think about.

[](!)

Another common problem is that some sites use a placeholder to replace a label, and we already discussed the problem at length in chapter 1.

To accomodate a well-design search form with an ever-present label we need to consider the header holistically. To do this we'd have to know what's going in your header.

- logo
- account navigation (login etc)
- main nav (product categories perhaps)
- search

Here's a few tips:

1. Account navigation is often used infrequently and should therefore be lowest priority. We might consider putting it elsewhere or collapsing it behind an icon.
2. Don't use a placeholder. If you absolutely must hide the label, then do so in an accessible way.
3. If you have to collapse the search form, then when revealing it, automically move focus to the text box.

### Input type search

TODO

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

When we have to squeeze stuff we may kid ourselves into removing the label. (Jeremy).
