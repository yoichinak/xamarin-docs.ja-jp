---
title: "第 6 章の概要です。 ボタンのクリック"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 972d0d76fd55981ebca70acce69533d98c3fc0b5
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-6-button-clicks"></a>第 6 章の概要です。 ボタンのクリック

[ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)は、ユーザーがコマンドを開始できるようにする表示です。 A`Button`テキストによって識別される (とで示したように必要に応じてイメージ[第 13 章、ビットマップ](chapter13.md))。 その結果、`Button`と同じプロパティを多数定義`Label`:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Text/)
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontFamily/)
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/)
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontAttributes/)
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.TextColor/)

`Button` また、3 つのプロパティ定義の境界線の外観を制御するが、これらのプロパティと、相互に依存しないのサポートはプラットフォーム固有。

- [`BorderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderColor/) 型の `Color`
- [`BorderWidth`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderWidth/) 型の `Double`
- [`BorderRadius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/) 型の `Double`

`Button` すべてのプロパティも継承`VisualElement`と`View`など、 `BackgroundColor`、 `HorizontalOptions`、および`VerticalOptions`です。

## <a name="processing-the-click"></a>クリックの処理

`Button`クラスを定義、 [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/)ユーザーがタップしたときに発生するイベント、`Button`です。 `Click`ハンドラーは型`EventHandler`です。 最初の引数は、`Button`オブジェクトのイベントを生成する。 2 番目の引数は、`EventArgs`ない追加情報を提供するオブジェクト。

[ **ButtonLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger)サンプルでは単純な`Clicked`を処理します。

## <a name="sharing-button-clicks"></a>共有ボタンをクリックします。

複数`Button`ビューには、同じ共有できます`Clicked`、ハンドラーが、ハンドラーを決定する必要があるでは一般に`Button`は特定のイベントを担当します。 1 つの方法は、さまざまなを格納する`Button`フィールドとチェックのうち、どれがハンドラーでイベントを発生させるオブジェクトです。

[ **TwoButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons)サンプルは、この手法を示します。 プログラムも設定する方法を示します、 [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/)のプロパティ、`Button`に`false`キーを押すと、`Button`は無効になりました。 無効になっている A`Button`を生成しません、`Clicked`イベント。

## <a name="anonymous-event-handlers"></a>匿名のイベント ハンドラー

定義することは`Clicked`匿名のラムダ関数は、ハンドラーとして、 [ **ButtonLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas)サンプルを示します。 ただし、匿名のハンドラーは、いくつか乱れたリフレクション コードなし共有できません。

## <a name="distinguishing-views-with-ids"></a>Id を持つビューを区別します。

複数`Button`を設定してオブジェクトを識別することも、 [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/)プロパティまたは[ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/)プロパティを`string`です。 このプロパティが定義されている`Element`Xamarin.Forms 内では使用できません。 メソッドは、アプリケーション プログラムによってのみ使用するものでは。

[ **SimplestKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad)サンプルは、テンキーのすべての 10 数字キーに同じイベント ハンドラーを使用し、それらを区別、`StyleId`プロパティ。

[![最も単純なキーパッドのトリプル スクリーン ショット](images/ch06fg04-small.png "電卓")](images/ch06fg04-large.png#lightbox "電卓")

## <a name="saving-transient-data"></a>一時的なデータの保存

多くのアプリケーションでは、プログラムが終了した場合に、データを保存し、プログラムをもう一度起動するときに、そのデータを再読み込みする必要があります。 [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/)クラスは、プログラムの保存し、一時的なデータを復元する際に役立ついくつかのメンバーを定義します。

- [ `Properties` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/)プロパティが使用してディクショナリを`string`キーおよび`object`項目。 ディクショナリの内容は自動的にプログラムの終了前にアプリケーションのローカル ストレージに保存され、プログラムを起動するときに再読み込みします。
- `Application`クラスは、プログラムの標準である、次の 3 つの保護仮想メソッドを定義`App`クラスのオーバーライド: [ `OnStart` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnStart()/)、 [ `OnSleep` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnSleep()/)、および[ `OnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnResume()/). これらを参照してください*アプリケーションのライフ サイクル*イベント。
- [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/)メソッドは、ディクショナリの内容を保存します。

呼び出す必要はありません`SavePropertiesAsync`です。 ディクショナリの内容は自動的にプログラムが終了する前に保存され、プログラムの起動する前に取得します。 プログラムは、プログラムがクラッシュした場合は、データを保存するテストの際に便利です。

役立ちます。

- [`Application.Current`](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Current/)、、現在を返す静的プロパティ`Application`オブジェクトを取得し、使用できる、`Properties`ディクショナリ。

最初の手順では、プログラムが終了するときに永続化する、ページ上のすべての変数を特定します。 これらの変数の変更がすべての場所がわかっている場合だけ追加できますを`Properties`ディクショナリ点に注意します。 ページのコンス トラクターから変数を設定することができます、`Properties`ディクショナリのキーが存在する場合。

大規模なプログラムは、アプリケーション ライフ サイクル イベントを処理する必要があります。 最も重要なは、`OnSleep`メソッドです。 このメソッドの呼び出しでは、プログラムがフォア グラウンドままにすることを示します。 ユーザーが押したおそらく、**ホーム**デバイスで、ボタンまたはすべてのアプリケーションを表示またはシャット ダウンして、電話します。 呼び出し`OnSleep`のみ通知は、プログラムが終了されるまでに受信します。 プログラムがいることを確認するには、この機会を利用する必要があります、`Properties`ディクショナリが最新の状態。

呼び出し`OnResume`いるプログラムは終了していません次の最後の呼び出しを示します`OnSleep`がもう一度実行フォア グラウンドでようになりました。 プログラムは、この営業案件を使用して、(たとえば) インターネット接続を更新する可能性があります。

呼び出し`OnStart`プログラムの起動中に発生します。 このメソッドの呼び出しにアクセスするまで待機する必要はありません、`Properties`ディクショナリ内容削除されているために復元時に、`App`コンス トラクターが呼び出されます。

[ **PersistentKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad)サンプルとよく似ています**SimplestKeypad** 、プログラムを使用する点を除いて、`OnSleep`オーバーライドを現在のキーパッド エントリを保存し、そのデータを復元するページのコンス トラクターです。



## <a name="related-links"></a>関連リンク

- [第 6 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [第 6 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [第 6 章 f# のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
