---
title: How to use PowerStation
permalink: /powerstation/docs/howtouse/
layout: powerstation
---

<div class="container" markdown="1">
<div class="row" markdown="1">
<div class="col-md-12" markdown="1">

We use a small [blogging project](https://github.com/jwyoung1818/blog) to explain how to use PowerStation.

1. Open your project with RubyMine:
![Open Project](../../screenshots/load_project.png)

2. Choose any file you want to analyze, and click `PowerStation`->`Analyze` to analyze the file:
![Analyze](../../screenshots/navigate.png)

3. Then you can click `PowerStation`->`Show` to check the result. Current version performs six types of analysis (LI, IA, CS, IR, DS and RD, which are explained in [features](../features/)). The problematic code lines are highlighted with blue.

* Loop Invariant (LI):
![Loop Invariant](../../screenshots/loop_invariant.png)
Window on the left shows the lines of problematic code (which are also highlighted).

* Inefficient API (IA):
![Inefficient API highlight](../../screenshots/inefficient_api_before.png)

When clicking `Fix` on the left-hand-side window, PowerStation fixes the selected lines with a more efficient API. In this case, the `count` is replaced with `size`.
![Inefficient API fix](../../screenshots/inefficient_api_after.png)

* Common Subpexression (CS):
![Common Subexpression](../../screenshots/common_subexpression.png)

* Inefficient Rendering (IR):
![Common Subexpression](../../screenshots/inefficient_render.png)

* Dead Store Queries (DS):
![Dead Store Queries](../../screenshots/dead_store.png)

* Redundant Data Retrieval (RD):
![Redundant Data Retrieval](../../screenshots/redundant_retrieval.png)
When moving the cursor to the problematic lines, PowerStation shows which fields are being retrieved but not used later.

</div>
</div>
</div>