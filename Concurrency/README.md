# Concurrency


### バイナリツリー演習問題
```
package main

import (
	"golang.org/x/tour/tree"
	"fmt"
)

// Walk walks the tree t sending all values
// from the tree to the channel ch.
func Walk(t *tree.Tree, ch chan int) {
	var f func(t *tree.Tree, ch chan int)
	f = func(t *tree.Tree, ch chan int)  {
		if t == nil {
			return
		} else {
			// fmt.Printf("t: %v, t.Value: %v \n", t, t.Value)
		}
		// fmt.Println("Left!")
		f(t.Left, ch)
		// fmt.Println("chennel send")
		ch <- t.Value
		// fmt.Println("Right!")
		f(t.Right, ch)
	}   // 再帰処理用関数

	f(t, ch)
	// fmt.Println("close!")
	close(ch)
}

// Same determines whether the trees
// t1 and t2 contain the same values.
func Same(t1, t2 *tree.Tree) bool {
    ch1 := make(chan int)
    ch2 := make(chan int)
    go Walk(t1, ch1)
    go Walk(t2, ch2)
    for i := range ch1 {
        if i != <-ch2 {
            return false
        }
    }
    return true
}


func main() {
	ch := make(chan int)
	t := tree.New(1)

	go Walk(t, ch)

	for i := range ch {
		fmt.Println(i)
	}

	fmt.Println(Same(tree.New(1), tree.New(1)))
	fmt.Println(Same(tree.New(1), tree.New(2)))
}
```
