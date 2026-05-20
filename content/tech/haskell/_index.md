---
title: "haskell"
weight: 1
---

# WindowsでHaskellを0から始める

今回は Windows 環境で Haskell を学べるように、環境構築から最初のコード実行までを行った。

---

# 1. Haskell 環境をインストールする

Haskell では現在、`GHCup` を使うのが標準的。

GHCup は：

- GHC（コンパイラ）
- cabal（パッケージ管理）
- HLS（補完機能）
- Stack
- MSYS2

などをまとめて管理してくれる。

公式：

https://www.haskell.org/ghcup/

---

## PowerShell を開く

まず Windows Terminal / PowerShell を開く。

---

## インストールコマンドを実行

以下を PowerShell に貼り付けて実行：

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; `
[System.Net.ServicePointManager]::SecurityProtocol = `
[System.Net.ServicePointManager]::SecurityProtocol -bor 3072; `
& ([scriptblock]::Create((Invoke-WebRequest https://www.haskell.org/ghcup/sh/bootstrap-haskell.ps1).Content))
```

インストール中は基本 Enter を押して進める。

---

# 2. Haskell が使えるか確認する

インストール後、PowerShell で：

```powershell
ghci
```

を実行。

成功すると：

```text
Prelude>
```

が表示される。

---

## GHCi とは

`GHCi` は Haskell の対話実行環境。

Python の REPL に近い。

その場でコードを試せる。

---

# 3. 作業フォルダを決める

今回は：

```text
C:\Users\koizu\training
```

を作業場所にする。

PowerShell で移動：

```powershell
cd "C:\Users\koizu\training"
```

---

## 現在地確認

```powershell
pwd
```

現在いるフォルダが表示される。

---

# 4. 最初の Haskell ファイルを作る

メモ帳で Haskell ファイルを作成：

```powershell
notepad hello.hs
```

ファイルが存在しない場合は「作成しますか？」と聞かれるので「はい」。

---

# 5. 最初の Haskell コードを書く

以下を貼り付け：

```haskell
main :: IO ()
main = putStrLn "Hello, Haskell!"
```

保存してメモ帳を閉じる。

---

# 6. Haskell プログラムを実行する

PowerShell に戻り：

```powershell
runghc hello.hs
```

実行結果：

```text
Hello, Haskell!
```

---

# 7. コード解説

## `main`

```haskell
main
```

はプログラム開始地点。

他言語の `main()` に近い。

---

## `putStrLn`

```haskell
putStrLn "Hello, Haskell!"
```

は：

> 文字列を画面表示する関数

という意味。

---

## `::`

```haskell
main :: IO ()
```

の `::` は：

> 「この値の型はこれです」

という意味。

Haskell は型を非常に重視する。

---

## `IO ()`

```haskell
IO ()
```

は：

> 画面表示など副作用を伴う処理

を表す型。

今は：

> 「表示系のプログラム」

くらいの理解で OK。

---

# 8. GHCi を使ってみる

PowerShell で：

```powershell
ghci
```

を実行。

---

# 9. 数値計算

```haskell
1 + 2
```

結果：

```haskell
3
```

---

```haskell
10 * 5
```

結果：

```haskell
50
```

---

# 10. Haskell の関数呼び出し

```haskell
succ 10
```

結果：

```haskell
11
```

---

## 重要ポイント

Haskell は：

```haskell
関数 引数
```

で関数を呼ぶ。

つまり：

```haskell
sqrt 16
```

のように書く。

他言語の：

```javascript
sqrt(16)
```

とは違う。

Haskell では「空白」が非常に重要。

---

# 11. 文字列

```haskell
"Hello"
```

結果：

```haskell
"Hello"
```

---

## 文字列結合

```haskell
"Hello" ++ " World"
```

結果：

```haskell
"Hello World"
```

`++` は文字列連結。

---

# 12. 型を確認する

Haskell は型システムが強力。

GHCi では `:t` を使って型確認できる。

---

## 数値の型

```haskell
:t 1
```

---

## 文字列の型

```haskell
:t "Hello"
```

---

## 関数の型

```haskell
:t sqrt
```

---

## 学んだこと

- Haskell は型をかなり重視する
- 型を見る習慣が重要
- `:t` は頻繁に使う

---

# 13. 自分で関数を作る

GHCi で：

```haskell
double x = x * 2
```

を実行。

---

## 関数を呼ぶ

```haskell
double 5
```

結果：

```haskell
10
```

---

# 14. 関数定義の意味

```haskell
double x = x * 2
```

は：

> x を受け取って 2 倍して返す関数

を定義している。

---

# 15. ここまでで見えてきた Haskell の特徴

- 関数型言語
- 型を重視する
- 関数呼び出しは空白
- 値を定義する感覚が強い
- 関数と値の境界が薄い
- GHCi で対話的に試せる

---

# 次にやる予定

- リスト
- map
- filter
- ラムダ式
- パターンマッチ
- 再帰
- Maybe
- モナド