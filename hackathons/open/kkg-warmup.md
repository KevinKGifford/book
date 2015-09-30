# Warmup

Complete this warmup exercise to get an idea how to put all the different pieces
together to generate an end-to-end data analysis viz report.

<a name="top"/>
<div id="autonav"></div>

{% data src="../fcq/fcq.clean.json" %}
{% enddata %}

{% viz %}

{% title %}

What is the distribution of courses across colleges?

{% solution %}

var groups = _.groupBy(data, 'CrsPBAColl')

var counts = _.map(groups, function(value, key) {
    return { 'name': key, 'count': _.pluck(value, 'Course').length }
})
console.log(counts)

// Produce a bottom-aligned vertical bar chart

function computeX(d, i) {
    return i * 30
}

function computeHeight(d, i) {
    return _.round((d.count/8.0925))
}

function computeWidth(d, i) {
    return 20
}

function computeY(d, i) {
    // return 400 - computeHeight(d, i)
    return 0
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
        color: computeColor(d, i)
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
      width="${d.width}"
      heigth="${d.height}"
      style="fill:${d.color};
             stroke-width:3;
             stroke:rgb(0,0,0)" />

{% endviz %}
