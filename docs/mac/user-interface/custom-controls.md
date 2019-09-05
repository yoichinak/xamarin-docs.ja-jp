---
title: Xamarin. Mac でカスタムコントロールを作成する
description: このドキュメントでは、Xamarin. Mac でカスタムコントロールを作成する方法について説明します。 この例では、カスタムコントロールを作成する方法、その状態を追跡する方法、インターフェイスを描画する方法、ユーザー入力に応答する方法、およびアプリケーションでコントロールを使用する方法を示します。
ms.prod: xamarin
ms.assetid: 004534B1-5AEE-452C-BBBE-8C2673FD49B7
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 24e4113f0437c626ba93f12c1124407c472fef8d
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70284951"
---
# <a name="creating-custom-controls-in-xamarinmac"></a>Xamarin. Mac でカスタムコントロールを作成する

Xamarin. Mac C#アプリケーションでと .net を使用する場合、Xcode、 *Swift* *、および*で作業する開発者が同じユーザーコントロールにアクセスできます。 Xcode は直接統合されているため、Xcode の_Interface Builder_を使用してユーザーコントロールを作成および管理できます (また、必要C#に応じて、コード内で直接作成することもできます)。

MacOS には豊富な組み込みのユーザーコントロールが用意されていますが、カスタムコントロールを作成して、すぐには提供されない機能を提供したり、カスタム UI テーマ (ゲームインターフェイスなど) と一致させたりすることが必要になる場合があります。

[![](custom-controls-images/intro01.png "カスタム UI コントロールの例")](custom-controls-images/intro01.png#lightbox)

この記事では、Xamarin. Mac アプリケーションで再利用可能なカスタムユーザーインターフェイスコントロールを作成するための基本について説明します。 最初に、 [Hello, Mac](~/mac/get-started/hello-mac.md)の記事を使用して作業することを強くお勧めします。具体的には、 [Xcode と Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)および[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)に関するセクションで説明します。これは、で使用する主要な概念と手法に関するものです。この記事をご覧ください。

[Xamarin. Mac の内部](~/mac/internals/how-it-works.md)ドキュメントの「[クラス/ C#メソッドを目的に公開](~/mac/internals/how-it-works.md)する」セクションを参照して、 C#クラスをに接続するために使用`Register`さ`Export`れるコマンドとコマンドについて説明します。目的 C オブジェクトと UI 要素。

<a name="Introduction-to-Outline-Views" />

## <a name="introduction-to-custom-controls"></a>カスタムコントロールの概要

前述のように、再利用可能なカスタムユーザーインターフェイスコントロールを作成して、Xamarin. Mac アプリの UI に固有の機能を提供したり、カスタム UI テーマ (ゲームインターフェイスなど) を作成したりすることが必要になる場合があります。

このような状況では、を`NSControl`簡単に継承して、カスタムツールを作成できます。カスタムツールは、 C#コードまたは Xcode の Interface Builder を使用してアプリの UI に追加できます。 カスタムコントロールから`NSControl`継承することにより、組み込みのユーザーインターフェイスコントロールに用意されているすべての標準機能 ( `NSButton`など) が自動的に設定されます。

カスタムユーザーインターフェイスコントロールに情報が表示されるだけの場合 (カスタムのグラフ作成ツールやグラフィックツールなど)、 `NSView`の`NSControl`代わりにを継承することをお勧めします。

どの基底クラスを使用する場合でも、カスタムコントロールを作成するための基本的な手順は同じです。

この記事では、独自のユーザーインターフェイスのテーマを提供するカスタムフリップスイッチコンポーネントと、完全に機能するカスタムユーザーインターフェイスコントロールを構築する例を作成します。

<a name="Building-the-Custom-Control" />

## <a name="building-the-custom-control"></a>カスタムコントロールのビルド

作成しているカスタムコントロールはユーザー入力 (マウスの`NSControl`左ボタンのクリック) に応答するため、を継承します。 このようにして、カスタムコントロールには、組み込みのユーザーインターフェイスコントロールが標準の macOS コントロールのように応答し、対応する標準機能がすべて自動的に設定されます。

Visual Studio for Mac で、カスタムユーザーインターフェイスコントロールを作成する (または新規作成する) Xamarin プロジェクトを開きます。 新しいクラスを追加し、それ`NSFlipSwitch`を呼び出します。

[![](custom-controls-images/custom01.png "新しいクラスの追加")](custom-controls-images/custom01.png#lightbox)

次に、 `NSFlipSwitch.cs`クラスを編集し、次のように表示します。

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

では、から`NSControl`継承し、 **Register**コマンドを使用して、このクラスを目的の C と Xcode の Interface Builder に公開しているので、カスタムクラスについて最初に説明します。

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

以降のセクションでは、上記のコードの残りの部分を詳しく見ていきます。

<a name="Tracking-the-Controls-State" />

### <a name="tracking-the-controls-state"></a>コントロールの状態の追跡

カスタムコントロールはスイッチであるため、スイッチのオン/オフの状態を追跡する方法が必要です。 で`NSFlipSwitch`は、次のコードを使用して処理を行います。

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

スイッチの状態が変化したときに、UI を更新する方法が必要です。 これを行うには、コントロールの UI を強制的に`NeedsDisplay = true`に再描画します。

コントロールで1つのオン/オフ状態 (たとえば、3つの位置を持つマルチ状態スイッチ) が必要である場合は、**列挙型**を使用して状態を追跡できます。 この例では、単純な**ブール**値が使用されます。

また、オンとオフの切り替えの状態をスワップするヘルパーメソッドも追加しました。

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

後で、このヘルパークラスを展開して、スイッチの状態が変化したときに呼び出し元に通知します。

<a name="Drawing-the-Controls-Interface" />

### <a name="drawing-the-controls-interface"></a>コントロールのインターフェイスの描画

ここでは、実行時にカスタムコントロールのユーザーインターフェイスを描画するために、コアグラフィックの描画ルーチンを使用します。 この操作を行う前に、コントロールのレイヤーをオンにする必要があります。 これを行うには、次のプライベートメソッドを使用します。

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

このメソッドは、コントロールが適切に構成されていることを確認するために、各コントロールのコンストラクターから呼び出されます。 例えば:

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

次に、 `DrawRect`メソッドをオーバーライドし、コアグラフィックルーチンを追加してコントロールを描画する必要があります。

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

コントロールの状態が変化したときに、コントロールの視覚的表現を調整します (**オン**から**オフ**への移動など)。 状態が変化するたびに、 `NeedsDisplay = true`コマンドを使用して、コントロールを強制的に新しい状態に再描画できます。

<a name="Responding-to-User-Input" />

### <a name="responding-to-user-input"></a>ユーザー入力への応答

カスタムコントロールにユーザー入力を追加するには、次の2つの基本的な方法があります。**マウス処理ルーチン**または**ジェスチャレコグナイザー**をオーバーライドします。 使用する方法は、コントロールが必要とする機能に基づいています。

> [!IMPORTANT]
> 作成するカスタムコントロールについては、**オーバーライドメソッド**_と_**ジェスチャレコグナイザー**のどちらかを使用する必要がありますが、両方を同時に競合させることはできません。

<a name="Summary" />

#### <a name="handling-user-input-with-override-methods"></a>オーバーライドメソッドを使用したユーザー入力の処理

(または`NSView`) `NSControl`を継承するオブジェクトには、マウスまたはキーボード入力を処理するためのオーバーライドメソッドがいくつかあります。 このコントロールの例では、ユーザーがマウスの左ボタンを使用してコントロールをクリックしたときの**オン**と**オフ**の切り替えの状態を切り替えます。 次のオーバーライドメソッドを`NSFlipSwitch`クラスに追加して、これを処理できます。

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

上記のコードでは、メソッド ( `FlipSwitchState`上`MouseDown`で定義) を呼び出して、メソッド内のスイッチのオン/オフの状態を反転します。 また、現在の状態を反映するために、コントロールが強制的に再描画されます。

<a name="Handling-User-Input-with-Gesture-Recognizers" />

#### <a name="handling-user-input-with-gesture-recognizers"></a>ジェスチャレコグナイザーを使用したユーザー入力の処理

必要に応じて、ジェスチャレコグナイザーを使用して、コントロールと対話するユーザーを処理できます。 上記で追加した上書きを削除`Initialize`し、メソッドを編集して次のようにします。

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

ここでは、ユーザーがマウス`NSClickGestureRecognizer`の左ボタン`FlipSwitchState`をクリックしたときにスイッチの状態を変更するために、新しいを作成し、メソッドを呼び出します。 メソッド`AddGestureRecognizer (click)`は、ジェスチャ認識エンジンをコントロールに追加します。

ここでも、どの方法を使用するかは、カスタムコントロールで何を実行しようとしているかによって異なります。 ユーザー操作に対して低レベルのアクセス権が必要な場合は、オーバーライドメソッドを使用します。 マウスクリックなどの定義済みの機能が必要な場合は、ジェスチャレコグナイザーを使用します。

<a name="Responding-to-State-Change-Events" />

### <a name="responding-to-state-change-events"></a>状態変更イベントへの応答

ユーザーがカスタムコントロールの状態を変更するときは、コードの状態変更に応答する方法が必要です (カスタムボタンをクリックしたときに何かを行うなど)。

この機能を提供するには`NSFlipSwitch` 、クラスを編集し、次のコードを追加します。

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

次に、 `FlipSwitchState`メソッドを編集し、次のように表示します。

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

まず、ユーザーがスイッチ`ValueChanged`の状態を変更したときにアクションC#を実行できるように、コード内にハンドラーを追加できるイベントを提供します。

次に、カスタムコントロールはから`NSControl`継承するため、Xcode の Interface Builder で割り当てることができる**アクション**が自動的に設定されます。 状態が変化したときにこの**アクション**を呼び出すには、次のコードを使用します。

```csharp
if (this.Action !=null)
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

まず、コントロールに**アクション**が割り当てられているかどうかを確認します。 次に、アクションが定義されている場合は、その**アクション**を呼び出します。

<a name="Using-the-Custom-Control" />

## <a name="using-the-custom-control"></a>カスタムコントロールの使用

カスタムコントロールが完全に定義されたので、コードまたは Xcode の Interface Builder を使用しC#て、Xamarin. Mac アプリの UI に追加することができます。

Interface Builder を使用してコントロールを追加するには、まず Xamarin. Mac プロジェクトのクリーンビルドを実行し、 `Main.storyboard`次にファイルをダブルクリックして編集用の Interface Builder で開きます。

[![](custom-controls-images/custom02.png "Xcode でストーリーボードを編集する")](custom-controls-images/custom02.png#lightbox)

次に、を`Custom View`ユーザーインターフェイスのデザインにドラッグします。

[![](custom-controls-images/custom03.png "ライブラリからカスタムビューを選択する")](custom-controls-images/custom03.png#lightbox)

カスタムビューが選択された状態で、 **Id インスペクター**に切り替えて、ビューのクラス`NSFlipSwitch`を次のように変更します。

[![](custom-controls-images/custom04.png "ビューのクラスの設定")](custom-controls-images/custom04.png#lightbox)

**アシスタントエディター**に切り替えて、カスタムコントロールの**アウトレット**を作成します (ファイルではなく`ViewController.h` `.m` 、ファイルにバインドするようにしてください)。

[![](custom-controls-images/custom05.png "新しいアウトレットの構成")](custom-controls-images/custom05.png#lightbox)

変更を保存し Visual Studio for Mac に戻り、変更を同期できるようにします。ファイルを編集し、メソッド`ViewDidLoad`を次のようにします。 `ViewController.cs`

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

ここでは、 `ValueChanged` `NSFlipSwitch`クラスで上で定義したイベントに応答し、ユーザーがコントロールをクリックしたときに現在の**値**を書き込みます。

必要に応じて Interface Builder に戻り、コントロールの**アクション**を定義することもできます。

[![](custom-controls-images/custom06.png "新しいアクションの構成")](custom-controls-images/custom06.png#lightbox)

ここでも、 `ViewController.cs`ファイルを編集し、次のメソッドを追加します。

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> Interface Builder では、**イベント**を使用するか、**アクション**を定義する必要がありますが、両方の方法を同時に使用したり、互いに競合させたりすることはできません。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリケーションで再利用可能なカスタムユーザーインターフェイスコントロールを作成する方法について詳しく説明しました。 ここでは、カスタムコントロール UI の描画方法、マウスとユーザー入力に応答する2つの主な方法、および Xcode の Interface Builder のアクションに新しいコントロールを公開する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [MacCustomControl (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/maccustomcontrol)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [マウスイベントの処理](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
