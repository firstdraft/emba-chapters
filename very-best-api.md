# Very Best API

## Your target

You can see [a functional version of the application you should build here](https://very-best-api.herokuapp.com/users).

## The domain model

Here's an [Entity Relationship Diagram](https://www.lucidchart.com/pages/er-diagrams#discovery__top){:target="_blank"} of the database, for reference. Also remember to refer to the comments at the top of the model files:

![](/assets/very-best-api-erd.png)

## API endpoints you need to build

Unless otherwise noted, **if multiple rows are being displayed, they should be ordered by the `created_at` column (ascending).**

### /cuisines

```
/cuisines
```

should show a list all cuisines.

### /dishes

```
/dishes
```

should show a list all dishes.

### /dishes/[ANY EXISTING DISH ID]

For example:

```
/dishes/26
```

should show the details of one dish.

### /neighborhoods

```
/neighborhoods
```

should show a list of all neighborhoods.

### /venues

```
/venues
```

should show a list of all venues.

### /venues/[ANY EXISTING VENUE ID]

For example:

```
/venues/27
```

should show the details of one venue.

### /users

```
/users
```

should show a list all users.

### /users/[ANY EXISTING USER ID]

For example:

```
/users/4
```

should show the details of one user.

### /users/[ANY EXISTING USER ID]/bookmarks

For example:

```
/users/4/bookmarks
```

should show a list of the bookmarks that belong to the user with ID 4.

### /dishes/[ANY EXISTING DISH ID]/bookmarks

For example:

```
/dishes/5/bookmarks
```

should show a list of the bookmarks that belong to the dish with ID 5.

### /venues/[ANY EXISTING VENUE ID]/bookmarks

For example:

```
/venues/6/bookmarks
```

should show a list of the bookmarks that belong to the venue with ID 6.

### /remove_bookmark/[ANY EXISTING BOOKMARK ID]

For example:

```
/remove_bookmark/32
```

should delete a record from the bookmarks table.

### /users/[ANY EXISTING USER ID]/bookmarked_dishes

For example:

```
/users/4/bookmarked_dishes
```

should show a list of the user's bookmarked dishes.

### /dishes/[ANY EXISTING DISH ID]/experts

For example:

```
/dishes/5/experts
```

should show a list of the venues that have been bookmarked for the dish.

### /venues/[ANY EXISTING VENUE ID]/specialties

For example:

```
/venues/6/specialties
```

should show a list of the dishes that have been bookmarked at a venue.

### /venues/[ANY EXISTING VENUE ID]/fans

For example:

```
/venues/6/fans
```

should show a list of the users that have bookmarked any dish at the venue.

## Query helper methods that you might find helpful

I found it helpful to define the following [association query shortcuts](https://emba.firstdraft.com/chapters/3) for myself in my models before I started to build out my API; but you are not required to.

 - `User#bookmarks`
 - `User#bookmarked_dishes`
 - `User#bookmarked_venues`
 - `User#bookmarked_neighborhoods`
 - `Venue#neighborhood`
 - `Venue#bookmarks`
 - `Venue#specialties`
 - `Venue#fans`
 - `Neighborhood#venues`
 - `Dish#bookmarks`
 - `Dish#cuisine`
 - `Dish#experts`
 - `Dish#fans`
 - `Cuisine#dishes`
 - `Cuisine#experts`
 - `Bookmark#dish`
 - `Bookmark#venue`
 - `Bookmark#user`

For me, it's worth the up-front investment to write these instance methods and have them at my fingertips for the rest of the time I spend working on the application, because most association-related queries are re-used many times.

### Note: .distinct

Remember that you can [use `.distinct` on a collection](https://emba.firstdraft.com/chapters/2#distinct) to remove duplicate records.
