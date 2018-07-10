---
title: 第 2 章の概要です。 アプリの詳細
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 2 章の概要。 アプリの詳細'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 94c575bdfdc2325def00de58381f9bc295d953b9
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935123"
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>第 2 章の概要です。 アプリの詳細

Xamarin.Forms アプリケーションで画面上の空き領域オブジェクトと呼ばれる*ビジュアル要素*、によってカプセル化された、 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)クラス。 ビジュアル要素は、これらのクラスに対応する 3 つのカテゴリに分割できます。

- [ページ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)
- [レイアウト](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [[表示]](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)

A`Page`派生物が画面全体、または画面全体ではほぼを占有します。 ページの子の場合は、`Layout`子ビジュアル要素を整理する派生クラス。 子、`Layout`他は、`Layout`クラスまたは`View`派生クラス (多くの場合と呼ばれる*要素*)、テキスト、ビットマップ、スライダー、ボタン、リスト ボックスなどの一般的なオブジェクトであります。

この章に重点を置いた、アプリケーションを作成する方法を示します、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)、これは、`View`から派生したテキストが表示されます。

## <a name="say-hello"></a>開始します。

インストールされている、Xamarin プラットフォームで作成新しい Xamarin.Forms ソリューションを Visual Studio または Visual Studio for mac。 [**こんにちは**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello)ソリューションでは、共通のコードにポータブル クラス ライブラリを使用します。 変更なしで Visual Studio で作成された Xamarin.Forms ソリューションを示します。 ソリューションは、6 つのプロジェクト (最後の 2 つの現在の Xamarin.Forms ソリューション テンプレートでは作成されません) で構成されます。

- [**こんにちは**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello)、ポータブル クラス ライブラリ (PCL) が、その他のプロジェクトで共有
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid)、android アプリケーション プロジェクト
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS)iOS 用のアプリケーション プロジェクト
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP)、ユニバーサル Windows プラットフォーム (Windows 10 および Windows 10 Mobile) のアプリケーション プロジェクト
- [**Hello.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Windows)、Windows 8.1 のアプリケーション プロジェクト
- [**Hello.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.WinPhone)、Windows Phone 8.1 用アプリケーション プロジェクト

これらのアプリケーション プロジェクトのいずれかのスタートアップ プロジェクトを作成しのビルド/デバイスまたはシミュレーターで、プログラムを実行します。

Xamarin.Forms プログラムの多くでは、アプリケーション プロジェクトを変更しません。 これら多くの場合、プログラムを起動するだけの小さいスタブを残ります。 フォーカスのほとんどは、すべてのアプリケーションに共通するポータブル クラス ライブラリになります。

## <a name="inside-the-files"></a>ファイルの内部

によって表示されるビジュアル、**こんにちは**プログラムが、コンス トラクターで定義されている、 [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs)クラス。 `App` Xamarin.Forms クラスから派生した[ `Application`](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/)します。

**参照**のセクション、**こんにちは**PCL プロジェクトには、次の Xamarin.Forms アセンブリが含まれています。

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

**参照**セクションでは 5 つのアプリケーション プロジェクトに含まれる個別のプラットフォームに適用される追加アセンブリ。

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

静的な呼び出しを含むアプリケーション プロジェクトの各`Forms.Init`メソッドで、`Xamarin.Forms`名前空間。 これには、Xamarin.Forms ライブラリを初期化します。 別のバージョンの`Forms.Init`プラットフォームごとに定義されます。 このメソッドの呼び出しは、次のクラスを参照してください。

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [ `App`クラス、`OnLaunched`メソッド](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- Windows 8.1: [ `App`クラス、`OnLaunched`メソッド](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/App.xaml.cs#L65)
- Windows Phone 8.1: [ `App`クラス、`OnLaunched`メソッド](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WinPhone/App.xaml.cs#L67)

さらに、各プラットフォームのインスタンスを作成する必要があります、`App`クラス、PCL に位置します。 これは、問題への呼び出しで発生`LoadApplication`次のクラス。

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)
- Windows 8.1： [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/MainPage.xaml.cs)
- Windows Phone 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WindowsPhone/MainPage.xaml.cs)

それ以外の場合、これらのアプリケーション プロジェクトは、通常の「何」のプログラムです。

## <a name="pcl-or-sap"></a>PCL または SAP でしょうか。

ポータブル クラス ライブラリ (PCL) または共有資産プロジェクト (SAP) のいずれかの一般的なコードで Xamarin.Forms ソリューションを作成することになります。 SAP ソリューションを作成するには、Visual Studio で共有オプションを選択します。 [ **HelloSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap)ソリューションは変更なしでの SAP テンプレートを示します。

すべての一般的なプラットフォームのアプリケーション プロジェクトによって参照されるライブラリ プロジェクトでコードの PCL アプローチ バンドル。 SAP アプローチでは、共通のコードは効果的にプラットフォームのすべてのアプリケーション プロジェクト内に存在して、それらの間で共有されます。

Xamarin.Forms のほとんどの開発者では、PCL のアプローチを選択します。 この本では、ソリューションのほとんどは、PCL です。 SAP を使用するものが含まれて、 **Sap**プロジェクト名にサフィックス。

すべての Xamarin.Forms プラットフォームをサポートするために、PCL で使用される .NET のバージョンが、次のプラットフォームに対応する必要があります。

- .NET Framework 4.5
- Windows 8
- Windows Phone 8.1
- Xamarin.Android
- Xamarin.iOS
- Xamarin.IOS (クラシック)

これは、PC プロファイル 111 と呼ばれます。

SAP アプローチでは、共有プロジェクト内のコードは、c# プリプロセッサ ディレクティブを使用して、さまざまなプラットフォームのさまざまなコードを実行できます (`#if`、#`elif`、および`#endif`) これらの定義済みの識別子。

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`
- Windows 8.1： `WINDOWS_APP`
- Windows Phone 8.1: `WINDOWS_PHONE_APP`

PCL では、この章では後でわかります、実行時実行しているプラットフォームを判断できます。

## <a name="labels-for-text"></a>テキストのラベル

[ **Greetings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings)ソリューションは、新しい c# ファイルを追加する方法を示します、 **Greetings**プロジェクト。 このファイルは、という名前のクラスを定義します。`GreetingsPage`から派生した`ContentPage`します。 この本では、ほとんどのプロジェクトは、1 つを含めることが`ContentPage`名前サフィックスを持つプロジェクトの名前は、派生物`Page`追加されます。

`GreetingsPage`コンス トラクターをインスタンス化、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)ビューで、テキストを表示する Xamarin.Forms のビューです。 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/)プロパティによって表示されるテキストに設定されて、`Label`します。 このプログラムの設定、`Label`を`Content`プロパティの`ContentPage`します。 コンス トラクター、`App`クラスのインスタンスを作成し、`GreetingsPage`に設定とその`MainPage`プロパティ。

テキストは、ページの左上隅に表示されます。 Ios では、これは、ページのステータス バーに重なっていることを意味します。 この問題を解決するいくつかあります。

### <a name="solution-1-include-padding-on-the-page"></a>解決策 1。 ページのスペースを含める

設定、 [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/)ページのプロパティ。 `Padding` 種類は[ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)、4 つのプロパティを持つ構造体。

- [`Left`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Left/)
- [`Top`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Top/)
- [`Right`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Right/)
- [`Bottom`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Bottom/)

`Padding` コンテンツが除外されているページ内の領域を定義します。 これにより、 `Label` iOS のステータス バーが上書きされないようにします。

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>解決策 2。 IOS (SAP のみ) 用だけ埋め込みを含む

C# プリプロセッサ ディレクティブで SAP を使用して iOS でのみ、'余白' プロパティを設定します。 これは、方法については、 [ **GreetingsSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap)ソリューション。

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>解決策 3。 IOS (PCL や SAP) 用だけ埋め込みを含む

Xamarin.Forms は、ブックの使用のバージョンで、`Padding`を使って PCL または SAP のいずれかで iOS に固有のプロパティを選択することができます、 [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/)または[ `Device.OnPlatform<T>` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform%7BT%7D/p/T/T/T/)静的メソッド。 これらのメソッドが非推奨となりました

`Device.OnPlatform`メソッドを使用してプラットフォーム固有のコードを実行するか、プラットフォーム固有の値を選択します。 ように内部的には、使用、 [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/)静的読み取り専用プロパティのメンバーを返します、 [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/)列挙体。

- [`iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)
- [`Android`](xref:Xamarin.Forms.TargetPlatform.Android)
- [`Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) Windows 8.1、Windows Phone 8.1、およびすべての UWP デバイス。
- [`WinPhone`](xref:Xamarin.Forms.TargetPlatform.WinPhone)、以前は Windows Phone 8.0 を識別するために使用されるが、現在未使用
- [`Other`](xref:Xamarin.Forms.TargetPlatform.Other) 使用されません。

`Device.OnPlatform`メソッド、`Device.OS`プロパティ、および`TargetPlatform`列挙型はすべての非推奨のようになりました。 代わりに、使用、 [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/)プロパティと比較、`string`以下の静的フィールドの値を返します。

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/)、文字列"iOS"
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/)、文字列"Android"
- [`UWP`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/)、文字列"UWP"、Windows ランタイムのプラットフォームを参照します。
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Windows/)、文字列"Windows"for Windows Runtime (Windows 8.1 および Windows Phone 8.1)
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/)、Windows Phone 8.0"WinPhone"文字列

[ `Device.Idiom` ](xref:Xamarin.Forms.Device.Idiom)静的読み取り専用プロパティが関連付けられています。 メンバーが返されます、 [ `TargetIdiom` ](xref:Xamarin.Forms.TargetIdiom)、これらのメンバーを持ちます。

- [`Desktop`](xref:Xamarin.Forms.TargetIdiom.Desktop)
- [`Tablet`](xref:Xamarin.Forms.TargetIdiom.Tablet)
- [`Phone`](xref:Xamarin.Forms.TargetIdiom.Phone)
- [`Unsupported`](xref:Xamarin.Forms.TargetIdiom.Unsupported) 使用されません。

IOS と Android の間のカットオフの`Tablet`と`Phone`は 600 ユニットの縦幅。 Windows プラットフォームの`Desktop`UWP アプリケーションが Windows 10 で実行されていることを示します`Tablet`は、Windows 8.1 のプログラムと`Phone`UWP アプリケーションが Windows 10 または Windows Phone 8.1 アプリケーションで実行されていることを示します。

## <a name="solution-3a-set-margin-on-the-label"></a>ソリューションの 3 a。 ラベルに余白を設定

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) 、ブックに含まれるプロパティが遅すぎるに導入されたが、型のも`Thickness`に設定して、`Label`外の領域の計算に含まれているビューを定義する、ビューのレイアウト。

`Padding`プロパティのみで提供されて[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)と[ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)派生クラス。 `Margin`プロパティがすべてで使用可能な[ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)派生クラス。

## <a name="solution-4-center-the-label-within-the-page"></a>解決策 4。 Center ページ内のラベル

中央に配置できます、`Label`内、 `Page` (またはその他の 8 つの場所のいずれかに配置) を設定して、 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)と[ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)のプロパティ、 `Label`型の値に[ `LayoutOptions`](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)します。 `LayoutOptions`構造体が 2 つのプロパティを定義します。

- [ `Alignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/)型のプロパティ[ `LayoutAlignment` ](xref:Xamarin.Forms.LayoutAlignment)、4 つのメンバーを持つ列挙: [ `Start` ](xref:Xamarin.Forms.LayoutAlignment.Start)、つまり左端または上端に応じて、印刷の向き、 [ `Center` ](xref:Xamarin.Forms.LayoutAlignment.Center)、 [ `End` ](xref:Xamarin.Forms.LayoutAlignment.End)、つまり右または下方向に応じて、 [ `Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill)します。

- [ `Expands` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/)型のプロパティ`bool`します。

通常、これらのプロパティが直接使用されることはありません。 型の 8 つ静的読み取り専用のプロパティでこれら 2 つのプロパティの組み合わせを指定する代わりに、 `LayoutOptions`:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

`HorizontalOptions` `VerticalOptions` Xamarin.Forms のレイアウトで最も重要なプロパティは、詳細については説明[**第 4 章です。スタックをスクロール**](chapter04.md)します。

結果を次に示します、`HorizontalOptions`と`VerticalOptions`プロパティの`Label`両方とも設定`LayoutOptions.Center`:

[![Greetings プログラムのスクリーン ショットをトリプル](images/ch02fg05-small.png "水平および垂直方向に中央揃えのラベル")](images/ch02fg05-large.png#lightbox "水平および垂直方向に中央揃えのラベル")

## <a name="solution-5-center-the-text-within-the-label"></a>5 のソリューションです。 ラベル内でテキストを中央揃え

テキストを中央揃え (したり、ページの他の 8 つの場所に配置) を設定して、 [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/)と[ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/)プロパティの`Label`のメンバーに[ `TextAlignment` ](xref:Xamarin.Forms.TextAlignment)列挙体。

- [`Start`](xref:Xamarin.Forms.TextAlignment.Start)、意味の左または上 (向き) によって異なります
- [`Center`](xref:Xamarin.Forms.TextAlignment.Center)
- [`End`](xref:Xamarin.Forms.TextAlignment.End)、つまり右または下 (に応じて向き)

これら 2 つのプロパティがでのみ定義されている`Label`であるのに対し、`HorizontalAlignment`と`VerticalAlignment`によってプロパティが定義されている`View`すべてによって継承されて`View`派生クラス。 ビジュアルの結果と同様に思えますが、[次へ] の章で示すように非常に異なるいます。



## <a name="related-links"></a>関連リンク

- [第 2 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)
- [第 2 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [第 2 章 f# のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Xamarin.Forms の概要](~/xamarin-forms/get-started/index.md)
