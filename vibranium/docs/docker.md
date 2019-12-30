---
title: How to run Vibranium on docker image 
permalink: /vibranium/docs/docker
layout: vibranium 
---

<div class="container" markdown="1">
<div class="row" markdown="1">
<div class="col-md-12" markdown="1">

We provide a docker image that contains Panorama and all the applications we use in the paper.
Below are steps to run Panorama and reproduce the experiments on docker.

### Pull and run the docker image 
* Login to you docker hub account and pull the [docker image](https://hub.docker.com/repository/docker/managedataconstraints/data-constraints-analyzer):
```
$ docker pull managedataconstraints/data-constraints-analyzer
```


### Reproduce experiments in Section VII.

* Figure2 

```
$ cd main278/formatchecker/ 
$ ruby run_app.rb --tva
```

The data is stored in the [excel file](http://bit.ly/app-versions-vs-constraint-changes).


</div>
</div>
</div>
