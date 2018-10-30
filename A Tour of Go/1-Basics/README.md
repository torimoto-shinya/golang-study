# Basics
## URL
https://go-tour-jp.appspot.com/basics/1

## Packages, variables, and functions.

### Hello World!!
```
package main

import (
    "fmt"
)

func main() {
    fmt.Printf("Hello World!!")
}
```

### ポイント
* Goのプログラムは、パッケージ（package）で構成される
* プログラムは `main` のパッケージから開始される
* `import` で別のパッケージをインポートできる
* パッケージ名はインポートパスの最後の要素と同じ名前にしなければならない（規約）
* 括弧でパッケージのインポートをグループ化している状態を `factored import statement` という
* パッケージがエクスポートする名前を参照するときは、大文字でなければならない（例： `math.Pi` ）
* 関数の引数には型を書く。複数の引数の型が同じ場合は省略する書き方がある（例： `x, y int`）
* 関数は複数の戻り値も返すことができる
* 変数の宣言方法
  * `var` ステートメント
    * 宣言と同時に初期化子を与えることもできる（型宣言が省略できる）
    * `factored`できる（括弧でまとめられる）
  * `:=`でも型宣言を省略できる（関数内限定）
  * `const` ステートメント
    * `:=` は使えない
    * 文字(character)、文字列(string)、boolean、数値(numeric)のみで使える
    * 数値は高精度な値である
* 変数 `v` 、型 `T` があった場合、 `T(v)` は、変数 `v` を `T` 型へ変換する

### 疑問
* "naked" returnって "named" returnの間違い？（https://go-tour-jp.appspot.com/basics/7）
* 型の種類って全部覚える必要はあるか？（https://go-tour-jp.appspot.com/basics/11）
* 変数の型宣言はどこまで省略するか？ケースバイケース？

## Flow control statements（制御構文）
### For
```
package main

import "fmt"

func main() {
    sum := 0
    for i := 0; i < 10; i++ {
        sum += i
    }
    fmt.Println(sum)
}
```
* 初期化と後処理ステートメントは省略できる
```
package main

import "fmt"

func main() {
    sum := 1
    for ; sum < 1000; {
        sum += sum
    }
    fmt.Println(sum)
}
```

### While
* Forの構文で書ける（セミコロンを全部省略すればWhileと同義）
```
package main

import "fmt"

func main() {
    sum := 1
    for sum < 1000 {
        sum += sum
    }
    fmt.Println(sum)
}
```

### Forever
* ループの条件をすべて省略すれば無限ループになる
```
package main

func main() {
    for {
    }
}
```

### If, Else
* ()は不要で{}だけでよい
```
package main

import (
    "fmt"
)

func main() {
    x := 100
    fmt.Println(x)
    if x > 50 {
        fmt.Println("50より大きいです")
    } else {
        fmt.Println("50より小さいです")
    }
}
```

### If with a short statement
* ifの条件式の前に簡単なステートメントを記述可能
  * 変数はifのスコープ内のみ有効

```
package main

import (
    "fmt"
    "math"
)

func pow(x, n, lim float64) float64 {
    if v := math.Pow(x, n); v < lim {
        return v
    }
    return lim
}

func main() {
    fmt.Println(
        pow(3, 2, 10),
        pow(3, 3, 20),
    )
}
```
### Switch
* これも評価式の前にセミコロンで簡単なステートメントを書ける
* `break` は書かずとも勝手にやってくれる

```
package main

import (
    "fmt"
    "runtime"
)

func main() {
    fmt.Print("Go runs on ")
    switch os := runtime.GOOS; os {
        case "darwin":
            fmt.Println("OS X.")
        case "linux":
            fmt.Println("Linux")
        default:
            fmt.Printf("%s.", os)
    }
}
```

* `if-then-else` が冗長になりそうなときは `switch` の条件を無くしたもので記述するとシンプルに書ける

```
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
}
```

### Defer
* `defer` へ渡した関数の実行を、呼び出し元の関数の終わり(returnする)まで遅延させる
* `defer` の実行がスタックされた場合、 LIFO(last-in-first-out) の順番で実行される

```
package main

import "fmt"

func main() {
    defer fmt.Println("world")
    fmt.Println("hello")
}
```


## More types

### Pointers

* Goではポインタを扱える
```
package main

import "fmt"

func main() {
	var p *int
	i := 42

	p = &i          // i へのポインタを渡す
	fmt.Println(*p) // ポインタを通して i の値を読む
	*p = 21         // i の値をポインタを通して変更
	fmt.Println(i)  // i の値が変わっている
	fmt.Println(p)  // ポインタを出力すると、メモリアドレスが見える
}
```

### Struct（構造体）
* フィールドの集まり
* structのフィールドにはどっと（.）を用いてアクセス
```
package main

import "fmt"

type Vertex struct {
    X int
    Y int
}

func main() {
    v := Vertex{1, 2}
    fmt.Println(v)
    fmt.Println(v.X)
}
```

* structのフィールドはstructのポインタを通じてアクセスできる
```
package main

import "fmt"

type Vertex struct {
    X int
    Y int
}

func main() {
    v1 := Vertex{1, 2}
    v2 := Vertex{X: 1}  //Xのみの初期化もできる
    p := &v1
    p.X = 3
    fmt.Println(v1, v2)
}

```

### Arrays
* `[n]T` 型は、型 `T` の `n` 個の変数の配列( `array` )を表す

```
package main

import "fmt"

func main() {
    var a [2]string
    a[0] = "Hello"
    a[1] = "World"
    fmt.Println(a[0], a[1])

    primes := [6]int{2, 3, 4, 56, 123, 2}
    fmt.Println(primes)
}
```

### Slices
* 配列は固定長。スライスは可変長

```
package main

import "fmt"

func main() {
	primes := [6]int{2, 3, 5, 7, 11, 13}

	var s []int = primes[1:4]
	fmt.Println(s)
}
```

* スライスは配列への参照のようなもの
* 必ず元となる配列が存在し、その部分列を指し示している
* なので、スライスの要素を変更すると、その元となる配列の対応する要素が変更される

```
package main

import "fmt"

func main() {
	names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
	fmt.Println(names)

    // スライスを定義
	a := names[0:2]
	b := names[1:3]
	fmt.Println(a, b)

    // スライスの一部を変えてみる
    b[0] = "Tori"

    // bしか変えてないのに、元の配列とaも変更されている
	fmt.Println(a, b)
	fmt.Println(names)
}
```

* `var a [10]int` において、以下のスライスは等価である

```
a[0:10]
a[:10]
a[0:]
a[:]
```

* スライスは長さ（length）と容量（capacity）を持つ
```
package main

import "fmt"

func main() {
    a := []int{1, 2, 3, 4, 5, 6}
    fmt.Printf("len=%d cap=%d %v\n", len(a), cap(a), a)
}
```

* スライスのゼロ値は `nil`

```
package main

import "fmt"

func main() {
    var s []int
    fmt.Println(s, len(s), cap(s))
    if s == nil {
        fmt.Println("nil!")
    }
}
```

* スライスは `make()` でも作れる。
* `make([]int, length, [capacity])`

```
package main

import "fmt"

func main() {
    a := make([]int, 5)
    b := make([]int, 0, 5)

    fmt.Printf("%s len=%d cap=%d %v\n", len(a), cap(a), a)
    fmt.Printf("%s len=%d cap=%d %v\n", len(b), cap(b), b)
}
```

* スライスはネストできる

```
package main

import (
    "fmt"
    "strings"
)

func main() {
    // Create a tic-tac-toe board.
	board := [][]string{
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
	}

	// The players take turns.
	board[0][0] = "X"
	board[2][2] = "O"
	board[1][2] = "X"
	board[1][0] = "O"
	board[0][2] = "X"

    for i := 0; i < len(board); i++ {
		fmt.Printf("%s\n", strings.Join(board[i], " "))
	}
}

```

* スライスのappend

```
package main

import "fmt"

func main() {
    var s []int
    fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)

    s = append(s, 0)
    fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s) // サイズが足りない時は、capが増える

    s = append(s, 1, 2, 4, 5)
    fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s) // capは倍々で増えていく
}
```

### Range
* forで使う
* スライスやマップを一つずつ反復処理できる

```
package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
	for i, v := range pow {  // iはインデックス、vはインデックスの場所の要素のコピー
		fmt.Printf("2**%d = %d\n", i, v)
	}
}
```

* インデックスや値は、`_` へ代入することで捨てることができる

```
package main

import "fmt"

func main() {
	pow := make([]int, 10)
	for i := range pow {  // インデックスだけ必要な場合
		pow[i] = 1 << uint(i) // == 2**i
	}
	for _, value := range pow {
		fmt.Printf("%d\n", value)
	}
}
```

### Maps
* キーと値を関連付け

```
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["Bell Labs"])
}
```

* mapの操作

```
package main

import "fmt"

func main() {
	m := make(map[string]int)

	m["Answer"] = 42 // 挿入
	fmt.Println("The value:", m["Answer"])　// 参照

	m["Answer"] = 48
	fmt.Println("The value:", m["Answer"])

	delete(m, "Answer")　// 消去
	fmt.Println("The value:", m["Answer"])

	v, ok := m["Answer"]　// 2つめの変数は存在確認
	fmt.Println("The value:", v, "Present?", ok)
}
```

### Function values
* 関数も変数扱いできる

```
package main

import (
	"fmt"
	"math"
)

func compute(fn func(float64, float64) float64) float64 {
	return fn(3, 4)
}

func main() {
	hypot := func(x, y float64) float64 {
		return math.Sqrt(x*x + y*y)
	}
	fmt.Println(hypot(5, 12))
	fmt.Println(math.Sqrt(5*5 + 12*12) // 上の式と同義

	fmt.Println(compute(math.Pow))
	fmt.Println(math.Pow(3, 4))   // 上の式と同義
}
```

* Goの関数はクロージャである。

### 疑問
* ポインタの使いどころ
* len()やcap()などは、どの名前空間から参照しているのか？（組み込み関数だから気にしなくて良い？）
