# Lodash Drills

Complete the following Lodash exercises. The goal is to become really good at
functional programming paradigm (e.g., `_.map`, `_.filter`, `_.all`, `_.any` ...etc) and
a number of really useful Lodash methods (e.g., `_.find`, `_.pluck` ... etc).

* enter your solution in the `solution` block
* utilize lodash functions as much as possible
* no `for/while` loop is allowed

Familiarity with programming in this way will not only make you a super productive
programmer but also will pave the way for you to learn MapReduce and MongoDB.

# Examples

{% lodashexercise %}

{% title %}

How many people?

{% data %}

[{name: 'John'}, {name: 'Mary'}, {name: 'Joe'}, {name: 'Ben'}]

{% output %}

4

{% solution %}
// Here we just need to count the number of elements in the data araay
return data.length

{% endlodashexercise %}


{% lodashexercise %}

{% title %}

What are the names?

{% data %}

[{name: 'John'}, {name: 'Mary'}, {name: 'Joe'}, {name: 'Ben'}]

{% output %}

['John', 'Mary','Joe','Ben']

{% solution %}
// Here we operate on every element of the data array (so no filter is
// necessary) and we return the property value of the name: field
// NOTE - _.map returns an array of values for each element in Collection while
//        _.filter returns an array of all elements for which predicate is true

return _.map(data, function(d) {
    return d.name
})

{% endlodashexercise %}

# Exercises

{% lodashexercise %}

{% title %}

What names begin with the letter J?

{% data %}

[{name: 'John'}, {name: 'Mary'}, {name: 'Joe'}, {name: 'Ben'}]

{% output %}

['John','Joe']

{% solution %}
// Here we filter based on if the property value of name: field starts with the letter 'J'
// Since filter returns the data array element that for which the test is true we
// pluck out the resulting value of the name: field from each returned array element

return _.pluck(_.filter(data, function(d) {
    return _.startsWith(d.name, 'J')
}), 'name')

{% endlodashexercise %}



{% lodashexercise %}

{% title %}

How many Johns?

{% data %}

[{name: 'John'}, {name: 'John'}, {name: 'John'}, {name: 'Ben'}]

{% output %}

3

{% solution %}
// Here we use filter with the shorthand _.matches notation to pick out which name: fields
// are of the property value 'John' and use _.size to count number of returned array elements

return _.size(_.filter(data, _.matches({'name': 'John'})))

{% endlodashexercise %}




{% lodashexercise %}

{% title %}

What are all the first names?

{% data %}

[{name: 'John Smith'}, {name: 'Mary Kay'}, {name: 'Peter Pan'}, {name: 'Ben Franklin'}]

{% output %}

["John","Mary","Peter","Ben"]

{% solution %}
// Here we use map since operating on every data array element with a nested
// splitting of the name: property value, returning the first string (first name) of value

return _.map(data, function(d) {
    return _.first(d.name.split(' '))
})

{% endlodashexercise %}







{% lodashexercise %}

{% title %}

What are the first names of Smith?

{% data %}

[{name: 'John Smith'}, {name: 'Mary Smith'}, {name: 'Peter Pan'}, {name: 'Ben Smith'}]

{% output %}

["John","Mary","Ben"]

{% solution %}
// Here we first filter the data to determine which array elements include
// the string 'Smith', then we used the filtered array result as an input
// to _.map which iterates over and pulls out (_.splits) the first name of 
// returned filtered array.  FIXME: This most likely can be improved

var f_data = _.filter(data, function(d) {
    if (_.includes(d.name, "Smith")) {
        return 1
    }
})
return _.map(f_data, function(f) {
    return _.first(f.name.split(' '))
})

{% endlodashexercise %}







{% lodashexercise %}

{% title %}

Change the format to lastname, firstname

{% data %}

[{name: 'John Smith'}, {name: 'Mary Kay'}, {name: 'Peter Pan'}]

{% output %}

[{name: 'Smith, John'}, {name: 'Kay, Mary'}, {name: 'Pan, Peter'}]

{% solution %}

// _.map() and reverse the name field seperating via a comma
//

// return _.map(data, function(d) {
    // console.log(_.last(d.name.split(' ')) + ', ' + _.first(d.name.split(' ')))
    // return ( '{ ' + 'name: ' + _.last(d.name.split(' ')) + ', ' + _.first(d.name.split(' ')) + ' }' )
// })

var result = 'not done'
return result

{% endlodashexercise %}



{% lodashexercise %}

{% title %}

How many women?

{% data %}

[{name: 'John Smith', gender: 'm'}, {name: 'Mary Smith', gender: 'f'}, {name: 'Peter Pan', gender: 'm'}, {name: 'Ben Smith', gender: 'm'}]

{% output %}

1

{% solution %}

// Here we use the _.filter shorthand _.matches notation to find all data
// array elements that have the gender: value of 'f' and count with _.size

return _.size(_.filter(data, {gender: 'f'}))

{% endlodashexercise %}







{% lodashexercise %}

{% title %}

How many men whose last name is Smith?

{% data %}

[{name: 'John Smith', gender: 'm'}, {name: 'Mary Smith', gender: 'f'}, {name: 'Peter Pan', gender: 'm'}, {name: 'Ben Smith', gender: 'm'}]

{% output %}

2

{% solution %}

// Filter to find males (gender: 'm'); Filter to find names: that include Smith
// _map to iterate result and find last names of 'Smith'

var males = _.filter(data, {gender: 'm'})
var names = _.filter(males, function(d) {
    if(_.includes(d.name, "Smith")) {return 1}
})
return _.size(_.map(names, function(f) {
    return _.last(f.name.split(' '))
}))

{% endlodashexercise %}







{% lodashexercise %}

{% title %}

Are there more men than women?

{% data %}

[{name: 'John Smith', gender: 'm'}, {name: 'Mary Smith', gender: 'f'}, {name: 'Peter Pan', gender: 'm'}, {name: 'Ben Smith', gender: 'm'}]

{% output %}

true

{% solution %}

var result = false
if (_.size(_.filter(data, {gender: 'm'})) > _.size(_.filter(data, {gender: 'f'}))) {result = true}
return result

{% endlodashexercise %}







{% lodashexercise %}

{% title %}

What is Peter Pan's gender?

{% data %}

[{name: 'John Smith', gender: 'm'}, {name: 'Mary Smith', gender: 'f'}, {name: 'Peter Pan', gender: 'm'}, {name: 'Ben Smith', gender: 'm'}]

{% output %}

'm'

{% solution %}
// Filter input data array to find {name: 'Peter Pan'}, then _.pluck to get 'gender' field
// Since _.pluck returns array, then use _.first to get first element of the array

return _.first(_.pluck(_.filter(data, {name: 'Peter Pan'}), 'gender'))

{% endlodashexercise %}






{% lodashexercise %}

{% title %}

What is the oldest age?

{% data %}

[{name: 'John Smith', age: 54}, {name: 'Mary Smith', age: 42}, {name: 'Peter Pan', age: 15}, {name: 'Ben Smith', age: 35}]

{% output %}

54

{% solution %}
// _.pluck to look at all 'age' fields and then get _.max

return _.max(_.pluck(data, 'age'))

{% endlodashexercise %}






{% lodashexercise %}

{% title %}

Is it true everyone is younger than 60?

{% data %}

[{name: 'John Smith', age: 54}, {name: 'Mary Smith', age: 42}, {name: 'Peter Pan', age: 15}, {name: 'Ben Smith', age: 35}]

{% output %}

true

{% solution %}

// use _.all

return _.all(data, function(n) {
    return n.age < 60
})

{% endlodashexercise %}




{% lodashexercise %}

{% title %}

Is it true someone is not an adult (younger than 18)?

{% data %}

[{name: 'John Smith', age: 54}, {name: 'Mary Smith', age: 42}, {name: 'Peter Pan', age: 15}, {name: 'Ben Smith', age: 35}]

{% output %}

true

{% solution %}

// use _.some

return _.some(data, function(n) {
    return n.age < 18
})

{% endlodashexercise %}




{% lodashexercise %}

{% title %}

How many people whose favorites include food?

{% data %}

[{name: 'John Smith', age: 54, favorites: ['food', 'movies']},
 {name: 'Mary Smith', age: 42, favorites: ['food', 'travel']},
 {name: 'Peter Pan', age: 15, favorites: ['minecraft', 'pokemo']},
 {name: 'Ben Smith', age: 35, favorites: ['craft', 'food']}]

{% output %}

3

{% solution %}

// Need to process all entries to determine for each entry 'food' is a favorite
// and count

return _.size(_.filter(data, function(n) {
    console.log(n.favorites)
    // return _.some(n.favorites, _.includes('food')) 
    return 
}))

{% endlodashexercise %}





{% lodashexercise %}

{% title %}

Who are over 40 and love travel?

{% data %}

[{name: 'John Smith', age: 54, favorites: ['food', 'movies']},
 {name: 'Mary Smith', age: 42, favorites: ['food', 'travel']},
 {name: 'Peter Pan', age: 15, favorites: ['minecraft', 'pokemo']},
 {name: 'Joe Johnson', age: 46, favorites: ['travel', 'movies']},
 {name: 'Ben Smith', age: 35, favorites: ['craft', 'food']}]

{% output %}

['Mary Smith', 'Joe Johnson']

{% solution %}

var result = 'not done'
return result

{% endlodashexercise %}




{% lodashexercise %}

{% title %}

Who is the oldest person loving food?

{% data %}

[{name: 'John Smith', age: 54, favorites: ['food', 'movies']},
 {name: 'Mary Smith', age: 42, favorites: ['food', 'travel']},
 {name: 'Peter Pan', age: 15, favorites: ['minecraft', 'pokemo']},
 {name: 'Joe Johnson', age: 46, favorites: ['travel', 'movies']},
 {name: 'Ben Smith', age: 35, favorites: ['craft', 'food']}]

{% output %}

'John Smith'

{% solution %}

var result = 'not done'
return result

{% endlodashexercise %}



{% lodashexercise %}

{% title %}

What are all the unique favorites?

{% data %}

[{name: 'John Smith', age: 54, favorites: ['food', 'movies']},
 {name: 'Mary Smith', age: 42, favorites: ['food', 'travel']},
 {name: 'Peter Pan', age: 15, favorites: ['minecraft', 'pokemo']},
 {name: 'Joe Johnson', age: 46, favorites: ['travel', 'movies']},
 {name: 'Ben Smith', age: 35, favorites: ['craft', 'food']}]

{% output %}

[
  "food",
  "movies",
  "travel",
  "minecraft",
  "pokemo",
  "craft"
]

{% solution %}

// hint: use _.pluck, _.uniq, _.flatten in some order

var result = 'not done'
return result

{% endlodashexercise %}



{% lodashexercise %}

{% title %}

What are all the unique last names?

{% data %}

[{name: 'John Smith', age: 54, favorites: ['food', 'movies']},
 {name: 'Mary Smith', age: 42, favorites: ['food', 'travel']},
 {name: 'Peter Pan', age: 15, favorites: ['minecraft', 'pokemo']},
 {name: 'Joe Johnson', age: 46, favorites: ['travel', 'movies']},
 {name: 'Ben Smith', age: 35, favorites: ['craft', 'food']}]

{% output %}

[
  "Smith",
  "Pan",
  "Johnson"
]

{% solution %}

var result = 'not done'
return result

{% endlodashexercise %}
