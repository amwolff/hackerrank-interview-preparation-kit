# Dynamic Programming

## [Max Array Sum](https://www.hackerrank.com/challenges/max-array-sum/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=dynamic-programming)

```go
func maxSubsetSum(arr []int32) int32 {
    aux := make([]int32, len(arr))
    var max int32
    for i, v := range arr {
        if i - 2 >= 0 {
            if aux[i - 2] > max {
                max = aux[i - 2]
            }
            if v + max > max {
                aux[i] = v + max
            } else {
                aux[i] = v
            }
        } else {
            aux[i] = v
        }
    }
    var ret int32
    for _, v := range aux {
        if v > ret {
            ret = v
        }
    }
    return ret
}
```

## [Abbreviation](https://www.hackerrank.com/challenges/abbr/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=dynamic-programming)

```go
func max(v1, v2 int) int {
    if v1 > v2 {
        return v1
    }
    return v2
}

func abbreviationMemo(a, b string, cache [][]int) int {
    i, j := len(a), len(b)

    if i == 0 && j == 0 {
        return 1
    }

    if i < j || (j == 0 && unicode.IsUpper(rune(a[i-1]))) {
        return 0
    }

    if j == 0 && unicode.IsLower(rune(a[i-1])) {
        return abbreviationMemo(a[:i-1], b, cache)
    }

    if cache[i-1][j-1] != -1 {
        return cache[i-1][j-1]
    }

    if a[i-1] == b[j-1] {
        cache[i-1][j-1] = abbreviationMemo(a[:i-1], b[:j-1], cache)
        return cache[i-1][j-1]
    }

    if unicode.IsUpper(rune(a[i-1])) && a[i-1] != b[j-1] {
        cache[i-1][j-1] = 0
        return cache[i-1][j-1]
    }

    lastUpp := fmt.Sprintf("%s%c", a[:i-1], unicode.ToUpper(rune(a[i-1])))
    branch1 := abbreviationMemo(lastUpp, b, cache)
    branch2 := abbreviationMemo(a[:i-1], b, cache)
    cache[i-1][j-1] = max(branch1, branch2)
    return cache[i-1][j-1]
}

func abbreviation(a string, b string) string {
    cache := make([][]int, len(a))
    for i := range cache {
        cache[i] = make([]int, len(b))
        for j := range cache[i] {
            cache[i][j] = -1
        }
    }
    if ans := abbreviationMemo(a, b, cache); ans > 0 {
        return "YES"
    }
    return "NO"
}
```

## [Candies](https://www.hackerrank.com/challenges/candies/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=dynamic-programming)

```go
func candies(n int32, arr []int32) int64 {
    distribution := make([]int32, n)
    for i := range distribution {
        distribution[i] = 1
    }

    for i := int32(0); i < n; i++ {
        if i > 0 {
            if arr[i] > arr[i-1] {
                distribution[i] = distribution[i-1] + 1
            }
        }
    }

    reinforcement := make([]int32, n)
    copy(reinforcement, distribution)

    for i := n - 1; i > 0; i-- {
        if arr[i-1] > arr[i] {
            if distribution[i-1] <= reinforcement[i] {
                reinforcement[i-1] = reinforcement[i] + 1
            }
        }
    }

    var ret int64
    for _, c := range reinforcement {
        ret += int64(c)
    }
    return ret
}
```
