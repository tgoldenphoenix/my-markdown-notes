# Data structure & Algorithm

## Data Structure

### Array & Linked Lists

Linked Lists are similar to array; store ordered data items. The difference is how they are stored inside the memory.

Array elements must be stored beside each other and form a "contiguous block" in the memory. Linked List have pointer so the elements can be store in any location.

Array's Advantage: fast at reading elements  
Disadvantages: slow when insert & delete elements because of how array are stored inside memory

Linked Lists is the opposite of array. Slower at reading but fast whenn insert/delete.

### Stack and Queue

Queue (hàng đợi) và ngăn xếp

Stack is FILO or LIFO (Last In, First Out)

Applications of Stacks: Function calls, Recursion

Queue is FIFO

Applications of Queye: Task Scheduling

### Set

Set is like array, but no duplicate item. Order does not matter. We care about whether or not an item is presence in the set.

### Binary Search Tree

Binary Tree is a non-linear and **hierarchical** data structure.

Terminnologies:

- An **edge** is a line that connects two nodes, specifically, a parent node to its child node.
- Leaf Node: A node that does not have any children or both children are null.
- Internal Node: A node that has at least one child. This includes all nodes except the leaf nodes.

### HashMaps, Hash Table

HashMaps giống array nhưng thay vì index, nó dùng keys.

HasMaps are un-ordered.

The reason Hash Tables are sometimes preferred instead of arrays or linked lists is because searching for, adding, and deleting data can be done really quickly, even for large amounts of data.

- In a Linked List, finding a person "Bob" takes time because we would have to go from one node to the next, checking each node, until the node with "Bob" is found.
- And finding "Bob" in an Array could be fast if we knew the index, but when we only know the name "Bob", we need to compare each element (like with Linked Lists), and that takes time.
- With a Hash Table however, finding "Bob" is done really fast because there is a way to go directly to where "Bob" is stored, using something called a **hash function**.

A hash function take in a value we want to store and return a **hash code**.

- A **HashSet** is an implementation of a set - a collection of unique keys.
- **HashMap and HashTable** are both implementations of a Map - a collection of key value pairs where the keys are unique (kind of like a lookup table). HashTable does not allow a null key, but HashMap does.

HashMaps = Hash tables = Dictionaries

## Algorithm Terminnologies

### Time Complexity and Space Complexity

Time Complexity: The time complexity of an algorithm quantifies the amount of time taken by an algorithm to run as a function of the length of the input. Note that the time to run is a function of the length of the input and NOT the actual execution time of the machine on which the algorithm is running on.

During analyses, mostly the worst-case scenario is considered. We ignore the lower order terms.

Ignore the constants. A constant is any steps that does not scale with the input to the function.

Big-O is a way to express the *upper bound* of an algorithm’s time or space complexity. Below are the major types of complexities (time and space) in ascending order of growth (Excelent to Horrible):

1. Connstant time: `O(1)`; the time does NOT grow when input increase.
2. Logarithmic: `O(log n)`.
3. Linear time scaling: `O(n)`.
4. Linearithmic `O(n log n)`.
5. Quadratic `O(n^2)`.
6. Cubic `O(n^3)`.
7. Exponential `O(2^n)`.
8. Factorial `O(n!)`.

Where n is the number of input elements (x-axis). The y output values of the function (y-axis) is the operations needed to complete the algorithm.

The base of the logarithm is always taken as 2 in finding time complexity of algorithms. But it's not really matter what the base is. We take the number 2 because (1) it's greater than 1 and (2) it's a typical base number.  
Còn một lý do để lấy base `2` là như trong **merge sort**, mỗi lần divide sẽ chia đôi cái original array thành `N/2`.

### Space complexity

For an algorithm to take **constant extra space**, the extra variables used to solve it should not change with the input size, for example, In **bubble sort**, the extra variable that we will use is the temp variable, it doesn't change with the size of the input array, we will always use the exact number of extra variables for any size.

## Sorting Algorithms

Khi học thì luôn sort **ascending** (biggest number stay on the right - at the end of array).

Khi giải một bài toán algorithm chỉ nên tập trung vào 1 cái trong 2 cái này vào 1 thời điểm:

- What the algo is doing
- How it is doing it?

### Bubble, Selection, Insertion Sort

These 3 algorithms both have time complexity of `O(n^2)`.

**Bubble Sort** is the simplest sorting algorithm. For each iteration, the biggest number bubble to the end of the array.

- Not suitable for large data sets as its average and worst-case time complexity are quite high.
- `i` (counter) starting from 0 chạy `n-1` iterations (còn có 1 item thì không chạy nữa), `j in range(n-i-1)`
- Best case `O(N)` (array is sorted) compare `i` n-1 lần nếu thấy no swap => end program
- Worst case `O(N^2)` (array is sorted in the opposite direction)

The **Selection Sort** algorithm finds (select) the lowest value in an array and moves it to the front of the array.

- 1st step make `n-1` comparison; 2nd step make `n-2`; 3rd makes `n-3` comparison... tới khi make `1` comparison
- Total comparision (for all iteration): `0+1+2+...+(N-1)= \frac{n(n-1)}{2}`. Sau khi simplify thì `O(N^2)` (best & worst cases)

Công thức tính sum of arithmetic series [here](https://en.wikipedia.org/wiki/Arithmetic_progression#Sum).

**Insertion sort** works by continually inserting each element from an unsorted subarray (from the right-hand side) into a sorted sub-array (hence the name “insertion” sort). We partially sort the array from left to right.

- `i` counter run from `[0, N-2]`; `0<j<=(i+1)`. `N` is the length of the array
- Worst case (descending sorted or sorted but opposite direction) `O(N^2)`
- Best case (already sorted) `O(N)`

### Merge Sort

This is a divide-and-conquer algorithm.

1. Dividide the original arr into 2 sub-arrs
2. Sort each sub-arr via **recursion**
3. Merger them together

The helper `merge()` function only merge two sorted arrays.  
Merging two sorted arrays is easy. Chạy từ left -> right, so sánh từng cặp giá trị của 2 arrays.  
Apply the same idea to merge to base-condition arrays (which contain only one element)

Time complexity

- Hình dùng divide theo hình nguyên phân môn sinh học.
- Dùng đại số tính số lần divide (number of iteration) `1=ff{N}{2^k}` => `k = log_2 N`
- For each iteration `k`, there are always `N` element being merged. Tức là có `N` phép so sánh cần thiết để merge those arrays together. Total complexity is: `O(N . log N)`

### Quick Sort

Mình phải chọn pivot. After that, whatever pivot you take, after every iteration:

- All element < pivot go to left side
- Element > pivot go to right side
- After every iteration, you are putting the pivot at the correct position.
- You then use recursion to continue

- The while loop to put the pivot at its correct position is `O(n)`
- `T(N) = T(k) + T(N-k-1) + O(N)`

- Worst case `O(N^2)`: `k=0` which means we take pivot as first or last element. The recursion partition is always un-balance, in this case on `(n-1)` element instead of `n/2` O(n^2). Nhiều lần parition `(n-1)` cuối cùng ra `O(N^2)`
- Average & Best case `O(n log n)`: Occurs when the pivot selection consistently divides the array into roughly equal halves

## Searching Algorithms

### Find duplicate value/number

There is only one repeated number in the array.

### Binary Search

The idea of binary search is to use the information that the array is **already sorted** and reduce the time complexity to O(log n)

## Other algorithms

i

## Recursion

Base case = stopping condition

Pros:

1. Biểu diễn, complex and elegant way algorithm
2. Works well with recursive structures like trees, graphs, JSON objects.

Cons:

1. Slowness due to CPU overhead; can lead to memory errors / stack overflow exceptions

## Resources

[Visualgo](https://visualgo.net/en)
