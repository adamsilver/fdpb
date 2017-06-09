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

It might be wise to use a the hint pattern to tell users what it is they can and can't search for.

[!]()

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
