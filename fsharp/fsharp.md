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


In F# we could easily implement this using the `@` operator, which is a function that takes `list1` and `list2` and concatenates the two lists. So in this case, we could concetenate the current list to the new element and we would wind up having the new person at the end of the list, and we've achieved to create a queue, right?
```fsharp
type Person = { Name: string }

let queue: Person list =
    [ { Name = "Simon" }; { Name = "Lars" }; { Name = "Peter" } ]
    @ [ { Name = "John" } ]
```

Well, actually no. The reason being that when concenating the first list with three elements n=3 to the last list with only one element n=1, we have to go through each and every single element of the first list and append to the second list, which cases the operation to run in linear time O(n). Now for a list of a small size, this won't be a problem, but in computer science, we usually need a lot than one million data in a list, hence why this would be a slow process in the end. 

And that's where the difference list comes into play. The difference list can queue elements at the end in constant time O(1). Hence why it's very useful to use. We know that list's in F# are implemented as linked lists. So, when adding an element to the start of the list using the cons-operator `::` it takes O(1) time, hence why we call the opposite snoc (cons spelled from behind is snoc) since it takes O(1) time to append it to the end of the queue and this is what we want to achehive with difference lists. 

#### So how does a difference list look like
{: .fs-4 }

The difference list itself is fairly simply to implement. It's essentially just a mapping from an alpha list to an alpha list or rather a function that takes an alpha list as parameter, and returns an alpha list once invoked. 
```fsharp
type 'a dlist = 'a list -> 'a list
```


