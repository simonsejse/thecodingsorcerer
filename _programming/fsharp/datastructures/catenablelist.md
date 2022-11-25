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


{: .warning }
> ```fsharp
> //Beware: our append function automaticcaly filters "Empty" catlists out when calling append function
> ```

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


