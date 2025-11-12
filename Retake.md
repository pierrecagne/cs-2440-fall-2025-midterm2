# CS2440 Midterm 2 Retake: Browser Tab Manager

## Introduction

You're building a browser's tab management system that uses a **circular queue with priority levels**. Unlike the midterm's CPU scheduler which managed processes in a circular doubly-linked list, this system manages browser tabs using a different approach: an **array-based circular queue** where tabs can be promoted to "priority" status.

The browser manages two types of tabs:
- **Regular tabs** stored in a circular queue (array-based)
- **Priority tabs** stored in a separate stack (tabs the user has pinned)

Your task is to implement this system using:
- A generic `CircularQueue<T>` class (array-based, not linked list)
- A `BrowserTab` class representing a tab
- A `TabManager` class that coordinates both regular and priority tabs

**Time limit: 1 hour**

---

## Part 1: Array-Based Circular Queue (46 points)

**Introduction to Part 1:**

An array-based circular queue uses a fixed-size array where elements wrap around from the end back to the beginning. You maintain two indices: `front` (where to remove from) and `rear` (where to add to).

Example with capacity 5:
```
Initial: [_, _, _, _, _]  front=0, rear=0, size=0
After adding A,B,C: [A, B, C, _, _]  front=0, rear=3, size=3
After removing A: [_, B, C, _, _]  front=1, rear=3, size=2
After adding D,E,F: [F, B, C, D, E]  front=1, rear=1, size=5 
```

---

**1.** (5 points) Write a generic class `CircularQueue<T>` with:
- Private fields: an array `queue`, an index `front`, an index `rear`, an integer `size`, an integer `capacity`
- A constructor that takes an integer capacity and initializes an empty queue of that capacity
- Methods `isEmpty()` and `isFull()`

---

**2.** (2 points) Implement a method `size()` that returns the current number of elements in the queue.

---

**3.** (6 points) Implement the `enqueue(T item)` method that adds an item to the rear of the queue. If the queue is full, throw an `IllegalStateException` with the message "Queue is full".

---

**4.** (6 points) Implement the `dequeue()` method that removes and returns the item from the front of the queue. If the queue is empty, throw an `IllegalStateException` with the message "Queue is empty".

---

**5.** (4 points) Implement a method `peek()` that returns (but doesn't remove) the front item, or returns `null` if the queue is empty.

---

**6.** (5 points) Trace through these operations and show the state of all fields of the object `q` after **each** operation:

```java
CircularQueue<String> q = new CircularQueue<>(4);
q.enqueue("A");
q.enqueue("B");
q.enqueue("C");
q.dequeue();
q.enqueue("D");
q.enqueue("E");
q.dequeue();
```

---

**7.** (4 points) What is the time complexity of `enqueue()`, `dequeue()`, and `peek()`? Justify your answers.

---

**8.** (3 points) Implement a method `clear()` that reinitialize the queue to an empty one.

---

**9.** (7 points) Make `CircularQueue<T>` iterable. When iterating over the queue, it should return elements from front to rear in order without looping infinitely.

---

## Part 2: Tab Manager with Priority System (29 points)

**Introduction to Part 2:**

Assume the class `BrowserTab` is given, with the following public methods available:
```java
public class BrowserTab {

    // private stuff

    public BrowserTab(int id, String url, String title, int memoryUsage);
    public int getId();
    public String getUrl();
    public String getTitle();
    public int getMemoryUsage();
    public void reduceMemory(int amount);

    //private stuff
}
```



The `TabManager` uses:
- A `CircularQueue<BrowserTab>` for regular tabs (max 10 tabs)
- A `Stack<BrowserTab>` for priority (pinned) tabs

Here's the skeleton:
```java
public class TabManager {
    private CircularQueue<BrowserTab> regularTabs;
    private Stack<BrowserTab> priorityTabs;
    
    public TabManager();
    public void openTab(BrowserTab tab);
    public BrowserTab getCurrentTab();
    public void closeCurrentTab();
    public void pinCurrentTab();
    public void unpinLastPinned();
    public void switchToNextTab();
    public int getTotalMemoryUsage();
}
```

---

**10.** (3 points) Implement the constructor that initializes:
- `regularTabs` as a CircularQueue with capacity 10
- `priorityTabs` as an empty Stack

---

**11.** (3 points) Implement `openTab(BrowserTab tab)` that:
- Tries to add the tab to regularTabs
- If the queue is full, prints "Cannot open tab - too many tabs open"
- Otherwise prints "Opened: " followed by the tab's title

---

**12.** (3 points) Implement `getCurrentTab()` that returns the tab at the front of the regular queue without removing it. Return `null` if no regular tabs exist.

---

**13.** (4 points) Implement `closeCurrentTab()` that:
- Removes the current tab from the regular queue
- Prints "Closed: " followed by the tab's title
- If no tabs exist, prints "No tabs to close"

---

**14.** (5 points) Implement `pinCurrentTab()` that:
- Removes the current tab from the regular queue
- Pushes it onto the priority stack
- Prints "Pinned: " followed by the tab's title
- If no regular tabs exist, prints "No tab to pin"

---

**15.** (4 points) Implement `unpinLastPinned()` that:
- Pops a tab from the priority stack
- Tries to add it back to the regular queue
- If successful, prints "Unpinned: " followed by the tab's title
- If the regular queue is full, prints "Cannot unpin - regular tabs full"
- If no pinned tabs exist, prints "No pinned tabs"

---

**16.** (3 points) Implement `switchToNextTab()` that:
- Removes the front tab from the regular queue
- Adds it back to the rear of the queue (circular rotation)
- Prints "Switched to: " followed by the new current tab's title
- If no tabs or only one tab exists, prints "Cannot switch tabs"

---

**17.** (4 points) Implement `getTotalMemoryUsage()` that returns the total memory usage of all open tabs (both regular and priority). 

_Hint_: To go through the stack of priority tabs without losing them, we recommend using an auxiliary stack to temporarily hold the tabs while summing their memory usage.

---

**18.** (2 points) What is the time complexity of `getTotalMemoryUsage()`, as a function of the total number of tabs? Justify your answer.

---

**Good luck!**
