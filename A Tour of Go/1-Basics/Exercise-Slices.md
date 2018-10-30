# Exercise: Slices
## URL
https://tour.golang.org/moretypes/18

## Answer
```
package main

import (
	"golang.org/x/tour/pic"
)

func Pic(dx, dy int) [][]uint8 {
    picture := make([][]uint8, dy)
    for y := range picture {
        picture[y] = make([]uint8, dx)
        for x := range picture[y] {
            picture[y][x] = uint8(x*y)
        }
    }
    return picture
}

func main() {
	// dx, dyはShowの中にconstで定義されている
	// https://github.com/golang/tour/blob/master/pic/pic.go#L15

	pic.Show(Pic)
}
```
