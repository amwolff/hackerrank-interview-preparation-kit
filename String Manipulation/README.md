# String Manipulation

## [Strings: Making Anagrams](https://www.hackerrank.com/challenges/ctci-making-anagrams/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=strings)

```go
var letters = []rune{
    'q',
    'w',
    'e',
    'r',
    't',
    'y',
    'u',
    'i',
    'o',
    'p',
    'a',
    's',
    'd',
    'f',
    'g',
    'h',
    'j',
    'k',
    'l',
    'z',
    'x',
    'c',
    'v',
    'b',
    'n',
    'm'}

func abs(v int32) int32 {
    if v < 0 {
        return v * -1
    }
    return v
}

func makeAnagram(a string, b string) int32 {
    mappedA := make(map[rune]int32)
    mappedB := make(map[rune]int32)
    for _, v := range a {
        mappedA[v]++
    }
    for _, v := range b {
        mappedB[v]++
    }
    var cnt int32
    for _, r := range letters {
        cnt += abs(mappedA[r] - mappedB[r])
    }
    return cnt
}
```

## [Alternating Characters](https://www.hackerrank.com/challenges/alternating-characters/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=strings)

```go
func alternatingCharacters(s string) int32 {
    var deletions int32
    prev := ' '
    for _, c := range s {
        if c == prev {
            deletions++
        }
        prev = c
    }
    return deletions
}
```

## [Sherlock and the Valid String](https://www.hackerrank.com/challenges/sherlock-and-valid-string/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=strings)

```go
func abs(v int) int {
    if v < 0 {
        return v * -1
    }
    return v
}

func isValid(s string) string {
    frequencies := make(map[rune]int)
    for _, c := range s {
        frequencies[c]++
    }
    var prev = frequencies[rune(s[0])]
    var used bool
    // fmt.Println(frequencies)
    for _, f := range frequencies {
        // fmt.Printf("f (%d) ?= prev (%d)\n", f, prev)
        if f != prev {
            if (abs(f-prev) == 1 || f == 1) && !used {
                used = true
                continue
            }
            return "NO"
        }
        prev = f
    }
    return "YES"
}
```

## [Special String Again](https://www.hackerrank.com/challenges/special-palindrome-again/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=strings)

```go
func min(a, b int64) int64 {
    if a < b {
        return a
    }
    return b
}

type t struct {
    r rune
    n int64
}

func substrCount(n int32, s string) int64 {
    var prev rune
    var list []t
    for _, r := range s {
        if prev == r {
            list[len(list)-1].n++
        } else {
            list = append(list, t{r, 1})
        }
        prev = r
    }

    var count int64
    for _, e := range list {
        count += (e.n * (e.n + 1)) / 2
    }

    var prevT t
    lenList := len(list)
    for i, e := range list {
        if e.n == 1 && i+1 < lenList && prevT.r == list[i+1].r {
            count += min(prevT.n, list[i+1].n)
        }

        prevT.n = e.n
        prevT.r = e.r
    }

    return count
}
```

## [Common Child](https://www.hackerrank.com/challenges/common-child/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=strings)

```go
func max(v1, v2 int32) int32 {
    if v1 > v2 {
        return v1
    }
    return v2
}

func lcs(s1, s2 string, seqs [][]int32) int32 {
    lenS1, lenS2 := len(s1), len(s2)
    if lenS1 == 0 || lenS2 == 0 {
        return 0
    } else if s1[lenS1-1] == s2[lenS2-1] {
        if seqs[lenS1-1][lenS2-1] == -1 {
            seqs[lenS1-1][lenS2-1] = lcs(s1[:lenS1-1], s2[:lenS2-1], seqs)
        }
        return seqs[lenS1-1][lenS2-1] + 1
    }
    if seqs[lenS1][lenS2-1] == -1 {
        seqs[lenS1][lenS2-1] = lcs(s1[:lenS1], s2[:lenS2-1], seqs)
    }
    if seqs[lenS1-1][lenS2] == -1 {
        seqs[lenS1-1][lenS2] = lcs(s1[:lenS1-1], s2[:lenS2], seqs)
    }
    return max(seqs[lenS1][lenS2-1], seqs[lenS1-1][lenS2])
}

func commonChild(s1 string, s2 string) int32 {
    seqs := make([][]int32, len(s1)+1)
    for i := range seqs {
        seqs[i] = make([]int32, len(s2)+1)
        for j := range seqs[i] {
            seqs[i][j] = -1
        }
    }
    return lcs(s1, s2, seqs)
}
```
