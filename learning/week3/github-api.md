# Github API

The objective is to learn how to use Github's API to pull in real data. Your need
to figure out how to compose an URL for accessing a particular API to grab the data
you want.

Github provides a wide range of APIs. We will focus on these three for now.

* [Issues API](https://developer.github.com/v3/issues/)
* [Users API](https://developer.github.com/v3/users/)
* [Repositories API](https://developer.github.com/v3/repos/)

Carefully go through the examples below.

### What is the data of the first issue?

{% githubapi %}
https://api.github.com/repos/bigdatahci2015/forum/issues/1
{% endgithubapi %}

The data is saved in `data`. It looks like below.

{{ data | json }}

### What is the title of the first issue?

{{ data.title }}

### Who created this issue?

{{ data.user.login }}


### What are the comments of this issue?

{% githubapi %}
https://api.github.com/repos/bigdatahci2015/forum/issues/1/comments
{% endgithubapi %}

The comments data look like

{{ data | json }}

There are {{ data.length }} comments.

{% lodash %}
return _.pluck(data, 'user.login')
{% endlodash %}

The github account names are {{ result }}.

# Exercises

Now it's your turn to answer the following questions using the real data.

### How many issues have been created to date in our class's forum repository?

{% githubapi %}
https://api.github.com/repos/bigdatahci2015/forum/issues
{% endgithubapi %}

This issues data is saved in `data`. It looks like below.

{{ data | json }}

{% lodash %}
return _.size(data)
{% endlodash %}

There are {{result}} issues in the HCI Big Data Forum

### What are the titles of these issues?

{% lodash %}
return _.pluck(data, 'title')
{% endlodash %}

The issues of the HCI Big Data Forum are titled: {{result}}


### How many repository have been created to date for our class?

Our class's Github organization is [bigdatahci2015](https://github.com/bigdatahci2015/).

{% githubapi %}
https://api.github.com/orgs/bigdatahci2015/repos
{% endgithubapi %}

This repositories data is saved in `data`. It looks like below.

{{ data | json }}

{% lodash %}
return _.size(data)
{% endlodash %}

There are {{result}} repositories for [bigdatahci2015](https://github.com/bigdatahci2015/).

### What are the fork counts of our class's repositories?

{% lodash %}
var repo_names = _.pluck(data, 'name')
var count = _.pluck(data, 'forks_count')
answer = ""
// FIXME: Do without a for loop
for (i = 0; i < repo_names.length; i++) {
    if (i < repo_names.length - 1) {answer += repo_names[i] + ' has ' + count[i] + ' forks; '}
    else {answer += repo_names[i] + ' has ' + count[i] + ' forks.'}
}
return answer
{% endlodash %}

The results are: {{result}}

### How many public repositories does the user `doubleshow` have?

{% githubapi %}
https://api.github.com/users/doubleshow/repos
{% endgithubapi %}

The doubleshow repositories data is saved in `data`. It looks like below.

{{ data | json }}

{% lodash %}
return _.size(data)
{% endlodash %}

User doubleshow has {{result}} public repositories

### How many public gists does the user `doubleshow` have?

{% githubapi %}
https://api.github.com/users/doubleshow/gists
{% endgithubapi %}

The doubleshow gist data is saved in `data`. It looks like below.

{{ data | json }}

{% lodash %}
return _.size(data)
{% endlodash %}

User `doubleshow` has {{result}} public gists

