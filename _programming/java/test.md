---
layout: default
title: The fold method in Java
nav_order: 2
parent: Java
tags: 
  - fold method
  - fold
  - functional interface in java
  - java
has_toc: true
---

# The fold method in Java
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


#### Fold method
{: .fs-5 }
{: .fw-700 }
```java
 public static <A, B> A fold(BiFunction<A,B,A> folder, A s, List<B> bList){
      if (bList.isEmpty()) return s;
      B b = bList.get(0);
      return folder.apply(fold(folder, s, bList.subList(1, bList.size())), b);
  }
```
#### Examples of use
{: .fs-5 }
{: .fw-700 }
```java
 Integer fold = fold((Integer::sum), 0, List.of(10, 10,20));
```
