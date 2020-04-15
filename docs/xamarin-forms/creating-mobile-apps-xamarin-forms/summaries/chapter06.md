---
title: '第 6 章の概要: ボタンのクリック'
description: 'Xamarin.Forms で Mobile Apps を作成する: 第 6 章の概要: ボタンのクリック'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 12c8cdc19f9e6765ca25ade97bcfdbffb7b60381
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "61334714"
---
# <a name="summary-of-chapter-6-button-clicks"></a>第 6 章の概要: ボタンのクリック

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)

[`Button`](xref:Xamarin.Forms.Button) は、ユーザーがコマンドを開始するためのビューです。 `Button` は、テキスト (および必要に応じて、「[第 13 章、ビットマップ](chapter13.md)」で示されているように画像) によって識別されます。 そのため、`Button` では `Label` と同じプロパティの多くが定義されています。

- [`Text`](xref:Xamarin.Forms.Button.Text)
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)

また、`Button` では、境界線の外観を制御する 3 つのプロパティも定義されていますが、これらのプロパティと相互の独立性のサポートはプラットフォーム固有です。

- `Color` 型の [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor)
- `Double` 型の [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth)
- `Double` 型の [`BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius)

`Button` では、`BackgroundColor`、`HorizontalOptions`、`VerticalOptions` など、`VisualElement` および `View` のすべてのプロパティも継承されます。

## <a name="processing-the-click"></a>クリックの処理

`Button` クラスで定義されている [`Clicked`](xref:Xamarin.Forms.Button.Clicked) イベントは、ユーザーが `Button` をタップしたときに発生します。 `Click` ハンドラーは `EventHandler` 型です。 最初の引数は、イベントを発生させる `Button` オブジェクトです。2 番目の引数は、追加情報を提供しない `EventArgs` オブジェクトです。

[**ButtonLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) サンプルでは、簡単な `Clicked` の処理が示されています。

## <a name="sharing-button-clicks"></a>ボタンのクリックの共有

複数の `Button` ビューで同じ `Clicked` ハンドラーを共有できますが、通常、ハンドラーでは特定のイベントに対応する `Button` を特定する必要があります。 1 つの方法は、さまざまな `Button` オブジェクトをフィールドとして格納し、ハンドラーでイベントを発生させているものを調べることです。

[**TwoButtons**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) サンプルでは、この手法が示されています。 そのプログラムでは、`Button` の押下が有効ではなくなったときに、`Button` の [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) プロパティを `false` に設定する方法も示されています。 無効な `Button` では、`Clicked` イベントは生成されません。

## <a name="anonymous-event-handlers"></a>匿名イベント ハンドラー

[**ButtonLambdas**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) サンプルで示されているように、`Clicked` ハンドラーを匿名ラムダ関数として定義できます。 ただし、匿名ハンドラーは、面倒なリフレクション コードを使用しないと共有できません。

## <a name="distinguishing-views-with-ids"></a>ID によるビューの区別

[`StyleId`](xref:Xamarin.Forms.Element.StyleId) プロパティまたは [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) プロパティを `string` に設定することにより、複数の `Button` オブジェクトを区別することもできます。 このプロパティは `Element` で定義されていますが、Xamarin.Forms では使用されていません。 アプリケーション プログラムによる使用のみが想定されています。

[**SimplestKeypad**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) サンプルでは、テンキーの 10 個の数字キーすべてに同じイベント ハンドラーを使用し、それらを `StyleId` プロパティで区別しています。

[![最も簡単なキーパッドのトリプル スクリーンショット](images/ch06fg04-small.png "計算機")](images/ch06fg04-large.png#lightbox "計算機")

## <a name="saving-transient-data"></a>一時的なデータの保存

多くのアプリケーションでは、プログラムが終了されるときにデータを保存し、プログラムが再び開始されるときにそのデータを再度読み込む必要があります。 [`Application`](xref:Xamarin.Forms.Application) クラスでは、プログラムで一時的なデータを保存および復元するのに役立ついくつかのメンバーが定義されています。

- [`Properties`](xref:Xamarin.Forms.Application.Properties) プロパティは、`string` キーと `object` 項目のディクショナリです。 ディクショナリの内容は、プログラムの終了前にアプリケーションのローカル ストレージに自動的に保存され、プログラムの起動時に再度読み込まれます。
- `Application` クラスでは、プログラムの標準の `App` クラスでオーバーライドされる次の 3 つの保護された仮想メソッドが定義されています: [`OnStart`](xref:Xamarin.Forms.Application.OnStart)、[`OnSleep`](xref:Xamarin.Forms.Application.OnSleep)、[`OnResume`](xref:Xamarin.Forms.Application.OnResume)。 これらでは、"*アプリケーション ライフサイクル*" イベントが参照されています。
- [`SavePropertiesAsync`](xref:Xamarin.Forms.Application.SavePropertiesAsync) メソッドでは、ディクショナリの内容が保存されます。

`SavePropertiesAsync` を呼び出す必要はありません。 ディクショナリの内容は、プログラムの終了前に自動的に保存され、プログラムの起動前に取得されます。 プログラムをテストするとき、プログラムがクラッシュした場合にデータを保存するのに便利です。

次のものも便利です。

- [`Application.Current`](xref:Xamarin.Forms.Application.Current) は現在の `Application` オブジェクトを返す静的プロパティで、`Properties` ディクショナリを取得するために使用できます。

最初のステップは、プログラムの終了時に保持する必要がある、ページ上のすべての変数を識別することです。 それらの変数が変更されるすべての場所がわかっている場合は、その時点で `Properties` ディクショナリに追加するだけで済みます。 ページのコンストラクターでは、キーが存在する場合は `Properties` ディクショナリから変数を設定できます。

大規模なプログラムでは、おそらく、アプリケーション ライフサイクル イベントを処理する必要があります。 最も重要なものは、`OnSleep` メソッドです。 このメソッドの呼び出しは、プログラムがフォアグラウンドではなくなったことを示します。 おそらく、ユーザーがデバイスの**ホーム** ボタンを押したか、すべてのアプリケーションを表示したか、または電話をシャットダウンしている可能性があります。 `OnSleep` の呼び出しは、プログラムが終了する前に受け取る唯一の通知です。 プログラムでは、この機会を利用して、`Properties` ディクショナリを最新の状態にする必要があります。

`OnResume` の呼び出しは、プログラムが最後の `OnSleep` の呼び出しの後で終了せず、もう一度フォアグラウンドで実行していることを示します。 プログラムでは、この機会を利用して、(たとえば) インターネット接続を更新することができます。

`OnStart` の呼び出しは、プログラムの起動中に発生します。 このメソッドが呼び出されるまで、`Properties` ディクショナリのアクセスを待つ必要はありません。内容は、`App` コンストラクターが呼び出されたときに既に復元されています。

[**PersistentKeypad**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) サンプルは **SimplestKeypad** と非常によく似ていますが、そのプログラムでは `OnSleep` のオーバーライドを使用して現在のキーパッド入力を保存し、ページ コンストラクターを使用してそのデータを復元している点が異なります。

> [!NOTE]
> プログラムの設定を保存するもう 1 つの方法が、Xamarin.Essentials の [Preferences](~/essentials/preferences.md) クラスで提供されています。

## <a name="related-links"></a>関連リンク

- [第 6 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [第 6 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [第 6 章の F# サンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
- [Xamarin.Forms のボタン](~/xamarin-forms/user-interface/button.md)
