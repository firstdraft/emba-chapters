# Getting strings from users

## gets

We can make our programs much more interesting if we allow the users of the program to interact with them by supplying input. We can do this with the `gets` method (pronounced "get S", short for "get string"), which will pause the program and wait for the user to type something in the terminal and press <kbd>return</kbd>. The return value of the `gets` method will be a `String` containing what the user typed, which we can store in a variable and then process further like any other `String`.

For example, rather than saying "Hello, world!", let's have the computer say hello to the user by name instead. When you run this program, it will pause after saying `"What's your name?"` and you will have to type something in and press <kbd>return</kbd>. **Click on the terminal to put focus there**, and then you'll be able to type into it:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/Hello-gets?lite=true"></iframe>

Great! Our first user input. However, you'll notice a couple of things. First of all, there's a `\n` sneaking into the input. `\n` represents a newline character, and it's in there because of the <kbd>return</kbd> that is pressed to submit the input.

## puts

If you want to see the newline in action, we can use a different printing method called `puts` (pronounced "put S", short for "put string"). `puts` is actually the printing method that is used most when crafting the final output of command-line programs; as opposed to `p`, which is used most for _making the invisible visible_ while debugging. Try switching

```ruby
p "Hello, " + their_name + "!"
```

to

```ruby
puts "Hello, " + their_name + "!"
```

and see how the output is different.

You can see that the quotes around the string are removed, which makes sense if you're actually displaying output to a user and not debugging — users should not know or care about the quotes around Ruby string literals. And the newline character causes a line break when a string is printed with `puts`, as it should.

Most of the time, we'll stick with `p`, since it provides more details while debugging; but it's good to know that `puts` exists.

## gets.chomp

We almost never want to keep the `\n` that results from the <kbd>return</kbd> keypress that submits the user's input. Fortunately, [the handy `.chomp` method](https://emba.firstdraft.com/chapters/25#chomp){:target="_blank"} does exactly what we need — if there's a `\n` at the end of a string, it will remove it; if there isn't, it does nothing. So, in practice, when we call `gets` we almost always tack a `.chomp` on to it immediately. Try modifying the program to:

```ruby
their_name = gets.chomp
```

and see how it's different.

Test your skills:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3075907/f9998ca0e45feb87972cfb616aa698a5"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3075914/13838173da964e3be6370c9195064b13"></iframe>
