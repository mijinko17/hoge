---
title: "C++erが競プロでRustを使おうとして困ったことたち"
date: 2020-05-09T20:05:43+09:00
draft: false
toc: false
images:
tags:
  - untagged
---

備忘録がてら。とりあえずうまくいく書き方を載せます。

ちょこちょこ追加予定。

## 入出力

何もわかりません。自分は AtCoder の rust の提出から適当に拝借しています。

現在の AtCoder の judge では proconio なるクレートがインクルードされているのでそれを用いると楽かもしれません。

<a href="https://docs.rs/proconio/0.4.1/proconio/" target="_blank">proconio</a>

## 配列を宣言したい

- 長さ n、初期値 0 の配列を宣言する

  ```rs
  let n: usize = 10;
  let v: Vec<i32> = vec![0; n];
  ```

  長さの指定は`usize`である必要があることに注意する。

  初期値の型が明らかな場合型指定はいらない。

  ```rs
  let n: usize = 10;
  let v = vec![0; n];
  ```

- `usize`とは限らない変数を用いて長さを指定する

  ```rs
  let n: i32 = 10;
  let v = vec![0; n as usize];
  ```

  `as`で型変換できる。

- 多次元配列を宣言する

  1.  n×m、初期値 0 の二次元配列を宣言する。

      ```rs
      let n: usize = 10;
      let m: usize = 18;
      let v = vec![vec![0; m]; n];
      ```

      3 次元でも同様。

  2.  (グラフなどを想定して) 空の配列を n 個持った配列を宣言する

      ```rs
      let n: usize = 10;
      // g,h,iどれでもOK
      let mut g: Vec<Vec<i32>> = vec![vec![]; n];
      let mut h = vec![vec![] as Vec<i32>; n];
      let mut i = vec![vec![0; 0]; n];
      ```

      空の配列は型がわからないので何らかの形での指定が必要。

## データ構造系

`use std::collections::*;`すると楽。以下はこれがある前提。

- BTreeMap

  1.  宣言

      ```rs
      let mut mp: BTreeMap<i32, i32> = BTreeMap::new();
      ```

  2.  個数をカウントする

      ```rs
      let v: Vec<i32> = vec![2, 1, 2, 3, 5, 2, 1];
      let mut mp: BTreeMap<i32, i32> = BTreeMap::new();
      for e in v.iter() {
          *mp.entry(*e).or_insert(0) += 1;
      }
      assert_eq!(mp[&2], 3);
      ```

      `or_insert`でキーが存在しない場合に 0 を入れてくれる。

## 三項演算子を使いたい

`if`文で頑張る。

```rs
let x = 3;
let a = if x % 2 == 0 { 5 } else { 0 };
assert_eq!(a, 0);
```
