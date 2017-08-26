# A search form

I'm an organised type of guy. Even as a boy, I remember always having ‘a place’ for things. To be fair, I've always been minimalist too. Organising things when you only own a few things is easy. So it's unsurprising that I rarely lost things. On the odd occasion that I did, I just shouted in the general direction of the resident search engine: ‘Where's my...’ and I'd have my answer.

By search engine, I mean Mum! Mum knew where everything was, not just my stuff&mdash;everyones. This was one of her many qualities. She didn't just know where stuff was, she knew the answer to everything (at least that's how I remember it). If I could go back in time and accelerate my nerdiness a little, I might have nickednamed her Google.

Now I'm a dad, and being a minimalist is hard with a wife and two children. Even if I meticulously organise our things, it's still hard to remember where everything is. Worst case scenario: I have to peruse each cupboard one-by-one hoping that the thing hasn't been lost which is time consuming.

We're going to look at how to make search easy. Just like Mum, we'll want the interface to ensure search is readily available to answer questions quickly and accurately.

## Search everything

Having designed the interface, next we should consider the search functionality itself. When I recounted my child-hood interaction with my mum I was able to ask her anything and she would know the answer.

In *Content and Design Are Inseparable Work Partners* Jared Spool explains that *content is the thing the user wants right now*. All too often search is limited to products or articles or videos. That is, the stuff stored in the database.

But users want more than that. As Jared says, they might be looking for a returns policy. But the returns policy doesn't reside in the database so a search yields no results.* Useless.

The label and optional hint should make it obvious to users what they can search for. If you must only show products, then tell users that it's a product search.


## A note about search analytics

We can improve our designs by tracking the most popular searches. Once we know the answer to this question we might change the way search works or we may add content or we may elevate something into the primary navigation bar. Either way, don't miss the opportunity to learn.

## A note on autofocus

## Interface design

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

## Summary

## Footnotes/todo

[jared search find more than products]:https://medium.com/uie-brain-sparks/content-and-design-are-inseparable-work-partners-5e1450ac5bba
- Not using a label for the search form? (Jeremy and link to my article?).