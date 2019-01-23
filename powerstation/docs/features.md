---
title: Powerstation Features
permalink: /powerstation/docs/features/
layout: powerstation_smallfont
---

<div class="container" markdown="1">
<div class="row" markdown="1">
<div class="col-md-12" markdown="1">

We use a small [blogging project](https://github.com/jwyoung1818/blog) to explain the features of PowerStation, i.e., the types of performance issues that it discovers. 

#### Loop Invariant Query

Loop invariant query is the query that is inside a loop, but remains the same in every loop iteration. For example,
```
[1].each do
  user = User.first
  user.blog.size
  ...
end
```
In this piece of code, the query `User.first` and `user.blog.size` are issued in every loop iteration without changing any parameters.
Usually there is no need to execution the same query multiple times, especially when the query is slow. To fix this issue, you can move such query out of the loop:
```
user = User.first
user.blog.size
[1].each do
  ...
end
```

#### Inefficient API

Rails provides many APIs to interact with databases, but some APIs issue more efficient queries than others. Powerstation uses a [static checker](https://hyperloop-rails.github.io/docs/home/) to loop for inefficient API uses and suggests replacement APIs. For instance,
```
user.where(:name=?).any?
``` 
The above code issues a query to check whether there exists a user with specific name. The `any?` API issues a `COUNT` query and return a boolean. However, replacing it with 
```
user.where(:name=?).exists?
``` 
is more efficient since the `exists?` issues a `LIMIT 1` query which usually runs faster than `COUNT`.

Following is the list of APIs PowerStaion checks and their replacements:

+ `any?`, to be replaced with (=>) `exists?`
+ `where.first?` => `find_by`
+ `*` => `*.except(order)`
+ `each.update` => `update_all`
+ `.count` => `size`
+ `.map` => `.pluck`
+ `pluck.sum` => `sum`
+ `.pluck + pluck` => `SQL UNION`
+ `if exists? find else create end` => `find_or_create_by`


#### Common Subpexression

Some queries share common subexpressions. For example,

```
user.blogs.size
user.blogs
```

This code piece issues two queries: `SELECT COUNT(*) from blogs where user_id = ?` and `SELECT * from blogs where user_id = ?`. They share the subexpression `where user_id = ?` (with the same parameter). Rewritten these queries to compute the common subexpression first may accelerate the queries. In this example, one can rewrite the code as:

```
@u = user.blogs
@u.count
```

Only one query is issued, while in the second line `u.count` computes the count
from the data already loaded in memory.

#### Inefficient Rendering

Rails provides many handy APIs to render objects, but some of them are not very efficient, for example,

```
@users = user.all
for u in @users:
  link_to u.id
```

The `link_to` is a ruby function that takes into a user id and generates a url link for that id. Such rendering functions are usually non-trivial. However, after figure out one link, the rest url links can just simply replace the `id` in the first link with another `id`:

```
@users = user.all
one_link = link_to @users[0].id
for u in @users:
  one_link.replace(@users[0].id, u.id)
```

Since string replacement is much faster that `link_to` rendering, if there are a great number of objects to render, using string replacement would be much more efficient. However, such change may harm the code readability, so you may only want to fix it when rendering is the bottleneck.

#### Dead Store Query

Dead store query refer to the query is repeatedly issued to load different database contents into the same memory object while the object has not been used between the reloads. For example,
```
blog.reload
blog.reload
render blog
```
where `blog.reload` issues a query. However, in between the too `blog.reload `, there is no reference to blog, there is no need to reload them again. Removing the first reload reduces unnecessary computation:
```
blog.reload
render blog
```

#### Redundant Data Retrieval

By default the queries issued by Rails loads the full tuple from the database into objects unless specified which fields to load specifically. Usually only a few fields are used, and loading a full tuple is unnecessary. For example,
```
blog = Blog.all.order('created_at')
link_to blog.id
```
Only the blog id is used later in the program to produce url links. However, the query loads the full object. A more efficient way is to project fields that are later used:
```
blog = Blog.all.order('created_at').select('id')
link_to blog.id
```
