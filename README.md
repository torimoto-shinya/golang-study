# golang-study

## 勉強メモ
* [A Tour of Go](A%20Tour%20of%20Go)
* [How to Write Go Code](How%20to%20Write%20Go%20Code)
* [Effective Go](Effective%20Go)

## ソースコード
https://github.com/torimoto-shinya/golang-study-examples

## 各ソースの実行方法
### 前提条件
* Goをインストール済み
* GOPATHはデフォルトのまま
  * `$HOME/go` on Unix, `$home/go` on Plan 9, and `%USERPROFILE%\go` (usually C:\Users\YourName\go) on Windows
* 環境変数 `PATH` に `GOPATH/bin` を通す

### 注意
* Windowsの場合は、`$(go env GOPATH)` の部分を `%GOPATH%` にして、パスの部分をダブルクォーテーションで囲う
  * 例： `mkdir $(go env GOPATH)/src/github.com/torimoto-shinya` -> `mkdir "%GOPATH%/src/github.com/torimoto-shinya"`

### リポジトリのクローン
```
$ mkdir $(go env GOPATH)/src/github.com/torimoto-shinya
$ git clone git@github.com:torimoto-shinya/golang-study-examples.git
```

### 1. Hello World
#### install
```
$ cd $(go env GOPATH)/src/github.com/torimoto-shinya/golang-study-examples/hello
$ go install
```

#### Run Result
* PATHに `GOPATH/bin` を通していれば、以下コマンドで実行可能
```
$ hello
Hello, world.
```

### 2. stringutil

* 簡単なテストのサンプルプログラム

#### Testing
```
$ cd $(go env GOPATH)/src/github.com/torimoto-shinya/golang-study-examples/stringutil
$ go test
PASS
ok      github.com/torimoto-shinya/golang-study-examples/stringutil   0.078s
```

### 3. stringutilをライブラリとして使う

#### build
```
$ cd $(go env GOPATH)/src/github.com/torimoto-shinya/golang-study-examples/stringutil
$ go build
```

#### install
```
# olleh.go は stringutil ライブラリを使っています
$ cd $(go env GOPATH)/src/github.com/torimoto-shinya/golang-study-examples/olleh
$ go install
```

#### Run Result
```
$ olleh
!dlroW olleH
```

### 4. Web Crawler

#### install
```
$ cd $(go env GOPATH)/src/github.com/torimoto-shinya/golang-study-examples/web-crawler
$ go install
```

#### Run Result
```
$ web-crawler
found: https://golang.org/ "The Go Programming Language"
found: https://golang.org/pkg/ "Packages"
found: https://golang.org/pkg/os/ "Package os"
found: https://golang.org/pkg/fmt/ "Package fmt"
not found: https://golang.org/cmd/
```
