# Stacks, Queues, Sorting & Filtering

## Stacks

A stack is a data structure that follows the *Last In, First Out* (LIFO) principle. 

Imagine it like a stack of plates in a restaurant - the last plate you put on the stack is the first one you pick up. Stacks are used in many programming scenarios, like managing function calls, parsing expressions, and undo mechanisms.

The primary things you can do with a stack is put an item onto the stack (a plate), or remove an item from the top of the stack.

These are generally referred to as *push* (add to stack) and *pop* (remove from top of stack). Many languages have these methods available on the data type, but in Go you often have to roll your own...

In Go, you can implement a stack using a slice.

```go
package main

import (
    "fmt"
)

type Stack []int

func (s *Stack) Push(item int) {
    *s = append(*s, item)
}

func (s *Stack) Pop() int {
    if len(*s) == 0 {
        return -1 // or handle error
    }
    item := (*s)[len(*s)-1]
    *s = (*s)[:len(*s)-1]
    return item
}

func main() {
    var s Stack
    s.Push(1)
    s.Push(2)
    s.Push(3)
    
    fmt.Println(s.Pop()) // Output: 3
    fmt.Println(s.Pop()) // Output: 2
}

```

### A Note On Reslicing

Some of the code looks *weird* in the `Pop` method. But let's explain it a bit more. We'll assume the first `if` statement is understandable - it's basically a guard clause to make sure you can't remove an element from an empty slice.

First up this line gets the last element of the slice and assigns it to the `item` variable:

```go
item := (*s)[len(*s)-1]

```

In this line, `(*s)` dereferences the pointer to the stack `(s)` to access the underlying slice. Then `len(*s)` calculates the length of the slice, and `len(*s)-1` calculates the index of the last element in the slice.

Next, we want to update the slice so it no longer contains the item:

```go
*s = (*s)[:len(*s)-1]

```

This creates a new slice that includes all elements of `*s` ***except*** for the last one. By assigning this new slice back to *s, we effectively modify the original slice, effectively "reslicing" it to exclude the last element.


## Queues

A queue is a data structure that follows the *First In, First Out* (FIFO) principle. It's like a queue at the pub - the first person who enters the queue is the first one to get served. Queues are used in applications like scheduling tasks, handling requests, and breadth-first search algorithms.

For queues, we use the terms *enqueue* when we add something to the queue, and *dequeue* when we remove something from it.

```go
package main

import (
    "fmt"
)

type Queue []int

func (q *Queue) Enqueue(item int) {
    *q = append(*q, item)
}

func (q *Queue) Dequeue() int {
    if len(*q) == 0 {
        return -1 // or handle error
    }
    item := (*q)[0]
    *q = (*q)[1:]
    return item
}

func main() {
    var q Queue
    q.Enqueue(1)
    q.Enqueue(2)
    q.Enqueue(3)
    
    fmt.Println(q.Dequeue()) // Output: 1
    fmt.Println(q.Dequeue()) // Output: 2
}

```

You'll see here when we reslice the array, we do it differently to when we use a stack. That's because we want the *first* item that went into the queue (remember FIFO?):

```go
    item := (*q)[0]
    *q = (*q)[1:]
```

We get the first item in the slice, which is always position zero. We then reslice it using `(*q)[1:]` which creates a new slice from index 1 up to and including the last element in the slice.

## Sorting & Filtering

*Sorting* involves arranging data in a particular order, such as numerical or alphabetical. 

*Filtering* is the process of selecting specific elements from a dataset based on certain criteria.

In Go, you can use built-in functions or libraries for sorting use the built-in `sort` package.

Whilst many other languages have builtin filter functions, in Go you must again roll your own. Filters are simply `for` loops with conditional logic (an `if` statement).

Here's an example of using the `sort` package, and a simple `for` loop that filters only the even numbers into a new slice:

```go
package main

import (
    "fmt"
    "sort"
)

func main() {
    numbers := []int{5, 2, 7, 3, 1, 8, 6, 4}

    // Sorting numbers in ascending order
    sort.Ints(numbers)
    fmt.Println("Sorted numbers:", numbers)

    // Filtering out odd numbers
    var filteredNumbers []int
    for _, num := range numbers {
        if num%2 == 0 { // Check if the number is even
            filteredNumbers = append(filteredNumbers, num)
        }
    }
    fmt.Println("Filtered even numbers:", filteredNumbers)
}

```