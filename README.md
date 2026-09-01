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
