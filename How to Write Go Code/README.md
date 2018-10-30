# How to Write Go Code
## URL
* https://golang.org/doc/code.html

## Note
* Windowsで試したため、実行ファイルに `.exe` がついていたり、パスが `\` で区切られていたりしている。

## introduction
このドキュメントでは、単純なGoパッケージの開発方法を紹介し、Goパッケージとコマンドをフェッチ、ビルド、およびインストールするための標準的な方法であるgoツールについて説明します。（本文抜粋）

## Code organization
### 概要
* 単一のワークスペース
* ワークスペースには多くのバージョン管理リポジトリが含まれる（例えば、Gitなどで管理されているもの）
* 各リポジトリには一つ以上のパッケージが含まれている
* 各パッケージは1つまたは複数のGoソースファイルにて構成されている
* パッケージのディレクトリへのパスによって、インポートするパスが決まる

### ワークスペース
* ディレクトリ構成は以下の通り
```
go  # GOPATHで設定したフォルダ
├─bin  # 実行ファイルが格納されている(環境変数PATHに設定すべき)
│      hello.exe
│
└─src  # Goのソースファイルが格納されている
    └─github.com
        └─torimoto-shinya
            ├─hello
            │      hello.go
            │
            └─stringutil
                    reverse.go
```

### go install
* コマンドを実行すると、実行ファイルを生成し、 `bin` ディレクトリに格納される

### go build
* `go build` はファイルを生成せず、コンパイルされたパッケージをローカルビルドキャッシュに保存する。

### パッケージ名
* パッケージ名は `package anyname` となる
* 実行ファイルになるものは `package main` としなければならない

### テスト
* `go test` コマンドと `testing` パッケージで構成される軽量なテストフレームワークがある
* パッケージと同一ディレクトリに、 `packagename_test.go` を配置する
* テストを実行するファイルの名前に規約はあるのか？
  * パッケージ名と同一にする必要があるっぽい。別名にすると以下のように怒られた
  ```
  $ go test .\src\github.com\torimoto-shinya\stringutil
  ?       github.com/torimoto-shinya/stringutil   [no test files]
  ```

### リモートのパッケージ
* `go get [path/to/package]` で取得できる
* 依存関係のあるパッケージも同時に取得できる
