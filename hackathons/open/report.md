{% data src="emergency.json" %}
{% enddata %}

# Report

We utilized only Javascript and SVG to produce this data analysis / visualization report.

# Authors

This report is prepared by
* [Kevin Gifford](http://github.com/kevinkgifford)
* [John Raesly](http://github.com/jraesly)
* [Robert Kendl](http://github.com/DomoYeti)

# Data

For this assignment the authors opted to spend time and search for Public Safety Open Data available online.  We found several good resources at the state level for several states and also for cities at a more local level.  In particular, New York City maintains an easy-to-use open data website at (https://nycopendata.socrata.com/)

The data for this exercise/report came from [here](https://data.cityofnewyork.us/Public-Safety/Emergency-Response-Incidents/pasr-j7fb) and looks like:

{{ data | json }}

<a name="top"/>
<div id="autonav"></div>

# Question 1: Which borough has the highest number of emergency incidents?

{% lodash %}
var groups = _.groupBy(data, 'Borough')
var result =_.mapValues(groups, function(d){
    return d.length
})

// Convert object to array and sort
result = _.sortBy(_.pairs(result))
return result

{% endlodash %}

Table 1-1: Which borough has the highest number of emergency incidents?
<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>

Use the warmup exercise as the template to produce an answer here.


# Question 2: What are the top-10 most common types of emergency incidents?

{% lodash %}
var groups = _.groupBy(data, 'Incident Type')
var result =_.mapValues(groups, function(d){
    return d.length
})

// Convert object to array and sort 
result = _.slice(_.sortByOrder(_.pairs(result),function(d){return d[1]},'desc'), 0, 10)
return result

{% endlodash %}

Table 2-1: What are the top-10 most common types of emergency incidents?
<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>


Use the warmup exercise as the template to produce an answer here.

# Question 3: Which borough has the highest number of 'Fire-1st Alarm' incidents?

{% lodash %}
var flist = _.filter(data, function(f) {
    return f['Incident Type'] == "Fire-1st Alarm"
})
var list = _.map(flist, function(n) {
    var borough = n['Borough']
    var incident = n['Incident Type']
    return { "Borough": borough, "Incident Type": incident }
})
var groups = _.groupBy(list, 'Borough')
var result =_.mapValues(groups, function(d){
    return d.length
})

// Convert object to array and sort 
result = _.sortBy(_.pairs(result))
return result

{% endlodash %}

Table 3-1: Which borough has the highest number of 'Fire-1st Alarm' incidents?
<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>


Use the warmup exercise as the template to produce an answer here.
