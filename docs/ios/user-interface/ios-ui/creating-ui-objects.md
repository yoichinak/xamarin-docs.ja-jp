---
title: Xamarin でのユーザーインターフェイスオブジェクトの作成
description: このドキュメントでは、Xamarin でユーザーインターフェイスを作成する方法の概要について説明します。 IOS Designer、Xcode Interface Builder、 C#、およびストーリーボードについて説明します。
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: c1e7d6cbb2598f64a331257c9b14ecfa119193f6
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768795"
---
# <a name="creating-user-interface-objects-in-xamarinios"></a>Xamarin でのユーザーインターフェイスオブジェクトの作成

Apple は、関連する機能の一部を "フレームワーク" にグループ化します。これは、Xamarin の iOS 名前空間に相当します。 `UIKit`は、iOS のすべてのユーザーインターフェイスコントロールを含む名前空間です。

ラベルやボタンなどのユーザーインターフェイスコントロールをコードで参照する必要がある場合は必ず、次の using ステートメントを含めるようにしてください。

```csharp
using UIKit;
```

この章で説明されているすべてのコントロールは、uikit 名前空間にあり、各ユーザー `UI`コントロールクラス名にはプレフィックスが付いています。

UI コントロールとレイアウトは、次の3つの方法で編集できます。

- **[Xamarin IOS Designer](~/ios/user-interface/designer/index.md)** – xamarin の組み込みレイアウトデザイナーを使用して、画面をデザインします。 ストーリーボードまたは XIB ファイルをダブルクリックして、組み込みのデザイナーで編集します。
- **Xcode Interface Builder** – Interface Builder を使用してコントロールを画面レイアウトにドラッグします。 **Solution Pad**内のファイルを右クリックし、 **[> Xcode Interface Builder で開く]** を選択して、ストーリーボードまたは XIB ファイルを開きます。
- **をC#使用**すると、コードを使用してプログラムで作成し、ビュー階層に追加することもできます。

新しいストーリーボードと XIB ファイルを追加するには、iOS プロジェクトを右クリックし、[**新しいファイルの追加 >.** ..] を選択します。

どちらの方法を使用しても、アプリケーションロジックC#でコントロールのプロパティとイベントを操作できます。

## <a name="using-xamarin-ios-designer"></a>Xamarin iOS Designer の使用

IOS Designer でユーザーインターフェイスの作成を開始するには、ストーリーボードファイルをダブルクリックします。 コントロールは、次に示すように、**ツールボックス**からデザインサーフェイスにドラッグできます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

 [![](creating-ui-objects-images/image2b.png "ツールボックスパッド")](creating-ui-objects-images/image2b.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 [![](creating-ui-objects-images/image2b-vs.png "ツールボックスパッド-Visual Studio")](creating-ui-objects-images/image2b.png#lightbox)

-----

デザインサーフェイスでコントロールを選択すると、そのコントロールの属性が**Properties Pad**に表示されます。 次のスクリーンショットに入力されている**ウィジェット > id > 名**フィールドは、*アウトレット*名として使用されます。 でコントロールを参照するには、次C#の手順を実行します。

 [![](creating-ui-objects-images/image3b.png "プロパティウィジェットパッド")](creating-ui-objects-images/image3b.png#lightbox)

IOS designer の使用方法の詳細については、 [Ios designer の概要](~/ios/user-interface/designer/introduction.md)に関するガイドを参照してください。

## <a name="using-xcode-interface-builder"></a>Xcode Interface Builder の使用

Interface Builder の使用に慣れていない場合は、Apple の[Interface Builder](https://developer.apple.com/xcode/interface-builder/)ドキュメントを参照してください。

Xcode でストーリーボードを開くには、右クリックしてストーリーボードファイルのコンテキストメニューにアクセスし、[ **Xcode Interface Builder**で開く] を選択します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

 [![](creating-ui-objects-images/imagexcode.png "ストーリーボードコンテキストメニュー-Xcode")](creating-ui-objects-images/imagexcode.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](creating-ui-objects-images/imagexcode-vs.png "ストーリーボードコンテキストメニュー-Xcode")](creating-ui-objects-images/imagexcode-vs.png#lightbox)

-----

コントロールは、次に示す**オブジェクトライブラリ**からデザインサーフェイスにドラッグできます。

 [![](creating-ui-objects-images/image5a.png "Xcode オブジェクトライブラリ")](creating-ui-objects-images/image5a.png#lightbox)

Interface Builder を使用して UI をデザインする場合は、参照するコントロールごとに**アウトレット**を作成するC#必要があります。 これを行うには、[Xcode] ツールバーボタンの [center**エディター** ] ボタンを使用して、**アシスタントエディター**を有効にします。

 [![](creating-ui-objects-images/image6a.png "アシスタントエディターボタン")](creating-ui-objects-images/image6a.png#lightbox)

ユーザーインターフェイスオブジェクトをクリックします。次に、.h ファイルに**ドラッグ**します。 **ドラッグを制御**するには、コントロールキーを押したまま、コンセント (またはアクション) を作成するユーザーインターフェイスオブジェクトをクリックしたままにします。 ヘッダーファイルにドラッグしたまま、Ctrl キーを押したままにしておきます。 定義の下に`@interface`ドラッグを完了します。 次のスクリーンショットに示すように、青い線が、キャプションの挿入コンセントまたはアウトレットコレクションと共に表示されます。

このボタンを離すと、アウトレットの名前を入力するように求められます。これは、コードでC#参照できるプロパティを作成するために使用されます。

 [![](creating-ui-objects-images/image8a.png "アウトレットの作成")](creating-ui-objects-images/image8a.png#lightbox)

Xcode の Interface Builder と Visual Studio for Mac との統合方法の詳細については、 [Xib コード生成](~/ios/internals/xib-code-generation.md#generated)に関するドキュメントを参照してください。

## <a name="using-c"></a>使用 (C を)\#

(ビューコントローラーまたはビューコントローラーなどで) をC#使用してプログラムでユーザーインターフェイスオブジェクトを作成する場合は、次の手順を実行します。

- ユーザーインターフェイスオブジェクトのクラスレベルフィールドを宣言します。 たとえば、 `ViewDidLoad`コントロール自体を1回作成します。 次に、ビューコントローラーのライフサイクルメソッド全体でオブジェクトを参照できます (
`ViewWillAppear`)
- コントロールの`CGRect`フレーム (画面上の X 座標と Y 座標、およびその幅と高さ) を定義するを作成します。 このための`using CoreGraphics`ディレクティブがあることを確認する必要があります。
- コントロールを作成して割り当てるには、コンストラクターを呼び出します。
- プロパティまたはイベントハンドラーを設定します。
- を`Add()`呼び出して、ビュー階層にコントロールを追加します。

次に、を使用して`UILabel` C#ビューコントローラーでを作成する簡単な例を示します。

```csharp
UILabel label1;
public override void ViewDidLoad () {
    base.ViewDidLoad ();
    var frame = new CGRect(10, 10, 300, 30);
    label1 = new UILabel(frame);
    label1.Text = "New Label";
    View.Add (label1);
}
```

<a name="partial_classes" />

## <a name="using-c-and-storyboards"></a>とC#ストーリーボードの使用

ビューコントローラーがデザインサーフェイスに追加されると、対応C#する2つのファイルがプロジェクトに作成されます。 この例では`ControlsViewController.cs` 、 `ControlsViewController.designer.cs`とが自動的に作成されています。

 [![](creating-ui-objects-images/image9b.png "ViewController 部分クラス")](creating-ui-objects-images/image9b.png#lightbox)

このファイルは、コードを対象としています。 `MainViewController.cs` ここでは、 `View`や`ViewWillAppear`など`ViewDidLoad`のライフサイクルメソッドを実装し、独自のプロパティ、フィールド、およびメソッドを追加できます。

は`ControlsViewController.designer.cs` 、部分クラスを含む生成されたコードです。 Visual Studio for Mac のデザイン画面にコントロールの名前を指定した場合、または Xcode でアウトレットまたはアクションを作成した場合は、対応するプロパティまたは部分メソッドがデザイナー (designer.cs) ファイルに追加されます。 次のコードは、2つのボタンとテキストビューに対して生成されるコードの例を示してい`TouchUpInside`ます。ボタンの1つにもイベントがあります。

部分クラスのこれらの要素を使用すると、コードでコントロールを参照し、デザインサーフェイスで宣言されているアクションに応答できます。

```csharp
[Register ("ControlsViewController")]
    partial class ControlsViewController
    {
        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button1 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button2 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UITextField textfield1 { get; set; }

        [Action ("button2_TouchUpInside:")]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        partial void button2_TouchUpInside (UIButton sender);

        void ReleaseDesignerOutlets ()
        {
            if (Button1 != null) {
                Button1.Dispose ();
                Button1 = null;
            }
            if (Button2 != null) {
                Button2.Dispose ();
                Button2 = null;
            }
            if (textfield1 != null) {
                textfield1.Dispose ();
                textfield1 = null;
            }
        }
    }
}
```

ファイル`designer.cs`を手動で編集することはできません。 IDE (Visual Studio for Mac または Visual Studio) は、ストーリーボードとの同期を維持する役割を担います。

ユーザーインターフェイスオブジェクトがプログラムによって`View`またはに追加される場合は`ViewController`、オブジェクト参照を自分でインスタンス化して管理するため、デザイナーファイルは必要ありません。

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
