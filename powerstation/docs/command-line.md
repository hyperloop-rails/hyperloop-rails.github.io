---
title: Powerstation command line tool
permalink: /powerstation/docs/command-line/
layout: powerstation_smallfont
---

<div class="container" markdown="1">
<div class="row" markdown="1">
<div class="col-md-12" markdown="1">

This is the documentation for powerstation command-line verion ([source code](https://github.com/hyperloop-rails/powerstation)).
It performs program analysis to find out performance anti-patterns.
The analysis contains two parts. The first finds the common API misuses by simply matching the patterns in the source code. The second requires program analysis, constructing program flow graphs to find out the pattern.

## [Static checker for API misuses](https://github.com/hyperloop-rails/powerstation/tree/master/static-checker):


##### Prerequisites:

Install [pcre](http://pcre.org/).

##### Steps to run:

* To use command line version of this checker, simply run
```
$./string-matching.sh
```

* To use the java version, run
```
$java -jar StaticChekcer.jar folder
```
and the result is dumped into "inefficientAPI.xml"

## [Program analyzer](https://github.com/hyperloop-rails/powerstation/tree/master/powerstation/command_line_tool):

This analyzer able to detect non-API-related patterns as listed in [features](../features).

Besides the anti-patterns, it also dumps a `stats.xml` which shows query-related statistics, for instance, 
+ the number of possibly-issued read/write queries, 
+ the number of queries in loop, 
+ the source of query parameters (from constant values, user inputs, other queries, etc)
+ the use of query result (shown on webpage, as parameter to other queries, as branching conditions, etc)
+ the number of queries return limited/unbounded results
+ the size of redundantly-retrieved data (when query issues `select *` instead of projecting fields that are used)
+ ...

These statistics will provide a sense of possible performance problems (and how serious they are) in each action. For more details, checkout our [database-related study](../../study_db.pdf).

##### Prerequisites

1. Get the [jruby for orm](https://github.com/congy/jruby_for_orm) and install following the instructions. You can also download the compioled jruby from [compiled_jruby](https://github.com/hyperloop-rails/compiled-jruby) and configure your environment path as follows:

* after cloning this repo, go to the directory and set `JRUBY_PATH`. Also make sure `JAVA_HOME` is properly set.
```
$ cd compiled-jruby
$ export JRUBY_PATH=`pwd`/bin
```

* then export the jruby path to your environment PATH
```
$ export PATH=$JRUBY_PATH:$PATH
```

2. Deploy the application and make sure it runs successfully.

3. Install [yard](https://github.com/lsegal/yard.git) by:
```
$ gem install yard
```
Yard is used as ruby file parser.


##### Steps to run

1. Create directory and get the list of actions.

* create a folder for your application under the `applications/` folder:
```
$ cd static-analyzer/
$ cd applications/
$ mkdir APP_NAME/
$ cd ..
```
Now we assume `APP_DIR` to be `path-to-static-analyzer/applications/APP_NAME`

* go to your application's directory, run:
```
$rake routes | tail -n +2 | awk '{ for (i=1;i<=NF;i++) if (match($i, /.#./)) print $i}' | sed -e 's/#/,/g' | sort | uniq
```
to get the list of actions that can be invoked by the app, copy it to `APP_DIR/calls.txt`


2. Replace view rendering calls with ruby code in view files.
Currently we only handle *.erb* file (.haml files might also work, but with little confidence...)

* MAKE A COPY of your application folder:
```
$ cp -r APP_FOLDER/ NEW_APP_FOLDER/
$ cd NEW_APP_FOLDER/
```

* make sure the following file exists:
```
$ ls db/schema.rb
$ ls config/routes.rb
```

* run the script to replace view rendering calls in controllers with all ruby code extracted from view files (mainly .erb files), and replace `ANALYZER_APP_PATH` with the path where the analyzer stores the application, e.g., `path_to_static-analyzer/applications/APP_NAME`:
```
$ ruby main.rb -a $APP_NAME
```

If `app/helpers` exists, all view rendering calls in helpers will be replaced too.

* copy other folders to `ANALYZER_APP_PATH`, if ruby file exists in those folders, for example:
```
$ cp app/mailers/ ANALYZER_APP_PATH/mailers/
```

Then we get all the files needed for static analysis.


3. Use jruby to get dataflow and control flow: 
```
$ cd path-to-static-analyzer/applications/
$ python generate_dataflow_log.py  APP_NAME
```

4. Run the static analysis:
```
$ cd controller_model_analysis
$ ruby main.rb -d DIR_TO_APP_FILES -s CONTROLLER_NAME,ACTION_NAME
```

For example,
```
$ruby main.rb -s -d ../applications/forem PostsController,index
```

You may run
```
$ruby main.rb --help
```
to get the options


* If you wish to run all actions from an application, do:
```
$ruby main.rb -a -d DIR_TO_APP_FILES
```

##### Analysis on Open-source Applications:

There are already many applications that are preprocessed and ready to be analyzed by this tool. 
These apps are under the `applications/` directory.


