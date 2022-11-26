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


#### The append method
```fsharp
let append c1 c2 =
    if c1 = Empty then c2
    else if c2 = Empty then c1
    else Append(c1, c2)
```
    
{: .fs-5 }
{: .fw-700 }
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

```

#### The filter function based on our fold function
{: .fs-5 }
{: .fw-700 }

```fsharp

```

{: .note }
> 
> Beware: our append function automaticcaly filters "Empty" catlists out when calling append function


#### The rev function based on our fold function
{: .fs-5 }
{: .fw-700 }

```fsharp


```


#### The full the data structure
{: .fs-5 }
{: .fw-700 }


```fsharp
module Catlist

type 'a catlist =
    | Empty // empty node
    | Single of 'a // leaf node
    | Append of 'a catlist * 'a catlist

//val nil : 'a catlist // the empty list
let nil: 'a catlist = Empty

//val single : 'a -> 'a catlist // singleton list
let single = (Single)

//val append : 'a catlist -> 'a catlist -> 'a catlist // append
let append c1 c2 =
    if c1 = Empty then c2
    else if c2 = Empty then c1
    else Append(c1, c2)

//val cons: 'a -> 'a catlist -> 'a catlist // cons / prepend
let cons (a: 'a) (c: 'a catlist) = append (single a) c

//val snoc : 'a catlist -> 'a -> 'a catlist // snoc / postpend
let snoc (c: 'a catlist) (a: 'a) = append c (single a)

let rec sum =
    function
    | Empty -> 0
    | Single a -> a
    | Append (ys, zs) -> sum (ys) + sum (zs)

//fold : (('a -> 'a -> 'a) * 'a) -> ('b -> 'a) -> 'b catlist -> 'a
let fold (cf: ('a -> 'a -> 'a) * 'a) (t: 'b -> 'a) (list: 'b catlist) : 'a =
    let rec f xs =
        match xs with
        | Empty -> snd cf
        | Single a -> t a
        | Append (ys, zs) -> fst (cf) (f ys) (f zs)

    f list
//val length: xs: catlist<'a> -> int
let length xs = fold ((+), 0) (fun _ -> 1) xs
let sum' xs = fold ((+), 0) (fun b -> b) xs


//val map : ('a -> 'b) -> 'a catlist -> 'b catlist
let map (f: 'a -> 'b) (xs: 'a catlist) : 'b catlist =
    fold ((append), Empty) (fun b -> single (f b)) xs
//append ~ fun a acc -> append a acc

//val filter : ('a -> bool ) -> 'a catlist -> 'a catlist
//Beware: our append function automaticcaly filters "Empty" catlists out when calling append function
let filter (f: 'a -> bool) (xs: 'a catlist) : 'a catlist =
    fold ((fun a acc -> append a acc), Empty) (fun b -> if f b then single b else Empty) xs
//append ~ fun a acc -> append a acc

//let rev (c: 'a catlist) : 'a catlist =
let rev (c: 'a catlist) : 'a catlist =
    fold ((fun a acc -> append acc a), Empty) (fun b -> single b) c
```


