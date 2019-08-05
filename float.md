# Float

Ruby calls decimal numbers `Float`s. To create a `Float` rather than an `Integer`, just make sure to include a decimal point:

```ruby
5.class # => Integer
5.0.class # => Float
```

## Methods

### + - * / ** (math)

The math methods work mostly like you'd expect, and similarly to [the ones for integers](https://emba.firstdraft.com/chapters/16#-------math){:target="_blank"}.

The main difference to keep in mind is with `/`. Division with floats works the way that we're used to — it returns fractional results, as a `Float`:

```ruby
12.0 / 5.0 # => 2.4
```

Try the following and see what you get:

```ruby
12 / 5
12.0 / 5
12 / 5.0
```

<iframe frameborder="0" width="100%" height="600px" src="
https://repl.it/@raghubetina/Float-math?lite=true"></iframe>

What did you discover? If _either_ side is a float, float division will be performed.

This is why `Integer`'s `.to_f` method can come in handy while doing math; at some point if you need to do division and need a fractional answer, then convert it to a `Float` first.

One other thing to keep in mind: you can use `**` in conjunction with fractions to calculate roots, since 9<sup>1/2</sup> is the same as the square root of 9, 8<sup>1/3</sup> is the same as the cube root of 8, etc.

```ruby
9 ** 0.5 # => 3.0
8 ** (1/3.0) # => 2.0
```

Test your skills:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055426/d721e1044eb3966a6258d16a9e267b93"></iframe>

### round

`Float`s can round themselves. Play around with the `.round` method:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/round?lite=true"></iframe>

Test your skills:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056033/7aa7099b585e775a081f42be4a82c72d"></iframe>

### rand

The `rand` method that we met earlier can also be called with no arguments, in which case it returns a `Float` between 0 and 1. This is very handy for e.g. probabilities. Give it a try:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/float-rand?lite=true"></iframe>

## Conclusion

That's it for `Float`s! Next up, we'll look at how we can use these basic data types together with _conditionals_ to start building smart programs that can make decisions based on varying input.
