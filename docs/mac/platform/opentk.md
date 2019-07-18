---
title: Xamarin.Mac で OpenTK の概要
description: この記事では、OpenTK、Xamarin.Mac アプリケーションでの操作に概要を示します。 作成および維持 [Game] ウィンドウ、単純なオブジェクトをレンダリングし、ユーザーにオブジェクトを表示することについて説明します。
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 835b8cd0f2e689c4d7d4cace1d846543863b7393
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61032688"
---
# <a name="introduction-to-opentk-in-xamarinmac"></a>Xamarin.Mac で OpenTK の概要

OpenTK (が開いている Toolkit) は、高度な低レベル c# ライブラリ OpenGL と OpenCL OpenAL の操作は簡単です。 OpenTK は、ゲーム、科学アプリケーションまたはその他の 3D グラフィックスを必要とするプロジェクト、オーディオまたはコンピューティングの機能を使用できます。 この記事では、Xamarin.Mac アプリで OpenTK の使用を簡単に紹介します。

[![](opentk-images/intro01.png "アプリの実行例")](opentk-images/intro01.png#lightbox)

この記事では、Xamarin.Mac アプリケーションで OpenTK の基礎を取り上げます。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)と[Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions)ほどのセクションでは、主要な概念と、この記事で使用する方法について説明します。

見てしたい場合があります、 [c# を公開するクラス/メソッドを OBJECTIVE-C](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac の内部](~/mac/internals/how-it-works.md)、について説明します、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素を c# クラスをワイヤ アップするために使用します。

<a name="About_OpenTK" />

## <a name="about-opentk"></a>OpenTK について

前述のように、OpenTK (が開いている Toolkit) は、高度な低レベル c# ライブラリ OpenGL と OpenCL OpenAL の操作は簡単です。 OpenTK を Xamarin.Mac アプリで使用すると、次の機能が用意されています。

- **迅速な開発**-OpenTK、コーディングのワークフローを改善し、容易にしより早くエラーをキャッチするには、強力なデータ型とインライン ドキュメントを提供します。
- **簡単に統合**-OpenTK が .NET アプリケーションと簡単に統合するように設計されました。
- **制限の緩やかなライセンス**-OpenTK/X11 MIT ライセンスのもとし、は完全に無料です。
- **豊富でタイプ セーフなバインド**-OpenTK OpenGL、OpenGL の最新バージョンのサポート | ES、OpenAL および拡張機能の自動読み込み、OpenCL エラー チェックとインライン ドキュメント。
- **柔軟な GUI オプション**-OpenTK、ネイティブの提供、高性能のゲームのウィンドウは、ゲーム、および Xamarin.Mac 専用に設計されました。
- **完全管理型の CLS 準拠コードの**-OpenTK の macOS では、アンマネージ ライブラリではなく 32 ビットおよび 64 ビット バージョンをサポートしています。
- **3D の数学 Toolkit** OpenTK 提供`Vector`、 `Matrix`、`Quaternion`と`Bezier`3D その数学 Toolkit を使用して構造体。

OpenTK は、ゲーム、科学アプリケーションまたはその他の 3D グラフィックスを必要とするプロジェクト、オーディオまたはコンピューティングの機能を使用できます。

詳細についてを参照してください[、開いているツールキット](http://www.opentk.com)web サイト。

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>OpenTK のクイック スタート

OpenTK、Xamarin.Mac アプリを使用する簡単な概要については、としては、ゲーム ビューを開き、そのビューと attachs ゲーム ビューで単純な三角形、三角形をユーザーに表示する、Mac アプリのメイン ウィンドウを表示するため、単純なアプリケーションを作成するいきます。

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>新しいプロジェクトを開始

Visual Studio Mac を起動し、新しい Xamarin.Mac ソリューションを作成します。 選択**Mac** > **アプリ** > **全般** > **Cocoa アプリ**:

[![](opentk-images/sample01.png "新しい Cocoa アプリを追加します。")](opentk-images/sample01.png#lightbox)

入力`MacOpenTK`の**プロジェクト名**:

[![](opentk-images/sample02.png "プロジェクト名を設定します。")](opentk-images/sample02.png#lightbox)

をクリックして、**作成**新しいプロジェクトをビルドするボタンをクリックします。

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>OpenTK を含む

開いている TK を使用するには、Xamarin.Mac アプリケーションで、前に、OpenTK アセンブリへの参照を含める必要があります。 **ソリューション エクスプ ローラー**を右クリックし、**参照**フォルダーと選択**参照の編集.**.

チェック ボックスをオン`OpenTK` をクリックし、 **OK**ボタン。

[![](opentk-images/sample03.png "プロジェクト参照の編集")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>OpenTK の使用

新しいプロジェクトを作成、ダブルクリック、`MainWindow.cs`ファイル、**ソリューション エクスプ ローラー**編集用に開きます。 ように、`MainWindow`クラスの次のようになります。

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacOpenTK
{
    public partial class MainWindow : NSWindow
    {
        #region Computed Properties
        public MonoMacGameView Game { get; set; }
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create new Game View and replace the window content with it
            Game = new MonoMacGameView(ContentView.Frame);
            ContentView = Game;

            // Wire-up any required Game events
            Game.Load += (sender, e) =>
            {
                // TODO: Initialize settings, load textures and sounds here
            };

            Game.Resize += (sender, e) =>
            {
                // Adjust the GL view to be the same size as the window
                GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
            };

            Game.UpdateFrame += (sender, e) =>
            {
                // TODO: Add any game logic or physics
            };

            Game.RenderFrame += (sender, e) =>
            {
                // Setup buffer
                GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
                GL.MatrixMode(MatrixMode.Projection);

                // Draw a simple triangle
                GL.LoadIdentity();
                GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
                GL.Begin(BeginMode.Triangles);
                GL.Color3(Color.MidnightBlue);
                GL.Vertex2(-1.0f, 1.0f);
                GL.Color3(Color.SpringGreen);
                GL.Vertex2(0.0f, -1.0f);
                GL.Color3(Color.Ivory);
                GL.Vertex2(1.0f, 1.0f);
                GL.End();

            };

            // Run the game at 60 updates per second
            Game.Run(60.0);
        }
        #endregion
    }
}
```

このコードを以下に詳しく見てみましょう。

<a name="Required_APIs" />

### <a name="required-apis"></a>必要な Api

OpenTK を Xamarin.Mac クラスで使用するには、いくつかの参照が必要です。 定義の先頭に、次を含めましたが`using`ステートメント。

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using CoreGraphics;
```
この最小限のセットは、OpenTK を使用して任意のクラスの必要になります。

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>ゲームのビューの追加

次に、ゲーム、対話 OpenTK のすべてが含まれてし、結果を表示するビューを作成する必要があります。 次のコードを使用しました。

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

ここで行いましたゲーム ビュー、メイン Mac ウィンドウと同じサイズで新しいウィンドウのコンテンツ ビューを置き換える`MonoMacGameView`します。 既存のウィンドウのコンテンツを交換しましたので、付けたビューに自動的にサイズを調整するメインの Windows のサイズが変更されたときにします。

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>イベントへの応答

各ゲーム ビューへの応答がいくつかの既定のイベントがあります。 このセクションでは必要な主要なイベントを説明します。

<a name="The_Load_Event" />

### <a name="the-load-event"></a>読み込みイベント

`Load`イベントは、イメージ、テクスチャまたは音楽などのディスクからリソースを読み込む場所。 テスト アプリをシンプルなは使用しません、`Load`イベント、参照に含めたは。

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>サイズ変更イベント

`Resize`ゲーム ビューのサイズが変更されるたびに、イベントを呼び出す必要があります。 このサンプル アプリを行っています GL ビューポート (これを自動で Mac のメイン ウィンドウのサイズを変更する)、ゲーム ビューと同じサイズを次のコード。

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>UpdateFrame イベント

`UpdateFrame`イベントは、ユーザー入力を処理して、オブジェクトの位置、物理運動の実行または AI 計算を更新するために使用します。 テスト アプリをシンプルなは使用しません、`UpdateFrame`イベント、参照に含めたは。 

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> OpenTK の Xamarin.Mac 実装は含まれません、 `Input API`、Apple Api を追加するキーボードとマウスのサポートを使用する必要があります。 カスタム インスタンスを作成する必要に応じて、`MonoMacGameView`をオーバーライドし、`KeyDown`と`KeyUp`メソッド。

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>RenderFrame イベント

`RenderFrame`イベントには、(描画) を表示するために使用されるコードが含まれています。 グラフィック。 この例のアプリでは、ゲーム ビュー入力単純な三角形で。 

```csharp
Game.RenderFrame += (sender, e) =>
{
    // Setup buffer
    GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
    GL.MatrixMode(MatrixMode.Projection);

    // Draw a simple triangle
    GL.LoadIdentity();
    GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
    GL.Begin(BeginMode.Triangles);
    GL.Color3(Color.MidnightBlue);
    GL.Vertex2(-1.0f, 1.0f);
    GL.Color3(Color.SpringGreen);
    GL.Vertex2(0.0f, -1.0f);
    GL.Color3(Color.Ivory);
    GL.Vertex2(1.0f, 1.0f);
    GL.End();

};
```

レンダリング コードが呼び出しを使用するには通常`GL.Clear`を既存の要素を削除する前に、新しい要素を描画します。

> [!IMPORTANT]
> OpenTK の Xamarin.Mac バージョン**しない**呼び出し、`SwapBuffers`のメソッド、`MonoMacGameView`レンダリング コードの最後のインスタンス。 こうと、レンダリングされるビューを表示するのではなく、迅速に strobe ゲーム ビュー。

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>ゲームのビューを実行しています。

すべての必要なイベントを定義、ゲーム ビューを実行し、グラフィックを表示するのには読んだゲーム ビューは、アプリのメイン Mac ウィンドウにアタッチされています。 次のコードを使用します。

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

更新するため、ゲーム ビューと考えている目的のフレーム レートで渡します、例では、選択した`60`(標準テレビとして同じのリフレッシュ レート) の秒あたりのフレーム。

アプリを実行して、出力を参照してください。

[![](opentk-images/intro01.png "アプリの出力のサンプル")](opentk-images/intro01.png#lightbox)

ウィンドウのサイズを変更場合、ゲーム ビューもされて存在する三角形のサイズ変更し、もリアルタイムで更新されます。

<a name="Where_to_Next" />

### <a name="where-to-next"></a>次の場所ですか。

OpenTk 完了、Xamarin.mac アプリケーションでの操作の基礎について、次を試して何の推奨事項を示します。

- 三角形の色とゲームのビューの背景色を変更してみて、`Load`と`RenderFrame`イベント。
- 三角形の色を変更するユーザーのキーを押したときに、`UpdateFrame`と`RenderFrame`イベントや、独自のカスタム`MonoMacGameView`クラスし、オーバーライド、`KeyUp`と`KeyDown`メソッド。
- 内の対応のキーを使用して画面上を移動する三角形を作成、`UpdateFrame`イベント。 ヒント: を使用して、`Matrix4.CreateTranslation`平行移動行列と呼び出しを作成する方法、`GL.LoadMatrix`メソッドを読み込むことで、`RenderFrame`イベント。
- 使用して、`for`ループでいくつかの三角形を表示するために、`RenderFrame`イベント。
- 3 次元空間で三角形の別のビューを提供するカメラを回転します。 ヒント: を使用して、`Matrix4.CreateTranslation`メソッドの呼び出しと平行移動行列を作成する、`GL.LoadMatrix`メソッドを読み込みます。 使用することも、 `Vector2`、 `Vector3`、`Vector4`と`Matrix4`カメラ操作用のクラス。

例についてを参照してください、 [OpenTK のサンプル GitHub](https://github.com/opentk/opentk/tree/master/Source/Examples)リポジトリ。 OpenTK の使用例の公式の一覧が含まれています。 OpenTK の Xamarin.Mac のバージョンを使用するため、これらの例を調整する必要があります。

OpenTK 実装のより複雑な Xamarin.Mac 例を参照してください、 [MonoMacGameView](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/)サンプル。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションで OpenTK の使用方法を簡単に説明をしました。 ゲームのウィンドウを作成する方法を説明しました Mac ウィンドウに [Game] ウィンドウをアタッチする方法と、[ゲーム] ウィンドウで単純な図形をレンダリングする方法。

## <a name="related-links"></a>関連リンク

- [MacOpenTK (サンプル)](https://developer.xamarin.com/samples/mac/MacOpenTK/)
- [MonoMacGameView (サンプル)](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows の操作](~/mac/user-interface/window.md)
- [開いているツールキット](http://www.opentk.com)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
