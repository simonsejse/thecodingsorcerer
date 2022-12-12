---
layout: default
title: Classes in F#
parent: FSharp
nav_order: 1
---


# Class and inheritance
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

#### What is a Catenable list?
{: .fs-5 }
{: .fw-700 }

Catenable lists are lists with efficient (constant-time) appending, $$O(1)$$ time complexity, like difference lists, and additional
operations. We were taught that they are widely used to implement text processing systems such as text editors, where
characters and text fragments need to be inserted and deleted efficiently, which is why arrays holding
the text are not used.
