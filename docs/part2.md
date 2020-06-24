# Part 2

## 1. Classes
Define a class Circle that stores position in  the x-y plane and its radius

```python
>>> from pytest import approx
>>> from math import pi

>>> class Circle:
...     ...

```

* Let the default behaviour be a unit circle centered around origo
* When we supply arguments to the class constructor the circle shape and position is saved

Default behaviour means here to provide non-required (keyword) parameters to the
constructor function `__init__` . If we have `x`, `y` for the center of a
circle and `r` for its radius

~~~
    def __init__(self, x=0.0, y=0.0, r=1.0):
        self.x = x
        self.y = y
        self.r = r
~~~


* Add class methods for calculating the area and circumference

Given that these value are saved for a circle object
the functions for area and circumference are straightforward

~~~
    def area(self):
        return pi*self.r**2

    def circumference(self):
        return 2*pi*self.r
~~~


* Define an overloading method such that we can compare two Circle objects, being equal if they have same radius and center. Use any comparison between floats with the pytest approx function


Equality between two Python objects has to be defined in the class,
if it is supposed to mean anything different from object identity.

e.g.

~~~
>>> class Foo:
...    pass
>>> Foo() == Foo()
False

~~~
returns False just because we have two different objects. To define equality
for a class, the `==` operator is implemented by the `__eq__` method
If we define two Circle objects to be equal when they have the same center and
radius we could implement, allowing for round-off errors that we have with
`float` operations

~~~
    def __eq__(self, other):
        return self.x == approx(other.x) and self.y == approx(other.y) and self.r == approx(other.r)
~~~

Then a total solution is

~~~
>>> class Circle:
...     ### BEGIN SOLUTION
...     def __init__(self, x=0.0, y=0.0, r=1.0):
...         self.x = x
...         self.y = y
...         self.r = r
...         
...     def area(self):
...         return pi*self.r**2
...     
...     def circumference(self):
...         return 2*pi*self.r
...     
...     def __eq__(self, other):
...         return self.x == approx(other.x) and self.y == approx(other.y) and self.r == approx(other.r)

~~~

satisfying tests

```python
>>> circ = Circle()
>>> assert circ.x == approx(0.0)
>>> assert circ.y == approx(0.0)
>>> assert circ.r == approx(1.0)

```


```python
>>> circ = Circle(r=2)
>>> assert circ.x == approx(0.0)
>>> assert circ.y == approx(0.0)
>>> assert circ.r == approx(2.0)
>>> 
>>> circ = Circle(x=1, y=1)
>>> assert circ.x == approx(1.0)
>>> assert circ.y == approx(1.0)
>>> assert circ.r == approx(1.0)

```


```python
>>> assert Circle().area() == approx(pi)
>>> assert Circle().circumference() == approx(2*pi)
>>> assert Circle(r=2).area() == approx(4*pi)
>>> assert Circle(r=3).circumference() == approx(6*pi)

```

```python
>>> circ_a = Circle(x=1, y=2, r=3)
>>> circ_b = Circle(x=1, y=2, r=3)
>>> assert circ_a == circ_b

```
