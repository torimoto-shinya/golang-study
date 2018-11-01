# golang-study

## 勉強メモ
[A Tour of Go](A%20Tour%20of%20Go)
[How to Write Go Code](How%20to%20Write%20Go%20Code)
[Effective Go](Effective%20Go)

## ソースコード
1. [Hello World](https://github.com/torimoto-shinya/hello)
1. [stringutil](https://github.com/torimoto-shinya/stringutil)
  * Testing Sample -> https://golang.org/doc/code.html#Testing
1. [dlroW olleH](https://github.com/torimoto-shinya/olleh)
  * 上記stringutilパッケージを実際に使ってみたサンプル
1. [Web Crawler](https://github.com/torimoto-shinya/web-crawler)
  * A Tour of Go Exercise -> https://tour.golang.org/concurrency/10

## 各ソースの実行方法
### 前提条件
* Goをインストール済み
* GOPATHはデフォルトのまま
  * `$HOME/go` on Unix, `$home/go` on Plan 9, and `%USERPROFILE%\go` (usually C:\Users\YourName\go) on Windows
* 環境変数 `PATH` に `GOPATH/bin` を通す

### 注意
* Windowsの場合は、`$(go env GOPATH)` の部分を `%GOPATH%` にして、ディレクトリの文字列になる部分をダブルクォーテーションで囲う
  * 例： `mkdir $(go env GOPATH)/src/github.com/torimoto-shinya` -> `mkdir "%GOPATH%/src/github.com/torimoto-shinya"`

### 1. Hello World
#### Clone
```
$ mkdir $(go env GOPATH)/src/github.com/torimoto-shinya
$ cd $(go env GOPATH)/src/github.com/torimoto-shinya
$ git clone git@github.com:torimoto-shinya/hello.git
```

#### install
```
$ cd $(go env GOPATH)/src/github.com/torimoto-shinya/hello
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

#### clone
```
$ mkdir $(go env GOPATH)/src/github.com/torimoto-shinya
$ cd $(go env GOPATH)/src/github.com/torimoto-shinya
$ git clone git@github.com:torimoto-shinya/stringutil.git
```

#### Testing
```
$ cd $(go env GOPATH)/src/github.com/torimoto-shinya/stringutil
$ go test
PASS
ok      github.com/torimoto-shinya/stringutil   0.078s
```

### 3. stringutilをパッケージとして使う
* 上記Testingまで終わっている前提

#### build
```
$ cd $(go env GOPATH)/src/github.com/torimoto-shinya/stringutil
$ go build
```

#### clone and install
```
# clone
$ cd $(go env GOPATH)/src/github.com/torimoto-shinya
$ git clone git@github.com:torimoto-shinya/olleh.git

# install
$ cd $(go env GOPATH)/src/github.com/torimoto-shinya/olleh
$ go install
```

#### Run Result
```
$ olleh
!dlroW olleH
```

### 4. Web Crawler
#### clone
```
$ mkdir $(go env GOPATH)/src/github.com/torimoto-shinya
$ cd $(go env GOPATH)/src/github.com/torimoto-shinya
$ git clone git@github.com:torimoto-shinya/web-crawler.git
```

#### install
```
$ cd $(go env GOPATH)/src/github.com/torimoto-shinya/web-crawler
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
