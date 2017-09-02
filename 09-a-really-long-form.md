# A long and complex application process

Some forms are really long. Other forms involve users having to upload multiple documents (and multiple other things too). Some applications all of these things together.

One Thing Per Page helps users complete a form in one sitting, but some online applications or case management tasks require various information and documents to be prepared over a number of weeks.

Here's an example of a digital service that requires case management and has a long-winded application process: people who unfortunately need help from the state can apply for government assistance in the UK by applying for what's called Universal Credit. As part of the application process, users need to provide various information about themselves. It's a long and unweildly process that requires information about their finances, living arrangements, family members and more.

Because the process is long and complex, and because users don't always have everything they need to hand. Once they finally complete their application, users need to carry out regular tasks to ensured their continued assistance. All of this requires heavy use of forms.

Making long, unweildly and repetitive tasks easy and inclusive, requires extra design considerations.

## Task list pattern

In chapter 3, ‘A checkout flow’ I introduced you to One Thing Per Page. It's an essential pattern because it breaks longer forms into bit-size chunks and keeps momentum. A good checkout flow should take just minutes to complete for the average user. But what if a transaction takes up to an hour or more?

This is where the task list pattern comes in. Simply put, it breaks down a very long form journey into several little ones that make sense to be sub-grouped. Then, as each sub-journey is completed, it's marked as completed.

![Task pattern UC](.)

This pattern lets users fill out various sections in their own time without losing progress. It also gives users a feel for how long the process is going to take. In ‘A checkout flow’ we discussed how little value there was in providing a progress indicator which makes sense given a 1 minute task. Here though, giving users an indicator of their time is essential.

If the user wants to check what they've entered they can. If they want to resume later on they can do that too. By marking each task as complete, the user can pick up where they left off easily.

- time for each task?

Once all tasks are completed there should be a large call to action button that takes users forward, normally into their account. At this point users can of course check their answers and make amends should they wish.

## Uploading documents

Some tasks involve users having to upload documents. On one hand, uploading a file is only marginally more complex than inputting any other type of data. On the other, there are some nuances that need to be taken into account. Uploading multiple documents blows the whole thing wide open for usability considerations. For the moment, let's start small.

### A file input

A file input (`type="file"`) is similar to most types of input. But instead of typing into it, you click a button that lets you select a file from your device or computer. Here's how it looks:

![File input](.)

```HTML
<input type="file">
```

Some designers like to restyle the file input because it's quite ugly. We know that *pretty and useless* is far worse than *ugly and useful*, but that doesn't mean beauty and aesthetics aren't important. Where possible we should marry the two together.

Styling file inputs is really tricky because browsers mostly ignore any attempt at doing so with CSS. One approach is to visually hide the input, demarcating solely via the label. Unlike file inputs, labels are far easier to style.

![File input just a label](.)

After you've hidden the input and styled the label, you need a little bit of Javascript to handle the focus and selected states. When the user selects a file, the `onchange` event updates the label text to ‘1 file selected’ (or similar).

![Hidden file input](.)

On the face of it, this implementation is an excellent enhancement. Keyboard users can tab to the component. Mouse users can click it. And screen reader users have a label associated with the focusable file input because it's only visually hidden. But even these crumbles under scrutiny.

First, updating the label text to reflect the state is confusing because the label should describe the field and remain unchanged regardless of state. In this case, when users move back to the field, instead of ‘Attach document’, screen readers will announce ‘1 file selected’.

Second, this visual design makes no allowance for a hint or error message which is normally positioned inside the label, as set out in ‘A registration form’.

Third and last, file inputs let mouse users drag and drop files. The input itself acts as a ‘dropzone’, which may be preferable for savvy users. Hiding the input means forgoing this functionality.

Together, the improvement to aesthetics just isn't worth the degradation in functionality.

### Multiple files

One way of letting users upload multiple files, is by providing multiple file inputs. This sounds simple but it's not by a long shot. We'll discuss why this is shortly. Before we do though, let's first discuss another way.

Some browsers and devices let users upload multiple files using a single file input by adding the `multiple` attribute.

![Multiple files](.)

```HTML
<input type="file" multiple>
```

This innocuous boolean attribute grants a lot of power and seemingly solves the multiple file problem in one hit, but it's not without it's problems.

The first problem is that users can only select files within a single directory. If they want to upload files across several folders they'll be stuck, unless they realise they have to move all the files into a single folder beforehand. This is a big ask for low confidence users, and hardly satisfactory for more confident users.

Alternatively, we could ask users to compress multiple files into a zip file before uploading. In this case, we'd just use the very broadly supported single file input. But this puts the onus on the user again. They may not have compression software or know what it is, let alone how to use it.

Most crucially though, is that this enhancement isn't widely supported. Even if it had good support, we'd still want to consider what happens in other browsers. In this case, it reverts back into a single file input, which means users won't be able to upload more than one document. This degradation leaves the interface broken.

We're going to have think of something a lot better than this if we want to use a pattern that is both simple and inclusive. Let's keep going.

### Drag and drop enhancement

If your inclusive design hat is on tight, the first thing you might be thinking is that drag and drop is just for mouse users and that the approach leaves out keyboard users. This is only partially true because it depends on how we design the enhancement in the first place, but we're getting a bit ahead of ourselves.

Earlier, I noted that a file input already has drag and drop capability. So if you're thinking this enhancement may be redundant then you may well be right. Only user research will determine the native drag and drop behaviour is enough or not. With that said, there are some subtle design issues that indicate a need.

First it's not immediately obvious that a file can be dragged onto the file input. There's no information and affordance that indicates this behaviour. Second, for it's not a particularly large hit area, which makes dragging and dropping particularly hard for motor impaired users. Creating an enhanced drag and drop interface gives us an opportunity to solve both of these problems.

--

Notes:

- One thing to be aware of though, is that you can't use a custom drag and drop approach to populate file inputs for submission due to security. This means that you have to upload the files immediately (`ondrop`) using AJAX. A request per drop.
- Due to this constraint you won't want to mix normal fields with upload fields. Mixing the two could cause confusion. Although consider gmail. Upload files and send email together in the UI.
- Another consideration is that as the interface is very different for drag and drop users, it's wise to give users the choice to convert the interface into a more standard input.
- This means the choice needs to convert the interface into multiple file inputs which has it's own set of problems (how many they need etc) discussed momentarily.
- If users need to drag and drop files from multiple directories, they'll have to drag and drop multiple times and keep track of the things they have already uploaded, things in progress etc.


With all that said, this solution just isn't going to work. We'll need to enhance the interface with Javascript let users either drag and drop OR use multiple file inputs. We need to give users choice.

Flow and interaction. Use GMAIL approach. Let users drag and drop files uploaded with AJAX. Then let the user submit the form to save and finish/move onto next step if there is one.

Without JS we're have to let users still upload multiple documents somehow anyway.

To give users an agreeable and inclusive experience, we need to rethink the entire design approach.
Also, this problem is worth solving in a more abstract fashion. Universal Credit, for example, doesn't just ask users to upload multiple documents, but to provide information about ‘multiple’ children too. Let's consider patterns for being able to ‘add another’.

## Add another

Patterns are always easier to understand when they are applied to real problems. I don't think there is a one-size fits-all for letting users add multiple of something, be it files or plain text.

Of course, if you know how many of something the user needs to add, then simply displaying those fields and making them required through validation is the way to go. But if you don't, then there are two approaches that broadly-speaking, work well depending on the frequency of usage.

You can probably marry frequency of use with level of ability. The user is far more likely to have low confidence if they use the service less frequently or as a one off. Even a high confidence user, who is experiening the service for the first time could benefit from a simpler but slightly more long-winded approach.

### Infrequent usage (One Thing Per Page)

1. Upload file input, press next.
2. Show success, plus ‘Do you want to add another?’
3. ‘No’ > confirmation. ‘Yes’ > back to #1.

### Frequent usage (One Page For All)

#### Add another button

1. Upload a file with Next and Add Another buttons.
2. ‘Add another’ reveals and focuses onto new field(s). ‘Next’ completes the task.
3. When another field is cloned, a Remove button is added. Clicking it removes the field and sets focus to where?

- users could miss secondary action and do the minimum. It doesn't guide the user.

#### Additional question

1. Upload file input, ‘Do you want to add another YES NO’ and Next button.
2. Clicking yes reveals and focuses onto new field(s).
3. Clicking Next without answering YES or NO generate validation error. Otherwise shoud confirmation.

- This forces the user to answer the question solving previous problem but adds another problem. That we're moving focus via selecting an input so user can't switch and breaks convention.

#### Keep form there, and add to a list

- on mobile, users have to scroll to see success or to see form.
- multiple call to actions.

## Summary

### Things to avoid

## Footnotes

## Todo?

- https://www.youtube.com/watch?v=hqSlVvKvvjQ

## add another js differences

- If you know how many things a user needs to add then show that many fields exactly.
- If you don't know how many things they'll add, but you know the max they are allowed to add, then show the max. Then with JS, show/hide/reveal them.
- If you don't know the max they are allowed to add, then you'll have to offer an add another link. Without js go to a page and back. With js, reveal an extra field OR
- Upload a file, show he file is uploaded, and ask if they want to add another with yes and no. One call to action, guide the user, but long winded for frequent users.
