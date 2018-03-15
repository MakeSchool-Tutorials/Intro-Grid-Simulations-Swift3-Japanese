---
title: "Your Custom Simulation: Forest Fires!" "オリジナルのシミュレーション: 森の炎たち！"
slug: forest-fire
---

> [info]
> You can open an emoji search popup on any OS X computer by holding `control + command` and press the `space bar`.
> `control + command`を押しながら、`space bar`をクリックすると絵文字バーが開きます。

Now that we've successfully replicated our Game of Life logic in a separate Xcode project, it's time to make our own custom Simulation, complete with new rules and emojis – one that simulates the spread of forest fires!
ライフゲームのロジックを無事にXcodeプロジェクトに移すことができたので、いよいよオリジナルのカスタムシミュレーションー森の中の炎シミュレーションを作ってみましょう！

> [action]
> Open up [this page](http://ncase.me/simulating/) and read through "A Tiny Forest For Tiny Trees". We'll be implementing this simulation!
> このページを開き、”A Tiny Forest For Tiny Trees”を読みましょう。これをアプリに実装します！

Here is a summary of the rules that we're implementing:
今から実装していくルールのまとめ:
- **Setup(設定):**
  1. **Palette setup（パレットの設定):** 🌲 and 🔥 should be in the palette.🌲 と 🔥 はパレットに入っている。
  1. **Random seeding:** Create a `8x10` grid randomly populated with a `50%` chance of a 🌲 tile, or empty otherwise.`8x10`のグリッドを作り、`50%`の確率で🌲が現れ、そうでなければ空のままにする。
- **Update（アップデート）:**
  1. **Spawning trees（木の誕生):** An empty tile has a `0.1%` chance of becoming a 🌲.空のタイルは、次のターンで`0.1%`の確率で🌲になる。
  1. **Burning trees:（焼ける木）** 🌲 turns into 🔥 if any of it's neighbors are 🔥.もし周りに🔥があれば、🌲は次のターンで🔥に変わる。
  1. **Sizzling out (焼け切る木):** 🔥 dies out and becomes an empty tile.　🔥は次のターンで死に絶えて、空のタイルになる。

We'll be conquering each of this rules one at a time.
このルールを一つずつ攻略していきます。

## ForestFireSimulation クラス

Sounds like fun! To begin, let's make a new file for our new simulation.
楽しそうですね！まずは、この新しいシミュレーションのための新しいファイルを作りましょう。

> [action]
> Click on the `Models` group in the left sidebar. With it selected, navigate to `File > New File`, or press `command+N`. Create a new Swift file (`iOS > Source > Swift File`) entitled `ForestFireSimulation.swift`, and put it inside the folder `Grid-Simulations-Xcode/Grid-Simulations`. Watch this video to see how to do it!
> ![ms-video](assets/new-file.mp4)
> `Models`グループをクリックして、`File > New File`をクリックするか、もしくは`command+N`を押しましょう。`iOS > Source > Swift File`と進んで新しいSwiftファイルを作ります。タイトルは`ForestFireSimulation.swift`で、`Grid-Simulations-Xcode/Grid-Simulations`フォルダの中にこのファイルを保存します。このビデオをみてみましょう！

Great! Now let's make a new class for our `ForestFireSimulation`, inherited from `Simulation`.
さて次は`ForestFireSimulation`という`Simulation`から継承された新しいクラスを作ります。

> [action]
> In `ForestFireSimulation.swift`, insert the following code:
`ForestFireSimulation.swift`のファイルに、下のコードを入れてください。
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

# Scene Setup　シーンのセットアップ

In the Game of Life, we set the initial state through a text-based loader that automatically read the grid from a `.txt` file. In our new simulation we won't have a use for this since our start grid state will be randomly generated. We will be including our initialization code in the `setup()` function of our `ForestFireSimulation` class.
このゲームライフでは、初めの状態を`.txt`ファイルに保存して、そのファイルを読み込ませることで表示させています。この新しいシミュレーションでは、この方法は使えません。なぜなら、今回のシミュレーションは最初の状態がランダムになっていて毎回違うものが表示されるからです。かわりに、`ForestFireSimulation`クラスの`setup()`関数の中に、初めの状態を定義します。

First, let's remove the code that loads the grid state from file and replace it with our own logic:
まず、ファイルからグリッドの状態を読み込むコードを削除し、代わりに自分たちのロジックを入れましょう。

<!-- ACTION: UPDATE THESE INSTRUCTIONS AFTER ADDING SIMULATION CHOOSE SCREEN -->

> [action]
> Open `GameViewController.swift`. Remember, this is where the loading of our scene happens!
> `GameViewController.swift`を開きます。ここのファイルがシーンの読み込みをしています。
>
> Comment out lines 24 through 26 and uncomment lines 36 & 37. This will load in your `ForestFireSimulation` class with the correct palette!
> 24行目と26行目をコメントアウトして、36行目と37行目をアンコメントしましょう。こうすることで、`ForestFireSimulation`クラスのパレットを読み込みます。

# (Pseudo) Random number generation　（擬似）乱数を発生させよう

Before we go back to the `setup()` function in `ForestFireSimulation.swift` let's talk about random numbers
`setup()`関数に戻る前に、乱数について考えてみましょう

Computers generally utilize an algorithm called [Pseudorandom Number Generator](https://en.wikipedia.org/wiki/Pseudorandom_number_generator) in order to generate sufficiently random numbers.
コンピュータは[擬似乱数](https://ja.wikipedia.org/wiki/%E6%93%AC%E4%BC%BC%E4%B9%B1%E6%95%B0)と呼ばれるアルゴリズムをよく用いています。
Swift provides several ways to do this, and here is one:
Swiftではいくつかのやり方でこのアルゴリズムを使うことができます。
例:

```swift
arc4random()
```

This will return a `Int` between values 0 and `RAND_MAX` – a global constant you can use in your code. So what if you instead want a `Double`, between the range 0 and 1?
このコードは0から`RAND_MAX`（自分のコードの中で使うことができる最大値、グローバル定数）までの`Int`を返します。では、もし0から1の間のランダムな`Double`が欲しかったらどうすればいいでしょうか？

```swift
Double(arc4random()) / Double(UInt32.max)
```

Since you'll be using this often, we've created a function called `randomZeroToOne()` that returns a `Double` between zero and one. You can use it anywhere in this app!
これはよく使うので、`randomZeroToOne()`という関数を作っておきました。この関数は0から1までの`Double`型の値を返します。このアプリの中でいつでも使える便利な関数です！

> [info]
> You have to cast both values to `Double`, since Swift will not implicitly convert `Int`s to `Double`s through mathematical operations. This point differentiates Swift from other languages like Java.
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

Let's move on and apply this logic to our `setup()` function.
では`setup()`関数にこのロジックを応用してみましょう。

# Randomly seeding the grid　ランダムに木を生やそう

Our guidelines mentioned that we were to create a 8x10 grid, randomly filled with a 50% chance of 🌲. So, let's start by creating a 2D array!
初めにまとめたガイドラインには、8x10のグリッドに、50%の確率で🌲が埋まると書かれていました。それを２次元配列を使って作り始めましょう！

From inside the `ForestFireSimulation` class, our grid variable can be accessed via `grid`. So, let's set `grid` to be a new 2D array:
`ForestFireSimulation`クラスの中から、グリッドは`grid`と呼び出すことで使うことができます。さあ、`grid`を編集して新しい2次元配列を作りましょう。

> [action]
> Insert the following into the `setup()` function in `ForestFireSimulation.swift`:
> `ForestFireSimulation.swift`の中の`setup()`の中に下記のコードを入れましょう。
>
```swift
grid = [[Character?]](repeating: [Character?](repeating: nil, count: 10), count: 10)
```

Now let's iterate through each tile in our 2D grid, and set a tile to 🌲 based on a 50% chance. But how do we do that?
次は、2Dのグリッドの一つ一つのタイルを繰り返し、50%の確率で🌲を入れていく作業をしましょう。でもどうすればいいのでしょう？

Remember how we said you have access to a `randomZeroToOne()` function that returned a value between 0 and 1? If we compare that number against `0.5`, we'll have a 50% chance of succeeding!
`randomZeroToOne()`関数を覚えていますか？これは0から1の間の値をランダムに返してくれる関数でしたね。この関数で返って来る値と`0.5`を比べれば、50%の確率を表現できることになりますね！

> [action]
> Go ahead and write the code to populate the tiles with 🌲. Do you remember how to iterate through elements of an array? Iterate through them and on each, change the value to 🌲 if `randomZeroToOne()` returns a value less than `0.5`.
> コードを書いてタイルを🌲で埋めましょう。配列の中身を繰り返し操作する方法を覚えていますか？
繰り返し操作をする中で、もし`randomZeroToOne()`が`0.5`より小さい値の時、🌲をセットしましょう。

<!--  -->

> [solution]
> Done? Your `setup()` function should look something like this:
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

If you run your code, you should now see random 🌲 filling the tiles!
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

# Retrieving neighbors 周りのセルのデータを見てみよう

Before we dive into the update loop, we should write some helper functions that will retrieve the neighbors of each position in the grid.
Update関数に入る前に、まずいくつかのヘルパー関数を作りましょう。このヘルパー関数は各セルの周りのデータを取得します。

## isLegalPosition

First, let's make a helper function, that determines if a grid position is in bounds of the grid, and returns a `Bool`. Let's call this function `isLegalPosition(x: Int, y: Int)`.
まず一つ目のヘルパー関数は、確認しようとしているセルがグリッドの中に収まっているかを確認して、`Bool`型のデータを返します。（つまり、もしグリッドの中に収まっていたら`true`を、そうでなければ`false`を返します)
この関数の名前を`isLegalPosition(x: Int, y:Int)`と名付けましょう。

> [action]
> Create a new function, right below the empty `update()` function block. It should look like this:
>まだ空の`update()`関数の下に、新しい関数を作りましょう。
>
```swift
func isLegalPosition(x: Int, y: Int) -> Bool {
>
}
```
> Now, insert code that returns true if the given x and y coordinates are inside the `grid` bounds, and false otherwise.
> そして、引数になっているxとyが`grid`の中に収まっているかを確認して、収まっていたらtrue, そうでなければfalseを返すコードを書きましょう。
<!--  -->

> [solution]
> Your `isLegalPosition` function should look like this:
`isLegalPosition`関数はこのようになっているはずです:
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

Next, we'll write a function to return the positions of a cell's neighbors -- `getNeighborPositions(x originX: Int, y originY: Int) -> [(x: Int, y: Int)]`. It should return an `Array` of `(Int, Int)` tuples representing the `(x, y)` location of each neighbor within the bounds of the grid.
次に、セルの周りの位置番号を取得する関数を書きます。名前は`getNeighborPositions(x originX: Int, y originY: Int) -> [(x: Int, y: Int)]`。この関数は`(Int, Int)`というタプル型の`配列`を返します。つまり、セルの周りの位置番号のリストです。

### Wait, wait, wait. What is a `tuple`?　タプル型って？

What's this return type, you ask? The parentheses syntax represents Swift's _tuple_ type, which you can read about [here](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID329). Basically, it's a way for you to quickly house multiple variables of potentially different types in one container. It's pretty much a lightweight class! So in this example, our function returns an _array_ of _tuples_ – containing two `Int`s, for the `x` and `y` indices.
`()`を使って表すデータのことをタプル型と言って、[ここ](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html#//apple_ref/doc/uid/TP40014097-CH5-ID329)から詳細を見ることができます（英語）。タプルは、異なるデータ型の値をひとまとめにできる簡単な方法です。今回、この関数は _タプル_　（`x`と`y`の値が入ったデータ）の配列を返します。

Tuples can be instantiated like this:
タプルの書き方の例:

```swift
let tuple = (2, 3)
```

...and if the tuple is a _named_ tuple, as is the case in our return statement, you can access the individual elements like this:
タプルはそれぞれの値に名前をつけることができて、その名前を使って後からデータを呼び出すことができます。
例:

```swift
let namedTuple = (x: 2, y: 3)
let x = namedTuple.x
let y = namedTuple.y
```

> [action]
> Create a new function, right below the `isLegalPosition` function. It should look like this:
> `isLegalPosition`関数の下に新しい関数を作りましょう。下のようになるはずです:
>
```swift
func getNeighborPositions(x originX: Int, y originY: Int) -> [(x: Int, y: Int)] {
>
}
```
> Now, insert code that returns an `Array` of `(Int, Int)` tuples representing the `(x, y)` location of each neighbor within the bounds of the grid.
>さて、この関数の中にセルの周りの`(x, y)`の値の配列を返すようなコードを書きましょう。
>
> How do you know if a grid position is legal? Call the function you just wrote – `isLegalPosition()`! Remember, you can use the `for x in a...b` syntax to traverse from values `a` and `b` (inclusively). You can append to a `(Int, Int)` tuple to an `Array` by calling `myArray.append((xValue, yValue))`.
> 計算をしたセルの周りの番号がグリッドの外にはみ出していないかどうか、どうやって確かめますか？ `isLegalPosition()`関数はそのための関数ですね！ `for x in a...b`を使うと、`a`から`b`までの値を全て繰り返し使うことができるのを覚えていますか？　配列に新しい`(Int, Int)`タプルの値を付け加えたい時は、`myArray.append((xValue, yValue))`と書きます。
>
> Before you get started, ask yourself: what ranges do you need to loop over? How will you make sure not to add `(originX, originY)` to the returned `Array`?
> 始める前に、あるセルの周りのセルの位置番号を取得するということは、どこからどこまでの番号を確認する必要があるのか考えて見ましょう。　`(originX, originY)`（自分自身）を間違えて入れないためにはどんなコードを書けばいいか少し注意して見ましょう。

<!--  -->

>[solution]
> Your `getNeighborPositions` function should look like this:
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

# Spawning trees　木を増やす

Great! Now that we have a few helper functions, we can get started with growing our forest by spawning new trees. If you take a look at the rules again, then you'll notice that an empty tile has a `0.1%` chance of becoming a 🌲.
いくつかヘルパー関数を書きましたね。これで、新しい木を生やすコードに取り掛かることができます。ルールを思い出しましょう。空のセルは次のターンで`0.1%`の確率で🌲になります。

Since these rules (like Conway's Game of Life) will require a temporary grid to hold the new state of the simulation. Let's get started by adding an instance variable to hold this temporary grid!
さて、このルールを実際にコードで表現するためには、”仮の”グリッドを作ることが必要です。`grid`をいきなり書き換えてしまうと、前のターンのデータと次のターンのデータが混ざってしまうので、別々にする必要があるのです。

> [action]
> Create a `newGrid: [[Character?]]` instance variable in the `ForestFireSimulation` class.
>`ForestFireSimulation`クラスの中に、`newGrid: [[Character?]]`インスタンスを作りましょう。
> Add this line above and outside of your `setup()` function:
> `setup()`上（関数の外側）に、下のコードを入れましょう。
>
```
var newGrid: [[Character?]] = []
```
>
Now, at the start of your `update()` method, set the `newGrid` equal to `grid`. Then create loops to iterate over each element in `grid` (see Game of Life if you need a hint). At the end of the function (after the loops), set `grid` equal to `newGrid` so that the state will update!
> まず`update()`関数の始めに、`newGrid`の値を`grid`にセットします。それから、`grid`の中の一つ一つの値を使いながら、`newGrid`の値を書き換えていきます。最後に、`grid`の値を`newGrid`の値にセットすれば完了です！
>
Inside of those loops, you should check if a tile is empty (remember, empty tiles are equal to `nil`). If it is empty, grab a value from `randomZeroToOne()` and spawn a tree with a `0.1%` chance.
> ループ処理の中で、セルが空（`nil`）かどうか確認して、もし空だったら、`0.1%`の確率で木が植えられるようにコードを書きます。（確率には`randomZeroToOne()`を使うのでしたね。）

Remember, `1%` translates to a value of `0.01`! Make sure you are spawning trees at the correct rate. It's also extremely important to remember that you should be reading values from `grid` and setting values to `newGrid`!
`1%`は`0.01`と表現します。正しい確率で木が生えているか注意しましょう。`grid`と`newGrid`の使い分けには注意しましょう！

>[solution]
> Your `update` function should currently look like this:
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

Run your code and tap the Play button! Spend some time watching the trees grow... It might take a little while before you see one popup (`0.1%` is small) but it's still pretty peaceful.
コードを実行してプレイボタンを押して見ましょう！少し時間を書けて、木が増えていく様子を見てみましょう。少し変化を感じるまで時間がかかりますが（`0.1%`がとても小さい値だからです）、すごく自然なシミュレーションですね。

# Burning trees and spreading fire　木が燃え、炎が広がるシミュレーション

Now that we've just centered ourself with the zen of create, it's time to write the code for fire propagation. The rule for 🔥 is a 🌲 turns into 🔥 if any of it's neighbors are 🔥. We already have all the loops and helper functions that we'll need so let's jump right in!
さて、次は火が燃え広がるシミュレーションをコードで表してみましょう。🌲の周りに🔥があれば、次のターンで🌲が🔥になるというルールでしたね。さて早速挑戦して見ましょう。

> [action]
> Can you modify the code for this new rule without any hints? Get started now if you think you can!
> このルールをコードで書くにはどうしたら良いでしょうか？
>
> If you need a hint... Add another check inside the loops to see if the current tile is a 🌲. If it is, call `getNeighborPositions`. Iterate over those values in `grid` and check for 🔥 tiles. If you find one, set the 🌲 tile in `newGrid` to 🔥!
> （ヒント）ループの中で、確認しているセルが🌲かどうかをチェックします。もしそうだったら、`getNeighborPositions`を使って木の周りの位置データを取って、その位置データを使って🔥がないかどうか確認します。もし一つでも🔥があれば、🌲のセルを🔥に変えます。
>
> Remember, you can access the `x` and `y` values of a tuple returned by `getNeighborPositions` with `.x` and `.y` on any of the elements in the `Array`.
> `.x` と `.y`　を使うことで、`getNeighborPositions`で返ってきた値の`x` と `y`のデータを取得することができます。

<!--  -->

> [solution]
> Your `update` function should currently look like this:
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

Ready for some destruction to balance out the creation from the last step? Run your code and tap the Play button. When you're ready to go, select 🔥 from the palette and tap a tile, and sit back while the forest gets engulfed in fire. Pretty scary, huh?
コードを実行してプレイボタンを押しましょう。パレットから🔥を選んで、セルをタップして🔥を追加しましょう。木に炎が燃え移るシミュレーションが見られるはずです。

> [action]
> Pay close attention to how your fire spreads. If it is not spreading to every neighboring 🌲 (diagonals included) then you should go back and debug your `getNeighborPositions` helper function!
> 炎の広がり方を観察して見ましょう。もしおかしなところがあれば、戻ってコードを確認して見ましょう。

# Sizzling out 炎の静まり

The fire is currently taking over the forest and never dies out! We need to implement the last rule of 🔥 dies out and becomes an empty tile. This one is pretty straightforward -- no random chance or neighbors involved!
今の状態では、🔥は全く静まりません。🔥についての最後のルールを付け加える必要があります。

> [action]
> Implement the last rule in your simulation so the fire dies out each generation.
> 🔥についての最後のルールを付け加えましょう。
>
> Add a third check on the tiles as you loop over `grid`. If it is a 🔥, update that tile in `newGrid` to be nil.
> これまで書いた条件のコードの下に、新たな条件を付け加えます。`grid`の中身を繰り返し確認して、もしセルに🔥があれば、`newGrid`のそのセルをnilに書き換えます。

<!--  -->

> [solution]
> Your `update` function should currently look like this:
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

Run your code and tap the Play button. When you're ready to go, select 🔥 from the palette and tap a tile, and sit back while the forest gets engulfed in fire. This time, the 🔥 should roll through the forest and die out after a generation.
コードを実行してプレイボタンを押しましょう。また同じように、🔥をいくつかセルに追加して、火が燃え移り、🔥が消えていく様子を確認しましょう。

![Burning down the forest](./burningTrees.gif)

# Checking your results　最終チェック

> [action]
> Open up [this page](http://ncase.me/simulating/) again and compare your simulation to "A Tiny Forest For Tiny Trees". Do your results match theirs?
> [このページ](http://ncase.me/simulating/)をまた開いて、このページにある"A Tiny Forest For Tiny Trees"シミュレーションと自分のシミュレーションを比べて見ましょう。同じように動いていますか？

Congrats! You've made yourself a working simulation of a forest fire in action. On the next page, we'll continue to add more and more realism to the simulation!
おめでとうございます！これで森と炎のシミュレーションができました。次のページでは、もっとリアルなシミュレーションに近づけていきます。
