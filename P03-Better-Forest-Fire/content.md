---
title: "よりリアルなシミュレーション"
slug: better-forest-fire
---

> [info]
> You can open an emoji search popup on any OS X computer by holding `control + command` and press the `space bar`.
> `control + command`を押しながら、`space bar`をクリックすると絵文字バーが開きます。

Now that we have the base simulation completed, let's try to implement the rest 🌲 simulations [here](http://ncase.me/simulating/)!
ここまでで、基本のシミュレーションを作りました。次は🌲のシミュレーションを実装しましょう。

We won't be giving you much guidance this time around so make sure to read the rules careful and plan out your code. It might be best to work with a partner so you can catch each others mistakes!
このページでは前のページのように細かいガイドは書いていません。自分の力でどんなコードを書けばいいかを考えて試して見ましょう。隣の仲間と一緒に考えながら進めてみましょう。

# Forest fires cause by lightning　雷の登場

Read through "A Forest With Thunderbolt & Lightning, Very Very Frightening" on this [page](http://ncase.me/simulating/) and play with the simulation. In our last simulation, fire was caused by a human act (you). In this simulation, we'll simulate random fires started by lightning!
[このページ](http://ncase.me/simulating/) の"A Forest With Thunderbolt & Lightning, Very Very Frightening"セクションを読みましょう。前のページで作ったシミュレーションは、ユーザー（あなた）が炎を追加していました。今回は、雷を登場させて炎が発生するようにしましょう。

> [action]
> Before we get started, create two new functions in your `ForestFireSimulation` class as shown below:
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
> Take all the code you wrote in your `update()` method and copy it into the body of `aTinyForest()`. Once copied, remove all the code from `update()` and replace it with a single line calling `thunderboltAndLightning()`. We'll write our new simulation in `thunderboltAndLightning()`.
> `update()`の中にあるコードを全てコピーして、`aTinyForest()`の中にペーストしましょう。コピーをしたら、`update()`の中身を全て消します。そのあと、`update()`関数の中に`thunderboltAndLightning()`を追加します。

## The rules　新たなルール

Here is a summary of the rules that we're implementing:
これから付け加える新たなルール:

- **Setup(セットアップ):**
  1. **Palette setup(パレット):** 🌲 and 🔥 should be in the palette.🌲 と 🔥がパレットの中にある。
  1. **Random seeding（ランダムに木が生える）:** Create a `8x10` grid randomly populated with a `50%` chance of a 🌲 tile, or empty otherwise.`8x10`のグリッドを作り、`50%`の確率で🌲が生える。
- **Update(アップデート):**
  1. **Spawning trees（木が増える）:** An empty tile has a `0.3%` chance of becoming a 🌲.空のセルは`0.3%`の確率で🌲になる。
  1. **Burning trees（木が燃える）:** 🌲 turns into 🔥 if any of it's neighbors are 🔥. Otherwise, there is a `0.01%` chance it get's struck by lightning and turns into 🔥.🌲の周りに一つでも🔥があれば、その木は燃える（次のターンで🔥になる）。もしそうでなかったら、`0.01%`の確率で雷が発生し次のターンで🔥になる。
  1. **Sizzling out（炎が静まる）:** 🔥 dies out and becomes an empty tile.🔥は次のターンで死に絶えて空のセルになる。

The rules to this are pretty similar to `aTinyForest()` so you can probably reuse most of your code. Good luck!
ルールは`aTinyForest()`の中にあるコードととても似ているので、そのコードを活用できるはずです。頑張って見ましょう！

> [action]
> Implement `thunderboltAndLightning()` according to the rules above!
> `thunderboltAndLightning()`関数に上のルールを表現するコードを書いて見ましょう。

Run your simulation for a while and watch as the forest burns down and grows back over time. Modify the growth rate of trees. How does that affect things?
シミュレーションを実行して観察しましょう。確率の値を書き換えてどうシミュレーションが変わっていくかを確認しましょう。

# Killing trees to save trees　木を間引きする

Read through "A Forest Where You Show Trees No Mercy" on this [page](http://ncase.me/simulating/) and play with the simulation. This time we'll be showing how killing trees can actually help stop the spread of forest fires!
[このページ](http://ncase.me/simulating/)の"A Forest Where You Show Trees No Mercy"を読みながら遊んでみましょう。木を上手く間引きすると、炎が燃え移るのを防ぐことができるのです！

> [action]
> Before we get started, create a new function in your `ForestFireSimulation` class as shown below:
> `ForestFireSimulation`に新しい関数を加えます:
>
```
func noMercy() {
>
}
>
```
>
> Replace the call to `thunderboltAndLightning()` in `update()` with a call to `noMercy()`. We'll write our new simulation in `noMercy()`.
> `update()`関数の中の`thunderboltAndLightning()`を消して、代わりに`noMercy()`関数を加えましょう。`noMercy()`関数の中にコードを書いていきます。

## The rules　ルール

We'll be adding tree killers (✄) into this simulation. Here is a summary of the rules that we're implementing:
ツリーキラー（✄）をこのシミュレーションに追加します。

- **Setup（セットアップ）:**
  1. **Palette setup（パレット）:** 🌲, 🔥, ✄ should be in the palette.🌲, 🔥, ✄がパレットの中にある。
  1. **Random seeding（木が生える）:** Create a `8x10` grid randomly populated with a `50%` chance of a 🌲 tile, or empty otherwise.`8x10`のグリッドを作り、`50%`の確率で🌲が生える。
- **Update(アップデート):**
  1. **Spawning trees（木が増える）:** An empty tile has a `1%` chance of becoming a 🌲.　空のセルは`1%`の確率で🌲になる。
  1. **Killing trees（木が死んでいく）:** 🌲 turns into 🔥 if any of it's neighbors are 🔥. It becomes an empty cell if any of it's neighbors are ✄. Otherwise, there is a `0.01%` chance a 🌲 get's struck by lightning and turns into 🔥.🌲の周りに一つでも🔥があれば、その木は燃える（次のターンで🔥になる）。🌲の周りに一つでも✄があれば、🌲が空のセルになる。もしそうでなかったら、`0.01%`の確率で雷が発生し次のターンで🔥になる。
  1. **Sizzling out（炎が静まる）:** 🔥 dies out and becomes an empty tile. 🔥は次のターンで空のタイルになる。

  > [action]
  > Implement `noMercy()` according to the rules above! Make sure to add ✄ to your palette so you can contain the forest fires!
  > `noMercy()`関数をルールにしたがって完成させましょう。✄をパレットに加えるのを忘れないようにしましょう。

Run your simulation and create a line of ✄. Is it effective at stopping the spread of forest fires?
シミュレーションを実行して、✄でグリッドの中に線を引きましょう。炎が燃え移るのを防ぐことができましたか？

# Invasive species and fires

Read through "A Forest Where Some Trees Are, Like, Total Jerks" on this [page](http://ncase.me/simulating/) and play with the simulation. This time we'll explore how fires spread when there are trees of different species! Some species rely on fires for their survival...
[このページ](http://ncase.me/simulating/)の"A Forest Where Some Trees Are, Like, Total Jerks"を見てみましょう。今回は、色々な種類の木がある時にどのように炎が広がるかを考えてみます。

> [action]
> Before we get started, create a new function in your `ForestFireSimulation` class as shown below:
> `ForestFireSimulation`の中に下の関数を新しく作りましょう:
>
```
func jerkTrees() {
>
}
>
```
>
> Replace the call to `noMercy()` in `update()` with a call to `jerkTrees()`. We'll write our new simulation in `jerkTrees()`.
>`update()`の中の`noMercy()`を消して、代わりに`jerkTrees()`を入れましょう。

## The rules　ルール

We'll be adding strong trees and jerk trees (🌳 and 🌱) into this simulation. Here is a summary of the rules that we're implementing:
このシミュレーションでは、強い木🌳と悪い木🌱が登場します。
ルールのまとめ:

- **Setup(セットアップ):**
  1. **Palette setup(パレット):** 🌳, 🌱, 🔥, ✄ should be in the palette.🌳, 🌱, 🔥, ✄ をパレットに加えます。
  1. **Random seeding(木が生える):** Create a `8x10` grid randomly populated with a `5%` chance of a 🌳 tile, a `5%` chance of a 🌱 tile, or empty otherwise.`8x10`のグリッドを作り、`5%`の確率で強い木🌳が、`5%`の確率で弱い木🌱が生えます。
- **Update(アップデート):**
  1. **Spawning trees(木が広がる):** An empty tile has a `0.5%` chance of becoming a 🌳 and a `1%` chance of becoming a 🌱. 空のセルは`0.5%`の確率で🌳になり、`1%`の確率で🌱になる。
  1. **Killing strong trees(強い木が死んでいく):** 🌳 are invincible to 🔥 but can be choked of nutrients by 🌱! 🌳 becomes an empty cell if any of it's neighbors are ✄ or at least 4 of it's neighbors are 🌱.🌳は🔥には強いが、🌱に栄養を奪われる。🌳は、周りに✄があるか、４つ以上の🌱に囲まれると死んで空のセルになる。
  1. **Killing jerk trees（悪い木が死んでいく）:**: 🌱 turns into 🔥 if any of it's neighbors are 🔥. 🌱 becomes an empty cell if any of it's neighbors are ✄. Otherwise, there is a `0.01%` chance a 🌱 get's struck by lightning and turns into 🔥.🌱は周りに１つでも🔥がある場合に🔥になる。🌱は周りに１つでも✄があると空のセルになる。それ以外の場合は、`0.01%`の確率で雷が落ちて🔥になる。
  1. **Sizzling out（炎が死んでいく）:** 🔥 dies out and becomes an empty tile.🔥は次のターンで空のセルになる。

  > [action]
  > Implement `jerkTrees()` according to the rules above! Make sure to set your palette to 🌳, 🌱, 🔥, ✄!
  > `jerkTrees()`関数を上のルールにしたがって完成させましょう。パレットに🌳, 🌱, 🔥, ✄が全て入っているか確認しましょう。

Run your simulation and watch it for a while. Does 🌳 or 🌱 take over? Or do they live in harmony? Do any patterns emerge?
シミュレーションを実行して、しばらく観察してみましょう。🌳か🌱のどちらかが侵略してしまいますか？それとも上手く共存しますか？

# Wrapping up with trees　森と炎のシミュレーション: 終わりに

Go back to this [page](http://ncase.me/simulating/) and read through the end of it. Play with the "Peeps Gettin' Sick", "Rodent Racism", and "Cat/Dog Civil Conflict" simulations. We'll be creating our own, custom simulation on the next page!
[このページ](http://ncase.me/simulating/)を最後までみてみましょう。"Peeps Gettin' Sick"、"Rodent Racism"、"Cat/Dog Civil Conflict"シミュレーションを触ってみましょう。次のページではいよいよオリジナルのシミュレーションを作りますよ！
