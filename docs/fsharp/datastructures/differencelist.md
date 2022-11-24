---
layout: default
title: Difference List
parent: Data Structures
grand_parent: FSharp
nav_order: 1
---

# Difference list


#### What is the use case for the data structure difference list
{: .fs-5 }

We all know the importance of data structures in programming, and how their use case can help write more efficient and in general better code.

Data structures are important in programming, and their use can make code more efficient and in general better.

So what exactly is a difference list, and what is its purpose?

Just like in real life, a queue simply means that whenever a new element or person is added to it, the item/person will be placed last in the queue. 


A comparison of the two lists can be easily accomplished in F# using the `@` operator, which takes two lists and concatenates them. 

If we concatenated the existing list to the new element, we would wind up with the new person at the end, achieving the same thing as creating a queue. So we're basically just done right, and no need to go further into processing. Well, actually no.

Here's an example of why. Suppose we were given the task of creating a queue system for Facebook in which it places the newly added elements at the end of the queue. We'll just pretend that Facebook has around 10^9th elements that already gets stored in our queue. 

A data structure with O(n) complexity is then implemented. Let's make a perhaps unrealistic assumption that our computer can handle 100000 operations per minute.

Now we divide the amount of operations our steps in total with operations per minute, and we get the amount of time it'd take in minutes. So assuming it had a O(n) time complexity, and O(10^9) we essentially divide the two 10^9/10000 and to get the total time in days we know that there are a total of 60*24 minutes in a day so we also divide the result by that, and we get 69 days in total it would take to compute this. Hence, not a good practice to use the concatenation operator when we're dealing with lists that are of large sizes.

And I can't specify the importance of how much it actually depends on the hardware of the machine. This is only an example of a computer that can handle 10000 steps/operations per minute. 
{: .warning }


 ```math
\frac{10^9}{10000}=\frac{100000}{(60*24)}=69 days
```

#### How come it's so slow?
{: .fs-5 }

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
{: .fs-5 }

The difference list itself is fairly simply to implement. It's essentially just a mapping from an alpha list to an alpha list or rather a function that takes an alpha list as parameter, and returns an alpha list once invoked. 
```fsharp
type 'a dlist = 'a list -> 'a list
//Create a difference list
//(@) list1 list2
let toDifferenceList (list1: 'a list) = (@) list1 //O(1) time complexity
//return normal cons list
let fromDifferenceListToCons (list: 'a dlist) = list [] //O(n) time complexity
let testfromDiffList (lst: int list) : int list = 3 :: lst
printfn "fromDiffList: %A" (fromDiffList testfromDiffList)
```

{: .console }
> fromDiffList: [3]

Here you notice 3 is baked into the list this is essentially the "magic". 

Now what exactly is the magic you might think. The magic is baking the alpha list `'a list` into the intermediate function, and then once it's invoked with the parameters of an empty cons list `[]` it takes constant time, O(1), since it's already baked into the function and doesn't need any extra operation steps. We'll try to dig a bit deeper, if this seems totally off, and you have no clue what's going on, you should read my blog post about [Currying in FSharp](another-page).

