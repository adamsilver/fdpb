# Search And Filter

I'm an organised person. Even as I boy I remember being this way. But, I've always been a minimalist too. Organising things when you only own a few things is easy.

As a boy, I rarely lost things, but when I did, I just had to shout in the general direction of the resident search engine 'Where's my...' and the search engine would tell me.

By search engine, I mean my mum. Mum knew where everything was. Not just my stuff. Everyone's. This was one of her many qualities. She didn't just know where stuff was, she knew the answer to almost any question I had.

If I could go back in time and accelerate my nerdyness a little bit, I might have nicknamed her Google.

Sometimes when I asked mum questions, she would ask me questions in return. Questions like "what colour is it?" and "is it a football or tennis top?".

I'd tell her and she would give me the answer. These questions are filters on a large set of results.

I'm a dad now, and as much as I try not to, I own more things. It's hard to be a minimalist with a wife and child. Even if I meticulously organise our things it's hard to remember where everything is.

Sometimes I'm left to peruse each cupboard one by one. And of course when I need said thing and can't find it, I become anxious. If I really want it and know it exists I'll perservere. If not, I'll give up.

In this chapter we're going to design a search and filter which helps users and avoids making them feel anxious. Just like mum we'll want the interface to answer our questions accurately and as quickly as possible.

## Search

The first thing to ask ourselves is *do we need search?*

In 2016, I worked at Kidly, a start-up that houses *the best stuff for your baby all in one place*. They wanted to build a lot of features. Naturally, they were also eager to launch.

Deciding what to cut became an essential skill in forming our MVP within a reasonable time frame. We realised that the product range, at least on day one, would be small. 

We felt we could do without search and rely on a well organised browse system. So we cut out search from the launch backlog.

Nurturing a do-less mindset is something that benefits everyone because it encourages momenturm, keeps the team focussed on the most essential thing for customers and in turn maximises the value of the delivery.

This chapter wouldn't be particularly interesting if we decided not to build a search feature so we'll assume that our users need it.

### Interface design

Throughout previous chapters we've easily been able to reuse components across different types of forms. This is because the form has been the primary feature on the page.

Search is typically located in the header. This is because a user may want to search at any time. The problem is that the header is premium real estate. That is there isn't much room available and it's highly sought after. 

We can't just keep adding features into the header as not only does it diminish the value of the header but it pushes the page content down. On mobile, there's even less room so we face an even greater design challenge.

This is why designers are often seduced by novel patterns that save space[^hamburgerdeepdesign]. But putting something in the header to make it readily accessible, only to hide it with Javascript somewhat defeats the point.

If it's going to be frequently used, then we should do our best to keep it visible. If we don't think it will be used often, we might want to question it's existence.

Another common problem is that some sites use placeholder text as a replacement for a separate label. We already know that this hurts users because we discussed this at length in chapter 1.

To accomodate a well-design search form with an ever-present label we need to consider the content of the header holistically and prioritse its importance. There are four main aspects you might find in a typical website.

- logo
- search
- main navigation (categories)
- account navigation (login, profile, orders)

Account navigation is probably used infrequently in comparision and so should probably be the lowest priority and a prime candidate to be collapsed down. And we're probably going to want to simplify our main navigation taxonomy to promote a mobile first and therefore small-screen accomodating design.

Despite this, we may still need to keep our search form small and potentially collapsible. Afterall it's better to have something reveal well (with large text and accomodating messaging) than it is to have something ever present and hard to read (due to small text and placeholder text for example).

In this case here are summarising list of tips:

- Don't use a placeholder.
- Always include a label.
- Strive to keep this label visually accessible.
- Avoid using a maginifying glass for an icon as the best icons are text.
- If you must hide the label do so in an accessible way.
- If you must hide the form then make sure that the search box is focussed when the user reveals the form (like Medium).
- Use input type=search due to the benefits we discussed in chapter 3

What it looks like:

[]()

HTML:

```HTML
<div>
	label
	input type search
</div>
```

### Search should find everything

- TODO
- if can find anything then tell users they can find anything. This is why it's good to reveal it and give it the space to tell users this with the hint pattern. 

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
