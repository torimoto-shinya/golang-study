# Exercise: Images

## URL
https://tour.golang.org/methods/25

## Answer
```
package main

import (
	"golang.org/x/tour/pic"
	"image"
	"image/color"
)

type Image struct{}

func (img Image) ColorModel() color.Model {
	return color.RGBAModel
}

func (img Image) Bounds() image.Rectangle {
	dx := 256
	dy := 256
	return image.Rect(0, 0, dx, dy)
}

func (img Image) At(x, y int) color.Color {
	return color.RGBA{uint8(x), uint8(y), 255, 255}
}

func main() {
	m := Image{}
	pic.ShowImage(m)
}

```
