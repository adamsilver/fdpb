# A Search Form

I'm an organised person. Even as a boy, I remember always having ‘a place’ for things. To be fair, I've always been minimalist too. Organising things when you only own a few things is easy. So I guess, it's unsurprising that I rarely lost things. On the odd occasion that I did, I just shouted in the general direction of the resident search engine: ‘Where's my...,’ and I'd have my answer.

By search engine, I mean Mum! Mum knew where everything was, not just my stuff—everyones. This was one of her many qualities. She didn't just know where stuff was, she knew the answer to everything (at least that's how I remember it). If I had grown up with search engines, I might have nicknamed her Google.

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

Notes:

- As noted in chapter 3, “Book a Flight”, the search input (`<input type="search">`) lets users clear the field more conveniently than a standard text box by clicking the “X” or by pressing <kbd>Escape</kbd>.
- The search form has a search landmark role: `role="search"`. Like other landmarks, this means it will be listed as a shortcut in most screen readers. The extra `<div>` is necessary because putting the landmark attribute directly on the `<form>` would override its semantics[^]

## There's No Room

Normally, search is placed within the header. Like navigation, this makes it easily discoverable and quick to access. Putting such an integral feature elsewhere on the page would be counterintuitive and unconventional.

The challenge, of course, is that it's hard to fit the search form inside the header along with everything else. The header is premium screen real estate. That is, there isn't much room available and it's highly sought after. The more that we put into the header, the more the main content is pushed down the page.

As noted in past chapters, we're often seduced by novel, space-saving techniques such as the hamburger menu[^2], but hiding content should always be a last resort. On desktop, the issue of space, isn't much of, well, an issue—there's usually plenty of room. On mobile though, we're going to have to think a bit more. There's only so much space available to play with.

Let's look at some ways to reduce the size of the search form, so that it might fit inside the header more easily. As part of this we'll look at various problems and considerations we need to think about with each technique.

### Place Submit Button Inline

As explained in chapter 1, “A Registration Form”, the best place for the submit button is directly below last field. But one way to reduce the amount of vertical space the search form takes up, is to place the submit button next to the field. 

![Search button placement](./images/06/vertical-space.png)

As it's a bit of a special case, this is okay if the search form consists of just one form field. But some search forms offer more than one field. In this case, the submit button should go directly below the last field as usual.

### Hiding The Label

Another space-saving technique is to hide the label. As part of this, you might consider using the placeholder attribute to supplant the label but this is problematic for a number of reasons. See chapter 1, “A Registration Form” for an in-depth run down.

We could forgo a visible label altogether because arguably the submit button acts as a quasi label for sighted users. But, you should still include a label for screen reader users because they shouldn't have to skip ahead to the button in the hope that its label provides a clue.

```HTML
<div class="field">
  <label for="search" class="visually-hidden">
    <span class="field-label">Search</span>
  </label>
  <input type="search" id="search" name="search">
</div>
```

*(Note: The CSS for the visually hidden class is set out in “Checkout”.)*

If search only retrieves products, for example, then the button would be better labelled “Search products”. This way users know what they can and can't search for. As noted above, users should be able to search everything, but admittedly technological and resource limitations may come into play.

Also, if you recall the hint and error patterns from “A Registration Form”, the text is injected into the `<label>`. If you need to show this information, you'll have to come up with a new and different solution which seems unnecessary.

In any case, hiding the label doesn't usually save enough space to fit the form inside the header anyway, especially in smaller viewports.

### Hiding The Button

We might also consider hiding the button, but this has several pitfalls. First, without a button you can't use it as a quasi label; the label would need to be reinstated taking up room again. 

Second, the button is a fundamental part of a form—without it, it's not clear how users are meant to perform the search. While *we* may be aware that forms can be submitted implicitly (by pressing <kbd>Enter</kbd>), not all users are. See chapter 5, “An Inbox” for more information about implicit submission.

Third, if your search form contains more than one field, omitting the submit button stops implicit submission from working. Fortunately, if your search form consists of a single field, implicit submission will still work without a button.

Interestingly, many of the sites that omit the submit button, normally find room to include a magnifying glass icon to signify its otherwise hidden affordance. In this case, they may as well place the icon inside a submit button solving both problems at the same time.

![Medium no button](./images/06/medium-search-no-button.png)

Caption: Medium puts a magnifying glass icon before the search box. If they combine the icon with a submit button placed after the search box, the form would work conventionally. That is, keyboard users, for example, would expect the search box to come before the submit button.

*(Note: if you do decide to hide the button using the `visually-hidden` class, remember the button would still be focusable. This means, sighted screen reader users[^adrian], for example, will find this problematic. You can fix this by adding the `tabindex="-1"` attribute.)*

## Toggling The Form's Visibility

Even if removing the label and submit button didn't degrade the usability of the form, it still wouldn't really solve the space problem enough to fit the form comfortably within the header.

Instead of messing around with pixels to this extent, we can toggle the entire form's visibility by letting users click a button. Finding room in the header for a button is relatively straightforward. And including a visible label, a hint (if needed) and a submit button is an easy task.

![Focus moved](./images/06/collapsible-search-form.png)

### The Basic Mark-Up

```HTML
<header>
  ...
</header>
<div role="search">...</div>
```

The basic page consists of a header containing the logo and navigation etc. The search form is positioned underneath the header.

### The Enhanced Mark-Up

When JavaScript is available, the mark-up is enhanced like this:

```HTML
<header>
  ...
  <button type="button" aria-haspopup="true" aria-expanded="false">Search form</button>
</header>
<div role="search" class="hidden">...</div>
```

Notes:

- A toggle button is injected into the header.
- The search form is hidden using the `hidden` class as introduced in chapter 1.
- The `aria-haspopup` attribute indicates that the button reveals a part of the interface (the search form). It acts as warning that, when pressed, the focus will be moved to the search form.
- The `aria-expanded` attribute tells screen reader users whether the search form is currently expanded or collapsed by toggling between `true` and `false` values using a simple script.

### A Small Script

```JS
function SearchForm() {
  this.header = $('header');
  this.form = $('.searchForm');
  this.form.addClass('hidden');
  this.button = $('<button type="button" aria-haspopup="true" aria-expanded="false"><img src="/public/img/magnifying-glass.png" width="20" height="20" alt="Search products"></button>');
  this.button.on('click', $.proxy(this, 'onButtonClick'));
  this.header.append(this.button);
}

SearchForm.prototype.onButtonClick = function() {
  if(this.button.attr('aria-expanded') == 'false') {
    this.button.attr('aria-expanded', 'true');
    this.form.removeClass('hidden');
    this.form.find('input').first().focus();
  } else {
    this.form.addClass('hidden');
    this.button.attr('aria-expanded', 'false');
  }
};
```

Notes:

- The constructor is responsible for enhancing the HTML and listening to the button's click event.
- When the button is clicked, the script checks the `aria-expanded` attribute to see if the form is currently expanded or collapsed. 
- If the form is collapsed, then the `aria-expanded` attribute is set to `true` and the `hidden` class is removed to reveal the form. Finally, the first input is focused which saves users an unnecessary extra click.
- If the form is expanded, the  `aria-expanded` attribute is set to `false` and the form is hidden by adding the class of `hidden` to the form.

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

## Demos

- Search form: http://nostyle.herokuapp.com/examples/search-form

## Footnotes

[^content and design]: https://medium.com/uie-brain-sparks/content-and-design-are-inseparable-work-partners-5e1450ac5bba
[^search role]: http://adrianroselli.com/2015/08/where-to-put-your-search-role.html
[^hamburger]: http://jamesarcher.me/hamburger-menu
[^sighted screen reader users]: http://adrianroselli.com/2017/02/not-all-screen-reader-users-are-blind.html
[^infinite scrolling]: https://adamsilver.io/articles/infinite-scrolling-is-probably-a-bad-idea/