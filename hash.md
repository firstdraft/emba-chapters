# Hash

`Array` is a very good structure for containing multiple objects, but it's not the only one. In some situations, another structure is a better tool for the job: `Hash`.

Suppose we have instructors and students:

```ruby
instructors = ["Raghu", "Logan", "Jelani"]
students = ["Jocelyn", "Arthur", "Tom", "Lindsey"]
```

Suppose we want add last names and roles?

```ruby
person1 = ["Raghu", "Betina", "Instructor"]
person2 = ["Jocelyn", "Williams", "Student"]
# etc

p person1.at(0) + " is a " + person1.at(2)
p person2.at(0) + " is a " + person2.at(2)
```

This may suffice if the list of attributes is small, but six months later when you come back to this code, do you really want to remember which index number was last name and which was first name and which was role? Not to mention always dealing with the array-indexes-begin-with-0-and-not-1 thing in your head.

There's a better way: rather than having `Array`s automatically number each piece of data, _we_ can give them meaningful labels in a `Hash`.

## A brief interlude: Symbols

There is one datatype that we haven't discussed until now that will come in handy: `Symbol`s.

`Symbol`s are like `String`s: a sequence of characters. However, `Symbol`s follow the same rules as variable names:

 - Cannot contain spaces.
 - Can only contain lowercase letters, underscores, and numbers.
 - Cannot begin with a number.

Otherwise, they are just like strings, and we can use them to hold text data:

```ruby
"hello" # I am a String
:hello  # I am a Symbol
```

`Symbol`s are created by starting them off with a colon. You don't need a closing colon, since they cannot contain spaces; Ruby can figure out where they end.

`String`s are usually used to contain copy for the user or input from the user; whereas `Symbol`s are mostly used when we, the developers, need to label something internally in our code. That's why they can't contain spaces, etc; we're not going to use them to hold user input. Accordingly, `Symbol`s don't have as many methods for transforming their contents, like `.reverse`.

So, that's that. `Symbol`s are lightweight strings that we, the developers, use when we need to label things. Let's continue.

## Creating hashes

Back to the problem of storing a list of attributes about a person effectively, without mixing them up.
`Hash`es are like `Array`s, except each cell isn't automatically numbered — **we get to label each cell ourselves**. So instead of representing a person with an Array like `["Raghu", "Betina", "Instructor"]`, we instead can use a `Hash` like this:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/hash-hash-new?lite=true"></iframe>

Click "run" and see what it looks like to build up a `Hash`. A few things to note:

 - Ruby represents a `Hash` with curly brackets, `{}`, as opposed to the square brackets (`[]`) of an `Array`.
 - We use the `.store` method to add elements to a `Hash`, as opposed to the `.push` method of `Array`.
 - The `.store` method takes _two_ arguments, not one: the first argument is the label, or **key** to store the element under; and the second argument is the piece of data itself, or the **value**.
 - Ruby represents each key/value pair by separating them with a `=>`, known as a "hash rocket"[^rubyists_are_weird].
 - As in `Array`s, elements in the list (each element is one key/value _pair_) are separated by a commas.
 - If the key already exists when you try to `.store` something under it, its value will be replaced.

[^rubyists_are_weird]: Rubyists are weird.

## fetch

To retrieve a piece of data from a `Hash`, we use the `.fetch` method (as opposed to `Array`'s `.at`):

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/hash-fetch?lite=true"></iframe>

Beautiful! Now we don't have to remember that position number 1 is last name, position number 2 is role, etc. We can retrieve objects from the list using meaningful labels instead.

Let's put it all together with multiple `Hash`es:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/hash-second-person?lite=true"></iframe>

A few things to try:

 - What happens when you try to `.fetch` using a `String` like `"role"`?
 - What happens when you try to `.fetch` using a key that doesn't exist, like `:middle_name`?

Get used to those error messages. You're going to see them a _lot_.

### fetch fallback

Sometimes you may want to call `.fetch` using a key that may not be present in the `Hash`, and you don't want the program to crash with the "key not found" error message. In that case, you can provide a second argument which will be used as a fallback return value:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/hash-fetch-with-fallback?lite=true"></iframe>

## Hash literals

Like `String` and `Array`, there's a shortcut to creating `Hash`es: rather than formally instantiating a new `Hash` with `Hash.new` and then building it up from scratch one key/value pair at a time with `.store`, you can type out the **hash literal** directly into your code:

```ruby
person1 = { :first_name => "Raghu", :last_name => "Betina", :role => "Instructor" }
person2 = { :first_name => "Jocelyn", :last_name => "Williams", :role => "Student" }
```

Like with `String` and `Array`, this is the style that we're going to use the vast majority of the time.

In particular, `Hash`es are very often used as the arguments to methods, because they let us pass in a list of inputs with nice labels. When we get to Ruby on Rails, especially, we will very often type hash literals directly into the parentheses of method arguments, things like:

```ruby
Movie.where({ :title => "The Shawshank Redemption" })
```

## at shorthand, []

Much like [`Array`'s shorthand for `.at`](https://chapters.firstdraft.com/chapters/5#at-shorthand-){:target="_blank"}, `Hash` also a shorthand for retrieving elements with `.fetch`: `.[]` (and the associated syntactic sugar). So we _could_ write:

```ruby
person1 = { :first_name => "Raghu", :last_name => "Betina", :role => "Instructor" }

p person1[:last_name]
```

_However_, unlike with `Array`, `Hash`'s `.[]` method and `.fetch` method do _not_ do the exact same thing. Experiment with them and see if you can find the difference:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/hash-fetch-shorthand?lite=true"></iframe>

Were you able to find the difference between the two methods?

---

`Hash`'s `.[]` method, when used with a key that is not present in the hash, _returns `nil` rather than throwing an error_.

I personally prefer getting the descriptive error message if the key is not present in the hash, because it means that I probably made a typo or some other mistake, and I prefer being alerted to that fact rather the program proceeding quietly only to fail elsewhere. In the rare case that it should be possible for a key to be optionally present in a hash, then I can use a fallback second argument to `.fetch`, as [described above](#fetch-fallback).

That said, out on the internet, using `.[]` is the most prevalent style of accessing hashes, so you should be familiar with it. But in this text, I will stick with `.fetch`.

## Use .keys to explore

A very important method to use when you're dealing with `Hash`es that you didn't create yourself is `.keys`:

```ruby
h = { "a" => 100, "b" => 200, "c" => 300, "d" => 400 }
h.keys   #=> ["a", "b", "c", "d"]
```

The `.keys` method returns an `Array` showing all of the keys that are present in the `Hash`, so that we know what we can `.fetch`.

This helps tremendously when dealing with a data structure that we didn't create ourselves, especially when it's deeply nested (a hash containing arrays which might contain other hashes, etc).

## keys can be anything

The keys in a `Hash` can be any class — `String`, `Integer`, whatever — but we almost always use `Symbol`s as keys to our `Hash`es. (I like using symbols as the keys simply because since the values are usually strings, syntax highlighting makes keys stand out from values in our code.)

## The bottom line

`Array`s are very useful for storing a list of things that are all basically the same, and for lists that are of unknown length, and so it's nice for Ruby to automatically number them for you.

But when you are storing a list of things that are categorically different from one another and you'd rather label them yourself, then `Hash`es are a better choice. That's about it!
