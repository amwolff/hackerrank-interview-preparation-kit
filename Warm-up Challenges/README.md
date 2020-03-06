# Warm-up Challenges

## [Sock Merchant](https://www.hackerrank.com/challenges/sock-merchant/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=warmup)

```go
func sockMerchant(n int32, ar []int32) int32 {
    socksPerColor := make(map[int32]int32)
    for _, v := range ar {
        socksPerColor[v]++
    }
    var pairs int32
    for _, v := range socksPerColor {
        if v%2 == 0 {
            pairs += v / 2
        } else {
            c := v - 1
            if c > 0 {
                pairs += c / 2
            }
        }
    }
    return pairs
}
```

## [Counting Valleys](https://www.hackerrank.com/challenges/counting-valleys/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=warmup)

```go
func countingValleys(n int32, s string) int32 {
    // Use a closure here to remember the state.
    var prevAlt int
    var valleys int32
    for _, v := range s {
        c := prevAlt
        switch v {
            case 'U': c++
            case 'D': c--
        }
        if prevAlt < 0 && c >= 0 {
            valleys++
        }
        prevAlt = c
    }
    return valleys
}
```

## [Jumping on the Clouds](https://www.hackerrank.com/challenges/jumping-on-the-clouds/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=warmup)

```go
func countZeroSteps(zeros int32) int32 {
    if zeros == 0 || zeros == 1 {
        return 0
    }
    if zeros%2 == 0 {
        return zeros / 2
    }
    return (zeros - 1) / 2
}

func jumpingOnClouds(c []int32) int32 {
    var steps int32
    var zeros int32
    for _, v := range c {
        switch v {
            case 1:
                steps++
                steps += countZeroSteps(zeros)
                zeros = 0
            case 0:
                zeros++
        }
    }
    steps += countZeroSteps(zeros)
    return steps
}
```

## [Repeated String](https://www.hackerrank.com/challenges/repeated-string/problem?h_l=interview&playlist_slugs%5B%5D=interview-preparation-kit&playlist_slugs%5B%5D=warmup)

```go
func repeatedString(s string, n int64) int64 {
    var cnt int64
    for _, r := range s {
        if r == 'a' {
            cnt++
        }
    }
    l := int64(len(s))
    if cnt == l { // hackish optimization because time limits
        return n
    }
    pie := n / l
    aux := n % l
    ret := pie * cnt
    var i int64
    for ; i < aux; i++ {
        if s[i] == 'a' {
            ret++
        }
    }
    return ret
}
```
