---
title: "ユーザー インターフェイス オブジェクトを作成します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 035956f5c39a77c625a6f4cb92cbfa67a42f2402
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="creating-user-interface-objects"></a>ユーザー インターフェイス オブジェクトを作成します。

機能と同じで Xamarin.iOS 名前空間の「フレームワーク」を使用する Apple のグループの関連部分です。 `UIKit` iOS 用のすべてのユーザー インターフェイス コントロールを含む名前空間がします。

コードは、ラベル、ボタンなどのユーザー インターフェイス コントロールを参照する必要があるとき、次のとおりに注意してください。 ステートメントを使用します。

```csharp
using UIKit;
```


UIKit 名前空間にこの章で説明されているすべてのコントロールがあり、各ユーザー コントロールのクラス名が、`UI`プレフィックス。

UI コントロールと 3 つの方法でのレイアウトを編集できます。

-  **[Xamarin iOS デザイナー](~/ios/user-interface/designer/index.md)**  – を使用して Xamarin の組み込みのレイアウトのデザイナー画面をデザインします。 ストーリー ボードまたは組み込みのデザイナーで編集する XIB ファイルをダブルクリックします。
-  **Xcode インターフェイス ビルダー** – インターフェイスのビルダーを使用して画面のレイアウトにコントロールをドラッグします。 内のファイルを右クリックして、Xcode でストーリー ボードまたは XIB ファイルを開く、**ソリューション パッド**を選択して**ファイルを開く > Xcode インターフェイス ビルダー**です。
-  **C# を使用して**– コントロールもプログラム コードで作成してビュー階層に追加します。

IOS プロジェクトを右クリックして を選択して新しいストーリー ボードおよび XIB ファイルを追加することができます**追加 > 新しいファイル.**.

どちらの方法を使用すると、コントロールのプロパティとイベントを操作できます c# を使用して、アプリケーション ロジックです。

## <a name="using-xamarin-ios-designer"></a>Xamarin iOS デザイナーを使用してください。

IOS デザイナーで、ユーザー インターフェイスの作成を開始するには、ストーリー ボード ファイルをダブルクリックします。 デザイン サーフェイスにコントロールをドラッグすることができます、**ツールボックス**下図のようにします。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 [ ![](creating-ui-objects-images/image2b.png "ツールボックス パッド")](creating-ui-objects-images/image2b.png)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

 [ ![](creating-ui-objects-images/image2b-vs.png "ツールボックス パッド - Visual Stuio")](creating-ui-objects-images/image2b.png)
 
-----

デザイン画面でコントロールを選択すると、**プロパティ パッド**そのコントロールの属性が表示されます。 **ウィジェット > Identity > 名前**としてフィールドには、次のスクリーン ショットの設定を使用、*コンセント*名。 C# でのコントロールを参照する方法を示します。

 [ ![](creating-ui-objects-images/image3b.png "プロパティのウィジェット パッド")](creating-ui-objects-images/image3b.png)

さらに詳しく知り iOS デザイナーを使用して、参照してください、 [iOS デザイナーの概要](~/ios/user-interface/designer/introduction.md)ガイドです。

## <a name="using-xcode-interface-builder"></a>Xcode インターフェイスのビルダーを使用します。

インターフェイスのビルダーを使用してに慣れていない場合は、Apple を参照してください[インターフェイス ビルダー](https://developer.apple.com/xcode/interface-builder/)ドキュメント。

Xcode でストーリー ボードを開き、右クリックして、ストーリー ボード ファイルのコンテキスト メニューにアクセスしてでを開く、 **Xcode インターフェイス ビルダー**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 [ ![](creating-ui-objects-images/imagexcode.png "ストーリー ボードのコンテキスト メニュー - Xcode")](creating-ui-objects-images/imagexcode.png)
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[ ![](creating-ui-objects-images/imagexcode-vs.png "ストーリー ボードのコンテキスト メニュー - Xcode")](creating-ui-objects-images/imagexcode-vs.png)

-----

デザイン サーフェイスにコントロールをドラッグすることができます、**オブジェクト ライブラリ**を次に示します。

 [ ![](creating-ui-objects-images/image5a.png "Xcode オブジェクト ライブラリ")](creating-ui-objects-images/image5a.png)

インターフェイスのビルダーを作成する必要がありますで UI を設計する際、**コンセント**c# で参照する各コントロールです。 これは、オンで、**アシスタント エディター**センターを使用して**エディター** Xcode ツール バー ボタンのボタン。

 [ ![](creating-ui-objects-images/image6a.png "アシスタント エディター ボタン")](creating-ui-objects-images/image6a.png)

ユーザー インターフェイス オブジェクト; をクリックします。**コントロールのドラッグ**.h ファイルにします。 * * コントロール ドラッグ * *、コントロールのキーを押しながらクリックし、ユーザー インターフェイス オブジェクト上に置くのコンセント (またはアクション) を作成しています。 保持コントロール キーを押しながら、ヘッダー ファイルにドラッグします。 [完了] の下にドラッグして、`@interface`定義します。 青い線は、次のスクリーン ショットに示すように、キャプション コンセントを挿入またはコンセント コレクションで表示されます。

クリックを解放する場合は、コードで参照できる c# プロパティを作成するために使用すると、出口の名前を指定するよう求められます。

 [ ![](creating-ui-objects-images/image8a.png "コンセントを作成します。")](creating-ui-objects-images/image8a.png)

Mac 用 Xcode のインターフェイスのビルダーを Visual Studio と統合する方法の詳細についてを参照してください、 [Xib コード生成](~/ios/internals/xib-code-generation.md#generated)ドキュメント。

##  <a name="using-c"></a>C# を使用してください。

(ビューや例については、ビューのコント ローラー) で c# を使用して、ユーザー インターフェイス オブジェクトをプログラムで作成する場合は、次の手順に従います。

-  ユーザー インターフェイス オブジェクトのクラス レベル フィールドを宣言します。 コントロール自体の作成に 1 回、`ViewDidLoad`例を示します。 オブジェクトを参照するビューのコント ローラー (のライフ サイクル メソッド
`ViewWillAppear`)
-  作成、`CGRect`コントロール (の X 座標と Y 座標の画面で、だけでなく、幅と高さに) のフレームを定義します。 あるかどうかを確認する必要があります、`using CoreGraphics`このディレクティブです。
-  作成して、コントロールを割り当てるコンス トラクターを呼び出します。
-  プロパティまたはイベント ハンドラーを設定します。
-  呼び出す`Add()`ビュー階層にコントロールを追加します。

ここで作成する単純な例を示します、`UILabel`ビュー コント ローラーを使用して C# の場合。

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

## <a name="using-c-and-storyboards"></a>C# とストーリー ボードを使用します。

コント ローラーの表示がデザイン画面に追加されると、c# の 2 つの対応するファイルがプロジェクトに作成されます。 この例では`ControlsViewController.cs`と`ControlsViewController.designer.cs`自動的に作成されています。

 [ ![](creating-ui-objects-images/image9b.png "ViewController 部分クラス")](creating-ui-objects-images/image9b.png)

`MainViewController.cs`ファイルはもの*コード*です。 ここには、`View`などのライフ サイクル メソッド`ViewDidLoad`と`ViewWillAppear`実装される独自のプロパティ、フィールドおよびメソッドを追加したりできます。

`ControlsViewController.designer.cs`を含む部分クラスを生成されたコードです。 設計上のコントロールに名前が for Mac を Visual Studio で画面を Xcode は、対応するプロパティ、または部分メソッド出口またはアクションを作成したりデザイナー (designer.cs) ファイルに追加されます。 次のコードもが、ボタンのいずれかの場所という 2 つのボタンとテキスト ビューに対して生成されたコードの例を示します、`TouchUpInside`イベント。

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

`designer.cs`ファイルを手動で編集しないで – はストーリー ボードと同期しておくために、IDE (Mac または Visual Studio の Visual Studio) があります。

ユーザー インターフェイス オブジェクトがプログラムに追加されたときに、`View`または`ViewController`のインスタンスを作成し、自分でのオブジェクト参照を管理するためのデザイナー ファイルを必要としません。



## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://developer.xamarin.com/samples/Controls/)
