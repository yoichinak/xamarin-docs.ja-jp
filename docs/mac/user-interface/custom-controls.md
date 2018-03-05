---
title: "カスタム コントロールの作成"
description: "この記事では、カスタム コントロールを作成し、インターフェイスのビルダーに処理する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 675B9405-D9A7-49F0-94AD-417F10A71D11
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: f3d6301bc2c0237a268669fff437801bfb2657d1
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="creating-custom-controls"></a>カスタム コントロールの作成

_この記事では、カスタム コントロールを作成し、インターフェイスのビルダーに処理する方法について説明します。_

同じアクセス権がある Xamarin.Mac アプリケーションでは、c# と .NET で作業するときのユーザー コントロールで作業する開発者*Objective C*、 *Swift*と*Xcode*は. Xamarin.Mac は、Xcode と直接統合を使用すると Xcode の_インターフェイス ビルダー_を作成し、ユーザー コントロールを管理 (または必要に応じて c# コードで直接作成すること)。

MacOS には、さまざまな組み込みのユーザー コントロールが用意されています、機能が指定されていないボックスを提供するか (ゲーム インターフェイス) などのカスタム UI テーマを一致するようにカスタム コントロールを作成する必要のある時刻である可能性があります。

[ ![](custom-controls-images/intro01.png "カスタムの UI コントロールの例")](custom-controls-images/intro01.png)

この記事で Xamarin.Mac アプリケーションで再利用可能なカスタム ユーザー インターフェイス コントロールの作成の基本について説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)と[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)セクションでは、これとは、主な概念と、この記事で使用する方法について説明します。

確認することも、 [c# を公開するクラス/OBJECTIVE-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素に、c# クラスをワイヤ アップするために使用します。

<a name="Introduction-to-Outline-Views" />

## <a name="introduction-to-custom-controls"></a>カスタム コントロールの概要

前述のように、時間、再利用可能な Xamarin.Mac アプリの UI のユニークな機能を提供するか (ゲーム インターフェイス) などのカスタム UI テーマを作成する、カスタムのユーザー インターフェイス コントロールを作成する必要がある場合である可能性があります。

このような場合は、簡単にから継承できます`NSControl`し、アプリの ui の c# コードで、または Xcode のインターフェイスのビルダーに追加できるか、カスタム ツールを作成します。 継承することによって`NSControl`カスタム コントロールに自動的にすべての組み込みのユーザー インターフェイス コントロールが標準的な機能 (など`NSButton`)。

カスタム ユーザー インターフェイス コントロールには、(カスタム グラフを作成およびグラフィック ツール) と同様の情報だけが表示されたら、場合から継承する`NSView`の代わりに`NSControl`です。

基本クラスを使用すると、カスタム コントロールを作成するための基本的な手順は同じです。

ここでは、一意のユーザー インターフェイスのテーマと完全に機能のカスタム ユーザー インターフェイス コントロールを作成する例を提供するカスタムの上下を反転スイッチ コンポーネントを作成します。

<a name="Building-the-Custom-Control" />

## <a name="building-the-custom-control"></a>カスタム コントロールのビルド

以降では作成するカスタム コントロールは、ユーザー入力 (マウスの左ボタンのクリック) に対する応答は、ここから継承`NSControl`です。 この方法でカスタム コントロールが自動的にすべての組み込みのユーザー インターフェイス コントロールが標準的な機能を持ち標準 macOS コントロールのような応答します。

Mac 用 Visual Studio でのカスタム ユーザー インターフェイス コントロールを作成 (または新規作成) する Xamarin.Mac プロジェクトを開きます。 新しいクラスを追加し、それを呼び出す`NSFlipSwitch`:

[ ![](custom-controls-images/custom01.png "新しいクラスの追加")](custom-controls-images/custom01.png)

次に、編集、`NSFlipSwitch.cs`クラスし、次のようになります。

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using AppKit;
using CoreGraphics;

namespace MacCustomControl
{
    [Register("NSFlipSwitch")]
    public class NSFlipSwitch : NSControl
    {
        #region Private Variables
        private bool _value = false;
        #endregion

        #region Computed Properties
        public bool Value {
            get { return _value; }
            set {
                // Save value and force a redraw
                _value = value;
                NeedsDisplay = true;
            }
        }
        #endregion

        #region Constructors
        public NSFlipSwitch ()
        {
            // Init
            Initialize();
        }

        public NSFlipSwitch (IntPtr handle) : base (handle)
        {
            // Init
            Initialize();
        }

        [Export ("initWithFrame:")]
        public NSFlipSwitch (CGRect frameRect) : base(frameRect) {
            // Init
            Initialize();
        }

        private void Initialize() {
            this.WantsLayer = true;
            this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
        }
        #endregion

        #region Draw Methods
        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Use Core Graphic routines to draw our UI
            ...

        }
        #endregion

        #region Private Methods
        private void FlipSwitchState() {
            // Update state
            Value = !Value;
        }
        #endregion

    }
}
```

最初に、カスタム クラスから継承していることに注目してください`NSControl`を使用して、**登録**Objective C と Xcode のインターフェイスのビルダーには、このクラスを公開するコマンド。

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

次のセクションで、上記のコードの詳細の残りの部分を見てをみましょう。

<a name="Tracking-the-Controls-State" />

### <a name="tracking-the-controls-state"></a>コントロールの状態の追跡

ユーザーのカスタム コントロールはため、スイッチ、スイッチのオン/オフ状態を追跡することが必要です。 次のコードで処理する`NSFlipSwitch`:

```csharp
private bool _value = false;
...

public bool Value {
    get { return _value; }
    set {
        // Save value and force a redraw
        _value = value;
        NeedsDisplay = true;
    }
}
```

スイッチの状態が変更されたときに、UI を更新する方法が必要です。 作業を行うことで UI を再描画するコントロールを強制して`NeedsDisplay = true`です。

ユーザーのコントロールでは、1 つのオン/オフの状態 (たとえば 3 つの位置での複数の状態のスイッチ) よりも多く必要に応じて場合使用することも、 **Enum**状態を追跡します。 この例では、単純な**bool**を実行します。

オンとオフの切り替えの状態をスワップするヘルパー メソッドも追加されています。

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

後で、スイッチの状態が変化したときに、呼び出し元に通知するには、このヘルパー クラスを展開してあります。

<a name="Drawing-the-Controls-Interface" />

### <a name="drawing-the-controls-interface"></a>コントロールのインターフェイスの描画

コア グラフィック図面ルーチンを使用して、実行時に、カスタム コントロールのユーザー インターフェイスを描画するでしょう。 この作業を行うことができます、前に、コントロールのレイヤーを有効にする必要があります。 次のプライベート メソッドを使用してこの作業を行います。

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

このメソッドは、各コントロールが正しく構成されていることを確認するコントロールのコンス トラクターから呼び出されます。 例:

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

次に、オーバーライドする必要があります、`DrawRect`メソッドにコントロールを描画するためにコア グラフィック ルーチンを追加します。

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

おある調整コントロールのビジュアル表現の状態が変更されたときに (などから**で**に**オフ**)。 状態の変更をいつでもを使用して、`NeedsDisplay = true`コマンドを強制的にコントロールの新しい状態の再描画します。

<a name="Responding-to-User-Input" />

### <a name="responding-to-user-input"></a>ユーザー入力に応答してください。

カスタム コントロールへのユーザー入力を追加できること、2 つの基本的な方法がある:**マウス処理ルーチンのオーバーライド**または**ジェスチャ レコグナイザー**です。 を使用する方法については、ユーザーのコントロールに必要な機能に基づきます。

> [!IMPORTANT]
> カスタム コントロールを作成するため、いずれかを使用する必要があります**メソッドのオーバーライド**_または_**ジェスチャ レコグナイザー**、両方ではなく同じ時刻のように互いに矛盾が生じます。

<a name="Summary" />

#### <a name="handling-user-input-with-override-methods"></a>オーバーライド メソッドでのユーザー入力の処理

継承するオブジェクト`NSControl`(または`NSView`) があるいくつかのマウスの処理メソッドをオーバーライドまたはキーボード入力です。 ユーザーの例のコントロールの間の切り替えの状態を反転する**で**と**オフ**ユーザーがコントロール上でマウスの左ボタンがクリックするとします。 次にメソッドをオーバーライドする追加できる、`NSFliwSwitch`クラスでこれを処理します。

```csharp
#region Mouse Handling Methods
// --------------------------------------------------------------------------------
// Handle mouse with Override Methods.
// NOTE: Use either this method or Gesture Recognizers, NOT both!
// --------------------------------------------------------------------------------
public override void MouseDown (NSEvent theEvent)
{
    base.MouseDown (theEvent);

    FlipSwitchState ();
}

public override void MouseDragged (NSEvent theEvent)
{
    base.MouseDragged (theEvent);
}

public override void MouseUp (NSEvent theEvent)
{
    base.MouseUp (theEvent);
}

public override void MouseMoved (NSEvent theEvent)
{
    base.MouseMoved (theEvent);
}
## endregion
```

上記のコードで呼び出して、 `FlipSwitchState` (定義されているメソッドの上) On を反転/にあるスイッチの状態を Off に、`MouseDown`メソッドです。 これが強制的に実行を現在の状態を反映するように描画するコントロール。

<a name="Handling-User-Input-with-Gesture-Recognizers" />

#### <a name="handling-user-input-with-gesture-recognizers"></a>ジェスチャ レコグナイザーを持つユーザーの入力の処理

必要に応じて、コントロールと対話するユーザーを処理するのにジェスチャ レコグナイザーを使用することができます。 上に追加、上書きを削除、編集、`Initialize`メソッドされ次のようになります。

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;

    // --------------------------------------------------------------------------------
    // Handle mouse with Gesture Recognizers.
    // NOTE: Use either this method or the Override Methods, NOT both!
    // --------------------------------------------------------------------------------
    var click = new NSClickGestureRecognizer (() => {
        FlipSwitchState();
    });
    AddGestureRecognizer (click);
}
```

ここでは、作成する新しい`NSClickGestureRecognizer`を呼び出すと、`FlipSwitchState`ユーザーがクリックすると、マウスの左ボタンでは、スイッチの状態を変更するメソッド。 `AddGestureRecognizer (click)`メソッドでは、ジェスチャ レコグナイザーをコントロールに追加します。

もう一度、使用する方法は、カスタム コントロールを使用して達成を試みていますのでに依存します。 低レベルのアクセス許可が必要である場合、ユーザーとの対話にオーバーライド メソッドを使用します。 定義済みの機能は必要がある場合は、マウス クリックなどのジェスチャ レコグナイザーを使用します。

<a name="Responding-to-State-Change-Events" />

### <a name="responding-to-state-change-events"></a>状態変更イベントに応答します。

コードの状態の変化に応答する方法が必要、ユーザーには、カスタム コントロールの状態が変更された、ときに (何などときにカスタム ボタンをクリックして)。 

この機能を提供するには、編集、`NSFlipSwitch`クラスし、次のコードを追加します。

```csharp
#region Events
public event EventHandler ValueChanged;

internal void RaiseValueChanged() {
    if (this.ValueChanged != null)
        this.ValueChanged (this, EventArgs.Empty);

    // Perform any action bound to the control from Interface Builder
    // via an Action.
    if (this.Action !=null) 
        NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
}
## endregion
```

次に、編集、`FlipSwitchState`メソッドされ次のようになります。

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

最初に、マイクロソフトが提供する、`ValueChanged`イベントを追加できることにハンドラーを c# コードでユーザーには、スイッチの状態が変更されたときにアクションを実行おできるようにします。

カスタム コントロールがから継承するために、2 番目`NSControl`が自動的に、**アクション**Xcode のインターフェイスのビルダーに割り当てることです。 これを呼び出すと**アクション**次のコードを使用して、状態が変更されたとき。

```csharp
if (this.Action !=null) 
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

まず、かどうかを確認、**アクション**コントロールに割り当てられています。 次と呼んで、**アクション**定義されている場合。

<a name="Using-the-Custom-Control" />

## <a name="using-the-custom-control"></a>カスタム コントロールの使用

完全に定義されているカスタム コントロール、おことができます、c# コードを使用して、Xamarin.Mac アプリの UI に、または追加 Xcode のインターフェイスのビルダー。

インターフェイスのビルダーを使用してコントロールを追加するには、まず Xamarin.Mac プロジェクトのクリーン ビルドを実行し、ダブルクリック、`Main.storyboard`編集用のインターフェイスのビルダーで開くファイル。

[ ![](custom-controls-images/custom02.png "Xcode でストーリー ボードの編集")](custom-controls-images/custom02.png)

次に、ドラッグ、`Custom View`ユーザー インターフェイスの設計に。

[ ![](custom-controls-images/custom03.png "ライブラリから、カスタム ビューを選択します。")](custom-controls-images/custom03.png)

カスタム ビューを選択したままに切り替え、 **Identity インスペクター**ビューの変更と**クラス**に`NSFlipSwitch`:

[ ![](custom-controls-images/custom04.png "設定すると、ビューのクラス")](custom-controls-images/custom04.png)

切り替えて、**アシスタント エディター**を作成し、**コンセント**カスタム コントロールの (でバインドすることを確認、`ViewControler.h`ファイルおよび not、`.m`ファイル)。

[ ![](custom-controls-images/custom05.png "新しいコンセントを構成します。")](custom-controls-images/custom05.png)

Mac 用の Visual Studio に戻り、変更を保存し、変更を同期できるようにします。編集、`ViewController.cs`ファイルし、`ViewDidLoad`次のようなメソッドの検索。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Do any additional setup after loading the view.
    OptionTwo.ValueChanged += (sender, e) => {
        // Display the state of the option switch
        Console.WriteLine("Option Two: {0}", OptionTwo.Value);
    };
}
``` 

ここでは、回答をお送り、`ValueChanged`上で定義したイベント、`NSFlipSwitch`クラスし、現在の書き込み**値**ときに、ユーザーがコントロールをクリックします。

必要に応じて、インターフェイスのビルダーに戻りを定義することが、**アクション**コントロールに。

[ ![](custom-controls-images/custom06.png "新しいアクションを構成します。")](custom-controls-images/custom06.png)

再度、編集、`ViewController.cs`ファイルし、次のメソッドを追加します。

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> いずれかを使用する必要があります、**イベント**を定義するか、**アクション**インターフェイス ビルダーでは、同時に両方のメソッドを使用しないでくださいまたは相互に競合することができます。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションで再利用可能なカスタム ユーザー インターフェイス コントロールの作成についての詳細を取得しました。 Xcode のインターフェイスのビルダーでの操作に新しいコントロールを表示する方法と、マウス、ユーザー入力に応答するカスタム コントロール UI で、2 つの主な方法を描画する方法を説明しました。

## <a name="related-links"></a>関連リンク

- [MacCustomControl (サンプル)](https://developer.xamarin.com/samples/mac/MacCustomControl/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)
- [OS X のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [マウス イベントの処理](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
