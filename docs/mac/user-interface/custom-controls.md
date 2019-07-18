---
title: Xamarin.Mac でカスタム コントロールの作成
description: このドキュメントでは、Xamarin.Mac でカスタム コントロールを構築する方法について説明します。 カスタム コントロールをビルド、その状態を追跡、そのインターフェイスを描画、ユーザーの入力に応答およびアプリケーションでコントロールを使用する方法を示します。
ms.prod: xamarin
ms.assetid: 004534B1-5AEE-452C-BBBE-8C2673FD49B7
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 015c1e315b6070777542a8f8c5871c00cf336b5c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61236199"
---
# <a name="creating-custom-controls-in-xamarinmac"></a>Xamarin.Mac でカスタム コントロールの作成

使用する場合C#へのアクセス権を持って、Xamarin.Mac アプリケーションで .NET では、同じユーザー コントロールで作業する開発者*Objective C*、 *Swift*と*Xcode*は。 Xamarin.Mac は直接 Xcode と統合、ためには、Xcode を使用して_Interface Builder_を作成し、ユーザー コントロールを維持 (または必要に応じて c# コードで直接作成) します。

MacOS には豊富な組み込みのユーザー コントロール、機能がサポートされていませんボックスの提供する、または (ゲームのインターフェイス) などのカスタム UI テーマと一致するカスタム コントロールを作成する必要がある回である可能性があります。

[![](custom-controls-images/intro01.png "カスタムの UI コントロールの例")](custom-controls-images/intro01.png#lightbox)

この記事では、Xamarin.Mac アプリケーションで再利用可能なカスタム ユーザー インターフェイス コントロールの作成の基礎を取り上げます。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)と[Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions)ほどのセクションでは、主要な概念と、この記事で使用する方法について説明します。

見てしたい場合があります、 [c# を公開するクラス/メソッドを OBJECTIVE-C](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac の内部](~/mac/internals/how-it-works.md)、について説明します、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素を c# クラスをワイヤ アップするために使用します。

<a name="Introduction-to-Outline-Views" />

## <a name="introduction-to-custom-controls"></a>カスタム コントロールの概要

前述のように、カスタム ユーザー インターフェイス コントロール、Xamarin.Mac アプリの UI 固有の機能を提供する、またはゲームのインターフェイス) などのカスタム UI テーマを作成する、再利用を作成する必要がある生じるである可能性があります。

このような場合は、簡単にから継承できます`NSControl`と c# コードを使用して、または Xcode の Interface Builder でのアプリの UI に追加するか、カスタム ツールを作成します。 継承することによって`NSControl`、カスタム コントロールのすべての組み込みのユーザー インターフェイス コントロールを含む標準機能が自動的に (など`NSButton`)。

継承する場合は、カスタム ユーザー インターフェイス コントロールには、(カスタムのグラフ作成やグラフィック ツール) などの情報だけが表示されます、する可能性があります`NSView`の代わりに`NSControl`します。

基本クラスを使用するに関係なく、カスタム コントロールを作成するための基本的な手順は同じです。

この記事では、一意のユーザー インターフェイスのテーマと完全に機能のカスタム ユーザー インターフェイス コントロールを作成する例を提供するカスタムの反転スイッチ コンポーネントを作成します。

<a name="Building-the-Custom-Control" />

## <a name="building-the-custom-control"></a>カスタム コントロールのビルド

継承するようにここから、作成するカスタム コントロールはユーザー入力 (マウスの左ボタンのクリック) に応答して、`NSControl`します。 この方法で、カスタム コントロールが自動的にすべての組み込みのユーザー インターフェイス コントロールに標準的な機能があるし、macOS の標準コントロールのような応答は。

Visual Studio for Mac でのカスタム ユーザー インターフェイス コントロールを作成します (または新規作成) する Xamarin.Mac プロジェクトを開きます。 新しいクラスを追加し、それを呼び出す`NSFlipSwitch`:

[![](custom-controls-images/custom01.png "新しいクラスの追加")](custom-controls-images/custom01.png#lightbox)

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

最初に、カスタム クラスから継承していることに注目してください`NSControl`を使用して、**登録**OBJECTIVE-C、Xcode の Interface Builder をこのクラスを公開するコマンド。

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

次のセクションで詳細には、上記のコードの残りの部分を見てをみましょう。

<a name="Tracking-the-Controls-State" />

### <a name="tracking-the-controls-state"></a>コントロールの状態の追跡

ユーザーのカスタム コントロールは、スイッチであるため、スイッチのオン/オフ状態を追跡する手段が必要です。 次のコードで処理する`NSFlipSwitch`:

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

スイッチの状態が変更されたときに、UI を更新する方法が必要です。 その UI を再描画するコントロールを強制することで行っている`NeedsDisplay = true`します。

私たちを使用しても、コントロールでは、1 つのオン/オフの状態 (3 つの位置での複数の状態スイッチなど) よりも多く必要な場合、 **Enum**状態を追跡します。 この例では、単純な**bool**はしないでください。

また、オンとオフの切り替えの状態を交換するヘルパー メソッドを追加しました。

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

後で、スイッチの状態が変更されたとき、呼び出し元に通知するには、このヘルパー クラスを拡張します。

<a name="Drawing-the-Controls-Interface" />

### <a name="drawing-the-controls-interface"></a>コントロールのインターフェイスを描画

コア グラフィックスの描画ルーチンを使用して、実行時に、カスタム コントロールのユーザー インターフェイスを描画するでしょう。 そのため、前に、コントロールのレイヤーを有効にする必要があります。 私たちは、次のプライベート メソッド。

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

このメソッドは、各コントロールが正しく構成されていることを確認するコントロールのコンス トラクターから呼び出されます。 例えば:

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

次に、オーバーライドする必要があります、`DrawRect`メソッドとコントロールを描画するコア グラフィック ルーチンを追加します。

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

私たちを調整するコントロールの視覚的表現の状態が変更されたとき (などから**で**に**オフ**)。 状態の変更をいつを使用して、`NeedsDisplay = true`コマンドを強制的に新しい状態の再描画するコントロール。

<a name="Responding-to-User-Input" />

### <a name="responding-to-user-input"></a>ユーザー入力に対応

カスタム コントロールへのユーザー入力を追加できること、2 つの基本的な方法があります。**マウス処理ルーチンをオーバーライド**または**ジェスチャ レコグナイザー**します。 使用して、どのメソッドは、コントロールに必要な機能に基づいて構築します。

> [!IMPORTANT]
> カスタム コントロールを作成するため、いずれかを使用する必要があります**メソッドのオーバーライド**_または_**ジェスチャ レコグナイザー**、両方で同じではなくよう互いに競合することができます。

<a name="Summary" />

#### <a name="handling-user-input-with-override-methods"></a>オーバーライド メソッドでユーザー入力の処理

継承されるオブジェクト`NSControl`(または`NSView`) があるいくつかのマウスの処理メソッドをオーバーライド入力やキーボード入力します。 間の切り替えの状態を反転する、コントロールの例の**で**と**オフ**ユーザーがコントロール上でマウスの左ボタンがクリックするとします。 次にメソッドのオーバーライドを追加、`NSFlipSwitch`これを処理するクラス。

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

上記のコードで呼び出して、`FlipSwitchState`メソッドの反転のオン/オフのスイッチの状態を (前述)、`MouseDown`メソッド。 これが強制的に実行する現在の状態を反映するように再描画するコントロール。

<a name="Handling-User-Input-with-Gesture-Recognizers" />

#### <a name="handling-user-input-with-gesture-recognizers"></a>ジェスチャ レコグナイザーを持つユーザーの入力を処理します。

必要に応じて、コントロールと対話するユーザーを処理するために、ジェスチャ レコグナイザーを使用できます。 上で追加、上書きを削除、編集、`Initialize`メソッドと、次のようになります。

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

ここでは、作成する新しい`NSClickGestureRecognizer`を呼び出すと、`FlipSwitchState`にユーザーがマウスの左ボタンをクリックしたときに、スイッチの状態を変更するメソッド。 `AddGestureRecognizer (click)`メソッドでは、ジェスチャ レコグナイザーをコントロールに追加します。

ここでも、使用する方法は、カスタム コントロールで実行すに依存します。 私たちに低レベルのアクセスが必要がある場合、ユーザーの操作にオーバーライド メソッドを使用します。 定義済みの機能必要がある場合、マウス クリックなどジェスチャ レコグナイザーを使用します。

<a name="Responding-to-State-Change-Events" />

### <a name="responding-to-state-change-events"></a>状態変更イベントに応答して

コードの状態の変化に応答する方法が必要、ユーザーに、カスタム コントロールの状態が変更されたとき (など、行っているときにカスタム ボタンをクリックして)。 

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

次に、編集、`FlipSwitchState`メソッドと、次のようになります。

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

まず、入力、`ValueChanged`イベントを追加できることをハンドラーの c# コードでスイッチの状態が変更されると、アクション実行できるようにします。

カスタム コントロールがから継承するために、2 番目`NSControl`が自動的に、**アクション**Xcode の Interface Builder で割り当てられていることができます。 これを呼び出す**アクション**次のコードを使用して、状態が変更されたとき。

```csharp
if (this.Action !=null) 
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

まず、かどうかを確認、**アクション**コントロールに割り当てられています。 次に、呼び出して、**アクション**が定義されている場合。

<a name="Using-the-Custom-Control" />

## <a name="using-the-custom-control"></a>カスタム コントロールを使用します。

完全に定義されているカスタム コントロール、ことができますか、追加または Xcode の Interface Builder で c# コードを使用して、Xamarin.Mac アプリの UI にします。

Interface Builder を使用して、コントロールを追加するには、まず、Xamarin.Mac プロジェクトのクリーン ビルドを実行し、ダブルクリック、`Main.storyboard`編集用のインターフェイス ビルダーで開くファイル。

[![](custom-controls-images/custom02.png "Xcode でストーリー ボードの編集")](custom-controls-images/custom02.png#lightbox)

次に、ドラッグ、`Custom View`ユーザー インターフェイスの設計に。

[![](custom-controls-images/custom03.png "ライブラリからのカスタム ビューの選択")](custom-controls-images/custom03.png#lightbox)

選択したまま、カスタム ビューに切り替え、 **Identity Inspector**ビューの変更と**クラス**に`NSFlipSwitch`:

[![](custom-controls-images/custom04.png "設定すると、ビューのクラス")](custom-controls-images/custom04.png#lightbox)

切り替えて、**アシスタント エディター**を作成し、**アウトレット**カスタム コントロールの (でバインドすることを確認、`ViewController.h`ファイルおよび not、`.m`ファイル)。

[![](custom-controls-images/custom05.png "新しいアウトレットを構成します。")](custom-controls-images/custom05.png#lightbox)

Visual Studio for Mac に戻り、変更を保存し、同期の変更を許可します。編集、`ViewController.cs`ファイル、`ViewDidLoad`メソッドの次のようになります。

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

ここでは、対応する、`ValueChanged`の上で定義したイベント、`NSFlipSwitch`クラスし、現在の書き込み**値**コントロールに、ユーザーがクリックしたとき。

必要に応じて、Interface Builder に戻り、定義が、**アクション**コントロール。

[![](custom-controls-images/custom06.png "新しいアクションの構成")](custom-controls-images/custom06.png#lightbox)

もう一度、編集、`ViewController.cs`ファイルを開き、次のメソッドを追加します。

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> いずれかを使用する必要があります、**イベント**定義や、**アクション**インターフェイス ビルダーでは同時に両方のメソッドを使用する必要がありますまたは互いに競合することができます。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションで再利用可能なカスタム ユーザー インターフェイス コントロールの作成について詳しく説明をしました。 マウスとユーザー入力に応答するカスタム コントロール UI を 2 つの主な方法を描画する方法と Xcode の Interface Builder での操作に新しいコントロールを公開する方法を説明しました。

## <a name="related-links"></a>関連リンク

- [MacCustomControl (サンプル)](https://developer.xamarin.com/samples/mac/MacCustomControl/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [マウス イベントの処理](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
