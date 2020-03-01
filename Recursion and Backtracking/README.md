# Recursion and Backtracking

## [Recursion: Fibonacci Numbers](https://www.hackerrank.com/challenges/ctci-fibonacci-numbers/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=recursion-backtracking)

```go
func fibonacci(n int) int {
    switch n {
        case 0: return 0
        case 1: return 1
        default: return fibonacci(n - 1) + fibonacci(n - 2)
    }
}
```
