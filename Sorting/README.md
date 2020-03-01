# Sorting

## [Sorting: Bubble Sort](https://www.hackerrank.com/challenges/ctci-bubble-sort/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=sorting)

```go
func countSwaps(a []int32) {
    var swaps int
    l := len(a)
    for i := 0; i < l; i++ {
        for j := 0; j < l - i - 1; j++ {
            if a[j] > a[j + 1] {
                a[j], a[j + 1] = a[j + 1], a[j]
                swaps++
            }
        }
    }
    fmt.Printf("Array is sorted in %d swaps.\n", swaps)
    fmt.Printf("First Element: %d\n", a[0])
    fmt.Printf("Last Element: %d\n", a[l - 1])
}
```

## [Mark and Toys](https://www.hackerrank.com/challenges/mark-and-toys/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=sorting)

```go
func maximumToys(prices []int32, k int32) int32 {
    sort.Slice(prices, func(i int, j int) bool {
        if prices[i] < prices[j] {
            return true
        }
        return false
    })
    var sum int32
    for i, v := range prices {
        sum += v
        if sum > k {
            return int32(len(prices[:i]))
        }
    }
    return 0
}
```

## [Sorting: Comparator](https://www.hackerrank.com/challenges/ctci-comparator-sorting/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=sorting)

```cpp
class Checker {
 public:
  // complete this method
  static int comparator(Player a, Player b) {
    if (a.score > b.score) {
      return 1;
    } else if (a.score == b.score) {
      if (a.name < b.name) {
        return 1;
      } else if (a.name == b.name) {
        return 0;
      }
      return -1;
    }
    return -1;
  }
};
```

## [Fraudulent Activity Notifications](https://www.hackerrank.com/challenges/fraudulent-activity-notifications/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=sorting)

```go
type minHeap []int

func (h minHeap) Len() int           { return len(h) }
func (h minHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h minHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *minHeap) Push(x interface{}) {
    *h = append(*h, x.(int))
}

func (h *minHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

type maxHeap []int

func (h maxHeap) Len() int           { return len(h) }
func (h maxHeap) Less(i, j int) bool { return h[i] > h[j] }
func (h maxHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *maxHeap) Push(x interface{}) {
    *h = append(*h, x.(int))
}

func (h *maxHeap) Pop() interface{} {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}

func abs(v int) int {
    if v < 0 {
        return v * -1
    }
    return v
}

func addAndBalance(min, max heap.Interface, maxRoot, val int) {
    if val < maxRoot {
        heap.Push(max, val)
    } else {
        heap.Push(min, val)
    }

    for abs(min.Len()-max.Len()) > 1 {
        if min.Len() > max.Len() {
            heap.Push(max, heap.Pop(min))
        } else {
            heap.Push(min, heap.Pop(max))
        }
    }
}

func binarySearchAsc(a []int, v int) (int, bool) {
    l, r := 0, len(a)-1
    for l <= r {
        m := int(math.Floor(float64((l + r) / 2)))
        if a[m] < v {
            l = m + 1
        } else if a[m] > v {
            r = m - 1
        } else {
            return m, true
        }
    }
    return 0, false
}

func binarySearchDsc(a []int, v int) (int, bool) {
    l, r := 0, len(a)-1
    for l <= r {
        m := int(math.Floor(float64((l + r) / 2)))
        if a[m] > v {
            l = m + 1
        } else if a[m] < v {
            r = m - 1
        } else {
            return m, true
        }
    }
    return 0, false
}

func linearSearch(a []int, v int) (int, bool) { // might be too slow...
    for i, e := range a {
        if e == v {
            return i, true
        }
    }
    return 0, false
}

func activityNotifications(expenditure []int32, d int32) int32 {
    var data []int
    for _, e := range expenditure {
        data = append(data, int(e))
    }
    period := int(d)

    prevData := data[:period]

    min, max := &minHeap{}, &maxHeap{}
    heap.Init(min)
    heap.Init(max)

    for _, v := range prevData {
        if max.Len() == 0 {
            addAndBalance(min, max, -math.MaxInt64, v)
        } else {
            addAndBalance(min, max, (*max)[0], v)
        }
    }

    j := period
    lenData := len(data)
    var notifications int
    for i := range data {
        if j == lenData {
            break
        }

        var median float64
        if min.Len() == max.Len() {
            median = float64((*min)[0]+(*max)[0]) / 2
        } else if min.Len() > max.Len() {
            median = float64((*min)[0])
        } else {
            median = float64((*max)[0])
        }

        if float64(data[j]) >= 2*median {
            notifications++
        }

        idx, ok := linearSearch(*min, data[i])
        if !ok {
            idx, ok = linearSearch(*max, data[i])
            if !ok {
                panic("too far")
            } else {
                heap.Remove(max, idx)
            }
        } else {
            heap.Remove(min, idx)
        }

        if max.Len() == 0 {
            addAndBalance(min, max, -math.MaxInt64, data[j])
        } else {
            addAndBalance(min, max, (*max)[0], data[j])
        }

        i++
        j++
    }

    return int32(notifications)
}
```

## [Merge Sort: Counting Inversions](https://www.hackerrank.com/challenges/ctci-merge-sort/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=sorting)

```go
func mergesort(arr []int32, aux []int32, ls, re int, cnt int64) int64 {
    if ls >= re {
        return cnt
    }
    m := (ls + re) / 2
    cnt = mergesort(arr, aux, ls, m, cnt)
    cnt = mergesort(arr, aux, m + 1, re, cnt)
    cnt += mergeHalves(arr, aux, ls, re)
    return cnt
}

func mergeHalves(arr, aux []int32, ls, re int) int64 {
    le := (ls + re) / 2
    rs := le + 1
    li := ls
    ri := rs
    ai := ls
    var cnt int64
    for (li <= le && ri <= re) {
        if arr[li] <= arr[ri] {
            aux[ai] = arr[li]
            li++
        } else {
            aux[ai] = arr[ri]
            cnt += int64(ri) - int64(ai)
            ri++
        }
        ai++
    }
    copy(aux[ai:], arr[li:le+1])
    copy(aux[ai:], arr[ri:re+1])
    copy(arr[ls:], aux[ls:re+1])
    return cnt
}

func countInversions(arr []int32) int64 {
    return mergesort(arr, make([]int32, len(arr)), 0, len(arr) - 1, 0)
}
```
