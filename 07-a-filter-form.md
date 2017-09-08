# A filter form

A filter widget, sometimes referred to as facet navigation or guided navigation, lets users refine a large set of search results. Were filters straightforward, I could have folded this pattern into the last one, but there are a number of challenges and concerns to address: the type of element to use; keeping to conventions; potential AJAX enhancements; how to make it work just as well on small screens as it does on large ones. All of these things need to be taken into account.

First of all, though, it needs to be said that&mdash;like anything any other feature or form field in the book&mdash;if you don't need a filter, don't include one. They're only useful if a search returns a large amount of items to wade through. Like Google, users are not really willing to click through beyond the third or fourth page.

In the introduction to ‘A Search Form’, you'll recall the somewhat typical conversation I used to have with my Mum when I asked her where my ‘black top’ was. The vagueness of that question meant she couldn't answer accurately. To find out exactly which one I was after she asked me questions like ‘Is it a football top’. In technical terms this question is a type of filter on a large set of results.

On the web, a search can yield thousands, or even, millions of results depending on the content available. As humans, we can't juggle more than approximately seven things at one time[^1], so being able to narrow them down is crucial. The ability to filter not only offers an additional dimension of control, but it does so in a way that matches users' own mental model.

In ‘Designing for Faceted Search’[^2], Stephanie Lemieux has an excellent example of why this is.

> Think of a cookbook: authors have to organize the recipes in one way only-by course or by main ingredient-and users have to work with whatever choice of organizing principle that has been made, regardless of how that fits their particular style of searching. An online recipe site using faceted search can allow users to decide how they’d like to navigate to a specific recipe [by course type, cuisine or cooking method, for example].

## Types of filters

Links, radio buttons and checkboxes: all of them can be, and are used to construct filters on the web, with their varied semantics and behaviour, this is what we'll look at first.

Links, let users filter a list of a results via the querystring. Admittedly, links are nothing to do with forms, but they are often used interchangeably with form controls so talking about them in-context now is important. Here's an example of a link filter used to find a car.

![Link filter](.)

Each time a user chooses a filter, such as Honda, that option will disappear from the list. The advantage of using links in this manner, is that every click is guaranteed to return results. This is becaue the link only appears in the first place, if there are results that lie behind it.

The disadvantage of using links is that each click requires a page refresh. In isolation, a page refresh isn't a problem at all. Lightweight, well-optimised, single-focus pages don't cause problems for users. However, if the user is interested in a few particular filters, waiting for three separate requests is not ideal.

If the user wants to select Honda and Audi, for example, a form with checkboxes may provide a better experience. By offering users a familiar set of form controls, we give them power and control to select as many options as they want in one go&mdash;both across and within different filters, such as brand and colour.

![Checkbox filter](.)

Unlike links, letting users choose multiple filters at once may result in zero results. If, for example, they select Red + Hondas, there may be none of those available. Of course, there will be away to remove a filter one at a time at the users discretion, but this doesn't eradicate the potential added friction.

Beware not to go crazy with filters. Overloading users with hundreds of them makes the job harder, not easier. If you're not sure which ones to show, conduct some research and check your analytics. Extract the most popular ones and include just those  ones in order. Either remove the others or reveal them as they become relevant as the user drills down.

Remember, that a broad and shallow taxonomy creates a better experience without sending users too far down a funnel.

## Material honesty (again)

We've talked about material honesty several times in the book. That's because it's both important and prevelant in the world of interface design. As a reminder of what it is, Jeremy Keith says that *one material should not be used as a substitute for another. Otherwise the end result is deceptive.*

In the case of filters, often links are made to look like radio buttons or checkboxes (using CSS backgrounds). The problem, of course, is that a radio button is not a link. When a link is clicked, it will immediately make a request and take the user to that page.

![](.)

As the design of form controls varies so widely across browsers and operating systems, the CSS styling always differs from native inputs which creates a slightly jarring, inconsistent and unprofessional visual tone. But there's more to this than just vaneer. If a user sees a radio button, conventionally speaking, they'll know that they can move through the choices before submitting it as a separate action. That's just how forms work. It would be confusing if clicking, what looks like a radio button, suddenly submitted the form automatically. This is materially dishonest and therefore deceptive.

> ‘Use checkboxes and radio buttons only to change settings, not as action buttons’&mdash;Jakob Nielsen

Similarly, checkboxes and radio buttons are made to behave more like links. With a little Javascript, clicking a radio button can be made to submit the form automatically. This is particularly problematic for keyboard (including screen reader) users, because they'll struggle to move through the options and negates being able to select multiple filters in one go.

Breaking widely understood conventions that relate to links and form controls can seriously harm the resulting user experience. By sticking to conventions, users who encounter these components both visually and audibly, don't have to think.

## Using AJAX

Earlier we discussed the pros and cons of using links and form controls. Using links, may cause users to endure many page refreshes. And using form controls, may increase the chance of users seeing no results.

For keyboard and screen reader users, having a page refresh means having to wade through all the page information again, such as header and navigation before getting back to the filter or the results, although they can skip that easily if landmarks are employed.

In any case, AJAX can be used to avoid this problem. For example, clicking a radio button, could instantly submit the form with AJAX. When the request finishes, the page is updated and the focus remains unaffected. This also reduces the chance of seeing no results because as soon as the user selects a filter, the interface updates.

As many applications have materially dishonest filters, some users have acclimatised to this change in convention (that clicking form controls instantly submits the form). I'm sad to say that, in this context, it may not be obvious to users, that they have to submit their choices. AJAX helps here because it potentially removes the need for the button.

You'd be forgiven then, for thinking this is a must-have, totally-beneficial enhancement. Unfortunately, using AJAX like this goes against principle 4, to *give users control*. By removing the explicit act of submission, it's possible that triggering multiple AJAX requests will cause an unexpected and confusing experience.

Additionally, keyboard users who are operating the filter must use their arrow keys to move through each radio button. Arrow keypresses not only set focus to the option but selects it too. Selecting the fourth radio button will have inadvertently created four AJAX requests unknowingly. This adds increased load on the server, but more importantly, it will eat users' data allowance and cause battery drain on their device.

That's not all though. Using AJAX requires a certain number of provisions such as a live region to indicate loading states. When the user selects an option, the live region would have to be populated with ‘Loading results, please wait.’, for example. Then when the request finishes, it would have to be populated with ‘212 results returned.’. Having to hear this four times is (like listening to an overzealous DJ) headache inducing.

Despite what you may have heard, AJAX isn't necessarily better or faster than a standard page refresh[^3]. This is for a number of reasons. First, it requires more Javascript code to be sent initially. Second, and more importantly, it engineers away progressive rendering (aka chunking) and removes loading states, both of which the browser provides for free.

AJAX is more suited and beneficial when making updates to small parts of the page. Filters involve updating the majority of the page, making AJAX somewhat counterproductive. With that said only thorough and diverse user research will show what works best.

## Responsive design considerations

Wherever possible, we should design interfaces mobile first. What this means is designing for small-screens first. To me, this really means essential only. If you put the most essential content and features on the screen, then normally speaking, designing for small screens is easy.

Of course, this translates well on large screens: a small increase to font-size perhaps coupled with more whitespace usually works well. When it comes to filters, though, it's not quite so straightforward. This is because the results themselves are only slightly more important than the ability to actually filter them. What I'm trying to say is that they both need to be prominent within the viewport.

On big screens this is straightforward. Due to the available space, we can simply position the filter to the side of the results laid out in plain site. On small screens, this just isn't possible. We could hide the filters behind a collapsible panel above the results themselves. This way the filters are still discoverable without taking up too much room.

![Stacked](.)

The problem that users face is that when they expand the filter and select various filters, users can struggle in two ways. First, if using AJAX, it's not immediately obvious that results have been loaded. You can't just move focus to the results because the user may not be finished making their selection. Second, without using AJAX, users may not realise they need to submit.

David House, a designer who used to work for Gumtree, explained that some users didn't realise they had to submit their choice. Here's what he had to say:

> Filters were being selected, but not submitted. We got a lot of feedback that said the filters were broken. We tried moving the apply button to the top (and the bottom) along with making it sticky, loads of things that didn't really make a difference.

David told me that they reluctantly ended up using an adaptive approach so that on mobile, clicking the filter menu button, sends users down a guided flow that users understood. But, on desktop, users got an ever-present filter on the side.

![Gumtree mobile view](.)

I know what you're thinking though: in chapter 6, ‘An inbox’ I lambasted the use of adaptive design due to the many associated pitfalls. But considering the breadth of the problems users faced along with the importance of the feature itself, it's the pragmatic way to go.

Of course, I don't advise you start with this approach. David wouldn't either. It's also worth noting that Gumtree is full of adverts which cognitvely speaking, doesn't help matters. Perhaps with careful design, a responsive solution can work. Once again, user research will guide you in the right direction.

## Summary

This chapter looked at some of the important details that are often missed when design a responsive filter component. We looked at how design can shape users&mdash;so much so&mdash;that occasionally we may have to break convention and best practice.

We also looked at ways to design an adaptive filter menu given that a responsive solution may not cut the mustard. We also noted that the only way to be sure of any of this is by testing with a diverse set of users.

### Things to avoid

- Making links look like form controls.
- Making form controls behave like links.
- Assuming that AJAX will create a faster and better experience.
- Being closed to breaking the rules (on occasion).
- Ignoring user research.

## Footnotes

[^1]: https://en.wikipedia.org/wiki/The_Magical_Number_Seven,_Plus_or_Minus_Two
[^2]: https://articles.uie.com/faceted_search/
[^3]: https://jakearchibald.com/2016/fun-hacks-faster-content/