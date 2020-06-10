---
title: Xamarin. Mac での OpenTK の概要
description: この記事では、Xamarin. Mac アプリケーションで OpenTK を使用する方法の概要について説明します。 ゲームウィンドウの作成と保守、単純なオブジェクトのレンダリング、およびオブジェクトのユーザーへの表示について説明します。
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: bd5217173e00de7d09e4ec14a5acf4d6a8389b10
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84574445"
---
# <a name="introduction-to-opentk-in-xamarinmac"></a>Xamarin. Mac での OpenTK の概要

OpenTK (オープンツールキット) は、OpenGL、OpenCL、OpenAL を簡単に操作できる高度な低レベルの C# ライブラリです。 OpenTK は、ゲーム、科学的なアプリケーション、または3D グラフィックス、オーディオ、または計算機能を必要とするその他のプロジェクトに使用できます。 この記事では、OpenTK アプリでの使用方法について簡単に説明します。

[![](opentk-images/intro01.png "An example app run")](opentk-images/intro01.png#lightbox)

この記事では、Xamarin. Mac アプリケーションでの OpenTK の基本について説明します。 この記事で使用する主要な概念と手法について説明しているように、最初に[Hello, Mac](~/mac/get-started/hello-mac.md)の記事「 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)」と「[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)」セクションをご覧になることを強くお勧めします。

「 [C# のクラス/メソッドを](~/mac/internals/how-it-works.md) [Xamarin. Mac の内部](~/mac/internals/how-it-works.md)ドキュメントの前に公開する」セクションを参照して `Register` `Export` ください。 c# クラスを目的の c オブジェクトと UI 要素に接続するために使用されるコマンドとコマンドについても説明します。

<a name="About_OpenTK"></a>

## <a name="about-opentk"></a>OpenTK について

前述のように、OpenTK (オープンツールキット) は、OpenGL、OpenCL、OpenAL を簡単に操作できる高度な低レベルの C# ライブラリです。 Xamarin. Mac アプリで OpenTK を使用すると、次の機能が提供されます。

- **迅速な開発**-OpenTK は、コーディングワークフローを改善し、エラーをより簡単かつ迅速にキャッチするための、強力なデータ型とインラインドキュメントを提供します。
- **簡単な統合**-OpenTK は、.net アプリケーションと簡単に統合できるように設計されています。
- OpenTK は MIT/X11**ライセンスで配布**され、完全に無料です。
- **リッチでタイプセーフなバインド**-OpenTK は、最新バージョンの OpenGL、OPENGL | ES、Openal、OpenCL をサポートし、自動拡張読み込み、エラーチェック、インラインドキュメントをサポートしています。
- **柔軟な GUI オプション**-OpenTK は、ゲームおよび Xamarin. Mac 専用に設計された、ネイティブでハイパフォーマンスなゲームウィンドウを提供します。
- **完全に管理された CLS 準拠コード OpenTK は、** アンマネージライブラリを使用しない32ビットおよび64ビットバージョンの macOS をサポートしています。
- **3D Math Toolkit**OpenTK `Vector` は、、 `Matrix` 、およびの `Quaternion` `Bezier` 各構造体を 3d Math Toolkit を介して提供します。

OpenTK は、ゲーム、科学的なアプリケーション、または3D グラフィックス、オーディオ、または計算機能を必要とするその他のプロジェクトに使用できます。

詳細については、 [Open Toolkit の](https://opentk.net)web サイトを参照してください。

<a name="OpenTK_Quickstart"></a>

## <a name="opentk-quickstart"></a>OpenTK クイックスタート

Xamarin. Mac アプリで OpenTK を使用する方法の概要として、簡単なアプリケーションを作成します。このアプリケーションでは、ゲームビューを開き、そのビューで単純な三角形をレンダリングして、ユーザーに三角形を表示するためのゲームビューを Mac アプリのメインウィンドウに添付します。

<a name="Starting_a_New_Project"></a>

### <a name="starting-a-new-project"></a>新しいプロジェクトの開始

Visual Studio for Mac を開始し、新しい Xamarin. Mac ソリューションを作成します。 **Mac**  >  **アプリ**の  >  **一般**  >  **共同 coa アプリ**を選択します。

[![](opentk-images/sample01.png "Adding a new Cocoa App")](opentk-images/sample01.png#lightbox)

`MacOpenTK`**プロジェクト名**として「」を入力します。

[![](opentk-images/sample02.png "Setting the project name")](opentk-images/sample02.png#lightbox)

[**作成**] ボタンをクリックして、新しいプロジェクトをビルドします。

<a name="Including_OpenTK"></a>

### <a name="including-opentk"></a>OpenTK を含む

Xamarin. Mac アプリケーションで Open TK を使用するには、OpenTK アセンブリへの参照を含める必要があります。 **ソリューションエクスプローラー**で、[**参照**] フォルダーを右クリックし、[**参照の編集**] を選択します。

チェックボックスをオンにし `OpenTK` 、[ **OK** ] ボタンをクリックします。

[![](opentk-images/sample03.png "Editing the project references")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK"></a>

### <a name="using-opentk"></a>OpenTK の使用

新しいプロジェクトが作成されたら、ソリューションエクスプローラー内のファイルをダブルクリックし `MainWindow.cs` て編集用に**Solution Explorer**開きます。 クラスは `MainWindow` 次のようになります。

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

次に、このコードについて詳しく説明します。

<a name="Required_APIs"></a>

### <a name="required-apis"></a>必要な Api

Xamarin. Mac クラスで OpenTK を使用するには、いくつかの参照が必要です。 定義の先頭には、次のステートメントが含まれてい `using` ます。

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

OpenTK を使用するクラスには、この最小セットが必要です。

<a name="Adding_the_Game_View"></a>

### <a name="adding-the-game-view"></a>ゲームビューの追加

次に、OpenTK とのすべての対話を含むゲームビューを作成し、結果を表示する必要があります。 次のコードを使用しています。

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

ここでは、メインの Mac ウィンドウと同じサイズのゲームビューを作成し、ウィンドウのコンテンツビューを新しいに置き換えました `MonoMacGameView` 。 既存のウィンドウの内容を置き換えたため、メインウィンドウのサイズを変更すると、表示されるビューのサイズは自動的に変更されます。

<a name="Responding_to_Events"></a>

### <a name="responding-to-events"></a>イベントへの応答

各ゲームビューで応答する必要がある既定のイベントがいくつかあります。 ここでは、必要な主なイベントについて説明します。

<a name="The_Load_Event"></a>

### <a name="the-load-event"></a>読み込みイベント

`Load`イベントは、イメージ、テクスチャ、音楽などのディスクからリソースを読み込む場所です。 単純なテストアプリでは、イベントを使用していません `Load` が、参照用に含まれています。

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event"></a>

### <a name="the-resize-event"></a>サイズ変更イベント

`Resize`ゲームビューのサイズが変更されるたびに、イベントを呼び出す必要があります。 このサンプルアプリでは、次のコードを使用して、GL ビューポートがゲームビュー (Mac のメインウィンドウによって自動的にサイズ変更される) と同じサイズになるようにしています。

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event"></a>

### <a name="the-updateframe-event"></a>UpdateFrame イベント

イベントは、 `UpdateFrame` ユーザー入力の処理、オブジェクトの位置の更新、実行の物理または AI 計算に使用されます。 単純なテストアプリでは、イベントを使用していません `UpdateFrame` が、参照用に含まれています。

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> OpenTK の Xamarin 実装にはが含まれていない `Input API` ため、キーボードとマウスのサポートを追加するには、Apple 提供の api を使用する必要があります。 必要に応じて、のカスタムインスタンスを作成 `MonoMacGameView` し、 `KeyDown` メソッドとメソッドをオーバーライドでき `KeyUp` ます。

<a name="The_RenderFrame_Event"></a>

### <a name="the-renderframe-event"></a>RenderFrame イベント

`RenderFrame`イベントには、グラフィックスのレンダリング (描画) に使用されるコードが含まれています。 このサンプルアプリでは、ゲームビューに単純な三角形を入力します。

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

通常、 `GL.Clear` 新しい要素を描画する前に既存の要素を削除するために、レンダリングコードはを呼び出します。

> [!IMPORTANT]
> OpenTK のバージョンでは、 **do not** `SwapBuffers` `MonoMacGameView` レンダリングコードの最後にインスタンスのメソッドを呼び出さないでください。 これにより、レンダリングされたビューを表示する代わりに、ゲームビューが急激にストロボになります。

<a name="Running_the_Game_View"></a>

### <a name="running-the-game-view"></a>ゲームビューの実行

すべての必須イベントの定義と、アプリのメインの Mac ウィンドウに関連付けられているゲームビューを使用して、ゲームビューを実行し、グラフィックスを表示します。 次のコードを使用します。

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

ゲームビューを更新するために必要なフレームレートを渡します。この例では、 `60` 1 秒あたりのフレーム数 (通常のテレビと同じリフレッシュレート) を選択しています。

アプリを実行し、出力を確認してみましょう。

[![](opentk-images/intro01.png "A sample of the apps output")](opentk-images/intro01.png#lightbox)

ウィンドウのサイズを変更すると、ゲームビューも存在し、三角形のサイズが変更され、リアルタイムで更新されます。

<a name="Where_to_Next"></a>

### <a name="where-to-next"></a>次の場所

Xamarin. mac アプリケーションで OpenTk を操作する方法の基本については、次のいくつかの提案を参考にしてください。

- イベントとイベントで、ゲームビューの三角形の色と背景色を変更してみてください `Load` `RenderFrame` 。
- ユーザーがイベントとイベントでキーを押したとき、 `UpdateFrame` `RenderFrame` または独自のカスタムクラスを作成 `MonoMacGameView` し、メソッドとメソッドをオーバーライドしたときに、三角形の色を変更し `KeyUp` `KeyDown` ます。
- イベントの認識キーを使用して、三角形を画面上で移動させ `UpdateFrame` ます。 ヒント: メソッドを使用して `Matrix4.CreateTranslation` 変換行列を作成し、メソッドを呼び出して `GL.LoadMatrix` イベントに読み込み `RenderFrame` ます。
- `for`イベントに複数の三角形を表示するには、ループを使用し `RenderFrame` ます。
- 3D 空間の三角形の別のビューを表示するには、カメラを回転させます。 ヒント: メソッドを使用して `Matrix4.CreateTranslation` 変換行列を作成し、 `GL.LoadMatrix` メソッドを呼び出してそれを読み込みます。 `Vector2` `Vector3` `Vector4` カメラの操作には、、、およびクラスを使用することもでき `Matrix4` ます。

その他の例については、 [OpenTK Samples github](https://github.com/opentk/opentk/tree/master/Source/Examples)リポジトリを参照してください。 OpenTK の使用例の公式リストが含まれています。 OpenTK の Xamarin バージョンでを使用するために、これらの例を適合させる必要があります。

OpenTK 実装のより複雑な Xamarin. Mac の例については、[モノ Mac view](https://docs.microsoft.com/samples/xamarin/mac-samples/monomacgamewindow)サンプルを参照してください。

<a name="Summary"></a>

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリケーションでの OpenTK の使用について簡単に説明しました。 ゲームウィンドウを作成する方法、ゲームウィンドウを Mac ウィンドウに接続する方法、およびゲームウィンドウに単純な図形を描画する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [MacOpenTK (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macopentk)
- [モノ Mac/ビュー (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/monomacgamewindow)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows の操作](~/mac/user-interface/window.md)
- [オープンツールキット](https://opentk.net)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
