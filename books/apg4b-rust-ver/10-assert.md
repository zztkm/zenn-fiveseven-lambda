---
title: "アサート"
---
# `assert!` マクロ
アサートは，プログラムの異常に気付きやすいようにするための手段です．次のコードを見てください．
```rust
use proconio::input;

fn main() {
    input! {
        x: i32,
    }
    let r = x % 10;
    assert!(0 <= r && r < 10);
    println!("あまりは {}", r);
}
```

`input!` マクロで整数 $x$ を受け取り， $10$ で割った余りを変数 $r$ に代入しています．このとき， $0 \leq r < 10$ は成り立つでしょうか．これをチェックするのが， **`assert!` マクロ**です．

`assert!( )` の括弧内に条件を書き， `assert!(0 <= r && r < 10);` とします．ここでもし `0 <= r && r < 10` が成り立っていれば，何事もなかったかのようにプログラムの続きが実行されます．一方，もし `0 <= r && r < 10` が成り立っていなければ，このプログラムは実行時エラーとなり，*続きは実行されません*（`panic!` マクロと同じ）．

まず，このプログラムに標準入力として `123` を与えて実行してみましょう． $x = 123$ なので $r = 3$ となり， $0 \leq r < 10$ が成り立つはずです．やってみると `あまりは 3` と出力され，ちゃんと `assert!` の続きが実行されていることが分かります．

次に，標準入力を `-123` に変えてみましょう．おや？標準出力には何も出力されず，代わりに標準エラー出力に
```
thread 'main' panicked at 'assertion failed: 0 <= r && r < 10', src/main.rs:8:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```
と出力されます．終了コードも 0 以外の値になっています．これが， `assert!` で括弧内の条件が成り立っていなかったときの挙動です．

実は， `%` 演算子を使って余りの計算をするときは，負の場合の挙動に注意する必要があります． $x \geq 0$ のときは「$x$ を $y$ で割った余り」ですが， $x < 0$ のときは「$-x$ を $y$ で割った余りに，負号を付けた値」になるのです．今回は， $x = -123$，$y = 10$ なので，「$123$ を $10$ で割った余り（$3$）に負号を付けた値」となって， $r = -3$ です．よって， `assert!` マクロが実行時エラーを引き起こしたのです．

今回は `println!` で余りを出力するだけでしたが，もしこの前後に膨大な処理が書かれていた場合，どこに異常があるのか特定するのが困難になります．アサートを使うことで，どこで異常が発生しているのか気付きやすくなります．
# メッセージ
```rust
assert!(0 <= r && r < 10);
```
の代わりに
```rust
assert!(0 <= r && r < 10, "割った余りが想定の範囲を超えています");
```
と書くと，失敗したときの文面が
```
thread 'main' panicked at '割った余りが想定の範囲を超えています', src/main.rs:8:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```
のようになります． `println!` マクロと同じように
```rust
assert!(0 <= r && r < 10, "割った余りが {} で，想定の範囲を超えています", r);
```
と書くこともできます．
# `assert_eq!` / `assert_ne!` マクロ
`assert!` マクロの中で `==` を使って `assert!(a == b)` のように書くときは，代わりに `assert_eq!` マクロを使って `assert_eq!(a, b)` と書くこともできます．
```rust
use proconio::input;

fn main() {
    input! {
        x: i32,
        y: i32
    }
    let rounded = x / y * y;
    assert_eq!(rounded % y, 0);
}
```
同様に， `assert!(a != b)` の代わりに `assert_ne!(a, b)` と書くことができます．

`assert_eq!` / `assert_ne!` の場合も， `assert_eq!(a, b, "メッセージ");` のようにして失敗時のメッセージを追加することができます．