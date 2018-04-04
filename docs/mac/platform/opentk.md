---
title: OpenTK の概要
description: この記事では、OpenTK Xamarin.Mac アプリケーションで作業を紹介します。 作成してゲーム ウィンドウを維持する、単純なオブジェクトを表示して、オブジェクトをユーザーに表示する方法を説明します。
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: d5f19dac8dc362e1ac4a36cbe5cf5db3e31ae363
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-opentk"></a>OpenTK の概要

OpenTK (「Open ツールキット) は、、下位の高度な c# ライブラリ OpenGL と OpenCL OpenAL の扱いが簡単しますです。 OpenTK は、ゲーム、科学アプリケーションまたはその他の 3 D グラフィックスを必要とするプロジェクト、オーディオや計算の機能を使用できます。 この記事では、Xamarin.Mac アプリで OpenTK の使用の概要を提供します。

[![](opentk-images/intro01.png "実行のサンプル アプリ")](opentk-images/intro01.png#lightbox)

この記事でしれません Xamarin.Mac アプリケーションにおける OpenTK の基礎をについて説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)と[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)セクションでは、これとは、主な概念と、この記事で使用する方法について説明します。

確認することも、 [c# を公開するクラス/OBJECTIVE-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素に、c# クラスをワイヤ アップするために使用します。

<a name="About_OpenTK" />

## <a name="about-opentk"></a>OpenTK について

前述のように、OpenTK (「Open ツールキット) は、高度な低レベル c# ライブラリ OpenGL と OpenCL OpenAL の操作が容易にするです。 Xamarin.Mac アプリで OpenTK を使用すると、次の機能があります。

- **迅速な開発**-OpenTK、コーディングのワークフローを改善し、容易にし、すぐに、エラーをキャッチするには、強力なデータ型とインラインのドキュメントを提供します。
- **簡単に統合**-OpenTK は、.NET アプリケーションで簡単に統合するように設計されました。
- **制限の緩やかなライセンス**-OpenTK MIT/X11 ライセンスに基づいて頒布されて、無料で見つかります。
- **豊富なタイプ セーフなバインド**-OpenTK OpenGL、OpenGL の最新バージョンをサポートしています | ES、OpenAL および拡張機能の自動読み込みと OpenCL エラー チェックおよびインライン ドキュメント。
- **柔軟な GUI オプション**-OpenTK がネイティブには、パフォーマンスの高いゲーム ウィンドウは、ゲームや Xamarin.Mac 専用に設計されました。
- **完全に管理、CLS 準拠コードの**-OpenTK macOS ありませんアンマネージ ライブラリでの 32 ビットおよび 64 ビット バージョンをサポートしています。
- **3D の Math Toolkit** OpenTK 提供`Vector`、 `Matrix`、`Quaternion`と`Bezier`の 3D の Math ツールキットを使用して構造体。

OpenTK は、ゲーム、科学アプリケーションまたはその他の 3 D グラフィックスを必要とするプロジェクト、オーディオや計算の機能を使用できます。

詳細についてを参照してください[「Open ツールキット](http://www.opentk.com)web サイトです。

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>OpenTK クイック スタート

Xamarin.Mac アプリで OpenTK を使用して簡単な説明、として行うゲーム ビューを開き、Mac アプリケーションのメイン ウィンドウにユーザーに三角形を表示するにそのビューと attachs ゲーム ビューで単純な三角形を描画する簡単なアプリケーションを作成します。

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>新しいプロジェクトを開始

Mac 用 Visual Studio を起動し、新しい Xamarin.Mac ソリューションを作成します。 選択**Mac** > **アプリ** > **全般** > **Cocoa アプリ**:

[![](opentk-images/sample01.png "新しい Cocoa アプリを追加します。")](opentk-images/sample01.png#lightbox)

入力`MacOpenTK`の**プロジェクト名**:

[![](opentk-images/sample02.png "プロジェクト名を設定します。")](opentk-images/sample02.png#lightbox)

をクリックして、**作成**新しいプロジェクトをビルドするボタンをクリックします。

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>OpenTK を含む

Xamarin.Mac アプリケーションで開いている TK を使用することができます、前に、OpenTK アセンブリへの参照を含める必要があります。 **ソリューション エクスプ ローラー**を右クリックし、**参照**フォルダーと選択**参照の編集.**.

チェック ボックスをオン`OpenTK` をクリックし、 **OK**ボタン。

[![](opentk-images/sample03.png "プロジェクト参照の編集")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>OpenTK を使用します。

新しいプロジェクトを作成をダブルクリックして、`MainWindow.cs`ファイルで、**ソリューション エクスプ ローラー**編集用に開きます。 ように、`MainWindow`次のようなクラスの検索。

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

このコードを以下で詳細を確認しましょう。

<a name="Required_APIs" />

### <a name="required-apis"></a>必要な Api

Xamarin.Mac クラスで OpenTK を使用するには、複数の参照が必要です。 定義の開始時に、次を追加して`using`ステートメント。

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
この最小限のセットは、任意のクラス OpenTK を使用する必要があります。

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>ゲームのビューを追加します。

次に、マイクロソフトとの対話 OpenTK すべてが含まれてし、結果を表示するゲーム ビューを作成する必要があります。 次のコードを使用しました。

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

ここで加えられたゲーム ビュー メイン Mac ウィンドウと同じサイズで新しいウィンドウのコンテンツ ビューを置き換える`MonoMacGameView`です。 ウィンドウの既存のコンテンツを交換しているためしたビューに自動的にサイズ変更されますメイン ウィンドウのサイズを変更します。

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>イベントへの応答

既定のイベントに応答する各ゲームのビューをいくつかがあります。 このセクションでは必要な主要なイベントを説明します。

<a name="The_Load_Event" />

### <a name="the-load-event"></a>Load イベント

`Load`イベントは、場所イメージ、テクスチャまたは音楽などのディスクからリソースを読み込めません。 シンプルのテスト アプリに対しては使用しない、`Load`イベント、参照用に含めていたが、します。

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>サイズ変更イベント

`Resize`ゲーム ビューのサイズが変更されるたびに、イベントを呼び出す必要があります。 サンプル アプリでのおが行って GL ビューポート、ゲーム ビューを自動 Mac のメイン ウィンドウの幅を変更する) と同じサイズを次のコード。

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>UpdateFrame イベント

`UpdateFrame`イベントは、ユーザー入力を処理するには、オブジェクトの位置、実行の物理または AI 計算の更新に使用します。 シンプルのテスト アプリに対しては使用しない、`UpdateFrame`イベント、参照用に含めていたが、します。 

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> OpenTK の Xamarin.Mac 実装は含まれません、`Input API`では、Apple を使用して、キーボードを追加するための Api とマウスをサポートする必要があります。 必要に応じて、カスタムのインスタンスを作成することができます、`MonoMacGameView`をオーバーライドし、`KeyDown`と`KeyUp`メソッドです。

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>RenderFrame イベント

`RenderFrame`イベントには (描画) を表示するために使用されるコードが含まれています、グラフィックです。 この例のアプリのゲーム ビューお入力に単純な三角形。 

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

通常への呼び出しになるのレンダリング コード`GL.Clear`を既存の要素を削除する前に、新しい要素を描画します。

> [!IMPORTANT]
> Xamarin.Mac バージョン OpenTK の**しない**を呼び出す、`SwapBuffers`のメソッド、`MonoMacGameView`レンダリング コードの最後のインスタンス。 こうと、レンダリングされるビューを表示するのではなく、迅速に strobe ゲーム ビュー。

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>ゲームのビューを実行しています。

すべての必要なイベントを定義およびゲーム ビューは、アプリのメイン Mac ウィンドウにアタッチされている、ゲーム ビューを実行し、グラフィックスの表示が読み取られることです。 次のコードを使用します。

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

更新をゲーム ビューの対象が目的のフレーム レートに渡す、選択したこの例の`60`(標準テレビとして同じのリフレッシュ レート) の秒あたりのフレームです。

それでは、アプリを実行して、出力を参照してください。

[![](opentk-images/intro01.png "アプリの出力のサンプル")](opentk-images/intro01.png#lightbox)

ウィンドウをサイズ変更、ゲーム ビューが存在して、三角形がサイズを変更し、同様にリアルタイム更新にもなります。

<a name="Where_to_Next" />

### <a name="where-to-next"></a>次にどこですか。

OpenTk Xamarin.mac アプリケーションで行う操作の基本には、次へお試しにどのようないくつかの提案を次に示します。

- 三角形の色とゲームのビューの背景色を変更してみて、`Load`と`RenderFrame`イベント。
- 色を変更するユーザーが内のキーを押したときの三角形を作成、`UpdateFrame`と`RenderFrame`イベントしたり、独自のカスタム`MonoMacGameView`クラスし、オーバーライド、`KeyUp`と`KeyDown`メソッドです。
- 内の対応するキーを使用して、画面に移動する三角形を作成、`UpdateFrame`イベント。 ヒント: を使用して、`Matrix4.CreateTranslation`メソッドを呼び出し、平行移動行列を作成、`GL.LoadMatrix`でファイルを読み込む方法、`RenderFrame`イベント。
- 使用して、`for`のいくつかの三角形をレンダリングするループ、`RenderFrame`イベント。
- 3D スペースで三角形の異なるビューを提供するカメラに回転させます。 ヒント: を使用して、`Matrix4.CreateTranslation`メソッドを呼び出し、平行移動行列を作成、`GL.LoadMatrix`メソッドを読み込みます。 使用することも、 `Vector2`、 `Vector3`、`Vector4`と`Matrix4`カメラ操作のためのクラスです。

例についてを参照してください、 [OpenTK サンプル GitHub](https://github.com/opentk/opentk/tree/master/Source/Examples)リポジトリです。 OpenTK の使用例の公式の一覧が含まれています。 OpenTK の Xamarin.Mac バージョンを使用する場合、これらの例を改変する必要があります。

例についてはより複雑な Xamarin.Mac OpenTK 実装を参照してください、 [MonoMacGameView](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/)サンプルです。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事の内容が、簡単に見て OpenTK Xamarin.Mac アプリケーションで作業しています。 ゲーム ウィンドウを作成する方法を説明しました Mac ウィンドウに、[ゲーム] ウィンドウをアタッチする方法と、[ゲーム] ウィンドウでの単純な図形をレンダリングする方法です。

## <a name="related-links"></a>関連リンク

- [MacOpenTK (サンプル)](https://developer.xamarin.com/samples/mac/MacOpenTK/)
- [MonoMacGameView (サンプル)](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [ウィンドウの操作](~/mac/user-interface/window.md)
- [開いている Toolkit](http://www.opentk.com)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
