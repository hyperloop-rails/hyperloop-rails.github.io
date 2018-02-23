---
title: Static Analyzer
permalink: /docs/static-analyzer/
---

Our [static analyzer](https://github.com/hyperloop-rails/static-analyzer)
reads the application code,
analyzes the control flow and data flow, and identifies database-query-related
performance inefficiencies listed in both our [database study](../../study_db.pdf)
and [program practice study](../../220-HowNotStructure.pdf).

Currently this tool is dumping analysis result into files. We are working
on an easier-to-understand wrapup to present the result.
It is able to detect the following database-related ineffiency patterns:

1. Loop invariants (i.e., redundant queries in loop, and the output includes the detailed information such as the location of the invariant query, the start, and end location of the loop, output is the loop_invariant.xml file)
2. the dead store queries (i.e., there is no referrence to the object between two reloads, the position of the dead store query will be output to the dead_store.xml file)

TODO:
1. inefficient partial rendering
2. common subexpression sharing
3. redundant table retrieval, e.g., issuing a `SELECT * FROM A JOIN B` query but only table A is being used
4. the query that might return unlimited tuples
5. the query that only uses other query's result as parameters
6. the queries that share subexpressions
7. the constant predicates in queries (i.e., predicate that does not take dynamic user input as parameters)
8. redundant field retrieval, e.g., issuing a `SELECT *` query but only a few fields are used

#### Prerequisites

1. Get the [jruby for orm](https://github.com/congy/jruby_for_orm) and install following the instructions. You can also download the compioled jruby from [compiled_jruby](https://github.com/hyperloop-rails/compiled-jruby) and configure your environment path as follows:

     * after cloning compiled_jruby repo, go to the directory, and get the path of jruby called JRUBY_PATH
     ```
     JRUBY_PATH=`pwd`
     ```
     * export the jruby path to your environment PATH
     ```
     export PATH=$JRUBY_PATH:$PATH
     ```

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
$python generate_dataflow_log.py  APPNAME
```

2. Run the static analysis:

* Go to the controller\_model\_analysis
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


