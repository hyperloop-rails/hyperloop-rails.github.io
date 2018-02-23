---
title: Static Analyzer
permalink: /docs/static-analyzer/
---

Our [static analyzer](https://github.com/hyperloop-rails/static_analyzer)
reads the application code,
analyzes the control flow and data flow, and identifies database-query-related
performance inefficiencies listed in both our [database study](../../study_db.pdf)
and [program practice study](../../220-HowNotStructure.pdf).

Currently this tool is dumping analysis result into files. We are working
on an easier-to-understand wrapup to present the result.
It is able to detect the following database-related ineffiency patterns:

1. Loop invariants (i.e., redundant computations/queries in loop)
2. redundant field retrieval, e.g., issuing a `SELECT *` query but only a few fields are used
3. redundant table retrieval, e.g., issuing a `SELECT * FROM A JOIN B` query but only table A is being used
4. the query that might return unlimited tuples
5. the query that only uses other query's result as parameters
6. the queries that share subexpressions
7. the constant predicates in queries (i.e., predicate that does not take dynamic user input as parameters)
8. ...

To turn on/off any of the above detections, modify the `controller_model_analysis/compute_switch.rb` file.

#### Prerequisites

1. Get the [jruby for orm](https://github.com/congy/jruby_for_orm) and install following the instructions.

2. Deploy the application and make sure it runs successfully.

#### Steps to run (TBD)


1. Preprocess.

* Copy the ruby files to some directory APPDIR, under the `applications/` folder:
```
$cd applications/
$mkdir APPNAME/
$cd ..
```

* Go to your application's directory, run:
```
$rake routes | tail -n +2 | awk '{ for (i=1;i<=NF;i++) if (match($i, /.#./)) print $i}' | sed -e 's/#/,/g' | sort | uniq
```
to get the list of actions that can be invoked by the app, copy it to `APPDIR/calls.txt`

* Follow `preprocess_views/README.md` to extract the code from view.

If there are only .erb files in views, it is probably fine. But if you are using haml, the convertion from haml to erb may run into some trouble.

* Copy schema.rb in your app to `APPDIR/`.

* Use installed jruby to generate dataflow log. Checkout `static_analyzer/applications/generate_dataflow_log.py` and applications under `applications/` folder for more details. 
```
$cd applications/
$ruby main.rb -a APPNAME
```

2. Run the static analysis:

* 
```
$cd controller_model_analysis
$ruby main.rb --help
```
to get the options

* Basically, to get the statistics and program flow of a specific action from an app, you would like to run:
```
$ruby main.rb -d DIR_TO_APP_FILES -s CONTROLLER_NAME,ACTION_NAME
```

For example,
```
$ruby main.rb -s -d ../applications/forem PostsController,index
```

The `instructions` in the form of [JRuby intermediate representation](https://www.infoq.com/news/2009/11/jruby-ir) that will be executed by an action, together with the data dependencies among these instructions,  will be printed to the screen.

* If you wish to run all actions from an application, do:
```
$ruby main.rb -a -d DIR_TO_APP_FILES
```

Results will be saved to the folder under the app directory.
The folder name is defined under global.rb, and by default is `result/`.

#### Analysis on Open-source Applications:

There are already many applications that are preprocessed and ready to be analyzed by this tool. 
These apps are under the `applications/` directory.


