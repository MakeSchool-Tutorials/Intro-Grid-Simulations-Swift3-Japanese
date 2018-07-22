---
title: "よりリアルなシミュレーション"
slug: better-forest-fire
---

> [info]
> `control + command`を押しながら、`space bar`をクリックすると絵文字バーが開きます。

ここまでで、基本のシミュレーションを作りました。次は[このページ](https://mikinomura.github.io/simulating-japanese/model/?local=forest/1_fire&play=1&edit=1)の通り、🌲のシミュレーションを実装しましょう。

このページでは前のページのように細かく手順は書いていません。自分の力でどんなコードを書けばいいかを考えて試してみましょう。隣の仲間と一緒に考えながら進めてみましょう。

# 雷の登場

[このページ](https://mikinomura.github.io/simulating-japanese/model/?local=forest/1_fire&play=1&edit=1) の"シミュレーションを試しましょう。前のページで作ったシミュレーションは、ユーザー（あなた）が炎を追加していました。今回は、雷を登場させて炎が発生するようにしましょう。

> [action]
> `ForestFireSimulation`の中に下にある二つの関数を加えましょう:
>
```
func thunderboltAndLightning() {
>
}
>
func aTinyForest() {
>
}
```
>
> `update()`の中にあるコードを全てコピーして、`aTinyForest()`の中にペーストしましょう。コピーをしたら、`update()`の中身を全て消します。そのあと、`update()`関数の中に`thunderboltAndLightning()`を追加します。

## 新たなルール

これから付け加える新たなルール:

- **Setup(セットアップ):**
  1. **Palette setup(パレット):** 🌲 と 🔥がパレットの中にある。
  1. **Random seeding（ランダムに木が生える）:** `8x10`のグリッドを作り、`50%`の確率で🌲が生える。
- **Update(アップデート):**
  1. **Spawning trees（木が増える）:** 空のセルは`0.3%`の確率で🌲になる。
  1. **Burning trees（木が燃える）:** 🌲の周りに一つでも🔥があれば、その木は燃える（次のターンで🔥になる）。もしそうでなかったら、`0.01%`の確率で雷が発生し次のターンで🔥になる。
  1. **Sizzling out（炎が静まる）:** 🔥は次のターンで死に絶えて空のセルになる。

ルールは`aTinyForest()`の中にあるコードととても似ているので、そのコードを活用できるはずです。頑張ってやってみましょう！

> [action]
> `thunderboltAndLightning()`関数に上のルールを表現するコードを書いて見ましょう。

シミュレーションを実行して観察しましょう。確率の値を書き換えてどうシミュレーションが変わっていくかを確認しましょう。

# 木を間引きする

[このページ](https://mikinomura.github.io/simulating-japanese/model/?local=forest/2_firebreak&edit=1&paused=1)を読みながら遊んでみましょう。木を上手く間引きすると、炎が燃え移るのを防ぐことができるのです！

> [action]
> `ForestFireSimulation`に新しい関数を加えます:
>
```
func noMercy() {
>
}
>
```
>
> `update()`関数の中の`thunderboltAndLightning()`を消して、代わりに`noMercy()`関数を加えましょう。`noMercy()`関数の中にコードを書いていきます。

## ルール

ツリーキラー（✄）をこのシミュレーションに追加します。

- **Setup(セットアップ):**
  1. **Palette setup（パレット）:** 🌲, 🔥, ✄がパレットの中にある。
  1. **Random seeding（木が生える）:** `8x10`のグリッドを作り、`50%`の確率で🌲が生える。
- **Update(アップデート):**
  1. **Spawning trees（木が増える）:** 空のセルは`1%`の確率で🌲になる。
  1. **Killing trees（木が死んでいく）:** 🌲の周りに一つでも🔥があれば、その木は燃える（次のターンで🔥になる）。🌲の周りに一つでも✄があれば、🌲が空のセルになる。もしそうでなかったら、`0.01%`の確率で雷が発生し次のターンで🔥になる。
  1. **Sizzling out（炎が静まる）:** 🔥は次のターンで空のタイルになる。

  > [action]
  > `noMercy()`関数をルールにしたがって完成させましょう。✄をパレットに加えるのを忘れないようにしましょう。

シミュレーションを実行して、✄でグリッドの中に線を引きましょう。炎が燃え移るのを防ぐことができましたか？

# 悪い木と炎

[このページ](https://mikinomura.github.io/simulating-japanese/model/?local=forest/3_plants&edit=1)を見てみましょう。今回は、色々な種類の木がある時にどのように炎が広がるかを考えてみます。

> [action]
> `ForestFireSimulation`の中に下の関数を新しく作りましょう:
>
```
func jerkTrees() {
>
}
>
```
>
>`update()`の中の`noMercy()`を消して、代わりに`jerkTrees()`を入れましょう。

## ルール

このシミュレーションでは、強い木🌳と悪い木🌱が登場します。
ルールのまとめ:

- **Setup(セットアップ):**
  1. **Palette setup(パレット):** 🌳, 🌱, 🔥, ✄ をパレットに加えます。
  1. **Random seeding(木が生える):** `8x10`のグリッドを作り、`5%`の確率で強い木🌳が、`5%`の確率で悪い木🌱が生えます。
- **Update(アップデート):**
  1. **Spawning trees(木が広がる):** 空のセルは`0.5%`の確率で🌳になり、`1%`の確率で🌱になる。
  1. **Killing strong trees(強い木が死んでいく):** 🌳は🔥には強いが、🌱に栄養を奪われる。🌳は、周りに✄があるか、４つ以上の🌱に囲まれると死んで空のセルになる。
  1. **Killing jerk trees（悪い木が死んでいく）:**: 🌱は周りに１つでも🔥がある場合に🔥になる。🌱は周りに１つでも✄があると空のセルになる。それ以外の場合は、`0.01%`の確率で雷が落ちて🔥になる。
  1. **Sizzling out（炎が死んでいく）:** 🔥は次のターンで空のセルになる。

  > [action]
  > `jerkTrees()`関数を上のルールにしたがって完成させましょう。パレットに🌳, 🌱, 🔥, ✄が全て入っているか確認しましょう。

シミュレーションを実行して、しばらく観察してみましょう。🌳か🌱のどちらかが侵略してしまいますか？それとも上手く共存しますか？

# 森と炎のシミュレーション: 終わりに
["Peeps Gettin' Sick（感染が広がる）"](https://mikinomura.github.io/simulating-japanese/model/?local=zoo/sick&edit=1), ["Rodent Racism（げっ歯動物の人種差別）"](https://mikinomura.github.io/simulating-japanese/model/?local=zoo/schelling&edit=1&paused=1), ["Cat/Dog Civil Conflict（猫と犬の内戦）"](https://mikinomura.github.io/simulating-japanese/model/?local=zoo/civil_war&edit=1)
をみてみましょう。シミュレーションを触ってみましょう。次のページではいよいよオリジナルのシミュレーションを作りますよ！
