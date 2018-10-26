---
title: Xamarin.iOS でのユーザー インターフェイス オブジェクトの作成
description: このドキュメントでは、Xamarin.iOS でのユーザー インターフェイスを作成する方法の概要を示します。 IOS Designer、Xcode の Interface Builder をについて説明してC#とストーリー ボード。
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: a4cf63b84d0686bc28b02b18a6266908251bdf6f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121920"
---
# <a name="creating-user-interface-objects-in-xamarinios"></a>Xamarin.iOS でのユーザー インターフェイス オブジェクトの作成

Apple のグループに関連の Xamarin.iOS の名前空間と同等の「フレームワーク」に機能します。 `UIKit` iOS 用のすべてのユーザー インターフェイス コントロールを含む名前空間です。

コードは、ラベルやボタンなどのユーザー インターフェイス コントロールを参照する必要があるたびに、以下に注意してください。 ステートメントを使用します。

```csharp
using UIKit;
```

UIKit 名前空間にこの章で説明されているすべてのコントロールがあり、各ユーザー コントロールのクラス名が、`UI`プレフィックス。

UI コントロールと 3 つの方法でレイアウトを編集することができます。

-  **[Xamarin iOS Designer](~/ios/user-interface/designer/index.md)**  – 使用して Xamarin の組み込みのレイアウトのデザイナー画面をデザインします。 ストーリー ボードまたは組み込みのデザイナーで編集する XIB ファイルをダブルクリックします。
-  **Xcode の Interface Builder** – インターフェイス ビルダーでは、画面のレイアウトにコントロールをドラッグします。 ファイルを右クリックして、Xcode でストーリー ボードまたは XIB ファイルを開く、 **Solution Pad**を選択して**プログラムから開く > Xcode の Interface Builder**します。
-  **使用してC#**  – コントロールもプログラムでコードを使用して構築でき、ビュー階層に追加します。

IOS プロジェクトを右クリックして新しいストーリー ボードと XIB ファイルを追加できる**追加 > 新しいファイル.**.

どちらの方法を使用すると、コントロールのプロパティおよびイベント操作は可能でC#アプリケーション ロジックにします。

## <a name="using-xamarin-ios-designer"></a>Xamarin iOS デザイナーを使用してください。

IOS Designer のユーザー インターフェイスの作成を開始するには、ストーリー ボード ファイルをダブルクリックします。 デザイン サーフェイスにコントロールをドラッグすることができます、**ツールボックス**下図のようにします。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

 [![](creating-ui-objects-images/image2b.png "ツールボックス パッド")](creating-ui-objects-images/image2b.png#lightbox)
 
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 [![](creating-ui-objects-images/image2b-vs.png "ツールボックス パッド - Visual Stuio")](creating-ui-objects-images/image2b.png#lightbox)
 
-----

デザイン サーフェイスにコントロールを選択すると、 **Properties Pad**そのコントロールの属性が表示されます。 **ウィジェット > Identity > 名前**としてフィールドには、次のスクリーン ショットで設定されますが、提供される、*アウトレット*名。 これで、コントロールを参照する方法は、 C#:

 [![](creating-ui-objects-images/image3b.png "プロパティのウィジェット パッド")](creating-ui-objects-images/image3b.png#lightbox)

IOS designer の使用について詳しくを参照してください、 [iOS Designer の概要](~/ios/user-interface/designer/introduction.md)ガイド。

## <a name="using-xcode-interface-builder"></a>Xcode の Interface Builder を使用します。

Interface Builder の使用に慣れていない場合は、Apple を参照してください[Interface Builder](https://developer.apple.com/xcode/interface-builder/)ドキュメント。

Xcode で開くストーリー ボードには、右クリックしてストーリー ボード ファイルのコンテキスト メニューにアクセスしてでを開く、 **Xcode の Interface Builder**:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

 [![](creating-ui-objects-images/imagexcode.png "Xcode のストーリー ボード コンテキスト メニュー")](creating-ui-objects-images/imagexcode.png#lightbox)
 
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](creating-ui-objects-images/imagexcode-vs.png "Xcode のストーリー ボード コンテキスト メニュー")](creating-ui-objects-images/imagexcode-vs.png#lightbox)

-----

デザイン サーフェイスにコントロールをドラッグすることができます、**オブジェクト ライブラリ**を次に示します。

 [![](creating-ui-objects-images/image5a.png "Xcode オブジェクト ライブラリ")](creating-ui-objects-images/image5a.png#lightbox)

作成する必要があります、Interface Builder で UI をデザインするとき、**アウトレット**で参照する各コントロールのC#します。 これは、オンで、**アシスタント エディター**センターを使用して**エディター** Xcode ツール バー ボタンのボタン。

 [![](creating-ui-objects-images/image6a.png "アシスタント エディター ボタン")](creating-ui-objects-images/image6a.png#lightbox)

ユーザー インターフェイス オブジェクトでは; をクリックします。**コントロールのドラッグ**.h ファイルにします。 * * コントロール ドラッグ * コントロール キーを押しながら をクリックし、ユーザー インターフェイス オブジェクト上でのコンセント (またはアクション) を作成します。 保持コントロール キーを押しながらヘッダー ファイルにドラッグします。 [完了] の下にドラッグ、`@interface`定義します。 青い線は、次のスクリーン ショットに示すように、キャプションの挿入のコンセントまたはコンセントのコレクションで表示されます。

作成するために使用すると、出口の名前を指定するメッセージが表示されます をクリックをリリースするときに、C#プロパティをコードで参照できます。

 [![](creating-ui-objects-images/image8a.png "アウトレットを作成します。")](creating-ui-objects-images/image8a.png#lightbox)

Visual Studio と Mac の Xcode の Interface Builder の統合の詳細についてを参照してください、 [Xib コードの生成](~/ios/internals/xib-code-generation.md#generated)ドキュメント。

##  <a name="using-c"></a>使用してください。C#

プログラムで使用するユーザー インターフェイス オブジェクトを作成する場合C#(でビューや例については、ビュー コント ローラー)、次の手順に従います。

-  ユーザー インターフェイス オブジェクトのクラス レベルのフィールドを宣言します。 コントロール自体の作成に 1 回、`ViewDidLoad`など。 オブジェクトのビュー コント ローラー (例: ライフ サイクル メソッド全体で参照できます。
`ViewWillAppear`)
-  作成、`CGRect`コントロール (その X 座標と Y 座標で、画面だけでなく、幅と高さ) のフレームを定義します。 あるかどうかを確認する必要があります、`using CoreGraphics`このディレクティブ。
-  作成し、コントロールを割り当てるコンス トラクターを呼び出します。
-  プロパティまたはイベント ハンドラーを設定します。
-  呼び出す`Add()`ビュー階層にコントロールを追加します。

作成する簡単な例を次に示します、`UILabel`ビュー コント ローラーを使用して、 C#:

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

## <a name="using-c-and-storyboards"></a>使用してC#とストーリー ボード

ビュー コント ローラーが、対応する 2 つデザイン サーフェイスに追加されたときC#ファイル、プロジェクトに作成されます。 この例で`ControlsViewController.cs`と`ControlsViewController.designer.cs`自動的に作成されています。

 [![](creating-ui-objects-images/image9b.png "ViewController 部分クラス")](creating-ui-objects-images/image9b.png#lightbox)

`MainViewController.cs`ファイルは、*コード*します。 これは、ような場合、`View`などのライフ サイクル メソッド`ViewDidLoad`と`ViewWillAppear`実装は、独自のプロパティ、フィールドおよびメソッドを追加できます。

`ControlsViewController.designer.cs`生成されたコードは、部分クラスを含むです。 設計上のコントロールに名前が for Mac、Visual Studio の画面を Xcode では、対応するプロパティ、または部分メソッド、コンセントまたはアクションを作成したり、デザイナー (designer.cs) ファイルに追加されます。 次のコードは、ボタンの 1 つもがある 2 つのボタンとテキスト ビューでは、生成されたコードの例を示します、`TouchUpInside`イベント。

部分クラスのこれらの要素には、コントロールを参照し、デザイン画面で宣言されているアクションに応答するようにコードが有効にします。

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

`designer.cs`ファイルを手動で編集しないでください: ストーリー ボードと同期を維持するため、IDE (Visual Studio for Mac または Visual Studio) が担当します。

ユーザー インターフェイス オブジェクトがプログラムに追加されたときに、`View`または`ViewController`、インスタンス化し、自分では、オブジェクト参照を管理する、したがってデザイナー ファイルは必要ありません。



## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://developer.xamarin.com/samples/Controls/)
