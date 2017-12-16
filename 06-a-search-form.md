# A Search Form

I'm an organised person. Even as a boy, I remember always having ‘a place’ for things. To be fair, I've always been minimalist too. Organising things when you only own a few things is easy. So I guess, it's unsurprising that I rarely lost things. On the odd occasion that I did, I just shouted in the general direction of the resident search engine: ‘Where's my...,’ and I'd have my answer.

By search engine, I mean Mum! Mum knew where everything was, not just my stuff — everyones. This was one of her many qualities. She didn't just know where stuff was, she knew the answer to everything (at least that's how I remember it). If I had grown up with search engines, I might have nicknamed her Google.

As I've gotten older and become a husband and father, my life is richer but also less minimalist. Even if I meticulously organise all our belongings, it's still hard to remember where it all is. Worst case scenario: I have to peruse each cupboard one-by-one hoping that the thing hasn't been lost which is time-consuming.

In this chapter, we're going to design a responsive search form. Like Mum, we'll want it to be readily-available and on-hand to answer *any* question users have. To make this happen, there are some crucial things to consider.

## Search Everything

When I recounted my childhood conversation with Mum, I was not only able to ask her where my stuff was, but really, I was able to ask her anything. When designing a global search form, users should be able to do the same thing. Too often, users can only find stuff that lives in the database. For example, on Amazon, the search will only return products. And on Youtube, the search will only return videos.

In “Content and Design Are Inseparable Work Partners”[^1], Jared Spool explains that “content is the thing the user wants right now.” He recounts a story from user research where someone was trying to buy a purse. 

The lady was happy enough to buy the purse on the proviso that she could return it. But the returns policy wasn't on the product page, or in the FAQ. In the end, she tries typing “Refund policy” into the search box but it didn't return any results. That was the end of the research session.

The returns policy isn't a product. Nor does it reside in the database. But this is what she wanted. We often hear how content is king, but the design of the site lets the content down. Wherever possible search should search everything. And if it doesn't, the label should be explicit. If, for example, the search only returns products then make that clear within the interface.

![Search label](./images/06/search-label.png)

*(Tip: use analytics to track what users are searching for. If the most popular searches are retrieving empty results, make provisions to improve the experience based on data.)*

## The Basic Form

The search form is simple enough and contains just three elements: the label, search input and submit button.

![Search form](./images/06/search-form.png)

```HTML
<div role="search">
  <form>
    <div class="field">
      <label for="search">
        <span class="field-label">Search</span>
      </label>
      <input type="search" id="search" name="search">
    </div>
    <input type="submit" value="Search">
  </form>
</div>
```

The search form's container has a `role="search"` attribute. This role is a landmark role, so it's navigable by shortcuts and aggregated landmark lists in most screen readers. The extra `div` is necessary because putting the landmark attribute directly on the `<form>` would override its semantics[^adrian].

Normally, search is placed within the header, which like navigation, makes it easily discoverable and quick to access. In fact, putting such an integral feature somewhere else on the interface would be counterintuitive.

## There's No Room

The challenge is that it's hard to fit the search form inside the header along with everything else. The header is premium screen real estate. That is, there isn't much room available and it's highly sought after. The more that we put into the header, the more the main content is pushed down the page. On mobile, of course, there's even less space.

As noted previously, we're often seduced by novel, space-saving techniques such as the hamburger menu[^2], but hiding content should always be a last resort. On desktop, the issue of space, isn't much of, well, an issue — there's usually plenty of room. On mobile though, we're going to have to think a bit more.

As discussed in “A Registration Form”, the best place for the submit button is directly below last field. But one way to reduce the amount of vertical space the search form takes up, is to place the submit button next to the field. This is okay as it's a bit of a special case.

![Search button placement](./images/06/vertical-space.png)

Let's look at some additional ways to reduce the size of the search form and make it fit inside the header more easily.

### Hiding The Label

The first space-saving technique is to hide the label. You might consider using a placeholder to supplant the label but we've already talked about why you shouldn't do this in chapter 1, “A Registration Form”.

With that said, you could argue that a visible label is unnecessary because the submit button acts as a quasi label for sighted users. But, you should still include a label for screen reader users, because they shouldn't have to skip ahead to the button in the hope that its label provides a clue.

```HTML
<div class="field">
  <label for="search" class="visually-hidden">
    <span class="field-label">Search</span>
  </label>
  <input type="search" id="search" name="search">
</div>
```

*(Note: The CSS for the visually-hidden class is set out in “Book A Flight”.)*

If search only retrieves products, for example, then the button's label would be better as “Search products” (or similar). 

Also, if you recall the patterns from “A Registration Form”, the hint and error text is injected into the `<label>`. If they're needed, you'll have to rethink how this works when there is no visible label to work with.

In any case, removing the label doesn't save enough space to make the search form fit inside the header on mobile anyway — certainly not without sacrificing the user experience.

### Hiding The Button

The second approach is to (visually) hide the button. However, this has several pitfalls.

First, without a button you can't use it as a quasi label, and would need to reinstate the actual label. Second, the button is a fundamental part of a form — without one, it's not clear how users are meant to perform the search. While we may be aware that pressing <kbd>Enter</kbd> submits the form, not everyone is.

Being able to submit the form by pressing <kbd>Enter</kbd> is called implicit submission and it's something we first discussed in chapter 5, “An Inbox”. But implicit submission can break if you remove the submit button. But this is only the case if the form has more than one field. As the search form has a single field, implicit submission would fortunately still work.

*(Note: if you do decide to hide the button using the `visually-hidden` class, remember the button would still be focusable. This means, sighted screen reader users, for example, will find this problematic. You can fix this by adding the `tabindex="-1"` attribute.)*

Some sites use AJAX to search as the user types but this can be jarring and it eats up people's data allowance. It also doesn't make the button any less useful. That is, if the predicted results aren't helpful, users should still be able to proceed with a full search by submitting the form and they should know exactly how to do that.

Funnily enough, the same sites that omit the submit button, normally find at least some space to include a magnifying glass icon to signify its otherwise hidden affordance. In this case, they may as well place the icon inside a submit button solving both problems at the same time.

![Medium no button](./images/06/medium-search-no-button.png)

Caption: Medium puts a magnifying glass icon before the search box. If they combine the icon with a submit button placed after the search box, the form would work conventionally. That is, keyboard users, for example, would expect the search box to come before the submit button.

### A Note About Removing the Form Element

Some sites omit the `<form>` element because they're rendering and routing on the client using Javascript. But this also stops users from being able to press <kbd>Enter</kbd> to submit the form.

Remember, even users who primarily use the mouse, may choose to submit the form by pressing <kbd>Enter</kbd> as it's easier.

### Toggling The Form's Visibility

The last approach is to toggle the form's visibility using a button. Finding room in the header just for a button is relatively straightforward. The form might be revealed as an overlay or immediately underneath the header. Either way, including a visible label, a hint (if you need one) and a submit button becomes a simple task.

#### Hiding The Form

First we need to hide the form and inject the toggle button into the header:

```HTML
<header>
	<button type="button" aria-haspopup="true" aria-expanded="false">...</button>
</header>
<div role="search" class="hidden">...</div>
```

*(Note: this uses the same pattern as the menu from chapter 5, “An Inbox”. You can read about the `aria-haspopup` and `aria-expanded` attributes there. The `hidden` class is explained in chapter 1, “A Registration Form”.)*

#### Clicking The Toggle Button

When the user clicks the button, the form will be revealed by removing the `hidden` class. Simultaneously, the focus is moved to the search box, which saves users an unnecessary extra click (or <kbd>Tab</kbd>).

![Focus moved](./images/06/collapsible-search-form.png)

We also need to update the state of the button by toggle the `aria-expanded` attribute to `true` so that it's reflected to screen reader users. Clicking the button for a second time, will hide the form and set `aria-expanded` back to `false`.

The full code is available on the accompanying Github repository[^].

## Displaying Search Results

Displaying search results is somewhat out of scope for a book about form design, but lets run through some important details quickly now:

- **Maintain search text**. When the user arrives at the search page, what they typed should be persisted. This way users can make tweaks without having to retype the entire query.
- **Display result count**. Tell users how many results have been returned. A simple approach would be to update the page's `<title>` text to read “Search results for [search term]” or similar. This way, users can decide what their next action is. For example, if there are many results, they may decide to filter them (more on this in the next chapter).
- **Let users sort**. Depending on the thing that's being searched it's often useful to let users sort by relevance, popularity, recency.
- **Don't employ infinite scrolling by default**. It's an inclusive design anti-pattern with several usability issues[^]. This leaves “Show more” or standard pagination. Show more is more appropriate for sites that have a lot of user-generated content where the location of the result is not important. Pagination is more appropriate for ecommerce sites where users are looking for a specific item - not just browsing aimlessly for entertainment.

## Summary

In this chapter, we started by looking at how important it is to give users what they searched for - not just products, or articles, but anything the site contains. We then went onto look at the interface. 

Specifically, how we can accomodate a search form as part of the header so that it's readily accessible from every page in the site. We also enhanced the experience for screen reader users by using the `role="search"` landmark.

Marrying the usability of the interface with the quality of the search engine gives users the best experience.

## Footnotes

[^1]: https://medium.com/uie-brain-sparks/content-and-design-are-inseparable-work-partners-5e1450ac5bba
[^2]: http://jamesarcher.me/hamburger-menu
[^]: http://adrianroselli.com/2015/08/where-to-put-your-search-role.html