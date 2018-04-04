---
title: アラート
description: この記事では、Xamarin.Mac アプリケーションでアラートの使用について説明します。 ユーザー インタラクションに対して応答の作成と c# コードからのアラートの表示について説明します。
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: a451d0a5535915d9e52f687ae07ea028c0ccd5ef
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="alerts"></a>アラート

_この記事では、Xamarin.Mac アプリケーションでアラートの使用について説明します。ユーザー インタラクションに対して応答の作成と c# コードからのアラートの表示について説明します。_

アクセス権がある Xamarin.Mac アプリケーションでは、c# と .NET で作業するときに同じ警告で作業する開発者*OBJECTIVE-C*と*Xcode*はします。 

警告は、特殊な種類 (エラー) など、深刻な問題が発生したときに表示されるダイアログ ボックスのまたは (ファイルを削除する準備をしています) などの警告として。 アラートは、ダイアログ ボックスであるため、必要もありますユーザーからの応答を終了する前にします。

[![](alert-images/alert06.png "例アラート")](alert-images/alert06.png#lightbox)

この記事で Xamarin.Mac アプリケーションでアラートの操作の基礎について説明します。 

<a name="Introduction_to_Alerts" />

## <a name="introduction-to-alerts"></a>アラートの概要

警告は、特殊な種類 (エラー) など、深刻な問題が発生したときに表示されるダイアログ ボックスのまたは (ファイルを削除する準備をしています) などの警告として。 アラートは、ユーザーがそのタスクにで続行する前に破棄する必要がありますので、ユーザーを中断、ために、どうしても必要である場合を除き、アラートを表示しないでください。

Apple では、次のガイドラインを提案します。

- 情報をユーザーに付与するだけで、アラートを使用しないでください。
- 一般的な取り消し可能なアクションのアラートを表示しません。 場合でも、このような場合は、データの損失にあります。
- 状況がアラートに値する場合は、表示するその他の UI 要素またはメソッドを使用しないでください。
- 警告アイコンを多用する必要があります。
- アラート メッセージで、アラート状態を明確かつ簡潔に説明します。
- 既定のボタン名、警告メッセージで記述したアクションに対応します。

詳細については、次を参照してください、[アラート](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAlerts.html#//apple_ref/doc/uid/20000957-CH44-SW1)Apple のセクション[OS X のヒューマン インターフェイス ガイドライン。](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Anatomy_of_an_Alert" />

## <a name="anatomy-of-an-alert"></a>アラートの構造

前述のように、深刻な問題が発生したときに、アプリケーションのユーザーに、または (未保存のファイルを閉じる) などのデータ損失の可能性を警告としてアラートを表示する必要があります。 Xamarin.Mac で、アラートが作成 c# コードで例を示します。

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Critical,
    InformativeText = "We need to save the document here...",
    MessageText = "Save Document",
};
alert.RunModal ();
```

上記のコードの警告アイコン、タイトル、警告メッセージと、1 つに重ね合わせて示す、アプリケーションのアイコンで警告が表示されます**OK**ボタンをクリックします。

[![](alert-images/alert01.png "[Ok] ボタンのアラート")](alert-images/alert01.png#lightbox)

Apple では、アラートをカスタマイズするために使用できるいくつかのプロパティを提供します。

- **AlertStyle**として、次のいずれかのアラートの種類を定義します。
    - **警告**- ユーザーが重要でない、または現在の差し迫ったイベントの警告に使用されます。 これは、既定のスタイルです。
    - **情報**- には、現在または間もなくイベントについてユーザーを警告するために使用されます。 現在、表示の違いはありません、**警告**と**情報**
    - **重要な**- には、イベント (ファイルを削除する) などの深刻な事態についてユーザーを警告するために使用されます。 この種類のアラートを多用する必要があります。
- **MessageText** -これは、メイン メッセージまたは警告のタイトルとユーザーに、この状況をすばやく定義する必要があります。
- **InformativeText** -これは、この状況を明確に定義および実行可能なオプションをユーザーに提示する必要があります、アラートの本文。
- **アイコン**-ユーザーに表示されるカスタム アイコンを使用します。
- **HelpAnchor** & **ShowsHelp** -HelpBook アプリケーションに関連付けるし、警告のヘルプを表示するアラートを許可します。
- **ボタン**-既定では、アラートは、 **OK**ボタンが、**ボタン**コレクションでは、必要に応じてより多くの選択肢を追加することができます。
- **ShowsSuppressionButton**場合 -`true`ユーザーの後続の出現箇所をトリガーしたイベントの警告を非表示に使用できるチェック ボックスを表示します。
- **AccessoryView** -別サブビューを追加するなど、追加の情報を提供するアラートにアタッチすることができます、**テキスト フィールド**データ入力用です。 新しい設定した場合**AccessoryView**または既存の変更、呼び出す必要がある、`Layout()`アラートの表示のレイアウトを調整する方法です。

<a name="Displaying_an_Alert" />

## <a name="displaying-an-alert"></a>通知を表示します。

表示されている、型指定されないアラートがあることができます、2 つの方法があるか、シートとして。 次のコードでは、型指定されないとアラートが表示されます。

```csharp
var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
    MessageText = "Alert Title",
};
alert.RunModal ();
```
このコードを実行すると、次が表示されます。

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

このコードを実行すると場合、次が表示されます。

[![](alert-images/alert03.png "シートとして表示されるアラート")](alert-images/alert03.png#lightbox)


<a name="Working_with_Alert_Buttons" />

## <a name="working-with-alert-buttons"></a>アラートのボタンの操作

既定では、アラートのみが表示されます、 **OK**ボタンをクリックします。 ただし、制限はありませんに追加することによって余分なボタンを作成するには、**ボタン**コレクション。 次のコードでは、型指定されないアラートが、 **[ok]**、**キャンセル**と**おそらく**ボタン。

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

追加の非常に最初のボタンになります、_ボタンの既定の_ユーザーが Enter キーを押した場合をアクティブ化されます。 返される値は、ユーザーが押されたボタンを表す整数になります。 ここで、次の値が返されます。

- **[OK]** - 1000 です。
- **キャンセル**1001 - です。
- **状況によって異なる**1002: です。

コードを実行する場合、次が表示されます。

[![](alert-images/alert04.png "次の 3 つのボタンのオプションのアラート")](alert-images/alert04.png#lightbox)

シートとして同じ警告のコードを次に示します。

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
このコードを実行すると場合、次が表示されます。

[![](alert-images/alert05.png "シートとして表示されるボタンの 3 つのアラート")](alert-images/alert05.png#lightbox)

> [!IMPORTANT]
> アラートには、複数の 3 つのボタンを追加しないでです。

<a name="Showing_the_Suppress_Button" />

## <a name="showing-the-suppress-button"></a>非表示 ボタンを示す

場合、アラートの`ShowSuppressButton`プロパティは`true`アラートは、ユーザーの後続の出現箇所をトリガーしたイベントの警告を非表示に使用できるチェック ボックスを表示します。 次のコードでは、フローティング状態のどちらのアラートを抑制するボタンが表示されます。

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

場合の値、`alert.SuppressionButton.State`は`NSCellStateValue.On`、抑制する チェック ボックスをオン、それ以外の場合がありません。

コードを実行すると場合、次が表示されます。

[![](alert-images/alert06.png "アラートと非表示ボタン")](alert-images/alert06.png#lightbox)

シートとして同じ警告のコードを次に示します。

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

このコードを実行すると場合、次が表示されます。

[![](alert-images/alert07.png "アラートと非表示ボタンがシートとして表示します。")](alert-images/alert07.png#lightbox)

<a name="Adding_a_Custom_SubView" />

## <a name="adding-a-custom-subview"></a>カスタム サブビューを追加します。

アラートが、`AccessoryView`をさらに、アラートをカスタマイズおよびなどを追加するために使用できるプロパティ、**テキスト フィールド**ユーザー入力用です。 次のコードでは、入力フィールドが追加されたテキストのフローティング状態のどちらのアラートを作成します。

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

ここで重要な行が`var input = new NSTextField (new CGRect (0, 0, 300, 20));`新たに作成する**テキスト フィールド**アラートを追加しています。 `alert.AccessoryView = input;` どのアタッチ、**テキスト フィールド**、アラートへの呼び出しに、`Layout()`メソッドは、新しいサブビューに収まるように、アラートのサイズを変更するために必要です。

コードを実行する場合、次が表示されます。

[![](alert-images/alert08.png "コードを実行して、次が表示されます。")](alert-images/alert08.png#lightbox)

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

このコードを実行する場合、次が表示されます。

[![](alert-images/alert09.png "カスタム ビューのアラート")](alert-images/alert09.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションでアラートの操作についての詳細を取得しました。 さまざまな種類およびアラート、作成し、アラートをカスタマイズする方法、および c# コードでアラートを処理する方法の使用方法を説明しました。

## <a name="related-links"></a>関連リンク

- [MacWindows (サンプル)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [ウィンドウの操作](~/mac/user-interface/window.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [NSAlert](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSAlert_Class/index.html#//apple_ref/doc/uid/TP40004001)
