---
layout: default
title: The power of difference list
parent: FSharp Component
nav_order: 2
---

----

### Difference list


#### What is the use case for the data structure difference list
{: .fs-4 }

We all know the importance of data structures in programming, and how their use case can help write more efficient and in general better code.

Now what is a difference list, and what is the purpose of it?


So let's assume we have been given the task to create some sort of queue system for some restaurent. Now a queue just like in real life essentially means that whenever a new element or a person is being added to the queue, the element/person is going to be placed at the end of the queue. 


In F# we could easily implement this using the `@` operator, which is a function that takes `list1` and `list2` and concatenates the two lists. So in this case, we could concetenate the current list to the new element and we would wind up having the new person at the end of the list, and we've achieved to create a queue, right?

Well, actually no.

Now let's look at an example of why. Let's say Facebook wants us to implement some sort of queue system where each newly added element is placed at the end of the queue. And let's assume facebook has around 10^9 data that we need to store in the list.

We then implement the data structure with O(n) time complexity. Let's make a perhaps unrealistic assumption that our computer can handle 100000 operations per minute, then we divide the two, and get amount in minutes, and we know that there are a total of 60*24 minutes a day so we divide it by that and we get a total of 69 days it'd take. 

And I can't specify the importance of how much it actually depends on the hardware of the machine. This is only an example of a computer that can handle 10000 steps/operations per minute. 
{: .warning }


 ```math
\frac{10^9}{10000}=\frac{100000}{(60*24)}=69 days
```

#### How come it's so slow?
{: .fs-4 }

```fsharp
type Person = { Name: string }

let queue: Person list =
    [ { Name = "Simon" }; { Name = "Lars" }; { Name = "Peter" } ]
    @ [ { Name = "John" } ]
```

The reason why it's so slow is that when concenating the first list with, with three elements n=3 to the last list with only one element n=1, we have to go through each and every single element of the first list and append to the second list, which cases the operation to run in linear time O(n). 

Now for a list of a small size, this won't be a problem, but in computer science, we usually need a lot than one million data in a list, hence why this would be a slow process in the end. 

And that's where the difference list comes into play. The difference list can queue elements at the end in constant time O(1). Hence why it's very useful to use. 

We know that list's in F# are implemented as linked lists. So, when adding an element to the start of the list using the cons-operator `::` it takes O(1) time, hence why we call the opposite snoc (cons spelled from behind is snoc) since it takes O(1) time to append it to the end of the queue. 

And this is why we want to try and implement the difference list. 

#### So how does a difference list look like
{: .fs-4 }

The difference list itself is fairly simply to implement. It's essentially just a mapping from an alpha list to an alpha list or rather a function that takes an alpha list as parameter, and returns an alpha list once invoked. 
```fsharp
type 'a dlist = 'a list -> 'a list
```


