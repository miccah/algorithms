# algorithms

Notes on the many algorithm books I recently acquired.

* *Grokking Algorithms* by Aditya Y Bhargava
* *Fabulous Adventures in Data Structures and Algorithms* by Eric Lippert
* *Advanced Algorithms and Data Structures* by Marcello La Rocca
* *Algorithms and Data Structures for Massive Datasets* by Dzejla Medjedovic, Emin Tahirovic, and Ines Dedovic


## Table of Contents


## Grokking Algorithms

This book is designed to be easy to follow and leads with examples. After
reading a few chapters, I think it is going to serve as more of a review for
me, because it assumes absolutely no previous knowledge of algorithms
(including big-O notation, how memory works, arrays, etc.).


### Binary Search

Binary search is an algorithm that efficiently finds an element in a sorted
list. It works by choosing the midpoints and comparing to the element,
eliminating half the search space with each step, which means it runs in
**O(log n)** time.

```py
# Copied from the book.
def binary_search(arr, item):
  low = 0
  high = len(arr)-1

  while low <= high:
    mid = (low + high) // 2
    guess = arr[mid]
    if guess == item:
      return mid
    elif guess > item:
      high = mid - 1
    else:
      low = mid + 1
  return None


my_list = [1, 3, 5, 7, 9]

print(binary_search(my_list, 3))  # => 1
print(binary_search(my_list, -1)) # => None
```

#### Exercises

> **1.1** Suppose you have a sorted list of 128 names, and you’re searching
> through it using binary search. What’s the maximum number of steps it would
> take?*

`log₂(128) = 7`

> **1.2** Suppose you double the size of the list. What’s the maximum number of
> steps now?

Each step reduces the search space by half, so if the initial search space is
doubled, that's one more step.

*1.3 through 1.6 are asking for the big O runtime.*

> **1.3** You have a name, and you want to find the person’s phone number in
> the phone book. What's the big O runtime?

You can do a binary search, so **O(log n)**

> **1.4** You have a phone number, and you want to find the person’s name in
> the phone book. (Hint: You’ll have to search through the whole book!)

Searching through the whole book is linear, or **O(n)**.

> **1.5** You want to read the numbers of every person in the phone book.

Going through the whole book is linear, or **O(n)**.

> **1.6** You want to read the numbers of just the As. (This is a tricky one! It
> involves concepts that are covered more in chapter 4. Read the answer—you may
> be surprised!)

The time still grows linearly, even if you don't go through the entire book, so
**O(n)**.


### Traveling Salesperson

The traveling salesperson is a classic problem in computer science, where a
"salesperson" needs to visit "cities" and travel the fewest "miles." Solving it
(perfectly) takes **O(n!)** time because we need to check every possible
combination of city orders.


### Selection Sort

Selection sort is an **O(n²)** algorithm to sort an array. It works by
iterating through the list to *select* the lowest (or highest) element and
putting it in a new list.

```python
def findSmallest(arr):
  smallest = arr[0]
  smallest_index = 0
  for i in range(1, len(arr)):
    if arr[i] < smallest:
      smallest = arr[i]
      smallest_index = i
  return smallest_index


def selectionSort(arr):
  newArr = []
  copiedArr = list(arr) # copy array before mutating
  for i in range(len(copiedArr)):
      smallest = findSmallest(copiedArr)
      newArr.append(copiedArr.pop(smallest))
  return newArr


print(selectionSort([5, 3, 6, 2, 10]))
```


### Quicksort

Quicksort is an (average) **O(n log n)** algorithm to sort an array. It
utilizes recursion to divide a list in half and sort the smaller copies.

1. Pick a pivot
2. Partition into two sub-arrays <= and > the pivot
3. Call quicksort on the two sub-arrays
4. Merge the results

**Inductive proofs** are introduced here, which is very similar to recursion in
that there is a *base case* and an *inductive case*.

```py
def quicksort(array):
  if len(array) < 2:
    return array
  else:
    pivot = array[0]
    less = [i for i in array[1:] if i <= pivot]
    greater = [i for i in array[1:] if i > pivot]
    return quicksort(less) + [pivot] + quicksort(greater)


print(quicksort([10, 5, 2, 3]))
```

On average, quicksort is an **O(n log n)** algorithm. In the worst case, it is
**O(n²)** (such as a poorly chosen pivot on a sorted array, or an array with
all of the same elements). The author compares quicksort with mergesort, which
is also an **O(n log n)** algorithm, but it is usually slower because of the
constants (which aren't written in big-O notation).
