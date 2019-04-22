# ApplicationRecord

`ApplicationRecord` is part of Ruby on Rails, and is one of the primary reasons to choose Rails. If you define a class, e.g. `Contact`, and inherit from `ApplicationRecord`:

```ruby
class Contact < ApplicationRecord
end
```

Then _boom_ — our class `Contact` has now inherited[^activerecord] a _tremendous_ number of powerful methods, most of them related to **creating, reading, updating, and deleting records in a database table**.

[^activerecord]: Actually, these methods mostly come from our grandparent class `ActiveRecord::Base` rather than our parent class `ApplicationRecord`. We'll go into this detail later.

We call these database-related classes **models**, and we place them in the `app/models` folder. They talk to the database for us, contain most of our business logic, and are, in many ways, the heart of our applications.

## The shortcut

For the impatient, here's a shortcut. If we wanted to create a table called "contacts" with three columns, `first_name` (string), `last_name` (string), and `date_of_birth` (date); then at a Terminal prompt (**not** within `rails console`), we would run the following command:

```bash
rails generate model contact first_name:string last_name:string date_of_birth:date
```

You'll see some output in the Terminal that looks something like this:

```bash
invoke  active_record
create    db/migrate/20190422125330_create_contacts.rb
create    app/models/contact.rb
```

Then, you would run this command:

```bash
rails db:migrate
```

You'd see some output in the Terminal that looks something like this:

```bash
== 20190422125330 CreateContacts: migrating ===================================
-- create_table(:contacts)
   -> 0.0037s
== 20190422125330 CreateContacts: migrated (0.0037s) ==========================

Annotated (1): app/models/contact.rb
```

That's it. With these two commands, you now have a fully-formed database table _and_ a Ruby class to interact with it (go check out the file that was created at `app/models/contact.rb`).

Incredible! If you want to, you can now skip ahead to the section on [Creating rows](). But, for the curious, here's what just happened, step-by-step:

## Pure Ruby Classes Review

In the old, pure Ruby days, if we wanted to store some attributes about a contact, we would have declared some attribute accessors like this:

```ruby
class Contact
  attr_accessor :first_name
  attr_accessor :last_name
  attr_accessor :date_of_birth
end
```

And then we'd be able to use our class like this:

```ruby
c = Contact.new
c.first_name = gets.chomp
c.last_name = gets.chomp

c.last_name # => "Betina"
```

However, once the program was finished running, whatever data we had stored would be lost. We weren't storing it _permanently_, on the hard drive of the computer; we were only storing it _temporarily_, in the working memory (RAM) of the computer.

## Storing information permanently in databases

If we want to store the information permanently, we need to save it to a file on the hard drive. We could cobble together a way to write this file ourselves using the `File` class, which makes it pretty easy to write and read strings to and from disk; however, there's a _much_ better way: using a database.

**Databases** are programs that have been optimized since the 1970s to be super-efficient at storing and retrieving information on disk. In particular, **relational** databases have been dominant since the 1980s, and use a language called Structured Query Language (SQL) to store and retrieve records from two-dimensional tables. Relational databases power the vast majority of software in use today.

Fortunately for us, we don't need to learn yet another language to use them. All we have to do is inherit from `ApplicationRecord` and our Ruby classes are empowered with methods, like `.save` and `.where`, that will _write all of the SQL for us_. Awesome!

## Creating the table

However, we do first need to actually create the database table, name it, and specify what columns we would like it to have. To create a table, we need to write a special Ruby program known as a **database migration**.

### Database migrations

Database migrations are responsible for evolving the structure of the database, one step at a time. Over the lifecycle of an application, we will make many changes to the database — adding tables here, renaming columns there — as we learn about our users and the problem domain.

For each evolution of the database, we create a little Ruby class that will make the change, and make any modifications to the data already existing within the table (if necessary). This Ruby class will inherit from a base Rails class known as `ActiveRecord::Migration`; which, as you might imagine, will give us a bunch of methods for free that make it very easy to create tables, add columns, etc.

In order to keep the order of the migrations straight, we begin the filenames with a timestamp. To make it easy on us, Rails will create the migration files for us and put the timestamp in the filename automatically if we run this command at a Terminal prompt (**not** inside `rails console`):

```bash
rails generate migration [ChooseANameForTheMigrationClass]
```

The name for the migration class can be anything. Let's start with a silly name, `Zebra`:

```bash
rails generate migration Zebra
```

If you ran this command, the Terminal output would look something like this:

```bash
invoke  active_record
create    db/migrate/20190422123800_zebra.rb
```

We are going to meet many `rails generate ...` commands. All any of the **Rails generators** do is save you some typing; they automate the creation of boilerplate files and code.

In this case, you can see that it created a file for us in the `db/migrate` folder whose name starts with a timestamp and ends in `_zebra.rb`.

If we head over to that file and take a look, we'll see something like this:

```ruby
class Zebra < ActiveRecord::Migration[5.2]
  def change
  end
end
```

The generator saved us some typing by defining our migration class, `Zebra`, and inheriting from the base class `ActiveRecord::Migration` for us.

It also stubbed out the crucial `change` method; this method is what will actually be invoked when we're ready to execute this migration to evolve the database.

Within the `change` method, we can write any Ruby we want; but of course we have a specific mission, to create a table called "contacts" with three columns, `first_name`, `last_name`, and `date_of_birth`.

Fortunately, we inherit methods from `ActiveRecord::Migration` that makes this very easy. First, there's the `create_table` method, which takes a `Symbol` (the name of the table that you want to create) and a block as inputs:

```ruby
class Zebra < ActiveRecord::Migration[5.2]
  def change
    create_table(:contacts) do |table|

    end
  end
end
```

We could, as always, name the block variable anything. I name it `table` because that object represents the new table that's about to be created; and within the block, we can call methods on it like `.string` and `.date` to add columns of those datatypes:

```ruby
class Zebra < ActiveRecord::Migration[5.2]
  def change
    create_table(:contacts) do |table|
      table.string(:first_name)
      table.string(:last_name)
      table.date(:date_of_birth)

      table.timestamps
    end
  end
end
```

Other commonly used datatypes for columns:

```
table.boolean    # true or false
table.date       # Jan 27th
table.datetime   # 7:23pm on Jan 27th
table.decimal    # 42.42
table.integer    # 42
table.string     # Up to 255 characters
table.text       # As many characters as you want
table.time       # 7:23pm
```

The `.timestamps` method will add two columns, `created_at` and `updated_at`, that will be automatically managed by the database. And, of course, every table will have an `id` primary key column that will also be automatically managed by the database; we don't have to say anything explicitly in the migration to get that.

### rails db:migrate

That's it! Our migration file is ready to be run. Look it over one last time, and when you're confident, you can execute it with the following command at a Terminal prompt:

```bash
rails db:migrate
```

This command will look at all of the files in the `db/migrate` folder, examine the timestamps at the beginning of the filenames, look at the current version of the database, and intelligently run only any migrations that have so far not been run.

That means you can re-run the command `rails db:migrate` as many times as you like and it won't harm anything; it won't, for example, try to add the same table twice. That's what the timestamps in the filenames are there to prevent.

Great! We now have a table called "contacts", with columns `first_name`, `last_name`, `date_of_birth`, `created_at`, `updated_at`, and `id`. But how do we actually enter records into this table?

## Models: our translators to the database

Well, one option is to learn Structured Query Language. But let's instead stick with Ruby.

Create a file in the `app/models` folder called `contact.rb` and within it define a class called `Contact`:

```ruby
class Contact
end
```

But, crucially, make it inherit from `ApplicationRecord`:

```ruby
class Contact < ApplicationRecord
end
```

Amazingly, that's all we have to do. We don't have to declare attribute accessors or anything else. In inheriting from `ApplicationRecord`, the `Contact` class will automatically connect to the database, find the table named "contacts", inspect it and see what columns are in it, and define methods that are able to create, read, update, and delete rows, and a whole lot more.

Let's see how it works. In a Terminal tab, let's fire up a `rails console` to experiment with our new model class. Then try the following:

## CREATE

### new

```ruby
Contact.count
```

You'll see that, at present, we have `0` rows in the table. A crucial thing to remember: when you are talking to the **whole table**, you are referencing the _class_ `Contact`, so **use a capital letter**. If you did `contact.count`, what error message would you expect? Try it and see.

Like any Ruby class, we instantiate a new object with `.new`:

```ruby
c = Contact.new
```

As usual we store the new object in a variable so that we can continue to work with it; in this case, we named the variable `c`. This object has attribute setter methods for every column in the table:

```ruby
c.first_name = "Raghu"
c.last_name = "Betina"
```

### save

So far, this is just like a pure Ruby class with `attr_accessor`s. But, crucially, we have the `.save` method now:

```ruby
c.save
```

Now you can just type `c` and it should show you that `c` has been inserted into the database **and it has been assigned an ID number**.

If you exit `rails console`, reboot the computer, and come back into `rails console`, and do `Contact.count`, you will see `1` — _the data will still be there_ (although the variable `c` will not). We have saved it _permanently_.

Add a few more contacts:

```ruby
c = Contact.new
c.first_name = "Minnie"
c.last_name = "Mouse"
c.date_of_birth = "November 18, 1928"
c.save
Contact.count # => 2

c = Contact.new
c.first_name = "Mickey"
c.last_name = "Mouse"
c.date_of_birth = "May 15, 1928"
c.save
Contact.count # => 3
```

Why does it work to re-use the variable `c` here? Well, when we `.save` on the prior one, we've entered the record in to the table and it is assigned an ID. Then throw away the contents of the _variable_ `c` and replace it with a brand new, blank row with `Contact.new` — but that's okay, because the old data is stored on disk and we can always look it up by its ID number if we need it.

## READ

### all

To retrieve all of the rows from a table, you can call `.all` on the class:

```ruby
contact_list = Contact.all
```

The return value of `.all` is an `Array`-like object (technically it's an `ActiveRecord::Relation`, but we can do all of our usual `Array` methods on it — `.each`, etc). From now on I'll refer to these `Array`-like objects as "**collections**".

### count

We've already met `.count`, which tells you how many records are in a collection:

```ruby
contact_list.count
```

### first

```ruby
c = contact_list.first
```

### last

```ruby
c = contact_list.last
```

### Attribute getter methods

However you got it, once you have an individual row stored in a variable, let's call it `c`, then you have a method for each column to retrieve the value for that cell:

```ruby
c.id
c.first_name
c.last_name
c.date_of_birth
c.created_at
c.updated_at
```

What is the return value if you try calling a method for a column that doesn't exist?

```ruby
c.middle_name
```

**RTEM!** You're going to see it _a lot_.

### order

The `.order` method lets you sort your collections by one or more columns. The argument to `.order` is a `Hash`, where the _key_ is the _column_ you want to sort by, and the _value_ is either `:asc` (for ascending order) or `:desc` (for descending order):

```ruby
contact_list.order({ :last_name => :asc })
```

To break ties, you can provide multiple columns in the `Hash`:

```ruby
contact_list.order({ :last_name => :asc, :first_name => :asc, :date_of_birth => :desc })
```

This would first order by last name, then break ties using first name, then break ties using date of birth.

Since collections are `Array`-like, you can use `.sort` or `.sort_by`; but `.order` is _much_ more performant. If at all possible, prefer using `.order` to `.sort` or `.sort_by` (but, at the end of the day, just get the job done one way or another).

### reverse

```ruby
contact_list.reverse
```

### where

Perhaps the most important READ method is `.where`. This is our bread-and-butter tool for _filtering_ a collection of rows down using various criteria.

The argument to `.where` is a `Hash`, where the _key_ is the _column_ you want to filter by, and the _value_ is the criteria you want to filter by:

```ruby
contact_list.where({ :id => 2 })
```

To save us some typing, we can call `.where` directly on the class if we want to, rather than calling `.all` first:

```ruby
Contact.where({ :last_name => "Mouse" })
```

You can provide multiple columns to filter by in the `Hash`:

```ruby
Contact.where({ :last_name => "Mouse", :first_name => "Minnie" })
```

You can also chain `.where`s one after the other:

```ruby
Contact.where({ :last_name => "Mouse" }).where({ :first_name => "Minnie" })
```

##### where always returns a collection, not a single row

**The return value from `.where` is always a collection, regardless of how many results there are.**

Whether there are 0, 1, or a million results, you still have an array. What would you expect if you tried the following?

```ruby
Contact.where({ :id => 2 }).first_name
```

Try it. RTEM!

 **So, use `.first`, `.last`, or `[]` to retrieve an object from the collection if you want to do something with an individual record:**

```ruby
c = Contact.where({ :id => 2 }).first
c.first_name
```

#### UPDATE

To update a row, first you need to locate it:

```ruby
c = Contact.where({ :id => 2 }).first
```

And then assign whatever new values you want to:

```ruby
c.first_name = "Minerva"
```

And then don't forget to save the changes:

```ruby
c.save
```

That's it.

#### Delete

To delete a row, first find it:

```ruby
c = Contact.where({ :id => 2 }).first
```

And then,

```ruby
c.destroy
```

Intense.

### Fuzzy filtering

`.where` can also be used to search for partial matches by passing a fragment of SQL in a string, rather than passing a hash:

    Instructor.where("last_name LIKE '%bet%'")

The `%` are wildcard characters, which match anything in that position.

You can search for rows less than or greater than certain criteria:

    Instructor.where("age > 30")
    Instructor.where("last_name >= 'A' AND last_name <= 'C'")

That last query, for a value within a range, can be more easily written as

    Instructor.where(:last_name => ('A'..'C'))

This is particularly nice for searching for records within a particular range of times:

    start_date = 7.days.ago
    end_date = Date.today
    Instructor.where(:created_at => (start_date..end_date))

Remember, with Ruby Ranges, two dots means inclusive of the second value, and three dots means exclusive of the second value. E.g., `(1..4)` is 1, 2, 3, and 4; `(1...4)` is only 1, 2, and 3.

You can even use an Array in the argument to `.where`; it will then bring back the rows that match ANY of the criteria for that column:

    Instructor.where({ :last_name => ["Betina", "Venkataswamy"] })

Once you've retrieved the right subset of records, you can peel off the values in just one column with `.pluck`:

    Instructor.where(:created_at => (start_date..end_date)).pluck(:first_name)
      # => ["Raghu", "Arjun"]

Other useful query methods are [.order][1], [.limit and .offset][2], and [calculations like .minimum, .maximum, and .average][3].

Much more information about querying can be found at the official Rails Guide:

http://guides.rubyonrails.org/active_record_querying.html


  [1]: http://guides.rubyonrails.org/active_record_querying.html#ordering
  [2]: http://guides.rubyonrails.org/active_record_querying.html#limit-and-offset
  [3]: http://guides.rubyonrails.org/active_record_querying.html#calculations