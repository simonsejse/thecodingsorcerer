---
layout: default
title: Functional Interface in Java
nav_order: 2
parent: Java
mathjax: true
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
### Examples of how to use fold
```java
Integer sum = fold((Integer::sum), 0, List.of(10, 10,20));

public static Integer length(int state, int a) {
    return state + 1;
}
Integer length = fold((Application::length), 0, List.of(10, 10,20));

public static List<Integer> incrementByOne(List<Integer> state, int a) {
    //If we don't add (0) as starting index it will be reversed list
    state.add(0, a + 1);
    return state;
}
List<Integer> incrementByOne = fold((Application::incrementByOne), new ArrayList<>(), List.of(10, 10,20));
```

Most of these have a running time on $$O(n)$$ time since they require to go throuch each element of the list and do something.
