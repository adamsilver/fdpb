# A filter form

Filters go with search, like ying goes with yang. There's only so much you can do with a free form search field. In the introduction to ‘A Search Form’, you'll recall the conversation I had with my Mum when I was trying to find ‘a t-shirt’. ‘A t-shirt’ being hardly specific, meant Mum had to ask me ‘is it for football or tennis?’ because she didn't have enough information to go on to give me a useful answer. This question is a filter.

On the web, a search can yield thousands, or even, millions of results depending on the content available. As humans, we can't juggle more than 7 discrete pieces of data at a time, so being able to narrow the breadth of results easily to get to ‘find the t-shirt’ is important. A filter, also referred to as *facet* or *guided navigation* is important because it offers users a way to find stuff using their own mental model.

In ‘Designing for Faceted Search’, Stephanie Lemieux has an excellent example of this. She says:

> Think of a cookbook: authors have to organize the recipes in one way only-by course or by main ingredient-and users have to work with whatever choice of organizing principle that has been made, regardless of how that fits their particular style of searching. An online recipe site using faceted search can allow users to decide how they’d like to navigate to a specific recipe [by course type, cuisine or cooking method for example].

In this chapter we're going to look at how links, radio buttons and checkboxes are used to design different types of filters and the impact they have on the experience for users. Then we'll look at some potential enhancements and some important considerations for designing for small screens.

## Types of filters

Links, radio buttons and checkboxes can all be used to design a filter.

Links, which admittedly, are nothing to do with forms, let users filter a list of a results via the querystring. Clicking a link under a particular facet, makes all the other options disappear. For example, if I'm searching for a car and I select ‘Honda’ then all the other makes disappear, because I'm only able to select one. Using links this way, ensures that results will always be returned, because the link will only be shown, if this is so.

This may not always be the desired behaviour. Perhaps you want to narrow down your seach based on two brands in particular such as Honda and Audi. In this case, offering links is not going to help because it's going to mean users have to click Honda, wait for the page to refresh, then select Audi.

A form with radio buttons and checkboxes can help here. By offering users a familiar set of form controls we can give them a way to select multiple options at once. Additionally, we can let users select many different options across different facets at the same time, such as brand and colour.

Unlike links, being able to select multiple options within and across facets could return no results depending on the facet in question. If I was to choose Honda and Audi, results should be guaranteed, just like if I was to only select Honda. This is because I would expect a range of Honda and Audi cars.

Conversely, if I selected a Honda but also specified just red ones, then there may not be any red Hondas available. This is the downside in letting users select more than one at a time. What you gain in power and performance, you lose in the overarching experience. Of course, users should be easily able to remove a facet in reverse priority order until results appear.

Beware not to go crazy with facets. Overloading users with hundreds of facets makes the job harder, not easier. If you're not sure which facets to show, conduct thorough user research and check analytics. Include the most popular ones and show them in that order, and kill off the others. Show additional facets as they become relevant.

Finally, remember that a broader and shallower taxonomy creates a better experience, without sending users too far down a funnel.

## Material honesty (again)

We've talked about material honesty several times in the book. This shows both its importance and prevelance to the world of interface design. As a reminder, Jeremy Keith says that *one material should not be used as a substitute for another. Otherwise the end result is deceptive.*

In the case of filters, often links are made to look like radio buttons or checkboxes (using CSS). The problem, of course, is that a radio button is not a link. When a link is clicked, it will immediately make a request and take the user to the page.

![](.)

As the design of form controls vary widely across browsers and operating systems, these CSS background images always differ slightly creating an inconsistent and unprofessional visual tone. But there's more to this than just vaneer. If a user sees a radio button, conventionally speaking, they'll know that they can move through the choices before submitting it. That's just how forms work. It will be confusing if clicking a radio button, immediately submits the form.

> ‘Use checkboxes and radio buttons only to change settings, not as action buttons’ -NN

Similarly, checkboxes and radio buttons are made to behave like links. That is, with a little Javascript, clicking a radio button can be made to submit the form instantly. This is particularly tricky for keyboard (including screen reader) users, because they'll struggle to move through the options and will actually stop users selecting options within multiple facets.

Breaking widely understood conventions that relate to links and form controls can seriously harm the resulting user experience. By sticking to conventions, users who encounter these components both visually and audibly, don't have to think.

## Enhancing with AJAX

AJAX is often used to patch over websites that have bad performance, normally because we cram so much shit on a page. But AJAX doesn't negate a server round trip and unfortunately it comes with some trade-offs:

- Adds weight to original request
- Engineers away chunking
- Removes the browser's loading indicator
- Needs to design loading and success states
- More code, more testing, more chance of problems
- Increases the chance of memory leaks
- Drains battery on mobile devices

AJAX is useful when you want to make a small update to the screen with the majority of the page staying the same. When filtering, the main feature of the page itself changes. Both the results and the filter itself.

In this case, not only does it defeat the point, but it may well make the page slower. See my article.

## Small screen problems

- Refer to Dave House conversation

---

## Summary


## Links

[!https://asset.uie.com/articles/img/faceted_search/BuzzillionsKMWorld.jpg](from UIE)
https://www.nngroup.com/articles/checkboxes-vs-radio-buttons/
[Facet search]: https://articles.uie.com/faceted_search/
[Don't make links look like checkbox/radios]: https://medium.com/@z_rose/oh-boy-form-design-df6a71e39d60)
- Don't make links look like checkbox/radios (https://medium.com/@z_rose/oh-boy-form-design-df6a71e39d60)
- select box problem without submit button?
- https://github.com/adamsilver/f/issues/40
- infinite scroll/show more/pagination perhaps