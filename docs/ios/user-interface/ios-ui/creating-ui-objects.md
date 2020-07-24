---
title: Xamarin でのユーザーインターフェイスオブジェクトの作成
description: このドキュメントでは、Xamarin でユーザーインターフェイスを作成する方法の概要について説明します。 IOS Designer、Xcode Interface Builder、C#、およびストーリーボードについて説明します。
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 729289c1764746f9777ef3d720e77865c9a71389
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937255"
---
# <a name="creating-user-interface-objects-in-xamarinios"></a>Xamarin でのユーザーインターフェイスオブジェクトの作成

Apple は、関連する機能の一部を "フレームワーク" にグループ化します。これは、Xamarin の iOS 名前空間に相当します。 `UIKit`は、iOS のすべてのユーザーインターフェイスコントロールを含む名前空間です。

ラベルやボタンなどのユーザーインターフェイスコントロールをコードで参照する必要がある場合は必ず、次の using ステートメントを含めるようにしてください。

```csharp
using UIKit;
```

この章で説明されているすべてのコントロールは、UIKit 名前空間にあり、各ユーザーコントロールクラス名にはプレフィックスが付いてい `UI` ます。

UI コントロールとレイアウトは、次の3つの方法で編集できます。

- **[Xamarin IOS Designer](~/ios/user-interface/designer/index.md)** – xamarin の組み込みレイアウトデザイナーを使用して、画面をデザインします。 ストーリーボードまたは XIB ファイルをダブルクリックして、組み込みのデザイナーで編集します。
- **Xcode Interface Builder** – Interface Builder を使用してコントロールを画面レイアウトにドラッグします。 **Solution Pad**内のファイルを右クリックし、[ **> Xcode Interface Builder で開く**] を選択して、ストーリーボードまたは XIB ファイルを開きます。
- **C# を使用**する–コントロールは、コードを使用してプログラムで作成し、ビュー階層に追加することもできます。

新しいストーリーボードと XIB ファイルを追加するには、iOS プロジェクトを右クリックし、[**新しいファイルの追加 >.**..] を選択します。

どちらの方法を使用しても、アプリケーションロジックでは、コントロールのプロパティとイベントを C# で操作できます。

## <a name="using-xamarin-ios-designer"></a>Xamarin iOS Designer の使用

IOS Designer でユーザーインターフェイスの作成を開始するには、ストーリーボードファイルをダブルクリックします。 コントロールは、次に示すように、**ツールボックス**からデザインサーフェイスにドラッグできます。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

 [![ツールボックスパッド](creating-ui-objects-images/image2b.png)](creating-ui-objects-images/image2b.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

 [![ツールボックスパッド-Visual Studio](creating-ui-objects-images/image2b-vs.png)](creating-ui-objects-images/image2b.png#lightbox)

-----

デザインサーフェイスでコントロールを選択すると、そのコントロールの属性が**Properties Pad**に表示されます。 次のスクリーンショットに入力されている**ウィジェット > id > 名**フィールドは、*アウトレット*名として使用されます。 C# でコントロールを参照するには、次の手順を実行します。

 [![プロパティウィジェットパッド](creating-ui-objects-images/image3b.png)](creating-ui-objects-images/image3b.png#lightbox)

IOS designer の使用方法の詳細については、 [Ios designer の概要](~/ios/user-interface/designer/introduction.md)に関するガイドを参照してください。

## <a name="using-xcode-interface-builder"></a>Xcode Interface Builder の使用

Interface Builder の使用に慣れていない場合は、Apple の[Interface Builder](https://developer.apple.com/xcode/interface-builder/)ドキュメントを参照してください。

Xcode でストーリーボードを開くには、右クリックしてストーリーボードファイルのコンテキストメニューにアクセスし、[ **Xcode Interface Builder**で開く] を選択します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

 [![ストーリーボードコンテキストメニュー-Xcode](creating-ui-objects-images/imagexcode.png)](creating-ui-objects-images/imagexcode.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![ストーリーボードコンテキストメニュー-Xcode](creating-ui-objects-images/imagexcode-vs.png)](creating-ui-objects-images/imagexcode-vs.png#lightbox)

-----

コントロールは、次に示す**オブジェクトライブラリ**からデザインサーフェイスにドラッグできます。

 [![Xcode オブジェクトライブラリ](creating-ui-objects-images/image5a.png)](creating-ui-objects-images/image5a.png#lightbox)

Interface Builder を使用して UI をデザインするときは、C# で参照するコントロールごとに**アウトレット**を作成する必要があります。 これを行うには、[Xcode] ツールバーボタンの [center**エディター** ] ボタンを使用して、**アシスタントエディター**を有効にします。

 [![アシスタントエディターボタン](creating-ui-objects-images/image6a.png)](creating-ui-objects-images/image6a.png#lightbox)

ユーザーインターフェイスオブジェクトをクリックします。次に、.h ファイルに**ドラッグ**します。 **ドラッグを制御**するには、コントロールキーを押したまま、コンセント (またはアクション) を作成するユーザーインターフェイスオブジェクトをクリックしたままにします。 ヘッダーファイルにドラッグしたまま、Ctrl キーを押したままにしておきます。 定義の下にドラッグを完了 `@interface` します。 次のスクリーンショットに示すように、青い線が、キャプションの挿入コンセントまたはアウトレットコレクションと共に表示されます。

このボタンを離すと、アウトレットの名前を入力するように求められます。これは、コードで参照できる C# プロパティを作成するために使用されます。

 [![アウトレットの作成](creating-ui-objects-images/image8a.png)](creating-ui-objects-images/image8a.png#lightbox)

Xcode の Interface Builder と Visual Studio for Mac との統合方法の詳細については、 [Xib コード生成](~/ios/internals/xib-code-generation.md#generated)に関するドキュメントを参照してください。

## <a name="using-c"></a>C\# の使用

C# (ビューまたはビューコントローラーなど) を使用してプログラムによってユーザーインターフェイスオブジェクトを作成する場合は、次の手順を実行します。

- ユーザーインターフェイスオブジェクトのクラスレベルフィールドを宣言します。 たとえば、コントロール自体を1回作成し `ViewDidLoad` ます。 次に、ビューコントローラーのライフサイクルメソッド全体でオブジェクトを参照できます (
`ViewWillAppear`).
- `CGRect`コントロールのフレーム (画面上の X 座標と Y 座標、およびその幅と高さ) を定義するを作成します。 このためのディレクティブがあることを確認する必要があり `using CoreGraphics` ます。
- コントロールを作成して割り当てるには、コンストラクターを呼び出します。
- プロパティまたはイベントハンドラーを設定します。
- を呼び出して、 `Add()` ビュー階層にコントロールを追加します。

`UILabel`C# を使用してビューコントローラーでを作成する簡単な例を次に示します。

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

<a name="partial_classes"></a>

## <a name="using-c-and-storyboards"></a>C# とストーリーボードの使用

ビューコントローラーがデザインサーフェイスに追加されると、対応する2つの C# ファイルがプロジェクトに作成されます。 この例では、 `ControlsViewController.cs` と `ControlsViewController.designer.cs` が自動的に作成されています。

 [![ViewController 部分クラス](creating-ui-objects-images/image9b.png)](creating-ui-objects-images/image9b.png#lightbox)

この `ControlsViewController.cs` ファイルは、*コード*を対象としています。 ここでは、 `View` やなどのライフサイクルメソッドを `ViewDidLoad` `ViewWillAppear` 実装し、独自のプロパティ、フィールド、およびメソッドを追加できます。

`ControlsViewController.designer.cs`は、部分クラスを含む生成されたコードです。 Visual Studio for Mac のデザイン画面にコントロールの名前を指定した場合、または Xcode でアウトレットまたはアクションを作成した場合は、対応するプロパティまたは部分メソッドがデザイナー (designer.cs) ファイルに追加されます。 次のコードは、2つのボタンとテキストビューに対して生成されるコードの例を示しています。ボタンの1つにもイベントがあり `TouchUpInside` ます。

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

`designer.cs`ファイルを手動で編集することはできません。 IDE (Visual Studio for Mac または Visual Studio) は、ストーリーボードとの同期を維持する役割を担います。

ユーザーインターフェイスオブジェクトがプログラムによってまたはに追加される場合 `View` は、 `ViewController` オブジェクト参照を自分でインスタンス化して管理するため、デザイナーファイルは必要ありません。

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
