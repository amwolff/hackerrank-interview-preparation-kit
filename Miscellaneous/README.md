# Miscellaneous

## [Time Complexity: Primality](https://www.hackerrank.com/challenges/ctci-big-o/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=miscellaneous)

```go
func primality(n int32) string {
    if n == 1 {
        return "Not prime"
    }
    for i := 2; i <= int(math.Floor(math.Sqrt(float64(n)))); i++ {
        if int(n) % i == 0 {
            return "Not prime"
        }
    }
    return "Prime"
}
```
