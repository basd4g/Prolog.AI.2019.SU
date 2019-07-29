# monkey  get banana Prolog

## 概要

埼玉大学 人工知能 第14回 課題  

Prologを用いた猿とバナナの問題を解くプログラムの制作

## ファイル一覧

- monkey-fail.swi 与えられた無限ループするコード
- monkey.swi 改良後の正しく動作するコード
- readme.md このファイル

## 問題設定

問題設定 — 猿とバナナの問題 — :
初期状態 state( atdoor, onfloor, atwindow, hasnot)
猿はド ア付近 (atdoor) の床の上 (onfloor) におり ， 台が窓付近にあり (atwindow)， バナナを手にしていない (hasnot)
猿が部屋の中央 (middle) で台の上に登ればバナナを 手にする ことができる (バナナは部屋の中央に吊るされている)
canget(S1) は状態 S1 から 猿がバナナを 手にする こ と ができ れば真
述語は move, canget の 2 種類， 関数は walk, push の 2 種類
canget( S1) :- move( S1, M, S2), canget( S2). は， 状態 S1 で行動 M を選択して状態 S2 に遷移し， S2から猿がバナナ を手にすることができるならば canget( S1)は真となる

## 課題

以下の Prolog プログラムで質問 canget( state( atdoor, onfloor, atwindow, hasnot)). を与えると true が出ることなく無限ループに陥る．これを節の順番を変えるだけで true と判定するプログラムに書き換えよ．

```
move( state( P1, onfloor, B, H), walk(P1,P2), state( P2, onfloor, B, H)).
move( state( P1, onfloor, P1, H), push(P1, P2), state( P2, onfloor, P2, H)).
move( state( P, onfloor, P, H), climb, state( P, onbox, P, H)).
move( state( middle, onbox, middle, hasnot), grasp, state( middle, onbox, middle, has)).
canget( state( P, C, B, has)).
canget( S1) :- move( S1, M, S2), canget( S2)
```

## 環境構築

```
$ brew install swi-prolog
```

## 実行

次の通り実行
```
$ swipl -f monkey.swi
?- canget( state( atdoor, onfloor, atwindow, hasnot)).
```

インタプリタから抜けて終了する
```
?- halt.
```


## 制作の方針

prologのバックトラックによる無限ループが、与えられたプログラムが正しく動かない原因。
バックトラックは、次のようなときに起こる(授業資料より引用)

- 単一化を行う際に， 頭部が一致する節が存在しない
- 単一化を行う際に， 頭部が一致する節は存在したが， 単一化
に失敗した
- (導出された節が偽である)
- 一度， 単一化に成功したが， 以降の導出過程によりバックト ラックが起こり， 親節の代替候補が存在しなかった

バックトラックの動作は深さ優先探索である。
複数の節が適用可能な場合、プログラムの上位に書かれたものをまず適用しながら進む。
行き詰まりバックトラックが起こる場合、最後に複数の説が適用可能なところを、次の節に書き換える。  

このバックトラックの動作において、どの節を先に適用するか(どの文を上位に書くか)を誤ると無限ループに陥る。

(他にも、節の中での順番など、無限ループに陥る要因は考えられるが、今回の原因は上記の通り。)

よってmonkey.swiのように文の順序を入れ替えることで無限ループを避けることが出来る。
条件の厳しいものを上位に書いておくように変更した。

## 制作日

- 課題出題日 2019/07/29
- 課題制作日 2019/07/29
- 課題提出日 2019/07/29
- 課題締切日 2019/08/09 13:00
