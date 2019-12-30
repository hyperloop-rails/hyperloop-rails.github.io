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


### Reproduce experiments in the paper.

* Figure2 
```
$ cd main278/formatchecker/ 
$ ruby run_app.rb --tva
```
The data is stored in the [excel file](http://bit.ly/app-versions-vs-constraint-changes).

* Table 4
```
$ ruby run_app.rb  --latest-version
```
The data is presented in http://bit.ly/data-constraints-in-web-applications under the `latest-version #constraints` tab. 

* Table 5

* Table 6 

* Table 7:  Top 5 popular types of different layer
Go to the `main278/formatchecker/` script folder and run:
``` 
$ ruby run_app.rb  --api-breakdown
```
This will generate a single log file for each application under ```log/api_breakdown_#{app_name}.log```
```
$ ruby api_breakdown_spread_sheets.rb 
```
The summarized breakdown will be written to output/api_total_breakdown.xlsx. 

Details presented in the `summary` tab of  http://bit.ly/top-5-popular-types-of-different-layers 

* Table 8: app versions vs constraint changes


Details presented in the `constraint-evolution` tab of http://bit.ly/app-versions-vs-constraint-changes 

Reproduce: go to the `main278/formatchecker/` script folder and run:

```
$ ruby run_app.rb --tva 
```

</div>
</div>
</div>
