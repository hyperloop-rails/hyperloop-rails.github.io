---
title: Summary of API misuses
permalink: /docs/api-misuse/
---

Our [static checker](https://github.com/hyperloop-rails/powerstation/tree/master/static-checker/) in powerstation 
finds all API misuses in source code by pattern matching and
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

We list how many times such misuses happen in the applications we studied, and each instance of misuse. The list can be found [here](https://github.com/hyperloop-rails/study-replication/tree/master/API-misuse-summary).

