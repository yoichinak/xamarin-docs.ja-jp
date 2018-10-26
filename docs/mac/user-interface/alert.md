---
title: Xamarin.Mac のアラート
description: この記事では、Xamarin.Mac アプリケーションでのアラートの操作について説明します。 作成してからのアラートを表示する説明C#コードとユーザーの操作に応答します。
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 8f84b688998251db52c8c2be71949e1a2e665dc0
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103960"
---
# <a name="alerts-in-xamarinmac"></a>Xamarin.Mac のアラート

_この記事では、Xamarin.Mac アプリケーションでのアラートの操作について説明します。作成してからのアラートを表示する説明C#コードとユーザーの操作に応答します。_

使用する場合C#へのアクセスがある、Xamarin.Mac アプリケーションで .NET、および作業する開発者で、同じアラートを*Objective C*と*Xcode*は。 

警告は、特殊な種類 (エラー) など、深刻な問題が発生したときに表示されるダイアログ ボックスのまたは (ファイルを削除する準備をしています) などの警告として。 アラートは、ダイアログ ボックスであるためも必要です、ユーザーの応答を閉じる前にします。

[![](alert-images/alert06.png "アラートの例")](alert-images/alert06.png#lightbox)

この記事では、アラート、Xamarin.Mac アプリケーションでの操作の基礎を取り上げます。 

<a name="Introduction_to_Alerts" />

## <a name="introduction-to-alerts"></a>アラートの概要

警告は、特殊な種類 (エラー) など、深刻な問題が発生したときに表示されるダイアログ ボックスのまたは (ファイルを削除する準備をしています) などの警告として。 アラートは、そのタスクをユーザーに続行前に破棄する必要がありますので、ユーザーを中断、ため、絶対に必要である場合を除き、アラートを表示しないでください。

Apple では、次のガイドラインをお勧めします。

- ユーザー情報を提供するだけで、アラートを使用しないでください。
- 共通の取り消し可能なアクションに関する警告は表示されません。 場合でも、そのような状況データの損失が発生する可能性があります。
- 状況は、アラート場合、は、それを表示するその他の UI 要素またはメソッドを使用しないでください。
- 注意のアイコンは控えめに使用する必要があります。
- アラートの状態を明確かつ簡潔に示す警告メッセージに説明します。
- ボタンの既定の名前は、警告メッセージで記述したアクションに対応しています。

詳細については、次を参照してください、[アラート](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAlerts.html#//apple_ref/doc/uid/20000957-CH44-SW1)の Apple の「 [OS X ヒューマン インターフェイス ガイドライン。](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Anatomy_of_an_Alert" />

## <a name="anatomy-of-an-alert"></a>アラートの詳細

前述のように、重大な問題が発生した場合、アプリケーションのユーザーに、または (保存されていないファイルを閉じる) などのデータ損失の可能性を警告としてアラートが表示する必要があります。 アラートを作成、Xamarin.Mac でC#コード、たとえば。

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Critical,
    InformativeText = "We need to save the document here...",
    MessageText = "Save Document",
};
alert.RunModal ();
```

上記のコードでは、警告アイコン、タイトル、警告メッセージ、および 1 つに重ね合わせて示す、アプリケーション アイコンと警告が表示されます**OK**ボタンをクリックします。

[![](alert-images/alert01.png "[Ok] ボタンのアラート")](alert-images/alert01.png#lightbox)

Apple では、アラートをカスタマイズするために使用できるいくつかのプロパティを提供します。

- **AlertStyle**として、次のいずれかのアラートの種類を定義します。
    - **警告**- ユーザーが重要でない、現在または近いイベントを警告するために使用します。 これは、既定のスタイルです。
    - **情報**- 現在または迫っているイベントについてユーザーに警告に使用されます。 現時点では、表示の違いはありません、**警告**と**情報**
    - **重要な**- の兆候のイベント (ファイルを削除する) などの深刻な事態についてユーザーに警告に使用されます。 この種類のアラートは、控えめに使用する必要があります。
- **MessageText** -これは、メイン メッセージまたは警告のタイトルとユーザーに、状況を簡単に定義する必要があります。
- **InformativeText** -これは、本文のアラートの状況を明確に定義および実行可能なオプションをユーザーに提示する必要があります。
- **アイコン**-ユーザーに表示されるカスタム アイコンを使用します。
- **HelpAnchor** & **ShowsHelp** -HelpBook アプリケーションに関連付けられるし、アラートのヘルプを表示するアラートを使用します。
- **ボタン**-既定では、アラートは、 **[ok]** ボタンが、**ボタン**コレクションでは、必要に応じてより多くの選択肢を追加できます。
- **ShowsSuppressionButton**場合 -`true`ユーザーがそれをトリガーしたイベントの後続の出現箇所の警告を非表示に使用できるチェック ボックスを表示します。
- **AccessoryView** -アラートの追加などの追加情報を提供するもう 1 つのサブビューをアタッチすることができます、**テキスト フィールド**データ入力用です。 新しい設定した場合**AccessoryView**または、既存の変更を呼び出す必要があります、`Layout()`アラートの表示レイアウトを調整する方法。

<a name="Displaying_an_Alert" />

## <a name="displaying-an-alert"></a>通知を表示します。

表示されている、フローティング状態であるアラートがあることができます、2 つの方法はありますか、シートとして。 次のコードでは、フローティング状態であると、アラートが表示されます。

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.RunModal ();
```
このコードを実行すると場合、次のように表示されます。

[![](alert-images/alert02.png "単純なアラート")](alert-images/alert02.png#lightbox)

次のコードでは、シートとして同じアラートが表示されます。

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.BeginSheet (this);
```

このコードを実行している場合、次が表示されます。

[![](alert-images/alert03.png "シートとして表示されるアラート")](alert-images/alert03.png#lightbox)


<a name="Working_with_Alert_Buttons" />

## <a name="working-with-alert-buttons"></a>アラートのボタンを使用

既定では、アラートのみが表示されます、 **OK**ボタンをクリックします。 ただし、制限はありませんに追加することで追加ボタンを作成するには、**ボタン**コレクション。 次のコードでは、フローティング状態であるアラートを**OK**、**キャンセル**と**かもしれません**ボタン。

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
var result = alert.RunModal ();
```

追加された最初のボタンは使用できます、_ボタンの既定の_ユーザーが Enter キーを押した場合をアクティブになります。 返される値は、ユーザーが押されたボタンを表す整数になります。 ここで、次の値が返されます。

- **[OK]** - 1000 です。
- **キャンセル**1001 - します。
- **おそらく**1002 - します。

コードを実行した場合、次が表示されます。

[![](alert-images/alert04.png "ボタンの 3 つのオプションのアラート")](alert-images/alert04.png#lightbox)

シートとして、同じアラートのコードを次に示します。

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}", result);
});
```
このコードを実行している場合、次が表示されます。

[![](alert-images/alert05.png "シートとして表示されるボタンの 3 つのアラート")](alert-images/alert05.png#lightbox)

> [!IMPORTANT]
> アラートには、複数の 3 つのボタンを追加しないでください。

<a name="Showing_the_Suppress_Button" />

## <a name="showing-the-suppress-button"></a>非表示 ボタンを示す

場合、アラートの`ShowSuppressButton`プロパティは`true`アラートは、ユーザーがそれをトリガーしたイベントの後続の出現箇所の警告を非表示に使用できるチェック ボックスを表示します。 次のコードでは、フローティング状態であるアラートを抑制する ボタンを表示します。

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

場合の値、`alert.SuppressionButton.State`は`NSCellStateValue.On`、抑制する チェック ボックスをオン、それ以外の場合、必要ありません。

コードが実行された場合、次が表示されます。

[![](alert-images/alert06.png "アラートを抑制する ボタン")](alert-images/alert06.png#lightbox)

シートとして、同じアラートのコードを次に示します。

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

このコードを実行している場合、次が表示されます。

[![](alert-images/alert07.png "シートとしてアラートを抑制する ボタンを表示します。")](alert-images/alert07.png#lightbox)

<a name="Adding_a_Custom_SubView" />

## <a name="adding-a-custom-subview"></a>カスタムのサブビューを追加します。

アラートが、`AccessoryView`さらに、アラートをカスタマイズなどを追加するために使用できるプロパティ、**テキスト フィールド**ユーザーの入力。 次のコードでは、追加したテキスト入力フィールドでフローティング状態であるアラートを作成します。

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
AlertStyle = NSAlertStyle.Informational,
InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
var result = alert.RunModal ();
Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
```

主要な行は`var input = new NSTextField (new CGRect (0, 0, 300, 20));`新たに作成される**テキスト フィールド**アラートを追加する予定こと。 `alert.AccessoryView = input;` どの接続、**テキスト フィールド**、アラートへの呼び出しに、`Layout()`メソッドで、新しいサブビューに収まるように、アラートのサイズを変更するには必要です。

コードを実行した場合、次が表示されます。

[![](alert-images/alert08.png "コードを実行した場合、次が表示されます。")](alert-images/alert08.png#lightbox)

シートとして同じアラートを次に示します。

```csharp
var input = new NSTextField (new CGRect (0, 0, 300, 20));

var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.AddButton ("Ok");
alert.AddButton ("Cancel");
alert.AddButton ("Maybe");
alert.ShowsSuppressionButton = true;
alert.AccessoryView = input;
alert.Layout ();
alert.BeginSheetForResponse (this, (result) => {
    Console.WriteLine ("Alert Result: {0}, Suppress: {1}", result, alert.SuppressionButton.State == NSCellStateValue.On);
});
```

このコードを実行した場合、次が表示されます。

[![](alert-images/alert09.png "カスタム ビューのアラート")](alert-images/alert09.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションでアラートの使用方法について詳しく説明をしました。 さまざまな種類と警告を作成し、アラートをカスタマイズする方法、アラートを操作する方法の使用方法を説明しましたC#コード。

## <a name="related-links"></a>関連リンク

- [MacWindows (サンプル)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows の操作](~/mac/user-interface/window.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [NSAlert](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSAlert_Class/index.html#//apple_ref/doc/uid/TP40004001)
