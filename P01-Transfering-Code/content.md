---
title: "Transferring Your Code"　"コードを移そう"
slug: transferring-code
---

Our first step is to transfer your code from Swift Playgrounds to the Xcode starter project. But to do this, you'll need to understand the hierarchy of different files in the project, and what their roles are. Bring up the starter project, and look at the sidebar on the left. With the folders expanded, it look like this:
最初のステップは自分のコードをSwiftプレイグラウンドからXcodeのスタータプロジェクトに移すことです。このためには、プロジェクトの中のファイルの構造とその意味を理解しておく必要があります。スタータプロジェクトを開き、左のサイドバーを見てみましょう。フォルダーを開くと、下のような構造が見えるはずです:

![The left sidebar of your Xcode project](assets/project-hierarchy.png)

Here are each of the folders and their corresponding roles:
それぞれのフォルダの意味:

- __VCs__: VC stands for View Controller, and is one of the concepts of the _Model-View-Controller programming pattern_ utilized by Apple's user interface code. The View Controller is essentially responsible for transferring data and data changes from the "Model" (the data which we want to represent on the screen), to the "View" (the visual representation of our data). Our project only has one View Controller: `GameViewController`, which sets up the Simulation and a scene to display it. We'll edit this when we want to display scenes other than our default Game of Life simulation.
:VCというのは"View Controller (ビューコントローラ)"の略で、これは"Model View Controller Programming Pattern"と呼ばれる考え方の一つです。この考え方はAppleのユーザーインターフェースコードで使用されています。ビューコントローラはデータを"Model"(モデル: 画面に写したいデータのこと)から"View"(ビュー:　画面に実際に表示されているビジュアル)に変換する役割を持っています。このプロジェクトでは、`GameViewController`というビューコントローラーだけを使います。`GameViewController`はシミュレーションを用意して、画面に表示する役割をします。デフォルトのライフゲームシミュレーション以外の画面を表示させたい時などに、このファイルを書きかえます。
  - You can learn more about the MVC pattern in [Apple's documentation](https://developer.apple.com/library/mac/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html).
  - MVCパターンについて詳しく学んでみたい人は: [Appleの公式ドキュメント(英語)](https://developer.apple.com/library/mac/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html)
- __Utilities__: Here we have code that serves supplementary functions for our project. In this case, we have a handy `StreamReader` class that handles the reading of grid data from a .txt file.
: これは補足的な役割をするコードを入れておくための場所です。今回は、`StreamReader`という、.txtファイルからグリッドのデータを読み込むためのクラスを使います。
- __Models__: The "M" in the _MVC pattern_, the Models essentially contain code for the data we're trying to present, and logic for how that data will change. In this case, the models we have are our Simulations! You will edit code in `GameOfLifeSimulation.swift`, which will house our update logic for Game of Life.
: "Model(モデル)"は、画面に表示させたいデータを扱うためのコードが入っています。今回の場合、モデルは”Simulations”のことですね！`GameOfLifeSimulation.swift`を書きかえることで、ライフゲームのロジックを更新することができます。
- __Nodes__: These files contain the behind-the-scenes code that represent the UI that you see on the screen – such as the grid, and the play/pause and step buttons. The folder is named "Node" because all of these classes inherit from SKNode – which is the generic Node class provided by Apple's SpriteKit. You won't need to touch these files for the most part.
: この中に入っているファイルは、画面上のUI(ユーザーインターフェース)を表示させるための、裏側で密かに働いているコードが入っています。例えば、グリッド、プレイ・ポーズ、ステップボタンなどです。名前が”Node”なのは、全てのクラスがSKNodeというものから継承されているからです。（SKNodeとはAppleのSpriteKitから提供されているNodeクラスのことです。)今回はほとんどこのファイルは触る必要がありません。
- __Resources__: These contain the images and text files that are necessary for the project. For example, `01.txt` contains a simple Glider pattern, which you can load into your grid. This is useful for testing if your Game of Life logic is working correctly.
: イメージやテキストファイルが入っています。例えば、`01.txt`はグライダーのシンプルなパターンが入っていて、グリッドにロードして使うことができます。これは、書いたコードがきちんと動くかどうか、テストをするのに役立ちます。
- __AppDelegate.swift__: This file is essentially the "entry point" for your iOS application. On application startup, a function inside `AppDelegate.swift` determines what View Controller to load, so that your simulation code can start running. We won't edit `AppDelegate` in our tutorial, but it's still important to know where the code starts running!
:このファイルは、iOSアプリケーションの重要な"エントリーポイント"です。アプリケーションが起動する時、`AppDelegate.swift`の中のファンクションがどのView Controllerをロードするかを命令します。このおかげで、シミュレーションが動き出すことができるのです。今回は`AppDelegate`は編集しませんが、このファイルがアプリケーションの立ち上げに必須であることを理解しておくのは大切です。

When working with large Xcode projects, it's often important to group code that executes similar functions in groups together. Understanding the relationships between multiple classes, and how it relates to the project hierarchy, will help you jump into existing projects more easily, and make your future projects more organized!
大規模なXcodeプロジェクトにとりかかる時、コードをグループ分けしておくことは大切です。コード同士、ファイル同士の関係や構造を理解しやすくすることは、後になって自分がプロジェクトに取り掛かる時にとても楽になるでしょう。

> [action]
> Quickly go through each of the files and folders in the sidebar, and make sure you can take a general guess as to what their roles are in the project overall. Pay special attention to the files that you'll edit, namely `GameOfLifeSimulation.swift`, and `GameViewController.swift`.
> さっと全てのフォルダとファイルを見て、だいたいそれぞれがどんな役割をしているのかを考えてみよう。特に`GameOfLifeSimulation.swift`,`GameViewController.swift`は大切なのでよく見てみよう。

# Running the Starter code スタータコードを動かしてみる

Run the project now, by hitting the "Play" triangle on the top left of your project window. If compilation finishes successfully, an iOS simulator window should pop up, and start running a starter simulation: an unmoving Glider, like below.
上の左側にあるPlayの三角マークをクリックして、プロジェクトを実行しましょう。コンパイルが成功したら、iOSシミュレーターが自動的に開いてスタータシミュレーション(動かないグライダー)が実行されるはずです。

![The starter code in action.](assets/starter-state.png)

You should also be able to press the Start & Step buttons, and also tap on the grid to turn any of the grid cells into 👾. You can tap again to turn the cell back off, or you can tap a different character in the palette above, to change the emoji that you want to "paint" on the grid.
StartボタンとStepボタンも押せるはずです。また、グリッドの中もクリックできて、クリックするとそのセルが👾のマークに変わるはずです。もう一度タップするとまた空のセルに変わるでしょう。次に、パレットの中の絵文字をクリックすると、今度はその絵文字をグリッドに追加できるようになります。

> [action]
> Run the starter code as is, and play with the UI. Try tapping the "Step" and "Play" buttons, selecting characters from the palette, and toggling cells on the grid.
> そのままスタータプロジェクトを実行して、遊んでみましょう。"Step" "Play"ボタンを押してみたり、パレットから絵文字を選んだりしてみましょう。

To see the code responsible for getting this much on the screen, open up `GameViewController.swift`. As said before, this is a View Controller – responsible for setting up the scene for our simulation. The initialization code is here:
この画面で起きていることを理解するために、`GameViewController.swift`を開いてみましょう。これは、View Controllerで、シミュレーションのシーンを設定する役割を果たします。
初期設定のためのコード:

```swift
let filePath = Bundle.main.path(forResource: "map01", ofType: "txt")!
let sim = GameOfLifeSimulation(file: filePath)!
let palette: [Character?] = ["👾", "😸", nil, nil, nil, nil, nil, nil]

// ...
// Setup code for other simulations commented out
// ...

let scene = SimulationScene(fileNamed: "SimulationScene")!
scene.setup(simulation: sim, palette: palette)
```

Here, the View Controller loads up a `GameOfLifeSimulation` using filePath `map01.txt` to fill up its initial grid state, and uses it to setup the `SimulationScene`, using its `sim` and `palette` variables as parameters. In order to see why our Glider isn't moving, let's open up `GameOfLifeSimulation.swift`:
ここでは、このビューコントローラーが、`map01.txt`を使って`GameOfLifeSimulation`をロードし、最初のグリッドの画面を決めて、`sim`と名付けています。この`sim`と`palette`を使って、`SimulationScene`をセットアップしています。
次になぜこのグライダーが動いていないのか、`GameOfLifeSimulation.swift`を開いてみてみましょう。

```swift
public override func update() {

}

func getAlive(grid: [[Character?]], column x: Int, row y: Int) -> Int {
    return 1
}

func countNeighbors(grid: [[Character?]], column x: Int, row y: Int) -> Int {
    return 1
}
```

Oh no! All of our functions are empty! To fix this, we'll need to copy over our code we wrote in the previous Playground over to this file, to make our Glider move.
全ての関数が空っぽです！これを直すために、前回のプレイグラウンドで書いたコードをコピーして移していきます。無事に移し終わると、実行した時にグライダーが動くはずです。

# Transferring Game Of Life logic　ゲームライフのロジックを移す

First of all, we need to transfer our logic for `countNeighbors` and `getAlive`, the functions we wrote to count the number of alive neighbors for each cell.
まずはじめに、`countNeighbors`と`getAlive`のロジックを移しましょう。(周りの生きているいるセルを数える関数のことでしたね。)

> [action]
> Go to Page 6 of our Swift Playgrounds course. Copy over the contents of function `getAlive`, and copy it to the empty `getAlive` function in Xcode, getting rid of the placeholder `return 1` statement. Do the same for function `countNeighbors`.
> Swiftプレイグラウンドの6ページを開きましょう。`getAlive`関数をコピーしてXcodeの`getAlive`関数に移し、`return 1`を削除します。`countNeighbors`関数に関しても同じことを繰り返します。

Great! Now we're ready to copy over the update loop for Game Of Life.
やった！これでゲームライフのアップデートループをコピーする準備が整いました。

> [action]
> Go to Page 7 of the Swift Playgrounds course. Copy over the contents of function `update` over to the corresponding function in `GameOfLifeSimulation.swift` in the Xcode project.
>
> Now run the project! Tap the "Start" triangle at the top left.
> Swiftプレイグラウンドの7ページを開いてください。`update`関数を`GameOfLifeSimulation.swift`の中の対応する関数にコピーして移しましょう。

Does your Glider move as expected? If not, go back to your Swift playgrounds and make sure your code is correct. When you're done, you can move on to the next step!
あなたのグライダーは予想通りに動きましたか？もしおかしければ、Swiftプレイグラウンドをもう一度よくみてコードが正しいか確認しましょう。完成したら次へ進みましょう！
