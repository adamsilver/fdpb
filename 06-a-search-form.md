# A search form

I'm an organised person. Even as a boy, I remember always having ‘a place’ for things. To be fair, I've always been minimalist too. Organising things when you only own a few things is easy. So I guess, it's unsurprising that I rarely lost things. On the odd occasion that I did, I just shouted in the general direction of the resident search engine: ‘Where's my..., ’ and I'd have my answer.

By search engine, I mean Mum! Mum knew where everything was, not just my stuff — everyones. This was one of her many qualities. She didn't just know where stuff was, she knew the answer to everything (at least that's how I remember it). If I could go back in time and accelerate my nerdiness a little, I might have nicknamed her Google.

Now I'm a dad, being a minimalist is hard with a wife and two children. Even if I meticulously organise our things, it's still hard to remember where everything is. Worst case scenario: I have to peruse each cupboard one-by-one hoping that the thing hasn't been lost which is time-consuming.

In this chapter, we're going to design a responsive search form. Like Mum, we'll want it to be readily-available and on-hand to answer *any* question users have. To make this happen, there are a few crucial things to consider.

## Search Everything

When I recounted my childhood conversation with Mum, I was not only able to ask her where my stuff was, but really, I was able to ask her anything. When designing a global search form, users should be able to do the same thing. Too often, users can only find stuff that lives in the database. For example, on Amazon, the search will only return products. And on Youtube, the search will only return videos.

In “Content and Design Are Inseparable Work Partners”[^1], Jared Spool explains that “content is the thing the user wants right now”. He recounts a story from user research where someone was trying to buy a purse. 

The lady was happy enough to buy the purse on the proviso that she could return it. But the returns policy wasn't on the product page, or in the FAQ. In the end, she tries typing “Refund policy” into the search box but it returned no results. That was the end of the research session.

The returns policy isn't a product. Nor does it reside in the database. But this is what she wanted. We often hear how content is king, but the design of the site lets the content down. Where ever possible search should search everything. And if it doesn't, the label should be explicit. If, for example, the search only returns products then make that clear within the interface.

On a side note, you should use analytics to track what your users are searching for. If the most popular searches are retriving empty results, then you can make provisions to improve the experience based on data.

## Interface design consideration

*TODO: reference Heydon's TODO article, but talk about how the placeholder shouldn't duplicate the hidden labels contents. And if you need to tell users what they can search, then you'll need to adjust your approach. That is by having a visible descriptive label and hint outside the field. Giving an example may be counterintuitive.*

The form itself is simple enough and will contain a label, a search input (`input type="search"`) and a submit button. What's tricky is that it's usually placed within the header. The purpose of placing it at the top is not only for cognition, but interaction. Like navigation, we want to make search readily accessible to both mouse and keyboard users. Putting such an important and commonly used feature elsewhere is counterintuitive.

The problem with placing it inside the header is that the header is premium screen real estate. That is, there isn't much room available and it's highly sought after. The more that we put into the header, the more the main content is pushed down the page. On mobile, of course, space is even more limited.

![Search form with label underneath a header pushing page down](.)

As we've talked about previously, designers are often seduced by novel, space-saving design patterns such as the hamburger menu[^2], but hiding content should always be a last resort. On desktop, this isn't as much of a problem as there's usually plenty of room. On mobile though, we're going to have to think about it.

One space-saving tactic is to (visually) hide the label, replacing it with placeholder text. [But, we've talked about how problematic this is in “A registration form”.] In any case, removing the label doesn't save all that much space. And without one, it's still going to be hard to squeeze the rest of the form into the header.

![Selfridges no label](.)

One approach is to give users a different treatment on small and large viewports. Harrods do this, for example. On large viewports they show the entire form. On small viewports, they toggle the search form's visibility with a button (usually styled conventionally as a magnifying glass).

Another, increasingly popular approach is to toggle the search form's visibility no matter the viewport size: big or small. Both Medium and Kidly take this approach. This simplies the code, but does mean making search unnecessarily less prevalent when there's plenty of space. 

- https://www.gov.uk/government/organisations/hm-revenue-customs
- https://www.harrods.com/en-gb




Really, for small viewports, we're going to have to toggle the search form's visibility using CSS and Javascript.

With this decision in place, accomodating a visual label and optional hint is easy. This way, the toggle button can be placed inside the header, with the search form directly below.

![Collapsed](.)

When the toggle button is clicked, the search form is shown and focus set to the search box. Not only does this save users an extra click, but on focus, screen readers will announce the label.

![Expanded](.)

## Summary

In this chapter, we reused patterns from previous chapters to design a responsive and togglable search form that saves space on mobile. We also ensured the quality of the interface is married to the quality of the search results: that whatever the user searches for is retrieved accordingly.

### Things To Avoid

- Only searching the database for content.
- Using placeholder text as a label replacement.

## Footnotes

[^1]: https://medium.com/uie-brain-sparks/content-and-design-are-inseparable-work-partners-5e1450ac5bba
[^2]: http://jamesarcher.me/hamburger-menu

## TODO

 *(Note: toggling the behaviour for small and large viewports is set out in the previous chapter, “An Inbox”, for the action menu.)*

 Every form needs a submit button. And the related implicit submission issue again without js.

Don't reduce hit area just to squeeze it in.

Field size. Don't crop too much

## After search

- Maintain entered text
- Display count
- ‎Let them sort
- ‎pagination not show more. When show more works and doesn't.
- ‎let them filter (next chapter)
