---
title: Xamarin. Mac のアラート
description: この記事では、Xamarin. Mac アプリケーションでのアラートの使用について説明します。 ここでは、コードからのC#アラートの作成と表示、およびユーザーの操作への応答について説明します。
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: c97006d1afb68d693e2792879788ea92907873fc
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69889539"
---
# <a name="alerts-in-xamarinmac"></a>Xamarin. Mac のアラート

_この記事では、Xamarin. Mac アプリケーションでのアラートの使用について説明します。ここでは、コードからのC#アラートの作成と表示、およびユーザーの操作への応答について説明します。_

Xamarin. Mac C#アプリケーションでと .net を使用する場合、 *Xcode および*で作業する開発者が行うのと同じアラートにアクセスできます。 

アラートは、重大な問題 (エラーなど) が発生したとき、または警告 (ファイルの削除の準備など) が発生したときに表示される特別な種類のダイアログです。 アラートはダイアログであるため、閉じる前にユーザーの応答も必要です。

[![](alert-images/alert06.png "アラートの例")](alert-images/alert06.png#lightbox)

この記事では、Xamarin. Mac アプリケーションでのアラートの操作の基本について説明します。 

<a name="Introduction_to_Alerts" />

## <a name="introduction-to-alerts"></a>アラートの概要

アラートは、重大な問題 (エラーなど) が発生したとき、または警告 (ファイルの削除の準備など) が発生したときに表示される特別な種類のダイアログです。 ユーザーがタスクを続行できるようになる前にアラートを破棄する必要があるため、警告が表示されないため、必要な場合を除き、アラートを表示しないようにしてください。

Apple では、次のガイドラインを提案します。

- ユーザー情報を提供するためだけにアラートを使用しないでください。
- 一般的な取り消し可能なアクションのアラートを表示しません。 そのような状況でもデータが失われる可能性があります。
- アラートが問題になる場合は、他の UI 要素またはメソッドを使用して表示することを避けてください。
- 警告アイコンは控えめに使用する必要があります。
- アラートの状況を明確かつ簡潔にアラートメッセージに記述します。
- 既定のボタン名は、警告メッセージに記載されているアクションに対応している必要があります。

詳細については、「Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)」の「[アラート](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAlerts.html#//apple_ref/doc/uid/20000957-CH44-SW1)」セクションを参照してください。

<a name="Anatomy_of_an_Alert" />

## <a name="anatomy-of-an-alert"></a>アラートの構造

前述のように、重大な問題が発生したとき、またはデータ損失の可能性に対する警告として、アプリケーションのユーザーに警告を表示する必要があります (保存されていないファイルを閉じるなど)。 Xamarin. Mac では、アラートがコードでC#作成されます。次に例を示します。

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Critical,
  InformativeText = "We need to save the document here...",
  MessageText = "Save Document",
};
alert.RunModal ();
```

上記のコードでは、警告アイコン、タイトル、警告メッセージ、および1つの **[OK** ] ボタンに重ねて表示された [アプリケーション] アイコンを含むアラートが表示されます。

[![](alert-images/alert01.png "[OK] ボタンのあるアラート")](alert-images/alert01.png#lightbox)

Apple には、アラートをカスタマイズするために使用できるいくつかのプロパティが用意されています。

- **Alertstyle**は、アラートの種類を次のいずれかとして定義します。
  - **警告**-重要ではない、現在または差し迫ったイベントをユーザーに警告するために使用されます。 これは既定のスタイルです。
  - **情報**-現在または将来発生するイベントについてユーザーに警告するために使用されます。 現在、**警告**と**情報**の間には、表示される違いはありません。
  - **重大**-イベント (ファイルの削除など) の重大な結果をユーザーに警告するために使用されます。 この種類のアラートは、控えめに使用する必要があります。
- **Messagetext** -アラートのメインメッセージまたはタイトルです。ユーザーに対して状況をすばやく定義します。
- **InformativeText** -これはアラートの本文であり、状況を明確に定義し、使用可能なオプションをユーザーに提示する必要があります。
- **アイコン**-ユーザーにカスタムアイコンを表示することを許可します。
- ShowsHelp-アラートをアプリケーションのヘルプブックに関連付けて、アラートのヘルプを表示することを許可します。  & 
- **ボタン**-既定では、警告には **[OK** ] ボタンしかありませんが、**ボタン**のコレクションでは、必要に応じてさらに選択肢を追加できます。
- **ShowsSuppressionButton** - `true`によって、トリガーされたイベントの後続の発生に対して警告を抑制するために使用できるチェックボックスが表示されます。
- **AccessoryView** -データ入力用の**テキストフィールド**の追加などの追加情報を提供するために、アラートに別のサブビューをアタッチすることができます。 新しい**AccessoryView**を設定するか、既存のものを変更する場合は、 `Layout()`メソッドを呼び出して、アラートの表示レイアウトを調整する必要があります。

<a name="Displaying_an_Alert" />

## <a name="displaying-an-alert"></a>アラートの表示

警告を表示する方法には、フリーフローティングまたはシートという2つの方法があります。 次のコードでは、アラートがフリーフローティングとして表示されます。

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.RunModal ();
```

このコードを実行すると、次のように表示されます。

[![](alert-images/alert02.png "単純なアラート")](alert-images/alert02.png#lightbox)

次のコードでは、シートと同じ警告が表示されます。

```csharp
var alert = new NSAlert () {
  AlertStyle = NSAlertStyle.Informational,
  InformativeText = "This is the body of the alert where you describe the situation and any actions to correct it.",
  MessageText = "Alert Title",
};
alert.BeginSheet (this);
```

このコードが実行されると、次のように表示されます。

[![](alert-images/alert03.png "シートとして表示される警告")](alert-images/alert03.png#lightbox)


<a name="Working_with_Alert_Buttons" />

## <a name="working-with-alert-buttons"></a>アラートボタンの操作

既定では、アラートには **[OK** ] ボタンのみが表示されます。 ただし、これに限定されているわけではありませんが、**ボタン**のコレクションに追加することによって追加のボタンを作成できます。 次のコードでは、" **OK**"、"**キャンセル**"、および "ボタン" を使用して、フリーフローティングの警告を作成します。

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

追加された最初のボタンは、ユーザーが Enter キーを押すとアクティブになる_既定のボタン_になります。 返される値は、ユーザーが押したボタンを表す整数です。 ここでは、次の値が返されます。

- **OK** -1000。
- **キャンセル**-1001。
- 1002の**可能性**があります。

コードを実行すると、次のように表示されます。

[![](alert-images/alert04.png "3つのボタンオプションを持つアラート")](alert-images/alert04.png#lightbox)

次に、同じ警告をシートとして使用するコードを示します。

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

このコードが実行されると、次のように表示されます。

[![](alert-images/alert05.png "シートとして表示される3つのボタンの警告")](alert-images/alert05.png#lightbox)

> [!IMPORTANT]
> 警告には、3つ以上のボタンを追加しないでください。

<a name="Showing_the_Suppress_Button" />

## <a name="showing-the-suppress-button"></a>[非表示] ボタンを表示する

警告の`ShowSuppressButton`プロパティが`true`の場合、アラートには、そのアラートをトリガーしたイベントの後続の発生についてアラートを抑制するために使用できるチェックボックスが表示されます。 次のコードは、非表示のボタンを使用して、フリーフローティングのアラートを表示します。

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

の`alert.SuppressionButton.State`値が`NSCellStateValue.On`の場合、ユーザーは抑制チェックボックスをオンにしています。そうでない場合は、非表示になります。

コードを実行すると、次のように表示されます。

[![](alert-images/alert06.png "抑制ボタンを持つアラート")](alert-images/alert06.png#lightbox)

次に、同じ警告をシートとして使用するコードを示します。

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

このコードが実行されると、次のように表示されます。

[![](alert-images/alert07.png "[非表示] ボタンがシートとして表示されているアラート")](alert-images/alert07.png#lightbox)

<a name="Adding_a_Custom_SubView" />

## <a name="adding-a-custom-subview"></a>カスタムサブビューの追加

アラートには`AccessoryView` 、さらにアラートをカスタマイズしたり、ユーザー入力用の**テキストフィールド**のようなものを追加したりするために使用できるプロパティがあります。 次のコードは、テキスト入力フィールドを追加して、フリーフローティングの警告を作成します。

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

ここで重要なの`var input = new NSTextField (new CGRect (0, 0, 300, 20));`は、アラートを追加する新しい**テキストフィールド**を作成する行です。 `alert.AccessoryView = input;`これは、**テキストフィールド**をアラートに添付し、 `Layout()`メソッドへの呼び出しを行います。これは、新しいサブビューに合わせて警告のサイズを変更するために必要です。

コードを実行すると、次のように表示されます。

[![](alert-images/alert08.png "コードを実行すると、次のように表示されます。")](alert-images/alert08.png#lightbox)

シートと同じ警告を次に示します。

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

このコードを実行すると、次のように表示されます。

[![](alert-images/alert09.png "カスタムビューを含むアラート")](alert-images/alert09.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>Summary

この記事では、Xamarin. Mac アプリケーションでのアラートの使用について詳しく説明しました。 ここでは、さまざまな種類とアラートの使用方法、アラートを作成およびカスタマイズする方法、およびコードC#でアラートを操作する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [MacWindows (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macwindows)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows の操作](~/mac/user-interface/window.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [NSAlert](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSAlert_Class/index.html#//apple_ref/doc/uid/TP40004001)
