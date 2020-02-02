# Dictionaries and Hashmaps

## Hash Tables: Ransom Note

```go
func checkMagazine(magazine []string, note []string) {
    wordCount := make(map[string]int)
    for _, s := range magazine {
        wordCount[s]++
    }
    for _, s := range note {
        if c, ok := wordCount[s]; ok {
            if c > 0 {
                wordCount[s]--
            } else {
                fmt.Println("No")
                return
            }
        } else {
            fmt.Println("No")
            return
        }
    }
    fmt.Println("Yes")
}
```

## Two Strings

```go
func twoStrings(s1 string, s2 string) string {
    charCnt := make(map[rune]struct{})
    for _, r := range s1 {
        charCnt[r] = struct{}{}
    }
    for _, r := range s2 {
        if _, ok := charCnt[r]; ok {
            return "YES"
        }
    }
    return "NO"
}
```

## Sherlock and Anagrams

```go
func sherlockAndAnagrams(s string) int32 {
    var subs []string
    for i := range s {
        for j := i; j < len(s); j++ {
            subs = append(subs, s[i:(j+1)])
        }
    }
    var pairs int32
    for i, v0 := range subs {
        mV0 := make(map[rune]int)
        for _, v := range v0 {
            mV0[v]++
        }
    ndLoop:
        for _, v1 := range subs[(i + 1):] {
            if len(v0) != len(v1) { // fast path
                continue
            }
            mV1 := make(map[rune]int)
            for _, v := range v1 {
                mV1[v]++
            }
            for k, v := range mV1 {
                if v != mV0[k] {
                    continue ndLoop
                }
            }
            pairs++
        }
    }
    return pairs
}
```

## Count Triplets

```go
func countTriplets(arr []int64, r int64) int64 {
    q1 := make(map[int64]int64)
    q2 := make(map[int64]int64)
    q3 := make(map[int64]int64)

    for _, v := range arr {
        if v%r != 0 {
            q1[v]++
            continue
        }

        q3[v] = q3[v] + q2[v/r]
        q2[v] = q2[v] + q1[v/r]
        q1[v]++
    }

    var sum int64
    for _, v := range q3 {
        sum += v
    }

    return sum
}
```

## Frequency Queries

```go
func remove(s []int32, val int32) []int32 {
	for i, v := range s {
		if v == val {
			s[len(s)-1], s[i] = s[i], s[len(s)-1]
			return s[:len(s)-1]
		}
	}
	return s
}

func freqQuery(queries [][]int32) []int32 {
	var ret []int32
	data := make(map[int32]int)
	freq := make(map[int][]int32)
	for _, q := range queries {
		switch q[0] {
		case 1:
			// Delete current frequency from frequency storage.
			f := data[q[1]]
			e := freq[f]

			e = remove(e, q[1])
			if len(e) == 0 {
				delete(freq, f)
			} else {
				freq[f] = e
			}

			// Add new frequency to frequency storage.
			n := freq[f+1]
			n = append(n, q[1])
			freq[f+1] = n
			data[q[1]]++
		case 2:
			// Delete current frequency from frequency storage.
			f := data[q[1]]
			e := freq[f]

			e = remove(e, q[1])
			if len(e) == 0 {
				delete(freq, f)
			} else {
				freq[f] = e
			}

			// Add new frequency to frequency storage.
			if f-1 <= 0 {
				delete(data, q[1])
			} else {
				n := freq[f-1]
				n = append(n, q[1])
				freq[f-1] = n
				data[q[1]]--
			}
		case 3:
			if len(freq[int(q[1])]) > 0 {
				ret = append(ret, 1)
			} else {
				ret = append(ret, 0)
			}
		}
	}
	return ret
}
```
