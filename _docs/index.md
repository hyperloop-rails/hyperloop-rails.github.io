---
title: Static Checker
permalink: /docs/home/
redirect_from: /docs/index.html
---

Our [static checker](https://github.com/hyperloop-rails/static-checker)
analyzes application's source code, finds all API misuses and
suggests a more efficient API to replace with.
Common API misuse patterns are summarized in our [paper](../../220-HowNotStructure.pdf).
More specifically, the API misuses detected in this checker includes:

1. `any?`, to be replaced with (=>) `exists?`
2. `where.first?` => `find_by`
3. `*` => `*.except(order)`
4. `each.update` => `update_all`
5. `.count` => `size`
6. `.map` => `.pluck`
7. `pluck.sum` => `sum`
8. `.pluck + pluck` => `SQL UNION`
9. `if exists? find else create end` => `find_or_create_by`

#### Prerequisites

For multiline regex expression matching, need to install [pcre link](http://pcre.org/)

#### Steps to run

1. copy `string-matching.sh` to your application's folder
2. run:

```
$ ./string-matching.sh
```

