---
title: Xamarin. Mac でカスタムコントロールを作成する
description: このドキュメントでは、Xamarin. Mac でカスタムコントロールを作成する方法について説明します。 この例では、カスタムコントロールを作成する方法、その状態を追跡する方法、インターフェイスを描画する方法、ユーザー入力に応答する方法、およびアプリケーションでコントロールを使用する方法を示します。
ms.prod: xamarin
ms.assetid: 004534B1-5AEE-452C-BBBE-8C2673FD49B7
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: db4cab313b1c1f2fc1aaa969735f7f136feb7015
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91429690"
---
# <a name="creating-custom-controls-in-xamarinmac"></a>Xamarin. Mac でカスタムコントロールを作成する

Xamarin. Mac アプリケーションで C# と .NET を使用する場合、Xcode、 *Swift* *、および* *Xcode*で作業する開発者が同じユーザーコントロールにアクセスできます。 Xcode は直接統合されているため、Xcode の _Interface Builder_ を使用してユーザーコントロールを作成および管理できます (または、必要に応じて C# コードで直接作成することもできます)。

MacOS には豊富な組み込みのユーザーコントロールが用意されていますが、カスタムコントロールを作成して、すぐには提供されない機能を提供したり、カスタム UI テーマ (ゲームインターフェイスなど) と一致させたりすることが必要になる場合があります。

[![カスタム UI コントロールの例](custom-controls-images/intro01.png)](custom-controls-images/intro01.png#lightbox)

この記事では、Xamarin. Mac アプリケーションで再利用可能なカスタムユーザーインターフェイスコントロールを作成するための基本について説明します。 この記事で使用する主要な概念と手法について説明しているように、最初に [Hello, Mac](~/mac/get-started/hello-mac.md) の記事「 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 」と「 [アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions) 」セクションをご覧になることを強くお勧めします。

「 [C# のクラス/メソッドを](~/mac/internals/how-it-works.md) [Xamarin. Mac の内部](~/mac/internals/how-it-works.md) ドキュメントの前に公開する」セクションを参照して `Register` `Export` ください。 c# クラスを目的の c オブジェクトと UI 要素に接続するために使用されるコマンドとコマンドについても説明します。

<a name="Introduction-to-Outline-Views"></a>

## <a name="introduction-to-custom-controls"></a>カスタムコントロールの概要

前述のように、再利用可能なカスタムユーザーインターフェイスコントロールを作成して、Xamarin. Mac アプリの UI に固有の機能を提供したり、カスタム UI テーマ (ゲームインターフェイスなど) を作成したりすることが必要になる場合があります。

このような状況では、を簡単に継承 `NSControl` して、カスタムツールを作成できます。カスタムツールは、C# コードまたは Xcode の Interface Builder を使用してアプリの UI に追加できます。 カスタムコントロールから継承することにより `NSControl` 、組み込みのユーザーインターフェイスコントロールに用意されているすべての標準機能 (など) が自動的に設定され `NSButton` ます。

カスタムユーザーインターフェイスコントロールに情報が表示されるだけの場合 (カスタムのグラフ作成ツールやグラフィックツールなど)、の代わりにを継承することをお勧めし `NSView` `NSControl` ます。

どの基底クラスを使用する場合でも、カスタムコントロールを作成するための基本的な手順は同じです。

この記事では、独自のユーザーインターフェイスのテーマを提供するカスタムフリップスイッチコンポーネントと、完全に機能するカスタムユーザーインターフェイスコントロールを構築する例を作成します。

<a name="Building-the-Custom-Control"></a>

## <a name="building-the-custom-control"></a>カスタムコントロールのビルド

作成しているカスタムコントロールはユーザー入力 (マウスの左ボタンのクリック) に応答するため、を継承 `NSControl` します。 このようにして、カスタムコントロールには、組み込みのユーザーインターフェイスコントロールが標準の macOS コントロールのように応答し、対応する標準機能がすべて自動的に設定されます。

Visual Studio for Mac で、カスタムユーザーインターフェイスコントロールを作成する (または新規作成する) Xamarin プロジェクトを開きます。 新しいクラスを追加し、それを呼び出し `NSFlipSwitch` ます。

[![新しいクラスの追加](custom-controls-images/custom01.png)](custom-controls-images/custom01.png#lightbox)

次に、クラスを編集 `NSFlipSwitch.cs` し、次のように表示します。

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

では、から継承 `NSControl` し、 **Register** コマンドを使用して、このクラスを目的の C と Xcode の Interface Builder に公開しているので、カスタムクラスについて最初に説明します。

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

以降のセクションでは、上記のコードの残りの部分を詳しく見ていきます。

<a name="Tracking-the-Controls-State"></a>

### <a name="tracking-the-controls-state"></a>コントロールの状態の追跡

カスタムコントロールはスイッチであるため、スイッチのオン/オフの状態を追跡する方法が必要です。 では、次のコードを使用して処理を `NSFlipSwitch` 行います。

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

スイッチの状態が変化したときに、UI を更新する方法が必要です。 これを行うには、コントロールの UI を強制的にに再描画し `NeedsDisplay = true` ます。

コントロールで1つのオン/オフ状態 (たとえば、3つの位置を持つマルチ状態スイッチ) が必要である場合は、 **列挙型** を使用して状態を追跡できます。 この例では、単純な **ブール** 値が使用されます。

また、オンとオフの切り替えの状態をスワップするヘルパーメソッドも追加しました。

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

後で、このヘルパークラスを展開して、スイッチの状態が変化したときに呼び出し元に通知します。

<a name="Drawing-the-Controls-Interface"></a>

### <a name="drawing-the-controls-interface"></a>コントロールのインターフェイスの描画

ここでは、実行時にカスタムコントロールのユーザーインターフェイスを描画するために、コアグラフィックの描画ルーチンを使用します。 この操作を行う前に、コントロールのレイヤーをオンにする必要があります。 これを行うには、次のプライベートメソッドを使用します。

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

このメソッドは、コントロールが適切に構成されていることを確認するために、各コントロールのコンストラクターから呼び出されます。 次に例を示します。

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

次に、メソッドをオーバーライドし、コアグラフィックルーチンを追加してコントロールを描画する必要があり `DrawRect` ます。

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

コントロールの状態が変化したときに、コントロールの視覚的表現を調整します ( **オン** から **オフ**への移動など)。 状態が変化するたびに、コマンドを使用して、 `NeedsDisplay = true` コントロールを強制的に新しい状態に再描画できます。

<a name="Responding-to-User-Input"></a>

### <a name="responding-to-user-input"></a>ユーザー入力への応答

カスタムコントロールにユーザー入力を追加できる基本的な方法が2つあります。 **マウス処理ルーチン** や **ジェスチャレコグナイザー**をオーバーライドします。 使用する方法は、コントロールが必要とする機能に基づいています。

> [!IMPORTANT]
> 作成するカスタムコントロールについては、**オーバーライドメソッド**_と_**ジェスチャレコグナイザー**のどちらかを使用する必要がありますが、両方を同時に競合させることはできません。

<a name="Summary"></a>

#### <a name="handling-user-input-with-override-methods"></a>オーバーライドメソッドを使用したユーザー入力の処理

(または) を継承するオブジェクトには、 `NSControl` `NSView` マウスまたはキーボード入力を処理するためのオーバーライドメソッドがいくつかあります。 このコントロールの例では、ユーザーがマウスの左ボタンを使用してコントロールをクリックしたときの **オン** と **オフ** の切り替えの状態を切り替えます。 次のオーバーライドメソッドをクラスに追加して、 `NSFlipSwitch` これを処理できます。

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

上記のコードでは、メソッド `FlipSwitchState` (上で定義) を呼び出して、メソッド内のスイッチのオン/オフの状態を反転し `MouseDown` ます。 また、現在の状態を反映するために、コントロールが強制的に再描画されます。

<a name="Handling-User-Input-with-Gesture-Recognizers"></a>

#### <a name="handling-user-input-with-gesture-recognizers"></a>ジェスチャレコグナイザーを使用したユーザー入力の処理

必要に応じて、ジェスチャレコグナイザーを使用して、コントロールと対話するユーザーを処理できます。 上記で追加した上書きを削除し、メソッドを編集 `Initialize` して次のようにします。

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

ここでは、 `NSClickGestureRecognizer` `FlipSwitchState` ユーザーがマウスの左ボタンをクリックしたときにスイッチの状態を変更するために、新しいを作成し、メソッドを呼び出します。 メソッドは、 `AddGestureRecognizer (click)` ジェスチャ認識エンジンをコントロールに追加します。

ここでも、どの方法を使用するかは、カスタムコントロールで何を実行しようとしているかによって異なります。 ユーザー操作に対して低レベルのアクセス権が必要な場合は、オーバーライドメソッドを使用します。 マウスクリックなどの定義済みの機能が必要な場合は、ジェスチャレコグナイザーを使用します。

<a name="Responding-to-State-Change-Events"></a>

### <a name="responding-to-state-change-events"></a>状態変更イベントへの応答

ユーザーがカスタムコントロールの状態を変更するときは、コードの状態変更に応答する方法が必要です (カスタムボタンをクリックしたときに何かを行うなど)。

この機能を提供するには、クラスを編集 `NSFlipSwitch` し、次のコードを追加します。

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

次に、メソッドを編集 `FlipSwitchState` し、次のように表示します。

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

まず、 `ValueChanged` ユーザーがスイッチの状態を変更したときにアクションを実行できるように、C# コードでハンドラーをに追加できるイベントを提供します。

次に、カスタムコントロールはから継承するため、 `NSControl` Xcode の Interface Builder で割り当てることができる **アクション** が自動的に設定されます。 状態が変化したときにこの **アクション** を呼び出すには、次のコードを使用します。

```csharp
if (this.Action !=null)
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

まず、コントロールに **アクション** が割り当てられているかどうかを確認します。 次に、アクションが定義されている場合は、その **アクション** を呼び出します。

<a name="Using-the-Custom-Control"></a>

## <a name="using-the-custom-control"></a>カスタムコントロールの使用

カスタムコントロールが完全に定義されているので、C# コードまたは Xcode の Interface Builder を使用して、Xamarin. Mac アプリの UI に追加することができます。

Interface Builder を使用してコントロールを追加するには、まず Xamarin. Mac プロジェクトのクリーンビルドを実行し、次にファイルをダブルクリックして `Main.storyboard` 編集用の Interface Builder で開きます。

[![Xcode でストーリーボードを編集する](custom-controls-images/custom02.png)](custom-controls-images/custom02.png#lightbox)

次に、を `Custom View` ユーザーインターフェイスのデザインにドラッグします。

[![ライブラリからカスタムビューを選択する](custom-controls-images/custom03.png)](custom-controls-images/custom03.png#lightbox)

カスタムビューが選択された状態で、 **Id インスペクター** に切り替えて、ビューの **クラス** を次のように変更し `NSFlipSwitch` ます。

[![ビューのクラスの設定](custom-controls-images/custom04.png)](custom-controls-images/custom04.png#lightbox)

**アシスタントエディター**に切り替えて、カスタムコントロールの**アウトレット**を作成します (ファイルではなく、ファイルにバインドするようにしてください `ViewController.h` `.m` )。

[![新しいアウトレットの構成](custom-controls-images/custom05.png)](custom-controls-images/custom05.png#lightbox)

変更を保存し Visual Studio for Mac に戻り、変更を同期できるようにします。ファイルを編集 `ViewController.cs` し、 `ViewDidLoad` メソッドを次のようにします。

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

ここでは、 `ValueChanged` クラスで上で定義したイベントに応答 `NSFlipSwitch` し、ユーザーがコントロールをクリックしたときに現在の **値** を書き込みます。

必要に応じて Interface Builder に戻り、コントロールの **アクション** を定義することもできます。

[![新しいアクションの構成](custom-controls-images/custom06.png)](custom-controls-images/custom06.png#lightbox)

ここでも、 `ViewController.cs` ファイルを編集し、次のメソッドを追加します。

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> Interface Builder では、 **イベント** を使用するか、 **アクション** を定義する必要がありますが、両方の方法を同時に使用したり、互いに競合させたりすることはできません。

<a name="Summary"></a>

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリケーションで再利用可能なカスタムユーザーインターフェイスコントロールを作成する方法について詳しく説明しました。 ここでは、カスタムコントロール UI の描画方法、マウスとユーザー入力に応答する2つの主な方法、および Xcode の Interface Builder のアクションに新しいコントロールを公開する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [MacCustomControl (サンプル)](/samples/xamarin/mac-samples/maccustomcontrol)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [マウスイベントの処理](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)