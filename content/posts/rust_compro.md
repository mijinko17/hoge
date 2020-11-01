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

何もわからない。自分は AtCoder の rust の提出から適当に拝借している。

現在の AtCoder の judge では proconio なるクレートがインクルードされているのでそれを用いると楽かもしれない。

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

      空の配列の型を指定せずに書くと、文脈から型が推測できないうちはコンパイラから怒られてしまう。型が確定するまで耐えられるなら次で良い。

      ```rs
      let n: usize = 10;
      let mut g = vec![vec![]; n];
      ```

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

- BinaryHeap

  1.  宣言
      ```rs
      let mut heap: BinaryHeap<i32> = BinaryHeap::new();
      ```
  1.  値を取り出す

      ```rs
      let mut heap = BinaryHeap::new();
      heap.push(3);
      heap.push(5);
      heap.push(17);

      // popで値の取得も同時に行う
      let x = heap.pop().unwrap();

      // ダイクストラが捗る書き方
      // 5,3の順に取り出される
      while let Some(y) = heap.pop() {
        println!("{}", y);
      }
      ```

      `pop()`の返り値が option である(今回は `Option<i32>`)ことに注意する。`Option<T>`は値として T だけでなく何らかの失敗を表す`None`を取ることができる。`pop()`は heap が空の時に値を取り出せないので、取得できなかったことも表現できる`Option<T>`を返り値にするのが自然。

  1.  最小を取り出す版

      ```rs
       let mut heap: BinaryHeap<std::cmp::Reverse<i32>> = BinaryHeap::new();
       heap.push(std::cmp::Reverse(3));
       heap.push(std::cmp::Reverse(5));
       let &std::cmp::Reverse(x) = heap.peek().unwrap();
       assert_eq!(x, 3);
      ```

      `std::cmp::Reverse`で向きを逆転させたタイプを得る。
      構造化束縛っぽく書くといい感じに取り出せる。

      BinaryHeap では型に実装された大小関係以外は使えないっぽい。少なくとも独自の比較関数をぶち込んだりはできない。自分で構造体を作って Ord を定義すればまあ。

## 三項演算子を使いたい

`if`文で頑張る。

```rs
let x = 3;
let a = if x % 2 == 0 { 5 } else { 0 };
assert_eq!(a, 0);
```
