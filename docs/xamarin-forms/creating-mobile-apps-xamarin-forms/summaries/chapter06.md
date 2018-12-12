---
title: 第 6 章の概要です。 ボタンのクリック
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 6 章の概要。 ボタンのクリック'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 12c8cdc19f9e6765ca25ade97bcfdbffb7b60381
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53057601"
---
# <a name="summary-of-chapter-6-button-clicks"></a>第 6 章の概要です。 ボタンのクリック

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)

[ `Button` ](xref:Xamarin.Forms.Button)コマンドを開始するユーザーのビューです。 A`Button`テキストによって識別されます (とで示した必要に応じてイメージ[第 13 章、ビットマップ](chapter13.md))。 その結果、`Button`と同じプロパティを多数定義しています`Label`:

- [`Text`](xref:Xamarin.Forms.Button.Text)
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)

`Button` 3 つのプロパティを定義も、境界線の外観が決定されますが、これらのプロパティとその相互独立性のサポートはプラットフォーム固有。

- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) 型の `Color`
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) 型の `Double`
- [`BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius) 型の `Double`

`Button` すべてのプロパティも継承`VisualElement`と`View`など、 `BackgroundColor`、 `HorizontalOptions`、および`VerticalOptions`します。

## <a name="processing-the-click"></a>クリックの処理

`Button`クラスを定義、 [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) 、ユーザーがタップしたときに発生するイベント、`Button`します。 `Click`ハンドラーが型`EventHandler`します。 最初の引数は、`Button`イベントを生成するオブジェクトは、2 番目の引数は、`EventArgs`しない追加情報を提供するオブジェクト。

[ **ButtonLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger)単純なサンプル`Clicked`を処理します。

## <a name="sharing-button-clicks"></a>クリックしたボタンの共有

複数`Button`ビューには、同じ共有できる`Clicked`判断するために必要な一般的に、ハンドラーがハンドラー`Button`特定のイベントを担当します。 さまざまなを格納する 1 つの方法も`Button`フィールドとチェックのどれがハンドラーでイベントを発生させるオブジェクト。

[ **TwoButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons)サンプルは、この手法を示します。 プログラムも設定する方法を示します、 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled)のプロパティを`Button`に`false`キーを押すと、`Button`が無効になっています。 無効になっている`Button`を生成しません、`Clicked`イベント。

## <a name="anonymous-event-handlers"></a>匿名のイベント ハンドラー

定義することは`Clicked`匿名のラムダ関数は、ハンドラーとして、 [ **ButtonLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas)サンプルを示します。 ただし、匿名のハンドラーは、乱雑なリフレクション コードなしに共有ことはできません。

## <a name="distinguishing-views-with-ids"></a>Id を持つ特徴的なビュー

複数`Button`オブジェクトを設定しても区別できる、 [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId)プロパティまたは[ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId)プロパティを`string`します。 このプロパティは`Element`Xamarin.Forms 内では使用できません。 アプリケーション プログラムでのみ使用されるものでは。

[ **SimplestKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad)サンプルは、テンキーのすべての 10 の数字キーを同じイベント ハンドラーを使用しでそれらを識別するため、`StyleId`プロパティ。

[![最も簡単なキーパッドの 3 倍になるスクリーン ショット](images/ch06fg04-small.png "電卓")](images/ch06fg04-large.png#lightbox "計算ツール")

## <a name="saving-transient-data"></a>一時的なデータを保存しています

多くのアプリケーションでは、プログラムが終了したときにデータを保存して、プログラムをもう一度起動するときに、そのデータを再読み込みする必要があります。 [ `Application` ](xref:Xamarin.Forms.Application)クラスは、プログラムを保存し、一時的なデータの復元に役立ついくつかのメンバーを定義します。

- [ `Properties` ](xref:Xamarin.Forms.Application.Properties)プロパティが使用してディクショナリを`string`キーと`object`項目。 ディクショナリの内容は自動的にプログラムの終了前にアプリケーションのローカル ストレージに保存され、プログラムを起動するときに再度読み込まれます。
- `Application`クラスは、プログラムの標準的なこと、3 つの保護された仮想メソッドを定義します`App`オーバーライド: [ `OnStart` ](xref:Xamarin.Forms.Application.OnStart)、 [ `OnSleep` ](xref:Xamarin.Forms.Application.OnSleep)、および[ `OnResume` ](xref:Xamarin.Forms.Application.OnResume). これらを参照してください*アプリケーションのライフ サイクル*イベント。
- [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync)メソッドは、ディクショナリの内容を保存します。

呼び出す必要はありません`SavePropertiesAsync`します。 ディクショナリの内容は自動的にプログラムの終了前に保存され、プログラムの起動する前に取得します。 プログラムがクラッシュした場合は、データを保存するプログラムがテスト中に便利です。

役立ちます。

- [`Application.Current`](xref:Xamarin.Forms.Application.Current)、、現在を返してを静的プロパティ`Application`オブジェクトを取得し、使用できる、`Properties`ディクショナリ。

最初の手順は、プログラムの終了時に保持するページのすべての変数を識別するためにです。 これらの変数を変更するすべての場所がわかっている場合は単に追加する、`Properties`その時点でのディクショナリ。 ページのコンス トラクターから変数を設定することができます、`Properties`ディクショナリ キーが存在する場合。

大規模なプログラムは、アプリケーション ライフ サイクル イベントを処理する必要があります。 最も重要なは、`OnSleep`メソッド。 このメソッドの呼び出しでは、プログラムがフォア グラウンドを離れたことを示します。 ユーザーが押したおそらく、**ホーム**ボタン デバイスをすべてのアプリケーションを表示またはシャット ダウンして、電話します。 呼び出し`OnSleep`only の通知は、それが終了する前にプログラムを受信します。 いることを確認するには、この機会を利用プログラム、`Properties`ディクショナリが最新の状態。

呼び出し`OnResume`プログラムが終了していないこと、最後の呼び出しを次に示します`OnSleep`がフォア グラウンドでもう一度実行されています。 プログラムは、この機会を使用して、(たとえば) インターネット接続を更新する可能性があります。

呼び出し`OnStart`プログラムの起動中に発生します。 このメソッドの呼び出しにアクセスするまで待機する必要はありません、`Properties`ディクショナリ内容は既にいるため、復元するときに、`App`コンス トラクターが呼び出されます。

[ **PersistentKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad)サンプルはほぼ**SimplestKeypad**プログラムを使用する点を除いて、`OnSleep`キーパッドの現在のエントリを保存するオーバーライドとそのデータを復元するページのコンス トラクターです。

> [!NOTE]
> プログラムの設定を保存する方法もありますが、Xamarin.Essentials によって提供される[設定](~/essentials/preferences.md)クラス。

## <a name="related-links"></a>関連リンク

- [第 6 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [第 6 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [第 6 章F#サンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
- [Xamarin.Forms のボタン](~/xamarin-forms/user-interface/button.md)
