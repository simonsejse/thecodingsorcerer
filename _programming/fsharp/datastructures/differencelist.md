---
layout: default
title: Difference List
nav_order: 1
mathjax: true
parent: Data Structures
grand_parent: FSharp
tags: 
  - latex
  - math
  - time complexity
  - difference list
  - dlist
  - fsharp
  - f#
---

# Difference list


#### What is the use case for the data structure difference list
{: .fs-5 }
{: .fw-700 }

We all know the importance of data structures in programming, and how their use case can help write more efficient and in general better code.

Data structures are important in programming, and their use can make code more efficient and in general better.

So what exactly is a difference list, and what is its purpose?

Just like in real life, a queue simply means that whenever a new element or person is added to it, the item/person will be placed last in the queue. 


A comparison of the two lists can be easily accomplished in F# using the `@` operator, which takes two lists and concatenates them. 

If we concatenated the existing list to the new element, we would wind up with the new person at the end, achieving the same thing as creating a queue. So we're basically just done right, and no need to go further into processing. Well, actually no.

Here's an example of why. Suppose we were given the task of creating a queue system for Facebook in which it places the newly added elements at the end of the queue. We'll just pretend that Facebook has around 10^9th elements that already gets stored in our queue. 

A data structure with $$O(n)$$ complexity is then implemented. Let's make a perhaps unrealistic assumption that our computer can handle $$100000$$ operations per minute.

Now we divide the amount of operations our steps in total with operations per minute, and we get the amount of time it'd take in minutes. So assuming it had a $$O(n)$$ time complexity, and $$O(10^9)$$ we essentially divide the two $$\frac{10^9}{10000}$$ and to get the total time in days we know that there are a total of $$60 \cdot 24$$ minutes in a day so we also divide the result by that, and we get $$69$$ days in total it would take to compute this. Hence, not a good practice to use the concatenation operator when we're dealing with lists that are of large sizes. Look in \eqref{eq:calculatingTimeExample} for clarity.

And I can't specify the importance of how much it actually depends on the hardware of the machine. This is only an example of a computer that can handle 10000 steps/operations per minute. 
{: .warning }


$$
\begin{equation}
\frac{10^9}{10000} = 100000\ minutes\ \Leftrightarrow \lfloor \frac{10^5}{60 \cdot 24} \rfloor = 69\ days
\label{eq:calculatingTimeExample}
\end{equation}
$$

#### How come it's so slow?
{: .fs-5 }
{: .fw-700 }

```fsharp
type Person = { Name: string }

let queue: Person list =
    [ { Name = "Simon" }; { Name = "Lars" }; { Name = "Peter" } ]
    @ [ { Name = "John" } ]
```

The reason why it's so slow is that when concenating the first list with, with three elements n=3 to the last list with only one element n=1, we have to go through each and every single element of the first list and append to the second list, which cases the operation to run in linear time $$O(n)$$. 

Now for a list of a small size, this won't be a problem, but in computer science, we usually need a lot than one million data in a list, hence why this would be a slow process in the end. 

And that's where the difference list comes into play. The difference list can queue elements at the end in constant time $$O(1)$$. Hence why it's very useful to use. 

We know that list's in F# are implemented as linked lists. So, when adding an element to the start of the list using the cons-operator `::` it takes $$O(1)$$ time, hence why we call the opposite snoc (cons spelled from behind is snoc) since it takes $$O(1)$$ time to append it to the end of the queue. 

And this is why we want to try and implement the difference list. 

#### So how does a difference list look like
{: .fs-5 }
{: .fw-700 }

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

Now what exactly is the magic you might think. The magic is baking the alpha list `'a list` into the intermediate function, and then once it's invoked with the parameters of an empty cons list `[]` it takes constant time, $$O(1)$$, since it's already baked into the function and doesn't need any extra operation steps. We'll try to dig a bit deeper, if this seems totally off, and you have no clue what's going on, you should read my blog post about [Currying in FSharp](https://simonsejse.github.io/blog/programming/fsharp/currying/).

So we've now seen how a difference list is defined, as a function that takes a generic cons list of type 'a and returns an intermediate function that returns a list of a cons list of the generic type 'a. Pheew...

We've also looked at how to create a difference list, and also how to go back to a cons list.

Now we take a look at adding elements.
```fsharp
// single x ~ [x] 
//This returns a difference list with that single element in it 
let single (x: 'a) = fun (ys: 'a list) -> x :: ys

let append = (<<)

//Normal cons
let cons (x: 'a) dl = append (single x) dl // = append (single x) dl
// append dl dl' ~ xs @ xs' if dl ~ xs and dl' ~ xs'

let snoc' (x: 'a) dl = snoc dl x // arguments flipped
```

#### The function single
{: .fs-5 }
{: .fw-700 }
So, the function `single` takes a parameter `x` of type `'a` and returns an intermediate function that that a parameter of an alpha list, `'a list` and this intermediate function returns the elementet added to the front of the list using cons operator, `x :: ys`. This function uses currying, since we want the single function to return a difference list, we just supply it with a parameter `x` and leave out the parameter `ys` in the intermediate function since we then will have `x` baked in. We use partial application to only supply the first parameter `x` which is baked in, and then it returns a function that takes a parameter `'a list` and returns `x :: ys` which is also of type `'a list` so now we actually see that the intermediate functions is a mapping of `'a list -> 'a list` which is how we defined our difference list. So, the function single essentially just returns a difference list with the one element, `x` baked in.

We could've also written the function `let single (x: 'a) = fun (ys: 'a list) -> x :: ys` as `let single' (x: 'a) (ys: 'a list) = x :: ys` whereas we'd still have to only supply the function `supply'` with one argument `x` to make use of partial application, so we end up having a `'a list -> 'a list` if we supply the second parameter `ys` we'd obviously just get a `'a list` in returns, so we only supply the first parameter. 

In my opinion, the function `single` on curried form clarifies the use of partial application a bit more than the uncurried form `single'`. And the lambda expression again just clarifies a bit more to make it easier to see that ones we give it one argument we still have a function that maps to a `'a list`. So, essentially both works, the `single` one is just a bit more clear to me, but both work.
