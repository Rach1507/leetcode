## 1. Brute Force

::tabs-start

```python
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        count = [0] * 26
        for task in tasks:
            count[ord(task) - ord('A')] += 1
        
        arr = []
        for i in range(26):
            if count[i] > 0:
                arr.append([count[i], i])

        time = 0
        processed = []
        while arr:
            maxi = -1
            for i in range(len(arr)):
                if all(processed[j] != arr[i][1] for j in range(max(0, time - n), time)):
                    if maxi == -1 or arr[maxi][0] < arr[i][0]:
                        maxi = i
            
            time += 1
            cur = -1
            if maxi != -1:
                cur = arr[maxi][1]
                arr[maxi][0] -= 1
                if arr[maxi][0] == 0:
                    arr.pop(maxi)
            processed.append(cur)
        return time
```

```java
public class Solution {
    public int leastInterval(char[] tasks, int n) {
        int[] count = new int[26];
        for (char task : tasks) {
            count[task - 'A']++;
        }
        
        List<int[]> arr = new ArrayList<>();
        for (int i = 0; i < 26; i++) {
            if (count[i] > 0) {
                arr.add(new int[]{count[i], i});
            }
        }

        int time = 0;
        List<Integer> processed = new ArrayList<>();
        while (!arr.isEmpty()) {
            int maxi = -1;
            for (int i = 0; i < arr.size(); i++) {
                boolean ok = true;
                for (int j = Math.max(0, time - n); j < time; j++) {
                    if (j < processed.size() && processed.get(j) == arr.get(i)[1]) {
                        ok = false;
                        break;
                    }
                }
                if (!ok) continue;
                if (maxi == -1 || arr.get(maxi)[0] < arr.get(i)[0]) {
                    maxi = i;
                }
            }
            
            time++;
            int cur = -1;
            if (maxi != -1) {
                cur = arr.get(maxi)[1];
                arr.get(maxi)[0]--;
                if (arr.get(maxi)[0] == 0) {
                    arr.remove(maxi);
                }
            }
            processed.add(cur);
        }
        return time;
    }
}
```

```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        vector<int> count(26, 0);
        for (char task : tasks) {
            count[task - 'A']++;
        }
        
        vector<pair<int, int>> arr;
        for (int i = 0; i < 26; i++) {
            if (count[i] > 0) {
                arr.emplace_back(count[i], i);
            }
        }

        int time = 0;
        vector<int> processed;
        while (!arr.empty()) {
            int maxi = -1;
            for (int i = 0; i < arr.size(); i++) {
                bool ok = true;
                for (int j = max(0, time - n); j < time; j++) {
                    if (j < processed.size() && processed[j] == arr[i].second) {
                        ok = false;
                        break;
                    }
                }
                if (!ok) continue;
                if (maxi == -1 || arr[maxi].first < arr[i].first) {
                    maxi = i;
                }
            }
            
            time++;
            int cur = -1;
            if (maxi != -1) {
                cur = arr[maxi].second;
                arr[maxi].first--;
                if (arr[maxi].first == 0) {
                    arr.erase(arr.begin() + maxi);
                }
            }
            processed.push_back(cur);
        }
        return time;
    }
};
```

```javascript
class Solution {
    /**
     * @param {character[]} tasks
     * @param {number} n
     * @return {number}
     */
    leastInterval(tasks, n) {
        const count = new Array(26).fill(0);
        for (const task of tasks) {
            count[task.charCodeAt(0) - 'A'.charCodeAt(0)]++;
        }
        
        const arr = [];
        for (let i = 0; i < 26; i++) {
            if (count[i] > 0) {
                arr.push([count[i], i]);
            }
        }

        let time = 0;
        const processed = [];
        while (arr.length > 0) {
            let maxi = -1;
            for (let i = 0; i < arr.length; i++) {
                let ok = true;
                for (let j = Math.max(0, time - n); j < time; j++) {
                    if (j < processed.length && processed[j] === arr[i][1]) {
                        ok = false;
                        break;
                    }
                }
                if (!ok) continue;
                if (maxi === -1 || arr[maxi][0] < arr[i][0]) {
                    maxi = i;
                }
            }
            
            time++;
            let cur = -1;
            if (maxi !== -1) {
                cur = arr[maxi][1];
                arr[maxi][0]--;
                if (arr[maxi][0] === 0) {
                    arr.splice(maxi, 1);
                }
            }
            processed.push(cur);
        }
        return time;
    }
}
```

```csharp
public class Solution {
    public int LeastInterval(char[] tasks, int n) {
        int[] count = new int[26];
        foreach (char task in tasks) {
            count[task - 'A']++;
        }
        
        List<int[]> arr = new List<int[]>();
        for (int i = 0; i < 26; i++) {
            if (count[i] > 0) {
                arr.Add(new int[] { count[i], i });
            }
        }

        int time = 0;
        List<int> processed = new List<int>();
        while (arr.Count > 0) {
            int maxi = -1;
            for (int i = 0; i < arr.Count; i++) {
                bool ok = true;
                for (int j = Math.Max(0, time - n); j < time; j++) {
                    if (j < processed.Count && processed[j] == arr[i][1]) {
                        ok = false;
                        break;
                    }
                }
                if (!ok) continue;
                if (maxi == -1 || arr[maxi][0] < arr[i][0]) {
                    maxi = i;
                }
            }
            
            time++;
            int cur = -1;
            if (maxi != -1) {
                cur = arr[maxi][1];
                arr[maxi][0]--;
                if (arr[maxi][0] == 0) {
                    arr.RemoveAt(maxi);
                }
            }
            processed.Add(cur);
        }
        return time;
    }
}
```

::tabs-end

### Time & Space Complexity

* Time complexity: $O(t * n)$
* Space complexity: $O(t)$

> Where $t$ is the time to process given tasks and $n$ is the cooldown time.

---

## 2. Heap

::tabs-start

```python
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        count = Counter(tasks)
        maxHeap = [-cnt for cnt in count.values()]
        heapq.heapify(maxHeap)

        time = 0
        q = deque()  # pairs of [-cnt, idleTime]
        while maxHeap or q:
            time += 1

            if not maxHeap:
                time = q[0][1]
            else:
                cnt = 1 + heapq.heappop(maxHeap)
                if cnt:
                    q.append([cnt, time + n])
            if q and q[0][1] == time:
                heapq.heappush(maxHeap, q.popleft()[0])
        return time
```

```java
public class Solution {
    public int leastInterval(char[] tasks, int n) {
        int[] count = new int[26];
        for (char task : tasks) {
            count[task - 'A']++;
        }

        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
        for (int cnt : count) {
            if (cnt > 0) {
                maxHeap.add(cnt);
            }
        }

        int time = 0;
        Queue<int[]> q = new LinkedList<>();
        while (!maxHeap.isEmpty() || !q.isEmpty()) {
            time++;
            
            if (maxHeap.isEmpty()) {
                time = q.peek()[1];
            } else {
                int cnt = maxHeap.poll() - 1;
                if (cnt > 0) {
                    q.add(new int[]{cnt, time + n});
                }
            }

            if (!q.isEmpty() && q.peek()[1] == time) {
                maxHeap.add(q.poll()[0]);
            }
        }
        
        return time;
    }
}
```

```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        vector<int> count(26, 0);
        for (char task : tasks) {
            count[task - 'A']++;
        }
        
        priority_queue<int> maxHeap;
        for (int cnt : count) {
            if (cnt > 0) {
                maxHeap.push(cnt);
            }
        }
        
        int time = 0;
        queue<pair<int, int>> q;
        while (!maxHeap.empty() || !q.empty()) {
            time++;
            
            if (maxHeap.empty()) {
                time = q.front().second;
            } else {
                int cnt = maxHeap.top() - 1;
                maxHeap.pop();
                if (cnt > 0) {
                    q.push({cnt, time + n});
                }
            }
            
            if (!q.empty() && q.front().second == time) {
                maxHeap.push(q.front().first);
                q.pop();
            }
        }
        
        return time;
    }
};
```

```javascript
class Solution {
    /**
     * @param {character[]} tasks
     * @param {number} n
     * @return {number}
     */
    leastInterval(tasks, n) {
        let count = new Array(26).fill(0);
        for (let task of tasks) {
            count[task.charCodeAt(0) - 'A'.charCodeAt(0)]++;
        }

        let maxHeap = new MaxPriorityQueue();
        for (let i = 0; i < 26; i++) {
            if (count[i] > 0) maxHeap.push(count[i]);
        }

        let time = 0;
        let q = new Queue(); 

        while (maxHeap.size() > 0 || q.size() > 0) {
            time++;

            if (maxHeap.size() > 0) {
                let cnt = maxHeap.pop() - 1; 
                if (cnt !== 0) {
                    q.push([cnt, time + n]); 
                }
            }

            if (q.size() > 0 && q.front()[1] === time) {
                maxHeap.push(q.pop()[0]);
            }
        }

        return time;
    }
}
```

```csharp
public class Solution {
    public int LeastInterval(char[] tasks, int n) {
        int[] count = new int[26];
        foreach (var task in tasks) {
            count[task - 'A']++;
        }

        var maxHeap = new PriorityQueue<int, int>();
        for (int i = 0; i < 26; i++) {
            if (count[i] > 0) {
                maxHeap.Enqueue(count[i], -count[i]);
            }
        }

        int time = 0;
        Queue<int[]> queue = new Queue<int[]>();  
        while (maxHeap.Count > 0 || queue.Count > 0) {
            if (queue.Count > 0 && time >= queue.Peek()[1]) {
                int[] temp = queue.Dequeue();
                maxHeap.Enqueue(temp[0], -temp[0]);
            }
            if (maxHeap.Count > 0) {
                int cnt = maxHeap.Dequeue() - 1;
                if (cnt > 0) {
                    queue.Enqueue(new int[] { cnt, time + n + 1 });
                }
            }
            time++;
        }
        return time;
    }
}
```

::tabs-end

### Time & Space Complexity

* Time complexity: $O(m)$
* Space complexity: $O(m)$

> Where $m$ is the number of tasks.

---

## 3. Greedy

::tabs-start

```python
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        count = [0] * 26
        for task in tasks:
            count[ord(task) - ord('A')] += 1
        
        count.sort()
        maxf = count[25]
        idle = (maxf - 1) * n

        for i in range(24, -1, -1):
            idle -= min(maxf - 1, count[i])
        return max(0, idle) + len(tasks)
```

```java
public class Solution {
    public int leastInterval(char[] tasks, int n) {
        int[] count = new int[26];
        for (char task : tasks) {
            count[task - 'A']++;
        }

        Arrays.sort(count);
        int maxf = count[25];
        int idle = (maxf - 1) * n;

        for (int i = 24; i >= 0; i--) {
            idle -= Math.min(maxf - 1, count[i]);
        }
        return Math.max(0, idle) + tasks.length;
    }
}
```

```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        vector<int> count(26, 0);
        for (char task : tasks) {
            count[task - 'A']++;
        }

        sort(count.begin(), count.end());
        int maxf = count[25];
        int idle = (maxf - 1) * n;

        for (int i = 24; i >= 0; i--) {
            idle -= min(maxf - 1, count[i]);
        }
        return max(0, idle) + tasks.size();
    }
};
```

```javascript
class Solution {
    /**
     * @param {character[]} tasks
     * @param {number} n
     * @return {number}
     */
    leastInterval(tasks, n) {
        const count = new Array(26).fill(0);
        for (const task of tasks) {
            count[task.charCodeAt(0) - 'A'.charCodeAt(0)]++;
        }

        count.sort((a, b) => a - b);
        const maxf = count[25];
        let idle = (maxf - 1) * n;

        for (let i = 24; i >= 0; i--) {
            idle -= Math.min(maxf - 1, count[i]);
        }
        return Math.max(0, idle) + tasks.length;
    }
}
```

```csharp
public class Solution {
    public int LeastInterval(char[] tasks, int n) {
        int[] count = new int[26];
        foreach (char task in tasks) {
            count[task - 'A']++;
        }

        Array.Sort(count);
        int maxf = count[25];
        int idle = (maxf - 1) * n;

        for (int i = 24; i >= 0; i--) {
            idle -= Math.Min(maxf - 1, count[i]);
        }
        return Math.Max(0, idle) + tasks.Length;
    }
}
```

::tabs-end

### Time & Space Complexity

* Time complexity: $O(m)$
* Space complexity: $O(1)$

> Where $m$ is the number of tasks.

---

## 4. Math

::tabs-start

```python
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        count = [0] * 26
        for task in tasks:
            count[ord(task) - ord('A')] += 1
        
        maxf = max(count)
        maxCount = 0
        for i in count:
            maxCount += 1 if i == maxf else 0

        time = (maxf - 1) * (n + 1) + maxCount
        return max(len(tasks), time)
```

```java
public class Solution {
    public int leastInterval(char[] tasks, int n) {
        int[] count = new int[26];
        for (char task : tasks) {
            count[task - 'A']++;
        }
        
        int maxf = Arrays.stream(count).max().getAsInt();
        int maxCount = 0;
        for (int i : count) {
            if (i == maxf) {
                maxCount++;
            }
        }

        int time = (maxf - 1) * (n + 1) + maxCount;
        return Math.max(tasks.length, time);
    }
}
```

```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        vector<int> count(26, 0);
        for (char task : tasks) {
            count[task - 'A']++;
        }

        int maxf = *max_element(count.begin(), count.end());
        int maxCount = 0;
        for (int i : count) {
            if (i == maxf) {
                maxCount++;
            }
        }

        int time = (maxf - 1) * (n + 1) + maxCount;
        return max((int)tasks.size(), time);
    }
};
```

```javascript
class Solution {
    /**
     * @param {character[]} tasks
     * @param {number} n
     * @return {number}
     */
    leastInterval(tasks, n) {
        const count = new Array(26).fill(0);
        for (const task of tasks) {
            count[task.charCodeAt(0) - 'A'.charCodeAt(0)]++;
        }

        const maxf = Math.max(...count);
        let maxCount = 0;
        for (const i of count) {
            if (i === maxf) {
                maxCount++;
            }
        }

        const time = (maxf - 1) * (n + 1) + maxCount;
        return Math.max(tasks.length, time);
    }
}
```

```csharp
public class Solution {
    public int LeastInterval(char[] tasks, int n) {
        int[] count = new int[26];
        foreach (char task in tasks) {
            count[task - 'A']++;
        }

        int maxf = count.Max();
        int maxCount = 0;
        foreach (int i in count) {
            if (i == maxf) {
                maxCount++;
            }
        }

        int time = (maxf - 1) * (n + 1) + maxCount;
        return Math.Max(tasks.Length, time);
    }
}
```

::tabs-end

### Time & Space Complexity

* Time complexity: $O(m)$
* Space complexity: $O(1)$

> Where $m$ is the number of tasks.