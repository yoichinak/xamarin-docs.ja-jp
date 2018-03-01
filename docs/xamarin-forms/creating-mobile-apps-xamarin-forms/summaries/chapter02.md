---
title: "第 2 章の概要です。 アプリの構造"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 893030170175403c7f7d6885e924e425b4f73c05
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>第 2 章の概要です。 アプリの構造

Xamarin.Forms のアプリケーションで画面上の領域を占有するオブジェクトと呼ばれる*視覚要素*、によってカプセル化、 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)クラス。 ビジュアル要素は、これらのクラスに対応する 3 つのカテゴリに分割できます。

- [ページ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)
- [レイアウト](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [表示](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)

A`Page`から派生した全画面、または画面全体ではほぼを占有します。 ページの子は、多くの場合、 `Layout` visual の子要素を整理するには、派生したものです。 子、`Layout`他は、`Layout`クラスまたは`View`派生型 (とも呼ば*要素*)、テキスト、ビットマップ、スライダー、ボタン、リスト ボックスなどの使い慣れたオブジェクトであります。

この章が注目することにより、アプリケーションを作成する方法を示します、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)、これは、`View`から派生したテキストを表示します。

## <a name="say-hello"></a>挨拶します。

インストールされている、Xamarin プラットフォームとできるソリューションを作成する新しい Xamarin.Forms Visual Studio または Visual Studio for mac [**こんにちは**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello)ソリューションは、一般的なコードをポータブル クラス ライブラリを使用します。 このサンプルでは、変更なしで Visual Studio で作成された Xamarin.Forms ソリューションを示します。 ソリューションは、6 つのプロジェクト (最後のうちの 2 台の現在の Xamarin.Forms ソリューション テンプレートでは作成されません) で構成されます。

- [**こんにちは**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello)、ポータブル クラス ライブラリ (PCL) が、他のプロジェクトで共有
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid)Android 用のアプリケーション プロジェクト
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS)iOS 用のアプリケーション プロジェクト
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP)、ユニバーサル Windows プラットフォーム (Windows 10 および Windows 10 Mobile) 用のアプリケーション プロジェクト
- [**Hello.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Windows)、Windows 8.1 用のアプリケーション プロジェクト
- [**Hello.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.WinPhone)、Windows Phone 8.1 用アプリケーション プロジェクト

これらのアプリケーション プロジェクトのスタートアップ プロジェクトを作成し、しのビルドし、デバイスまたはシミュレーターで、プログラムを実行します。

Xamarin.Forms プログラムの多くでは、アプリケーション プロジェクトを変更しません。 これらは、小さなスタブ プログラムを起動するだけのままに多くの場合。 フォーカスのほとんどは、すべてのアプリケーションに共通のポータブル クラス ライブラリになります。

## <a name="inside-the-files"></a>ファイル内

によって表示されるビジュアル、**こんにちは**プログラムがのコンス トラクターで定義されている、 [ `App` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs)クラスです。 `App` Xamarin.Forms クラスから派生した[ `Application`](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/)です。

**参照**のセクションで、**こんにちは**PCL プロジェクトには、次の Xamarin.Forms アセンブリが含まれています。

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

**参照**5 つのアプリケーション プロジェクトのセクションに含まれる個々 のプラットフォームに適用される追加のアセンブリ。

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

静的な呼び出しを含むアプリケーション プロジェクトの各`Forms.Init`メソッドで、`Xamarin.Forms`名前空間。 これには、Xamarin.Forms ライブラリを初期化します。 別のバージョンの`Forms.Init`プラットフォームごとに定義されています。 このメソッドを呼び出すことができます、次のクラスにあります。

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [ `App`クラス、`OnLaunched`メソッド](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- Windows 8.1: [ `App`クラス、`OnLaunched`メソッド](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/App.xaml.cs#L65)
- Windows Phone 8.1: [ `App`クラス、`OnLaunched`メソッド](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WinPhone/App.xaml.cs#L67)

さらに、各プラットフォームのインスタンスを作成する必要があります、`App`クラス、PCL に位置します。 これは、エラーへの呼び出しで発生`LoadApplication`次のクラス。

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)
- Windows 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/MainPage.xaml.cs)
- Windows Phone 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WindowsPhone/MainPage.xaml.cs)

それ以外の場合、これらのアプリケーション プロジェクトは、通常の「何もしない」のプログラムです。

## <a name="pcl-or-sap"></a>PCL または SAP しますか。

ポータブル クラス ライブラリ (PCL) または共有アセット プロジェクト (SAP) のいずれかで一般的なコードを含む Xamarin.Forms ソリューションを作成することができます。 SAP ソリューションを作成するには、Visual Studio の共有 オプションを選択します。 [ **HelloSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap)ソリューションを変更なしで SAP テンプレートを示しています。

プラットフォームのアプリケーション プロジェクトによって参照されるライブラリ プロジェクト内のすべての共通コード PCL アプローチ バンドルします。 SAP アプローチで、一般的なコードは効果的にプラットフォームのすべてのアプリケーション プロジェクト内が存在し、それらの間で共有されます。

Xamarin.Forms のほとんどの開発者は、PCL アプローチを選択します。 このドキュメントでは、ほとんどのソリューションは、PCL です。 SAP を使用するものが含まれて、 **Sap**プロジェクト名にサフィックス。

Xamarin.Forms のすべてのプラットフォームをサポートするために PCL によって使用される .NET のバージョンは、次のプラットフォームに対応する必要があります。

- .NET Framework 4.5
- Windows 8
- Windows Phone 8.1
- Xamarin.Android
- Xamarin.iOS
- Xamarin.IOS (クラシック)

これは、PC プロファイル 111 と呼ばれます。

SAP アプローチでは共有プロジェクトのコードは、c# プリプロセッサ ディレクティブを使用して、さまざまなプラットフォーム用の別のコードを実行できます (`#if`、#`elif`、および`#endif`) これら定義済みの識別子。

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`
- Windows 8.1: `WINDOWS_APP`
- Windows Phone 8.1: `WINDOWS_PHONE_APP`

PCL に後の「わかりのように、実行時に、で実行しているプラットフォームを指定できます。

## <a name="labels-for-text"></a>テキストのラベル

[ **Greetings** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings)ソリューションは、新しい c# ファイルを追加する方法を示します、 **Greetings**プロジェクト。 このファイルは、という名前のクラスを定義`GreetingsPage`から派生した`ContentPage`です。 このドキュメントでは、ほとんどのプロジェクトが含まれている 1 つ`ContentPage`名前サフィックスの付いたプロジェクトの名前は、派生`Page`追加されます。

`GreetingsPage`コンス トラクターをインスタンス化、 [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)ビューは、テキストを含む Xamarin.Forms ビューです。 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/)プロパティによって表示されるテキストに設定されて、`Label`です。 このプログラムの設定、`Label`を`Content`プロパティ`ContentPage`です。 コンス トラクター、`App`クラスのインスタンスを作成し、`GreetingsPage`設定およびその`MainPage`プロパティです。

テキストは、ページの左上隅に表示されます。 Ios の場合これは、ページのステータス バーに重なっていることを意味します。 この問題を解決するいくつかあります。

### <a name="solution-1-include-padding-on-the-page"></a>1 のソリューションです。 ページのスペースを含める

設定、 [ `Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/)ページのプロパティです。 `Padding` 型は[ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/)、4 つのプロパティを持つ構造体。

- [`Left`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Left/)
- [`Top`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Top/)
- [`Right`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Right/)
- [`Bottom`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Bottom/)

`Padding` コンテンツが除外されているページ内の領域を定義します。 これにより、 `Label` iOS ステータス バーが上書きされないようにします。

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>2 のソリューションです。 IOS (SAP のみ) 用だけ埋め込みを含む

C# プリプロセッサ ディレクティブを使用して、SAP を使用して iOS にのみ '埋め込み' プロパティを設定します。 これに示されている、 [ **GreetingsSap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap)ソリューションです。

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>3 のソリューションです。 IOS (PCL または SAP) 用だけ埋め込みを含む

Xamarin.Forms は、ブックの使用のバージョンで、`Padding`を使用して PCL または SAP で iOS に固有のプロパティを選択することができます、 [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/)または[ `Device.OnPlatform<T>` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform%7BT%7D/p/T/T/T/)静的メソッドです。 これらのメソッドは使用されなくなりました

`Device.OnPlatform`メソッドを使用してプラットフォーム固有のコードを実行するか、プラットフォーム固有の値を選択します。 ように内部的には、使用、 [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/)静的な読み取り専用プロパティのメンバーを返します、 [ `TargetPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/)列挙します。

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/)
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/)
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/) Windows 8.1、Windows Phone 8.1、およびすべての UWP デバイス。
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/)、以前に Windows Phone 8.0 の識別に使用されるが、現在使用されていません。
- [`Other`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Other/) 使用されません。

`Device.OnPlatform`メソッド、`Device.OS`プロパティ、および`TargetPlatform`列挙型はすべての非推奨ようになりました。 代わりに、使用、 [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/)プロパティと比較、`string`次の静的フィールドの値を返します。

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/)、[iOS] の文字列 
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/)、文字列"Android"
- [`UWP`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/)、文字列"UWP"、Windows ランタイムのプラットフォームを参照します。
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Windows/)、(Windows 8.1 および Windows Phone 8.1)、Windows ランタイム用には、"Windows"の文字列
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/)、Windows Phone 8.0 の場合に、"WinPhone"文字列 

[ `Device.Idiom` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.Idiom/)静的な読み取り専用プロパティが関連付けられています。 メンバーが返されます、 [ `TargetIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/)、これらのメンバーを持ちます。

- [`Desktop`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Desktop/)
- [`Tablet`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Tablet/)
- [`Phone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Phone/)
- [`Unsupported`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Unsupported/) 使用されません。

IOS および Android、間カットオフ`Tablet`と`Phone`600 ユニットの縦幅がします。 Windows プラットフォームの`Desktop`UWP アプリケーションが Windows 10 で実行されていることを示します`Tablet`Windows 8.1 のプログラムと`Phone`UWP アプリケーションが Windows 10 または Windows Phone 8.1 アプリケーションで実行されていることを示します。

## <a name="solution-3a-set-margin-on-the-label"></a>ソリューション 3a です。 ラベルに余白を設定

[ `Margin` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) 、ブックに含まれるプロパティは遅すぎますに導入されましたがの型も`Thickness`に設定して、`Label`外の領域の計算に含まれているビューを定義する、ビューのレイアウトです。

`Padding`プロパティで使用できるだけ[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)と[ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)派生します。 `Margin`プロパティは、すべての使用[ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)派生します。

## <a name="solution-4-center-the-label-within-the-page"></a>4 のソリューションです。 センター ページ内のラベル

中央に配置できます、`Label`内で、 `Page` (またはその他の 8 つの場所のいずれかに格納) を設定して、 [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/)と[ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/)のプロパティ、 `Label`型の値に[ `LayoutOptions`](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/)です。 `LayoutOptions`構造体は 2 つのプロパティを定義します。

- [ `Alignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/)型のプロパティ[ `LayoutAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/)、4 つのメンバーを持つ列挙: [ `Start` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/)、つまり左端または上端に応じて、印刷の向き、 [ `Center` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/)、 [ `End` ](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/)、つまり、右端または下端、印刷の向きに応じてと[ `Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/)です。

- [ `Expands` ](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/)型のプロパティ`bool`です。

通常、これらのプロパティは直接使用されません。 代わりに、これら 2 つのプロパティの組み合わせは、型の 8 静的な読み取り専用のプロパティによって提供される`LayoutOptions`:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

`HorizontalOptions` および`VerticalOptions`Xamarin.Forms レイアウト内で最も重要なプロパティで詳細に説明した[**第 4 章します。スタックのスクロール**](chapter04.md)です。

ここでは、結果を`HorizontalOptions`と`VerticalOptions`プロパティの`Label`の両方に設定`LayoutOptions.Center`:

[![Greetings プログラムの 3 倍のスクリーン ショット](images/ch02fg05-small.png "水平方向および垂直方向に中央揃えのラベル付け")](images/ch02fg05-large.png "水平方向および垂直方向に中央揃えのラベル付け")

## <a name="solution-5-center-the-text-within-the-label"></a>5 のソリューションです。 ラベル内でテキストを中央揃え

テキストを中央揃え (したり、ページの他の 8 つの場所に配置) を設定して、 [ `HorizontalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/)と[ `VerticalTextAlignment` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/)プロパティの`Label`のメンバーに[ `TextAlignment` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/)列挙します。

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/)、意味の左または上 (に応じて向き)
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.End/)、つまり右端または下端 (に応じて向き)

これら 2 つのプロパティがでのみ定義されている`Label`であるのに対し、`HorizontalAlignment`と`VerticalAlignment`によってプロパティが定義されている`View`すべてによって継承および`View`派生します。 ビジュアルの結果と同様に見えるかもしれませんが、次のチャプターに示すよう非常に異なるです。



## <a name="related-links"></a>関連リンク

- [第 2 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [第 2 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [第 2 章 f# のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Xamarin.Forms の概要](~/xamarin-forms/get-started/index.md)
