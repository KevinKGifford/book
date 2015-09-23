{% data src="birdstrike.json" %}
{% enddata %}

# Report

As a team, answer a subset of the questions submitted during the hackathon.
But instead of using Tableau, you will need to write Javascript/Lodash code
to derive your answers. Similar to before, each team member is responsible for
one question. But everyone should work together to come up with a good solution.
Your answer should consist of Lodash code and a brief writeup.
Utilize `_.map`, `_.filter`, `_.group` ...etc. Do not use any for loop.

This time, the data is not already prepared for you in a nice JSON format. You
will need to do it on your own, replacing the placeholder `birdstrike.json` with
real data.

This report is prepared by
* [William Farmer](http://github.com/willzfarmer)
* [Parker Illig](http://github.com/pail4944)
* [Kevin Gifford](http://github.com/kevinkgifford)
* [John Raesly](http://github.com/jraesly)
* [Andrew Krodinger](http://github.com/drewdinger)


NOTE: Command to transform birdstrikes.xlsx to birdstrikes.csv:

prompt> in2csv birdstrikes.xlsx > birdstrike.csv

NOTE: Command to cut the data and output the birdstrikes.json file utilized as the source data for this report:

prompt> csvcut -c 'Airport: Name','Aircraft: Airline/Operator','Aircraft: Make/Model','Effect: Indicated Damage','When: Phase of flight','Feet above ground','Conditions: Sky','Cost: Total $' birdstrike.csv | csvjson --indent 4 > birdstrike.json


The data looks like:

{{ data | json }}


# Question 1: What are the condition:sky where usually caused a damage? From: [fadhilfath](http://github.com/fadhilfath) By: [William Farmer](http://github.com/willzfarmer)

{% lodash %}
var flist = _.filter(data, function(f) {
    return _.any(f['Effect: Indicated Damage'], function(d) {
        return d != "No Damage"
    })
})
var list = _.map(flist, function(n) {
    var weather = n['Conditions: Sky']
    if (n['Conditions: Sky'] == "No Cloud") weather = "No Clouds"
    if (n['Conditions: Sky'] == "Some Cloud") weather = "Some Clouds"
    var airport = n['Airport: Name']
    return { "airportName": airport, "skyCondition": weather }
})
var groups = _.groupBy(list, 'skyCondition')
return _.mapValues(groups, function(d) {
    return d.length
})
{% endlodash %}

Table 1-1: What is the distibution of sky conditions where damaged occurred
<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>



# Question 2b: What is the most common flight phase where a birdstrike occurred? From: [KevinKGifford](http://github.com/kevinkgifford) By: [KevinKGifford](http://github.com/kevinkgifford)

{% lodash %}
var groups = _.groupBy(data, 'When: Phase of flight')
var results = _.mapValues(groups, function(d){
    return d.length
})
var sorted_output = _.sortBy(_.pairs(results), function(d) {
    return d[1]
})
return sorted_output
{% endlodash %}

Table 2-1: What is the most common flight phase where a birdstrike occurred
<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>



# Question 2b: What is the correlation between the feet above the ground and the number of bird strikes? From: [Linenfelser](http://github.com/Linenfelser) By: [KevinKGifford](http://github.com/kevinkgifford)

{% lodash %}
var list = _.map(data, function(n) {
    if (n['Feet above ground'] == "") {
        n['Feet above ground'] = "Not Reported"
    }
    var feet = (_.round(n['Feet above ground'] / 1000)) * 1000
    var airport = n['Airport: Name']
    return { "airportName": airport, "Feet above ground": n['Feet above ground'], "elevationBin": feet }
})
var groups = _.groupBy(list, 'elevationBin')
return _.mapValues(groups, function(d) {
    return d.length
})

{% endlodash %}

Table 2-2: What is the correlation between the feet above the ground and the number of bird strikes?
<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>



# Question 3: Which airport had the highest number of bird strikes? From: [Zhya215](http://github.com/Zhya215) By: [Andrew Krodinger](http://github.com/drewdinger)

{% lodash %}
var groups = _.groupBy(data, 'Airport: Name')
var results = _.mapValues(groups, function(d) {
    return d.length
})
var sorted_output = _.sortByOrder(_.pairs(results), function(d) {
    return d[1]
},'desc')
return sorted_output
{% endlodash %}

Table 3-1: Which airport had the highest number of bird strikes?
<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>




# Question 4: Which airline have to incur most repair cost due to damage? From: [sumi6109](http://github.com/sumi6109) By: [Parker Illig](http://github.com/pail4944)

{% lodash %}
var list = _.filter(data, function(f) {
    return _.any(f['Cost: Total $'], function(d) {
        return d > 0
    })
})
var groups = _.groupBy(list, 'Aircraft: Airline/Operator')
// Sum the damage costs on a per airline (key) basis
var results = _.mapValues(groups, function(d) {
    return _.sum(_.pluck(d, 'Cost: Total $'))
})
var sorted_output = _.sortByOrder(_.pairs(results), function(d) {
    return d[1]
},'desc')
return sorted_output
{% endlodash %}

Table 4-1: Which airline incurred the most repair cost due to damage? 
<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>



# Question 5: Which plane model strikes the most birds? From: [twagar95](http://github.com/twagar95) By: [John Raesly](http://github.com/jraesly)

{% lodash %}
var groups = _.groupBy(data, 'Aircraft: Make/Model')
var results = _.mapValues(groups, function(d) {
    return d.length
})
var sorted_output = _.sortByOrder(_.pairs(results), function(d) {
    return d[1]
},'desc')
return sorted_output
{% endlodash %}

Table 5-1: Which plane model strikes the most birds?
<table>
{% for key, value in result %}
    <tr>
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
{% endfor %}
</table>


