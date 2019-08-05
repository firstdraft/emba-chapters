# A few program notes

Before we continue, I want to pause and discuss what, why, and how we're going to be learning.

## What's our goal?

Our goal is to build and deploy a fully-functional application, which will require that we learn a little bit about every part of the "stack" — the interface that users interact with all the way through to the server that the application runs on.

We're going to work from the inside-out. We'll start by learning how to manage information with a _database_, because that is the heart of any useful application. Then we're going to learn how to expose an _interface_ for users to get information in and out of our database — we'll be starting with a web client because that's the quickest way to get up and running, but we could add an iPhone or Android client later.

## Why Ruby?

For all of the above, we're going to use the Ruby language. There are many languages we could have chosen, but Ruby has a lot of advantages for beginners:

### Developer happiness

Compared to other languages, Ruby is a pleasure to write and to read. Here's Yukihiro Matsumoto (a.k.a. "Matz"), the creator of Ruby:

> Often people, especially computer engineers, focus on the machines. They think, "By doing this, the machine will run faster. By doing this, the machine will run more effectively. By doing this, the machine will something something something." They are focusing on machines. But in fact we need to focus on humans, on how humans care about doing programming or operating the application of the machines.
>
> — Yukihiro Matsumoto, [The Philosophy of Ruby](https://www.artima.com/intv/ruby4.html){:target="_blank"}

Matz's focus when he designed Ruby was on "developer happiness", which was pretty bold in back 1995 when Ruby was first released. Optimizing for human readability rather than computer readability meant paying a cost in terms of performance, and computers were slow back then; but Matz didn't care.

Happily, computers are very fast today[^developer_hours], and they can run Ruby just fine; so now we beginners can have the best of both worlds. Besides, once you've learned the basic concepts of programming, they're not hard to translate into the syntax of another language[^right_tool].

[^right_tool]:
    If you decide to pursue software development, you'll end up learning at _least_ half-a-dozen languages. Whatever challenge is thrown your way, you'll choose the right tool for the job.

[^developer_hours]: From a business perspective, servers are very cheap while developers are _very_ expensive; so you should pick the language that makes developers the most productive.

### Ruby on Rails and other libraries

People often ask "What's the best language for task X?" Depending on the task, sometimes a language is particularly well-suited to it technically; but that is very rare. In general, any programming language can do anything that any other language can (unless maybe there is some kind of proprietary platform lock-out).

However, there's another very important consideration: which language has the largest **community** of developers doing task X? More community means more shared code (known as "libraries", or in Ruby-land, "gems"), more blog posts, more answers when you Google a question, etc.

For example, Python and Ruby are very similar languages in terms of their technical features and performance profile. However, by some historical accident, Python seems to have gathered more of the scientific/data analysis/machine learning community around it, so more open-source libraries in those areas have been developed and shared in Python than in Ruby; and so now Python is the go-to language for those tasks.

On the other hand, for agile development of database-backed applications, Ruby has a huge and thriving community. In particular, there is a gem for building applications called Ruby on Rails ("Rails", for short) that makes small teams, or even solo developers, incredibly productive. The existence of Rails alone makes the Ruby language a good choice[^rails_relevant] for both startups and beginners.

[^rails_relevant]:
    Here's a longer blog post on why Ruby on Rails is still a good choice in 2019:

    [https://devbrett.com/2019/03/why-i-believe-rails-is-still-relevant-in-2019.html](https://devbrett.com/2019/03/why-i-believe-rails-is-still-relevant-in-2019.html){:target="_blank"}

The philosophy of Rails is "convention over configuration" — it makes a lot of decisions on your behalf, and if you go with the flow, then things "just work". (If you want to fiddle with settings, then of course you can, to your heart's content; but you don't _have to_ spend hours or days doing so before anything will even show up, like you do in most other frameworks.) You can focus on building the unique features of _your_ application, not on plumbing that's common to all applications.

There are a lot of other powerful, easy-to-use Ruby libraries that have philosophies similar to Rails. Ruby seems to have gathered a community of developers around it who are more about creating value for our users, and less about [bike shedding](https://en.wiktionary.org/wiki/bikeshedding){:target="_blank"} over what to name folders[^choose_boring].

[^choose_boring]:
    I, personally, agree with this author who prefers boring technologies over cutting-edge ones:

    [https://mcfunley.com/choose-boring-technology](https://mcfunley.com/choose-boring-technology){:target="_blank"}

### MINASWAN

The programming world at large can be pretty competitive and harsh, but I've found that the Ruby community is very inclusive and welcoming, which is a relief for beginners. Maybe this can also be traced back to Matz; from [his Wikipedia page](https://en.wikipedia.org/wiki/Yukihiro_Matsumoto){:target="_blank"},

> His demeanor has brought about a motto in the Ruby community: "Matz is nice and so we are nice," commonly abbreviated as MINASWAN.

### Heroku and other integrations

Another way in which the ergonomics of Ruby excels is the amount of tooling that exists around it. For example, [Heroku](https://heroku.com) was the first "platform as a service", a layer on top of Amazon Web Services that makes it astonishingly easy to deploy applications to an industrial-grade infrastructure. Something that would have itself required a whole separate course to learn how to do, we can now do with just one click; and we can focus instead on building features for our users.

Rails was the first platform that Heroku supported, and Rails developers are still Heroku's primary users. There are many other services, everything from performance monitoring to error reporting to email delivery to A/B testing to analytics, that offer tight integrations with Ruby and/or Rails and make using them hyper-productive for small teams, solo developers, and therefore also for us beginners.

## How are we going to go from zero to Hero(ku)?

To go from _complete beginner_ to _deploying a fully functional application_ is quite a bit of ground to cover. Here's how we're going to do it:

### Cloud editors

As you've already seen, we're going to start by writing Ruby right here in your browser. This is the lowest-friction way to get started, and will allow us to focus on learning Ruby without having to waste mental energy on installing tools.

### Make the invisible visible

One of our foundational principles while programming, learning, and almost anything else, is to find a way to **make the invisible visible** — that is, find a way to give yourself concrete feedback on what you're doing. Don't _guess_ what's happening at each step of the program — find a way to _see_ what's happening, if at all possible. **Feedback** is crucial.

#### Printing values

Let's talk a little bit more about how the REPLs work. So far, we've been clicking "run ▶" and seeing the return value of the expression of the _last line_ of our programs in the black window (known as the "terminal") at the bottom; but what if we wanted to see more than just the return value of the last line?

It turns out there's a special method in Ruby called `Kernel.p` (`p` is short for "print inspect") that will display its argument in the terminal. Try running the following:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/first-real-output?lite=true"></iframe>

You usually don't want to print every line, but you certainly should be printing at least one line; otherwise your program isn't doing much good. In the real world (outside of these REPLs), the green line at the end will not show up when we run our programs; only things that we explicitly print will.

Printing values to the terminal is something we're going to want to do _a lot_, and so Ruby makes it easy for us; rather than having to type `Kernel.p`, we can just type the shorthand of `p`. Change `Kernel.p` to `p` above and give it a try.

### Optional parentheses around arguments

To make it even easier to _make the invisible visible_, here's another shortcut:

When we discussed [Arguments](https://chapters.firstdraft.com/chapters/18#arguments-are-inputs){:target="_blank"}, we said that they always come within parentheses. That's not quite true. Ruby allows you to, optionally, leave out the parentheses; so the following will also work:

```ruby
p "HELLO WORLD!"
```

And when you are roaming the internet, you will see this style often, especially with `p`.

My advice to you is: you can drop the parentheses around arguments when you are  using the `p` method; but other than that, include them. They help to keep things clear, and they prevent order-of-operations issues.

### Use p a lot

I like to use `p` _a lot_ while I am programming. As I said, one of my fundamental programming principles is **make the invisible visible** — don't try to _guess_ what's going on, find a way to _see_ what's going on. `p` is an excellent tool for that, so I use `p` like crazy, sometimes before almost every line while I am debugging.

It can be tedious to wrap every line in parentheses, and it's convenient instead to just pop a `p` at the beginning of a line. So in this _one_ case, I give you permission to omit the parentheses around arguments to a method. Enjoy!

### Passing tests

I'm also going to start including challenges in some of the REPLs that you have to pass. This should give you confidence that the code that you've written does the correct thing.

You will have to submit your solutions to these challenges, so you'll have to sign in now using a repl.it account[^replit]:

[^replit]:
    The interactive Ruby playgrounds that are embedded in these readings are hosted on a service called repl.it. If you see a message below asking you to sign up or sign in, then click the <i class="fab fa-github fa-fw"></i> icon to sign in using your GitHub account.

    If you see the message "repl.it refused to connect" after signing in, you might have to refresh this page to get the sandbox to load.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3047597/c3646e3314f9eaa6aedda51dfdc8d59b"></iframe>

Once you completed the challenge, got the tests to pass, and submitted your solution successfully, you should see something like this (the below is a screenshot, not a real REPL):

![](/assets/solution-submitted.png)

After you've submitted a solution, you'll be able to click "See Model Solution" to see one that I've written. Remember that there's no single "correct" solution — if yours worked, then it's just as good as mine!

## Onwards

Okay! With that housekeeping out of the way, let's move on and learn more about the Fundamental Classes of Ruby, starting with [our friend `String`](https://chapters.firstdraft.com/chapters/25).
