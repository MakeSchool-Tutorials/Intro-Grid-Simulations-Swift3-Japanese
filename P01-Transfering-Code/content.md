---
title: "コードを移そう"
slug: transferring-code
---

最初のステップは自分のコードをSwift PlaygroundsからXcodeのスタータプロジェクトに移すことです。このためには、プロジェクトの中のファイルの構造とその意味を理解しておく必要があります。スタータプロジェクトを開き、左のサイドバーを見てみましょう。フォルダーを開くと、下のような構造が見えるはずです:

![The left sidebar of your Xcode project](assets/project-hierarchy.png)

それぞれのフォルダの意味:

- __VCs__: :VCというのは"View Controller (ビューコントローラ)"の略で、これは"Model View Controller Programming Pattern"と呼ばれる考え方に出てくる要素の一つです。この考え方はAppleのユーザーインターフェースコードで使用されています。ビューコントローラはデータを"Model"(モデル: 画面に写したいデータのこと)から"View"(ビュー:　画面に実際に表示されているビジュアル)に変換する役割を持っています。このプロジェクトでは、`GameViewController`というビューコントローラーだけを使います。`GameViewController`はシミュレーションを用意して、画面に表示する役割をします。デフォルトのライフゲームシミュレーション以外の画面を表示させたい時などに、このファイルを書きかえます。
  - MVCパターンについて詳しく学んでみたい人は: [Appleの公式ドキュメント(英語)](https://developer.apple.com/library/mac/documentation/General/Conceptual/DevPedia-CocoaCore/MVC.html)
- __Utilities__: これは補足的な役割をするコードを入れておくための場所です。今回は、`StreamReader`という、.txtファイルからグリッドのデータを読み込むためのクラスを使います。
- __Models__: "Model(モデル)"は、画面に表示させたいデータや、ロジックを扱うためのコードが入っています。今回の場合、モデルは”Simulations”のことですね！`GameOfLifeSimulation.swift`を書きかえることで、ライフゲームのロジックを更新することができます。
- __Nodes__: この中に入っているファイルは、画面上のUI(ユーザーインターフェース)を表示させるための、裏側で密かに働いているコードが入っています。例えば、グリッド、プレイ・ポーズ、ステップボタンなどです。名前が”Node”なのは、全てのクラスがSKNodeというものから継承されているからです。（SKNodeとはAppleのSpriteKitから提供されているNodeクラスのことです。)今回はほとんどこのファイルは触る必要がありません。
- __Resources__: イメージやテキストファイルが入っています。例えば、`01.txt`はグライダーのシンプルなパターンが入っていて、グリッドにロードして使うことができます。これは、書いたコードがきちんと動くかどうか、テストをするのに役立ちます。
- __AppDelegate.swift__:このファイルは、iOSアプリケーションの重要な"エントリーポイント"です。アプリケーションが起動する時、`AppDelegate.swift`の中のファンクションがどのView Controllerをロードするかを命令します。このおかげで、シミュレーションが動き出すことができるのです。今回は`AppDelegate`は編集しませんが、このファイルがアプリケーションの立ち上げに必須であることを理解しておくのは大切です。

大規模なXcodeプロジェクトにとりかかる時、コードをグループ分けしておくことは大切です。コード同士、ファイル同士の関係や構造を理解しやすくすることは、後になって自分がプロジェクトに取り掛かる時にとても楽になるでしょう。

> [アクション]
> 全てのフォルダとファイルを見て、だいたいそれぞれがどんな役割をしているのかを考えてみましょう。特に`GameOfLifeSimulation.swift`,`GameViewController.swift`は大切なのでよく見てみましょう。

# スタータコードを動かしてみる

上の左側にあるPlayの三角マークをクリックして、プロジェクトを実行しましょう。コンパイルが成功したら、iOSシミュレーターが自動的に開いてスタータシミュレーション(動かないグライダー)が実行されるはずです。

![The starter code in action.](assets/starter-state.png)

StartボタンとStepボタンも押せるはずです。また、グリッドの中もクリックできて、クリックするとそのセルが👾のマークに変わるはずです。もう一度タップするとまた空のセルに変わるでしょう。次に、パレットの中の絵文字をクリックすると、今度はその絵文字をグリッドに追加できるようになります。

> [アクション]
> そのままスタータプロジェクトを実行して、遊んでみましょう。"Step" "Play"ボタンを押してみたり、パレットから絵文字を選んだりしてみましょう。

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

全ての関数が空っぽです！これを直すために、前回のプレイグラウンドで書いたコードをコピーして移していきます。無事に移し終わると、実行した時にグライダーが動くはずです。

# ライフゲームのロジックを移す

まずはじめに、`countNeighbors`と`getAlive`のロジックを移しましょう。(周りの生きているいるセルを数える関数のことでしたね。)

> [アクション]
> Swiftプレイグラウンドの6ページを開きましょう。`getAlive`関数をコピーしてXcodeの`getAlive`関数に移し、`return 1`を削除します。`countNeighbors`関数に関しても同じことを繰り返します。

やった！これでライフゲームのアップデートループをコピーする準備が整いました。

> [アクション]
> Swiftプレイグラウンドの7ページを開いてください。`update`関数を`GameOfLifeSimulation.swift`の中の対応する関数にコピーして移しましょう。

あなたのグライダーは予想通りに動きましたか？もしおかしければ、Swift Playgroundをもう一度よくみてコードが正しいか確認しましょう。完成したら次へ進みましょう！
