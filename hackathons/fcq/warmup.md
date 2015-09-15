{% data src="fcq.clean.json" %}
{% enddata %}

# Warmup

Next, complete the following warmup exercises as a team.

## How many unique subject codes?

{% lodash %}
return _.size(_.uniq(_.pluck(data, 'Subject')))
{% endlodash %}

They are {{ result }} unique subject codes.



## How many computer science (CSCI) courses?

{% lodash %}
return _.size(_.filter(data, function(d) {
    return d.CrsPBADept == 'CSCI'
}))
{% endlodash %}

They are {{ result }} computer science courses.



## What is the distribution of the courses across subject codes?

{% lodash %}
// TODO: replace with code that computes the actual result

var groups = _.groupBy(data, 'Subject')
return _.mapValues(groups, function(d) {
    return d.length
})

{% endlodash %}

<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>



## What subset of these subject codes have more than 100 courses?

{% lodash %}

var groups = _.groupBy(data, 'Subject')
return  _.pick(_.mapValues(groups, function(d) {
    return d.length
}), function(x) {
    return x > 100
})

{% endlodash %}

<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>



## What subset of these subject codes have more than 5000 total enrollments?

{% lodash %}

var groups = _.groupBy(data, 'Subject')
return _.pick(_.mapValues(groups, function(d) {
    return _.sum(_.pluck(d, 'N.ENROLL'))
}), function(x) {
    return x > 5000
})

{% endlodash %}

<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>



## What are the course numbers of the courses Tom (PEI HSIU) Yeh taught?

{% lodash %}
// NOTE: 'YEH, PEI HSUI' can teach for any college
// any department -> so no need to groupBy

return _.pluck(_.filter(data, function(d) {
    var instructor_name = _.pluck(d.Instructors, 'name')
    return instructor_name == 'YEH, PEI HSIU'
}), 'Course')

{% endlodash %}

They are {{result}}.


