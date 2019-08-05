# draft_generators

## Installation

If it's not already there, add this line to your `Gemfile`, which you'll find in
the root folder of your app:

```ruby
# /Gemfile

gem "draft_generators", :git => "https://github.com/firstdraft/draft_generators", :branch => "spring-2019"
```

Then, at a Terminal prompt:

```bash
bundle install
```

If you are starting with the [base final project repository](https://github.com/appdev-projects/final-project), then you've already got the gem included.

## Generating a resource

To generate a complete, database-backed Golden Seven web resource:

```bash
rails generate draft:resource <MODEL_NAME_SINGULAR> <COLUMN_1_NAME>:<COLUMN_1_DATATYPE> <COLUMN_2_NAME>:<COLUMN_2_DATATYPE> # etc
```

For example,

```bash
rails generate draft:resource photo image:string caption:text owner_id:integer
```

- The model name must be singular.
- Separate column names and datatypes with colons (NO SPACES).
- Separate name:datatype pairs with spaces (NO COMMAS).

In other words, the format of the command is exactly the same as when you were [generating only a model and table](https://emba.firstdraft.com/chapters/2#the-draftmodel-generator-does-everything), but `draft:model` is replaced with `draft:resource`.

> Note: `rails g` is short for `rails generate`, like  `c` is for `console` and `s` is for `server`.

As usual, we have to run the command `rails db:migrate` at a Terminal prompt to execute the migrations that were written for us by the generator, to actually create the new table.

### What just happened?

Whoa, what just happened? Well, let's look at the output from the command:

```bash
create  app/controllers/photos_controller.rb
invoke  active_record
create    db/migrate/20190529135532_create_photos.rb
create    app/models/photo.rb
create  app/admin/photos.rb
insert  app/admin/photos.rb
create  app/views/photo_templates
create  app/views/photo_templates/list.html.erb
create  app/views/photo_templates/blank_form.html.erb
create  app/views/photo_templates/prefilled_form.html.erb
create  app/views/photo_templates/details.html.erb
create  app/views/photo_templates/_validation_failures.html.erb
 route  RESTful routes
insert  config/routes.rb
```

 - A model and a migration for the table.
 - A controller named after the table.
 - Some view templates related to the table.
 - Registering the table with the ActiveAdmin dashboard.
 - Some routes related to the table.

If you look at the routes, controller, and views, you'll see that it has essentially added in all of the standard Golden Seven boilerplate for us — automatically! Try navigating to `/photos`, or whatever your table is called, and you'll see that you have a fully-functional interface to CRUD records. Awesome!

This makes sense because, as you might have noticed by now, the Golden Seven RCAVs relating to letting users Create, Read, Update, and Delete records through their browser are basically the same for every table; only the form inputs vary, really, based on the different columns in the tables.

And since we're going to need most of the Golden Seven for most of our tables, why not automate the boilerplate? Then we can just delete the stuff that we don't want, and modify what's left to match our needs.

Whew! What a time saver. However, it only saves time if you are comfortable with all of the code that it is writing for you — otherwise the code is intimidating and you are scared to change it. Then, rather than saving you time, the generators slow you down; relative to you writing code by hand that you understand 💯. So choose your approach wisely. When in doubt, pick a URL that you want the user to be able to visit, connect the RCAV dots yourself, and make it happen.

### What if I made a mistake when I generated a resource?

#### If you've already run rails db:migrate

If you made a mistake when generating your `draft:resource` AND you've _already_ run `rails db:migrate`, then first you have to run the command:

```bash
rails db:rollback
```

to get the database back into the state that it was in before. The command looks at the most recent migration and reverses it. If you haven't run `rails db:migrate` yet, then you can proceed to `rails destroy`, below.

#### rails destroy draft:resource

Anything that you can `rails generate`, you can also `rails destroy`; the latter command will undo whatever the former one does. So, for example, you can:

```bash
rails destroy draft:resource photo
```

This will remove all the files that `rails generate draft:resource photo ...` had previously added. You can then correct your error and re-generate from scratch.

## A better Application Layout file

To generate an application layout file that includes links to Bootstrap, Font Awesome, and some boilerplate markup for a nav bar and alert messages, run the command

```bash
rails g draft:layout <THEME>
```

`<THEME>` can either be blank, or the name of any [Bootswatch](http://bootswatch.com) (downcase), e.g., `cerulean`.

It will warn you that it is going to overwrite your existing `app/views/layouts/application.html.erb` -- say yes if you are sure that's okay. Copy out any important stuff if necessary, to be re-pasted back in.

## Making Changes

That's it! You now have a solid starting point to work from.

If you decide, however, to [add a new column](https://emba.firstdraft.com/chapters/2#adding-or-removing-columns-from-your-table), then you'll have to go in and edit all of the relevant files (forms, processing actions) by hand.

There's no magic to this generator; all it did was automate the tedium of building The Golden Seven boilerplate by hand. From here on out, it's up to you to add RCAVs and make the interface match your specifications.
