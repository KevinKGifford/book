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

{% viz %}

{% title %}

# Question 1: Which borough has the highest number of emergency incidents?

{% solution %}

var groups = _.groupBy(data, 'Borough')
var counts = _.map(groups, function(value, key) { 
    return {'name': key, 'count': _.pluck(value, 'Incident Type').length} 
})

console.log(counts)

// Produce a bottom-aligned bar chart with margins

function computeX(d, i) { 
    return i * 90 
}

function computeHeight(d, i) {
    var value = _.max(_.pluck(counts, 'count'))
    return (d.count/(value/380))
}

function computeWidth(d, i) {
    return 80
}

function computeY(d, i) { 
    return 400 - computeHeight(d, i)
}

function computeColor(d, i) { 
    return 'red' 
}

function computeLabel(d, i) {
    return d.name
}

var viz = _.map(counts, function(d, i) { 
    return { 
        x: computeX(d, i), 
        y: computeY(d, i), 
        height: computeHeight(d, i), 
        width: computeWidth(d, i), 
        color: computeColor(d, i),
        label: computeLabel(d, i)
    } 
}) 
console.log(viz)

var result = _.map(viz, function(d) { 
    // invoke the compiled template function on each viz data 
    return template({d: d}) 
}) 
return result.join('\n')

{% template %}

<rect x="${d.x}"
      y="${d.y}"
      height="${d.height}"
      width="${d.width}"
      style="fill:${d.color};
             stroke-width:3;
             stroke:rgb(0,0,0)" />
    <text transform="translate(${d.x} ${d.y})">
        ${d.label}
    </text>

{% endviz %}


{% viz %}

{% title %}

# Question 2: What are the top-12 most common types of emergency incidents?

{% solution %}

var groups = _.groupBy(data, 'Incident Type')

// Using _.map and _.filter enables easy composition of unsorted output for viz purposes
var counts = _.map(groups, function(value, key) { 
    return {'name': key, 'count': _.pluck(value, 'Incident Type').length} 
})
console.log(counts)
var filtered_counts = _.filter(counts, function(d) {
    return d.count > 30
})
console.log(filtered_counts)

// Produce a bottom-aligned bar chart with margins

function computeX(d, i) { 
    return i * 60 
}

function computeHeight(d, i) {
    var value = _.max(_.pluck(counts, 'count'))
    return (d.count/(value/380))
}

function computeWidth(d, i) {
    return 50
}

function computeY(d, i) { 
    return 400 - computeHeight(d, i)
}

function computeColor(d, i) { 
    return 'red' 
}

function computeLabel(d, i) {
    return d.name
}

var viz = _.map(filtered_counts, function(d, i) { 
    return { 
        x: computeX(d, i), 
        y: computeY(d, i), 
        height: computeHeight(d, i), 
        width: computeWidth(d, i), 
        color: computeColor(d, i),
        label: computeLabel(d, i)
    } 
}) 
console.log(viz)

var result = _.map(viz, function(d) { 
    // invoke the compiled template function on each viz data 
    return template({d: d}) 
}) 
return result.join('\n')

{% template %}

<rect x="${d.x}"
      y="${d.y}"
      height="${d.height}"
      width="${d.width}"
      style="fill:${d.color};
             stroke-width:3;
             stroke:rgb(0,0,0)" />
    <text transform="translate(${d.x} ${d.y})">
        ${d.label}
    </text>

{% endviz %}


{% viz %}

{% title %}

# Question 3: Which borough has the highest number of 'Fire-1st Alarm' incidents?

{% solution %}

var flist = _.filter(data, function(f) {
    return f['Incident Type'] == "Fire-1st Alarm"
})
var groups = _.groupBy(flist, 'Borough')
// Using _.map and _.filter enables easy composition of unsorted output for viz purposes
var counts = _.map(groups, function(value, key) { 
    return {'name': key, 'count': _.pluck(value, 'Incident Type').length} 
})
console.log(counts)

// Produce a bottom-aligned bar chart with margins

function computeX(d, i) { 
    return i * 100
}

function computeHeight(d, i) {
    var value = _.max(_.pluck(counts, 'count'))
    return (d.count/(value/380))
}

function computeWidth(d, i) {
    return 90
}

function computeY(d, i) { 
    return 400 - computeHeight(d, i)
}

function computeColor(d, i) { 
    return 'red' 
}

function computeLabel(d, i) {
    return d.name
}

var viz = _.map(counts, function(d, i) { 
    return { 
        x: computeX(d, i), 
        y: computeY(d, i), 
        height: computeHeight(d, i), 
        width: computeWidth(d, i), 
        color: computeColor(d, i),
        label: computeLabel(d, i)
    } 
}) 
console.log(viz)

var result = _.map(viz, function(d) { 
    // invoke the compiled template function on each viz data 
    return template({d: d}) 
}) 
return result.join('\n')

{% template %}

<rect x="${d.x}"
      y="${d.y}"
      height="${d.height}"
      width="${d.width}"
      style="fill:${d.color};
             stroke-width:3;
             stroke:rgb(0,0,0)" />
    <text transform="translate(${d.x} ${d.y})">
        ${d.label}
    </text>

{% endviz %}

