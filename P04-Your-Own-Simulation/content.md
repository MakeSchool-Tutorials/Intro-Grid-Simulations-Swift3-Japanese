---
title: "オリジナルのシミュレーションを作ろう！"
slug: make-your-own
---

In this tutorial, we've learned how to make use of two dimensional arrays, helper functions, and tuples. We programed Conway's Game of Life and a few forest fire simulations along the way. Now it's your turn – make your own awesome, original simulation!
このチュートリアルの中で、２次元の配列、ヘルパー関数、タプルの使い方を学びました。ライフゲームをプログラムして、いくつかの森と炎のシミュレーションを作りました。さぁ、今度は自分のオリジナルのシミュレーションを作る時間です！

Here are some guidelines to help you through the process:
ガイドライン:

1. Imagine any real-life or made-up scenario that you might be able to represent with a grid of cells. Will it be chickens running around, laying eggs? Sheep being chased by a wolf? Then, think about what the "players" of the simulation are – what will you want to keep track in your grid? Decide on emoji or characters for these players, and place them in your palette.
グリッドで表現できる、現実の世界や架空の世界をイメージしましょう。鶏と卵のシミュレーションはどうでしょう？狼に追いかけられる羊は？
次にどんな"プレーヤー"が必要かを決めましょう。そのプレーヤーになる絵文字や文字を決めて、パレットに入れましょう。
2. Think about what _states_ your Simulation should encompass. In `aTinyForest`, there were three states to each grid cell: 🔥, 🌲, and empty. Each of these states had pathways in which the cell could transition to another state. What are the different "actions" that each player in your simulation makes? How would that translate to the logic that you will write?
シミュレーションで表現したい _状態_ を考えてみましょう。`aTinyForest`関数では、🔥, 🌲, 空という３つの状態が存在しましたね。それぞれの状態は、別の状態に移るためのルールを持っていました。あなたの考えるオリジナルのシミュレーションでは、どんなロジックやアクションが必要でしょうか？
3. Once you've decided on an idea, make yourself a new class for your simulation – just like we did for `ForestFiresSimulation`. Comment out lines 36 & 37. Uncomment lines 47 & 47. Replace `YourSimulationClass()` with your actual class name. In your class, override and start implementing the `setup()` and `update()` functions. You will probably want to copy the helper functions from `ForestFireSimulation` over!
アイディアを思いついたら、`ForestFiresSimulation`で作ったような、新しいクラスを作りましょう。36と37行目をコメントアウトして、47行目と48行目をアンコメントしましょう。`YourSimulationClass()`を実際のクラス名に書き換えましょう。そのクラスでは、`setup()`と`update()`関数を書き換えていきます。`ForestFireSimulation`の中にある、前のページで既に作ったヘルパー関数をコピーして再利用しましょう！
4. Make sure your code compiles, and hit Run! Step through and see your simulation in action to make sure that your code behaves as intended.
コードが完成したら、実行してシミュレーションを実験しましょう。自分の思い通りのシミュレーションができましたか？

For quick reference, here's some quick documentation on the classes and functions you can use.
下のクラスや関数を参考にして、使ってみましょう。

## Simulationクラス

The Simulation class is what you will subclass your new Simulation from. It contains your `grid` variable that you will modify to update your simulation state, and `setup()` and `update()` that you can override to implement your simulation behavior. Here are some other useful functions:
Simulationクラスから、新たなシミュレーションをサブクラスとして作ることができます。このシミュレーションクラスは、`grid`変数や`setup()`, `update()`関数などを持っていて、サブクラスを作ると、それらを書き換ながら使うことができるようになります。
例えば他にはこのような関数も持っています:

- `init()` Default initializer. Sets `grid` to `[]`, or an empty array.初期化の設定をする関数。`grid`を`[]`に設定したりします。
- `init(other: Simulation)` Initializer that takes in another Simulation as input. Copies the other simulation's `grid`.　他のシミュレーションを引数として使って初期化をする関数。
- `init?(file: String)` Initializer that takes in a path to a text file to read the grid size and characters from. Note that in the text file, `0` denotes an empty cell. If anything goes wrong during the file parse, this function will return `nil`. Look at `map01.txt` for an example of how to create your text file.
テキストファイルを引数にして初期化をする関数。このテキストファイルは、グリッドのサイズや文字が入っています。このテキストファイルの中に入っている`0`という数字は、空の状態を意味します。`map01.txt`を例として見てみましょう。
- `setup()` The setup function. Called once, before the Simulation is loaded onto the scene.
セットアップ関数。シミュレーションが画面に読み込まれる前に一度だけ呼び出されます。
- `update()` The update function. Called each time the timer ticks during play, or when the user taps the step button.
アップデート関数。プレイ中にタイマーが進む度に呼び出されます。ユーザーがストップボタンを押すときも呼び出されます。
- `grid` A variable of type `[[Character?]]` that stores the state of the simulation. Read and write to this variable in your `setup()` and `update()` functions to make changes to the grid. Make sure to set it up as an `8x10` grid!
`[[Character?]]`タイプの変数。シミュレーションの状態を保存しています。`setup()`や `update()` の中で書き換えられることで、グリッドの状況が変化します。

## SimulationSceneクラス

The SimulationScene is what runs the whole operation – loading of Simulations, UI and touch event handling, and timer operations. Here are a few functions that you might find useful:
SimulationSceneは、全体のプログラムを動かす役割をします。Simulationsを読み込んだり、UIやタッチイベント、タイマーなどを管理します。
いくつかの例:

- `init(sim: Simulation, palette: [Character?])` Creates a SimulationScene with the given Simulation and character palette. This will call `setup()` on the Simulation.
SimulationSceneを与えられたシミュレーションとパレットを元に作ります。この中で、`setup()`関数が呼び出されます。
- `play()` Starts playing the simulation when this is called.
シミュレーションをスタートさせる関数です。
- `pause()` Pauses the simulation when this is called.`
シミュレーションをストップさせる関数です。

Have fun creating your simulation!
さあ、シミュレーションづくりを楽しみましょう！！
