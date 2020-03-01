# Stacks and Queues

## [Balanced Brackets](https://www.hackerrank.com/challenges/balanced-brackets/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=stacks-queues)

```go
func isBalanced(s string) string {
    // purely academic approach. :)
    var st []rune
    for _, r := range s {
        switch r {
            case '(', '{', '[':
                st = append(st, r)
            case ')':
                if len(st) == 0 {
                    return "NO"
                }
                var l rune
                l, st = st[len(st)-1], st[:len(st)-1]
                if l != '(' {
                    return "NO"
                }
            case '}':
                if len(st) == 0 {
                    return "NO"
                }
                var l rune
                l, st = st[len(st)-1], st[:len(st)-1]
                if l != '{' {
                    return "NO"
                }
            case ']':
                if len(st) == 0 {
                    return "NO"
                }
                var l rune
                l, st = st[len(st)-1], st[:len(st)-1]
                if l != '[' {
                    return "NO"
                }
            default:
                return "NO"
        }
    }
    if len(st) > 0 {
        return "NO"
    }
    return "YES"
}
```

## [Queues: A Tale of Two Stacks](https://www.hackerrank.com/challenges/ctci-queue-using-two-stacks/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=stacks-queues)

```go
package main

import (
    "bufio"
    "fmt"
    "os"
    "strconv"
    "strings"
)

type fifo []int

func (f fifo) empty() bool {
    return len(f) == 0
}

func (f *fifo) push(x int) {
    *f = append(*f, x)
}

func (f *fifo) pop() (int, bool) {
    if !f.empty() {
        var x int
        x, *f = (*f)[0], (*f)[1:]
        return x, true
    }
    return 0, false
}

func (f fifo) peek() (int, bool) {
    if !f.empty() {
        return f[0], true
    }
    return 0, false
}

type lifo []int

func (l lifo) empty() bool {
    return len(l) == 0
}

func (l *lifo) push(x int) {
    *l = append(*l, x)
}

func (l *lifo) pop() (int, bool) {
    if !l.empty() {
        var x int
        x, *l = (*l)[len(*l)-1], (*l)[:len(*l)-1]
        return x, true
    }
    return 0, false
}

func (l lifo) peek() (int, bool) {
    if !l.empty() {
        return l[len(l)-1], true
    }
    return 0, false
}

func main() {
    scanner := bufio.NewScanner(os.Stdin)

    var stack lifo
    var queue fifo

    var i int
    for scanner.Scan() {
        params := strings.Split(scanner.Text(), " ")
        if i == 0 {
            _, err := strconv.Atoi(params[0])
            if err != nil {
                panic("Atoi: " + err.Error())
            }
            i++
            continue
        }
        switch len(params) {
        case 2:
            x, err := strconv.Atoi(params[1])
            if err != nil {
                panic("Atoi: " + err.Error())
            }
            stack.push(x)
            queue.push(x)
        case 1:
            x, err := strconv.Atoi(params[0])
            if err != nil {
                panic("Atoi: " + err.Error())
            }
            switch x {
            case 2:
                stack.pop()
                queue.pop()
            case 3:
                if a, ok := queue.peek(); ok {
                    fmt.Println(a)
                }
            }
        }
    }
    if err := scanner.Err(); err != nil {
        panic("Scan: " + err.Error())
    }
}
```

### Disclaimer

After reading CTCI (this is the challenge from this book) I realized this solution is obviously wrong (or rather not the intended one).

## [Largest Rectangle](https://www.hackerrank.com/challenges/largest-rectangle/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=stacks-queues)

```go
func max(a, b, c int64) int64 {
    if a > b {
        if a > c {
            return a
        }
        return c
    }
    if b > c {
        return b
    }
    return c
}

func largestRectangle(h []int32) int64 {
    if len(h) == 0 {
        return 0
    }
    min := int32(10e6)
    idx := 0
    for i, v := range h {
        if v < min {
            min = v
            idx = i
        }
    }
    lMax := largestRectangle(h[:idx])
    rMax := largestRectangle(h[idx+1:])
    bMax := int64(min) * int64(len(h))
    return max(lMax, rMax, bMax)
}
```
