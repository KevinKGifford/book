{% data src="fcq.clean.json" %}
{% enddata %}

# Report

As a team, answer all the questions the team's members submitted on our course forum. Each team member is responsible for one question. But everyone should work together to come up with a good solution. Your answer should consist of Lodash code and a brief writeup. Utilize _.map, _.filter, _.group ...etc. Do not use any for loop.

It is important for everyone to understand all the solutions and make sure you will be able to independently reproduce these solutions when asked to do so. Coming up, we will incorporate variations of these questions into a future hackathon and you are expected to be capable of reproducing and adapting your solutions.

As a class, we brainstormed and came up with a long list of further questions we
can ask based on the FCQ data. Out of these questions, our team chose to tackle on
the following questions. Each member on our team is reponsible for one question.

# Question 1 [Zachary Lamb]: How does retention compare across departments?

{% lodash %}
/* Group by department, then calculate the average withdrawel rate */
var grps = _.groupBy(data, 'CrsPBADept')
var result =_.mapValues(grps, function(d){
    return _.sum(_.pluck(d, 'PCT.WDRAW'))/d.length
})

/* Convert object to array and sort by withdrawal rate */
result = _.sortByOrder(_.pairs(result),function(d){return d[1]},'desc')
return result

{% endlodash %}

Table 1-1: How does retention compare across departmentsing
<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>



# Question 2 [Kevin Gifford]:  What is the ranking of Departments within the College of Engineering & Applied Sciences based on averaged course rating and average instructor rating?

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
})
var sorted_output = _.sortBy(_.pairs(results), function(d) {
    return d[1]
})
return sorted_output
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
var en_data = _.filter(data, function(d) {
    return d.CrsPBAColl == 'EN'
})

var en_depts = _.groupBy(en_data, 'CrsPBADept')

var results = _.mapValues(en_depts, function(d) {
    var value = _.pluck(d, 'AvgInstructor')
    var filtered_value = _.filter(value, function(d) {
        return d != null
    })
    var filtered_average = _.round(_.sum(filtered_value)/filtered_value.length, 2)
    return filtered_average
})

var sorted_output = _.sortBy(_.pairs(results), function(d) {
    return d[1]
})
return sorted_output

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



{% lodash %}
// For the ASEN department create a sorted list of [key, value] = [AvgCourse, Instructors]

var asen_data = _.filter(data, function(d) {
    return d.CrsPBADept == 'ASEN'
})
var below_average = _.filter(asen_data, function(d) {
    return d.AvgCourse < 4.90
})
var groups = _.groupBy(below_average, 'AvgCourse')

var courses = _.mapValues(groups, function(d) {
    return _.pluck(_.flatten(_.pluck(d, 'Instructors')), 'name')
})
var sorted_output = _.sortBy(_.pairs(courses), function(d) {
    return d[0]
})
return sorted_output

{% endlodash %}


Table 2-4: Instructors with below average (ave = 4.90) course-rating in ASEN
<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>



{% lodash %}
// For the ASEN department create a sorted list of [key, value] = [AvgInstructor, Instructors]

var asen_data = _.filter(data, function(d) {
    return d.CrsPBADept == 'ASEN'
})
var below_average = _.filter(asen_data, function(d) {
    return d.AvgInstructor < 5.16
})
var groups = _.groupBy(below_average, 'AvgInstructor')

var courses = _.mapValues(groups, function(d) {
    return _.pluck(_.flatten(_.pluck(d, 'Instructors')), 'name')
})
var sorted_output = _.sortBy(_.pairs(courses), function(d) {
    return d[0]
})
return sorted_output

{% endlodash %}


Table 2-5: Instructors with below average (ave = 5.16) course-rating in ASEN
<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>





# Question 3 [Karen Blakemore]: What is the distribution of instructor type (e.g., Lecturer, Assistant Professor, Instructor) across departments? 

{% lodash %}
/* First group by department */
var departments = _.groupBy(data, 'CrsPBADept')

/* Then collect titles for all classes in each department */
var titles = _.mapValues(departments, function(d) {
	return _.pluck(_.flatten(_.pluck(d, 'Instructors')), 'title')
})

/* Finally, compute length for each title */
var grps = _.mapValues(titles, function(d) {
	return _.mapValues(_.groupBy(d, function(title) {return title}), 'length')
})

return grps

{% endlodash %}

{% for dept, titles in result %}
<p><b>{{dept}}</b></p>	
<table>
	{% for title, count in titles %}
    <tr>
        <td>{{title}}</td>
        <td>{{count}}</td>
    </tr>
	{% endfor %}
</table>
{% endfor %}



# Question 4 [John Murphy]: Which class has the highest rating with the least amount of time spent each week?

{% lodash %}
/* First, extract all courses with the easiest work load */
var courses = _.groupBy(data, 'Workload.Hrs_Wk')['0-3']

/* Then, find those which have a 4.0 grade, remove duplicates and sort. */
return easy_As = _.unique(_.pluck(_.filter(courses, {'AVG_GRD': 4.0}), 'CourseTitle')).sort()
{% endlodash %}

Table 4-1: Which class has the highest rating with the least amount of time spent each week?

<p><b>Easy A Courses</b></p>
<table>
{% for course in result %}
    <tr>
        <td>{{course}}</td>
    </tr>
{% endfor %}
</table>




# Question 5 [Andrew Linenfelser]: Which course level has the lowest retention?

{% lodash %}
/* Group by level, then calculate average course withdrawal rate for each level */
var grps = _.groupBy(data, 'Level')
var result =_.mapValues(grps, function(d){
    return _.sum(_.pluck(d, 'PCT.WDRAW'))/d.length
})

/* Convert object to array and sort by withdrawal rate */
result = _.sortByOrder(_.pairs(result),function(d){return d[1]},'desc')
return result
{% endlodash %}


Table 5-1: Which course level has the lowest retention?
<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>


