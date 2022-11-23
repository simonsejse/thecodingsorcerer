---
layout: default
title: The power of difference list
parent: FSharp Component
nav_order: 2
---

----

### Difference list


#### The power of the data structure difference list
{: .fs-4 }

We all know the importance of data structures in programming, and how their use case can help write more efficient and in general better code.

Now what is a difference list, and what is the purpose of it?


So let's assume we have been given the task to create some sort of queue system for some restaurent. Now a queue just like in real life essentially means that whenever a new element or a person is being added to the queue, the element/person is going to be placed at the end of the queue. 
'
In F# we could easily implement this using the `@` operator, which is a function that takes `list1` and `list2` and concatenates the two lists. So in this case, we could concetenate the current list to the new element and we would wind up having the new person at the end of the list, and we've achieved to create a queue, right?
```fsharp
let queue: Person list =
    [ { Name = "Simon" }; { Name = "Lars" }; { Name = "Peter" } ]
    @ [ { Name = "John" } ]
```
{% highlight some_language %}
let queue: Person list =
    [ { Name = "Simon" }; { Name = "Lars" }; { Name = "Peter" } ]
    @ [ { Name = "John" } ]
{% endhighlight %}





