# Array

Next, we have a _very_ important class: `Array`. Most of what we do as
developers is manage _lists of things_. Lists of photos, likes, followers,
reviews, listings, messages, rides, events, concerts, projects, etc etc etc.

The first data type we're going to learn to help us manage lists of things is
`Array`. This class, unlike the ones we've seen until now, is really just a
_container_ for other objects, and can hold however many objects we want.

## Creating arrays

As always, we have the formal way of creating a new object in Ruby: use the `.new` method on the parent class:

```ruby
cities = Array.new
```

Try it out and see what you get if you `p cities`:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/Arraynew?lite=true"></iframe>

### push

As you can see, Ruby represents an `Array` with square brackets, `[]`. The brand new array is empty; let's add some **elements** to it with the `.push` method. Try this:

```ruby
cities = Array.new

cities.push("Chicago")
cities.push("NYC")
p cities

cities.push("LA")
# Add some of your own, if you like
p cities
```

Now we're talking! We've stored multiple strings within a single array using the `.push` method[^push_alias]. Ruby separates the elements in an array with commas.

[^push_alias]:
    You might come across a shorthand for `.push`, the `<<` method, known as the "shovel" operator. This allows you to write something like:

    ```ruby
    cities.<<("Chicago")
    ```

    Or with the syntactic sugar that we're very accustomed to by now:

    ```ruby
    cities << "Chicago"
    ```

    I personally prefer `.push` — I think it's more readable — but feel free to use the shovel if you like it better.

### Array literals

Much like with `String`s, there's a shortcut to creating `Array`s. Rather than starting with `Array.new` and building it up from scratch one `.push` at a time, we can type an array "**literal**" directly into the code:

```ruby
cities = ["Chicago", "NYC", "LA"]
```

This is the technique that we'll be using most often.

## Methods

Now let's familiarize ourselves with some of `Array`s methods.

### at

After adding elements to the list, the next most important thing we need to be able to do is retrieve an element back out from the list. Our first tool for doing that is `.at`.

The `.at` method takes an `Integer` argument, which is interpreted as the position, or **index**, in the `Array` of the element that you want to retrieve. Give it a try:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/array-at?lite=true"></iframe>

Whoa! Did you expect `cities.at(2)` to return `"LA"`? I sure didn't, the first time I tried it; I was expecting `"NYC"`.

It turns out that pretty much every programming language indexes the elements in an array starting at _zero_, not at one. So the first element is retrieved with `cities.at(0)`, the second element with `cities.at(1)`, etc. You'll get used to it after a while.

A couple of other things for you to experiment with:

 - What happens when you use an index greater than the length of the array?

    This is our first contact with `nil`, an object that represents **the absence of anything**. When you use an index "outside" the array, you might have expected to see an error message; but instead, Ruby returns `nil`.
 - What happens when you use a negative index?

### at shorthand, []

There's a shorthand for `.at()` which is very common, so you should be familiar with it. It's the `.[]` method, so we _could_ write:

```ruby
cities = ["Chicago", "NYC", "LA", "SF", "NOLA"]

p cities.[](2) # => "LA"
```

You guessed it — there's some syntactic sugar coming up. When a class has a method named `.[]`, Ruby allows the dot to be dropped, but the method name has to then move immediately next to the object it's being called on (no space), and the argument moves _inside_ the method name! Altogether, this allows us to write:

```ruby
cities = ["Chicago", "NYC", "LA", "SF", "NOLA"]

p cities[2] # => "LA"
```

Which is sort of nice. I prefer `.at` because I think it reads better, but feel free to use the square brackets if you like that style better.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/array-square-bracket?lite=true"></iframe>

### first, last

Since retrieving the elements at positions `0` (the first one) and `-1` (the last one) is so common, there are handy shortcut methods for those: `.first` and `.last`. Give them a try.

### index

The `.index` method is sort of the inverse of `.at`: given an object, `.index` searches within the array and returns the index where it resides. Give it a try:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/array-index?lite=true"></iframe>

Some further things for you to experiment with:

 - What will `.index` return if the element is present in the array more than once?
 - What will `.index` return if the element is not present in the array at all?

### String#split

Before we proceed with more `Array` methods, I want to go back for a minute and talk about [the `.split` method from the `String` class](https://emba.firstdraft.com/chapters/25#split){:target="_blank"}. This method, when called on a `String`, will return an `Array` of substrings:

```ruby
"alice bob carol".split # => ["alice", "bob", "carol"]
```

If you provide no argument, the string is split upon whitespace, which is handy for e.g. turning a sentence into a list of words:

If you do provide an argument to `.split`, then the string will be chopped up wherever that argument occurs instead of whitespace — for example, use `"4,8,15,16,23,42".split(",")` to split on commas.

You can also `split` with the empty string, `""`, as an argument in order to turn a string into an `Array` of its individual characters:

```ruby
a = "Hello!".split("") # => ["H", "e", "l", "l", "o", "!"]
a.at(0) # => "H"
a.at(-1) # => "!"
```

This is particularly handy for us because it allows us to get a `String` of input from users with `gets` and then transform it into an `Array` for processing:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/gets-with-split?lite=true"></iframe>

We'll be using this technique for the remainder of our test REPLs, to make things more interesting.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056046/2a38280792b66bcfd9b18610c1bc7b81"></iframe>

### count

 `.count` counts how many elements are in the list, if called with no arguments. If an argument is provided, it counts how many times that argument occurs in the list.

 <iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/count?lite=true"></iframe>

 <iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055583/c12a04f9499595923047a48b7780f4ba"></iframe>
 
### include?

A thin convenience layer on top of `.count`, `.include?` will quickly tell you whether a value is present within an `Array`:

```ruby
a = [ "a", "b", "c" ]
a.include?("b")   #=> true
a.include?("z")   #=> false
```

### reverse

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/array-reverse?lite=true"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055729/b0a7a5e4e9aefc1f901e468b66e72170"></iframe>

### sort

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/sort?lite=true"></iframe>

### shuffle

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/shuffle?lite=true"></iframe>

### sample

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/sample?lite=true"></iframe>

### min

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/min?lite=true"></iframe>

### max

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/max?lite=true"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056042/7591baf04059bc88bdb3ff4ca1dd0a58"></iframe>

### sum

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/sum?lite=true"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056044/a9ab190d99bac78dae3ac95dbb523719"></iframe>
