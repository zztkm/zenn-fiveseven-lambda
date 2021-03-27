---
title: "はじめに"
---

# この本について
Rust は 2010 年に登場した言語ですが， C++ や python など他の言語をよく知っている人に向けた説明ばかり出回っていて，プログラミングそのものの初心者に向けた解説がほとんど存在しないのが現状です．この本は， AtCoder を利用しながら，プログラミング初心者に向けて Rust を分かりやすく解説することを狙いとしています．

内容も基礎的な部分のみです．ここで「基礎」の基準は「競技プログラミングで用いられるか」であり， GUI ，マルチスレッド， Web アプリケーションなどについては説明しません． unsafe や FFI などの高度な内容についても説明しません．

また，この記事では AtCoder のジャッジシステムを利用するため，まず AtCoder にログインすることが必要になります．アカウントを持っていない人は，[ここ](https://atcoder.jp/register)からアカウント作成を行ってください．

# 入出力
レストランにやってきた客が店員に料理を注文し，店員が客に料理を出すという流れを考えてください．競技プログラミングにおけるプログラムと人（ユーザ）のやり取りは，この店員と客のやり取りと同じように捉えられます．すなわち，プログラムはユーザから何らかの注文を受け，処理を行って，結果をユーザに返します．この対話には，注文にあたる**標準入力**，料理にあたる**標準出力**，そしてプログラム内でエラーが発生したときにそれをユーザに報告する**標準エラー出力**の 3 つが用いられます．これらのやり取りは全て文字列で行われます．

例として，「 2 つの数を受け取って，その和を計算するプログラム」を考えます．このプログラムを実行するとき，例えば標準入力として `10 20` をユーザがプログラムに渡すと，標準出力として `30` をプログラムがユーザに返します．計算の途中で何らかのエラーが発生した場合は，その旨を標準エラー出力としてプログラムがユーザに返します．

競技プログラミングでは，「 2 つの数を受け取って，その和を計算するプログラムを作れ」というように，標準入力で受け取ったデータに対し正しく処理を行って結果を標準出力に返すプログラムを作る問題が出題されます．この問題であれば，プログラムが 2 つの数を受け取ったときに正しく和を出力できていれば正解となります．一方，間違った数を出力している場合や，計算に時間がかかりすぎている場合は不正解となります．

# 提出
AtCoder での提出の仕方を知るために，問題を 1 つ解いてみましょう．ログインした後， [practice contest A - Welcome to AtCoder](https://atcoder.jp/contests/practice/tasks/practice_1) を開いてください．

問題文に，どのようなプログラムを作るのか書いてあります．以下の手順で，この問題に対する提出を行ってみましょう．

1. 一番下までスクロールすると，ソースコードを入力する欄があるので，次の内容をコピーして貼り付けます．
   ```rust
   use proconio::input;
   
   fn main() {
       input! {
           a: i32,
           b: i32,
           c: i32,
           s: String,
       }
       println!("{} {}", a + b + c, s);
   }
   ```
1. 言語の選択欄から Rust を探し，選択します．
1. 「提出」をクリックします．
1. 結果が灰色の「WJ」から緑色の「AC」に変われば成功です．

AC は Accepted の略で，提出したソースコードが正解とみなされたことを意味します．これらの用語の説明は[ここ](https://atcoder.jp/contests/practice/glossary)にあります．

# コードテスト
[このページ](https://atcoder.jp/contests/practice/custom_test)では，コードテストを行うことができます．コードテストは次の手順で行います．

1. 提出するときと同じように，入力欄にソースコードを入力します．
1. 「標準入力」の欄に，プログラムに渡す内容を書きます．
1. 言語の選択欄から Rust を探し，選択します．
1. 「実行」をクリックします．
1. プログラムが実行され，結果が「標準出力」と「標準エラー出力」の欄に表示されます．
1. 正常に終了したときは「終了コード」が 0 になります．一方，プログラム内で異常が起こったときは終了コードが 0 以外になります．

問題を解いたら，コードを提出する前に一度コードテストを使って正常に動くか確かめましょう．