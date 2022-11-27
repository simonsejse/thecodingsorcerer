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
let append (c1: 'a catlist) (c2: 'a catlist) : 'a catlist =
    if c1 = Empty then c2
    else if c2 = Empty then c1
    else Append(c1, c2)
```
    

but ye it makes sense, if a is empty then just returns b no matter what, and if b is empty returns a nomatter what and if they not empty then returns them both appended

#### The fold method
{: .fs-5 }
{: .fw-700 }


```fsharp
let fold (cf: ('a -> 'a -> 'a) * 'a) (f: 'b -> 'a) (list: 'b catlist) : 'a =
    let rec g (xs: 'b catlist) =
        match xs with
        | Empty -> snd cf
        | Single (a: 'b) -> f a
        | Append (ys: 'b catlist, zs: 'b catlist) -> fst (cf) (g ys) (g zs)

    g list
```



#### The map function based on our fold function
{: .fs-5 }
{: .fw-700 }

```fsharp
let map (f: 'a -> 'b) (xs: 'a catlist) : 'b catlist =
    fold ((append), Empty) (fun (b: 'a) -> single (f b)) xs
//append ~ fun a acc -> append a acc
```

#### The filter function based on our fold function
{: .fs-5 }
{: .fw-700 }

```fsharp
let filter (f: 'a -> bool) (xs: 'a catlist) : 'a catlist =
    fold
        ((fun (a: 'a catlist) (acc: 'a catlist) -> append a acc), Empty)
        (fun (b: 'a) -> if f b then single b else Empty)
        xs
//append ~ fun a acc -> append a acc
```

{: .note }
> Beware: our append function automatically filters "Empty" 'a catlists out when calling append function!


#### The rev function based on our fold function
{: .fs-5 }
{: .fw-700 }

```fsharp
let rev (c: 'a catlist) : 'a catlist =
    fold ((fun (a: 'a catlist) (acc: 'a catlist) -> append acc a), Empty) (fun (b: 'a) -> single b) c
```


#### The full the data structure
{: .fs-5 }
{: .fw-700 }


```fsharp
module CatList

open DiffList

type 'a catlist =
    | Empty
    | Single of 'a
    | Append of 'a catlist * 'a catlist

let nil: 'a catlist = Empty

let single (elm: 'a) : 'a catlist = Single(elm)

let append (c1: 'a catlist) (c2: 'a catlist) : 'a catlist =
    if c1 = Empty then c2
    else if c2 = Empty then c1
    else Append(c1, c2)

let cons (elm: 'a) (xs: 'a catlist) : 'a catlist = append (single elm) xs

let snoc (xs: 'a catlist) (elm: 'a) : 'a catlist = append xs (single elm)

let fold (cf: ('a -> 'a -> 'a) * 'a) (f: 'b -> 'a) (list: 'b catlist) : 'a =
    let rec g (xs: 'b catlist) =
        match xs with
        | Empty -> snd cf
        | Single (a: 'b) -> f a
        | Append (ys: 'b catlist, zs: 'b catlist) -> fst (cf) (g ys) (g zs)

    g list

let length (xs: 'a catlist) = fold ((+), 0) (fun _ -> 1) xs

let map (f: 'a -> 'b) (xs: 'a catlist) : 'b catlist =
    fold ((append), Empty) (fun (b: 'a) -> single (f b)) xs

let filter (f: 'a -> bool) (xs: 'a catlist) : 'a catlist =
    fold
        ((fun (a: 'a catlist) (acc: 'a catlist) -> append a acc), Empty)
        (fun (b: 'a) -> if f b then single b else Empty)
        xs

let rev (c: 'a catlist) : 'a catlist =
    fold ((fun (a: 'a catlist) (acc: 'a catlist) -> append acc a), Empty) (fun (b: 'a) -> single b) c

let fromCatList (xs: 'a catlist) : 'a list = fold ((@), []) (fun b -> [ b ]) xs


//val fold:folder: ('State -> 'T -> 'State) -> state : 'State -> list  : list<'T> -> 'State
let inline (^@) s elm = append s (single elm)
let toCatList (xs: 'a list) : 'a catlist = xs |> List.fold ((^@)) Empty

let item (i: int) (xs: 'a catlist) : 'a =
    if (i >= 0) && (i < length xs) then
        fromCatList(xs).[i]
    else
        raise (System.IndexOutOfRangeException("Du har valgt et index ude for størrelsen på catlisten!"))


let insert (i: int) (elm: 'a) (xs: 'a catlist) : 'a catlist =
    if (i >= 0) && (i < length xs) then
        let t = fromCatList xs
        let f = t.[0 .. i - 1]
        let f1 = t.[i - 1 .. t.Length]
        let newList = f @ [ elm ] @ f1
        toCatList newList
    else
        raise (System.IndexOutOfRangeException("Du har valgt et index ude for størrelsen på catlisten!"))


let delete (i: int) (xs: 'a catlist) : 'a catlist =
    if (i >= 0) && (i < length xs) then
        let t = fromCatList xs
        let f = t.[0 .. i - 2]
        let f1 = t.[i .. t.Length]
        toCatList (f @ f1)
    else
        raise (System.IndexOutOfRangeException("Du har valgt et index ude for størrelsen på catlisten!"))
```


