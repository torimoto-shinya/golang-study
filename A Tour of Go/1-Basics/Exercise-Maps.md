# Exercise: Maps

## URL
https://tour.golang.org/moretypes/23

## Answer
```
package main

import (
	"golang.org/x/tour/wc"
	"strings"
	"fmt"
)

func WordCount(s string) map[string]int {
	fmt.Println(strings.Fields(s))
	ret := make(map[string]int)
	for _, word := range strings.Fields(s) {
		ret[word]++
	}
	return ret
}

func main() {
	wc.Test(WordCount)
}
```
