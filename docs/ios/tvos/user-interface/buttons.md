---
title: Xamarin での tvOS ボタンの使用
description: このドキュメントでは、Xamarin でビルドされた tvOS アプリでボタンを操作する方法について説明します。 ここでは、コードとストーリーボードでボタンを操作する方法と、ボタンのスタイルを確認する方法について説明します。
ms.prod: xamarin
ms.assetid: DA6EF400-A4E3-4245-A0D4-F2398CAE2C9B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/07/2017
ms.openlocfilehash: e0f2939c534c009e54d6812c3a45ac6c759d1247
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657305"
---
# <a name="working-with-tvos-buttons-in-xamarin"></a>Xamarin での tvOS ボタンの使用

クラスのインスタンスを使用`UIButton`して、tvOS ウィンドウにフォーカスがある、選択可能なボタンを作成します。 ユーザーがボタンを選択すると、ターゲットオブジェクトにアクションメッセージが送信され、tvOS アプリがユーザーの入力に応答できるようになります。

[![](buttons-images/buttons01.png "ボタンの例")](buttons-images/buttons01.png#lightbox)

Siri リモートを使用してフォーカスを操作する方法の詳細については、「[ナビゲーションとフォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)、 [Siri リモートおよび Bluetooth コントローラー](~/ios/tvos/platform/remote-bluetooth.md)の操作」のドキュメントを参照してください。

<a name="About-Buttons" />

## <a name="about-buttons"></a>ボタンの概要

TvOS では、ボタンはアプリ固有のアクションに使用され、タイトル、アイコン、またはその両方が含まれる場合があります。 ユーザーが[Siri リモート](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)を使用してアプリのユーザーインターフェイスを移動すると、指定したボタンにフォーカスが移り、テキストと背景色が変更されます。 影は、ボタンにも適用されます。これにより、ユーザーインターフェイスの他の部分を超えて3D 効果が上がります。

[![](buttons-images/buttons01.png "ボタンの例")](buttons-images/buttons01.png#lightbox)

Apple では、ボタンの操作に関して次のような推奨事項があります。

- **タイトルまたはアイコンを使用**します。一方、ボタンにはアイコンとタイトルの両方を含めることができますが、スペースは制限されているので、両方を結合しないようにしてください。
- **破棄ボタンを明確にマーク**する-ボタンが破壊的な操作 (ファイルの削除など) を実行する場合は、テキストやアイコンを使用して、それを明確にマークします。 破壊的なアクションでは、ユーザーにアクションを制限するように求める[アラート](~/ios/tvos/user-interface/alerts.md)を常に提示する必要があります。
- [**戻る] ボタンを使用しない**-Siri リモートのメニューボタンは、前の画面に戻るために使用されます。 このルールの唯一の例外は、 **[キャンセル**] ボタンが表示されるアプリ内購入または破壊的なアクションです。

フォーカスとナビゲーションの操作の詳細については、「[ナビゲーションとフォーカスのドキュメントの操作](~/ios/tvos/app-fundamentals/navigation-focus.md)」を参照してください。

<a name="Button-Icons" />

### <a name="button-icons"></a>ボタンアイコン

Apple では、ボタンアイコンに対して、シンプルで認識しやすいイメージを使用することが提案されています。 非常に複雑なアイコンは、ソファの部屋にあるテレビ画面で認識するのは困難です。そのため、考えられる最も単純な表現を使用してください。 可能な限り、標準のよく知られた画像をアイコンに使用します (検索用の虫眼鏡など)。

<a name="Button-Titles" />

### <a name="button-titles"></a>ボタンタイトル

Apple では、ボタンのタイトルを作成するときに次のような推奨事項があります。

- **アイコンボタンの下にわかりやすいテキストを表示**します。可能な場合は、[クリア]、[説明テキストをアイコンのみ] の下に配置して、ボタンの目的をさらに詳細に取得します。
- **タイトルに動詞または動詞句を使用する**-ユーザーがボタンをクリックしたときに実行されるアクションを明確にします。
- **タイトルとスタイルの大文字小**文字を使用します。記事、接続詞、または前置詞 (4 文字以下) を除き、ボタンのタイトルのすべての単語は大文字にする必要があります。
- **短いタイトルのタイトルを使用**する-可能な限り短い表現を使用して、ボタンのアクションを記述します。

<a name="Buttons-and-Storyboards" />

## <a name="buttons-and-storyboards"></a>ボタンとストーリーボード

TvOS アプリのボタンを操作する最も簡単な方法は、Xamarin Designer for iOS を使用してアプリの UI に追加することです。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)


1. **ソリューションエクスプローラー**で、 `Main.storyboard`ファイルをダブルクリックして開き、編集します。
1. **ライブラリ**から**ボタン**をドラッグし、ビューにドロップします。 

    [![](buttons-images/storyboard01.png "ボタン")](buttons-images/storyboard01.png#lightbox)
1. **プロパティエクスプローラー**では、ボタンの**タイトル**や**テキストの色**など、いくつかのプロパティを調整できます。 

    [![](buttons-images/storyboard02.png "ボタンのプロパティ")](buttons-images/storyboard02.png#lightbox)
1. 次に、[**イベント] タブ**に切り替えて、**ボタン**から**イベント**を接続し、次`ButtonPressed`のように呼び出します。 

    [![](buttons-images/storyboard03.png "[イベント] タブ")](buttons-images/storyboard03.png#lightbox)
1. **上**方向キーと**下**方向キー `ViewController.cs`を使用して、新しいアクションをコードに配置できるビューに自動的に切り替えられます。 

    [![](buttons-images/storyboard04.png "コードに新しいアクションを配置する")](buttons-images/storyboard04.png#lightbox)
1. **Enter**キーを押して場所を選択します。 

    [![](buttons-images/storyboard05.png "コードエディター")](buttons-images/storyboard05.png#lightbox)
1. すべてのファイルに変更を保存します。


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューションエクスプローラー**で、 `Main.storyboard`ファイルをダブルクリックして開き、編集します。
1. **ライブラリ**から**ボタン**をドラッグし、ビューにドロップします。 

    [![](buttons-images/storyboard01vs.png "ボタン")](buttons-images/storyboard01vs.png#lightbox)
1. **プロパティエクスプローラー**では、ボタンの**タイトル**や**テキストの色**など、いくつかのプロパティを調整できます。 

    [![](buttons-images/storyboard02vs.png "プロパティエクスプローラー")](buttons-images/storyboard02vs.png#lightbox)
1. 次に、[**イベント] タブ**に切り替えて、**ボタン**から**イベント**を接続し、次`ButtonPressed`のように呼び出します。 

    [![](buttons-images/storyboard03vs.png "[イベント] タブ")](buttons-images/storyboard03vs.png#lightbox)
1. すべてのファイルに変更を保存します。



ビューコントローラー (例`ViewController.cs`) ファイルを編集し、選択されているボタンを処理する次のコードを追加します。


```

using System;
using UIKit;

namespace tvRemote
{
    public partial class ViewController : UIViewController
    {
        ...

        partial void ButtonPressed (UIButton sender) {
            // Handle click here
            ...
        }
    }
}

```

-----

ボタンの`Enabled`プロパティが`true`であり、他のコントロールまたはビューでカバーされていない場合は、siri リモートを使用してフォーカスを設定された項目にすることができます。 ユーザーがボタンを選択し、タッチ画面をクリックすると`ButtonPressed` 、上で定義されたアクションが実行されます。

> [!IMPORTANT]
> `TouchUpInside` **イベントハンドラー**の作成時に、などの`UIButton`アクションを iOS デザイナーでに割り当てることはできますが、Apple TV にタッチスクリーンやタッチイベントのサポートがないため、このようなアクションは呼び出されません。 TvOS ユーザーインターフェイス要素の**アクション**を作成するときは、常に既定の**アクションの種類**を使用する必要があります。




ストーリーボードの操作の詳細については、「 [Hello, tvOS クイックスタートガイド](~/ios/tvos/get-started/hello-tvos.md)」を参照してください。

<a name="Buttons-and-Code" />

## <a name="buttons-and-code"></a>ボタンとコード

必要に応じ`UIButton`て、をコードC#で作成し、tvOS アプリのビューに追加することができます。 例えば:

```csharp
var button = new UIButton(UIButtonType.System);
button.Frame = new CGRect (25, 25, 300, 150);
button.SetTitle ("Hello", UIControlState.Normal);
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
View.AddSubview (button);
```

コードに新しい`UIButton`を作成する場合は、次の`UIButtonType`いずれかとしてを指定します。

- **システム**-tvOS によって表示される標準のボタンの種類であり、最も頻繁に使用する型です。
- 詳細情報の**開示**-詳細情報を表示または非表示にするために使用されるボタンの "停止" の種類を示します。
- **インフォダーク**-[濃い詳細情報] ボタンをクリックすると、円に "i" が表示されます。
- [ライトの詳細情報] ボタンをクリックすると、円に "i" が表示されます。
- **Addcontact** -ボタンを [Add contact] ボタンとして表示します。
- **[カスタム]** : ボタンのいくつかの特徴をカスタマイズできます。

次に、画面上のサイズとボタンの位置を定義します。 例:

```csharp
button.Frame = new CGRect (25, 25, 300, 150);
```

次に、ボタンのタイトルを設定します。 `UIButtons`のほとんど`UIKit`のコントロールとは異なります。これは、単にタイトルを変更するだけではなく、特定`UIControlState`ののために変更する必要があるためです。 例えば:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

次に、 `AllEvents`イベントを使用して、ユーザーがボタンをクリックしたことを確認します。 例:

```csharp
button.AllEvents += (sender, e) => {
    // Do something when the button is clicked
    ...
};
```

最後に、ボタンをビューに追加して表示します。

```csharp
View.AddSubview (button);
```

> [!IMPORTANT]
> など`TouchUpInside` の`UIButton`アクションをに割り当てることはできますが、Apple TV はタッチスクリーンやサポートタッチイベントを持っていないため、呼び出されません。 **AllEvents**や**primaryactiontriggered**などのイベントを常に使用する必要があります。




<a name="Styling-a-Button" />

## <a name="styling-a-button"></a>ボタンのスタイル設定

tvOS には、のタイトル`UIButton`を提供したり、背景色や画像などのスタイルを設定したりするために使用できる、のいくつかのプロパティが用意されています。

<a name="Button-Titles" />

### <a name="button-titles"></a>ボタンタイトル

前述のように、 `UIButtons`のほとんど`UIKit`のコントロールとは異なります。これは、単にタイトルを変更するだけではなく、特定`UIControlState`のに変更する必要があるためです。 例えば:

```csharp
button.SetTitle ("Hello", UIControlState.Normal);
```

ボタンのタイトルの色は、 `SetTitleColor`メソッドを使用して設定できます。 例えば:

```csharp
button.SetTitleColor (UIColor.White, UIControlState.Normal);
```

また、 `SetTitleShadowColor`を使用して、タイトルの影を調整することもできます。 例えば:

```csharp
button.SetTitleShadowColor(UIColor.Black, UIControlState.Normal);
```

次のコードを使用して、ボタンが強調表示されているときに、影を*浮き彫り*から*エンボス*に変更するように設定できます。

```csharp
button.ReverseTitleShadowWhenHighlighted = true;
```

また、ボタンのタイトルとして属性付きテキストを使用することもできます。 例えば:

```csharp
var normalAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Blue, strikethroughStyle: NSUnderlineStyle.Single);
myButton.SetAttributedTitle (normalAttributedTitle, UIControlState.Normal);

var highlightedAttributedTitle = new NSAttributedString (buttonTitle, foregroundColor: UIColor.Green, strikethroughStyle: NSUnderlineStyle.Thick);
myButton.SetAttributedTitle (highlightedAttributedTitle, UIControlState.Highlighted);
```

### <a name="button-images"></a>ボタンイメージ

は`UIButton` 、イメージを添付し、背景としてイメージを使用できます。

特定`UIControlState`ののボタンの背景イメージを設定するには、次のコードを使用します。

```csharp
button.SetBackgroundImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

ボタンが強調表示`true`されているときにイメージを明るくする場合は、プロパティをに設定します(これが既定値です)。`AdjustsImageWhenHiglighted` ボタンが無効に`true`なっているときにイメージを暗くするには、プロパティをに設定します(ここでも、これが既定値です)。`AdjustsImageWhenDisabled`

ボタンに表示されるイメージを設定するには、次のコードを使用します。

```csharp
button.SetImage(UIImage.FromFile("my image.png"), UIControlState.Normal);
```

タイトルと`TintColor`ボタンのイメージの両方に適用される色の濃淡を設定するには、プロパティを使用します。 `Custom`型のボタンの場合、このプロパティは効果がありません。自分`TintColor`で動作を実装する必要があります。

<a name="Summary" />

## <a name="summary"></a>Summary

この記事では、tvOS アプリ内のボタンの設計と操作について説明しました。 ここでは、iOS デザイナーでボタンを操作する方法と、コードでC#ボタンを作成する方法を示しました。 最後に、ボタンのタイトルを変更し、そのスタイルと外観を変更する方法を示しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
