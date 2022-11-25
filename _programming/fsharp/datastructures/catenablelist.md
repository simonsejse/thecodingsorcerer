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
---

# Catenable Lists

#### What is the use case for the data structure difference list
{: .fs-5 }
{: .fw-700 }



#### The fold method
{: .fs-5 }
{: .fw-700 }

{% fsharp %}
```fsharp
let fold (cf: ('a -> 'a -> 'a) * 'a) (t: 'b -> 'a) (list: 'b catlist) : 'a =
    let rec f xs =
        match xs with
        | Empty -> snd cf
        | Single a -> t a
        | Append (ys, zs) -> fst (cf) (f ys) (f zs)

    f list
```
{% endhighlight %}

