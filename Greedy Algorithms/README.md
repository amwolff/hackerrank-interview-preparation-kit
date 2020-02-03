# Greedy Algorithms

## [Minimum Absolute Difference in an Array](https://www.hackerrank.com/challenges/minimum-absolute-difference-in-an-array/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=greedy-algorithms)

```go
func abs(v int32) int32 {
    if v < 0 {
        return v * -1
    }
    return v
}

func minimumAbsoluteDifference(arr []int32) int32 {
    sort.Slice(arr, func(i int, j int) bool {
        if arr[i] > arr[j] {
            return true
        }
        return false
    })
    min := abs(arr[0] - arr[1])
    for i := 1; i < len(arr) - 1; i++ {
        if cur := abs(arr[i] - arr[i + 1]); cur < min {
            min = cur
        }
    }
    return min
}
```

## [Luck Balance](https://www.hackerrank.com/challenges/luck-balance/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=greedy-algorithms)

```go
func luckBalance(k int32, contests [][]int32) int32 {
    sort.Slice(contests, func(i int, j int) bool {
        if contests[i][0] < contests[j][0] {
            return true
        }
        return false
    })
    var num int32
    for _, v := range contests {
        if v[1] == 1 {
            num++
        }
    }
    toWin := num - k
    var sum int32
    var com int32
    for _, v := range contests {
        if toWin > 0 && v[1] == 1 {
            com += v[0]
            toWin--
            continue
        }
        sum += v[0]
    }
    return sum - com
}
```

## [Greedy Florist](https://www.hackerrank.com/challenges/greedy-florist/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=greedy-algorithms)

```go
func getMinimumCost(k int32, c []int32) int32 {
    if k >= int32(len(c)) { // fast path
        var sum int32
        for _, v := range c {
            sum += v
        }
        return sum
    }
    sort.Slice(c, func(i, j int) bool {
        return c[i] > c[j]
    })
    var sum int32
    var idx int32
    for ; idx < k; idx++ {
        sum += c[idx]
    }

    var nPurchases int32
    var iterations int32
    for i := idx; i < int32(len(c)); i++ {
        if iterations%k == 0 {
            nPurchases++
        }
        sum += (nPurchases + 1) * c[i]
        iterations++
    }

    return sum
}
```

## [Max Min](https://www.hackerrank.com/challenges/angry-children/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=greedy-algorithms)

```go
func maxMin(k int32, arr []int32) int32 {
    sort.Slice(arr, func(i, j int) bool {
        return arr[i] < arr[j]
    })
    var unfairness int32 = math.MaxInt32
    for i, j := 0, k - 1; j < int32(len(arr)); i, j = i + 1, j + 1 {
        if v := arr[j] - arr[i]; v < unfairness {
            unfairness = v
        }
    }
    return unfairness
}
```
