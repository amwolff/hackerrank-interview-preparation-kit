# Search

## [Hash Tables: Ice Cream Parlor](https://www.hackerrank.com/challenges/ctci-ice-cream-parlor/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=search)

```go
func whatFlavors(cost []int32, money int32) {
    m := make(map[int32]int)
    for i, v := range cost {
        m[v] = i
    }
    for i, v := range cost {
        if v < money {
            if idx, ok := m[money-v]; ok && idx != i {
                if idx < i {
                    fmt.Printf("%d %d\n", idx+1, i+1)
                    return
                }
                fmt.Printf("%d %d\n", i+1, idx+1)
                return
            }
        }
    }
}
```

## [Swap Nodes [Algo]](https://www.hackerrank.com/challenges/swap-nodes-algo/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=search)

```go
func preprocess(indexes [][]int32, i, q, lvl int32) {
    if (lvl-1)%q == 0 {
        indexes[i][0], indexes[i][1] = indexes[i][1], indexes[i][0]
    }
    if indexes[i][0] != -1 {
        preprocess(indexes, indexes[i][0]-1, q, lvl+1)
    }
    if indexes[i][1] != -1 {
        preprocess(indexes, indexes[i][1]-1, q, lvl+1)
    }
}

func inorder(indexes [][]int32, i int32, o []int32) []int32 {
    if indexes[i][0] != -1 {
        o = inorder(indexes, indexes[i][0]-1, o)
    }
    o = append(o, i+1)
    if indexes[i][1] != -1 {
        o = inorder(indexes, indexes[i][1]-1, o)
    }
    return o
}

func swapNodes(indexes [][]int32, queries []int32) [][]int32 {
    ret := make([][]int32, len(queries))
    for i, q := range queries {
        preprocess(indexes, 0, q, 2)
        ret[i] = inorder(indexes, 0, []int32{})
    }
    return ret
}
```

## [Pairs](https://www.hackerrank.com/challenges/pairs/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=search)

```go
func pairs(k int32, arr []int32) int32 {
    aux := make(map[int32]int)
    for i, v := range arr {
        aux[v] = i
    }
    var pairs int32
    for i, v := range arr {
        if idx, ok := aux[v - k]; ok && i != idx {
            pairs++
        }
    }
    return pairs
}
```

## [Triple sum](https://www.hackerrank.com/challenges/triple-sum/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=search)

```go
func deduplicate(a []int32) []int32 {
    var ret []int32

    m := make(map[int32]struct{})
    for _, v := range a {
        if _, ok := m[v]; ok {
            continue
        }

        ret = append(ret, v)

        m[v] = struct{}{}
    }

    return ret
}

func triplets(a []int32, b []int32, c []int32) int64 {
    dedupA := deduplicate(a)
    dedupB := deduplicate(b)
    dedupC := deduplicate(c)

    sort.Slice(dedupA, func(i, j int) bool { return dedupA[i] < dedupA[j] })
    sort.Slice(dedupC, func(i, j int) bool { return dedupC[i] < dedupC[j] })

    var count int64
    for _, q := range dedupB {
        aCnt := sort.Search(len(dedupA), func(i int) bool { return dedupA[i] > q })
        cCnt := sort.Search(len(dedupC), func(i int) bool { return dedupC[i] > q })
        count += int64(aCnt * cCnt)
    }
    return count
}
```

## [Minimum Time Required](https://www.hackerrank.com/challenges/minimum-time-required/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=search)

```go
func minTime(machines []int64, goal int64) int64 {
    min := int64(10e9)
    max := int64(0)
    for _, v := range machines {
        if v < min {
            min = v
        }
        if v > max {
            max = v
        }
    }

    p := goal / int64(len(machines))

    if p < 1 {
        p = 1
    }

    l, r := p*min, p*max
    for l < r {
        mid := (l + r) / 2
        var count int64
        for _, m := range machines {
            count += mid / m
        }
        if count < goal {
            l, r = mid+1, r
        } else if count >= goal {
            l, r = l, mid
        }
    }
    return l
}
```
