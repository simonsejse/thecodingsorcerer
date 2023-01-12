---
layout: default
title: Second week of Python
nav_order: 3
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
import csv
import traceback


class Step:
    """Step"""

    def apply(self, inp):
        pass

    def description(self) -> str:
        return self.__doc__.lower()


class DoNothing(Step):
    """Do nothing"""

    def apply(self, inp):
        return inp

    def description(self) -> str:
        return self.__doc__.lower()


class AddConst(Step):
    """Add a constant"""

    def __init__(self, c) -> None:
        self.c = c

    def apply(self, inp) -> None:
        return self.c + inp

    def description(self) -> str:
        return f'add {self.c}'


class Repeater(Step):
    """Repeat numbers"""

    def __init__(self, num) -> None:
        self.num = num

    def apply(self, num) -> list[int]:
        if self.num < 0:
            return []
        return [num] * self.num

    def description(self) -> str:
        return f'repeat {self.num} times, as a list'


class GeneralSum(Step):
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


class Map(Step):
    """Map konstrueres ved at give et andet Step.
    Hvor apply skal kalde apply på det indre step, for alle elementer i input og returnere resultatet."""

    def __init__(self, step) -> None:
        super().__init__()
        self.step = step

    def apply(self, inp):
        if not (isinstance(inp, list)):
            raise Exception("You did not provide a list for mapping!")
        S = []
        for x in inp:
            S.append(self.step.apply(x))
        return S

    def description(self) -> str:
        return f'{self.step.description()} to each element'


class Pipeline(Step):
    def __init__(self, steps) -> None:
        super().__init__()
        self.steps = steps

    def add_step(self):
        self.steps.append(self)

    def apply(self, inp):
        c = 0
        try:
            r = self.steps[0].apply(inp)
            for x in range(1, len(self.steps)):
                c += 1
                r = self.steps[x].apply(r)
            return r
        except Exception as e:
            traceback.print_exc()
            output = "Error is at STEP {}\nAfter succesful steps:\n{}\n".format(
                c, '\n'.join("   " + x.description() for x in self.steps[:c]))
            print(output)

    def description(self) -> str:
        return ' -> '.join(x.description() for x in self.steps)


class CsvReader(Step):
    """Read file"""

    def __init__(self) -> None:
        super().__init__()

    def apply(self, filename):
        with open(filename, newline='') as csvfile:
            critter_reader = csv.reader(csvfile)
            return [{
                    "execution": row[0],
                    "dob": row[1],
                    "doo": row[2],
                    "highest_edu_level": row[3],
                    "last_name": row[4],
                    "first_name": row[5],
                    "tdcj_number": row[6],
                    "age_at_execution": row[7],
                    "date_recieved": row[8],
                    "execution_date": row[9],
                    "race": row[10],
                    "county": row[11],
                    "eye_color": row[12],
                    "weight": row[13],
                    "height": row[14],
                    "native_county": row[15],
                    "native_state": row[16],
                    "last_statement": row[17]
                    } for row in critter_reader][1:]


class CritterStats(Step):
    """Create dict with key-value pairs"""

    def __init__(self) -> None:
        super().__init__()

    def apply(self, dict_list):
        S = {}
        for x in dict_list[1:]:
            color = x['eye_color']
            if not color:
                continue
            S[color] = S[color] + 1 if color in S else 1
        return S

class ShowAsciiBarchart(Step):
    """Showing Ascii Bar Chart"""

    def __init__(self) -> None:
        super().__init__()

    def apply(self, data):
        for x, y in data:
            print(x, y)


initialState = ""
ls = Pipeline([
        Append("I can be used as a StringBuilder too! "), #I can be used as a StringBuilder too! 
        Append("Isn't that pretty cool?"), #I can be used as a StringBuilder too! Isn't that pretty cool?
        WrapList(), #[I can be used as a StringBuilder too! Isn't that pretty cool?]
        Append(5),
        Map(Stringify()),
        Append("New element"), #[I can be used as a StringBuilder too! Isn't that pretty cool?, "New element"]
        Append("Now all these will have arrows soon"), #[I can be used as a StringBuilder too! Isn't that pretty cool?, "New element", "Now all these will have arrows soon"]
        Append("It's very cool!"), #[..., "Now all these will have arrows soon", "It's very cool!]
        Unwrap(' -> '), #Joins the list using the seperator -> 
    ])
result = ls.description()
print(result)


class WrapList(Step):
    """Wrap to a list"""

    def __init__(self) -> None:
        super().__init__()

    def apply(self, x):
        return [x]


class Append(Step):
    """Append element to str/list"""

    def __init__(self, num) -> None:
        super().__init__()
        self.num = num

    def apply(self, xs):
        if isinstance(xs, str):
            return xs + self.num
        elif isinstance(xs, list):
            return xs + [self.num]
        else:
            raise Exception("You did not provide a string or list for append")


class Sort(Step):
    """Sort a list"""

    def __init__(self, reverse=False) -> None:
        super().__init__()
        self.reverse = reverse

    def apply(self, xs):
        if not (isinstance(xs, list)):
            raise Exception("You did not provide a list for sort")
        return sorted(xs, reverse=self.reverse)


class Square(Step):
    """Take sqrt"""

    def __init__(self) -> None:
        super().__init__()

    def apply(self, x):
        if not isinstance(x, (int, float, complex)):
            raise Exception("You did not provide a number to square!")
        return x*x


class Cube(Step):
    """Take cube root"""

    def __init__(self) -> None:
        super().__init__()

    def apply(self, x):
        if not isinstance(x, (int, float, complex)):
            raise Exception("You did not provide a number to cube!")
        return x*x*x


class Average(Step):
    """Take average of a list"""

    def __init__(self) -> None:
        super().__init__()

    def apply(self, xs):
        if (isinstance(xs, list)):
            return sum(xs) / len(xs)
        elif (isinstance(xs, dict)):
            return sum(xs.values()) / len(xs)
        else:
            raise Exception(
                "You input is not a str or a list to find the average of!")
    
class Equal(Step):
    def __init__(self, o) -> None:
        super().__init__()
        self.o = o

    def apply(self, o):
        return self.o == o
    def description(self) -> str:
        return f'Returns true/false based on a equality constraint on (Element:{self.o})'
    
class Unwrap(Step):
    def __init__(self, sep) -> None:
        super().__init__()
        self.sep = sep

    def apply(self, xs):
        return f'{self.sep}'.join(xs)
    def description(self) -> str:
        return f'Unwraps list by seperator (Sep:\'{self.sep}\')'
    
class Get(Step):
    def __init__(self, key) -> None:
        super().__init__()
        self.key = key

    def apply(self, xs=None):
        if isinstance(xs, dict) and isinstance(self.key, str):
            return xs[self.key]
        elif isinstance(xs, list) and isinstance(self.key, (int, float, complex)):
            return xs[self.key]
        else:
            raise Exception("Something wen't wrong trying to fetch the associated value mapped to the specified {}".format(self.key))
    def description(self) -> str:
        return f'Get value associated to (key: {self.key})'
    
class Stringify(Step):
    """Stringify"""
    def __init__(self) -> None:
        super().__init__()

    def apply(self, inp):
        return str(inp)

```

```python
import unittest
from pipeline import *


class TestStringMethods(unittest.TestCase):
    def test_add_const(self):
      adderOne = AddConst(1)
      self.assertNotEqual(adderOne.c, 42)
      self.assertEqual(adderOne.apply(41), 42)
      
    def test_repeater(self):
      r = Repeater(5)
      self.assertListEqual(r.apply(42), [42, 42, 42, 42, 42])
      
    def test_repeater_negative(self):
      r = Repeater(-1000)
      self.assertListEqual(r.apply(42), [])
    
    def test_sum(self):
      summer = SumNum()
      x = summer.apply([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
      self.assertEqual(x, 55)
    
    def test_product(self):
        p = ProductNum()
        x = p.apply([1,2,3,4,5])
        self.assertEqual(x, 120)
        
    def test_pipeline(self):
      initialState = 0
      pipeline = Pipeline([
        AddConst(10), #10
        Repeater(5), #[10, 10, 10, 10, 10]
        Map(Square()), #[100, 100, 100, 100, 100]
        SumNum(), #500
        WrapList(), #[500]
        Append(10), #[500, 10]
        Average() #(500 + 10)/2=255
      ])
      result = pipeline.apply(initialState)
      self.assertEqual(result, 255)
      
    def test_pipeline_description(self):
      pipeline = Pipeline([
        AddConst(10), #10
        Repeater(5), #[10, 10, 10, 10, 10]
        Map(Square()), #[100, 100, 100, 100, 100]
        SumNum(), #500
        WrapList(), #[500]
        Append(10), #[500, 10]
        Average() #(500 + 10)/2=255
      ])
      description = pipeline.description()
      self.assertEqual(description, 
                       'add 10 -> repeat 5 times, as a list -> take sqrt to each element -> '
                       'sum of all elements -> wrap to a list -> append element to str/list -> '
                       'take average of a list')  
    
    def test_critters_stats_blue(self):
      initialState = ""
      ls = Pipeline([
        Append("./tx_deathrow_full.csv"),
        CsvReader(),
        CritterStats(),   
        Get('Blue'),
        Equal(85)
      ])
      result = ls.apply(initialState)
      self.assertTrue(result)
    
    def test_critters_stats_brown(self):
      initialState = ""
      ls = Pipeline([
        Append("./tx_deathrow_full.csv"),
        CsvReader(),
        CritterStats(),   
        Get('Brown'),
      ])
      result = ls.apply(initialState)
      self.assertEqual(result, 351)
    
    def test_critters_stats_average(self):
      initialState = ""
      ls = Pipeline([
        Append("./tx_deathrow_full.csv"),
        CsvReader(),
        CritterStats(),   
        Average()
      ])
      result = ls.apply(initialState)
      self.assertEqual(result, 57.55555555555556)
       
    def test_critters_stats_average_description(self):
      ls = Pipeline([
        Append("./tx_deathrow_full.csv"),
        CsvReader(),
        CritterStats(),   
        Average()
      ])
      result = ls.description()
      self.assertEqual(result, 'append element to str/list -> read file -> '
                       'create dict with key-value pairs -> take average of a list')
      
      
    def test_critters_stats_description(self):
      ls = Pipeline([
        Append("./tx_deathrow_full.csv"),
        CsvReader(),
        CritterStats(),   
        Get('Blue'),
        Equal(85)
      ])
      description = ls.description()
      self.assertEqual(description, 
                        'append element to str/list -> read file -> create dict with key-value pairs -> '
                        'Get value associated to (key: Blue) -> '
                        'Returns true/false based on a equality constraint on (Element:85)')
    def test_sort(self):
      initialState = []
      ls = Pipeline([
              Append(1),
              Append(2),
              Append(3),
              Append(4),
              Append(5),
              Append(6),
              Append(7),
              Append(8),
              Append(9),
              Append(10),
              Sort()
      ])
      self.assertListEqual(ls.apply(initialState), [1,2,3,4,5,6,7,8,9,10])
            
    def test_sort_reversed(self):
      initialState = []
      ls = Pipeline([
              Append(1),
              Append(2),
              Append(3),
              Append(4),
              Append(5),
              Append(6),
              Append(7),
              Append(8),
              Append(9),
              Append(10),
              Sort(reverse=True)
      ])
      self.assertListEqual(ls.apply(initialState), [10,9,8,7,6,5,4,3,2,1])
    def test_use_pipeline_as_string_builder(self):
      initialState = ""
      ls = Pipeline([
              Append("I can be used as a StringBuilder too! "), #I can be used as a StringBuilder too! 
              Append("Isn't that pretty cool?"), #I can be used as a StringBuilder too! Isn't that pretty cool?
              WrapList(), #[I can be used as a StringBuilder too! Isn't that pretty cool?]
              Append(5),
              Map(Stringify()),
              Append("New element"), #[I can be used as a StringBuilder too! Isn't that pretty cool?, "New element"]
              Append("Now all these will have arrows soon"), #[I can be used as a StringBuilder too! Isn't that pretty cool?, "New element", "Now all these will have arrows soon"]
              Append("It's very cool!"), #[..., "Now all these will have arrows soon", "It's very cool!]
              Unwrap(' -> '), #Joins the list using the seperator -> 
            ])
      
      self.assertEqual(ls.apply(initialState), 
                       "I can be used as a StringBuilder too! Isn't that pretty cool? -> 5 -> "
                       "New element -> Now all these will have arrows soon -> It's very cool!")
    def test_use_pipeline_as_string_builder_description(self):
      ls = Pipeline([
              Append("I can be used as a StringBuilder too! "), #I can be used as a StringBuilder too! 
              Append("Isn't that pretty cool?"), #I can be used as a StringBuilder too! Isn't that pretty cool?
              WrapList(), #[I can be used as a StringBuilder too! Isn't that pretty cool?]
              Append(5),
              Map(Stringify()),
              Append("New element"), #[I can be used as a StringBuilder too! Isn't that pretty cool?, "New element"]
              Append("Now all these will have arrows soon"), #[I can be used as a StringBuilder too! Isn't that pretty cool?, "New element", "Now all these will have arrows soon"]
              Append("It's very cool!"), #[..., "Now all these will have arrows soon", "It's very cool!]
              Unwrap(' -> '), #Joins the list using the seperator -> 
            ])
      
      self.assertEqual(ls.description(), 
                       "append element to str/list -> append element to str/list -> wrap to a list -> "
                       "append element to str/list -> stringify to each element -> "
                       "append element to str/list -> append element to str/list -> "
                       "append element to str/list -> Unwraps list by seperator (Sep:' -> ')")

if __name__ == '__main__':
    unittest.main()
```
