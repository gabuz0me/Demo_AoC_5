Version 4.5

---

# Advent of Code - Day 5

<!-- @import "[TOC]" {cmd="toc" depthFrom=2 depthTo=6 orderedList=false} -->
<!-- code_chunk_output -->

- [1 - Introduction](#1---introduction)
- [2 - The day 5 problem](#2---the-day-5-problem)
  - [2.1 - The first part of the problem](#21---the-first-part-of-the-problem)
    - [2.1.1 - Formalization](#211---formalization)
    - [2.1.2 - Writing some code](#212---writing-some-code)
      - [2.1.2.1 - Python 101](#2121---python-101)
        - [2.1.2.1.1 - The basics](#21211---the-basics)
        - [2.1.2.1.2 - About OOP](#21212---about-oop)
      - [2.1.2.2 - Creating the base objects](#2122---creating-the-base-objects)
        - [2.1.2.2.1 - Shifters](#21221---shifters)
        - [2.1.2.2.2 - Intermediate Function](#21222---intermediate-function)
        - [2.1.2.2.3 - Importing the data](#21223---importing-the-data)
        - [2.1.2.2.4 - Creating the final function](#21224---creating-the-final-function)
    - [2.1.3 - Solving Part One](#213---solving-part-one)
  - [2.2 - The Second part of the problem](#22---the-second-part-of-the-problem)
    - [2.2.1 - Naive approach](#221---naive-approach)
    - [2.2.2 - Complexity](#222---complexity)
      - [2.2.2.1 - What is complexity?](#2221---what-is-complexity)
        - [2.2.2.1.1 - Examples](#22211---examples)
        - [2.2.2.1.1.1 - The Third Value](#222111---the-third-value)
        - [2.2.2.1.1.2 - The minimum of the list](#222112---the-minimum-of-the-list)
        - [2.2.2.1.1.3 - Multiplication Table](#222113---multiplication-table)
        - [2.2.2.1.2 - Formalization](#22212---formalization)
      - [2.2.2.2 - Other ways to express complexity](#2222---other-ways-to-express-complexity)
        - [2.2.2.2.1 - On other measures of algorithmic complexity](#22221---on-other-measures-of-algorithmic-complexity)
        - [2.2.2.2.2 - On parallelization](#22222---on-parallelization)
      - [2.2.2.3 - Finding the complexity of our algorithm](#2223---finding-the-complexity-of-our-algorithm)
    - [2.2.3 - Another perspective](#223---another-perspective)
      - [2.2.3.1 - The solution layed in front of our eyes since the beginning](#2231---the-solution-layed-in-front-of-our-eyes-since-the-beginning)
      - [2.2.3.2 - Introducing ranges](#2232---introducing-ranges)
      - [2.2.3.3 - On Complexity... again!](#2233---on-complexity-again)
      - [2.2.3.4 - Finally, the solution \o/](#2234---finally-the-solution-o)
- [3 - Conclusion](#3---conclusion)
  - [3.1 - Quick recap](#31---quick-recap)
  - [3.2 - What could have been done better?](#32---what-could-have-been-done-better)
  - [3.3 - Final words](#33---final-words)

<!-- /code_chunk_output -->

---

## 1 - Introduction

This video is about the beauty of problem solving, of making a computer do what you want it to do, and do so efficiently.

---

![Before](../images/website/before_gif.gif)

Every year before Christmas, there is a coding challenge. Its name? The **Advent of Code**.

Each day, instead of eating chocolates, you go on the [AoC website](https://adventofcode.com/) to solve little problems that are easy enough to be solved in about an hour, but sufficiently hard so they can't be solved by hand alone. Each problem is part of a cute story: this year your character climbed floating islands and helped Elves make the weather be snowy by Christmas.

Each problem is divided in two parts. The first one can be done quite simply, you have to write a program that solves the problem. The second part is trickier, the problem is a more complex version of the first one, and you often have to *optimize* your *algorithm* in order for the problem to be solved in less than two months of your computer running. Each solved problem gives you a **gold star** and reveals in color the part of the map you gave your help to.

![After](../images/website/after_gif.gif)

This year, during your journey, you played cards, found your way through mazes, watched the stars, made balls roll, propagated beams through mirrors, minimized the heat loss when transporting hot lava, counted the many paths taken by metal scraps, simulated electronic circuits, built Jenga towers out of sand, and so on.

To do so, you could use computer science stuff such as regular expressions, graph and automata theory and mathematical tools like combinatorial analysis, the famous quadratic equation, modular arithmetic, polynomial interpolation and so on.

| <img src="../images/other_problems/day_22.gif" height="400" alt="falling bricks tower"/> | <img src="../images/other_problems/day_12_L78.gif" width="600" alt="automaton"/> | <img src="../images/other_problems/day_14_simple.svg" width="400" alt="rolling balls"/> |
|:-:|:-:|:-:|
| Day 22, a huge Jenga tower | Day 12, using language theory | Day 14, making balls roll |

But wait! Don't leave now, I understand this is a tough beginning, but in this video I'll talk about a problem that can be solved using none of this intimidating stuff. It requires no background in math nor in computer science.

Let's talk about...

## 2 - The day 5 problem

![During](../images/website/during_gif.gif)

In the AoC, the problems get increasingly difficult each day. The first problem that required me to pause and ponder was the challenge of the 5th of December. I realized that this day's problem's second part was trickier than the previous days. Finally solving it brought me joy and made me remember what I loved in CS and in maths: problem solving and that little feeling of having understood something, the beauty of finding a clever way to tackle a problem. Because of that, I consider computer science is part of the family of mathematics.

In this video, my goal is to make you feel this little something. So let's get started!

### 2.1 - The first part of the problem

You can find the very problem [here](https://adventofcode.com/2023/day/5). Here's an explanation:

In your journey, you help a gardener to plant their seeds. You are given a list of seeds, each one has a number. You are also given seven maps, mapping seeds to a kind of soil, a kind of soil to a kind of fertilizer, fertilizer to water, water to light, light to temperature, temperature to humidity and finally, humidity to location. For each seed, you have, according to these maps, to find its soil, fertilizer, water, light, temperature, humidity and location. These characteristics are also identified with numbers.

| ![](../images/generation/icons/icon_0_seed.png) | ![](../images/generation/icons/icon_1_soil.png) | ![](../images/generation/icons/icon_2_fertilizer.png) | ![](../images/generation/icons/icon_3_water.png) | ![](../images/generation/icons/icon_4_light.png) | ![](../images/generation/icons/icon_5_temperature.png) | ![](../images/generation/icons/icon_6_humidity.png) | ![](../images/generation/icons/icon_7_location.png) |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|

`2.1-3` The final goal is to find the location with the lowest number. The input data for the problem is a text file, and on the website a dummy input is also provided to help understanding the problem. Here's the dummy input:

```r
seeds: 79 14 55 13

seed-to-soil map:
50 98 2
52 50 48

soil-to-fertilizer map:
0 15 37
37 52 2
39 0 15

fertilizer-to-water map:
49 53 8
0 11 42
42 0 7
57 7 4

water-to-light map:
88 18 7
18 25 70

light-to-temperature map:
45 77 23
81 45 19
68 64 13

temperature-to-humidity map:
0 69 1
1 0 69

humidity-to-location map:
60 56 37
56 93 4
```

`2.1-3.5` As you can see, the first line gives all the seed numbers, and it is followed by the description of all the maps. Since a map takes a number as an input, and gives another number as an output, we can plot it like so, with the inputs on the left and the outputs on the right. Here, the higher the number, the higher it it is displayed.

`2.1-4` Each map is composed of **rules**, one per line. A rule shifts a range of inputs to a range of outputs. Each rule is defined by three numbers : the **destination range start**, that we will plot on the right side, the **source range start** plotted on the left side, and the **range length**.  On the first map, the "seed to soil map", the first rule is `50 98 2`. So the **destination range start** is `50`, the **source range start** is `98` and the **range length** is `2`. Because the length is two, that means the range concerns only two seed numbers, starting with the **source range start**, that is `98` and `99`. Since the **destination range start** is `50`, the seed numbers `98` and `99` are mapped to soil numbers `50` and `51` respectively.
The second line, `52 50 48`, means that the `48` seed numbers `50, 51, ..., 96, 97` are mapped to the `48` soil numbers `52, 53, ..., 98, 99`.

`2.1-5` Because I find the visuals to be prettier, the rules of these maps won't always be shown with the grid. From now on, they will also look like this. Though, you must keep in mind we're still dealing with discrete numbers.

<!-- We don't talk about intermediate functions yet! -->

`2.1-6` Since all these rules do is to shift numbers, we will call them the *shifters*. If, on a given map, no *input characteristic number* matches its *shifters*, the *output characteristic number* is the same as the input. Thus, on the dummy seed-to-soil map, if we had a seed number of value `99`, if would have been mapped to soil number `51` since it matches the first *shifter*. On the contrary, the seed number `13` is mapped to soil `13` since no shifter matches with it.

![The first map](../images/generation/part_1/s_first_function.svg)

`2.1-7` To solve the problem, we have to find, for each seed number, the next *characteristic number* through each map until obtaining the *location number*, and then finding the minimum of all these numbers. Easy, right? Well for our dummy input, yes. But on the real input problem, you have 20 seeds, and each map has like 30 rules. So I do not encourage you to do it by hand. We'll create a program to do that for us. Let's begin.

| The example | The real problem |
|:-:|:-:|
| ![All the maps](../images/generation/part_1/s_shifters_blank.svg) | ![All the maps](../images/generation/part_1/r_shifters_blank.svg) |
| Interactive image | Interactive image |

| What we want to achieve for every seed |
|:-:|
| ![Moving seed](../images/generation/part_1/r_moving_seed.gif) |


#### 2.1.1 - Formalization

`2.1.1-1` A good start is to simplify the problem, to understand what the problem is *really* about, and to discard what's useless.

`2.1.1-2` Each map is what we call in maths a *function*. A function is an object taking one or more inputs defined in a set called the *domain*, and giving one or more outputs defined in another set called the *codomain*.

`x` Here we draw artistic representations of functions as machines with variable numbers of inputs (on the left) and outputs (on the right).

| ![](../images/generation/functions/arity/function_1_2.png) | ![](../images/generation/functions/arity/function_3_1.png) | ![](../images/generation/functions/arity/function_2_3.png) |
|:-:|:-:|:-:|

<!-- For instance, we have the *absolute value* function. We write it: $\mathbb{R} \to \mathbb{R}_+$, $x \mapsto |x|$ That means the inputs are defined on the *domain* of the *real numbers*, which are all the number we use : integers such as 1 or 42 , negative numbers such as -5, fractions, such as $\frac{2}{3}$, and even more elaborate well defined numbers such as $\sqrt{5}$, $\cos(2)$ or $\pi$. The output *codomain* of the function is only the set of these number that is above zero. And for each $x$ we associate it its absolute value.
Another example is $g : [0,1]\to\mathbb{R}^2, x\mapsto(x^3-3, x\cdot\sqrt{x})$. That means that our function $g$ takes real numbers in the interval $[0,1]$, and maps them to a pair of two real number, the first element of the pair is given for an entry $x$ by $x^3-3$ and the second by $x\cdot\sqrt{x}$. -->

<!-- In mathematics, it is sometimes useful to say we map only one object to another, so there's only one input and one output, but the input and the output can have dimensions matching what we called our number of inputs and outputs. For instance, let's take a look at this scary mathematical writing:

$$ f:\mathbb{R}^2\to\mathbb{R}^3, (x, y)\mapsto(x+y, x-y, x\cdot y) $$

Here that means that:

- Our function is named $f$,
- The *domain* is $\mathbb{R}^2$ and the *codomain* is $\mathbb{R}^3$, meaning the input is defined on two real numbers, and the output is defined on three real numbers,
- For any pair $(x,y)$ where both $x$ and $y$ are real numbers, $f$ outputs a triplet of numbers, the first one being $x+y$, the second being $x-y$ and the third one being $x\cdot y$.

For us, it is equivalent to say that the function takes two inputs and gives three outputs, or to say it takes one input of dimension two, and gives an output of dimension three. -->

`2.1.1-3` Here, we have seven maps, hence seven functions. They all map one integer to another integer, but all differently of course. We'll write $f_1(n)$ the value mapped to $n$ by the first function, $f_2(n)$ the value mapped to $n$ by the second and so on. For each function, the starting value and the output value are integers, but they do not represent the same thing. $15$ for instance does not have the same meaning here if it is a seed number, a temperature or a location. That is what units are for: the first function maps integers representing seeds to integers representing soils. So, even if $f_1(f_1(n))$ would be mathematically correct, it makes no sense since the second time we apply $f_1$ we expect a *seed number* but we give it a *soil number*, as it is already an output from $f_1$.

| ![f_1](../images/generation/functions/colors/function_1.png) | ![f_2](../images/generation/functions/colors/function_2.png) | ![f_3](../images/generation/functions/colors/function_3.png) | ![f_4](../images/generation/functions/colors/function_4.png) | ![f_5](../images/generation/functions/colors/function_5.png) | ![f_6](../images/generation/functions/colors/function_6.png) | ![f_7](../images/generation/functions/colors/function_7.png) |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
<!-- | Seed to Soil | Soil to Fertilizer | Fertilizer to Water | Water to Light | Light to Temperature | Temperature to Humidity | Humidity to Location | -->

`2.1.1-4` As it is clear now, we do not care about the *names* of the maps. We could have given the name $f_{\text{seed to soil}}$ to the first function, but it's quite longer than $f_1$, and we lost the semantics of it being the first function we have to apply. Here, we only care about the order of these functions. $f_2(f_1(n))$ is legal, but $f_1(f_2(n))$ is not semantically, even though, once again, it's not wrong mathematically speaking. And what we *really* care about is the output given by the last function when our seed number $n$ has been transformed by all functions, finally giving a *location number*. That is, the value of:

$$ f_7(f_6(f_5(f_4(f_3(f_2(f_1(n))))))) $$

`2.1.1-5` Since that's a lot of parenthesis, we can also write it like so:

$$ (f_7 \circ f_6 \circ f_5 \circ f_4 \circ f_3 \circ f_2 \circ f_1) (n) $$

Which is a notation that represents exactly the same thing, but is more readable as we understand we want the value given by the *composition* of all $f_i$ of an input $n$. Let's call these $f_i$ the *intermediate functions*. It is worth noticing that in maths our functions are applied *right to left*, from the closest to $n$ to the farthest, while in our drawings the numbers are processed from left to right, as I find more intuitive to have the inputs on the left, and the outputs on the right.

`2.1.1-6` Let's call $f$ the *composition* of all our intermediate functions.

$$ f = f_7 \circ f_6 \circ f_5 \circ f_4 \circ f_3 \circ f_2 \circ f_1 $$

`2.1.1-7` The problem of the day 5 can now be written as: "Find the lowest value $f(n)$ for every $n$ in the input seed set". What we'll have to do is to find a way to express $f(n)$ for a given $n$. And to compute $f$, we'll first have to know how to compute the intermediate functions.

Now that we know what to do, let's write some code.

#### 2.1.2 - Writing some code

##### 2.1.2.1 - Python 101

`2.1.2.1-1` In this video, we'll use Python as our programming language because it's quite easy to use and understand even if you're not familiar with it. You don't have to compile it and it's fast enough for the kind of program we want. We're not doing rocket science here. For instance look how easy it is to write a program displaying "Hello World!" on the screen, compared to writing the same program in another language called C++.

<table>
<tr>
<!-- <td>

```python
print("Hello, world!")
```

</td>
<td>

```cpp
#include <iostream>

using namespace std;
int main(int argc, char* argv[])
{
    cout << "Hello, world!" << endl;
    return 0; 
}
```
</td> -->

<tr>
    <th width="50%">Simple Hello World in Python</th>
    <th width="50%">"Simple" Hello World in C++</th>
</tr>

<tr>
    <td><img src="https://c.tenor.com/n36qbwYGTOAAAAAd/tenor.gif"></td>
    <!-- <td><img src="https://c.tenor.com/kq7GyBPPIj0AAAAd/tenor.gif"></td> -->
    <td><img src="https://i.imgflip.com/5f4drd.gif"></td>
</tr>

<tr>
    <td>

```python
print("Hello, world!")
```
</td>
<td><img src="../images/other_visuals/helloworld_cpp.gif"></td>
</tr>
</table>

`2.1.2.1-2` Now don't get me wrong, I'm a huge fan of C++, but for this kind of tasks, we'll stick with Python.
If you're already familiar with very basic Python and basic *Object-Oriented Programming*, you can skip to the [next chapter](#2122---creating-the-base-objects). 

###### 2.1.2.1.1 - The basics

`2.1.2.1.1-1` The first concept we will see is what we call a `variable`. A variable is a way to give a symbolic name to a value. For instance, in the following Python code, we create a variable named `my_variable` and through the use of the operator `=` we give it a value of 17. We can see what's inside a variable by printing it to the screen for instance:

```python
my_variable = 17
# the_answer = True
# some_text = "Lorem Ipsum"

print(my_variable)
```

`2.1.2.1.1-2` The second concept we'll see is... functions. They behave more or less like in mathematics: you give inputs, they do stuff and you get outputs. Let's create in Python our first function to multiply a number by two. We will call this function `doubled`.

```python
def doubled(value):
    return value * 2
```

`2.1.2.1.1-3` Could it be even simpler? The keyword `def` says we create a function, we then give it a name, here `doubled`. Between the parenthesis are the inputs variables, if any. Here we have only one input represented by the variable `value`. Finally the keyword `return` stops the function and returns something, here the input times two.

Then we can use it like so:

```python
result = doubled(45)
print(result)
# 90
```

`2.1.2.1.1-4` Unlike in maths, in programming, functions can have no input or no output, or even neither. For instance, we have already seen the `print` function, that takes stuff to print but technically returns nothing, it just prints it on the screen, which is different. And as another example we could think of a function returning the current time, thus taking no input.

```python
import datetime
get_current_time = datetime.datetime.now
...
current_time = get_current_time()
print(current_time)
```

`2.1.2.1.1-5` Just one more thing on functions and variables: variables store data, but data is a rather large concept. Data can represent multiple things such as integers, real numbers, string of characters, booleans (which are a way to express two states, True or False) and so on. In Python we don't have to explicitly express the type of the variables we use. We can write this:

```python
# a = "Hello everyone!"  # a is a string, of type str
a = 1.876              # a is a real number, of type float
a = False              # a is a boolean, of type bool
a = 54                 # a is an integer, of type int
```

`2.1.2.1.1-6` But when writing functions or declaring variables, we can give some *hints* as to what the function takes and returns, or what the variable is supposed to store:

```python
def add(a:int, b:int) -> int:
    return a + b
```

Here, we created a function named `add` and *type hinted* what the function was supposed to take and to give, here two integer inputs, and one integer output. But in Python nothing stops us from writing that:

```python
r = add(8, 9)      # Adding two integers
r = add(8.0, 9.5)  # Adding two floating numbers
r = add(8, 9.4)    # Adding an integer and a floating number
```

As long as the operation `a+b` is well defined for the objects `a` and `b`, adding variables is legal. Be they integers or real numbers, that will add them as expected. In order to keep our code clean, even if that doesn't stop us from doing silly mistakes, we'll use *type hinting*.

<!--
If $f$ were more complicated, for instance:

$$ f:\mathbb{R}^2\to\mathbb{R}^3, (x, y)\mapsto(2x+y+2xy^2, \sqrt{|x-y|}, \dfrac{x\cdot y}{x^y}) $$

We could write in python:

```python
def f(x, y):
    return 2*x + y + 2*x*y*y, sqrt(abs(x-y)), (x*y)/(x**y)
```

Where `sqrt`, `abs` are functions that compute the square root of a number and its absolute value, and the *operator* `**` is the power operator. But that is not quite readable, so we could create temporary numbers to store every operation, and then return a triplet of these three numbers:

```python
def f(x, y):
    a = 2*x + y + 2*x*y*y
    b = sqrt(abs(x - y))
    c = (x*y) / (x**y)
    return a, b, c
```

That does exactly the same, but we've created three additional *variables* holding these temporary results.
-->


<!-- The most straightforward approach to compute the value of $f(n)$ is to loop in all the intermediate function, applying them in order to our input value $n$. So let's create an object named `IntermediateFunction`. In this video, we'll use Python, a programming language quite simple to understand even if you never used it before. Let's look back at our problem definition of the first map:

As we see, our *intermediate function* can be constructed using a *list* (of variable size, here 2 or 3) of parameters, the **destination range start**, the **source range start** and the **range length** we talked about. We will call them the **Shifters**. So when we *construct* an intermediary function, we'll do so with a list of *Shifters*. What we are doing now is to abstract all concepts, giving them a name so we can refer to it using some semantics. `0 15 37` is no longer just a bunch of numbers, it is now an object representing a *Shifter* that is used to create an *Intermediary Function*.

The main programming tool we'll use is called a **function**. As in mathematics, a function is called with inputs parameters (or not), and returns outputs (or not). Once again, the nature of the inputs and the outputs vary according to our needs.

```python
def add(a:int, b:int) -> int:
    return a + b

def getCurrentTime() -> float:
    current_time = ... # Complicated stuff here
    return current_time
```

Here we created two functions, one that takes two integers and output an integer that is the sum of the two inputs, and one that returns the current time, taking no inputs. To be a little bit more precise, in python we do not have to explicitly say what are our variable made of. As long as the operation is legal it will work. So `add(1, 3)` is legal, so is `add(-99, 0.4865)` since the addition `a + b` is legal between numbers. But if we try to do `add("hello!", 8)` it won't work, since there's no way in python to "add" a string of characters and an integer. In order for the code to be as clear as possible, I'll use syntax hinting in order to explicitly show the type of our variables.
-->

`2.1.2.1.1-7` In this project, we will also use the concept of *conditional statement*. It allows the program to do something specific at a given moment depending on if a given condition is true or false. Let's use this example:

```python
year_of_birth = 1986

if year_of_birth < 1969:
    print("You witnessed the Apollo 11 mission!")
else:
    print("You weren't born to witness the Apollo 11 mission :(")

print(20 < 15)
```

Here, we have an `if` / `else` block. The condition checked here is if the variable `year_of_birth` is smaller than 1969. Depending on the result of that comparison, the instructions that are to be run are those from the if block or the else block, here printing two different messages. The comparison is done with the operator `<`, which acts like a function returning `True` or `False` depending on the inputs. Using different comparison operators, we can also check for a greater value, a greater or equal value, a strictly equal value, or a strictly different value.

`2.1.2.1.1-8` In this video, we will also need to iterate through objects. We will create the simplest collection of objects, the *list*. In Python it is written using square brackets, like so:

```python
my_list = [8, 3.8, False]
print(my_list)
```

As you can see, we can have objects with different types in the same list. Now if we want to iterate through them, we can use the `for ... in ...` syntax, like so:

```python
for my_object in my_list:
    print(my_object)
```

In such a `for` block, we can iterate through all the objects of a list, and we can manipulate the current object using a variable we declared in the block. Here, every object of the list named `my_list` can be used, once at a time, through the variable `my_object`. In that example, we only print their values.

###### 2.1.2.1.2 - About OOP

`2.1.2.1.2-1` In the code we'll write, we'll use a paradigm called *OOP*, short for *Object-Oriented Programming*. That is, we'll represent the data we'll manipulate as **Objects**. An object is roughly a bunch of organized data that is used to represent concepts. The pattern that defines what the object is like is called the *class*, and the real objects we can manipulate are called the *instances* of that particular class. As an example, we could represent these shapes as *instances* of the class `Circle`. Objects have *fields*, which are variables bounded to the instances of a class. For example to these circles have a center and a radius, as defined per the class in order to fully describe them, but the values stored per objects might be different. Objects can also have *methods*, which are functions allowing the objects to do things.

`2.1.2.1.2-2` As an example, let's suppose we want to manipulate 2D shapes. We could create an object representing a *Circle*. Let's say our `Circle` is defined by a center point and a radius. On a plane such as a screen or a sheet of paper, the center is a point on that plane and is represented by two coordinates, x and y. We can create another object, `Point2D` that represents such concept. We would write:

```python
class Point2D:
    """This is a basic 2D point."""

    def __init__(self, x: float, y: float) -> None:
        self.x = x
        self.y = y


class Circle:
    """This is a basic 2D circle."""

    def __init__(self, center: Point2D, radius: float) -> None:
        self.center = center
        self.radius = radius
```

`2.1.2.1.2-3` This example is a bit verbose. The keyword `class` says we're creating a new class, whose name comes after. Then comes an optional description of the object in order to document code. The `__init__` method is in Python a special *method* that creates the actual object, and its parameters are what we have to give to construct the said object. In Python, the first parameter of an object's method is always `self`, and it is a variable representing the object itself. Basically, here what we are saying is that when we construct a 2D point, we have to give it two real numbers represented by `float`, x and y, and when we create a circle, we have to say where it is located by giving it a center of type `Point2D`, and a radius which is a real number. Then we create the fields of our classes, without surprise the x and y coordinates for the point, and the center and radius for the circle. Now we have two classes, let's create instances:

```python
position = Point2D(0.6, 23.7)
circle = Circle(position, 6.5)
```

Or even simpler,

```python
circle = Circle(Point2D(0.6, 23.7), 6.5)
```

If we don't care that much about giving a label to the center of the circle. Now we have created a circle at position $(0.6, 23.7)$ with a radius of $6.5$. For now, it doesn't do much, here we just stored data and we can access it through the *fields* we've just created.

```python
print(circle.radius)
print(circle.center.x)
```

`2.1.2.1.2-4` Ideally, in OOP, we should't use the fields directly when dealing with an object, so for the sake of good practice, let's create methods for our `Circle` class to get the radius, and let's create some more to compute the diameter and the area.

```python
class Circle:
    """This is a basic 2D circle."""

    def __init__(self, center: Point2D, radius: float) -> None:
        self.center = center
        self.radius = radius

    def get_radius(self) -> float:
        return self.radius

    def get_diameter(self) -> float:
        return 2 * self.get_radius()

    def get_area(self) -> float:
        return 3.14159 * self.get_radius() * self.get_radius()
```

Note how `self`, the first parameter of our methods, allows us to retrieve the *fields* we're interested in. Now we can use these methods to manipulate an abstract representation of a circle.

```python
r = circle.get_radius()
d = circle.get_diameter()
a = circle.get_area()
```

`2.1.2.1.2-5 x` We could decide that what defines a circle is not its radius, but its diameter. We could write:

```python
class Circle:
    """This is a basic 2D circle."""

    def __init__(self, center: Point2D, diameter: float) -> None:
        self.center = center
        self.diameter = diameter

    def get_radius(self) -> float:
        return self.diameter / 2

    def get_diameter(self) -> float:
        return self.diameter

    def get_area(self) -> float:
        return 3.14159 * self.get_radius() * self.get_radius()
```

And if we wrote

```python
circle = Circle(position, 12.5) # <-- changed here
r = circle.get_radius()
d = circle.get_diameter()
a = circle.get_area()
```

We would get the same result. See in `get_area()` how we use the function `get_radius()` to retrieve the radius of the circle even though it's been defined with its diameter. In fact, we do not care about *how* the information is stored when we want to compute the area and more generally when we manipulate an object. This layer of abstraction is what makes OOP powerful, we care only when we define how the object should behave and when we create it, but for simple tasks, we do not care at all afterwards. We just manipulate it using a high-level semantics, close to the way we talk. Here, the members of our objects are exactly what is given when constructing the object, but that will not always be the case, as we'll see later.

`2.1.2.1.2-6` Now that we're familiar with Python and OOP, let's code our program!

##### 2.1.2.2 - Creating the base objects

`2.1.2.2-1` So now that the way of handling data is known, let's create real objects for our **Shifters** and our **Intermediate Functions**. The `Shifters` are defined by three numbers, as we've seen before, and the `IntermediateFunctions` are defined with a list of `Shifters`.

```python
class Shifter:
    """A `Shifter` represents the shift of a range `[source, source+length[`
    to a range `[destination, destination+length[`.
    """

    def __init__(self, destination: int, source: int, length: int) -> None:
        self.src = source
        self.dst = destination
        self.length = length


class IntermediateFunction:
    """An `IntermediateFunction` is composed of multiple `Shifters`.

    It can process an input value to give an output value.
    """

    def __init__(self, shifters: list[Shifter]) -> None:
        self.shifters = shifters
```

`2.1.2.2-2` That's a good start. But for now, these objects don't do much, except storing numbers. The first thing we want to know is which *shifter* is to be used for a given input of an intermediate function. To apply our *intermediate function* to a value, we will loop through all *shifters*: if we find a shifter whose start range contains our input value, we stop and return our value shifted. If we don't find any shifters matching our value, we give it back, unchanged.

###### 2.1.2.2.1 - Shifters

`2.1.2.2.1-1` Let's begin with our `Shifter` objects. Let's create a function checking whether a value is in the starting range of a given *shifter*. Let's call this function `can_act_on`. It should take one input, one integer, the value we want to test our *shifter* against, and should return a *boolean*, *True* or *False*.
To check if the shifter can act on a value, that is, the value is contained in the input range of that shifter, we simply check whether the input value is greater or equal than the **source range start** and strictly less than the **source range start** plus the **range length**. You can convince yourself that it is strictly less than that sum and not less or equal by imagining a shifter shifting only one input, the only input that could be shifted would be the very **source range start**, so the condition for a value $v$ would be written like so: `src` $\leq v <$ `src` $+1$, a less or equal condition on the right would lead to two values matching the condition.

```python
class Shifter:
    ...
    def can_act_on(self, value:int) -> bool:
        if self.src <= value and value < self.src + self.length:
            return True
        else:
            return False
```

The function can be written more compactly like we would write in maths: `src` $\leq v <$ `src + length`.

```python
    def can_act_on(self, value:int) -> bool:
        return self.src <= value < self.src + self.length
```


So now we can perform tests like:

```python
if our_current_shifter.can_act_on(a_value):
    # Do something
else:
    # Do something else
```

`2.1.2.2.1-2` Let's now create a method `apply` on our *shifter* objects. It takes one input, the value we want to shift and returns the shifted value. The amount of the shift is simply the **destination range start** minus the **source range start**.

```python
class Shifter:
    ...
    def apply(self, value:int) -> int:
        return value + (self.dst - self.src)
```

Note that we do not check whether it is legal to apply the shift to our value. It's been done just before. Here, we only want the result of the operation. We could use it wrongly, but we won't.

`2.1.2.2.1-3` Now, we're done with the `Shifter` objects. It is quite nice to notice here that for any given *shifter* `s`, `s.dst - s.src` and `s.src + s.length` would always have constant values. We will talk later about *optimizing* algorithms, but here we would perform extra subtraction or addition each time we want to test a value or to apply the mapping on it. Instead, we can modify the object like this:

```python
class Shifter:
    def __init__(self, destination:int, source:int, length:int):
        self.in_min = source
        self.in_max = source + length
        self.delta  = destination - source

    def can_act_on(self, value:int) -> bool:
        return self.in_min <= value < self.in_max
    
    def apply(self, value:int) -> int:
        return value + self.delta
```

`2.1.2.2.1-4` The object is constructed and used in the same way as before, but what happens inside is different. What is stored does not correspond exactly to what's been given during the construction of the object, and if its methods do the same things as before, they do it a tiny bit better. Now, internally, the objects store the input range start, called here `in_min`, the input range end, or more exactly one more than the input range end, as we've seen before, called here `in_max`, and `delta`, the amount of the shift. It's not a big deal to have an extra operation, as modern computers can perform tons of computation per seconds, but here it makes the code more readable and we can begin to see what *optimization* could mean.

###### 2.1.2.2.2 - Intermediate Function

`2.1.2.2.2-1` Let's now come back to our `IntermediateFunction` objects. They will have one important method, which will be about computing $f_i(n)$. We will also call this method `apply`, allowing us to write code like that:

```python
f_1 = IntermediateFunction(...)
f_2 = IntermediateFunction(...)
intermediate_result = f_2.apply(f_1.apply(n))
```

Let's do that:

```python
class IntermediateFunction:
    def __init__(self, shifters:list[Shifter]):
        self.shifters = shifters

    def apply(self, value:int) -> int:
        for shifter in self.shifters:
            if shifter.can_act_on(value):
                return shifter.apply(value)
        return value
```

`2.1.2.2.2-2` It's as simple as that. Using the `for element in iterable` syntax, we loop through all shifters, the variable `shifter` representing the `Shifter` of the current iteration in the loop, and if we find one that can act on our value, we return the value of this transformation. If after looping through all shifters we find nothing, we simply return the same value we had as an input.

`2.1.2.2.2-3?` It is worth mentioning that for a given input value, at most one *shifter* can act on it. If two or more shifters of the same intermediate function could act on the same input value, that would mean there would be two or more different outputs, and that is not possible, since for a given $n$ and a given intermediate function $f_i$, $f_i(n)$ is well defined and has a unique value. Nothing here actually prevents two different *shifters* to return `True` on their `can_act_on` methods. Thus, in the current implementation of this problem, only the first *shifter* in the loop would trigger the shifting. So, we expect the data we use to create the objects to be consistent and to not create undefined behaviors, as we are 1. lazy and 2. trusting the authors of the problem to provide meaningful data.

`2.1.2.2.2-4` We can now test our objects, creating the "Seed to soil map" from the dummy input:

```python
map_seed_to_soil = IntermediateFunction(
    [
        Shifter(50, 98, 2),
        Shifter(52, 50, 48),
    ]
)

print(map_seed_to_soil.apply(55)) # 57
print(map_seed_to_soil.apply(13)) # 13
```

Everything behaves as expected!

###### 2.1.2.2.3 - Importing the data

`2.1.2.2.3-1` Let's now import the data from the advent of code text file into our data structure. We won't enter into the details of how to do that in Python, as it is not the topic of this video, but we now have two lists: `seeds`, a list of integers representing the seed numbers, and `intermediate_functions` a list containing all our intermediate functions properly constructed with the shifters. Here's a way of doing so. Not the cleanest, but it gets the job done.

```python
seeds: list[int] = []
intermediate_functions: list[IntermediateFunction] = []

with open("input.txt") as file:
    seeds_chunk, *map_chunks = file.read().strip().split('\n\n')
    seeds = [int(s) for s in seeds_chunk.split(':')[1].strip().split(' ')]
    for map_chunk in map_chunks:
        map_chunk = map_chunk.split('\n')[1:]
        shifters = [
            Shifter(*[int(p) for p in params.split(' ')])
            for params in map_chunk
        ]
        intermediate_functions.append(IntermediateFunction(shifters))
```

###### 2.1.2.2.4 - Creating the final function

`2.1.2.2.4-1` We now need to create our final function $f$, the one that takes as an input a seed number and outputs a location number. To do so, we will create a variable named `value`, first being our seed number input. For each *intermediate functions*, `value` will be updated by the current function. So for a seed input $n$, at first `value` = $n$, then `value` = $f_1(n)$, then `value` = $f_2(f_1(n))$, then `value` = $f_3(f_2(f_1(n)))$ and so on until `value` = $(f_7 \circ f_6 \circ f_5 \circ f_4 \circ f_3 \circ f_2 \circ f_1) (n) = f(n)$ (the composition of all our intermediate functions).

```python
def compute_location_from_seed(seed_nb:int) -> int:
    value = seed_nb
    for i_f in intermediate_functions:
        value = i_f.apply(value)
    return value
```

And if we name our function input `value` we don't even need to write `value = seed_nb`:

```python
def compute_location_from_seed(value:int) -> int:
    for i_f in intermediate_functions:
        value = i_f.apply(value)
    return value
```

#### 2.1.3 - Solving Part One

`2.1.3-1` Now all we have to do is to finally solve part one! We need, for each input seed, to compute its location, and then return the location with the lowest value. To do so we'll create a variable named `min_location` that, for each iteration, will keep the lowest location we've seen so far, being updated if we find a location with a lower value than the current minimal location. To begin with, we'll assign a *huge* value to this variable so we're sure it will be updated at least once. Let's put this value at `float("+inf")`, which basically represents infinity, ensuring every value is lower than this one. The function `min` returns the minimum value from the inputs.

```python
min_value = float("+inf")
for seed in seeds:
    value = compute_location_from_seed(seed)
    min_value = min(min_value, value)
print(min_value)
```

`2.1.3-2` And voilà! When we run the code, for the sample input we get `35`, which is the answer we expected! Let's do the same for the real input: yes, `38####265` is the right answer! Congrats, now let's do part two!

| ![The process of finding the minimum value](../images/generation/part_1/r_finding_minimum.gif) |
|:-:|
| Keeping the lowest value in memory |

| ![Playing with the graph](../images/generation/part_1/r_full_transparent.svg) |
|:-:|
| Interactive image! |

### 2.2 - The Second part of the problem

`2.2-1` Our character discovers that the number of seeds we tested is too low: The first line of our input actually describes *ranges* of seed numbers. The values come in pairs, the first of which describes the **start** number, and the second the **length** of the range. So in our example, `79 14 55 13` means we'll have to test two ranges of seeds, the first one starting at `79` and having `14` seeds, so the seeds `79, 80, ..., 91, 92` and the second starting at `55` and having `13` seeds, `55, 56, ..., 66, 67`. Instead of four seed numbers, we have to test 14+13=27 seed numbers.

#### 2.2.1 - Naive approach

`2.2.1-1` All we have to do is to change the import, instead of reading the seed numbers one by one, we'll read them two by two, and instead of adding one seed number to our seed list, we'll add a `range` object. In Python, `range(n)` represents the discrete interval $⟦0, n⟦$ open on the right. (Here the double square brackets represent an interval of integers, not real numbers). If we want to create the closed integer interval $⟦a, b⟧$ in Python, we have to write `range(a, b+1)` since $⟦a, b+1⟦ = ⟦a, b⟧$. (the discrete interval a, b+1 open on the right is equal to the closed discrete interval a, b) We can then iterate through the `range` iterator using the `for ... in ...` syntax we've seen before. For instance, here, `print` will be called on the value 3, 4, 5 and 6.

```python
for i in range(3, 7):
    print(i)
```

`2.2.1-2` Let's change the way we import the data. The intermediate function list doesn't change, but instead of a list named `seeds`, we create a list named `seed_ranges` containing the ranges.

```python
for seed_range in seed_ranges:
    print(seed_range)
# range(79, 93)
# range(55, 68)
```

`2.2.1-3` Here's the actual code if you're interested.

```python
seed_ranges: list[range] = []
intermediate_functions: list[IntermediateFunction] = []


with open("input.txt", "r") as file:
    seeds_str, *maps = file.read().strip().split("\n\n")
    seeds = seeds_str.split(":")[1].strip().split(" ")
    for start, length in zip(seeds[::2], seeds[1::2]):
        start = int(start)
        length = int(length)
        seed_ranges.append(range(start, start + length))
    for chunk in maps:
        chunk = chunk.split("\n")[1:]
        shifters = [Shifter(*[int(p) for p in params.split(" ")]) for params in chunk]
        intermediate_functions.append(IntermediateFunction(shifters))
```

`2.2.1-4` So let's modify our main program, we'll iterate through our `seed_ranges`, and for every range we've created we'll iterate through it, and call `compute_location_from_seed` on every seed of the range. We update the minimum location, as we've done before.

```python
min_location = float("+inf")
for seed_range in seed_ranges:
    for seed in seed_range:
        location = compute_location_from_seed(seed)
        min_location = min(min_location, location)
```

When we do that on the test input and `print` the minimum value, we also find `46` which is the correct answer for the dummy input! Being confident, let's find the real answer on the real data set.

![What we would like to happen](../images/generation/part_1/r_all_trajectories.gif)

`2.2.1-5` Well... Nothing happens... Why? Wait, it has not crashed or something, it's still running. Let's visualize where it is by adding progression bars. Ouch it's slow! In fact, there are 1 699 478 662 seeds to process. In the first part, there were only 20 seeds. Based on the number of seeds we can process per second, it should take more than four hours... That is way too long for this video.

#### 2.2.2 - Complexity

`2.2.2-1` As we just saw, the same algorithm (which is: for each seed number, compute the location number and keep in mind which one was the lowest) can crunch the numbers really fast for the first part, but is way too slow for the second part. Why? We just *scaled up* the *number of seeds* of the problem. We went from $20$ seeds to more than $10^9$.

---

---

##### 2.2.2.1 - What is complexity?

`2.2.2.1-1` In computer science, problems have a *size*. For instance:

- If you want to find the minimum of a list, the size of the problem is the length of that list,
<!-- - If you have a list of objects you want to sort, the size of the problem is the number of objects you have, -->
- If you want to find the shortest path allowing you to visit every city you have in mind, the size of the problem is the number of cities you think of,
- If you want to find the definition of a given word in a dictionary, the size of the problem is the number of words your dictionary contains.

`2.2.2.1-2` For a given problem and a given algorithm solving that problem, the size of the problem is a measure of the size of the input data. In our problem we gave seeds to our algorithm. So, the size was the number of seeds.

Intuitively, we understand:

- It's easier to find the minimum of two numbers versus the minimum of one hundred numbers,
<!-- - It's easier to sort a list of three numbers versus a list of one billion, -->
- It's easier to find the shortest path between four points versus fifteen,
- It's easier to find a word in a dictionary of ten words versus a dictionary of ten thousand words.
- And for us, it is easier to compute the location of 20 seed numbers, versus one billion seed numbers...

`2.2.2.1-3` Okay. But *how much easier* is it? Can we *quantify* how easy it is for an algorithm to solve a problem? Yes! Enters **complexity**.

There are mostly two things we may want to measure when writing an algorithm to solve a given problem: How much *time* it will take before ending (and hopefully give a solution) and how much *space* is needed, which is the memory required by the program. They are called, well, **time complexity** and **spatial complexity** and yes, you could say you're a computer scientist performing *space-time analysis* and earn two points of charisma even though all you want to do is
1. know if your program will stop one day and if possible not after the end of the universe, and
2. know if you have enough RAM or disk space to run your piece of code.

`2.2.2.1-4` Even though *space complexity* is truly interesting, we will not really focus on it for this video.
In fact, as opposed to *time complexity*, it's quite hard today to need a tremendous amount of space for simple puzzles. When computer science began to be a thing and punched cards were the only thing you had to store information, yeah, one had to be extremely careful with the spatial needs of the program. But nowadays, your algo needs 1000 GB and you only have 500? You buy a second hard drive and call it a day. On the other hand, as we've seen, there's a huge difference between the problem being solved in less than a second versus being solved in more than four hours...
<!-- We won't discuss about how "times were better" or "computer scientists now cannot write efficient code", but instead we will focus on how to deal with time complexity. -->

`2.2.2.1-5` Time complexity isn't about measuring precisely how much time is required for the algo, since it does depend on a lot of factors, such as the programming language you're using, the machine you're on, how busy your computer is, and so on. In fact, when doing time complexity we focus on *how many operations* are needed for the algorithm to give a result, and we roughly state that all computer operations are the same, even though you may intuitively feel that it's "easier" to add two numbers than to divide them, for a human or a computer.

Time complexity analysis allows us to **classify algorithms** according to how their run time evolves as the size of the input set gets higher.

###### 2.2.2.1.1 - Examples

`2.2.2.1.1-1` It's time for some examples. Let's consider a list of $n$ integers. Let's create three problems whose input is our list of size $n$. We'll create three functions to solve them using only the tools we've seen so far.

- First, let's create a function returning one plus the third value of our list (we assume the list has at least three numbers),
- Then, let's create a function to find the minimum of our list,
- Finally, let's create a function to write the multiplication table of our list, that is $a\times b$ for every $a$ and $b$ in our list.

###### 2.2.2.1.1.1 - The Third Value

`2.2.2.1.1.1-1` To solve the first problem, it's easy: we iterate through our list, and keep in a dedicated variable `current_index` the index of the number we're on. If our index is 3, we add one to the current number and return the result.

```python
def find_the_third_value_plus_one(numbers: list[int]) -> int:
    current_index = 0
    for number in numbers:
        current_index = current_index + 1
        if current_index == 3:
            return number + 1
```

`2.2.2.1.1.1-2` Let's count the number of operations we performed:

- We create a new variable, `current_index`
- Then we iterate through all numbers using a `for` loop. For each iteration:
  - We add one to the variable `current_index`
  - We check if `current_index` is equal to 3. If it is:
    - We add one to number and return this value.

`2.2.2.1.1.1-3` So in fact, the size of the problem doesn't matter here: what is inside the `for` loop will be executed exactly three times, our variable `current_index` being 1, then 2, then 3 and then we return. So we have:

- 1 operation : the creation of the variable
- 2 operations (adding one to `current_index` and checking if it is equal to 3) that we will do three times
- 1 operation, adding one to the current number

Thus, for any list of size $n$, we always perform $8$ operations.

<!--
(`2.2.2.1.1.1-4?` And yes, the function could have been written like so:)

```python
def find_the_third_value_plus_one(numbers: list[int]) -> int:
    return numbers[3] + 1
```
-->

###### 2.2.2.1.1.2 - The minimum of the list

`2.2.2.1.1.2-1` Let's now compute the minimum of a list. We've seen this one before! We just need to keep in mind the lowest value we've seen so far, and update it when iterating through our list.

```python
def find_the_lowest_number(numbers: list[int]) -> int:
    lowest_value_so_far = float("+inf")
    for nb in numbers:
        lowest_value_so_far = min(lowest_value_so_far, nb)
    return lowest_value_so_far
```

`2.2.2.1.1.2-2` Let's count the number of operations:

- We create a variable `lowest_value_so_far`
- For every element of our list:
  - We compute the `min` between the current number and the `lowest_value_so_far`,
  - We update the variable with whatever the result of that computation is. As we can see, there is not a 1:1 correlation between an operation and a line of code. We have to remain attentive!
- After the `for` loop, there is no more computation to do, `lowest_value_so_far` stores what we wanted.

So, since we do one initial operation and then two operations for every number from our list, in total that's $1 + 2\times n$ operations for a list of size $n$.

###### 2.2.2.1.1.3 - Multiplication Table

`2.2.2.1.1.3-1` To print the multiplication table of a list of numbers, we want to print on the screen $a\times b$ for every $a$ of the list, for every $b$ of the list. So we'll nest two `for` loops:

```python
def print_multiplication_table(numbers: list[int]) -> int:
    for a in numbers:
        for b in numbers:
            ab = a * b
            print(a, "x", b, "=", ab)
```

(We can print more than one variable by separating them using commas in the `print` function.)

`2.2.2.1.1.3-1` The first loop will be executed once per number `a` in our list, so $n$ times in total. For the same reason, the second loop will be executed $n$ times, one per value `b`, but that is $n$ times for each number `a`. So we enter the body of the second `for` loop $n^2$ times. And the body of that second loop contains two instructions, so that's two instructions for every `b` for every `a`, so in total we have $2n^2$ instructions for a list of size $n$.

###### 2.2.2.1.2 - Formalization

`2.2.2.1.2-1` So, what was the point of all this? Well, when we counted the number of operations we found that for a given problem of size $n$, the first algorithm had a constant number of operations, the second had $a + bn$ operations, where $a$ and $b$ are two positive integers, and the third had $a + bn + cn^2$ operations, where $a$, $b$ and $c$ are three positive integers. For a modern computer, for small numbers $n$ it doesn't make a difference, as computers do many, many operations per second. But when $n$ becomes bigger and bigger, it starts to make a difference.

| The same functions, low and high value of $n$ |
|---|
| ![Low](../images/other_visuals/2.2.2.1/1.1.png) |
| ![Low](../images/other_visuals/2.2.2.1/1.2.png) |

We start to understand that, according to how their number of operations are expressed in terms of $n$, these three functions would be classified differently, and have different time complexity.

`2.2.2.1.2-2` When we express complexity we do not quite care about the exact number of operations, we want to express an estimation of how the total number of operations based on the problem size to classify the algorithms. And since it would be really fast for small values of $n$, we want to focus on big values of $n$.

`2.2.2.1.2-3` In fact, we can intuitively feel that if we have a constant number of operations, one more or one less doesn't make a difference. If you have a faster computer, a higher but still constant number of operations doesn't matter as the numbers would still be crushed in no time. It's just like we had one unique big operation.

`2.2.2.1.2-4` In the second function, we have $a + bn$ operations, where $a$ is 1 and $b$ is 2. Even if $b$ was only 1, the value of $a$ being unrelated to $n$ ends up being insignificant compared to the total number of operations when $n$ goes high. What matters in that function, is what happens in the `for` loop, being responsible for the $2n$ operations, not the extra operation where we declare `lowest_value_so_far` and say it has a huge value. And like we said just before, here the value of $b$ doesn't matter, since a faster computer could crunch the numbers faster, or a better version of the algorithm would lower that value $b$. But that family of algorithm still depends on $n$, and there is a kind of proportional nature between the number of operations and $n$.

`2.2.2.1.2-5` In the third example, we have $a + bn + cn^2$ operations, and we can see likewise that what matters the most is the $n^2$ part, since $a$ and $bn$ become insignificant when $n$ goes high. And, like before, what's important is not the number $c$, but the link between the number of operations and $n^2$.

|   $bn$ and $a$ become insignificant compared to $n^2$   |
|---------------------------------------------------------|
| ![Quadratic Complexity](../images/other_visuals/quadratic_complexity.gif) |

`2.2.2.1.2-6` Let introduce the "big O notation". It is one of the most important tool in computer science when it comes to expressing the complexity of an algorithm. If $f$ and $g$ are two functions taking an integer as an input, and outputting another integer, we say " $f$ is big O of $g$ " if there's a value $n_0$ big enough such that for every $n$ greater than this $n_0$ we have $f(n)\leq K\cdot g(n)$ where $K$ is a non-zero positive constant. (Written more mathematically, it reads:)

$$f=O(g)\Longleftrightarrow\exists n_0\in\mathbb{N},\exists K\in\mathbb{R}_+^*,\forall n\in\mathbb{N},\; n\geq n_0 \Rightarrow f(n)\leq K\cdot g(n)$$

`2.2.2.1.2-7` (As a side note, here we will write $f=O(g)$ and not $f\in O(g)$ even though we say "f is big O of g" as it's the way it's usually written in computer science, even though one may argue it's mathematically inaccurate to use the equal sign.)

`2.2.2.1.2-8` The "big O notation" allows us to express the complexity in terms of the component that grows the faster, discarding what matters less. For instance, let's call $p_1$, $p_2$ and $p_3$ the three functions expressing the exact number of operations for the problems 1, 2 and 3 depending on the length of the input list. For a list of length $n$, we have:

- $p_1(n) = 8$
- $p_2(n) = 1+2n$
- $p_3(n) = 2n^2$

`2.2.2.1.2-9` For $p_1$, we can find the function $g$ such that $g(n) = 1$ no matter what $n$ is, the constant $n_0=0$ and the constant $K=8$. So, for every $n$ we do have $p_1(n) \leq K\cdot g(n)$ and we can write $p_1 = O(1)$. We say $O(1)$ algorithms have **constant** complexity.

It is important to understand all $O(1)$ algorithms don't have necessarily a constant number of operations. It only means you can find a *constant number* whose value is always greater or equal than the number of operations. For instance, if $p_1(n)$ was 8 or 10 depending on $n$, we could find a number $K=10$ for instance such that $p_1(n)\leq K$ and thus still say our first function has a *constant complexity*.

| A non-constant function still having a *constant complexity* |
|---|
| ![Constant](../images/other_visuals/2.2.2.1/9.png) |

`2.2.2.1.2-10` For $p_2$, it goes the same. Lets say:

- $g(n) = n$
- $n_0 = 1$
- $K = 3$

And we now have $p_2 = O(n)$. We say this algorithm has a **linear** complexity.

| A *linear complexity* |
|---|
| ![Linear](../images/other_visuals/2.2.2.1/10.png) |

`2.2.2.1.2-11` And finally, for $p_2$, we can say:

- $g(n) = n^2$
- $n_0 = 3$
- $K = 2.5$

And we have $p_3 = O(n^2)$, that is a **quadratic** complexity.

As you can see, we don't even need to find lowest value of $K$ nor the very $n_0$ where $K\cdot g(n)$ starts to be greater than our number of operations. We only need to find numbers satisfying the condition and we can state the complexity family of our algorithm.

| A *quadratic complexity* |
|---|
| ![Linear](../images/other_visuals/2.2.2.1/11.png) |

`2.2.2.1.2-12` Here, our three problems had polynomial complexity, of degree 0, 1 and 2, but we can find problems whose complexity involves more exotic functions, such as the factorial or the logarithm for instance.

##### 2.2.2.2 - Other ways to express complexity

###### 2.2.2.2.1 - On other measures of algorithmic complexity

`2.2.2.2.1-0` The big O notation isn't the only tool computer scientists have in their toolbox. There are other ways to measure the algorithmic complexity of a program.

`2.2.2.2.1-1` Like we have *instances* for our classes, let's call *instances* of a problem a given input for a problem. As an example, `[1, 2, 3, 4]` and `[4, 1, 3, 2]` are two different instances of the same size for an algorithm sorting a list of numbers. An important thing to have in mind is that for two instances of a problem with the same size $n$, the same algorithm may do a different exact number of operations. For our two lists that are to be sorted, for most efficient sorting algorithms, the first step is to see how shuffled the list is, and then sort the shuffled parts. So on the first list, the algo would read all numbers, see that the list is already sorted and thus terminate. That would have taken a linear amount of operation (reading every number a constant number of time) so the complexity of sorting an already sorted list is related to $n$. On the other hand, our same algo would then sort the second list, and that would take roughly $n^2$ operations if our algo is inefficient, and $n\log(n)$ if we're a bit clever in the writing of our algorithm. So here, we have a kind of "best case scenario". It may be interesting to also provide information on how well the algo performs in the best case. As we saw, saying that our algo is in $O(f)$ means that for large numbers $n$, our algo does less operations than $f(n)$ times a constant. So we do have an upper bound, saying that in the worst case our algo won't do worse than $f$ times a constant for inputs with large sizes, but we don't express a lower bound here. In fact, the big O notation is a kind of metric for the worst case scenario.

`2.2.2.2.1-2` The worst case scenario is quite every time a better indication than the best case scenario, not because we computer scientists are pessimistic persons, but because it allows us to quickly know for a given size when we can expect an algorithm to have stopped.

`2.2.2.2.1-3` And of course, there are other complexity measures. There is for instance the *average* complexity, harder to compute, that indicates on average how many operations are expected. For some problems, we can have worst case scenario in $O(n^3)$ which is really bad for huge numbers, but know that on average the time complexity is $O(n)$ so in most cases the algorithm is usable for big tasks. When a task is repeated a lot, we can measure on a set of instances another kind of average complexity. All these indicators can help us understand an algorithm's complexity. In this video, already too long, we'll only focus on the $O$ notation.

###### 2.2.2.2.2 - On parallelization

`2.2.2.2.2-1` In this video, we'll only talk about a computer being able to do only one task at a time. Some problems, like this one in fact, can be solved faster by using multiple processes working together simultaneously. We could divide all our seed numbers let say into 100 lists, give that to 100 computers, they would all find their minimum location, then comparing 100 numbers to find the real minimum is a piece of cake but even that we could parallelize, and in less than one minute it would give the solution. But we won't do that as it feels like cheating to me to bring out the big guns to solve a coding challenge, and I don't own 100 computers nor do I have that many friends. So our optimization will be focused on solving the problem doing only one operation at a time, like we would do by hand, just a bazillion times faster.

---

---

---

##### 2.2.2.3 - Finding the complexity of our algorithm

Let's find out how efficient our algorithm is. The algorithm looks like:

```
- Constructing the objects:
    - For every intermediate function            - p times
        - For every shifter inside                   - k times at most
            - Add it                                     - O(1)
- Main algorithm:
    - min_value is infinity                      - O(1)
    - for every seed:                            - n times
        - compute the location value                 - O(?)
        - update the minimum value                   - O(1)
```

So, let's suppose we have $n$ seeds, we we'll do $n$ times the computation, which has a complexity unknown for now. Let's now find the complexity of that computation.

```
Computing the location value of seed:        
- for every intermediate function:           - p times
    - update the value                           - O(?) 
- return the value                           - O(1)

```

There's another loop, that would iterate $p$ times, $p$ being the number of intermediate functions we have. Everything now depends on how the value is updated, *i.e.* how we use `IntermediateFunction.apply`

```
Calling an IntermediateFunction:
- for every shifter:                         - at most k times
    - if the current shifter can be used:    - O(?)
        return the updated value                 - O(?)
- return the same value                      - O(1)
```

So in the best case we found a working shifter in the first try, and at worst we have to iterate through all shifters before realizing none could work. Let's suppose the max number of shifters an IntermediateFunction has is $k$. The last things we have to understand is the complexity of the update test, and the computation, *i.e.* the functions `Shifter.can_act_on` and `Shifter.apply`. Both of them only perform a constant number of operations, so they're both in $O(1)$.

The final complexity of our program is thus $O(p\cdot k) + O(n\cdot p\cdot k) = O(n\cdot p\cdot k)$, where $n$ is the number of seeds, $p$ the number of intermediate functions, and $k$ the maximum number of shifters in an intermediate function. Here, in both part 1 and 2, the number of intermediate functions stays constant, and same goes for the numbers of shifters per intermediate function. Thus, we would consider these numbers as constant. Finally, without much surprise, we can state our algo has a linear complexity, $O(n)$, which is what we expected when we computed the time we supposed it would take for part 2. You double the input's size, you double the expected time.

Is there good ways of optimizing an algorithm that is already linear?

#### 2.2.3 - Another perspective

We can't lie to ourselves: by the way the problem is stated, if we have $n$ inputs, we must at least test all our inputs to find the minimum, since in the way we implemented the problem we don't know how big the final location number would be only based on the seed number. The algorithm **must** be at least in $O(n)$. So that does mean we cannot reduce the amount of time base on our input's size. Sorry. But are we doomed? What if we change the way we measure our problem's size, and thus create another algorithm?

##### 2.2.3.1 - The solution layed in front of our eyes since the beginning

See how the part 2 describes not individual numbers of seeds, but *ranges of seeds*. What if we could manipulate the ranges themselves instead of individual numbers? In fact, we could even say that if we have $n$ ranges of max length $l$, our algo compute a location for every single seed, that is for every range of the $n$ ranges, and for every seed number of the $l$ seeds contained in that range. So, we're applying an operation taking a linear amount of time to deal with $n\cdot l$ seeds. If we assume (and as per the numbers, we can!) $l$ is larger than $n$, it takes at least $n\cdot n$ operations, since $l\geq n\Rightarrow n\cdot l\geq n\cdot n$. So, we could say in part one the $n$ numbers provided were handled in $O(n)$, but in part two they represented a much much bigger problem, handled in $O(n^2)$.

There is no contradiction between the complexity we computed and what we just stated. The complexity of our solution was linear since it was in the form:

```
- for every seed:                          - n times
    - compute and update the location          - O(1)
```

The "compute the location" part was constant, but done $n$ times. Now in part two we have:

```
- for every range:                         - n times
    - for every seed in that range:            - n times
        - compute and update the location          - O(1)
```

Which result in an $O(n^2)$ problem, being quite bad.

What we would like is something like:

```
- for every range:                         - n times 
    - compute and update the location          - O(f(n))
```

Where $f(n)$ growing slower than $n$ in order to have a worst case complexity smaller than $O(n^2)$

##### 2.2.3.2 - Introducing ranges

As we can see, in general close enough inputs result in close enough outputs. That is because our intermediary functions are based on shifters, and they do move a whole range altogether. So if $a$ and $a+1$ can be mapped by a shifter, they will become $a+d$ and $a+d+1$ respectively. When they can't be mapped by the same shifter, their path would split, resulting in a gap between the two outputs. So instead of computing every number of a range, we can take advantage in dividing our initial range into new ranges, corresponding to all the "breaking points" of an intermediate function, the points where two consecutive input numbers are mapped to two not consecutive numbers. We then know that each range corresponds to only one shifter, so we can map each one to another output range. So, for a given intermediate function and a given range of length $l$, the output will be a list of $k$ ranges; finding the lowest value of them takes only $k$ operations, instead of testing every $l$ values of our beginning range!

| ![What we would like about ranges](../images/generation/part_2/function_on_range.gif) |
|:-:|
| instead of testing every seed of the input range, we test only the minimums of the four output ranges! |

Let's create our new object, the `Range` object. It has a min value, and a max value. We'll consider an open range $⟦a, b⟦$, so 

```python
class Range:
    def __init__(self, min: int, max: int):
        """Represents the range [min, max["""
        self.min = min
        self.max = max
```
And that's basically it. What we want to do on a range is:

- Split it,
- Shift it.

Let's begin by shifting it as it's easier. The whole range shifts by a value `delta`, so from `[min, max[` it becomes `[min+delta, max+delta[`.

```python
    def shift(self, amount: int) -> None:
        """Shift the range by a given amount"""
        self.min += amount
        self.max += amount
```

For the splitting of a range, we'll first consider a simple case, where it's cut in two ranges. If we have `a`, `b` and `c` such that `a` $<$ `b` $<$ `c`, then the range `[a, c[` cut in `b` gives the range `[a, b[` and the range `[b, c[`. Simple, right? We consider `b` to be the min value of our second range, and not the max value of our first range. `b` $\not\in$ `[a, b[`.
Now let's split a range into more ranges. To do so we'll need, beside a `Range`, a list of splitting points. We assume these points are ordered. Then, we'll keep only the values of these points between the interval represented by the range, surrounded by our min-max values. For instance the range `Range(5, 10)` and the splitting point list `[1, 3, 7, 8, 14]` give the list `[5, 7, 8, 10]`. By taking these values two by two, we can construct ranges (here 3) corresponding to our initial range subdivided, here `Range(5, 7)`, `Range(7, 8)` and `Range(8, 10)`. Because our `Range` structure is open on the right, it allows us to simply divide ranges. See for instance if we had represented ranges closed on the right, the initial range would have been written as `Range(5, 9)`, since the value 10 doesn't belong to our range, and splitting it with the same values would have resulted in ranges `Range(5, 6)`, `Range(7, 7)` and `Range(8, 9)`. Same concept, but a different representation.

<p align="center"><img src="../images/other_visuals/ranges.svg" height="400"/></p>

As we can see, even though it's easier to understand that after the split, one of the ranges is a single element (7), with all the shift-by-one errors we'll stick to an open range on the right, since it's the easiest.

```python
    def split_range(self, breakpoints: list[int]) -> list["Range"]:
        valid_points: list[int] = [self.min] # insert the min
        for point in breakpoints: # insert every value between min and max
            if self.min < point < self.max:  
                valid_points.append(point)
        valid_points.append(self.max) # insert the max

        final_ranges: list[Range] = []
        for i in range(len(valid_points) - 1): # insert every pair of points as a new range
            final_ranges.append(Range(valid_points[i], valid_points[i + 1]))
        return final_ranges
```

Now we know how to split a range, we want apply an intermediary function to a whole range. To do so, we need to:

1. Understand where we have to split the range,
2. Split the range accordingly,
3. For every new range, shift that new range according to the corresponding shifter of our intermediate function.

As we can see, the splitting points can only be the min and max of our shifters. So for a given `IntermediateFunction` we want to know all the mins and maxes of its shifters. Let's create a method called `compute_breakpoints` that returns an ordered list of integers corresponding to these splitting points.

```python
    def compute_breakpoints(self) -> list[int]:
        breakpoints = set()
        for rule in self.shifters:
            breakpoints.add(rule.min)
            breakpoints.add(rule.max)
        breakpoints = list(breakpoints)
        breakpoints.sort()
        return breakpoints
```

If a min value of a shifter is the same as the max value of another, we do not want to use this value twice, we only want to know that this value exists. So we first create a `set`, which is a collection of unique items. If you add an item in a set, if it is not in the set yet it will be added, and if it already is, nothing happens. Then for every shifter of our intermediate function we add to the set the mins and the maxes. Finally, we convert the set into a list, and we sort that list using python built-in function `sort` (whose complexity is $O(n\log(n))$ ). Easy.

Now, we're ready to apply an Intermediate Function on a Range! The first thing to do is to divide our function into two new functions:

```python
    def apply(self, value: int | Range) -> int | list[Range]:
        if type(value) == int:
            return self._call_on_int(value)
        else:
            return self._call_on_range(value)

    def _call_on_int(self, value: int) -> int: # The old function
        for param in self.shifters:
            if param.can_act_on(value):
                return param.apply(value)
        return value

    def _call_on_range(self, value: Range) -> list[Range]:
        ... # What we're about to fill
```

In order to keep the code simple, we check if the parameter `value` is a regular integer, or a `Range` object, and depending on that we delegate the computation to `_call_on_int` which is the old function, or `_call_on_range`, a new function we're about to write.

In Python it's common to create function beginning by an underscore to denote "private" functions, functions that are mostly for internal shenanigans, here someone wanting to use our object is not interested in how it works internally, they only want to use the highest level function `apply` and not to care about the rest. So only the functions useful for the very resolution of the problem and not the internal mechanism begin without underscores.

To call an intermediate function on a Range, we first have to split it on the breakpoints we've just computed. But these breakpoints don't depend on our Range, so it's inefficient to compute them again and again for every range we apply our function on. So when constructing the object, let's rename and call `_compute_breakpoints` once and for all and store the result in a field called `breakpoints`:

```python
class IntermediateFunction:
    def __init__(self, shifters: list[Shifter]):
        self.shifters = shifters
        self.breakpoints = self._compute_breakpoints()
    ...
```

Okay, now finally for the computation:

```python
    def _call_on_range(self, value: Range) -> list[Range]:
        new_ranges = value.split_range(self.breakpoints)
        for range in new_ranges:
            for shifter in self.shifters:
                if shifter.can_act_on(range.min):
                    range.shift(shifter.delta)
                    break
        return new_ranges
```

For every split range, like we did with integers, we iterate through all shifters to find one we can apply. Since it's guaranteed our ranges are split by the shifter, they individually either fit exactly in only one shifter, or fit none of them. So we only have to test a shifter on a single value of the range. Since the ranges could technically be one element, and that `our_range.min` is inside our range, we can try only the min value of the range. If the shifter can act on that value, it can act on our range and we shift the whole range by the shifter's delta. We then return all the shifted ranges. And voilà, a `Range` can now pass through an `IntermediateFunction`!

##### 2.2.3.3 - On Complexity... again!

But wait you may ask, on part 1 our functions turned single values into single values, now our intermediate functions turn a range into a list of ranges. So there are two things to think about:

1. How do we map the outputs of an intermediate function to the next intermediate function's input?
2. How many ranges will there be in our final list..? Isn't that number going to be quite huge?

To quickly answer the first question, we will consider our intermediate functions can take a list or ranges as input and outputs another list of range. That is for every range in our input list we compute the output ranges and we combine them in a single list. Simple as.

For the second question, let's look at real data from the solved problem. As we can see, we can indeed have simple cases and less simple cases. If a ranges is mapped to a list of new ranges, and every output range is also mapped to a list of new ranges, we have a tree of ranges, growing quite complexly.

![A simple case](../images/generation/part_2/r_trees_simple.svg)

![A more complex case](../images/generation/part_2/r_trees_complex.svg)

| ![All trees](../images/generation/part_2/r_trees_all.svg) |
|:-:|
| Interactive image! |

| ![All trees](../images/generation/part_2/r_trees_all_groups.svg) |
|:-:|
| Interactive image! |

Before finding the worst case scenario, let's begin with a very simple set. We have one input range and one single shifter in our single intermediary function. Immediately we can see that to create more output ranges, the breakpoints of our intermediate function must interfere with our input. If we are fully outside, we have the same number of output ranges (here one), if only one breakpoint splits the input, we create one more output range, and if both breakpoints split the input, we create two more ranges in our outputs.

| Fully outside | One breakpoint inside | Both breakpoints inside |
|:-:|:-:|:-:|
| ![0_0](../images/generation/part_2/number_of_ranges_0_0.gif) | ![0_1](../images/generation/part_2/number_of_ranges_0_1.gif) | ![0_2](../images/generation/part_2/number_of_ranges_0_2.gif) |

If we have now two input ranges, that's the same: the more breakpoints inside our input ranges, the more output ranges.

| Breakpoints on two different ranges | Breakpoints on the same range |
|:-:|:-:|
| ![1_0](../images/generation/part_2/number_of_ranges_1_0.gif) | ![1_0](../images/generation/part_2/number_of_ranges_1_1.gif) |

As we can see it doesn't matter where a breakpoint ends, as long as it's inside a range. Here for instance, we still have four ranges at the end if the two breakpoints of our single shifter do split some ranges, be they the same or be they two different ranges.

| One shifter | Two shifters | Three shifters |
|:-:|:-:|:-:|
| ![2_0](../images/generation/part_2/number_of_ranges_2_0.gif) | ![2_1](../images/generation/part_2/number_of_ranges_2_1.gif) | ![2_2](../images/generation/part_2/number_of_ranges_2_2.gif) |

So, for every shifter we add in our intermediate function, we can make two breakpoints split our input ranges and thus create two more ranges, at most.

So now let's do some math. Let's suppose our initial list of ranges has a length of $l_0$. Let's suppose our intermediate functions have at most $k$ shifters. Let denote $l_n$ the maximum number of ranges we're dealing with after they went through $f_1, \cdots, f_n$. Since at every step we can create only $2k$ ranges, we have:

$$l_{n+1} = l_n + 2\cdot k$$

So:

$$
\begin{split}
    l_1 &= l_0 + 2k = l_0+2k\\
    l_2 &= l_1 + 2k = l_0+4k\\
    l_3 &= l_2 + 2k = l_0+6k\\
    &\cdots\\
    l_n &= l_0+2kn\\
\end{split}
$$

The final number of ranges at the end is in $O(l_0+kp)$, where $k$ is the max number of shifters per intermediate function, $l_0$ the number of ranges we have at the beginning and $p$ the number of intermediate functions we have to go through. But does that mean our algorithm is efficient? Well, that's not really the number of computation we have to perform. We only computed the number of ranges we had after the nth function.

Let's decompose our algo in pseudo code:

```
1 - Creation of the objects
- For every function                       - p times
    - For every shifter inside, add it         - O(k)
    - Create the breakpoints                   - O(k*log(k))

2 - Main Loop
- For every function f_i:                  - p times
    - For every input range:                   - l_i times
        - Split it                                 - O(k)
        - For every new range:                     - How do we count that ???
            - Find the shifter                         - O(k)
            - Shift it                                 - O(1)

3 - Finding the minimum
- For every final range:                   - at most l_0 + p*k times
    - Update the minimum                       - O(1)
```

Okay, we have ourselves in a really complicated situation, because we can't really know for a given input range in how much ranges it will be split... What we can know is to modify the pseudo code so that it's easier to count. Let's focus on the second part, the main loop:

```
- For every function f_i:                  - p times
    - For every input range:                   - l_(i-1) times
        - Split it                                 - O(k)
        - Keep it in memory                        - O(1)
    - For every new range we kept in memory:   - l_(i) times
        - Find the shifter                         - O(k)
        - Shift it                                 - O(1)
```

As we can see, our pseudo code is different but still performs the exact same number of operations, as all the input list of ranges for a given intermediate function becomes a list with a new amount or ranges that are to be shifted, and per unsplit range we don't know how many new ranges are created after the split, we know that number when dealing with all the input ranges at one.

Let's now count the number of operations for part two. Let's call $N_1$, $N_2$ and $N_3$ the number of operations of the three main parts of our pseudo code. Each count is a function of $k$, the max number of shifters per intermediate function, $l_0$ the initial number of ranges and $p$ the number of intermediate functions we have to deal with.

$$
\begin{split}
N(k, l_0, p) &= N_1(k, l_0, p) + N_2(k, l_0, p) + N_3(k, l_0, p)\\
&= O(p\cdot k\log(k)) + N_2(k, l_0, p) + O(l_0+pk)\\
\end{split}
$$

And now let's focus on $N_2(k, l_0, p)$. We will sum the number of operation for every intermediate function, from $f_1$ up to $f_p$. For every function, as we saw that's $l_{i-1}\cdot O(k) + l_i\cdot O(k)$. So we have:

$$
\begin{split}
N_2(k, l_0, p) &= \sum_{i=1}^p \left((l_{i-1} + l_i)\cdot O(k) \right)\\
&= O(k)\cdot\sum_{i=1}^p ((l_0 + 2k(i-1)) + (l_0 + 2ki))\\
&= O(k)\cdot\sum_{i=1}^p (2l_0 + 4ki - 2k)\\
&= O(k)\cdot2\sum_{i=1}^p (l_0 - k + 2ki)\\
&= O(k)\cdot2\left(\sum_{i=1}^p (l_0 - k)  + 2k\sum_{i=1}^p (i) \right)\\
&= O(k)\cdot2\left( p(l_0 - k) + 2k\dfrac{p(p+1)}{2} \right)\\
&= O(k)\cdot2\left( pl_0 - kp + kp(p+1) \right)\\
&= O(k)\cdot2( pl_0 + kp^2)\\
&= O(kpl_0 + k^2p^2)\\
\end{split}
$$

And since $N_2$ grows faster than $N_1$ and $N_3$ for every variable, that's also the final complexity of our algorithm.

That seems quite bad, right? That literally means that the number of operations grows like a quadratic function if we increase $k$ the number of shifters in our intermediate functions or increase $p$ the number of intermediate functions we stack, and that it grows linearly if we increase the number $l_0$ of ranges we have at the beginning.

But here comes the good news: We don't consider all these variables as part of the problem's size. Between part one and part two, our shifters and our intermediate functions did not change. In fact, we will only consider the number of items we give as an input to our algorithm as the size of the problem, be there single numbers or be there ranges. In fact, we could treat single numbers as ranges of length 1. 🤯 We consider the number of intermediate functions we have to be constant (here $p=7$), and since these intermediate functions are *given*, the maximum number of shifters per intermediate function is also constant. Here, $k=45$. So, we have an algorithm in $O(n)$ where $n$ is the real size of the problem, i.e. the number of input ranges we previously called $l_0$. We created an algorithm with the same complexity as the first, but dealing with ranges of numbers instead of single numbers!

It is now guaranteed that after the 7th operation, we have a maximum number of $l+14\cdot k$ ranges. Since $k$ and $l$ are quite small numbers, in our instance $l=10$ and $k=45$ that could mean a max number of $640$ output ranges. That's not a lot for a computer, and fortunately, all intermediate functions don't have 45 shifters, in fact the second highest number of shifters in "only" 23 in our instance. And when computing the exact number for this instance of the problem, we "only" have 54 ranges at the very end, and throughout every step we called an intermediary function on a range only 192 times. These numbers are indeed really small compared to our billions of calls at the beginning.

##### 2.2.3.4 - Finally, the solution \o/

Let's finally solve that second part. Instead of handling every input range separately, we'll combine them into a single list. So for every intermediate function we have a list of input ranges, and we want to compute a list of output ranges. Let's write our last function:

```python
def compute_min_location_for_seed_range(ranges_in: list[Range]) -> int:
    for i_f in intermediate_functions:
        ranges_out = []  # We create an empty list that will store all our output ranges
        for r in ranges_in:
            current_ranges_out = i_f.apply(r)  # We compute the output ranges of our current range
            for range_out in current_ranges_out:  # We add them into our big output list
                ranges_out.append(range_out) 
        ranges_in = ranges_out  # We set our output ranges as the inputs for the next iteration

    # Finally, we iterate through all final output ranges to find the minimum value
    min_value = float("+inf")
    for r in ranges_in:  # Now the last ranges we have after all the computations
        min_value = min(min_value, r.min)  # The lowest value must be the lowest bound of a range
    return min_value
```

We test on the dummy input, it instantly gives the right answer, `46`, and if we enter the right values... `13####820` Here we are! We finally have the answer!

## 3 - Conclusion

### 3.1 - Quick recap

Here, we solved a problem from the Advent of Code. The first part of the problem was no challenge to us, as we just had to formalize and create the logics to deal with it. Part two could not be solved with the same algorithm, or at least not in a short amount of time since we went from an algorithm in $O(n)$ where $n$ was small to an algorithm in $O(n^2)$ where $n$ was rather big. We then tweaked our algorithm to make it work in $O(n)$ where the size of the problem became not the number of seeds we had, but the number of ranges of seeds we had.

### 3.2 - What could have been done better?

As perfectly working, our algorithm is far from perfect. For instance, when ranges are outputted from an intermediate function, if they're close enough or even worse, overlapping, they could have been merged into a single one. Oh and remember iterating through *all* shifters to find the one that can shift our seeds or our range? That could have been optimized too. It wouldn't change the overall complexity, but there's a lot we could do to improve our algorithm. What I tried to do here, is not to show you how to craft the most efficient (and unreadable) algorithm, but to understand what are the problems we face and the tools we use in computer science, and used an enjoying puzzle from a lovely challenge I found amusing. There are of course many other ways to tackle down this problem. We could have followed the train of thought saying that two inputs cannot be mapped to the same output, and that every output has an input to conclude $f$ is a bijective function, and thus inversible. From there we could have literally tried to take the problem backward...

| ![Range optimization](../images/generation/part_2/number_of_ranges_simplification.gif) |
|:-:|
| Optimization of the ranges after passing through a function |

Mathematically, there's a lot of approximation, like the way we dealt with $O(n)$ like if was a number, in order not to make the video more complicated, or the way we talked about the $O$ notation and not the $\Theta$ notation that would sometimes make more sense... I home my fellow maths enjoyers and computer scientists will forgive me on this one, as I intended not to give a course on computer science, but to show a bit of what computer science or complexity is about.

### 3.3 - Final words

I hope you enjoyed this video as much as I enjoyed making it ; I hope if these topics were new to you you found interest in these and learnt something, and if you were already familiar with basic computer science, you were pleased with the way it was shown. Let me know in the comments, I'll read every feedbacks, so if i ever do another video I'll know what to improve! 😃