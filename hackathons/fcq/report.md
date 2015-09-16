{% data src="fcq.clean.json" %}
{% enddata %}

# Report

As a class, we brainstormed and came up with a long list of further questions we
can ask based on the FCQ data. Out of these questions, our team chose to tackle on
the following questions. Each member on our team is reponsible for one question.

# (Question 1) by (Name)

{% lodash %}
return "[answer]"
{% endlodash %}


# Question 2 [KevinKGifford]:  What is the ranking of Departments within the College of Engineering & Applied Sciences based on averaged course rating and average instructor rating?

{% lodash %}
// Get the subset of College of Engineering data records
var en_data = (_.filter(data, function(d) {
    return d.CrsPBAColl == 'EN'
}))
return _.size(en_data)
{% endlodash %}

The number of courses in the College of Engineering is {{result}}.


{% lodash %}
// Determine distribution of courses across the College of Engineering Departments
var en_data = (_.filter(data, function(d) {
    return d.CrsPBAColl == 'EN'
}))
var en_depts = _.groupBy(en_data, 'CrsPBADept')
return _.mapValues(en_depts, function(d) {
    return d.length
})
{% endlodash %}

Table 2-1: The breakdown of courses by department in the College of Engineering
<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>


{% lodash %}
// Determine the average course rating for all courses in each Engineering Department
var en_data = (_.filter(data, function(d) {
    return d.CrsPBAColl == 'EN'
}))
var en_depts = _.groupBy(en_data, 'CrsPBADept')
var results = _.mapValues(en_depts, function(d) {
    var value = _.pluck(d, 'AvgCourse')
    var filtered_value = _.filter(value, function(d) {
        return d != null
    })
    var filtered_average = _.round(_.sum(filtered_value)/filtered_value.length, 2)
    return filtered_average
    // return { average: filtered_average }
})
console.log(results)
sorted_results = _.sortBy(results)
return sorted_results
{% endlodash %}

Table 2-2: The average course rating by department in the College of Engineering
<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>


{% lodash %}
// Determine the average instructor rating for all courses in each Engineering Department
var en_data = (_.filter(data, function(d) {
    return d.CrsPBAColl == 'EN'
}))
var en_depts = _.groupBy(en_data, 'CrsPBADept')
// return _.mapValues(en_depts, function(d) {
return _.mapValues(en_depts, function(d) {
    var value = _.pluck(d, 'AvgInstructor')
    var filtered_value = _.filter(value, function(d) {
        return d != null
    })
    var filtered_average = _.round(_.sum(filtered_value)/filtered_value.length, 2)
    return filtered_average
})
{% endlodash %}

Table 2-3: The average instructor rating by department in the College of Engineering
<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>













# (Question 3) by (Name)

{% lodash %}
return "[answer]"
{% endlodash %}

# (Question 4) by (Name)

{% lodash %}
return "[answer]"
{% endlodash %}

# (Question 5) by (Name)

{% lodash %}
return "[answer]"
{% endlodash %}
