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
// TODO: replace with code that computes the actual result
return ['4830','4830']
{% endlodash %}

They are {{result}}.



## Prototyping for "What are the course numbers of the courses Tom (PEI HSIU) Yeh taught?"

# How many computer science (CSCI) courses?

{% lodash %}

var groups = _.groupBy(data, 'Subject')
return _.pluck(_.filter(groups, function(d) {
    return _.mapValues(d.Instructors, _.matches({'name': 'YEH, PEI HSIU'}))
}), 'Course')

// return _.size(_.filter(data, function(d) {
//     if(_.pluck(d.Instructors, 'name') == 'YEH, PEI HSIU') {
//         console.log('------************---------')
//         console.log(d.Instructors)
//         console.log(d.Course)
//     }
//     return d.CrsPBADept == 'CSCI'
// }))

{% endlodash %}

They are {{ result }} computer science courses.
