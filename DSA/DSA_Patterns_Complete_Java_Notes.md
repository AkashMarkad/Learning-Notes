# DSA Problem-Solving Patterns --- Complete Java Interview Notes

> A practical reference for recognizing recurring Data Structures and
> Algorithms (DSA) patterns, choosing an approach, implementing it in
> Java, and analyzing complexity.
>
> **Primary reference:** [14 Patterns to Ace Any Coding Interview
> Question ---
> HackerNoon](https://hackernoon.com/14-patterns-to-ace-any-coding-interview-question-c5bb3357f6ed)
> by Fahim ul Haq.\
> **Scope:** The original 14 patterns plus additional patterns widely
> used in coding interviews. The list is a learning framework, not a
> guarantee that every problem fits one pattern.

------------------------------------------------------------------------

## Table of Contents

1.  [How to Use These Notes](#1-how-to-use-these-notes)
2.  [Pattern Recognition Cheat
    Sheet](#2-pattern-recognition-cheat-sheet)
3.  [The Original 14 Patterns](#3-the-original-14-patterns)
    -   Sliding Window
    -   Two Pointers
    -   Fast and Slow Pointers
    -   Merge Intervals
    -   Cyclic Sort
    -   In-Place Linked List Reversal
    -   Tree BFS
    -   Tree DFS
    -   Two Heaps
    -   Subsets / BFS Construction
    -   Modified Binary Search
    -   Top K Elements
    -   K-Way Merge
    -   Topological Sort
4.  [Additional High-Value Patterns](#4-additional-high-value-patterns)
    -   Hashing and Frequency Counting
    -   Prefix Sum and Difference Array
    -   Monotonic Stack and Monotonic Deque
    -   Binary Search on the Answer
    -   Backtracking
    -   Dynamic Programming
    -   Greedy Algorithms
    -   Graph BFS and DFS
    -   Union-Find / Disjoint Set Union
    -   Shortest-Path Algorithms
    -   Minimum Spanning Tree
    -   Trie / Prefix Tree
    -   Sweep Line
    -   Bit Manipulation
    -   Quickselect
    -   Matrix Traversal
    -   Tree Construction and Lowest Common Ancestor
    -   State-Space Search
    -   Divide and Conquer
    -   Stack-Based Expression Processing
    -   Kadane's Algorithm
    -   Meet in the Middle
5.  [Java Templates Library](#5-java-templates-library)
6.  [Choosing Between Similar
    Patterns](#6-choosing-between-similar-patterns)
7.  [Complexity Reference](#7-complexity-reference)
8.  [Common Mistakes](#8-common-mistakes)
9.  [Interview Problem-Solving
    Workflow](#9-interview-problem-solving-workflow)
10. [Suggested Learning Roadmap](#10-suggested-learning-roadmap)
11. [Practice Checklist](#11-practice-checklist)
12. [References](#12-references)

------------------------------------------------------------------------

## 1. How to Use These Notes

A **DSA pattern** is a reusable problem-solving approach. Instead of
memorizing one solution per problem, learn to identify the structure
shared by many problems.

For each problem:

1.  **Identify the input structure:** array, string, linked list, tree,
    graph, intervals, or a stream.
2.  **Identify the goal:** search, count, optimize, enumerate, order,
    connect, or maintain a range.
3.  **Look for constraints:** sorted input, contiguous range, bounded
    values, dependencies, positive numbers, or large input size.
4.  **Choose a candidate pattern.**
5.  **State the invariant:** what must remain true as the algorithm
    runs?
6.  **Choose the data structure** that supports the required operations.
7.  **Analyze time and space complexity.**
8.  **Test edge cases** before considering the solution complete.

### Important principle

A pattern is a hypothesis, not a keyword-matching rule. For example, a
subarray-sum problem may use a sliding window when values are
non-negative, but prefix sums plus a hash map are often needed when
negative values are allowed.

------------------------------------------------------------------------

## 2. Pattern Recognition Cheat Sheet

  -------------------------------------------------------------------------------------
  Problem signal                   First pattern to        Typical use
                                   consider                
  -------------------------------- ----------------------- ----------------------------
  Longest/shortest contiguous      Sliding Window          Maintain a valid range
  substring or subarray                                    

  Sorted array; pair or triplet    Two Pointers            Move from both ends
  target                                                   

  Linked-list cycle or middle node Fast and Slow Pointers  Different traversal speeds

  Overlapping ranges               Merge Intervals / Sweep Combine or process intervals
                                   Line                    

  Values lie in a known range such Cyclic Sort / Index     Find missing or duplicate
  as `1..n`                        Placement               values

  Reverse a linked list or groups  In-Place Reversal       Rewire pointers
  of nodes                                                 

  Level-by-level tree traversal    Tree BFS                Queue-based traversal

  Root-to-leaf paths, tree height, Tree DFS                Preorder/inorder/postorder
  recursive relationships                                  

  Running median                   Two Heaps               Maintain lower and upper
                                                           halves

  Generate every                   Backtracking or Subsets Explore a search space
  subset/permutation/combination                           

  Sorted or rotated sorted input   Binary Search           Eliminate half the search
                                                           space

  K largest/smallest/frequent      Heap / Top K /          Maintain or select
  items                            Quickselect             candidates

  Merge multiple sorted sequences  K-Way Merge             Min-heap of current heads

  Prerequisites or dependency      Topological Sort        Order a directed acyclic
  order                                                    graph

  Fast lookups, duplicates,        Hashing                 HashMap / HashSet
  frequencies                                              

  Many range-sum queries           Prefix Sum              Precompute cumulative totals

  Next greater/smaller element     Monotonic Stack         Maintain ordered candidates

  Feasibility changes              Binary Search on Answer Minimize/maximize a feasible
  monotonically with a value                               value

  All valid arrangements under     Backtracking            Choose, explore, undo
  constraints                                              

  Repeated overlapping subproblems Dynamic Programming     Cache states

  Local choice can be proven       Greedy                  Make a justified local
  globally optimal                                         choice

  Connected components or          Union-Find              Maintain disjoint sets
  connectivity under merges                                

  Unweighted shortest path         BFS                     Explore by distance layers

  Non-negative weighted shortest   Dijkstra                Priority-queue relaxation
  path                                                     

  Negative edges, single-source    Bellman-Ford            Repeated edge relaxation
  shortest path                                            

  Prefix queries over words        Trie                    Traverse characters

  Number of set bits / XOR         Bit Manipulation        Work at the binary level
  uniqueness                                               

  Maximum subarray sum             Kadane's Algorithm      Maintain best ending here

  Matrix islands / regions         Grid BFS or DFS         Model cells as graph nodes
  -------------------------------------------------------------------------------------

------------------------------------------------------------------------

## 3. The Original 14 Patterns

## 3.1 Sliding Window

### Idea

Maintain a contiguous window `[left, right]` while moving through an
array or string. Add the new right element, update window state, and
move `left` when the window violates a condition.

### Recognize it when

-   The question asks about a **contiguous** substring or subarray.
-   You need a longest, shortest, maximum, minimum, or count under a
    window constraint.
-   Recomputing every possible range would be too slow.

### Variants

-   **Fixed-size window:** window length is exactly `k`.
-   **Variable-size window:** expand and shrink according to a
    condition.
-   **Window with frequency map:** track character/number counts.
-   **Monotonic deque window:** track a window maximum or minimum.

### Java template: fixed-size sum

``` java
int windowSum = 0;
int best = Integer.MIN_VALUE;

for (int right = 0; right < nums.length; right++) {
    windowSum += nums[right];

    if (right >= k) {
        windowSum -= nums[right - k];
    }

    if (right >= k - 1) {
        best = Math.max(best, windowSum);
    }
}
```

Time: `O(n)`. Extra space: `O(1)`.

### Java template: variable-size window

``` java
int left = 0;
int best = 0;

for (int right = 0; right < nums.length; right++) {
    // Add nums[right] to the window state.

    while (left <= right && /* window is invalid */) {
        // Remove nums[left] from the window state.
        left++;
    }

    // Update the answer using the valid window [left, right].
}
```

### Example problems

-   Maximum sum subarray of size `k`
-   Longest substring without repeating characters
-   Minimum size subarray sum
-   Permutation in a string
-   Minimum window substring

### Pitfalls

-   Sliding-window shrinking is not automatically valid when negative
    values make the condition non-monotonic.
-   Be precise about whether the window is valid before or after
    shrinking.
-   For frequency maps, remove keys whose count becomes zero when that
    simplifies equality checks.

------------------------------------------------------------------------

## 3.2 Two Pointers

### Idea

Use two indices that move through a sequence. They may move toward each
other, move in the same direction, or maintain a fast/slow relationship.

### Recognize it when

-   Input is sorted and you need a pair or triplet.
-   You need to remove duplicates in-place.
-   You need to compare characters from both ends.
-   You need to partition elements around a condition.

### Java template: pair sum in a sorted array

``` java
int left = 0;
int right = nums.length - 1;

while (left < right) {
    long sum = (long) nums[left] + nums[right];

    if (sum == target) {
        // Found a pair.
        break;
    } else if (sum < target) {
        left++;
    } else {
        right--;
    }
}
```

Time: `O(n)`. Extra space: `O(1)`.

### Common variants

-   Opposite-direction pointers: two-sum in sorted input, container with
    most water.
-   Same-direction pointers: remove duplicates, move zeroes.
-   Partition pointers: sort colors, quicksort partition.
-   Three-sum: sort, then use two pointers for each fixed element.

### Pitfalls

-   The opposite-direction method relies on sorted order or another
    valid monotonic property.
-   Skip duplicate values when a problem requires unique pairs/triplets.
-   Use a wider numeric type if adding two integers can overflow.

------------------------------------------------------------------------

## 3.3 Fast and Slow Pointers

### Idea

Move one pointer faster than another. Their relative speeds reveal cycle
structure or positional relationships.

### Recognize it when

-   Detecting a linked-list cycle.
-   Finding the middle of a linked list.
-   Finding the start of a cycle.
-   Determining whether a number sequence eventually repeats, as in
    Happy Number.

### Java template: cycle detection

``` java
ListNode slow = head;
ListNode fast = head;

while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;

    if (slow == fast) {
        // A cycle exists.
        break;
    }
}
```

Time: `O(n)`. Extra space: `O(1)`.

### Find the middle

``` java
ListNode slow = head;
ListNode fast = head;

while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}

// slow is at the middle (for an even-length list, the second middle).
```

### Pitfalls

-   Check `fast != null` and `fast.next != null` before moving two
    steps.
-   Pointer identity (`slow == fast`) matters; comparing node values is
    not equivalent.
-   The exact middle for even-length lists depends on the loop
    condition.

------------------------------------------------------------------------

## 3.4 Merge Intervals

### Idea

Sort intervals by start time, then process them from left to right.
Merge an interval with the last output interval when their ranges
overlap.

### Recognize it when

-   You need to merge overlapping ranges.
-   You need to insert an interval into a set of ranges.
-   You need to find interval intersections or remove conflicts.
-   Time ranges or resource schedules are involved.

### Java template

``` java
Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));

List<int[]> merged = new ArrayList<>();

for (int[] interval : intervals) {
    if (merged.isEmpty()
            || merged.get(merged.size() - 1)[1] < interval[0]) {
        merged.add(new int[]{interval[0], interval[1]});
    } else {
        int[] last = merged.get(merged.size() - 1);
        last[1] = Math.max(last[1], interval[1]);
    }
}
```

Time: `O(n log n)` due to sorting. Extra output space: `O(n)`.

### Boundary detail

The example treats `[1, 3]` and `[3, 5]` as overlapping because the
non-overlap condition is `lastEnd < nextStart`. For half-open intervals
such as `[1, 3)` and `[3, 5)`, touching boundaries may not overlap.
Match the problem's interval convention.

### Example problems

-   Merge Intervals
-   Insert Interval
-   Non-overlapping Intervals
-   Meeting Rooms II

------------------------------------------------------------------------

## 3.5 Cyclic Sort / Index Placement

### Idea

When values belong to a constrained range and each value has a natural
target index, place each value at its correct index using swaps.

### Recognize it when

-   The array contains numbers in `1..n` or `0..n`.
-   The task asks for missing, duplicate, or smallest missing positive
    values.
-   The expected solution is in-place and near-linear.

### Java template: values in `1..n`

``` java
int i = 0;

while (i < nums.length) {
    int correct = nums[i] - 1;

    if (nums[i] >= 1
            && nums[i] <= nums.length
            && nums[i] != nums[correct]) {
        int temp = nums[i];
        nums[i] = nums[correct];
        nums[correct] = temp;
    } else {
        i++;
    }
}
```

After placement, scan for an index `i` where `nums[i] != i + 1`.

Time: `O(n)` under the standard bounded-range setup. Extra space:
`O(1)`.

### Pitfalls

-   Duplicates can cause infinite loops if you swap without checking
    `nums[i] != nums[correct]`.
-   Validate the range before indexing.
-   This pattern is not appropriate for arbitrary values with no natural
    index mapping.

------------------------------------------------------------------------

## 3.6 In-Place Linked List Reversal

### Idea

Reverse links one node at a time using `previous`, `current`, and
`next`.

### Java template

``` java
ListNode previous = null;
ListNode current = head;

while (current != null) {
    ListNode next = current.next;
    current.next = previous;
    previous = current;
    current = next;
}

return previous;
```

Time: `O(n)`. Extra space: `O(1)`.

### Extensions

-   Reverse a sublist.
-   Reverse nodes in groups of `k`.
-   Reverse the first `k` nodes.
-   Reverse alternating groups.
-   Reverse a linked list between positions `left` and `right`.

### Pitfalls

-   Save `current.next` before overwriting it.
-   The new head is `previous`, not the original head.
-   Carefully reconnect the reversed segment to the unreversed list.

------------------------------------------------------------------------

## 3.7 Tree BFS (Level-Order Traversal)

### Idea

Use a queue to visit nodes level by level.

### Recognize it when

-   The question asks for level order, level averages, minimum depth, or
    nearest node.
-   Nodes at the same distance from the root must be processed together.
-   You need shortest path in an unweighted tree/graph.

### Java template

``` java
Queue<TreeNode> queue = new ArrayDeque<>();
List<List<Integer>> result = new ArrayList<>();

if (root != null) {
    queue.offer(root);
}

while (!queue.isEmpty()) {
    int levelSize = queue.size();
    List<Integer> level = new ArrayList<>();

    for (int i = 0; i < levelSize; i++) {
        TreeNode node = queue.poll();
        level.add(node.val);

        if (node.left != null) queue.offer(node.left);
        if (node.right != null) queue.offer(node.right);
    }

    result.add(level);
}
```

Time: `O(n)`. Space: `O(w)`, where `w` is the maximum tree width; output
storage is additional.

### Pitfalls

-   Capture `queue.size()` before processing a level.
-   Use `ArrayDeque` rather than legacy `Stack` or `LinkedList` when a
    standard queue/deque suffices.
-   Handle a null root.

------------------------------------------------------------------------

## 3.8 Tree DFS

### Idea

Explore a branch before moving to another. DFS is commonly implemented
recursively or with an explicit stack.

### Traversal orders

-   **Preorder:** node, left, right.
-   **Inorder:** left, node, right. In a binary search tree, this visits
    values in sorted order.
-   **Postorder:** left, right, node. Useful when child results are
    needed before processing a parent.

### Recursive template

``` java
void dfs(TreeNode node) {
    if (node == null) return;

    // Preorder work.
    dfs(node.left);
    // Inorder work.
    dfs(node.right);
    // Postorder work.
}
```

Time: `O(n)`. Auxiliary space: `O(h)` for recursion, where `h` is tree
height; worst case `O(n)`.

### Typical uses

-   Tree height and diameter
-   Root-to-leaf paths
-   Validate a BST
-   Lowest common ancestor
-   Tree path sums
-   Subtree properties

### Pitfalls

-   Deep skewed trees can cause Java stack overflow.
-   Distinguish global state (e.g., a maximum) from values returned by
    recursive calls.
-   Decide whether each calculation belongs before, between, or after
    recursive calls.

------------------------------------------------------------------------

## 3.9 Two Heaps

### Idea

Maintain two heaps to keep a collection split around a boundary. The
classic example is a running median.

-   Max-heap: lower half.
-   Min-heap: upper half.
-   Keep sizes balanced and ensure every lower-half value is no larger
    than every upper-half value.

### Java template: running median structure

``` java
PriorityQueue<Integer> lower =
        new PriorityQueue<>(Collections.reverseOrder());
PriorityQueue<Integer> upper = new PriorityQueue<>();

void addNumber(int x) {
    if (lower.isEmpty() || x <= lower.peek()) {
        lower.offer(x);
    } else {
        upper.offer(x);
    }

    if (lower.size() > upper.size() + 1) {
        upper.offer(lower.poll());
    } else if (upper.size() > lower.size()) {
        lower.offer(upper.poll());
    }
}

double median() {
    if (lower.size() > upper.size()) {
        return lower.peek();
    }
    return ((long) lower.peek() + upper.peek()) / 2.0;
}
```

Insertion: `O(log n)`. Median lookup: `O(1)`.

### Other uses

-   Scheduling tasks around a median.
-   Splitting values into lower and upper halves.
-   Some problems involving the smallest and largest available values.

### Pitfalls

-   Preserve both size balance and ordering across heaps.
-   Handle an empty structure before calling `peek()`.
-   Cast before adding two integers to avoid overflow.

------------------------------------------------------------------------

## 3.10 Subsets / BFS Construction

### Idea

Build subsets incrementally. For each new element, extend the subsets
already constructed.

### Iterative subset template

``` java
List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    result.add(new ArrayList<>());

    for (int value : nums) {
        int size = result.size();

        for (int i = 0; i < size; i++) {
            List<Integer> next = new ArrayList<>(result.get(i));
            next.add(value);
            result.add(next);
        }
    }

    return result;
}
```

There are `2^n` subsets; materializing them takes `O(n * 2^n)` total
time and output space.

### Recognize it when

-   You need all subsets or combinations.
-   Each input element can be included or excluded.
-   A solution can be constructed by expanding partial solutions.

### Related techniques

-   Backtracking with a start index.
-   Bitmask enumeration for small `n`.
-   Permutations, where order matters.
-   Combination Sum, where elements may be reused depending on the
    rules.

------------------------------------------------------------------------

## 3.11 Modified Binary Search

### Idea

Use sorted or monotonic structure to eliminate half the remaining search
space each step.

### Standard Java template

``` java
int binarySearch(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;

    while (left <= right) {
        int mid = left + (right - left) / 2;

        if (nums[mid] == target) return mid;
        if (nums[mid] < target) left = mid + 1;
        else right = mid - 1;
    }

    return -1;
}
```

Time: `O(log n)`. Extra space: `O(1)`.

### Variants

-   First occurrence / lower bound.
-   Last occurrence / upper bound.
-   Search in a rotated sorted array.
-   Find a peak.
-   Search a 2D matrix.
-   Search an unbounded or conceptually infinite sorted array.

### Pitfalls

-   Be consistent about inclusive versus exclusive boundaries.
-   `left + (right - left) / 2` avoids overflow from
    `(left + right) / 2`.
-   Duplicate values can change how rotated-array search handles
    ambiguous halves.

------------------------------------------------------------------------

## 3.12 Top K Elements

### Idea

Use a heap to maintain the best `k` candidates without sorting the
entire input.

### Example: K largest values

Maintain a min-heap of at most `k` elements. The heap root is the
smallest of the retained largest values.

``` java
PriorityQueue<Integer> heap = new PriorityQueue<>();

for (int value : nums) {
    heap.offer(value);

    if (heap.size() > k) {
        heap.poll();
    }
}
```

Time: `O(n log k)`. Extra space: `O(k)`.

### Recognize it when

-   The question asks for the `k` largest, smallest, closest, or most
    frequent.
-   You need a running top K as data arrives.
-   Sorting everything is unnecessary.

### Alternatives

-   Full sorting: `O(n log n)`, often simplest.
-   Quickselect: average `O(n)` for a single order statistic.
-   Bucket/frequency methods: useful when the value or frequency range
    is bounded.

### Pitfalls

-   A min-heap is appropriate for retaining the `k` largest values; a
    max-heap is commonly used for retaining the `k` smallest.
-   Clarify how ties should be handled.
-   For most-frequent elements, heap entries usually contain both value
    and frequency.

------------------------------------------------------------------------

## 3.13 K-Way Merge

### Idea

Merge `k` sorted sequences by keeping the current smallest candidate
from each sequence in a min-heap.

### Algorithm

1.  Insert the first element of each non-empty sequence into the heap.
2.  Remove the smallest item and append it to the output.
3.  Insert the next item from the same sequence.
4.  Repeat until the heap is empty.

Time for `N` total elements across `k` sequences: `O(N log k)`. Heap
space: `O(k)`.

### Recognize it when

-   Several sorted arrays or lists must be merged.
-   You need the smallest next item among multiple streams.
-   Each input sequence is already sorted.

### Common problems

-   Merge K Sorted Lists
-   Smallest Range Covering Elements from K Lists
-   K Pairs with Smallest Sums

------------------------------------------------------------------------

## 3.14 Topological Sort

### Idea

Order the vertices of a directed acyclic graph (DAG) so every
prerequisite appears before the dependent vertex.

### Kahn's algorithm (BFS)

1.  Build an adjacency list.
2.  Compute indegree for every vertex.
3.  Add all zero-indegree vertices to a queue.
4.  Remove a vertex, append it to the order, and decrement its
    neighbors' indegrees.
5.  If a neighbor reaches indegree zero, enqueue it.
6.  If fewer than `V` vertices are processed, the graph contains a
    directed cycle.

### Java template

``` java
int[] indegree = new int[n];
List<List<Integer>> graph = new ArrayList<>();

for (int i = 0; i < n; i++) {
    graph.add(new ArrayList<>());
}

for (int[] edge : edges) {
    int from = edge[0];
    int to = edge[1];
    graph.get(from).add(to);
    indegree[to]++;
}

Queue<Integer> queue = new ArrayDeque<>();
for (int i = 0; i < n; i++) {
    if (indegree[i] == 0) queue.offer(i);
}

List<Integer> order = new ArrayList<>();

while (!queue.isEmpty()) {
    int node = queue.poll();
    order.add(node);

    for (int next : graph.get(node)) {
        if (--indegree[next] == 0) queue.offer(next);
    }
}

boolean hasCycle = order.size() != n;
```

Time: `O(V + E)`. Space: `O(V + E)` for graph and bookkeeping.

### Recognize it when

-   Tasks have prerequisites.
-   Courses or jobs have dependencies.
-   You need a valid order in a DAG.

------------------------------------------------------------------------

## 4. Additional High-Value Patterns

## 4.1 Hashing and Frequency Counting

### Idea

Use `HashMap` or `HashSet` to make lookups, counting, and membership
checks efficient on average.

### Recognize it when

-   Detecting duplicates.
-   Counting frequencies.
-   Finding complements, such as Two Sum.
-   Grouping values by a canonical key.
-   Tracking seen states or visited values.

### Java template: frequency map

``` java
Map<Integer, Integer> frequency = new HashMap<>();

for (int value : nums) {
    frequency.put(value, frequency.getOrDefault(value, 0) + 1);
}
```

Typical time: `O(n)` average. Space: `O(n)`.

### Pitfalls

-   HashMap operations are average-case `O(1)`, not an unconditional
    worst-case guarantee.
-   A hash set tells you membership, not sorted order.
-   Define carefully whether the current element may pair with itself.

------------------------------------------------------------------------

## 4.2 Prefix Sum and Difference Array

### Prefix sum idea

Precompute cumulative totals so range-sum queries can be answered
quickly.

For an array `a`, define `prefix[0] = 0` and
`prefix[i + 1] = prefix[i] + a[i]`. Then the sum of inclusive range
`[left, right]` is:

`prefix[right + 1] - prefix[left]`.

``` java
long[] prefix = new long[nums.length + 1];

for (int i = 0; i < nums.length; i++) {
    prefix[i + 1] = prefix[i] + nums[i];
}

long rangeSum(int left, int right) {
    return prefix[right + 1] - prefix[left];
}
```

Preprocessing: `O(n)`. Each range query: `O(1)`. Space: `O(n)`.

### Prefix sum + HashMap

Useful for counting subarrays with sum `k`, including arrays containing
negative values.

``` java
Map<Long, Integer> count = new HashMap<>();
count.put(0L, 1);

long prefix = 0;
int answer = 0;

for (int value : nums) {
    prefix += value;
    answer += count.getOrDefault(prefix - k, 0);
    count.put(prefix, count.getOrDefault(prefix, 0) + 1);
}
```

Time: `O(n)` average. Space: `O(n)`. Use a wider type when prefix totals
can exceed `int`.

### Difference array

For many range updates, mark the start and end+1 boundaries, then
compute a prefix sum over the difference array to recover final values.
This can turn each range update into `O(1)` work after setup.

### Pitfalls

-   Prefix sums work for negative values; the usual sum-based
    sliding-window shrink rule often does not.
-   Confirm whether the right endpoint is inclusive.
-   Use `long` when cumulative sums may overflow `int`.

------------------------------------------------------------------------

## 4.3 Monotonic Stack and Monotonic Deque

### Idea

Maintain elements in increasing or decreasing order while removing
candidates that can no longer be answers.

### Recognize it when

-   Next greater/smaller element.
-   Previous greater/smaller element.
-   Daily temperatures or stock span.
-   Largest rectangle in a histogram.
-   Sliding-window maximum/minimum (usually a deque).

### Next greater element template

``` java
int n = nums.length;
int[] nextGreater = new int[n];
Arrays.fill(nextGreater, -1);

Deque<Integer> stack = new ArrayDeque<>(); // indices

for (int i = 0; i < n; i++) {
    while (!stack.isEmpty() && nums[i] > nums[stack.peek()]) {
        nextGreater[stack.pop()] = nums[i];
    }
    stack.push(i);
}
```

Time: `O(n)` because each index is pushed and popped at most once.
Space: `O(n)`.

### Deque for sliding-window maximum

Store indices in decreasing order of their values. Remove indices that
leave the window from the front, and smaller values from the back.

### Pitfalls

-   Decide whether equal values should be popped; use strict or
    non-strict comparisons according to the problem.
-   Store indices when distance or expiration matters.
-   A monotonic stack is not a normal stack of all values; it
    intentionally discards dominated candidates.

------------------------------------------------------------------------

## 4.4 Binary Search on the Answer

### Idea

Instead of searching for an element, binary-search a candidate answer
when feasibility is monotonic.

Example structure: if capacity `x` is sufficient, every capacity greater
than `x` is also sufficient. Find the smallest feasible capacity.

### Recognize it when

-   Asked to minimize the maximum or maximize the minimum.
-   You can write `canAchieve(candidate)`.
-   The feasibility predicate changes from false to true, or true to
    false, only once.

### Template: first feasible value

``` java
int left = low;
int right = high;

while (left < right) {
    int mid = left + (right - left) / 2;

    if (canAchieve(mid)) {
        right = mid;
    } else {
        left = mid + 1;
    }
}

return left;
```

Time: `O(log(range) * costOfCheck)`. Space depends on the feasibility
function.

### Example problems

-   Koko Eating Bananas
-   Capacity to Ship Packages Within D Days
-   Split Array Largest Sum
-   Minimum Days to Make Bouquets

### Pitfalls

-   Prove monotonicity before using this technique.
-   Ensure `low` and `high` contain at least one feasible answer.
-   Use `long` when midpoint arithmetic, sums, or search bounds may
    overflow.

------------------------------------------------------------------------

## 4.5 Backtracking

### Idea

Build a candidate incrementally. Choose an option, recurse, and undo the
choice when returning.

### Recognize it when

-   Enumerating permutations or combinations.
-   Generating valid parentheses.
-   Solving Sudoku or N-Queens.
-   Partitioning a string.
-   Finding all paths or all valid configurations.

### Template

``` java
void backtrack(/* state */) {
    if (/* complete solution */) {
        // Record a copy of the solution.
        return;
    }

    for (/* each available choice */) {
        if (/* choice is valid */) {
            // Choose.
            // Update state.

            backtrack(/* updated state */);

            // Undo state changes.
            // Unchoose.
        }
    }
}
```

Complexity is problem-dependent and often exponential. For `n` distinct
items, permutations number `n!`; subsets number `2^n`.

### Optimization techniques

-   Prune invalid partial candidates early.
-   Sort input to make duplicate skipping possible.
-   Use a `visited[]` array for permutations.
-   Use a start index for combinations where order should not matter.
-   Record a copy of mutable paths rather than storing the same changing
    list repeatedly.

### Pitfalls

-   Forgetting to undo state changes.
-   Adding duplicate results.
-   Confusing combinations (order irrelevant) with permutations (order
    matters).

------------------------------------------------------------------------

## 4.6 Dynamic Programming (DP)

### Idea

Use DP when a problem has **overlapping subproblems** and **optimal
substructure**. Define a state, a transition, base cases, and an
evaluation order.

### A reliable DP design process

1.  Define what `dp[state]` means in one sentence.
2.  Identify the choices available from that state.
3.  Write the recurrence.
4.  Establish base cases.
5.  Decide memoization (top-down) or tabulation (bottom-up).
6.  Determine the dependency order.
7.  Reduce memory only after correctness is clear.

### Common DP families

-   **1D DP:** Climbing Stairs, House Robber.
-   **Grid DP:** Unique Paths, Minimum Path Sum.
-   **Knapsack:** 0/1 knapsack, subset sum, partition equal subset sum.
-   **String DP:** Longest Common Subsequence, Edit Distance.
-   **Subsequence DP:** LIS and variants.
-   **Interval DP:** Matrix Chain Multiplication, Burst Balloons.
-   **Tree DP:** independent set-like tree problems.
-   **Bitmask DP:** small `n`, states represented by subsets.
-   **State-machine DP:** stock trading and cooldown constraints.

### Java memoization shape

``` java
int solve(int state, int[] memo) {
    if (/* base case */) return /* base value */;
    if (memo[state] != -1) return memo[state];

    int result = /* recurrence using smaller states */;
    memo[state] = result;
    return result;
}
```

### Complexity

Usually `number of states × work per state`. For example, `O(n)` states
with `O(1)` work each gives `O(n)` time.

### Pitfalls

-   State definition is vague or incomplete.
-   A transition misses a valid choice.
-   Base cases are incorrect.
-   Memoization does not include all variables that affect the answer.
-   Space optimization overwrites a value before it is used.
-   Recursive depth can be too large.

------------------------------------------------------------------------

## 4.7 Greedy Algorithms

### Idea

Make a locally optimal choice at each step. A greedy solution is correct
only when the choice can be justified globally.

### Recognize it when

-   Sorting by end time enables selecting the most compatible intervals.
-   You need a locally optimal next step with a proof.
-   The problem has exchange properties or a cut-like argument.
-   Some scheduling and resource-allocation problems are involved.

### Common examples

-   Activity/interval selection
-   Jump Game
-   Gas Station (with the right invariant)
-   Huffman coding
-   Fractional Knapsack
-   Minimum number of platforms (with sorted arrival/departure times)

### How to validate a greedy idea

-   State the greedy choice explicitly.
-   Try small counterexamples.
-   Explain why changing the choice cannot improve the optimum, often
    through an exchange argument.
-   Do not assume that a locally best choice is always globally optimal.

### Greedy vs DP

Greedy commits to a choice that can be proven safe. DP compares
alternatives across states. If an early choice can block a better later
solution and no safe-choice proof exists, consider DP or search.

------------------------------------------------------------------------

## 4.8 Graph BFS and DFS

### Graph representation

Adjacency list is usually preferred for sparse graphs.

``` java
List<List<Integer>> graph = new ArrayList<>();
for (int i = 0; i < n; i++) {
    graph.add(new ArrayList<>());
}
```

### BFS

Use a queue for shortest path in an **unweighted** graph and for
level-by-level exploration.

``` java
int[] distance = new int[n];
Arrays.fill(distance, -1);

Queue<Integer> queue = new ArrayDeque<>();
distance[source] = 0;
queue.offer(source);

while (!queue.isEmpty()) {
    int node = queue.poll();

    for (int next : graph.get(node)) {
        if (distance[next] == -1) {
            distance[next] = distance[node] + 1;
            queue.offer(next);
        }
    }
}
```

Time: `O(V + E)`. Space: `O(V)` excluding graph storage.

### DFS

Useful for reachability, connected components, cycle detection, and
exploring all reachable nodes. Track visited state to avoid repeatedly
processing vertices.

### Pitfalls

-   Mark nodes visited when enqueuing in BFS to avoid duplicate queue
    entries.
-   For disconnected graphs, start a traversal from each unvisited
    vertex if all components must be processed.
-   Directed and undirected cycle detection require different logic.
-   For grids, check row/column bounds before accessing a cell.

------------------------------------------------------------------------

## 4.9 Union-Find / Disjoint Set Union (DSU)

### Idea

Maintain groups under merge operations. `find(x)` returns the
representative of a component; `union(a, b)` merges components.

### Java template

``` java
class DSU {
    private final int[] parent;
    private final int[] size;

    DSU(int n) {
        parent = new int[n];
        size = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            size[i] = 1;
        }
    }

    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // path compression
        }
        return parent[x];
    }

    boolean union(int a, int b) {
        int rootA = find(a);
        int rootB = find(b);

        if (rootA == rootB) return false;

        if (size[rootA] < size[rootB]) {
            int temp = rootA;
            rootA = rootB;
            rootB = temp;
        }

        parent[rootB] = rootA;
        size[rootA] += size[rootB];
        return true;
    }
}
```

With path compression and union by size/rank, operations take amortized
`O(α(n))`, effectively constant for practical input sizes.

### Recognize it when

-   Dynamic connectivity or component merging.
-   Detecting redundant edges in an undirected graph.
-   Kruskal's minimum spanning tree.
-   Grouping equivalent items.

------------------------------------------------------------------------

## 4.10 Shortest-Path Algorithms

Choose the algorithm based on edge weights and graph properties.

  ------------------------------------------------------------------------
  Algorithm             Suitable situation                    Typical time
  --------------------- --------------------- ----------------------------
  BFS                   Unweighted graph /                      `O(V + E)`
                        equal edge costs      

  Dijkstra              Non-negative edge        `O((V + E) log V)` with a
                        weights                    heap and adjacency list

  Bellman-Ford          Negative edges;                            `O(VE)`
                        detect reachable      
                        negative cycles       

  Floyd-Warshall        All-pairs shortest                        `O(V^3)`
                        paths, usually        
                        smaller dense graphs  
  ------------------------------------------------------------------------

### Dijkstra outline

1.  Set source distance to zero and others to infinity.
2.  Push `(distance, node)` into a min-priority queue.
3.  Pop the smallest distance.
4.  Ignore stale queue entries.
5.  Relax outgoing edges.
6.  Stop when the queue is empty or a target is finalized.

**Important:** Standard Dijkstra is not valid with negative-weight
edges.

------------------------------------------------------------------------

## 4.11 Minimum Spanning Tree (MST)

### Goal

Connect all vertices in a connected, undirected, weighted graph with
minimum total edge weight and no cycles.

### Kruskal's algorithm

1.  Sort edges by weight.
2.  For each edge, use DSU to check whether its endpoints are already
    connected.
3.  Add the edge if it connects two components.
4.  Stop after selecting `V - 1` edges for a connected graph.

Time: `O(E log E)` primarily due to sorting.

### Prim's algorithm

Grow a tree from a start vertex by repeatedly selecting the lightest
edge connecting the current tree to an unvisited vertex. A priority
queue is commonly used.

### Pitfalls

-   MST applies to undirected weighted graphs.
-   A disconnected graph produces a minimum spanning forest rather than
    one spanning tree.
-   Do not confuse MST with shortest paths from a source; they optimize
    different objectives.

------------------------------------------------------------------------

## 4.12 Trie / Prefix Tree

### Idea

Store strings by character path, making prefix operations efficient.

### Recognize it when

-   Prefix search or autocomplete.
-   Word dictionary with many queries.
-   Word search with a dictionary.
-   Counting words sharing prefixes.

### Java node shape

``` java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isWord;
}
```

For lowercase English letters, insert/search takes `O(L)` time for a
word of length `L`, assuming constant-time child access. Space depends
on the number of created trie nodes.

### Pitfalls

-   The 26-child representation only supports lowercase English letters
    as written.
-   Use a map for broader character sets or sparse alphabets.
-   Distinguish a complete stored word from a prefix.

------------------------------------------------------------------------

## 4.13 Sweep Line

### Idea

Sort events by coordinate/time and maintain active state as the sweep
moves from left to right.

### Recognize it when

-   Counting simultaneous meetings or intervals.
-   Finding maximum overlap.
-   Processing geometric events.
-   Comparing arrival/departure or start/end times.

### Typical approach

Represent each interval with start/end events, sort them, then update an
active count or ordered set. When start and end coordinates tie, the
event ordering must match the interval's boundary semantics.

Time is often `O(n log n)` due to sorting.

### Related patterns

-   Merge Intervals combines ranges.
-   Sweep Line processes all event boundaries.
-   Difference Array handles discrete range updates efficiently.

------------------------------------------------------------------------

## 4.14 Bit Manipulation

### Useful operations

``` java
boolean isSet = (x & (1 << bit)) != 0;
int setBit = x | (1 << bit);
int clearBit = x & ~(1 << bit);
int toggleBit = x ^ (1 << bit);
boolean isPowerOfTwo = x > 0 && (x & (x - 1)) == 0;
```

### Recognize it when

-   Numbers are represented as bit masks.
-   You need to test/set/toggle flags.
-   XOR can isolate a unique element.
-   A small set of options can be encoded as a bitmask.

### Important Java details

-   `int` has 32 bits; `long` has 64 bits.
-   `1 << bit` uses an `int`; use `1L << bit` when working with `long`.
-   Bitwise behavior for negative numbers follows two's-complement
    representation.
-   Avoid assuming bit tricks are clearer or safer than straightforward
    code.

------------------------------------------------------------------------

## 4.15 Quickselect

### Idea

Partition an array so the element at a target rank is placed correctly,
without fully sorting the array.

### Recognize it when

-   Find the kth smallest/largest element.
-   Find a median or percentile from an unsorted array.
-   Only one order statistic is required.

Average time: `O(n)`. Worst-case time: `O(n^2)` for a poor pivot
strategy. In-place implementations often use `O(1)` auxiliary space
aside from recursion or iterative bookkeeping.

### Quickselect vs heap

-   Quickselect is attractive for one rank query on an array.
-   A heap is often better for streaming data or maintaining top K
    dynamically.
-   Sorting is simpler if the full order is needed.

### Pitfalls

-   Be precise whether `k` is one-based or zero-based.
-   Partition logic must match the chosen pivot scheme.
-   Randomized pivots reduce the risk of consistently bad partitions but
    do not change the worst-case theoretical bound.

------------------------------------------------------------------------

## 4.16 Matrix / Grid Traversal

### Idea

Treat each cell as a graph node connected to valid neighboring cells.

### Recognize it when

-   Number of islands.
-   Flood fill.
-   Shortest path through a grid.
-   Connected regions or surrounded regions.
-   Rotting oranges or multi-source spread.

### Checklist

-   Define four-directional or eight-directional movement.
-   Validate row and column bounds.
-   Mark visited cells at the right time.
-   Use BFS for unweighted shortest paths; DFS/BFS for component
    exploration.
-   Use multi-source BFS when all initial sources spread simultaneously.

Time is typically `O(rows * cols)` when each cell is processed a
constant number of times.

------------------------------------------------------------------------

## 4.17 Tree Construction and Lowest Common Ancestor

### Construction

-   Preorder + inorder (with unique values) can reconstruct a binary
    tree.
-   Inorder + postorder can reconstruct a binary tree.
-   Preorder + postorder alone does not generally uniquely determine an
    arbitrary binary tree.

Use a map from value to inorder index to avoid repeatedly scanning the
inorder array. Typical time: `O(n)` with the map.

### Lowest Common Ancestor (LCA)

For a general binary tree, recurse into left and right subtrees. If each
side returns a target, the current node is the LCA. For a BST, ordering
can guide the search.

### Pitfalls

-   Reconstruction assumptions (unique values and traversal pairing)
    matter.
-   Clarify whether both target nodes are guaranteed to exist.
-   A BST-specific LCA strategy is not valid for an arbitrary binary
    tree.

------------------------------------------------------------------------

## 4.18 State-Space Search

### Idea

Model each possible configuration as a state and each allowed move as an
edge. Search the state graph using BFS, DFS, Dijkstra, or DP.

### Recognize it when

-   Minimum moves to transform one state into another.
-   Lock/key puzzles, word ladder, board games, or operations on a
    number.
-   The same configuration can be reached by multiple paths.

### Design checklist

1.  Define a canonical representation for each state.
2.  Define transitions.
3.  Define goal condition.
4.  Track visited states or best-known costs.
5.  Choose BFS for equal-cost moves, Dijkstra for non-negative varying
    costs, or DFS when exhaustive reachability is the goal.

------------------------------------------------------------------------

## 4.19 Divide and Conquer

### Idea

Split a problem into smaller independent subproblems, solve each, and
combine their results.

### Examples

-   Merge Sort
-   Binary Search
-   Quick Sort (partition-based)
-   Maximum subarray using a divide-and-conquer formulation
-   Many recursive tree algorithms

### Analysis

Use a recurrence such as `T(n) = 2T(n/2) + O(n)` for Merge Sort, which
yields `O(n log n)` time. Recursion depth and temporary storage depend
on the algorithm.

### Pitfalls

-   Ensure subproblems become strictly smaller.
-   Combine results correctly.
-   Distinguish divide-and-conquer from DP: DP is especially useful when
    subproblems overlap and their results can be reused.

------------------------------------------------------------------------

## 4.20 Stack-Based Expression Processing

### Idea

Use stacks to process nested structure or expressions where the most
recent unmatched item must be handled first.

### Recognize it when

-   Balanced parentheses.
-   Evaluate Reverse Polish Notation.
-   Convert infix to postfix.
-   Decode nested strings.
-   Simplify paths or process nested scopes.

### Pitfalls

-   Validate stack emptiness before popping.
-   Track operator precedence and associativity for expression
    conversion.
-   Be careful with unary minus, integer division, and malformed input
    if those cases are in scope.

------------------------------------------------------------------------

## 4.21 Kadane's Algorithm

### Goal

Find the maximum-sum non-empty contiguous subarray in linear time.

### Java template

``` java
int current = nums[0];
int best = nums[0];

for (int i = 1; i < nums.length; i++) {
    current = Math.max(nums[i], current + nums[i]);
    best = Math.max(best, current);
}
```

Time: `O(n)`. Extra space: `O(1)`.

### Idea

At each index, the best subarray ending here either starts at the
current element or extends the best subarray ending at the previous
index.

### Pitfalls

-   Initialize from the first element, not zero, if all values may be
    negative.
-   Use `long` if sums can overflow `int`.
-   Maximum product subarray requires tracking both maximum and minimum
    ending products because multiplying by a negative swaps their roles.

------------------------------------------------------------------------

## 4.22 Meet in the Middle

### Idea

For an exponential search space of moderate size, split the input into
two halves, enumerate results for each half, and combine them
efficiently.

### Recognize it when

-   `n` is too large for `O(2^n)` but small enough for roughly
    `O(2^(n/2))`.
-   Subset sums or combinations are involved.
-   Constraints suggest about 30--40 elements rather than hundreds or
    thousands.

### Trade-off

The approach reduces the exponential exponent but uses memory to store
partial results. It is more advanced and usually learned after
backtracking, hashing, and binary search.

------------------------------------------------------------------------

## 5. Java Templates Library

These compact templates are starting points, not complete solutions for
every problem.

### 5.1 Lower Bound: first index with value `>= target`

``` java
int lowerBound(int[] nums, int target) {
    int left = 0, right = nums.length; // half-open [left, right)

    while (left < right) {
        int mid = left + (right - left) / 2;

        if (nums[mid] < target) left = mid + 1;
        else right = mid;
    }

    return left; // may equal nums.length
}
```

### 5.2 BFS on an adjacency list

``` java
boolean[] visited = new boolean[n];
Queue<Integer> queue = new ArrayDeque<>();

visited[source] = true;
queue.offer(source);

while (!queue.isEmpty()) {
    int node = queue.poll();

    for (int next : graph.get(node)) {
        if (!visited[next]) {
            visited[next] = true;
            queue.offer(next);
        }
    }
}
```

### 5.3 DFS on an adjacency list

``` java
void dfs(int node, List<List<Integer>> graph, boolean[] visited) {
    visited[node] = true;

    for (int next : graph.get(node)) {
        if (!visited[next]) {
            dfs(next, graph, visited);
        }
    }
}
```

For very deep graphs, consider iterative DFS to avoid stack overflow.

### 5.4 Frequency map

``` java
Map<String, Integer> map = new HashMap<>();
for (String item : items) {
    map.merge(item, 1, Integer::sum);
}
```

### 5.5 Min-heap and max-heap

``` java
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
PriorityQueue<Integer> maxHeap =
        new PriorityQueue<>(Collections.reverseOrder());
```

### 5.6 Comparator safety

Prefer:

``` java
Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
```

Avoid `a[0] - b[0]` in comparators because subtraction can overflow.

### 5.7 Linked-list node definition

``` java
class ListNode {
    int val;
    ListNode next;

    ListNode(int val) {
        this.val = val;
    }
}
```

### 5.8 Tree node definition

``` java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int val) {
        this.val = val;
    }
}
```

For snippets using `Arrays`, `ArrayList`, `List`, `Map`, `HashMap`,
`Set`, `HashSet`, `Queue`, `Deque`, `ArrayDeque`, `PriorityQueue`, or
`Collections`, import the appropriate classes from `java.util`.

------------------------------------------------------------------------

## 6. Choosing Between Similar Patterns

### Sliding Window vs Prefix Sum

-   **Sliding window:** best when a valid contiguous window can be
    maintained by expanding/shrinking boundaries; common with
    positive/non-negative constraints or character-frequency conditions.
-   **Prefix sum:** best for range sums and many subarray-sum counting
    tasks, especially when negative values are allowed.
-   **Prefix sum + HashMap:** often ideal for counting subarrays with
    sum exactly `k`.

### Two Pointers vs HashMap

-   **Two pointers:** often `O(n)` after sorting or when the input
    already has useful order.
-   **HashMap:** often `O(n)` average without sorting, at the cost of
    extra space.
-   Sorting first may change the original order, which can matter.

### BFS vs DFS

-   **BFS:** shortest path in unweighted graphs; level-based answers.
-   **DFS:** reachability, component exploration, recursion over tree
    structure, exhaustive exploration.
-   Both can be `O(V + E)` for graph traversal.

### Heap vs Sorting vs Quickselect

-   **Heap:** top K, streaming, or repeatedly extracting extremes.
-   **Sorting:** when complete order is needed or simplicity is
    preferred.
-   **Quickselect:** one or a few order-statistic queries on an array.

### Greedy vs Dynamic Programming

-   **Greedy:** a provably safe local choice is available.
-   **DP:** multiple choices interact across states; subproblem answers
    can be reused.
-   Test a greedy strategy against small counterexamples before trusting
    it.

### Backtracking vs DP

-   **Backtracking:** enumerate or search candidates; pruning can reduce
    work.
-   **DP:** merge repeated states by caching results.
-   A search problem can often be optimized into DP when many branches
    reach the same state.

### Merge Intervals vs Sweep Line

-   **Merge intervals:** output a compact list of merged ranges.
-   **Sweep line:** process events and maintain active counts or objects
    over coordinates/time.

### Binary Search in Data vs Binary Search on Answer

-   **In data:** search for a value or boundary in sorted data.
-   **On answer:** search a range of candidate outputs using a monotonic
    feasibility test.

------------------------------------------------------------------------

## 7. Complexity Reference

  -----------------------------------------------------------------------
  Operation / algorithm                                Typical complexity
  ------------------------------ ----------------------------------------
  Array access by index                                            `O(1)`

  Linear scan                                                      `O(n)`

  Binary search                                                `O(log n)`

  Comparison sorting                                         `O(n log n)`

  HashMap lookup/insert                                    `O(1)` average

  Heap insertion/removal                                       `O(log n)`

  Read heap root                                                   `O(1)`

  BFS / DFS on adjacency list                                  `O(V + E)`

  Union-Find operations            Amortized `O(α(n))` with optimizations

  Prefix-sum preprocessing                                         `O(n)`

  Sliding window with each                                         `O(n)`
  pointer advancing at most `n`  
  times                          

  Merge `k` sorted sequences,                                `O(N log k)`
  `N` total elements             

  Enumerate all subsets           `O(2^n)` outputs; `O(n 2^n)` if copying
                                                              each subset

  Enumerate all permutations                              `O(n!)` outputs

  Standard 0/1 knapsack DP               `O(nW)` time, often `O(W)` space

  Dijkstra with binary heap                    `O((V + E) log V)` typical
                                                     adjacency-list bound

  Bellman-Ford                                                    `O(VE)`

  Floyd-Warshall                                                 `O(V^3)`

  Kruskal MST                                                `O(E log E)`
  -----------------------------------------------------------------------

`n` is the number of elements, `V` vertices, `E` edges, `W` a numeric
capacity, and `α(n)` the inverse Ackermann function.

Complexity depends on the exact implementation and assumptions.
Output-producing algorithms may require substantial output space even if
their auxiliary space is small.

------------------------------------------------------------------------

## 8. Common Mistakes

1.  **Pattern forcing:** choosing a familiar pattern without checking
    the constraints.
2.  **Ignoring input limits:** an `O(n^2)` solution may fail for large
    `n`.
3.  **Wrong window invariant:** updating the answer before restoring
    validity or shrinking too far.
4.  **Binary-search boundary bugs:** mixing inclusive and half-open
    ranges.
5.  **Integer overflow:** sums, products, midpoint calculations, and
    comparator subtraction.
6.  **Missing visited tracking:** graph traversals can repeat work or
    loop forever.
7.  **Incorrect graph assumptions:** using Dijkstra with negative edges
    or topological sort on a cyclic dependency graph.
8.  **Incorrect duplicate handling:** especially in three-sum, cyclic
    sort, and backtracking.
9.  **Broken pointer updates:** overwriting a linked-list link before
    saving the next node.
10. **Incorrect DP state:** failing to include a variable that changes
    future choices.
11. **Greedy without proof:** passing a few examples does not establish
    correctness.
12. **Mutating shared backtracking state:** forgetting to undo changes
    or copy completed results.
13. **Forgetting empty/single-element cases:** empty arrays, one-node
    lists, null roots, or `k = 0`.
14. **Ignoring input semantics:** inclusive vs half-open intervals,
    directed vs undirected edges, and whether duplicates are allowed.

------------------------------------------------------------------------

## 9. Interview Problem-Solving Workflow

Use this sequence in interviews and timed practice.

### Step 1: Clarify

-   What are the input sizes?
-   Can values be negative or duplicated?
-   Is input sorted?
-   Is the graph directed? Can it contain cycles?
-   Is the result a count, one answer, or all answers?
-   Are in-place changes allowed?

### Step 2: Start with a simple approach

Describe a brute-force approach first. It provides a correctness
baseline and reveals what work must be optimized.

### Step 3: Find the bottleneck

Ask what is repeated: scanning ranges, looking up complements, exploring
repeated states, sorting all values, or revisiting graph nodes.

### Step 4: Match the pattern

Use the recognition table to propose one or two candidate approaches.
Explain why the input constraints fit.

### Step 5: State the invariant

Examples:

-   Sliding window: the current window satisfies the required condition.
-   Binary search: the answer remains within the search interval.
-   Monotonic stack: indices in the stack preserve the chosen order.
-   BFS: nodes are processed in non-decreasing distance from the source.
-   DP: each state represents a clearly defined subproblem.

### Step 6: Code in small pieces

Name pointers and state clearly. Avoid over-compressing logic. Explain
important updates as you write them.

### Step 7: Test

Test: - Empty or minimal input where permitted. - One element. -
Duplicates. - All values equal. - Negative values and zeros. - Already
sorted and reverse sorted input. - No solution / multiple solutions. -
Maximum constraints and overflow boundaries.

### Step 8: State complexity

Give both time and auxiliary space, and mention output space separately
if relevant.

------------------------------------------------------------------------

## 10. Suggested Learning Roadmap

A practical order for a Java developer preparing for coding interviews:

### Phase 1 --- Foundations

-   Arrays and strings
-   HashMap / HashSet
-   Two pointers
-   Sliding window
-   Prefix sums
-   Basic sorting and binary search

### Phase 2 --- Core data structures

-   Linked lists and in-place reversal
-   Stacks and queues
-   Monotonic stack/deque
-   Heaps and Top K
-   Intervals and cyclic sort

### Phase 3 --- Trees and graphs

-   Tree DFS and BFS
-   Binary search trees
-   Graph representation
-   Graph BFS/DFS
-   Topological sort
-   Union-Find
-   Dijkstra and MST basics

### Phase 4 --- Search and optimization

-   Backtracking
-   Greedy algorithms
-   Binary search on answer
-   Dynamic programming: 1D, grid, knapsack, strings, subsequences

### Phase 5 --- Advanced / specialized

-   Tries
-   Bitmasking and bit manipulation
-   Quickselect
-   Sweep line
-   Interval DP
-   Meet in the middle
-   Advanced graph algorithms as needed

### Suggested practice method

For each pattern: 1. Learn the invariant and a basic template. 2. Solve
2 easy problems without looking at a solution. 3. Solve 3--5 medium
problems with variations. 4. Re-solve missed problems after a few days.
5. Write down the clue that should have helped you recognize the
pattern. 6. Explain one solution aloud and state its complexity.

Do not measure progress only by the number of problems completed.
Measure whether you can recognize the pattern and derive the solution
independently.

------------------------------------------------------------------------

## 11. Practice Checklist

Mark each pattern when you can explain it and solve a representative
problem without notes.

### Arrays and strings

-   [ ] Hashing / frequency counting
-   [ ] Two pointers
-   [ ] Sliding window
-   [ ] Prefix sum
-   [ ] Difference array
-   [ ] Binary search
-   [ ] Binary search on answer
-   [ ] Cyclic sort / index placement
-   [ ] Kadane's algorithm
-   [ ] Monotonic stack / deque
-   [ ] Quickselect

### Linked lists

-   [ ] Fast and slow pointers
-   [ ] Cycle detection
-   [ ] In-place reversal
-   [ ] Reverse in groups / reverse a sublist

### Intervals and heaps

-   [ ] Merge intervals
-   [ ] Sweep line
-   [ ] Top K elements
-   [ ] Two heaps
-   [ ] K-way merge

### Trees

-   [ ] DFS traversals
-   [ ] BFS / level order
-   [ ] BST properties
-   [ ] Tree construction
-   [ ] Lowest common ancestor
-   [ ] Tree DP

### Graphs

-   [ ] BFS and DFS
-   [ ] Connected components
-   [ ] Grid traversal
-   [ ] Topological sort
-   [ ] Union-Find
-   [ ] Dijkstra
-   [ ] Bellman-Ford concept
-   [ ] Minimum spanning tree

### Search and optimization

-   [ ] Backtracking: subsets and combinations
-   [ ] Backtracking: permutations
-   [ ] Greedy with correctness reasoning
-   [ ] DP: 1D
-   [ ] DP: grid
-   [ ] DP: knapsack/subset sum
-   [ ] DP: strings/LCS/edit distance
-   [ ] DP: LIS
-   [ ] Trie
-   [ ] Bit manipulation
-   [ ] Meet in the middle

------------------------------------------------------------------------

## 12. References

1.  **Fahim ul Haq, HackerNoon:** [14 Patterns to Ace Any Coding
    Interview
    Question](https://hackernoon.com/14-patterns-to-ace-any-coding-interview-question-c5bb3357f6ed)
    --- the core framework for the original 14 patterns.
2.  **John Vandivier, HackerNoon:** [5 to 23 Patterns to Ace Any Coding
    Interview](https://hackernoon.com/5-to-23-patterns-to-ace-any-coding-interview)
    --- discussion of extending the original pattern framework.
3.  **InterviewPilot:** [15 DSA Patterns for Coding
    Interviews](https://www.interviewpilot.in/blog/dsa-patterns-for-coding-interviews/)
    --- a complementary pattern overview.
4.  **LeetCode discussion:** [14 Patterns to Ace Any Coding Interview
    Question](https://leetcode.com/discuss/post/4039411/14-Patterns-to-Ace-Any-Coding-Interview-Question/)
    --- a community reference to the original framework.

------------------------------------------------------------------------

## Final Advice

**Recognize the shape, state the invariant, then implement.** Patterns
are not substitutes for understanding the problem. The strongest
interview preparation combines pattern recognition with careful
constraint analysis, correctness reasoning, complexity analysis, and
repeated practice.
