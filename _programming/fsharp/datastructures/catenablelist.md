---
layout: default
title: Catenable Lists
nav_order: 2
mathjax: true
parent: Data Structures
grand_parent: FSharp
tags: 
  - catenable lists
  - catlist
  - clist
  - fsharp
  - f#
has_toc: true
---

# Catenable Lists
{: .py-1 }
{: .m-1 }
Week 7
{: .py-1 }
{: .m-1 }
{: .fs-5 }
{: .fw-700 }

<details markdown="block">
  <summary>
    Contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

<hr/>

#### What is a Catenable list?
{: .fs-5 }
{: .fw-700 }

Catenable lists are lists with efficient (constant-time) appending, $$O(1)$$ time complexity, like difference lists, and additional
operations. We were taught that they are widely used to implement text processing systems such as text editors, where
characters and text fragments need to be inserted and deleted efficiently, which is why arrays holding
the text are not used.

#### What does the Catenable list look like?
{: .fs-5 }
{: .fw-700 }
```fsharp
type 'a catlist =
    | Empty // empty node
    | Single of 'a // leaf node
    | Append of 'a catlist * 'a catlist
```

#### What is the use case for the data structure difference list
{: .fs-5 }
{: .fw-700 }


#### The append method
{: .fs-5 }
{: .fw-700 }
```fsharp

```
    

but ye it makes sense, if a is empty then just returns b no matter what, and if b is empty returns a nomatter what and if they not empty then returns them both appended

#### The fold method
{: .fs-5 }
{: .fw-700 }


```fsharp
let fold (cf: ('a -> 'a -> 'a) * 'a) (t: 'b -> 'a) (list: 'b catlist) : 'a =
    let rec f xs =
        match xs with
        | Empty -> snd cf
        | Single a -> t a
        | Append (ys, zs) -> fst (cf) (f ys) (f zs)

    f list
```



#### The map function based on our fold function
{: .fs-5 }
{: .fw-700 }

```fsharp

//append ~ fun a acc -> append a acc
```

#### The filter function based on our fold function
{: .fs-5 }
{: .fw-700 }

```fsharp

//append ~ fun a acc -> append a acc
```

{: .note }
> Beware: our append function automatically filters "Empty" 'a catlists out when calling append function!


#### The rev function based on our fold function
{: .fs-5 }
{: .fw-700 }

```fsharp

```


#### The full the data structure
{: .fs-5 }
{: .fw-700 }


```fsharp

```


