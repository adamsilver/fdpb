# A search form

I'm an organised type of guy. Even as a boy, I remember always having ‘a place’ for things. To be fair, I've always been minimalist too. Organising things when you only own a few things is easy. So it's unsurprising that I rarely lost things. On the odd occasion that I did, I just shouted in the general direction of the resident search engine: ‘Where's my..., ’ and I'd have my answer.

By search engine, I mean Mum! Mum knew where everything was, not just my stuff—everyones. This was one of her many qualities. She didn't just know where stuff was, she knew the answer to everything (at least that's how I remember it). If I could go back in time and accelerate my nerdiness a little, I might have nicknamed her Google.

Now I'm a dad, being a minimalist is hard with a wife and two children. Even if I meticulously organise our things, it's still hard to remember where everything is. Worst case scenario: I have to peruse each cupboard one-by-one hoping that the thing hasn't been lost which is time-consuming.

In this chapter, we're going to design a responsive search form. Like Mum, we'll want it to be readily-available and on-hand to answer *any* question users may have. To make this happen, there are a few crucial things to consider.

## Search everything

When I recounted my childhood conversation with Mum, I was not only able to ask her where my stuff was, but really, I was able to ask her anything. When designing a site-wide and global search form, users should be able to do the same thing. Too often, users can only find stuff that lives in the database. For example, on Amazon, the search will only return products, and on Youtube, the search will only return videos.

In ‘Content and Design Are Inseparable Work Partners’[^1], Jared Spool explains that *content is the thing the user wants right now*. He recounts the following user research session where someone was trying to buy a purse. She was happy enough to buy it on the proviso that she could return it.

The returns policy wasn't on the product page, or in the FAQ. In the end, she tries the search box. She types ‘Refund policy’ and the search returns no results. That was the end of the research session.

The returns policy isn't a product. Nor does it reside in the database. But this is what she wanted. We often hear how content is king, but the design of the site let the content down. Where ever possible search should search everything. And if it doesn't, the label should be explicit. If, for example, the search only returns products then make that clear.

If you have analytics in place, you can track the most popular search terms and the results that come back. If for example, people are searching for the returns policy, then you can make provisions to make it easily accessible.

## Responsive design

The search form itself is really simple: a single text box (`input type="search"`) and a submit button. The search form is usually positioned within the header. The purpose of placing it at the top is not only for cognition, but interaction. Like navigation, we want to make search readily accessible to both mouse and keyboard users. Putting such important and commonly used features is counter intuitive.

The problem with placing it inside the header is that the header is premium screen real estate. That is, there isn't much room available and it's highly sought after. The more that we put into the header, the more the main page content is pushed down the page. On mobile, of course, space is even more limited.

As we've talked about previously, designers are often seduced by novel space-saving design patterns such as the hamburger menu[^2], but hiding content should always be a last resort. On desktop, this isn't a problem as there's ample space in which to show the form entirely. On mobile, though, we're going to have to try and save space somehow.

One space-saving tactic is to (visually) hide the label, replacing it with placeholder text. But, we've talked about how problematic this is in ‘A registration form’. Crucially though, removing the label doens't save that much space anyway. Even without a one, it's still going to be hard to squeeze the rest of the form into the header.

Really, for mobile, we're going to have to toggle the search form's visibility using CSS and Javascript.  Note: The CSS and Javascript for creating a toggler just for small screens is set out in ‘An inbox’ for the action menu.

With this decision in place, accomodating a visual label and optional hint is easy. This way, the toggle button can be placed inside the header with the search form itself, directly below.

When the toggle button is clicked, the search form is shown and focus set to the search box. Not only does this save users an extra click, but on focus, screen readers will announce the label.

## Summary

In this chapter, we reused patterns from previous chapters to design a responsive and togglable search form that crucially saves space on mobile. And we ensured the quality of the interface is married to search algorithm itself: that whatever the user searches for is surfaced for users. Together, this creates the best user experience.

### Things to avoid

- Only searching the database for content.
- Using placeholders as a label replacement.

## Footnotes

[^1]: https://medium.com/uie-brain-sparks/content-and-design-are-inseparable-work-partners-5e1450ac5bba
[^2]: http://jamesarcher.me/hamburger-menu