# Effective Go

## URL
* https://golang.org/doc/effective_go.html

## Introduction
* Goらしいコードを書くためのティップスを学ぶ。そうすることで、他のGoプログラマが簡単にあなたのプログラムを読むことができるようになる
* このドキュメントは言語仕様と、チュートリアルの知識を補完するもの。

## Formatting
* `go fmt` でコードのフォーマットを修正してもらえる

## Commentary
* `/* any comment */` もしくは `// any comment` でコメントを記述できる
* パッケージの一番上にあるコメントは、godocで出力されるドキュメントの説明文となる
* コメントの頭に、記述する「項目名」を書くようにすると、 `godoc 項目名 | grep -i parse` が便利になる

## Names
### Getter and Setter
* 例えばフィールド名が"owner"の場合、以下のように命名する
  * ゲッター： Owner()
  * セッター： SetOwner()

### Interface Name
* 慣例では `-er` をつける
  * 例： Reader, Writer, Formatter, CloseNotifier etc
* Goの標準的な特徴と意味を持つメソッドを実装するときは、名前も標準的な方に合わせる。
  * Read, Write, Close, Flush, String etc

### MixedCaps
* 命名にアンダースコアは使わない（MixedCaps, mixedCaps）

## Semicolons
* セミコロンは分離記号

## Control structures
### Redeclaration and reassignment(再宣言と再割り当て)
* `err` が再宣言されているが、これはオッケー
```
f, err := os.Open(name)
if err != nil {
    return
}

d, err := f.Stat()
```

## Functions
* チュートリアルの内容で包含している

## Data
### Allocation with
* new()は、新たに割り当てられた型のゼロ値へのポインタを返す
* 以下のような型宣言がある場合

```
type SyncedBuffer struct {
    lock    sync.Mutex
    buffer  bytes.Buffer
}
```

* 以下のように割り当てられる

```
p := new(SyncedBuffer)  // type *SyncedBuffer
var v SyncedBuffer      // type  SyncedBuffer
```

* make(T)： 初期化された(=ゼロ値でない)T型の値を返す
* スライス・マップ・チャネルは、値が初期化されている必要があるため、 `make` が存在する

```
// 必要以上に複雑
var p *[]int = new([]int)
*p = make([]int, 100, 100)

// 一般的にはこう書く
v := make([]int, 100)
```

### Array and Slice
* Arrayは値型、Sliceは参照型

## Initialization
### init関数

```
func init() {
    if user == "" {
        log.Fatal("$USER not set")
    }
    if home == "" {
        home = "/home/" + user
    }
    if gopath == "" {
        gopath = home + "/go"
    }
    // gopath may be overridden by --gopath flag on command line.
    flag.StringVar(&gopath, "gopath", gopath, "override default GOPATH")
}
```

## Methods
### Pointers vs. Values
* メソッドは、名前がつけられていればポインタとインタフェースを除くすべての型に定義することができる
* 関数内で更新された変数の値を返す時は、値渡しではなくポインタを使うと便利
```
func (p *ByteSlice) Append(data []byte) {
    slice := *p
    // 本体部分は上とおなじだが、returnは除外
    *p = slice
}
```

## Interfaces and other types
### Interfaces
* オブジェクトの振る舞いを決定できる手だて

### Conversions
* 異なるメソッド群を使用するために式の型を変換することがよく行われる

```
type Sequence []int

// Method for printing - sorts the elements before printing
func (s Sequence) String() string {
    sort.IntSlice(s).Sort()
    return fmt.Sprint([]int(s))
}
```



## The blank identifier
* `_` で表現していたやつ
* インポートしたパッケージは使わないとビルドが通らないが、デバッグのために以下のように記述することもある

```
package main

import (
    "fmt"
    "io"
    "log"
    "os"
)

var _ = fmt.Printf // For debugging; delete when done.
var _ io.Reader    // For debugging; delete when done.

func main() {
    fd, err := os.Open("test.go")
    if err != nil {
        log.Fatal(err)
    }
    // TODO: use fd.
    _ = fd
}
```

* 型がインターフェースを実装しているかどうかの時にも使える

```
if _, ok := val.(json.Marshaler); ok {
    fmt.Printf("value %v of type %T implements json.Marshaler\n", val, val)
}

```
## Embedding
* 構造体、またはインターフェースに型を埋め込むことができる

```
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}

// ReadWriter is the interface that combines the Reader and Writer interfaces.
type ReadWriter interface {
    Reader
    Writer
}
```

## Concurrency

### Share by communicating
* Do not communicate by sharing memory; instead, share memory by communicating
  * 共有メモリを使った通信は行わず、代わりに通信を使うことでメモリを共有するようにする

## Error
* Goでは、エラーのインターフェースが用意されている
```
type error interface {
    Error() string
}
```

### panic
* エラーがリカバリできないとき(プログラムは全く継続できない事態に陥る、など)のエラー処理として、実質的にプログラムを停止させるランタイムエラーを作成するpanic関数がある

```
// A toy implementation of cube root using Newton's method.
func CubeRoot(x float64) float64 {
    z := x/3   // Arbitrary initial value
    for i := 0; i < 1e6; i++ {
        prevz := z
        z -= (z*z*z-x) / (3*z*z)
        if veryClose(z, prevz) {
            return z
        }
    }
    // A million iterations has not converged; something is wrong.
    panic(fmt.Sprintf("CubeRoot(%g) did not converge", x))
}
```

### Recover
* panicが呼び出されたときは、すぐさま、カレントの関数を停止し、ゴルーチンのスタックの巻き戻しを開始
* その途中、遅延指定(defer)されている関数をすべて実行
* 組み込み関数recoverを使うことで、ゴルーチンの制御を取り戻し、通常の実行を再開させることが可能
* recoverは遅延指定された関数内のみで使える

```
func server(workChan <-chan *Work) {
    for work := range workChan {
        go safelyDo(work)
    }
}

func safelyDo(work *Work) {
    defer func() {
        if err := recover(); err != nil {
            log.Println("work failed:", err)
        }
    }()
    do(work)
}
```

## A web server

### sample
```
package main

import (
    "flag"
    "html/template"
    "log"
    "net/http"
)

var addr = flag.String("addr", ":1718", "http service address") // Q=17, R=18

var templ = template.Must(template.New("qr").Parse(templateStr))

func main() {
    flag.Parse()
    http.Handle("/", http.HandlerFunc(QR))
    err := http.ListenAndServe(*addr, nil)
    if err != nil {
        log.Fatal("ListenAndServe:", err)
    }
}

func QR(w http.ResponseWriter, req *http.Request) {
    templ.Execute(w, req.FormValue("s"))
}

const templateStr = `
<html>
<head>
<title>QR Link Generator</title>
</head>
<body>
{{if .}}
<img src="http://chart.apis.google.com/chart?chs=300x300&cht=qr&choe=UTF-8&chl={{.}}" />
<br>
{{.}}
<br>
<br>
{{end}}
<form action="/" name=f method="GET"><input maxLength=1024 size=70
name=s value="" title="Text to QR Encode"><input type=submit
value="Show QR" name=qr>
</form>
</body>
</html>
`
```

### flag
* ドキュメント： https://golang.org/pkg/flag/
* コマンドライン引数を扱うことができる

### http
* ドキュメント： https://golang.org/pkg/net/http/
* HTTPクライアントとサーバーの実装を提供できる

### html/template
* ドキュメント： https://golang.org/pkg/html/template/
* HTMLの文字列を解釈し、テンプレートを提供してくれる
