---
title: How to use PowerStation
permalink: /powerstation/docs/howtouse/
layout: powerstation
---

<div class="container" markdown="1">
<div class="row" markdown="1">
<div class="col-md-12" markdown="1">

We use a small [blogging project](https://github.com/jwyoung1818/blog) to explain how to use PowerStation.
You can also watch the introduction video(https://www.youtube.com/watch?v=bba_L0_IAuo).

1. Open your project with RubyMine:<br/>
![Open Project](../../screenshots/load_project.png)<br/>

2. Choose any file you want to analyze, and click `PowerStation`->`Analyze` to analyze the file:<br/>
![Analyze](../../screenshots/navigate.png)<br/>

3. Then you can click `PowerStation`->`Show` to check the result. Current version performs six types of analysis (LI, IA, CS, IR, DS and RD, which are explained in [features](../features/)). The problematic code lines are highlighted with blue.

* **Loop Invariant (LI)**:<br/>
![Loop Invariant](../../screenshots/loop_invariant.png)<br/>
Window on the left shows the lines of problematic code (which are also highlighted).

* **Inefficient API (IA)**:<br/>
![Inefficient API highlight](../../screenshots/inefficient_api_before.png)<br/>

When clicking `Fix` on the left-hand-side window, PowerStation fixes the selected lines with a more efficient API. In this case, the `count` is replaced with `size`.<br/>
![Inefficient API fix](../../screenshots/inefficient_api_after.png)<br/>

* **Common Subpexression (CS)**:<br/>
![Common Subexpression](../../screenshots/common_subexpr.png)<br/>

* **Inefficient Rendering (IR)**:<br/>
![Common Subexpression](../../screenshots/inefficient_render.png)<br/>

* **Dead Store Queries (DS)**:<br/>
![Dead Store Queries](../../screenshots/dead_store.png)<br/>

* **Redundant Data Retrieval (RD)**:<br/>
![Redundant Data Retrieval](../../screenshots/redundant_retrieval.png)<br/>
When moving the cursor to the problematic lines, PowerStation shows which fields are being retrieved but not used later.

</div>
</div>
</div>
