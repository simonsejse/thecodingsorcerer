---
layout: default
title: Second week of Python
nav_order: 2
parent: Python
mathjax: true
tags: 
  - python
  - second week
  - learning experience
has_toc: true
---

# Ugeseddel 12
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

```python
from OpgaveA import *
import operator


def test(s, a, b, f, isWhitebox=False):
    print("[WHITEBOX] " if isWhitebox else "[BLACKBOX] ", end="")
    match f(a, b):
        case True:
            print(f'PASS:     {s}')
        case False:
            print(f'FAIL:     {s}')


class TestCase:
    TEST_1 = "AddConst(1).c does NOT equal 42"
    TEST_2 = "AddConst(1).apply(41) equals 42"
    TEST_3 = "Repeater(5).apply(3) is equal to [3, 3, 3, 3, 3]"
    TEST_4 = "Repeater(-3).apply() would always return []"
    TEST_5 = "Sum of [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] is 55"
    TEST_6 = "The product of [1, 2, 3, 4, 5] is 120"


# AddConst
adderOne = AddConst(1)
test(TestCase.TEST_1, adderOne.c, 42, operator.ne)

adderOne.apply(41)
test(TestCase.TEST_2, adderOne.c, 42, operator.eq)

# Repeater
quintiplyer = Repeater(5)
xs = [42, 42, 42, 42, 42]
test(TestCase.TEST_3, quintiplyer.apply(42), xs, operator.eq, True)

plyer = Repeater(-3)
test(TestCase.TEST_4, plyer.apply(10), [], operator.eq, True)


# SumNum
summer = SumNum()
xs = summer.apply([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
test(TestCase.TEST_5, xs, 55, operator.eq)

# ProductNum
proder = ProductNum()
xs = proder.apply([1, 2, 3, 4, 5])
test(TestCase.TEST_6, xs, 120, operator.eq)

pipeline = Pipeline([
    AddConst(45),
    Repeater(3),
    Map(AddConst(-3)),
    DoNothing(),
    ProductNum(),
    Repeater(4),
    SumNum()
])
ls = Pipeline([
    AddConst(45),
    Repeater(3),
    Repeater(3),
    Repeater(3),
    AddConst(-3),  # Can't add const since it's a list [45, 45, 45]
    DoNothing(),
    ProductNum()
])
pipeline.apply(0)
print(pipeline.description())

```

```python
import traceback


class DoNothing:
    """Do nothing"""

    def apply(self, inp):
        return inp

    def description(self) -> str:
        return self.__doc__.lower()


class AddConst(DoNothing):
    """Add a constant"""

    def __init__(self, c) -> None:
        self.c = c

    def apply(self, inp) -> None:
        return self.c + inp

    def description(self) -> str:
        return f'add {self.c}'


class Repeater(DoNothing):
    """Repeat numbers"""

    def __init__(self, num) -> None:
        self.num = num

    def apply(self, num) -> list[int]:
        if self.num < 0:
            return []
        return [num] * self.num

    def description(self) -> str:
        return f'repeat {self.num} times, as a list'


class GeneralSum(DoNothing):
    def __init__(self, elm, operator) -> None:
        self.elm = elm
        self.operator = operator


class SumNum(GeneralSum):
    """Sum of all elements"""

    def __init__(self) -> None:
        super().__init__(0, lambda x, y: x + y)

    def apply(self, xs) -> int:
        return sum(xs, self.elm)


class ProductNum(GeneralSum):
    """Multiply all elements"""

    def __init__(self) -> None:
        super().__init__(1, lambda x, y: x * y)

    def apply(self, xs) -> int:
        s = self.elm
        for x in xs:
            s *= x
        return s


class Map(DoNothing):
    """Map konstrueres ved at give et andet Step.
    Hvor apply skal kalde apply på det indre step, for alle elementer i input og returnere resultatet."""

    def __init__(self, step) -> None:
        super().__init__()
        self.step = step

    def apply(self, inp):
        S = []
        for x in inp:
            S.append(self.step.apply(x))
        return S

    def description(self) -> str:
        return f'{self.step.description()} to each element'


class Pipeline(DoNothing):
    def __init__(self, steps) -> None:
        super().__init__()
        self.steps = steps

    def apply(self, inp):
        c = 0
        try:
            r = self.steps[0].apply(inp)
            for x in range(1, len(self.steps)):
                c += 1
                r = self.steps[x].apply(r)
            return r
        except Exception as e:
            print(traceback.format_exc())

            output = "Error is at STEP {}\nAfter succesful steps:\n{}\n".format(
                c, '\n'.join("   " + x.description() for x in self.steps[:c]))
            print(output)

    def description(self) -> str:
        return ' -> '.join(x.description() for x in self.steps)


# Exception: Error 'can only concatenate list (not "int") to
# list' at step 3 'add -3',
# after succesful steps:
 # add 45
 # repeat 3 times, as a list

```
