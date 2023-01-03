---
layout: default
title: My first day of learning Python
nav_order: 2
parent: Python
mathjax: true
tags: 
  - python
  - day one
  - learning experience
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


<hr/>
## Opgave 2
### Et palindrom er et ord, som er stavet ens forfra og bagfra. Fx er "ebbe" og "malayalam" palindromer. Du må gerne antage, at der er forskel på store og små bogstaver, så fx "EbbE" er et palindrom, men "Ebbe" ikke er. Du må også gerne antage, at mellemrum hører med til stavningen, så f.eks. "vi ser gammel lemlæstet sæl mellem magre siv" ikke er et palindrom. Erklær en funktion med navn palindrom, med et argument. Funktionen skal returnere True, hvis argumentet er et palindrom, og False ellers.
```python
def palindrom(x:str):
    return x[::-1] == x
  
print(palindrom("ebbe")) #Skal være true
print(palindrom("malayalam")) #Skal være true
print(palindrom("EbbE")) #Skal være true
print(palindrom("Ebbe")) #Skal være false
print(palindrom("vi ser gammel lemlæstet sæl mellem magre siv")) #Skal være false
```

{: .console }
> True
> 
> True
> 
> True
> 
> False
> 
> False

<hr/>

## Opgave 3

### På engelsk starter alle centrale ord med store bogstaver i titler og overskrifer.Erklær en funktion, title, som tager en streng som argument og returner strengen som en titel. Fx så skal title("weekly exercises") returnere "Weekly Exercises". Hvis funktionen skal være helt korrekt, så er det kun centrale ord der skal skrives med stort. Det vil sige, at artikler, (a, an, the), præpositioner (by, for, in) og konjunktioner (and, or, because) kun skal skrives med stort hvis de er det første ord i en titel. Du bestemmer selv om du vil prøve at håndtere disse særtilfælde.


```python
artikler_præpositioner_and_konjunktioner = ['by','for', 'in', 'and', 'or', 'because', 'a', 'an', 'the']
apk=artikler_præpositioner_and_konjunktioner

def title(title):
    x:list[str] = title.split(' ')
    a = ' '.join(str(e) for e in list(map(lambda t : f'{t[:1].upper()}'f'{t[1:]}' if t.lower() not in apk else t, x)))
    return f'{a[:1].upper()}'f'{a[1:]}'
    
print(title("hello world!"))
print(title("weekly exercises"))
print(title("weekly exercises and the beautiful man"))
print(title("and weekly exercises and the beautiful man"))
```

{: .console }
> Hello World! 
> 
> Weekly Exercises 
> 
> Weekly Exercises and the Beautiful Man 
> 
> And Weekly Exercises and the Beautiful Man 

## Opgave 5

```python
def only_odds(liste):
    return liste[1::2]
  
print(only_odds(["Jan", "Feb", "March", "April", "May"]))
print(only_odds([10,20,30,40,50,60,70,80,90,100]))
```

{: .console }
> ['Feb', 'April']
> 
> [20, 40, 60, 80, 100]


