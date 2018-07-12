---
title: Installing PowerStation
permalink: /powerstation/docs/install/
layout: powerstation
---

<div class="container" markdown="1">
<div class="row" markdown="1">
<div class="col-md-12" markdown="1">

Steps to install Powerstation:

1. Download the compiled jruby from [compiled_jruby](https://github.com/hyperloop-rails/powerstation/tree/master/powerstation/lib/compiled-jruby) and configure your environment path as follows:
```
$ cd compiled-jruby
$ export JRUBY_PATH=`pwd`/bin
$ export PATH=$JRUBY_PATH:$PATH
```
Make sure the `export` commands are also saved to `~/.bashrc`.

2. Install [yard](https://github.com/lsegal/yard.git) and [activesupport](https://github.com/rails/rails/tree/master/activesupport):
```
$ gem install yard
$ gem install activesupport
$ gem install work_queue
```
 If your view file is written in haml, you also need to install python3 and bs4.

3. Download and install [RubyMine](https://www.jetbrains.com/ruby/).

4. Download [powerstation](https://plugins.jetbrains.com/plugin/10604-powerstation).

5. Open RubyMine and go to `preference`->`plugin`, then click `Install plugin from disk`, and select the powerstation jar file just downloaded.<br/>
![Install plugin](../../screenshots/load_plugin.png)

6. The tool needs to know all entrance controller actions from your application. It assumes them to be stored in a file called `calls.txt`. You can generate that file by running:
  ```
  $rake routes | tail -n +2 | awk '{ for (i=1;i<=NF;i++) if (match($i, /.#./)) print $i}' | sed -e 's/#/,/g' | sort | uniq
  ```
  in your app, and then copying it to `<APPDIR>/calls.txt`.

7. Open your project with RubyMine and you're ready to go!

</div>
</div>
</div>
