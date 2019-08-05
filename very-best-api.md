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

### /add_bookmark?input_user_id=1&input_dish_id=2&input_venue_id=3

For example:

```
/add_bookmark?input_user_id=1&input_dish_id=2&input_venue_id=3
```

should add a record to the bookmarks table.

Be sure to name the keys in the query string **exactly** `input_user_id`, `input_dish_id`, and `input_venue_id`.

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

### /dishes?cuisine_id=[ANY EXISTING CUISINE ID]

If a query string is present on the end of `/dishes` with a cuisine ID, use it to filter the list of dishes.

For example, if I know that "Breakfast" has the ID of 2 in the cuisines table, then:

```
/dishes?cuisine_id=2
```

should show only Breakfast dishes.

Be careful not to break plain old `/dishes` (without a query string on the end)! Requests both with and without a query string should be handled gracefully.

There [are](https://emba.firstdraft.com/chapters/14#use-keys-to-explore) [many](https://emba.firstdraft.com/chapters/5#count) [different](https://emba.firstdraft.com/chapters/5#include) [tools](https://emba.firstdraft.com/chapters/14#fetch-fallback) [that](https://emba.firstdraft.com/chapters/14#at-shorthand-) [you](https://emba.firstdraft.com/chapters/2#chaining-wheres) [can](https://emba.firstdraft.com/chapters/18#updating-variables) [use](https://emba.firstdraft.com/chapters/6) to achieve this.

### /venues?neighborhood_id=[ANY EXISTING NEIGHBORHOOD ID]

If a query string is present on the end of `/venues` with a neighborhood ID, use it to filter the list of venues.

For example, if I know that "Hyde Park" has the ID of 10 in the neighborhoods table, then:

```
/venues?neighborhood_id=10
```

should show only the venues in Hyde Park.

### /users/[ANY EXISTING USER ID]/bookmarks?dish_id=[ANY EXISTING DISH ID]

If a query string is present on the end of `/users/[ANY EXISTING USER ID]/bookmarks` with a dish ID, use it to filter the list of bookmarks.

For example, if I know that "Biscuits and Gravy" has the ID of 26 in the dishes table, then:

```
/users/4/bookmarks?dish_id=26
```

should show only the bookmarks that belong to the user with ID 4 that are for Biscuits and Gravy.

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
