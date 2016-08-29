---
title: UDFNode
note: Auto generated by tickdoc

menu:
  kapacitor_1_0:
    name: UDF
    identifier: u_d_f_node
    weight: 220
    parent: nodes
---

A [UDFNode](/kapacitor/v1.0/nodes/u_d_f_node/) is a node that can run a User Defined Function (UDF) in a separate process. 

A UDF is a custom script or binary that can communicate via Kapacitor&#39;s UDF RPC protocol. 
The path and arguments to the UDF program are specified in Kapacitor&#39;s configuration. 
Using TICKscripts you can invoke and configure your UDF for each task. 

See the [README.md](https://github.com/influxdata/kapacitor/tree/master/udf/agent/) 
for details on how to write your own UDF. 

UDFs are configured via Kapacitor&#39;s main configuration file. 

Example: 


```javascript
    [udf]
    [udf.functions]
        # Example moving average UDF.
        [udf.functions.movingAverage]
            prog = "/path/to/executable/moving_avg"
            args = []
            timeout = "10s"
```

UDFs are first class objects in TICKscripts and are referenced via their configuration name. 

Example: 


```javascript
     // Given you have a UDF that computes a moving average
     // The UDF can define what its options are and then can be
     // invoked via a TICKscript like so:
     stream
         |from()...
         @movingAverage()
             .field('value')
             .size(100)
             .as('mavg')
         |httpOut('movingaverage')
```

NOTE: The UDF process runs as the same user as the Kapacitor daemon. 
As a result make the user is properly secured as well as the configuration file. 


Index
-----

### Properties

-	[UDFName](/kapacitor/v1.0/nodes/u_d_f_node/#udfname)

### Chaining Methods

-	[Alert](/kapacitor/v1.0/nodes/u_d_f_node/#alert)
-	[Bottom](/kapacitor/v1.0/nodes/u_d_f_node/#bottom)
-	[Combine](/kapacitor/v1.0/nodes/u_d_f_node/#combine)
-	[Count](/kapacitor/v1.0/nodes/u_d_f_node/#count)
-	[Deadman](/kapacitor/v1.0/nodes/u_d_f_node/#deadman)
-	[Default](/kapacitor/v1.0/nodes/u_d_f_node/#default)
-	[Delete](/kapacitor/v1.0/nodes/u_d_f_node/#delete)
-	[Derivative](/kapacitor/v1.0/nodes/u_d_f_node/#derivative)
-	[Distinct](/kapacitor/v1.0/nodes/u_d_f_node/#distinct)
-	[Elapsed](/kapacitor/v1.0/nodes/u_d_f_node/#elapsed)
-	[Eval](/kapacitor/v1.0/nodes/u_d_f_node/#eval)
-	[First](/kapacitor/v1.0/nodes/u_d_f_node/#first)
-	[Flatten](/kapacitor/v1.0/nodes/u_d_f_node/#flatten)
-	[GroupBy](/kapacitor/v1.0/nodes/u_d_f_node/#groupby)
-	[HoltWinters](/kapacitor/v1.0/nodes/u_d_f_node/#holtwinters)
-	[HoltWintersWithFit](/kapacitor/v1.0/nodes/u_d_f_node/#holtwinterswithfit)
-	[HttpOut](/kapacitor/v1.0/nodes/u_d_f_node/#httpout)
-	[InfluxDBOut](/kapacitor/v1.0/nodes/u_d_f_node/#influxdbout)
-	[Join](/kapacitor/v1.0/nodes/u_d_f_node/#join)
-	[Last](/kapacitor/v1.0/nodes/u_d_f_node/#last)
-	[Log](/kapacitor/v1.0/nodes/u_d_f_node/#log)
-	[Max](/kapacitor/v1.0/nodes/u_d_f_node/#max)
-	[Mean](/kapacitor/v1.0/nodes/u_d_f_node/#mean)
-	[Median](/kapacitor/v1.0/nodes/u_d_f_node/#median)
-	[Min](/kapacitor/v1.0/nodes/u_d_f_node/#min)
-	[Percentile](/kapacitor/v1.0/nodes/u_d_f_node/#percentile)
-	[Sample](/kapacitor/v1.0/nodes/u_d_f_node/#sample)
-	[Shift](/kapacitor/v1.0/nodes/u_d_f_node/#shift)
-	[Spread](/kapacitor/v1.0/nodes/u_d_f_node/#spread)
-	[Stats](/kapacitor/v1.0/nodes/u_d_f_node/#stats)
-	[Stddev](/kapacitor/v1.0/nodes/u_d_f_node/#stddev)
-	[Sum](/kapacitor/v1.0/nodes/u_d_f_node/#sum)
-	[Top](/kapacitor/v1.0/nodes/u_d_f_node/#top)
-	[Union](/kapacitor/v1.0/nodes/u_d_f_node/#union)
-	[Where](/kapacitor/v1.0/nodes/u_d_f_node/#where)
-	[Window](/kapacitor/v1.0/nodes/u_d_f_node/#window)

Properties
----------

Property methods modify state on the calling node.
They do not add another node to the pipeline, and always return a reference to the calling node.
Property methods are marked using the `.` operator.


### UDFName

```javascript
node.uDFName(value string)
```


Chaining Methods
----------------

Chaining methods create a new node in the pipeline as a child of the calling node.
They do not modify the calling node.
Chaining methods are marked using the `|` operator.


### Alert

Create an alert node, which can trigger alerts. 


```javascript
node|alert()
```

Returns: [AlertNode](/kapacitor/v1.0/nodes/alert_node/)


### Bottom

Select the bottom `num` points for `field` and sort by any extra tags or fields. 


```javascript
node|bottom(num int64, field string, fieldsAndTags ...string)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### Combine

Combine this node with itself. The data is combine on timestamp. 


```javascript
node|combine(expressions ...ast.LambdaNode)
```

Returns: [CombineNode](/kapacitor/v1.0/nodes/combine_node/)


### Count

Count the number of points. 


```javascript
node|count(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### Deadman

Helper function for creating an alert on low throughput, aka deadman&#39;s switch. 

- Threshold -- trigger alert if throughput drops below threshold in points/interval. 
- Interval -- how often to check the throughput. 
- Expressions -- optional list of expressions to also evaluate. Useful for time of day alerting. 

Example: 


```javascript
    var data = stream
        |from()...
    // Trigger critical alert if the throughput drops below 100 points per 10s and checked every 10s.
    data
        |deadman(100.0, 10s)
    //Do normal processing of data
    data...
```

The above is equivalent to this 
Example: 


```javascript
    var data = stream
        |from()...
    // Trigger critical alert if the throughput drops below 100 points per 10s and checked every 10s.
    data
        |stats(10s)
            .align()
        |derivative('emitted')
            .unit(10s)
            .nonNegative()
        |alert()
            .id('node \'stream0\' in task \'{{ .TaskName }}\'')
            .message('{{ .ID }} is {{ if eq .Level "OK" }}alive{{ else }}dead{{ end }}: {{ index .Fields "emitted" | printf "%0.3f" }} points/10s.')
            .crit(lambda: "emitted" <= 100.0)
    //Do normal processing of data
    data...
```

The `id` and `message` alert properties can be configured globally via the &#39;deadman&#39; configuration section. 

Since the [AlertNode](/kapacitor/v1.0/nodes/alert_node/) is the last piece it can be further modified as normal. 
Example: 


```javascript
    var data = stream
        |from()...
    // Trigger critical alert if the throughput drops below 100 points per 10s and checked every 10s.
    data
        |deadman(100.0, 10s)
            .slack()
            .channel('#dead_tasks')
    //Do normal processing of data
    data...
```

You can specify additional lambda expressions to further constrain when the deadman&#39;s switch is triggered. 
Example: 


```javascript
    var data = stream
        |from()...
    // Trigger critical alert if the throughput drops below 100 points per 10s and checked every 10s.
    // Only trigger the alert if the time of day is between 8am-5pm.
    data
        |deadman(100.0, 10s, lambda: hour("time") >= 8 AND hour("time") <= 17)
    //Do normal processing of data
    data...
```



```javascript
node|deadman(threshold float64, interval time.Duration, expr ...ast.LambdaNode)
```

Returns: [AlertNode](/kapacitor/v1.0/nodes/alert_node/)


### Default

Create a node that can set defaults for missing tags or fields. 


```javascript
node|default()
```

Returns: [DefaultNode](/kapacitor/v1.0/nodes/default_node/)


### Delete

Create a node that can delete tags or fields. 


```javascript
node|delete()
```

Returns: [DeleteNode](/kapacitor/v1.0/nodes/delete_node/)


### Derivative

Create a new node that computes the derivative of adjacent points. 


```javascript
node|derivative(field string)
```

Returns: [DerivativeNode](/kapacitor/v1.0/nodes/derivative_node/)


### Distinct

Produce batch of only the distinct points. 


```javascript
node|distinct(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### Elapsed

Compute the elapsed time between points 


```javascript
node|elapsed(field string, unit time.Duration)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### Eval

Create an eval node that will evaluate the given transformation function to each data point. 
A list of expressions may be provided and will be evaluated in the order they are given 
and results of previous expressions are made available to later expressions. 


```javascript
node|eval(expressions ...ast.LambdaNode)
```

Returns: [EvalNode](/kapacitor/v1.0/nodes/eval_node/)


### First

Select the first point. 


```javascript
node|first(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### Flatten

Flatten points with similar times into a single point. 


```javascript
node|flatten()
```

Returns: [FlattenNode](/kapacitor/v1.0/nodes/flatten_node/)


### GroupBy

Group the data by a set of tags. 

Can pass literal * to group by all dimensions. 
Example: 


```javascript
    |groupBy(*)
```



```javascript
node|groupBy(tag ...interface{})
```

Returns: [GroupByNode](/kapacitor/v1.0/nodes/group_by_node/)


### HoltWinters

Compute the holt-winters forecast of a data set. 


```javascript
node|holtWinters(field string, h int64, m int64, interval time.Duration)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### HoltWintersWithFit

Compute the holt-winters forecast of a data set. 


```javascript
node|holtWintersWithFit(field string, h int64, m int64, interval time.Duration)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### HttpOut

Create an http output node that caches the most recent data it has received. 
The cached data is available at the given endpoint. 
The endpoint is the relative path from the API endpoint of the running task. 
For example if the task endpoint is at &#34;/api/v1/task/&lt;task_name&gt;&#34; and endpoint is 
&#34;top10&#34;, then the data can be requested from &#34;/api/v1/task/&lt;task_name&gt;/top10&#34;. 


```javascript
node|httpOut(endpoint string)
```

Returns: [HTTPOutNode](/kapacitor/v1.0/nodes/http_out_node/)


### InfluxDBOut

Create an influxdb output node that will store the incoming data into InfluxDB. 


```javascript
node|influxDBOut()
```

Returns: [InfluxDBOutNode](/kapacitor/v1.0/nodes/influx_d_b_out_node/)


### Join

Join this node with other nodes. The data is joined on timestamp. 


```javascript
node|join(others ...Node)
```

Returns: [JoinNode](/kapacitor/v1.0/nodes/join_node/)


### Last

Select the last point. 


```javascript
node|last(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### Log

Create a node that logs all data it receives. 


```javascript
node|log()
```

Returns: [LogNode](/kapacitor/v1.0/nodes/log_node/)


### Max

Select the maximum point. 


```javascript
node|max(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### Mean

Compute the mean of the data. 


```javascript
node|mean(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### Median

Compute the median of the data. Note, this method is not a selector, 
if you want the median point use .percentile(field, 50.0). 


```javascript
node|median(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### Min

Select the minimum point. 


```javascript
node|min(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### Percentile

Select a point at the given percentile. This is a selector function, no interpolation between points is performed. 


```javascript
node|percentile(field string, percentile float64)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### Sample

Create a new node that samples the incoming points or batches. 

One point will be emitted every count or duration specified. 


```javascript
node|sample(rate interface{})
```

Returns: [SampleNode](/kapacitor/v1.0/nodes/sample_node/)


### Shift

Create a new node that shifts the incoming points or batches in time. 


```javascript
node|shift(shift time.Duration)
```

Returns: [ShiftNode](/kapacitor/v1.0/nodes/shift_node/)


### Spread

Compute the difference between min and max points. 


```javascript
node|spread(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### Stats

Create a new stream of data that contains the internal statistics of the node. 
The interval represents how often to emit the statistics based on real time. 
This means the interval time is independent of the times of the data points the source node is receiving. 


```javascript
node|stats(interval time.Duration)
```

Returns: [StatsNode](/kapacitor/v1.0/nodes/stats_node/)


### Stddev

Compute the standard deviation. 


```javascript
node|stddev(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### Sum

Compute the sum of all values. 


```javascript
node|sum(field string)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### Top

Select the top `num` points for `field` and sort by any extra tags or fields. 


```javascript
node|top(num int64, field string, fieldsAndTags ...string)
```

Returns: [InfluxQLNode](/kapacitor/v1.0/nodes/influx_q_l_node/)


### Union

Perform the union of this node and all other given nodes. 


```javascript
node|union(node ...Node)
```

Returns: [UnionNode](/kapacitor/v1.0/nodes/union_node/)


### Where

Create a new node that filters the data stream by a given expression. 


```javascript
node|where(expression ast.LambdaNode)
```

Returns: [WhereNode](/kapacitor/v1.0/nodes/where_node/)


### Window

Create a new node that windows the stream by time. 

NOTE: Window can only be applied to stream edges. 


```javascript
node|window()
```

Returns: [WindowNode](/kapacitor/v1.0/nodes/window_node/)
