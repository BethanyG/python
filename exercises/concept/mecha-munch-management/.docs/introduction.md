# Dictionary Methods in Python

A dictionary (`dict`) is a Python data structure that associates [hashable][term-hashable] _keys_ to _values_.
It is known in other programming languages as a resizable [hash table][hashtable-wikipedia], hashmap, or [associative array][associative-array].
Dictionaries are Python's only built-in [mapping type][mapping-types-dict].

Dictionaries enable the retrieval of a `value` in (on average) constant O(1) time, given the `key`.
Compared to searching for a value within a `list` or `array` (_without knowing the `index` position_), a `dict` uses significantly more memory, but has very rapid retrieval.
Dictionaries are especially useful in scenarios where the collection of items is large and must be updated and accessed frequently.


## Dictionary Methods

The `dict` class in Python provides many useful [methods][dict-methods] for working with dictionaries.
Some were introduced in the concept for `dicts`.
Here we cover a few more - along with some techniques for iterating through and manipulating dictionaries.

### `setdefault()` for Error-Free Insertion

The dictionary concept previously covered that `.get(key, <default value>)` returns an existing `value` or the `default value` if a `key` is not found in a dictionary, thereby avoiding a `KeyError`.
 This works well in situations where you would rather not have extra error handling but cannot trust that a looked-for `key` will be present.

For a similarly "safe" (_without KeyError_) insertion operation, there is the `.setdefault(key, <default return value>)` method.
`setdefault(key, <default value>)` will return the `value` if the `key` is found in the dictionary.
If the key is **not** found, it will _insert_ the (key, `default value`) pair and return the `default value` for use.

```python
>>> palette_I = {'Grassy Green': '#9bc400', 'Purple Mountains Majesty': '#8076a3', 'Misty Mountain Pink': '#f9c5bd'}

#Looking for the value associated with key "Rock Brown".
#The key does not exist, so it is added with the default value, and the value is returned.
>>> palette.setdefault('Rock Brown', '#694605')
'#694605'

#The (key, default value) pair has now been added to the dictionary.
>>> palette_I
{'Grassy Green': '#9bc400', 'Purple Mountains Majesty': '#8076a3', 'Misty Mountain Pink': '#f9c5bd', 'Rock Brown': '#694605'}
```

### Use `fromkeys()` to Populate a Dictionary

To quickly populate a dictionary with various `keys` and default values, the _class method_ [`fromkeys(iterable, <default value>)`][fromkeys] will iterate through an iterable of `keys` and create a new `dict`.
All `values` will be set to the `default value` provided:

```python
>>> new_dict = dict.fromkeys(['Grassy Green', 'Purple Mountains Majesty', 'Misty Mountain Pink'], 'fill in hex color here')

{'Grassy Green': 'fill in hex color here',
 'Purple Mountains Majesty': 'fill in hex color here',
 'Misty Mountain Pink': 'fill in hex color here'}
```

### Remove and Return a (key, value) Pair With `.popitem()`

`.popitem()` removes & returns a single (`key`, `value`) pair from a dictionary.
Pairs are returned in Last-in,First-out (`LIFO`) order.
If the dictionary is empty, calling `popitem()` will raise a `KeyError`:

```python
>>> palette_I = {'Grassy Green': '#9bc400', 'Purple Mountains Majesty': '#8076a3', 'Misty Mountain Pink': '#f9c5bd'}
>>> palette_I.popitem()
('Misty Mountain Pink', '#f9c5bd')

>>> palette_I.popitem()
('Purple Mountains Majesty', '#8076a3')

>>> palette_I.popitem()
('Grassy Green', '#9bc400')

#All (key, value) pairs have been removed.
>>> palette_I.popitem()
Traceback (most recent call last):

  line 1, in <module>
    palette_I.popitem()

KeyError: 'popitem(): dictionary is empty'
```

### Iterating Over Entries in a Dictionary

The `.keys()`, `.values()`, and `.items()` methods return [_iterable views_][dict-views] of a dictionary.

These views can be used for looping over entries without altering them.
They are also _dynamic_ -- when underlying dictionary data changes, the associated view object will reflect the change:

```python
>>> palette_I = {'Grassy Green': '#9bc400',
                 'Purple Mountains Majesty': '#8076a3',
                  'Misty Mountain Pink': '#f9c5bd'}

#Using .keys() returns a list of keys.
>>> palette_I.keys()
dict_keys(['Grassy Green', 'Purple Mountains Majesty', 'Misty Mountain Pink'])

#Using .values() returns a list of values.
>>> palette_I.values()
dict_values(['#9bc400', '#8076a3', '#f9c5bd'])

#Using .items() returns a list of (key, value) tuples.
>>> palette_I.items()
dict_items([('Grassy Green', '#9bc400'), ('Purple Mountains Majesty', '#8076a3'), ('Misty Mountain Pink', '#f9c5bd')])

#Views are dynamic.  Changing values in the dict changes all of the associated views.
>>> palette_I['Purple Mountains Majesty'] = (128, 118, 163)
>>> palette_I['Deep Red'] = '#932432'

>>> palette_I.values()
dict_values(['#9bc400', (128, 118, 163), '#f9c5bd', '#932432'])

>>> palette_I.keys()
dict_keys(['Grassy Green', 'Purple Mountains Majesty', 'Misty Mountain Pink', 'Deep Red'])

>>> palette_I.items()
dict_items([('Grassy Green', '#9bc400'), ('Purple Mountains Majesty', (128, 118, 163)), ('Misty Mountain Pink', '#f9c5bd'), ('Deep Red', '#932432')])
```

### More on `.keys()`, `.values()`, and `.items()`

In Python 3.7+, `dicts` preserve the order in which entries are inserted allowing First-in, First-out (_`FIFO`_),  iteration using `.keys()`, `.values()`, or `.items().`
In Python 3.8+, views are also _reversible_.
This allows keys, values, or (key, value) pairs to be iterated over in Last-in, First-out (`LIFO`) order by using `reversed(<dict>.keys())`, `reversed(<dict>.values())`, or `reversed(<dict>.items())`:

```python
>>> palette_II = {'Factory Stone Purple': '#7c677f', 'Green Treeline': '#478559', 'Purple baseline': '#161748'}

#Iterating in insertion order 
>>> for item in palette_II.items():
...     print(item)
...
('Factory Stone Purple', '#7c677f')
('Green Treeline', '#478559')
('Purple baseline', '#161748')


#Iterating in the reverse direction.
>>> for item in reversed(palette_II.items()):
...    print (item)
...
('Purple baseline', '#161748')
('Green Treeline', '#478559')
('Factory Stone Purple', '#7c677f')

```

### Combining Dictionaries with `.update()`

`<dict_one>.update(<dict_two>)` can be used to _combine_ two dictionaries.
This method will take the (`key`,`value`) pairs of `<dict_two>` and write them into `<dict_one>`:

```python
>>> palette_I = {'Grassy Green': '#9bc400',
                 'Purple Mountains Majesty': '#8076a3',
                  'Misty Mountain Pink': '#f9c5bd'}
>>> palette_II = {'Factory Stone Purple': '#7c677f',
                  'Green Treeline': '#478559',
                  'Purple Baseline': '#161748'}

>>> palette_I.update(palette_II)

#Note that new items from palette_II are added.
>>> palette_I
{'Grassy Green': '#9bc400', 'Purple Mountains Majesty': '#8076a3', 'Misty Mountain Pink': '#f9c5bd', 'Factory Stone Purple': '#7c677f', 'Green Treeline': '#478559', 'Purple Baseline': '#161748'}
```

Where keys in the two dictionaries _overlap_, the `value` in `dict_one` will be _overwritten_ by the corresponding `value` from `dict_two`:

```python
>>> palette_I =   {'Grassy Green': '#9bc400', 'Purple Mountains Majesty': '#8076a3', 'Misty Mountain Pink': '#f9c5bd', 
                   'Factory Stone Purple': '#7c677f', 'Green Treeline': '#478559', 'Purple baseline': '#161748'}
>>> palette_III = {'Grassy Green': (155, 196, 0), 'Purple Mountains Majesty': (128, 118, 163),
                   'Misty Mountain Pink': (249, 197, 189)}
>>> palette_I.update(palette_III)

#Overlapping values in palette_I are replaced with values from palette_III
>>> palette_I
{'Grassy Green': (155, 196, 0),
  'Purple Mountains Majesty': (128, 118, 163), 
  'Misty Mountain Pink': (249, 197, 189), 
  'Factory Stone Purple': '#7c677f', 
  'Green Treeline': '#478559', 'Purple baseline': '#161748'}
```

### Merging and Updating Dictionaries Via the Union (`|`) Operator

Python 3.9 introduces a different means of merging `dicts`:  the `union` operators.
`dict_one | dict_two` will create a **new dictionary**, made up of the (`key`, `value`) pairs of `dict_one` and `dict_two`.
When both dictionaries share keys, `dict_two` values take precedence.


```python
>>> palette_I = {'Grassy Green': '#9bc400', 'Purple Mountains Majesty': '#8076a3', 'Misty Mountain Pink': '#f9c5bd'}
>>> palette_II = {'Factory Stone Purple': '#7c677f', 'Green Treeline': '#478559', 'Purple baseline': '#161748'}
>>> new_dict = palette_I | palette_II
>>> new_dict
...
{'Grassy Green': '#9bc400',
 'Purple Mountains Majesty': '#8076a3',
 'Misty Mountain Pink': '#f9c5bd',
 'Factory Stone Purple': '#7c677f',
 'Green Treeline': '#478559',
 'Purple baseline': '#161748'}
```

`dict_one |= other` behaves similar to `<dict_one>.update(<dict_two>)`, but in this case, `other` can be either a `dict` or an iterable of (`key`, `value`) pairs:

```python
 >>> palette_III = {'Grassy Green': (155, 196, 0),
                    'Purple Mountains Majesty': (128, 118, 163),
                    'Misty Mountain Pink': (249, 197, 189)}
 >>> new_dict |= palette_III
 >>> new_dict
 ...
 {'Grassy Green': (155, 196, 0),
 'Purple Mountains Majesty': (128, 118, 163),
 'Misty Mountain Pink': (249, 197, 189),
 'Factory Stone Purple': '#7c677f',
 'Green Treeline': '#478559',
 'Purple baseline': '#161748'}
```


### Sorting a Dictionary

Dictionaries do not have a built-in sorting method.
 However, it is possible to sort a `dict` _view_ using the built-in function `sorted()` with `.items()`.
The sorted view can then be used to create a new dictionary.
Unless a _sort key_ is specified, the default sort is over dictionary `keys`.

```python
# Default ordering for a dictionary is last in, first out (LIFO).
>>> color_palette = {'Grassy Green': '#9bc400', 
                    'Purple Mountains Majesty': '#8076a3', 
                    'Misty Mountain Pink': '#f9c5bd', 
                    'Factory Stone Purple': '#7c677f', 
                    'Green Treeline': '#478559', 
                    'Purple baseline': '#161748'}
 
 
# The default sort order for a dictionary uses the keys.
>>> sorted_palette = dict(sorted(color_palette.items()))
>>> sorted_palette
{'Factory Stone Purple': '#7c677f',
 'Grassy Green': '#9bc400',
 'Green Treeline': '#478559',
 'Misty Mountain Pink': '#f9c5bd',
 'Purple Mountains Majesty': '#8076a3',
 'Purple baseline': '#161748'}


# A sort key can be provided in the for of an anonymous function (lambda). 
>>> value_sorted_palette = dict(sorted(color_palette.items(), key=lambda color: color[1]))
>>> value_sorted_palette
{'Purple baseline': '#161748',
 'Green Treeline': '#478559',
 'Factory Stone Purple': '#7c677f',
 'Purple Mountains Majesty': '#8076a3',
 'Grassy Green': '#9bc400',
 'Misty Mountain Pink': '#f9c5bd'} 
```

For a detailed explanation of dictionaries and methods for working with them, the [official tutorial][dicts-docs] and the [official library reference][mapping-types-dict] are excellent starting places.
For more on sorting, see the [Sorting HOW TO][sorting-howto] in the python docs.
 [Real Python][how-to-dicts] and [Finxter][fi-dict-guide] also have very thorough articles on Python dictionaries.


[associative-array]: https://en.wikipedia.org/wiki/Associative_array#:~:text=In%20computer%20science%2C%20an%20associative,a%20function%20with%20finite%20domain.
[dict-methods]: https://docs.python.org/3/library/stdtypes.html#dict
[dict-views]: https://docs.python.org/3/library/stdtypes.html#dict-views
[dicts-docs]: https://docs.python.org/3/tutorial/datastructures.html#dictionaries
[fi-dict-guide]: https://blog.finxter.com/python-dictionary
[fromkeys]: https://docs.python.org/3/library/stdtypes.html#dict.fromkeys
[hashtable-wikipedia]: https://en.wikipedia.org/wiki/Hash_table
[how-to-dicts]: https://www.w3schools.com/python/python_dictionaries.asp
[mapping-types-dict]: https://docs.python.org/3/library/stdtypes.html#mapping-types-dict
[sorting-howto]: https://docs.python.org/3/howto/sorting.html
[term-hashable]: https://docs.python.org/3/glossary.html#term-hashable