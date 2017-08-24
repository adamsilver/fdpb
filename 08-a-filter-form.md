# A filter form

Filters go with search, like ying goes with yang. There's only so much you can do with a free form search field. In the introduction to ‘A Search Form’, you'll recall the conversation I had with my Mum when I was trying to find ‘a t-shirt’. ‘A t-shirt’ being hardly specific, meant Mum had to ask me ‘is it for football or tennis?’ because she didn't have enough information to go on to give me a useful answer. This question is a filter.

On the web, a search can yield thousands, or even, millions of results depending on the content available. As humans, we can't juggle more than 7 discrete pieces of data at a time, so being able to narrow the breadth of results easily to get to ‘find the t-shirt’ is important. A filter, also referred to as *facet* or *guided navigation* is important because it offers users a way to find stuff using their own mental model.

In ‘Designing for Faceted Search’, Stephanie Lemieux has an excellent example of this. She says:

> Think of a cookbook: authors have to organize the recipes in one way only-by course or by main ingredient-and users have to work with whatever choice of organizing principle that has been made, regardless of how that fits their particular style of searching. An online recipe site using faceted search can allow users to decide how they’d like to navigate to a specific recipe [by course type, cuisine or cooking method for example].

In this chapter we're going to look at how links, radio buttons and checkboxes are used to design different types of filters and the impact they have on the experience for users. Then we'll look at some potential enhancements and some important considerations for designing for small screens.

## Types of filters

Links, radio buttons and checkboxes can all be used to design a filter.

Links, let users filter a list of a results via the querystring. Admittedly, links are nothing to do with forms, but they are often used interchangeably with form controls so talking about them in context here is important. Here's an example of a link filter used to find a car.

![](.)

Each time a user chooses a filter, such as Honda, that option will disappear from the list. The advantage of using links in this manner, is that every click is guaranteed to return results. This is becaue the link will only appear when there are results behind it.

The disadvantage of using links is that each click requires a page refresh. On its own this is not a problem at all. If pages are simple and optimised correctly, a page refresh will do users no harm. However, if the user wants to select two or more filters, waiting for two or three requests is far from ideal.

If the user wants to select Honda and Audi, for example, a form with checkboxes is helpful. By offering users a familiar set of form controls, we hand over power and control to select as many options as they want at once, both across and within different facets, such as brand and colour.

Unlike links, offering multiple options at once may return no results, depending on the fact. If, for example, the user selects Honda for brand and Red for colour, they may get frustrated when no results appear. Of course, if users get too specific, we can offer them a way to remove a filter in reverse priority order, until results appear.

Beware not to go crazy with facets. Overloading users with hundreds of facets makes the job harder, not easier. If you're not sure which facets to show, research it and check analytics. Include the most popular ones and show them in that order. Kill off the other or reveal them as they become relevant as the user drills down.

A broad and shallow taxonomy creates a better experience without sending users too far down a funnel.

## Material honesty (again)

We've talked about material honesty several times in the book. This shows both its importance and prevelance to the world of interface design. As a reminder, Jeremy Keith says that *one material should not be used as a substitute for another. Otherwise the end result is deceptive.*

In the case of filters, often links are made to look like radio buttons or checkboxes (using CSS). The problem, of course, is that a radio button is not a link. When a link is clicked, it will immediately make a request and take the user to the page.

![](.)

As the design of form controls vary widely across browsers and operating systems, these CSS background images always differ slightly creating an inconsistent and unprofessional visual tone. But there's more to this than just vaneer. If a user sees a radio button, conventionally speaking, they'll know that they can move through the choices before submitting it. That's just how forms work. It will be confusing if clicking a radio button, immediately submits the form.

> ‘Use checkboxes and radio buttons only to change settings, not as action buttons’ -NN

Similarly, checkboxes and radio buttons are made to behave like links. That is, with a little Javascript, clicking a radio button can be made to submit the form instantly. This is particularly tricky for keyboard (including screen reader) users, because they'll struggle to move through the options and will actually stop users selecting options within multiple facets.

Breaking widely understood conventions that relate to links and form controls can seriously harm the resulting user experience. By sticking to conventions, users who encounter these components both visually and audibly, don't have to think.

## Using AJAX

Earlier we discussed the pros and cons of using links and form controls. Using links may cause many refreshes. Using form controls increases the chance of seeing no results at all.

For keyboard and screen reader users, having a page refresh means having to wade through all the page information again, such as header and navigation before getting back to the filter or the results.

We can instead use AJAX to avoid a page refresh. For example, clicking a radio button, will instantly submit the form with AJAX. When the request finishes, the page is updated and the focus is in the same place. This also avoids the chance of seeing no results, because as the user selects one filter, the results and the filter component itself is updated.

Also, many applications have designed filters in a materially dishonest and deceptive manner: that is, clicking a radio button immediately submits the form. Therefore, as users have become acclimatised to this change in convention, I'm sad to say that it may not always be obvious to users that they have to submit their choices. In this case, AJAX can also help here because it removes the need for the button.

You'd be forgiven then, for thinking this is a must-have, totally beneficial enhancement. Unfortunately, using AJAX like this goes against principle 4, to *give users control*. By removing an explicit submission action, it's possible that triggering multiple AJAX requests will cause an unexpected and confusing experience for users.

Additionally, keyboard users who are operating the filter must use their arrow keys to move through radio buttons. Each arrow keypress, not only focuses the option but selects it too. If there are 4 radio buttons, and the user wants to select the last one, this will cause 4 AJAX requests. This adds increased load on the server. But more importantly, this will eat users' data allowance and cause battery drain.

That's not all, by using AJAX we would need certain provisions such as a live region that announces the loading states of the page. When the user selects an option, the live region might be populated with ‘Loading results, please wait’. Then when the results come back, it will be populated with ‘212 results loaded.’. Having this be read out 4 times, when once could have done it is headache inducing.

Despite what you may have heard, AJAX isn't necessarily better or faster than a standard page refresh[^fasthacksjake]. This is for a number of reasons. First, it rquires more code to be sent initially. Second, it engineers away progressive rendering (aka chunking) and an indication of loading states, both of which the browser does for free.

AJAX is more suited and beneficial when making updates to small parts of the page. Filters, however, involve updating the vast majority of the page, making AJAX somewhat counterproductive in this case. With all of that put on the table, only thorough and diverse user research will show what works best.

## Responsive design considersations

- Refer to Dave House conversation

---

## Summary


## Links

[!https://asset.uie.com/articles/img/faceted_search/BuzzillionsKMWorld.jpg](from UIE)
https://www.nngroup.com/articles/checkboxes-vs-radio-buttons/
[Facet search]: https://articles.uie.com/faceted_search/
[Don't make links look like checkbox/radios]: https://medium.com/@z_rose/oh-boy-form-design-df6a71e39d60)
- select box problem without submit button

## Dave House notes

- Use an adaptive approach. For big screens, display the filters on the left to aid discovery and perhaps do AJAX.
- On small screens (e.g. Gumtree), take them down a mini flow (pages or otherwise). Click filter > choose filters > get results. This is because on mobile, updating the filters at the top of the page means not being able to see the refreshed content. Could move focus, but doing this sucks (maybe they want to select another filter before moving).
- Multi-select: Users didn't expect to have to submit, or did not notice the submit button. They expected AJAX style loading. Filters were being selected but never submitted. Got a lot of feedback “You’re filters are broken”. Tried moving the apply button [to the top], making it sticky…loads of things that didn’t really make a difference. But Gumtree has tons of ads getting in the way cognitively.
- Multi-select: Some users would over filter and end up with 0 results. For this we played around with the “Apply” button displaying the amount of results adding these filters will return so they have an informed choice if they want to apply the filters.
- Multi-select (mobile): Users were able to make all the refinements they wanted without the fear of getting 0 results (or having to go back and forth multiple times). It was great to see users keep adding filters until they felt like they had a manageable amount of results, and filter more than they had previously. (this was a site with 2,000,000 listings)
- Don't have scrollable panels for facets. Scroll inside scroll is frustrating on mobile. Users get stuck inside the inner scroll and can't scroll the actual page.
