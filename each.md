# Each

When we met the `Array` class, we noted that most of what we do as developers is
manage _lists of things_ — photos, likes, followers, reviews, listings,
messages, rides, events, concerts, projects, etc etc etc — and `Array` is the
data structure that we'll most commonly use to contain these lists.

Therefore, the most common reason we'll have to write loops is to **visit each
element in an `Array` and do something interesting with it** — for example,
display the element to the user with some formatting around it.

## Iterating over arrays with Integer's times method

Try transforming the words in an Array using what you've learned so far about [loops](https://chapters.firstdraft.com/chapters/17){:target="_blank"}:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3097392/981114309d419ae15703d1917e2b7782"></iframe>

---

After you've got it working, examine the model solution. You'll see that I chose to use `.times` for this job.

 - On Line 6, we count the length of the array.
 - On Line 8, we use that length with the `.times` method to kick off a loop with the correct number of iterations.
 - Within the block, we use the block variable (which we named `the_index`) to access the correct element within the array.

Using `.times` to iterate over an `Array` is not bad at all, especially because `.times`'s block variable starts at `0`, just like array indexing does. Using `.times` is certainly cleaner than using `while`, where we would have to create and increment a counter variable ourselves, and then write a condition to make sure that the loop stops after the correct number of iterations (the length of the array).

## Array's each method

But we can do even better than using `Integer`'s `.times` method to iterate over an `Array`. There's a method that you can call directly on the `Array` itself called `.each`. Compare the code below to the model solution above and try to find the differences:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/each-iterating-with-each?lite=true"></iframe>

Click "run" and verify that both programs do the same thing.

Nice! `.each` has two clear benefits over using `.times`:

 - We don't need to count the length of the array; `.each` does it for us and will take care of looping for the correct number of iterations.
 - The block variable, rather than containing an integer that we can use to access the correct element, will contain **the element itself**.

     So now when we name the block variable, we should choose a name[^singular_vs_plural] that reflects what each element in the list is.

     `.each` will, behind the scenes, pull the correct element out of the array before each iteration begins and assign it to that block variable.

     Then, **we just use that variable directly**, and we don't have to worry about accessing the array with `.at`.

[^singular_vs_plural]:
    I like to name the variables that contain arrays _plurally_ (e.g. `photos`), and block variables _singularly_ (e.g. `photo`) to make it clear to myself which is which — the list itself versus one element within the list.

    Whatever you do, don't name the block variable plurally — that's very confusing when you come back to your code later and have to make sense of it.

The hardest part, I think, is getting your head around the block variable; in this case, `|the_word|`. It takes some practice.

Try to remember that it's just a name that _we make up_, and `.each` takes care of putting each element in that variable for us behind the scenes. I could have called it `zebra` if I wanted to; there's nothing special about the name — in particular, it doesn't have to match the name of the variable containing the array. Just try to pick something descriptive of an individual element in the list.

## Sneak peek

Just a sneak peek as to why we `.each` is so important to get comfortable with: soon, you'll be embedding Ruby loops in your web applications to create dynamic, data-driven pages with code that looks something like this:

```erb
<% newsfeed_photos.each do |the_photo| %>
  <div class="card">
    <img src="<%= the_photo.image_source %>">

    <p>
      <%= the_photo.caption %>
    </p>
  </div>
<% end %>
```

Code like this is what drives the dozens of dynamic applications you interact with on a daily basis — we pull a list of records from a database table, then we loop over them, and then we format each one using some _markup language_ (in this case HTML for the browser, but it could be XML for native apps, etc).

## each_with_index

There are some rare cases when you are looping over an array and, within the block, you would like access to the element _and_ its index. For example, maybe you want to print a line after every other element. You could fall back to `.times` in these scenarios, but there's also another `Array` method that has your back: `.each_with_index`. It looks like this:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/each-each-with-index?lite=true"></iframe>

As you can see, some methods provide more than one block variable. `.each_with_index` allows you to name two variables within the pipes; the first one will receive the element, and the second one will receive the index of the iteration. Within the block you can use both variables as you see fit. That's that.
