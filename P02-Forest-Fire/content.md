---
title: "オリジナルのシミュレーション: 森の炎たち！"
slug: forest-fire
---

> [info]
> `control + command`を押しながら、`space bar`をクリックすると絵文字バーが開きます。

ライフゲームのロジックを無事にXcodeプロジェクトに移すことができたので、いよいよオリジナルのカスタムシミュレーション - 森の中の炎シミュレーションを作ってみましょう！

> [アクション]
> [このページ](https://mikinomura.github.io/simulating-japanese/model/?local=forest/0_growth&play=0&edit=1) を開き、シミュレーションのルールを読みましょう。これをアプリに実装します！

今から実装していくルールのまとめ:

- **Setup(設定):**
  1. **Palette setup（パレットの設定):** 🌲 と 🔥 はパレットに入っている。
  1. **Random seeding (ランダムに生える木):** `8x10`のグリッドを作り、`50%`の確率で🌲が現れ、そうでなければ空のままにする。
- **Update（アップデート）:**
  1. **Spawning trees（木の誕生):** 空のタイルは、次のターンで`0.1%`の確率で🌲になる。
  1. **Burning trees:（焼ける木）** もし周りに🔥があれば、🌲は次のターンで🔥に変わる。
  1. **Sizzling out (焼け切る木):** 🔥は次のターンで死に絶えて、空のタイルになる。

このルールを一つずつ攻略していきます。

## ForestFireSimulation クラス

楽しそうですね！まずは、この新しいシミュレーションのための新しいファイルを作りましょう。

> [アクション]
> `Models`グループをクリックして、`File > New File`をクリックするか、もしくは`command+N`を押しましょう。`iOS > Source > Swift File`と進んで新しいSwiftファイルを作ります。タイトルは`ForestFireSimulation.swift`で、`Grid-Simulations-Xcode/Grid-Simulations`フォルダの中にこのファイルを保存します。このビデオをみてみましょう！
> ![ms-video](assets/new-file.mp4)

さて次は`ForestFireSimulation`という`Simulation`から継承された新しいクラスを作ります。

> [アクション]
> `ForestFireSimulation.swift`のファイルに、下のコードを入れてください。
>
```swift
public class ForestFireSimulation: Simulation {
    public override func setup() {
>
    }
>
    public override func update() {
>
    }
}
```

# シーンのセットアップ

このライフゲームでは、初めの状態を`.txt`ファイルに保存して、そのファイルを読み込ませることで表示させています。この新しいシミュレーションでは、この方法は使えません。なぜなら、今回のシミュレーションは最初の状態がランダムになっていて毎回違うものが表示されるようにしたいからです。かわりに、`ForestFireSimulation`クラスの`setup()`関数の中で、初めの状態を定義します。

まず、ファイルからグリッドの状態を読み込むコードを削除し、代わりに自分たちのロジックを入れましょう。

<!-- ACTION: UPDATE THESE INSTRUCTIONS AFTER ADDING SIMULATION CHOOSE SCREEN -->

> [アクション]
> `GameViewController.swift`を開きます。ここのファイルがシーンの読み込みをしています。
>
> 24行目と26行目をコメントアウトして、36行目と37行目をアンコメントしましょう。こうすることで、`ForestFireSimulation`クラスのパレットを読み込みます。

# (擬似）乱数を発生させよう

`setup()`関数に戻る前に、乱数について考えてみましょう。

コンピュータは[擬似乱数](https://ja.wikipedia.org/wiki/%E6%93%AC%E4%BC%BC%E4%B9%B1%E6%95%B0)と呼ばれるアルゴリズムをよく用いています。
Swiftではいくつかのやり方でこのアルゴリズムを使うことができます。
例:

```swift
arc4random()
```

このコードは0から`RAND_MAX`（自分のコードの中で使うことができる最大値、グローバル定数）までの`Int`を返します。では、もし0から1の間のランダムな`Double`が欲しかったらどうすればいいでしょうか？

```swift
Double(arc4random()) / Double(RAND_MAX)
```
これはよく使うので、`randomZeroToOne()`という関数を作っておきました。この関数は0から1までの`Double`型の値を返します。このアプリの中でいつでも使える便利な関数です！

> [info]
> Swiftでは、分母、分子の値の二つとも、`Double`型にキャスト(変換)する必要があります。Swiftは`Int`を`Double`に自動的に変換することができないのです。Javaのような他のプログラミング言語では、自動的に変換されるので少し異なります。

<!-- ## Seeding your random numbers

However, because the pseudorandom algorithm is _deterministic_, it will give you the same results each time you run your app. That's not what we want!

Pseudorandom number generators often have something called a "seed" value – a single truly random number that all subsequent `random()` calls are based off. But what is a truly random seed that we can use? Let's try the current time:

```swift
let time = UInt32(NSDate().timeIntervalSinceReferenceDate)
srandom(time) // this "seeds" subsequent random() calls
```

So, you call the `srandom()` function once in your app, before you generate any `random()` numbers – and then your random numbers will be different each time you open your app! Let's go ahead and write this `srandom()` code somewhere that runs only once – the `viewDidLoad()` function in `GameViewController`:

> [action]
> In line 17 of `GameViewController` – right after the call to `super.viewDidLoad()` – add in the above code to "seed" the random number generator, using `srandom` and the current time. -->

では`setup()`関数にこのロジックを応用してみましょう。

# ランダムに木を生やそう

初めにまとめたガイドラインには、8x10のグリッドに、50%の確率で🌲が埋まると書かれていました。それを２次元配列を使って作り始めましょう！

`ForestFireSimulation`クラスの中から、グリッドは`grid`と呼び出すことで使うことができます。さあ、`grid`を編集して新しい2次元配列を作りましょう。

> [アクション]
> `ForestFireSimulation.swift`の中の`setup()`の中に下記のコードを入れましょう。
>
```swift
grid = [[Character?]](repeating: [Character?](repeating: nil, count: 10), count: 10)
```

次は、2Dのグリッドの一つ一つのタイルを繰り返し、50%の確率で🌲を入れていく作業をしましょう。でもどうすればいいのでしょう？

`randomZeroToOne()`関数を覚えていますか？これは0から1の間の値をランダムに返してくれる関数でしたね。この関数で返って来る値と`0.5`を比べれば、50%の確率を表現できることになりますね！

> [アクション]
> コードを書いてタイルを🌲で埋めましょう。配列の中身を繰り返し操作する方法を覚えていますか？
もし`randomZeroToOne()`が`0.5`より小さい値の時、🌲をセットしましょう。

<!--  -->

> [solution]
>書き終わりましたか？`setup()`関数はこのようになっているはずです:
>
```swift
grid = [[Character?]](repeating: [Character?](repeating: nil, count: 10), count: 10)
for x in 0..<8 {
    for y in 0..<10 {
        if randomZeroToOne() < 0.5 {
            grid[x][y] = "🌲"
        }
    }
}
```

ここでコードを実行したら、ランダムに生える🌲が見られるはずです！

<!-- Great! Now we spawn our random 🔥 at a random tile location. Here, we learn about the modulo `%` operator:

```swift
10 % 3
```

The modulo operator, or `%`, will return the remainder of an integer division. So in this example, the above code will return a `1`.

Why is this useful? Well, say you wanted a random number between 0 and 9 – just perfect for the index position of our grid:

```swift
arc4random() % 10
```

Since `random()` returns an `Int`, we can modulo it against 10 to return a value between 0 (inclusive) and 10 (exclusive). Now all we have to do is call this twice to get our random x and y coordinates, and set the corresponding tile to 🔥!

Let's see if you can write this code – place it at the end of the `setup()` function – right after the code you just wrote.

> [solution]
> Your code should look something like this:
>
```swift
let x = arc4random() % 10
let y = arc4random() % 10
grid[x][y] = "🔥"
```

Run your code again. You should now see a single 🔥 placed at a random location! -->

# 周りのセルのデータを見てみよう

Update関数に入る前に、まずいくつかのヘルパー関数を作りましょう。このヘルパー関数は各セルの周りのデータを取得します。

## isLegalPosition

まず一つ目のヘルパー関数は、確認しようとしているセルがグリッドの中に収まっているかを確認して、`Bool`型のデータを返します。（つまり、もしグリッドの中に収まっていたら`true`を、そうでなければ`false`を返します)
この関数の名前を`isLegalPosition(x: Int, y:Int)`と名付けましょう。

> [アクション]
>まだ空の`update()`関数の下に、新しい関数を作りましょう。
>
```swift
func isLegalPosition(x: Int, y: Int) -> Bool {
>
}
```

> そして、引数になっている x と y が`grid`の中に収まっているかを確認して、収まっていたらtrue, そうでなければfalseを返すコードを書きましょう。
<!--  -->

> [答え]
> `isLegalPosition`関数はこのようになっているはずです:
>
```swift
func isLegalPosition(x: Int, y: Int) -> Bool {
    if x >= 0 && x < grid.count && y >= 0 && y < grid[0].count {
        return true
    } else {
        return false
    }
}
```

## getNeighborPositions - 周りの位置の番号を取得しよう

次に、セルの周りの位置番号を取得する関数を書きます。名前は`getNeighborPositions(x originX: Int, y originY: Int) -> [(x: Int, y: Int)]`。この関数は`(Int, Int)`というタプル型の`配列`を返します。つまり、セルの周りの位置番号のリストです。

### タプル型って？

`()`を使って表すデータのことをタプル型と言って、[ここ](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID329)から詳細を見ることができます（英語）。タプルは、異なるデータ型の値をひとまとめにできる簡単な方法です。今回、この関数は _タプル_　（`x`と`y`の値が入ったデータ）の配列を返します。

タプルの書き方の例:

```swift
let tuple = (2, 3)
```

タプルはそれぞれの値に名前をつけることができて、その名前を使って後からデータを呼び出すことができます。
例:

```swift
let namedTuple = (x: 2, y: 3)
let x = namedTuple.x
let y = namedTuple.y
```

> [アクション]
> `isLegalPosition`関数の下に新しい関数を作りましょう。下のようになるはずです:
>
```swift
func getNeighborPositions(x originX: Int, y originY: Int) -> [(x: Int, y: Int)] {
>
}
```
>さて、この関数の中にセルの周りの`(x, y)`の値の配列を返すようなコードを書きましょう。
>
> 計算をしたセルの周りの番号がグリッドの外にはみ出していないかどうか、どうやって確かめますか？ `isLegalPosition()`関数はそのための関数ですね！ `for x in a...b`を使うと、`a`から`b`までの値を全て繰り返し使うことができるのを覚えていますか？　たとえば、myArrayという配列に(xValue, yValue)というタプルの値を付け加えたい時は、myArray.append((xValue, yValue))と書きます。
>
> 始める前に、あるセルの周りのセルの位置番号を取得するということは、どこからどこまでの番号を確認する必要があるのか考えて見ましょう。　`(originX, originY)`（自分自身）を間違えて入れないためにはどんなコードを書けばいいか少し注意して見ましょう。

<!--  -->

>[答え]
> `getNeighborPositions`関数は下のようになっているはずです:
>
```swift
func getNeighborPositions(x originX: Int, y originY: Int) -> [(x: Int, y: Int)] {
    var neighbors: [(x: Int, y: Int)] = []
    for x in (originX - 1)...(originX + 1) {
        for y in (originY - 1)...(originY + 1) {
            if isLegalPosition(x: x, y: y) {
                if !(x == originX && y == originY) {
                    neighbors.append((x, y))
                }
            }
        }
    }
    return neighbors
}
```

# 木を増やす

いくつかヘルパー関数を書きましたね。これで、新しい木を生やすコードに取り掛かることができます。ルールを思い出しましょう。空のセルは次のターンで`0.1%`の確率で🌲になります。

さて、このルールを実際にコードで表現するためには、”仮の”グリッドを作ることが必要です。`grid`をいきなり書き換えてしまうと、前のターンのデータと次のターンのデータが混ざってしまうので、別々にする必要があるのです。

> [アクション]
>`ForestFireSimulation`クラスの中に、`newGrid: [[Character?]]`インスタンスを作りましょう。
> `setup()`より上（関数の外側）に、下のコードを入れましょう。
>
```
var newGrid: [[Character?]] = []
```
> まず`update()`関数の始めに、`newGrid`の値を`grid`にセットします。それから、`grid`の中の一つ一つの値を使いながら、`newGrid`の値を書き換えていきます。最後に、`grid`の値を`newGrid`の値にセットすれば完了です！
> ループ処理の中で、セルが空（`nil`）かどうか確認して、もし空だったら、`0.1%`の確率で木が植えられるようにコードを書きます。（確率には`randomZeroToOne()`を使うのでしたね。）

`1%`は`0.01`と表現します。正しい確率で木が生えているか注意しましょう。`grid`と`newGrid`の使い分けには注意しましょう！

>[答え]
> `update`関数はこの時点でこのようになっているはずです:
>
```
public override func update() {
    newGrid = grid
>    
    for x in 0..<grid.count {
        for y in 0..<grid[x].count {
            let tile = grid[x][y]
            if tile == nil {
                if randomZeroToOne() < 0.001 {
                    newGrid[x][y] = "🌲"
                }
            }
        }
    }
>    
    grid = newGrid
}
```

![Growing trees](./bruceSprucesteen.png)

コードを実行してプレイボタンを押して見ましょう！少し時間を書けて、木が増えていく様子を見てみましょう。少し変化を感じるまで時間がかかりますが（`0.1%`がとても小さい値だからです）、すごく自然なシミュレーションですね。

# 木が燃え、炎が広がるシミュレーション

さて、次は火が燃え広がるシミュレーションをコードで表してみましょう。🌲の周りに🔥があれば、次のターンで🌲が🔥になるというルールでしたね。さて早速挑戦して見ましょう。

> [アクション]
> このルールをコードで書くにはどうしたら良いでしょうか？
>
> （ヒント）ループの中で、確認しているセルが🌲かどうかをチェックします。もしそうだったら、`getNeighborPositions`を使って木の周りの位置データを取って、その位置データを使って🔥がないかどうか確認します。もし一つでも🔥があれば、🌲のセルを🔥に変えます。
>
> `.x` と `.y`　を使うことで、`getNeighborPositions`で返ってきた値の`x` と `y`のデータを取得することができます。

<!--  -->

> [答え]
> `update`関数は下のようになっているはずです:
>
```
public override func update() {
    newGrid = grid
>
    for x in 0..<grid.count {
        for y in 0..<grid[x].count {
            let tile = grid[x][y]
            if tile == nil {
                if randomZeroToOne() < 0.001 {
                    newGrid[x][y] = "🌲"
                }
            } else if tile == "🌲" {
                let neighborCords = getNeighborPositions(x: x, y: y)
                for neighborCoord in neighborCords {
                    let neighbor = grid[neighborCoord.x][neighborCoord.y]
                    if neighbor == "🔥" {
                        newGrid[x][y] = "🔥"
                    }
                }
            }
        }
    }
>
    grid = newGrid
}
```

![It was always burning since the world's been turning](./weDidntStartTheFire.png "It was always burning, since the world's been turning")

コードを実行してプレイボタンを押しましょう。パレットから🔥を選んで、セルをタップして🔥を追加しましょう。木に炎が燃え移るシミュレーションが見られるはずです。

> [アクション]
> 炎の広がり方を観察して見ましょう。もしおかしなところがあれば、戻ってコードを確認して見ましょう。

# 炎の静まり

今の状態では、🔥は全く静まりません。🔥についての最後のルールを付け加える必要があります。

> [アクション]
> 🔥についての最後のルールを付け加えましょう。
>
> これまで書いた条件のコードの下に、新たな条件を付け加えます。`grid`の中身を繰り返し確認して、もしセルに🔥があれば、`newGrid`のそのセルをnilに書き換えます。

<!--  -->

> [答え]
> `update`関数は下のようになっているはずです:
>
```
public override func update() {
    newGrid = grid
>
    for x in 0..<grid.count {
        for y in 0..<grid[x].count {
            let tile = grid[x][y]
            if tile == nil {
                if randomZeroToOne() < 0.001 {
                    newGrid[x][y] = "🌲"
                }
            } else if tile == "🌲" {
                let neighborCords = getNeighborPositions(x: x, y: y)
                for neighborCoord in neighborCords {
                    let neighbor = grid[neighborCoord.x][neighborCoord.y]
                    if neighbor == "🔥" {
                        newGrid[x][y] = "🔥"
                    }
                }
            } else if tile == "🔥" {
                newGrid[x][y] = nil
            }
        }
    }
>
    grid = newGrid
}
```

コードを実行してプレイボタンを押しましょう。また同じように、🔥をいくつかセルに追加して、火が燃え移り、🔥が消えていく様子を確認しましょう。

![Burning down the forest](./burningTrees.gif)

# 最終チェック

> [アクション]
> [このページ](https://mikinomura.github.io/simulating-japanese/model/?local=forest/0_growth&play=0&edit=1)をまた開いて、シミュレーションと自分のシミュレーションを比べて見ましょう。同じように動いていますか？

おめでとうございます！これで森と炎のシミュレーションができました。次のページでは、もっとリアルなシミュレーションに近づけていきます。
