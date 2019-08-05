# String

Let's start by getting a more formal introduction to our friend, `String`.

First of all, notice that when I refer to Ruby classes, I capitalize the first letter. The _only_ time we use capital letters when we're programming is when we refer to Ruby classes. All other times — variable names, file names, etc — we're going to use lowercase letters only (other than when we're writing some copy inside a string, of course).

## Creating strings

We've actually been taking a shortcut this whole time when we've been saying something like

```ruby
s = "Hello, world!"
```

In Ruby, the formal way to create a new object is to use the `.new` method on the parent class:

```ruby
s = String.new
```

This will, however, just give us back an empty string `""`. We would then have to add each character to it one by one. One way to do so is by using the `.concat` method, which accepts a number as an argument, interprets it as an ASCII code, translates it into a single character, and adds it on to the end of the original string.

### ASCII Codes

What's an ASCII code? At the hardware level, computers only store integers (specifically, in _binary_ form — using only `0`s and `1s`); so all other datatypes need to be encoded somehow as a number. [ASCII](https://en.wikipedia.org/wiki/ASCII){:target="_blank"}, or American Standard Code for Information Interchange, was one scheme that was developed in the early days of computing to store English characters as integers[^unicode]. The codes are as follows:

[^unicode]: Nowadays we use much more sophisticated encoding schemes such as [Unicode](https://en.wikipedia.org/wiki/Unicode){:target="_blank"} that supports glyphs from many more languages, and even emojis 🙌🏾 Fortunately, Ruby handles most of this low-level stuff for us behind the scenes, so we never really have to worry about it anymore.

**ASCII Code**|**Character**|**ASCII Code**|**Character**|**ASCII Code**|**Character**|**ASCII Code**|**Character**|**ASCII Code**|**Character**|**ASCII Code**|**Character**
:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:
32|(space)|48|`0`|64|`@`|80|`P`|96|<code>`</code>|112|`p`
33|`!`|49|`1`|65|`A`|81|`Q`|97|`a`|113|`q`
34|`"`|50|`2`|66|`B`|82|`R`|98|`b`|114|`r`
35|`#`|51|`3`|67|`C`|83|`S`|99|`c`|115|`s`
36|`$`|52|`4`|68|`D`|84|`T`|100|`d`|116|`t`
37|`%`|53|`5`|69|`E`|85|`U`|101|`e`|117|`u`
38|`&`|54|`6`|70|`F`|86|`V`|102|`f`|118|`v`
39|`'`|55|`7`|71|`G`|87|`W`|103|`g`|119|`w`
40|`(`|56|`8`|72|`H`|88|`X`|104|`h`|120|`x`
41|`)`|57|`9`|73|`I`|89|`Y`|105|`i`|121|`y`
42|`*`|58|`:`|74|`J`|90|`Z`|106|`j`|122|`z`
43|`+`|59|`;`|75|`K`|91|`[`|107|`k`|123|`{`
44|`,`|60|`<`|76|`L`|92|`\`|108|`l`|124|`|`
45|`-`|61|`=`|77|`M`|93|`]`|109|`m`|125|`}`
46|`.`|62|`>`|78|`N`|94|`^`|110|`n`|126|`~`
47|`/`|63|`?`|79|`O`|95|`_`|111|`o`|

Given those ASCII codes, we can now build up a new string from scratch like so:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/creating-objects-with-new?lite=true"></iframe>

### String literals

What a pain! Now that we've shown that, under the hood, even creating a string follows the syntax of `noun.verb` — let's never do it again. From now on, we'll use the shortcut of creating string "**literals**" in place by typing the characters we want within quotes: `"Thank goodness!"`

These kinds of exceptions to the regular grammar in order to make life easier are known as "**syntactic sugar**".

## Methods

Next, let's familiarize ourselves with some of the `String` class's methods. For each method below, we've provided some REPLs. Some of them are just there for you to experiment with the code, click "run ▶", and see how the methods work; but in other ones **you need to use the methods you're reading about to make some tests pass**.

**Make sure you click "Submit" for any REPL that has tests in order to get credit for your work!**

### String addition, a.k.a. +

We've already met the `.concat` method. `.concat` can accept an integer as an argument, which it interprets as an [ASCII code](https://emba.firstdraft.com/chapters/25#ascii-codes), translates into a single character, and adds to the original string:

```ruby
"hi".concat(33) # => "hi!"
```

`.concat` can also accept a string literal as an argument, in which case it just adds the whole thing to the end of the original string.

```ruby
"hi".concat(" there") # => "hi there"
```

There's also a shorthand for `.concat`: `.+`.[^concat_lie] That may look a little funny, but it's nothing special, really; it's just a method with a very short (one letter long) name:

[^concat_lie]: This is not _quite_ true. The `+` method is not just an alias for `concat` — they do slightly different things. But they're close enough, for our purposes.

```ruby
"hi".+(" there") # => "hi there"
```

But here's where it gets interesting; Ruby has another bit of nice _syntactic sugar_ for us. If a class has a method named `+`, then you are allowed to drop the `.` before the method name when you call it, and just say:

```ruby
"hi" +(" there") # => "hi there"
```

Crazy! And, as we learned earlier when we were [introduced](https://emba.firstdraft.com/chapters/21#make-the-invisible-visible){:target="_blank"} to the `p` method, Ruby also allows you to omit the parentheses around arguments if you want to; so this can be further shortened to:

```ruby
"hi" + " there" # => "hi there"
```

Now this is really starting to look familiar! It's a lot like the calculator language, actually. [Developer happiness](https://emba.firstdraft.com/chapters/21#developer-happiness){:target="_blank"}, indeed.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/concatenation?lite=true"></iframe>

### String multiplication, a.k.a *

`String`s can be multiplied by numbers using the `*` method[^more_sugar]:

```ruby
"Ya" * 5 # => "YaYaYaYaYa"
```

This sort of makes sense, if you think about multiplication as being repeated addition.

[^more_sugar]: More syntactic sugar here, like with the `+` method above; you can say `"Ya" * 5` rather than `"Ya".*(5)`.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/multiplication?lite=true"></iframe>

The order matters, though. See what happens when you try:

```ruby
3 * "Hello"
```

Read The Error Message ([RTEM](https://emba.firstdraft.com/chapters/18#seriously-please-read-the-error-message){:target="_blank"})!

Does this make sense? `"Hello" * 3` is calling the `String` method `*` with an argument of `3`, which kinda makes sense (add `"Hello"` to itself `3` times).

But `3 * "Hello"` is calling the `Integer` method `*` with an argument of `"Hello"`, which doesn't make much sense (what would it mean to add `3` to itself `"Hello"` times?).

Thus, we can see why the `String` version of `*` and the `Integer` version of `*` both need an integer argument. Again, [the bottom line](https://emba.firstdraft.com/chapters/18#the-bottom-line){:target="_blank"} is — at all times as you are writing Ruby, you should be thinking: "What **class** is this object? What **methods** does _this_ class have available?" Even when there's some syntactic sugar making things _look_ unconventional, don't forget your basics! It's still `noun.verb` under the hood.

Test your skills:

**Here come's a submittable REPL!** You need to write some code to make the tests pass and then click Submit. Notice how it looks different — I won't keep reminding you which ones are which!

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055761/b8091d3eabe958cc55c0dd0d0845ec75"></iframe>

### upcase

The upcase method returns a copy of the `String` with all lowercase letters replaced with their uppercase counterparts.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/upcase?lite=true"></iframe>

### downcase

The downcase method returns a copy of the `String` with all uppercase letters replaced with their lowercase counterparts.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/downcase?lite=true"></iframe>

### swapcase

The swapcase returns a copy of the `String` with uppercase letters replaced with lowercase counterparts, and vice versa.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/swapcase?lite=true"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3054730/4e44347737416ad6d635474a0e3369f1"></iframe>

### reverse

The reverse method returns a new `String` with the characters from the `String` in reverse order.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/reverse?lite=true"></iframe>

### length

The length method  returns the number of characters (as an `Integer`) that a `String` has.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/length?lite=true"></iframe>

### chomp

The `chomp` method is mostly used to remove the `"\n"` (newline) character from the end of a string, if it is present:

```ruby
"Raghu\n".chomp # => "Raghu"
"Raghu".chomp # => "Raghu"
```

This seemingly strange task is very common due to the way that getting user input works; usually someone has to type something at a prompt and then they press <kbd>return</kbd> to submit it, and that adds a newline to the end of the string that they typed. Typically, we want to `chomp` that off the end of their input before we do anything further with it.

`chomp` can also remove other specified character(s) from the end of the string, if they are provided as an argument:

```ruby
"1 apples".chomp("s") # => "1 apple"
"1 apple".chomp("s") # => "1 apple"
```

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/chomp?lite=true"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055768/5eff4e7c735dc2a2821e719a87c82c36"></iframe>

### gsub

The gsub method returns a copy of the `String` it was called on with all occurrences of the first argument substituted for the second argument.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/gsub?lite=true"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055200/37255ba7d887c35597b69abd8c7cdc9d"></iframe>

#### Advanced gsub techniques

You can provide some advanced pattern-matchers known as _regular expressions_ as the first argument to `gsub`. For example,

```ruby
my_string.gsub(/\s+/, "")
```

will match and remove all whitespace from `my_string.`

```ruby
my_string.gsub(/[^0-9]/, "")
```

will match and remove everything _except_ numeric digits.


```ruby
my_string.gsub(/[^a-z]/i, "")
```

will match and remove everything _except_ letters.


```ruby
my_string.gsub(/[^a-z0-9\s]/i, "")
```

will match and remove everything _except_ alphanumerics and whitespace.

### to_i

Sometimes you have a string that contains a number, usually input from a user, and want to do math on it. `to_i` will convert a `String` into an `Integer`.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/toi?lite=true"></iframe>

### to_f

Similarly to `to_i`, `to_f` will convert a `String` into a `Float` (decimal).

### strip

`strip` removes all leading and trailing whitespace.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/strip?lite=true"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056024/e3e79ed5afa47389e0299abf09e3369f"></iframe>

### capitalize

capitalize returns a `String` with the first character converted to uppercase and the remainder to lowercase.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/capitalize?lite=true"></iframe>

### split

This transforms the `String` into an `Array` (a list), which we'll [read more about later](https://emba.firstdraft.com/chapters/5).

If you provide no argument, the string is split upon whitespace, which is handy for e.g. turning a sentence into a list of words:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/split?lite=true"></iframe>

If you do provide an argument to `.split`, then the string will be chopped up wherever that argument occurs instead of whitespace — for example, use `"4,8,15,16,23,42".split(",")` to split on commas.

You can also `split` with the empty string, `""`, as an argument in order to turn a string into an `Array` of its individual characters:

```ruby
a = "Hello!".split("") # => ["H", "e", "l", "l", "o", "!"]
a.at(0) # => "H"
a.at(-1) # => "!"
```

## More on adding strings together

We spend a lot of time composing strings of output for our users, so let's see a few more examples. Try this:

```ruby
number = 6 * 7
message = "Your lucky number for today is " + number + "."
```

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/String-interpolation?lite=true"></iframe>

You'll see that Ruby gets confused (RTEM!), because we are trying to add an integer to a string and it doesn't feel comfortable with that.

The solution is to tell the `Integer` to convert itself to a `String` first using the method called `.to_s`, or "to string". Try this instead:

```ruby
number = 6 * 7
message = "Your lucky number for today is " + number.to_s + "."
```

The above technique for composing strings, adding them together with `+`, is called string addition.

There's another technique for composing strings that I personally find a bit easier; it's called **string interpolation**. Try this instead:

```ruby
number = 6 * 7
message = "Your lucky number for today is #{number}."
```

Basically, inside the string, you place `#{}` where you eventually want your value to go. Inside the curly braces, you can write any Ruby expression without worrying about whether it is a string or not. The expression will be evaluated, converted to a string, and added to the string right in that spot. You can interpolate as many expressions as you want into a single string. Pretty neat!

If you find interpolation confusing, feel free to just use addition.

## Conclusion

That's about all we'll need to know about strings to do most anything related to web applications! Next, we'll take a look at numbers, [starting with `Integer`](https://emba.firstdraft.com/chapters/16).
