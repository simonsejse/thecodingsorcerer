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

{% highlight some_language %}
```fsharp
let fold' (folder: 'a -> 'b -> 'a) (s: 'a) (l: 'b list) : 'a =
    let rec f xs =
        match xs with
        | [] -> s
        | x :: xs' -> folder (f xs') x

    f l
```
{% endhighlight %}

