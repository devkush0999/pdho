# 05. DSA Deep: 16 patterns, ~170 problems, practice ka tareeka

> Product companies (18–35 LPA band) mein 1–2 rounds DSA ke hote hain: **LeetCode medium, 45 minute, bolte hue**.
> Ginti se zyada **pattern pehchanna** zaroori hai. Ye list NeetCode 150 / Blind 75 se milti-julti hai, pattern-wise kram mein.
> Language: jo interview mein use karoge (Kotlin / Swift / Dart / TypeScript / Java / Python). Ek hi rakho.

---

## 1. Practice ka tareeka (yahi asli secret hai)

**Ek problem ka process (max 60 min):**
1. **5 min:** sawaal padho, examples haath se chalao, edge cases likho (empty, ek element, duplicates, negative, bahut bada).
2. **5 min:** brute force bolo + complexity. Phir better approach socho: kaunsa pattern?
3. **15 min:** code karo (bina run kiye sahi likhne ki aadat).
4. **5 min:** dry run apne example par, edge cases.
5. Atak gaye 25 min par → solution dekho, **samjho**, band karo, **khud dobara likho**.
6. Notebook mein: problem, pattern, trick 1 line mein, galti kya hui.

**Spaced repetition:** jo khud solve nahi hua, use 3 din baad, phir 7 din baad, phir 21 din baad dobara. Ek spreadsheet: `Problem | Pattern | Date | Khud hua? | Revisit dates`.

**Bolna practice:** har 5th problem zor se bol kar solve karo jaise interviewer saamne ho. Ye alag skill hai.

**Complexity har baar:** time + space, kyun.

---

## 2. Patterns + problems (kram se)

Difficulty: E = easy, M = medium, H = hard. ⭐ = zaroor karna.

### Pattern 1: Arrays + Hashing (Hafta 1–2)
Idea: lookups O(1), counting, grouping.
| Problem | Diff |
|---|---|
| Contains Duplicate ⭐ | E |
| Valid Anagram ⭐ | E |
| Two Sum ⭐ | E |
| Group Anagrams ⭐ | M |
| Top K Frequent Elements ⭐ | M |
| Product of Array Except Self ⭐ | M |
| Valid Sudoku | M |
| Encode and Decode Strings | M |
| Longest Consecutive Sequence ⭐ | M |
| Subarray Sum Equals K ⭐ (prefix sum + hashmap) | M |
| Majority Element | E |
| Sort Colors | M |

### Pattern 2: Two Pointers (Hafta 2)
Idea: sorted array, ya do sire se andar.
| Problem | Diff |
|---|---|
| Valid Palindrome ⭐ | E |
| Two Sum II (sorted) | M |
| 3Sum ⭐ | M |
| Container With Most Water ⭐ | M |
| Trapping Rain Water | H |
| Remove Duplicates from Sorted Array | E |
| Move Zeroes | E |

### Pattern 3: Sliding Window (Hafta 3)
Template: `right` badhao, condition toote to `left` badhao, answer update.
| Problem | Diff |
|---|---|
| Best Time to Buy and Sell Stock ⭐ | E |
| Longest Substring Without Repeating Characters ⭐ | M |
| Longest Repeating Character Replacement ⭐ | M |
| Permutation in String | M |
| Minimum Window Substring ⭐ | H |
| Sliding Window Maximum (deque) | H |
| Minimum Size Subarray Sum | M |
| Max Consecutive Ones III | M |

### Pattern 4: Stack (Hafta 4)
Idea: matching, "next greater", monotonic stack.
| Problem | Diff |
|---|---|
| Valid Parentheses ⭐ | E |
| Min Stack ⭐ | M |
| Evaluate Reverse Polish Notation | M |
| Generate Parentheses | M |
| Daily Temperatures ⭐ (monotonic) | M |
| Car Fleet | M |
| Largest Rectangle in Histogram | H |
| Decode String | M |

### Pattern 5: Binary Search (Hafta 5)
Template: `lo, hi`, `while lo <= hi` (ya `<`), mid; **"answer par binary search"** pattern.
| Problem | Diff |
|---|---|
| Binary Search ⭐ | E |
| Search a 2D Matrix | M |
| Koko Eating Bananas ⭐ (answer par BS) | M |
| Find Minimum in Rotated Sorted Array ⭐ | M |
| Search in Rotated Sorted Array ⭐ | M |
| Time Based Key-Value Store | M |
| Find First and Last Position of Element | M |
| Capacity To Ship Packages Within D Days | M |
| Median of Two Sorted Arrays | H |

### Pattern 6: Linked List (Hafta 6)
| Problem | Diff |
|---|---|
| Reverse Linked List ⭐ | E |
| Merge Two Sorted Lists ⭐ | E |
| Linked List Cycle ⭐ (fast/slow) | E |
| Reorder List | M |
| Remove Nth Node From End ⭐ | M |
| Copy List with Random Pointer | M |
| Add Two Numbers | M |
| Find the Duplicate Number | M |
| **LRU Cache ⭐⭐** (hashmap + doubly linked list; mobile/backend dono mein poocha jaata hai) | M |
| Merge K Sorted Lists | H |
| Reverse Nodes in k-Group | H |

### Pattern 7: Trees (Hafta 7–8)
Recursion: "har node par kya chahiye, children se kya milega".
| Problem | Diff |
|---|---|
| Invert Binary Tree ⭐ | E |
| Maximum Depth of Binary Tree ⭐ | E |
| Diameter of Binary Tree ⭐ | E |
| Balanced Binary Tree | E |
| Same Tree / Subtree of Another Tree | E |
| Lowest Common Ancestor of a BST ⭐ | M |
| Binary Tree Level Order Traversal ⭐ (BFS) | M |
| Binary Tree Right Side View | M |
| Count Good Nodes in Binary Tree | M |
| Validate Binary Search Tree ⭐ | M |
| Kth Smallest Element in a BST | M |
| Construct Binary Tree from Preorder and Inorder | M |
| Lowest Common Ancestor of a Binary Tree ⭐ | M |
| Binary Tree Maximum Path Sum | H |
| Serialize and Deserialize Binary Tree | H |

### Pattern 8: Tries (Hafta 8)
| Problem | Diff |
|---|---|
| Implement Trie ⭐ | M |
| Design Add and Search Words Data Structure | M |
| Word Search II | H |

### Pattern 9: Heap / Priority Queue (Hafta 9)
| Problem | Diff |
|---|---|
| Kth Largest Element in a Stream ⭐ | E |
| Last Stone Weight | E |
| K Closest Points to Origin ⭐ | M |
| Kth Largest Element in an Array ⭐ | M |
| Task Scheduler ⭐ | M |
| Design Twitter | M |
| Find Median from Data Stream ⭐ (do heaps) | H |

### Pattern 10: Backtracking (Hafta 10)
Template: choose → explore → un-choose.
| Problem | Diff |
|---|---|
| Subsets ⭐ | M |
| Combination Sum ⭐ | M |
| Permutations ⭐ | M |
| Subsets II | M |
| Combination Sum II | M |
| Word Search ⭐ | M |
| Palindrome Partitioning | M |
| Letter Combinations of a Phone Number | M |
| N-Queens | H |

### Pattern 11: Graphs (Hafta 11–13)
BFS (shortest path unweighted), DFS, visited set, grid = graph.
| Problem | Diff |
|---|---|
| Number of Islands ⭐ | M |
| Max Area of Island | M |
| Clone Graph ⭐ | M |
| Walls and Gates | M |
| Rotting Oranges ⭐ (multi-source BFS) | M |
| Pacific Atlantic Water Flow | M |
| Surrounded Regions | M |
| **Course Schedule ⭐⭐** (topological sort; dependency/build order) | M |
| Course Schedule II ⭐ | M |
| Graph Valid Tree | M |
| Number of Connected Components | M |
| Redundant Connection (Union-Find) | M |
| Word Ladder | H |
| Network Delay Time ⭐ (Dijkstra) | M |
| Cheapest Flights Within K Stops | M |
| Min Cost to Connect All Points (MST) | M |

### Pattern 12: Intervals (Hafta 13)
| Problem | Diff |
|---|---|
| Merge Intervals ⭐ | M |
| Insert Interval ⭐ | M |
| Non-overlapping Intervals | M |
| Meeting Rooms / Meeting Rooms II ⭐ | E / M |
| Minimum Interval to Include Each Query | H |

### Pattern 13: Greedy (Hafta 14)
| Problem | Diff |
|---|---|
| Maximum Subarray ⭐ (Kadane) | M |
| Jump Game ⭐ | M |
| Jump Game II | M |
| Gas Station | M |
| Hand of Straights | M |
| Partition Labels | M |
| Valid Parenthesis String | M |

### Pattern 14: 1-D Dynamic Programming (Hafta 15–16)
Steps: state kya hai → recurrence → base case → memo → tabulation → space optimize.
| Problem | Diff |
|---|---|
| Climbing Stairs ⭐ | E |
| Min Cost Climbing Stairs | E |
| House Robber ⭐ | M |
| House Robber II | M |
| Longest Palindromic Substring ⭐ | M |
| Palindromic Substrings | M |
| Decode Ways ⭐ | M |
| Coin Change ⭐ | M |
| Maximum Product Subarray | M |
| Word Break ⭐ | M |
| Longest Increasing Subsequence ⭐ | M |
| Partition Equal Subset Sum | M |

### Pattern 15: 2-D Dynamic Programming (Hafta 17)
| Problem | Diff |
|---|---|
| Unique Paths ⭐ | M |
| Longest Common Subsequence ⭐ | M |
| Best Time to Buy and Sell Stock with Cooldown | M |
| Coin Change II | M |
| Target Sum | M |
| Edit Distance ⭐ | M |
| Interleaving String | M |

### Pattern 16: Bit manipulation + Math (Hafta 18)
| Problem | Diff |
|---|---|
| Single Number ⭐ | E |
| Number of 1 Bits | E |
| Counting Bits | E |
| Missing Number | E |
| Reverse Integer | M |
| Pow(x, n) | M |
| Rotate Image ⭐ | M |
| Spiral Matrix ⭐ | M |
| Set Matrix Zeroes | M |

**Total ≈ 170.** ⭐ waale (~75) pehle; baaki time ho to.

---

## 3. Design-type coding sawaal (mobile/backend interviews mein common)

Ye LeetCode se thode alag hain, lekin bahut poochhe jaate hain:
- **LRU Cache** (zaroor)
- LFU Cache
- Rate limiter class (token bucket)
- Debounce / throttle function likhna
- Event emitter / observer
- Simple in-memory key-value store with TTL
- Iterator for nested list
- Retry with exponential backoff function
- Concurrent requests with max N parallel (promise pool / semaphore)
- Parse/format: JSON flatten, URL query parse

---

## 4. Templates jo yaad hone chahiye

```text
Sliding window:
  left = 0
  for right in range(n):
      add(arr[right])
      while window_invalid():
          remove(arr[left]); left += 1
      best = max(best, right - left + 1)

Binary search on answer:
  lo, hi = min_possible, max_possible
  while lo < hi:
      mid = (lo + hi) // 2
      if feasible(mid): hi = mid
      else: lo = mid + 1
  return lo

BFS (grid):
  queue = [start]; visited = {start}
  while queue:
      cell = queue.pop_front()
      for nb in neighbors(cell):
          if valid(nb) and nb not in visited:
              visited.add(nb); queue.push(nb)

Topological sort (Kahn):
  indegree count → queue of 0-indegree → pop, decrement neighbors → push new zeros
  if processed != n: cycle

Backtracking:
  def bt(path, choices):
      if done(path): result.append(copy(path)); return
      for c in choices:
          path.append(c); bt(path, next(choices, c)); path.pop()
```

---

## 5. Interview day: 45 minute ka structure

| Min | Kya |
|---|---|
| 0–5 | Sawaal dohrao, clarifying questions (input size? negatives? sorted? duplicates?), example |
| 5–10 | Brute force + complexity bolo; better approach; interviewer se agree karo |
| 10–30 | Code, bolte hue ("yahan hashmap kyunki…") |
| 30–38 | Dry run, edge cases, bugs khud pakdo |
| 38–45 | Complexity, follow-ups (agar input stream ho? memory kam ho?) |

**Atak gaye to:** chup mat raho. Jo soch rahe ho bolo; chhota version solve karo; hint maango. Interviewer process dekh raha hai.

---

## 6. Timeline (job ke saath, roz 45 min)

| Mahina | Patterns | Count (cumulative) | Mock |
|---|---|---|---|
| 1 | 1–3 | 30 | — |
| 2 | 4–5 | 60 | 1 |
| 3 | 6–7 | 90 | 2 |
| 4 | 8–10 | 115 | 2 |
| 5 | 11 | 135 | 3 |
| 6 | 12–14 | 155 | 3 |
| 7 | 15–16 + revisit | 170 | 4 |
| 8–9 | Mixed random, timed, company-tagged | + revisits | Har hafte 1–2 |

**Ready signal:** random LeetCode medium, 3 mein se 2, 35 min ke andar, bolte hue.

**Resources:** NeetCode.io (video explanations, same kram), LeetCode, "Grokking the Coding Interview" patterns, Pramp / interviewing.io (mock).
