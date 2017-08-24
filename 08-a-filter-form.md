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

### Material dishonesty (again)

In *Resilient Web Design* Jeremy Keith talks about the idea of Material Honesty. He says *one material should not be used as a substitute for another. Otherwise the end result is deceptive.* In this case we want our links to look like links as that's what they are.

The problem is that some designers make links look like radio buttons. They'll use background images to make it appear as though the user is clicking a radio button. The problem is that it's not a radio button and doesn't have the functionality of a radio button (clicking it immediately navigates, without having to submit). And they due to the custom imagery, the radio buttons always look a little off (or completely different) to the ones provided be browsers natively.

### Separate submission

‘Use checkboxes and radio buttons only to change settings, not as action buttons’ https://www.nngroup.com/articles/checkboxes-vs-radio-buttons/

We've talked about this before[^?] but we're going to want to stay true to familar conventions by ensuring submission is separate from selection. Like select boxes, we shouldn't submit a form when the user checks a radio button for example.

A good reason for this is that the user may want to choose several facets before submission. For example, if they're searching for a pair of shoes they may want to select the size and colour at the same time. Doing this with two diseparate interactions is wasteful of their time and puts unnecessary load on the server.

## Enhancing with AJAX

## Small screen problems

- Refer to Dave House conversation

---

### Do you need to refresh results with AJAX?

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

## Links

[!https://asset.uie.com/articles/img/faceted_search/BuzzillionsKMWorld.jpg](from UIE)

[Facet search]: https://articles.uie.com/faceted_search/
[Don't make links look like checkbox/radios]: https://medium.com/@z_rose/oh-boy-form-design-df6a71e39d60)
- Don't make links look like checkbox/radios (https://medium.com/@z_rose/oh-boy-form-design-df6a71e39d60)
- select box problem without submit button?
- https://github.com/adamsilver/f/issues/40
- infinite scroll/show more/pagination perhaps