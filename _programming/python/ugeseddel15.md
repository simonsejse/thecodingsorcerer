---
layout: default
title: Functional Interface in Java
nav_order: 2
parent: Python
mathjax: true
tags: 
  - fold method
  - fold
  - functional interface in java
  - java
has_toc: true
---

# Ugeseddel 15
{: .py-1 }
{: .m-1 }

<details markdown="block">
  <summary>
    Contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

<hr/>

## Løs opgave 3ø0 – 3ø3 fra ugeseddel 3i, samt opgave 8ø0, 8ø2 – 8ø4 men denne gang i Python. Kig gerne på jeres gamle besvarelser i F#. Bemærk at List i Python minder mere om typen array i F#, frem for typen list.

### 3ø0 In the following, you are to work with different ways to create a list:

#### Make an empty list, and bind it with the name lst.
```python
lst = []
print(lst)
```

#### Create a second list lst2, which prepends the string "F#" to lst using the cons operator::. Consider whether the types of the old and new list are the same.
```python
lst.append("F#")
lst2 = lst
print(lst2)
```

#### Create a third list lst3 which consists of 3 identical elements "Hello", and which is created with List.init and the anonymous function fun i -> "Hello".
```python
lst3 = ["Hello" for _ in range(3)]
print(lst3)
lst3 = ["Hello"] * 3
print(lst3)
```
#### Create a fourth list lst4 which is a concatenation of lst2 and lst3 using “@”.
```python
lst4 = lst2 + lst3
print(lst4)
```
#### Create a fifth list lst5 as [1; 2; 3] using List.init
```python
lst5 = [i for i in range(1, 4)]
print(lst5)
```

#### Write a recursive function oneToN : n:int -> int list which uses the concatenation operator, “@”, and returns the list of integers [1; 2; ...; n]. Consider whether it would be easy to create this list using the “::” operator.
```python
def oneToN(n):  
  return [1] if n == 1 else oneToN(n - 1) + [n]

def oneToNAcc(n, acc):
  return [1] + acc if n == 1 else oneToNAcc(n - 1, [n] + acc)
```
#Tail recursion does not even work in Python, or it does work, but tail recursion is the same as T
```python
print(oneToNAcc(9991, []))
```
#### Write a recursive function oneToNRev : n:int -> int list which uses the cons op-erator, “::”, and returns the list of integers [n; ...; 2; 1]. Consider whether it would be easy to create this list using the “@” operator.
```python
def oneToNRev(n):  
  return [1] if n == 1 else [n] + oneToNRev(n - 1)
  
print(oneToNRev(10))
```

## Opgave 1

### Erklær en funktion, forekomster, som tager to argumenter: en List arr og et potentielt element x. Funktionen skal returnere hvormange gange x forekommer i arr. Fx skal kaldet forekomster(["Napoleon", "Wellington", "Bonaparte", "Wellington"], "Wellington") returnere 2, og ligeledes skal forekomster([1,2,3,4,5,1,5,4,3,2,1], 1) returnere 3.
```python
def forekomster(arr, x):
    return arr.count(x)

print(forekomster(["Napoleon", "Wellington",
"Bonaparte", "Wellington"], "Wellington"))

print(forekomster([1,2,3,4,5,1,5,4,3,2,1], 1))
```

{: .console }
 > 2
 > 
 > 3
