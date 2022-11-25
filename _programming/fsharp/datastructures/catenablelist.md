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

```fsharp
type 'a catlist =
    | Empty // empty node
    | Single of 'a // leaf node
    | Append of 'a catlist * 'a catlist
```

#### What is the use case for the data structure difference list
{: .fs-5 }
{: .fw-700 }



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
let map (f: 'a -> 'b) (xs: 'a catlist) : 'b catlist =
    fold ((fun a b -> Append(a, b)), Empty) (fun a -> single (f a)) xs
```

#### The filter function based on our fold function
{: .fs-5 }
{: .fw-700 }

```fsharp
let map (f: 'a -> 'b) (xs: 'a catlist) : 'b catlist =
    fold ((fun a b -> Append(a, b)), Empty) (fun a -> single (f a)) xs
```

#### The rev function based on our fold function
{: .fs-5 }
{: .fw-700 }

```fsharp
let map (f: 'a -> 'b) (xs: 'a catlist) : 'b catlist =
    fold ((fun a b -> Append(a, b)), Empty) (fun a -> single (f a)) xs
```


