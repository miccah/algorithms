# algorithms

Notes on the many algorithm books I recently acquired.

* *Grokking Algorithms* by Aditya Y Bhargava
* *Fabulous Adventures in Data Structures and Algorithms* by Eric Lippert
* *Advanced Algorithms and Data Structures* by Marcello La Rocca
* *Algorithms and Data Structures for Massive Datasets* by Dzejla Medjedovic, Emin Tahirovic, and Ines Dedovic


## Table of Contents

* [Grokking Algorithms](#grokking-algorithms)
    * [Binary Search](#binary-search)
    * [Traveling Salesperson](#traveling-salesperson)
    * [Selection Sort](#selection-sort)
    * [Quicksort](#quicksort)
    * [Hash tables](#hash-tables)
    * [Breadth first search](#breadth-first-search)
    * [Trees](#trees)


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


### Hash tables

The book covers hash tables here and how they are **O(1)** on average for all
types of access. It works by hashing the input to a consistent value for
constant time lookup. It didn't go into too much detail about how they work
under the hood, but the hash is used to index a linked-list, which the item
gets added to.

The *load factor* is the
$\frac{number\ of\ items\ in\ hash}{total\ number\ of\ slots}$ and is used to
resize the backing datastructure (typically when greater than $0.7$).

The book also mentions [cityhash](https://github.com/google/cityhash) as a good
hashing algorithm, although the repository was recently archived. Searching
around revealed [xxHash](https://github.com/Cyan4973/xxHash) and
[MurmurHash](https://en.wikipedia.org/wiki/MurmurHash) among others.


### Breadth first search

This section introduces graphs, which is a collection of nodes and edges. In
the graph `A -> B`, `A` is an **in-neighbor** of `B` and `B` is an
**out-neighbor** of `A`. In an undirected graph, they are simply **neighbors**.

**Breadth first search** is an algorithm that finds if there is a path between
two nodes, and what that shortest path is. It works by searching all the
neighboring nodes before recursively expanding.

Queues are first-in-first-out (FIFO) data structures used in this algorithm to
find the shortest path. Potential candidates are added to the queue, and we
want to check them in the order they are added.

```python
def search(name):
    search_queue = deque()
    search_queue += graph[name]
    searched = set()
    while search_queue:
        person = search_queue.popleft()
        if not person in searched:
            if person_is_seller(person):
                print(person + " is a mango seller!")
                return True
            else:
                search_queue += graph[person]
                searched.add(person)
    return False

search("you")
```

The runtime complexity of this algorithm is **O(V+E)**. It's at least **O(E)**
because we might have to search the entire network (follow all of the edges)
and it's also at least **O(V)** because we enqueue / dequeue a vertex once
each.

If you represent a list of tasks as dependencies in a graph, a topologically
sorted list would be a valid ordering of the tasks. Additionally, a graph with
no cycles is called a tree.


### Trees

Trees are a type of graph with no cycles. More precisely, they are connected
acyclic graphs. There are many types of trees used for lots of different
algorithms.

**Rooted trees** are trees that have one node that leads to all other nodes.
Nodes in a rooted tree can have a parent and children. The only node with no
parent is the *root node*. Nodes with no children are called *leaf nodes*.

File directories are an example of trees (ignoring symlinks). BFS is an
algorithm that traverses every node in a graph, so we can use it for traversing
every file in a directory tree. An important distinction to the previous BFS
algorithm is that we don't need to check if we've seen a node twice because a
tree does not have cycles.

If we change our function to use recursion like so:

```python
from os import listdir
from os.path import isfile, join

def printnames(dir):
    for file in sorted(listdir(dir)):
        fullpath = join(dir, file)
        if isfile(fullpath):
            print(file)
        else:
            printnames(fullpath)

printnames("pics")
```

It prints the files in a different order because this is a **depth-first
search** algorithm. As soon as we find a directory, we recursively process that
directory, where previously we would add it to a queue for later processing.

BFS and DFS are similar algorithms that traverse every node in a graph, however
DFS cannot find the shortest path. One other use for DFS is for finding a
topological sort.

**Binary trees** are a special type of **rooted tree** where nodes can have at
most two children. Traditionally these children are called left child and right
child.

**Huffman coding** is an example of binary trees. It is a compression algorithm
that uses the frequency of letters to determine a variable length binary
representation (so more frequent letters have fewer bits). A binary tree is the
map between letters and bits. Each letter sits at a leaf node. Traversing left
is a `0` and right is a `1`.
