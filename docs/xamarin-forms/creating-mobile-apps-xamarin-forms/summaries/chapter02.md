---
title: 第 2 章の概要です。 アプリの詳細
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 2 章の概要。 アプリの詳細'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
ms.openlocfilehash: f900cb1532ba4415127c95b07e777881e1d74994
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724995"
---
# <a name="summary-of-chapter-2-anatomy-of-an-app"></a>第 2 章の概要です。 アプリの詳細

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)

> [!NOTE]
> このページに関する注意事項は、この本で説明されている内容が Xamarin.Forms が異なっている領域を示しています。

Xamarin のフォームアプリケーションでは、画面上の領域を占有するオブジェクトは、 [`VisualElement`](xref:Xamarin.Forms.VisualElement)クラスによってカプセル化された*ビジュアル要素*と呼ばれます。 ビジュアル要素は、これらのクラスに対応する 3 つのカテゴリに分割できます。

- [ページ](xref:Xamarin.Forms.Page)
- [レイアウト](xref:Xamarin.Forms.Layout)
- [表示](xref:Xamarin.Forms.View)

`Page` の派生物は、画面全体、または画面全体を占めます。 多くの場合、ページの子は、子ビジュアル要素を整理するための `Layout` 派生しています。 `Layout` の子は、他の `Layout` クラスまたは `View` 派生物 (多くの場合、*要素*と呼ばれます) です。これは、テキスト、ビットマップ、スライダー、ボタン、リストボックスなどの使い慣れたオブジェクトです。

この章では、 [`Label`](xref:Xamarin.Forms.Label)に焦点を当てることによってアプリケーションを作成する方法を示します。これは、テキストを表示する派生 `View` です。

## <a name="say-hello"></a>開始します。

インストールされている、Xamarin プラットフォームで作成新しい Xamarin.Forms ソリューションを Visual Studio または Visual Studio for mac。 [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello)ソリューションでは、共通コードにポータブルクラスライブラリを使用します。

> [!NOTE]
> ポータブル クラス ライブラリが .NET Standard ライブラリに置き換えられました。 .NET standard ライブラリを使用するブックからのすべてのサンプル コードが変換されました。

このサンプルでは、変更なしで Visual Studio で作成された Xamarin.Forms ソリューションを示します。 このソリューションは、次の4つのプロジェクトで構成されています。

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), 他のプロジェクトで共有されているポータブルクラスライブラリ (PCL)
- [**こんにちは**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid)、Android 用アプリケーションプロジェクトの id
- IOS 用のアプリケーションプロジェクトである[**Hello. ios**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS)
- ユニバーサル Windows プラットフォーム用のアプリケーションプロジェクトである[**HELLO UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP)(windows 10 および Windows 10 Mobile)

> [!NOTE]
> Xamarin.Forms Windows 8.1、Windows Phone 8.1、または Windows 10 Mobile のをサポートしていませんが、Xamarin.Forms アプリケーションは、Windows 10 デスクトップで実行しないでください。

これらのアプリケーション プロジェクトのいずれかのスタートアップ プロジェクトを作成しのビルド/デバイスまたはシミュレーターで、プログラムを実行します。

Xamarin.Forms プログラムの多くでは、アプリケーション プロジェクトを変更しません。 これら多くの場合、プログラムを起動するだけの小さいスタブを残ります。 フォーカスのほとんどは、すべてのアプリケーションに共通のライブラリになります。

## <a name="inside-the-files"></a>ファイルの内部

**Hello**プログラムによって表示されるビジュアルは、 [`App`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs)クラスのコンストラクターで定義されます。 `App` は、Xamarin. Forms クラス[`Application`](xref:Xamarin.Forms.Application)から派生します。

> [!NOTE]
> Xamarin.Forms 用の Visual Studio のソリューション テンプレートは、XAML ファイルを使用して、ページを作成します。 この書籍では、[第7章](chapter07.md)まで XAML は説明されていません。

**Hello** PCL プロジェクトの**参照**セクションには、次の Xamarin. Forms アセンブリが含まれています。

- **Xamarin. Forms. Core**
- **Xamarin. .Xaml**
- **Xamarin. Forms. Platform**

5つのアプリケーションプロジェクトの**参照**セクションには、個々のプラットフォームに適用される追加のアセンブリが含まれています。

- **Xamarin. Platform. Android**
- **Xamarin. Platform.string. iOS**
- **Xamarin... Platform. UWP**
- **Xamarin. Platform.object**
- **Xamarin.....................**
- **Xamarin. Platform.object...-Phone**

> [!NOTE]
> これらのプロジェクトの**参照**セクションには、アセンブリの一覧が表示されなくなりました。 代わりに、プロジェクトファイルには、 **PackageReference** NuGet パッケージを参照するタグが含まれています。 Visual Studio の **[参照]** セクションには、Xamarin. forms アセンブリではなく、 **xamarin.** forms パッケージが一覧表示されます。

各アプリケーションプロジェクトには、`Xamarin.Forms` 名前空間の静的 `Forms.Init` メソッドの呼び出しが含まれています。 これには、Xamarin.Forms ライブラリを初期化します。 プラットフォームごとに異なるバージョンの `Forms.Init` が定義されています。 このメソッドの呼び出しは、次のクラスを参照してください。

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`App` クラス、`OnLaunched` メソッド](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)

さらに、各プラットフォームでは、共有ライブラリ内の `App` クラスの場所をインスタンス化する必要があります。 これは、次のクラスの `LoadApplication` を呼び出すと発生します。

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)

それ以外の場合、これらのアプリケーション プロジェクトは、通常の「何」のプログラムです。

## <a name="pcl-or-sap"></a>PCL または SAP でしょうか。

ポータブル クラス ライブラリ (PCL) または共有資産プロジェクト (SAP) のいずれかの一般的なコードで Xamarin.Forms ソリューションを作成することになります。 SAP ソリューションを作成するには、Visual Studio で共有オプションを選択します。 [**HelloSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap)ソリューションは SAP テンプレートを変更せずにデモンストレーションします。

> [!NOTE]
> ポータブル クラス ライブラリは .NET Standard ライブラリによって置き換えられました。 .NET standard ライブラリを使用するブックからのすべてのサンプル コードが変換されました。 それ以外の場合、PCL および .NET Standard ライブラリは、概念的には非常に似ています。

プラットフォームのアプリケーション プロジェクトによって参照されるライブラリ プロジェクト内のすべての一般的なコード ライブラリ アプローチ バンドルします。 SAP アプローチでは、共通のコードは効果的にプラットフォームのすべてのアプリケーション プロジェクト内に存在して、それらの間で共有されます。

Xamarin.Forms のほとんどの開発者では、ライブラリのアプローチを選択します。 この本では、ソリューションの多くは、ライブラリを使用します。 SAP を使用するものには、プロジェクト名に**sap**サフィックスが含まれています。

SAP アプローチでは、共有プロジェクトのコードは、次の定義済みの識別子を使用しC#て、プリプロセッサディレクティブ (`#if`、#`elif`、および `#endif`) を使用して、さまざまなプラットフォームに対して異なるコードを実行できます。

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`

共有ライブラリでは、この章では後でわかります、実行時実行しているプラットフォームを判断できます。

## <a name="labels-for-text"></a>テキストのラベル

[**グリーティング**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings)ソリューションでは、新しいC#ファイルを**グリーティング**プロジェクトに追加する方法を示します。 このファイルは、`ContentPage`から派生する `GreetingsPage` という名前のクラスを定義します。 この書籍では、ほとんどのプロジェクトに、名前がサフィックス `Page` 追加されたプロジェクトの名前を持つ1つの `ContentPage` 派生が含まれています。

`GreetingsPage` コンストラクターは[`Label`](xref:Xamarin.Forms.Label)ビューをインスタンス化します。これは、テキストを表示する Xamarin. Forms ビューです。 [`Text`](xref:Xamarin.Forms.Label.Text)プロパティは、`Label`によって表示されるテキストに設定されます。 このプログラムにより、`Label` が `ContentPage`の `Content` プロパティに設定されます。 `App` クラスのコンストラクターは、`GreetingsPage` をインスタンス化し、その `MainPage` プロパティに設定します。

テキストは、ページの左上隅に表示されます。 Ios では、これは、ページのステータス バーに重なっていることを意味します。 この問題を解決するいくつかあります。

### <a name="solution-1-include-padding-on-the-page"></a>解決策 1。 ページのスペースを含める

ページの[`Padding`](xref:Xamarin.Forms.Page.Padding)プロパティを設定します。 `Padding` は[`Thickness`](xref:Xamarin.Forms.Thickness)型で、次の4つのプロパティを持つ構造体です。

- [`Left`](xref:Xamarin.Forms.Thickness.Left)
- [`Top`](xref:Xamarin.Forms.Thickness.Top)
- [`Right`](xref:Xamarin.Forms.Thickness.Right)
- [`Bottom`](xref:Xamarin.Forms.Thickness.Bottom)

`Padding` は、コンテンツが除外されるページ内の領域を定義します。 これにより、`Label` が iOS ステータスバーを上書きしないようにすることができます。

### <a name="solution-2-include-padding-just-for-ios-sap-only"></a>解決策 2。 IOS (SAP のみ) 用だけ埋め込みを含む

C# プリプロセッサ ディレクティブで SAP を使用して iOS でのみ、'余白' プロパティを設定します。 これは、 [**GreetingsSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap)ソリューションで示されています。

### <a name="solution-3-include-padding-just-for-ios-pcl-or-sap"></a>解決策 3。 IOS (PCL や SAP) 用だけ埋め込みを含む

本に使用される Xamarin. 形式のバージョンでは、 [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action))または[`Device.OnPlatform<T>`](xref:Xamarin.Forms.Device.OnPlatform*)の静的メソッドを使用して、PCL または SAP で iOS に固有の `Padding` プロパティを選択できます。 これらのメソッドが非推奨となりました

`Device.OnPlatform` メソッドは、プラットフォーム固有のコードを実行したり、プラットフォーム固有の値を選択したりするために使用されます。 内部的には、 [`Device.OS`](xref:Xamarin.Forms.Device.OS)静的な読み取り専用プロパティを使用します。このプロパティは、 [`TargetPlatform`](xref:Xamarin.Forms.TargetPlatform)列挙体のメンバーを返します。

- [`iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)
- [`Android`](xref:Xamarin.Forms.TargetPlatform.Android)
- UWP デバイス用の[`Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) 。

`Device.OnPlatform` メソッド、`Device.OS` プロパティ、および `TargetPlatform` 列挙型はすべて非推奨となりました。 代わりに、 [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform)プロパティを使用し、`string` の戻り値と次の静的フィールドを比較します。

- [`iOS`](xref:Xamarin.Forms.Device.iOS)、文字列 "iOS"
- [`Android`](xref:Xamarin.Forms.Device.Android)、文字列 "Android"
- ユニバーサル Windows プラットフォームを参照する文字列 "UWP" を[`UWP`](xref:Xamarin.Forms.Device.UWP)します。

[`Device.Idiom`](xref:Xamarin.Forms.Device.Idiom)の静的な読み取り専用プロパティが関連付けられています。 次のメンバーを含む[`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom)のメンバーが返されます。

- [`Desktop`](xref:Xamarin.Forms.TargetIdiom.Desktop)
- [`Tablet`](xref:Xamarin.Forms.TargetIdiom.Tablet)
- [`Phone`](xref:Xamarin.Forms.TargetIdiom.Phone)
- [`Unsupported`](xref:Xamarin.Forms.TargetIdiom.Unsupported)が使用されていません

IOS と Android の場合、`Tablet` と `Phone` 間のカットオフは600単位の縦幅です。 Windows プラットフォームの場合、`Desktop` は Windows 10 で実行されている UWP アプリケーションを示し、`Phone` は Windows 10 アプリケーションで実行されている UWP アプリケーションを示します。

## <a name="solution-3a-set-margin-on-the-label"></a>ソリューションの 3 a。 ラベルに余白を設定

[`Margin`](xref:Xamarin.Forms.View.Margin)プロパティは、ブックに含めるには遅すぎましたが、`Thickness` 型でもあります。また、`Label` に設定して、ビューのレイアウトの計算に含まれるビューの外側の領域を定義することもできます。

`Padding` プロパティは、 [`Layout`](xref:Xamarin.Forms.Layout)および[`Page`](xref:Xamarin.Forms.Page)の派生物でのみ使用できます。 `Margin` プロパティは、すべての[`View`](xref:Xamarin.Forms.View)導関数で使用できます。

## <a name="solution-4-center-the-label-within-the-page"></a>解決策 4。 Center ページ内のラベル

`Page` 内で `Label` を中央に配置することも (または、他の8つの場所に配置することもできます)、`Label` の[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions)および[`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions)プロパティを[`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions)型の値に設定します。 `LayoutOptions` 構造体は、次の2つのプロパティを定義します。

- [`LayoutAlignment`](xref:Xamarin.Forms.LayoutAlignment)型の[`Alignment`](xref:Xamarin.Forms.LayoutOptions.Alignment)プロパティ。4つのメンバーを持つ列挙体です。 [`Start`](xref:Xamarin.Forms.LayoutAlignment.Start)は、向き、 [`Center`](xref:Xamarin.Forms.LayoutAlignment.Center)、 [`End`](xref:Xamarin.Forms.LayoutAlignment.End)に応じて左または下を意味します。これは、向きに応じて右または下を意味し、 [`Fill`](xref:Xamarin.Forms.LayoutAlignment.Fill)ます。

- `bool`型の[`Expands`](xref:Xamarin.Forms.LayoutOptions.Expands)プロパティです。

通常、これらのプロパティが直接使用されることはありません。 代わりに、これら2つのプロパティの組み合わせは、`LayoutOptions`型の8つの静的な読み取り専用プロパティによって提供されます。

- [`LayoutOptions.Start`](xref:Xamarin.Forms.LayoutOptions.Start)
- [`LayoutOptions.Center`](xref:Xamarin.Forms.LayoutOptions.Center)
- [`LayoutOptions.End`](xref:Xamarin.Forms.LayoutOptions.End)
- [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill)
- [`LayoutOptions.StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand)
- [`LayoutOptions.CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand)
- [`LayoutOptions.EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand)
- [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand)

`HorizontalOptions` と `VerticalOptions` は、Xamarin. フォームレイアウトで最も重要なプロパティです。詳細については、[**第4章を参照してください。スタックをスクロールして**](chapter04.md)います。

次の例では、`Label` の `HorizontalOptions` と `VerticalOptions` のプロパティが `LayoutOptions.Center`に設定されています。

[![グリーティングプログラムのトリプルスクリーンショット](images/ch02fg05-small.png "水平および垂直方向の中央揃えのラベル")](images/ch02fg05-large.png#lightbox "水平および垂直方向の中央揃えのラベル")

## <a name="solution-5-center-the-text-within-the-label"></a>5 のソリューションです。 ラベル内でテキストを中央揃え

また、`Label` の[`HorizontalTextAlignment`](xref:Xamarin.Forms.Label.HorizontalTextAlignment)および[`VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment)プロパティを[`TextAlignment`](xref:Xamarin.Forms.TextAlignment)列挙体のメンバーに設定することによって、テキストを中央揃えにする (または、ページ上の他の8つの場所に配置することもできます)。

- [`Start`](xref:Xamarin.Forms.TextAlignment.Start)(向きに応じて左または上) を意味します。
- [`Center`](xref:Xamarin.Forms.TextAlignment.Center)
- [`End`](xref:Xamarin.Forms.TextAlignment.End)(向きに応じて右または下)

これら2つのプロパティは、`Label`によってのみ定義されます。一方、`HorizontalAlignment` プロパティと `VerticalAlignment` プロパティは `View` で定義され、すべての `View` 派生元によって継承されます。 ビジュアルの結果と同様に思えますが、[次へ] の章で示すように非常に異なるいます。

## <a name="related-links"></a>関連リンク

- [第2章フルテキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch02-Apr2016.pdf)
- [第2章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02)
- [第 2 F#章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/FS)
- [Xamarin. Forms でのはじめに](~/get-started/index.yml)
