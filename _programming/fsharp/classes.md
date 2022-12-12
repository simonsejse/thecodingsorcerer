---
layout: default
title: Classes in F#
parent: FSharp
nav_order: 1
---


# Nedarvning i F#
{: .py-1 }
{: .m-1 }
Week 12
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


#### Hvornår bruger man inheritance?
{: .fs-5 }
{: .fw-700 }

```fsharp
type car(f: float) =
    do printfn "Creating a car object"
    member this.wheels = 4
    member this.fuelEfficiency = f

type bike(_weight: float) =
    do printfn "Creating a bike object"
    member this.wheels = 2
    member this.weight = _weight
```
Som vi kan se, har vi to klasser, som hver især har de tilfælles, at de er køretøjer, og at de begge har hjul samt deres constructors er ens at de printer en tekst ud på skærmen, altså en sideeffekt. Yderligere, har de dog nogle properties som er forskellige til hinanden, altså hhv. `fuelEfficiency` samt `weight`.

Det her kan laves meget mere elegant ved brug af inhertiance, altså nedarvning, hvor at vores child classes, bike og car begge inheriter fra deres parent class vehicle, som ses herunder.

```fsharp
type vehicle (w : int) = 
  do printfn "Creating a vehicle object")
  member this.wheels = w
  
type car(f: float) =
    inherit vehicle(4)
    do printfn "Creating a car object"
    member this.fuelEfficiency = f

type bike(_weight: float) =
    inherit vehicle(2
    do printfn "Creating a bike object"
    member this.weight = _weight
```

Og nu har vi opnået et hierarkisk familieskab for begge to.
Eksempler på at instantiere kunne nu se således her ud:
```fsharp
let aVehicle = vehicle 5
let car = car 55.7
let bike = bike 23.6
```


#### (parent class, child class) or (super class, sub class) or (base class, derived class)
{: .fs-5 }
{: .fw-700 }
