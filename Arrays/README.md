# Arrays

## [2D Array - DS](https://www.hackerrank.com/challenges/2d-array/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=arrays)

```go
func hourglassSum(arr [][]int32) int32 {
    var max int32
    unassigned := true
    for i := 0; i < 4; i++ {
        for j := 0; j < 4; j++ {
            sum := arr[i][j] + arr[i][j+1] + arr[i][j+2] +
                    arr[i+1][j+1] +
                    arr[i+2][j] + arr[i+2][j+1] + arr[i+2][j+2]
            if sum > max || unassigned {
                unassigned = false
                max = sum
            }
        }
    }
    return max
}
```

## [Arrays: Left Rotation](https://www.hackerrank.com/challenges/ctci-array-left-rotation/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=arrays)

```go
func rotLeft(a []int32, d int32) []int32 {
    l := int32(len(a))
    n := d % l
    var aux []int32
    for i := int32(0); i < n; i++ {
        aux = append(aux, a[i])
    }
    var ret []int32
    for i := n; i < l; i++ {
        ret = append(ret, a[i])
    }
    for _, a := range aux {
        ret = append(ret, a)
    }
    return ret
}
```

## [New Year Chaos](https://www.hackerrank.com/challenges/new-year-chaos/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=arrays)

```go
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func minimumBribes(q []int32) {
    var steps int
    qLen := len(q)
    for i := 0; i < qLen; i++ {
        if q[i] == int32(i)+1 {
            continue
        }
        d := int(q[i]) - (i + 1)
        if d > 2 {
            fmt.Println("Too chaotic")
            return
        } else if d > 0 {
            for j := 0; j < d; j++ {
                if i+j+1 < qLen && q[i+j] > q[i+j+1] {
                    q[i+j], q[i+j+1] = q[i+j+1], q[i+j]
                    steps++
                } else {
                    break
                }
            }
        } else if d < 0 {
            for j := 0; j < d*-1; j++ {
                if i-j-1 >= 0 && q[i-j] < q[i-j-1] {
                    q[i-j], q[i-j-1] = q[i-j-1], q[i-j]
                    steps++
                } else {
                    break
                }
            }
        }
        if (i-1 >= 0 && q[i-1] > q[i]) || (i+1 < qLen && q[i] > q[i+1]) {
            i = max(i-max(d, 0), 0)
        }
    }
    fmt.Println(steps)
}
```

## [Minimum Swaps 2](https://www.hackerrank.com/challenges/minimum-swaps-2/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=arrays)

```go
func minimumSwaps(arr []int32) int32 {
    var swaps int32
    for i := 0; i < len(arr); i++ {
        if arr[i] == int32(i)+1 { // fast path
            continue
        }
        arr[i], arr[arr[i]-1] = arr[arr[i]-1], arr[i]
        swaps++
        i--
    }
    return swaps
}
```
