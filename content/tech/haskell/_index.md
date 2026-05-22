---
title: "Haskell"
weight: 1
---

# Haskell 学習ログ

## GHCi を起動する

`今回は Windows 環境で Haskell を学べるように、環境構築から最初のコード実行までを行った。

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

# 数値計算

```haskell
1 + 2
```

結果：

```haskell
3
```

---

# `+` は何なのか

Haskell では `+` は特殊構文ではなく関数。

つまり：

```haskell
1 + 2
```

は実質：

```haskell
(+) 1 2
```

と同じ。

---

# なぜ `(+)` と書くのか

通常、関数は：

```haskell
f x
```

の形で適用する。

しかし `+` は「中置演算子」として定義されているので：

```haskell
1 + 2
```

のように真ん中に置ける。

---

# 関数として扱うには括弧が必要

```haskell
(+)
```

と書くと、

> 「演算子そのもの」

を表せる。

例えば：

```haskell
:t (+)
```

を実行すると：

```haskell
(+) :: Num a => a -> a -> a
```

となる。

---

# この型を分解する

```haskell
Num a => a -> a -> a
```

は：

> Num 型クラスに属する型 `a` を2つ受け取り、`a` を返す関数

という意味。

---

# `->` の意味

Haskell の関数型は：

```haskell
入力型 -> 出力型
```

で表す。

つまり：

```haskell
Int -> Int
```

なら：

> Int を受け取って Int を返す関数

---

# `(+)` の場合

```haskell
a -> a -> a
```

は右結合なので：

```haskell
a -> (a -> a)
```

と同じ。

つまり `(+)` は本質的には：

1. 最初の引数を受け取る
2. 「残り1引数を待つ関数」を返す

という構造。

---

# これはカリー化

Haskell の関数は基本的に全て：

> 1引数関数

として扱われる。

複数引数関数に見えるものも、

実際は：

```text
a -> (b -> c)
```

のようなネスト構造。

これをカリー化（currying）という。

---

# `succ`

```haskell
succ 10
```

結果：

```haskell
11
```

---

# `succ` の型を見る

```haskell
:t succ
```

結果：

```haskell
succ :: Enum a => a -> a
```

---

# `Enum`

`Enum` は型クラス。

「順番を持つ型」を表す。

例えば：

- Int
- Char
- Ordering

など。

---

# つまり `succ`

```haskell
succ
```

は：

> 次の値を返す関数

---

例えば：

```haskell
succ 'a'
```

結果：

```haskell
'b'
```

---

# 関数適用

```haskell
sqrt 16
```

結果：

```haskell
4.0
```

---

# なぜ `sqrt(16)` ではないのか

Haskell において：

```haskell
f x
```

は：

> 関数適用

を表す基本構文。

関数適用は演算子より優先順位が高い。

例えば：

```haskell
f x + y
```

は：

```haskell
(f x) + y
```

として解釈される。

---

# 文字列

```haskell
"Hello"
```

結果：

```haskell
"Hello"
```

---

# 文字列の型

```haskell
:t "Hello"
```

結果：

```haskell
"Hello" :: String
```

---

# `String` の正体

実は：

```haskell
String
```

は型シノニム。

定義は：

```haskell
type String = [Char]
```

つまり：

> 文字列 = Char のリスト

---

# リスト記法

```haskell
[1,2,3]
```

はリスト。

型を見る：

```haskell
:t [1,2,3]
```

結果：

```haskell
[1,2,3] :: Num a => [a]
```

---

# `[a]`

これは：

> 型 `a` のリスト

を意味する。

---

# `String` が `[Char]` ということ

つまり：

```haskell
"abc"
```

は本質的には：

```haskell
['a','b','c']
```

と同じ。

---

# `++`

```haskell
"Hello" ++ " World"
```

結果：

```haskell
"Hello World"
```

---

# `++` の型

```haskell
:t (++)
```

結果：

```haskell
(++) :: [a] -> [a] -> [a]
```

---

# つまり `++` は

> リスト結合関数

文字列専用ではない。

例えば：

```haskell
[1,2] ++ [3,4]
```

結果：

```haskell
[1,2,3,4]
```

---

# 型を見る

```haskell
:t 1
```

結果：

```haskell
1 :: Num p => p
```

---

# なぜ型が確定していないのか

`1` 単体では：

- Int
- Integer
- Float
- Double

などどれにもなれる。

そのため：

```haskell
Num p => p
```

という形になっている。

---

# 型クラス制約

```haskell
Num p =>
```

は：

> p は Num 型クラスのインスタンスでなければならない

という制約。

---

# 関数を定義する

```haskell
double x = x * 2
```

---

# これは何か

これは：

```haskell
double = \x -> x * 2
```

の糖衣構文。

---

# `\`

Haskell では：

```haskell
\x -> ...
```

でラムダ式を書く。

つまり：

```haskell
double x = x * 2
```

は、

> x を受け取って x*2 を返す無名関数

を `double` に束縛している。

---

# 型を見る

```haskell
:t double
```

結果：

```haskell
double :: Num a => a -> a
```

---

# 意味

- Num 型クラスに属する型を受け取り
- 同じ型を返す関数

---

# 実行

```haskell
double 5
```

結果：

```haskell
10
```

---

# ここまでで見えてきたこと

現時点で重要なのは：

- 演算子も関数
- 関数適用が中心
- 関数はカリー化されている
- 型クラスがある
- String は `[Char]`
- `++` はリスト結合
- 関数定義はラムダへの束縛

という点。

これは今後：

- 高階関数
- map
- foldr
- モナド
- 関数合成

などを理解する基礎になる。