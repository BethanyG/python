# Instructions

## Instructions

On a trick to Vegas, Elyse is excited to play some poker. Not wanting to play too many rounds, she wants to track how many hands she plays - and which hands those are. Every hand has a number which makes tracking them easier. In this exercise, we'll use lists to help Elyse track what hands she plays.

## 1. Creating a List

Elyse sits down at the table to play her first hand. The table shows the hand number and Elyse plans on playing three hands.

Implement a function `to_list` that takes and hand number and returns a single list with that hand and the next two in the list:

```python
>>> to_list(27)
[27, 28, 29]
```

## 2. Concatenating Lists

Elyse played a few hands, took a break and played some more hands ... but accidentally started a new list! She wants to put those together.

Implement a function `concatenate_lists` that takes two lists and returns a single list consisting of all the elements in the first list, followed by all the elements in the second list:

```python
>>> concatenate_lists([27, 28, 29], [35, 36])
[27, 28, 29, 35, 36]
```

## 4. Testing List Membership

Talking about prior hands, someone remarks how similar two hands played out. Elyse is not sure if she played those hands.

Implement a function `list_contains_object` that takes two arguments, a list and a number, and returns `True` if the number is an element of the list:

```python
>>> list_contains_object([27, 28, 29, 35, 36], 29)
True

>>> list_contains_object([27, 28, 29, 35, 36], 30)
False
```

## 3. Iterating Over List Items

Elyse likes analyzing data and seeing what can be done with them. One very basic way to summary a list of numbers if with the average. The average can be found by summing up all the values and the number of values then dividing the sum by the count.

Implement a function `average` that returns the average value of a list of numbers.

```python
>>> average([5, 6, 7])
6.0
```

## 4. Accessing List Elements by Index

Elyse realizes she can very roughly get an average-like number by taking the average of the first and last number or by taking the middle number (the median). Would either of those happen to give the same number as the average?

Implement a function `approx_average_is_average` that returns a boolean, indicating if either approximation is the same as the average. For the sake of a simple median, assume the list always has an odd number of values.

```python
>>> approx_average_is_average([1, 2, 3])
True
>>> approx_average_is_average([1, 2, 4, 5, 8])
True
>>> approx_average_is_average([2, 3, 4, 8, 8])
True
>>> approx_average_is_average([1, 2, 3, 5, 9])
False
```

## 5. Accessing Sublists by Slicing

Intrigued by these results, Elyse is wondering if taking the average of even elements versus the average of odd elements would give the same results.

Implement a function `average_even_is_average_odd` that returns a boolean indicating of the average of the even is the same as the average of the odd elements.

```python
>>> average_even_is_average_odd([1, 2, 3])
True
>>> average_even_is_average_odd([1, 2, 3, 4])
False
```

## 6. Modifying Values in Lists

The last number in the list looks a bit low. It might need doubling if it is below a certain threshold.

Implement a function `maybe_double_last` that takes a list and a number. If the last element in the list is smaller than the number, double it.

```python
>>> _list = [5, 10, 15]
>>> maybe_double_last(_list, 20)
>>> _list
[5, 10, 30]
>>> maybe_double_last(_list, 20)
>>> _list
[5, 10, 30]
```
