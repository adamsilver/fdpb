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

## Interface Design

The form itself is simple enough and contains just a label, search input and a submit button. What's tricky is that it's usually placed within the header which is not only for cognition, but interaction. Like navigation, we want to make search readily accessible to both mouse and keyboard users. Putting such a commonly-used feature somewhere else would be counterintuitive.

![Standalone search form](.)

### There's No Room

The problem with placing it inside the header is that the header is premium screen real estate. That is, there isn't much room available and it's highly sought after. The more that we put into the header, the more the main content is pushed down the page. On mobile, of course, there's even less space.

As noted previously, we're often seduced by novel, space-saving techniques such as the hamburger menu[^2], but hiding content should always be a last resort. On desktop, the issue of space, isn't much of, well, an issue, as there's usually plenty of room. On mobile though, we're going to have to think a little harder.

To reduce the amount of vertical space, we can place the submit button beside the search box. As discussed in the first chapter, the best place to put it is underneath the last field. But this is a bit of a special case.

There are other techniques employed to save space, and to keep the content high up the page - or dare I say, above the fold.

#### Hiding The Label

The first space-saving technique is to hide the label. You might consider using a placeholder to supplant the label but we've already talked about why you shouldn't do this in chapter 1, “A Registration Form”. As noted in the same discussion, using a placeholder in addition to a label is certainly less troublesome, but not trouble free.

You could argue that a visible label is unnecessary because the submit button's label is enough for sighted users. But, you should still include a label for screen reader users, because they shouldn't have to skip ahead to the button in the hope that its label provides a clue.

```HTML
Hidden label code
```

With that said, if your search mechanism doesn't return everything, then you might need a more descriptive label. For example, if the search only retrieves products, then the label should read “Search products” (or similar). Hiding the label in this case would exclude sighted users.

In any case, removing the label doesn't save all that much space. And even without a label, it's still going to be hard to fit the search box and submit button without sacrificing the affordance and usability of it.

#### Hiding The Button

The second approach is to (visually) hide the button. Obviously, if you hide the button and the label, much of the previous advice should be discarded. That's not all though. Without a submit button, it's not clear how a user can submit their search term. Not all users are aware of implicit submission, nor should they have to be.

Some fancy sites use AJAX to search as the user types but this is unconventional, jarring and eats up people's data allowance. It also doesn't eradicate the need for a button - if the predicted results are no good, the user can't proceed with a full search.

Medium is an example of a site that omits the submit button. To give the form affordance, they use a magnifying glass icon before the search box. But they could just as easily place it after and combine it with a submit button. This way, the form would work conventionally.

![Medium no button](.)

*(Note: Some sites omit the `<form>` element because they're rendering and routing with Javascript. But in doing so users aren't able to press <kbd>Enter</kbd> to submit the form. Even users who primarily use the mouse, may choose to submit by pressing <kbd>Enter</kbd> as it's easier.)*

#### Toggle The Form

The last approach is to toggle the form's visibility using a button. It's much easier to accomodate a button inside the header. The form might be revealed as an overlay, or immediately underneath the header. No matter which approach you take, including a visual label, optional hint and submit button is no longer a problem.

```HTML
<header>
  <button type="button" aria-haspopup="true" aria-expanded="false">
</header>
<form class="search hidden">
	...
</form>
```

In this example, the search form is placed directly after the header. Clicking the button should show the form, and move focus to the search box, saving uses an unnecessary click. At the same time `aria-expanded` should be set to `true` so that the state is reflected for screen reader users.

![Click button -> move focus](.)

Most users at this point will search and submit the form. But we can accomodate users who change their mind by letting them press <kbd>Escape</kbd>. In this case, the search form should be hidden again and focus returned to the button.

```JS
Code here
```

## Summary

In this chapter, we combined 2 vital concepts with regards to search. First, we looked at how important it is to give users what they searched for, that's anything in the site, not just products. Second, we looked at how the page can accomodate the search form as part of the header, where it makes most sense to reside. By marrying these two concepts together, users should be able to find what they're looking for with minimal fuss.

### Checklist

- Search everything
- Include a visible label and submit button
- Don't sacrifice affordance just to fit the form inside the header

## Footnotes

[^1]: https://medium.com/uie-brain-sparks/content-and-design-are-inseparable-work-partners-5e1450ac5bba
[^2]: http://jamesarcher.me/hamburger-menu

## After search perhaps

- Maintain entered text
- Display count
- ‎Let them sort
- ‎pagination not show more. When show more works and doesn't.
- ‎let them filter (next chapter)

 *(Note: toggling the behaviour for small and large viewports is set out in the previous chapter, “An Inbox”, for the action menu.)*

 - https://www.gov.uk/government/organisations/hm-revenue-customs
- https://www.harrods.com/en-gb
