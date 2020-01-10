---
title: DataPages の概要
description: この記事では、Xamarin.Forms DataPages を使用して単純なデータ ドリブンのページの構築を開始する方法について説明します。
ms.prod: xamarin
ms.assetid: 6416E5FA-6384-4298-BAA1-A89381E47210
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 653d2420a9101203412b91a93cc7b6f681e2f5f2
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728305"
---
# <a name="getting-started-with-datapages"></a>DataPages の概要

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)

![](~/media/shared/preview.png "This API is currently in preview")

> [!IMPORTANT]
> DataPages を表示するには、Xamarin. Forms Theme リファレンスが必要です。 これには、プロジェクトに[xamarin. theme. Base](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/) nuget パッケージをインストールし、その後に、 [xamarin. theme](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/)パッケージまたは[xamarin](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/) . theme. theme パッケージのいずれかをインストールする必要があります。

DataPages Preview を使用して単純なデータ ドリブンのページの構築を開始、次の手順に従います。 このデモを使用して、プレビューでハードコーディングされたスタイル (「イベント」) をビルドするは、コード内の特定の JSON 形式でのみ機能します。

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

## <a name="1-add-nuget-packages"></a>1. NuGet パッケージを追加する

次の NuGet パッケージを Xamarin. Forms .NET Standard ライブラリおよびアプリケーションプロジェクトに追加します。

- Xamarin.Forms.Pages
- Xamarin.Forms.Theme.Base
- テーマ実装 NuGet (例 (Xamarin. Theme)

## <a name="2-add-theme-reference"></a>2. テーマ参照を追加する

**App.xaml**ファイルに追加し、カスタム`xmlns:mytheme`テーマのテーマがアプリケーションのリソース ディクショナリにマージされるかを確認してください。

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
  xmlns:mytheme="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light"
  x:Class="DataPagesDemo.App">
    <Application.Resources>
        <ResourceDictionary MergedWith="mytheme:LightThemeResources" />
    </Application.Resources>
</Application>
```

> [!IMPORTANT]
> また、iOS `AppDelegate` と Android `MainActivity`に定型コードを追加して、[テーマアセンブリ (下記) を読み込む](#loadtheme)手順にも従う必要があります。 これは、将来のプレビュー リリースで改善されます。

## <a name="3-add-a-xaml-page"></a>3. XAML ページを追加する

Xamarin.Forms アプリケーションに新しい XAML ページを追加し、*基本クラスを変更*から`ContentPage`に`Xamarin.Forms.Pages.ListDataPage`。 これは、c# と、XAML の両方で行う必要があります。

**C# ファイル**

```csharp
public partial class SessionDataPage : Xamarin.Forms.Pages.ListDataPage // was ContentPage
{
  public SessionDataPage ()
  {
    InitializeComponent ();
  }
}
```

**XAML ファイル**

ルート要素を変更するだけでなく`<p:ListDataPage>`カスタム名前空間を`xmlns:p`も追加する必要があります。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage">

    <ContentPage.Content></ContentPage.Content>

</p:ListDataPage>
```

**アプリケーション サブクラス**

変更、`App`クラスのコンス トラクターを`MainPage`に設定されている、`NavigationPage`を含む新しい`SessionDataPage`します。 ナビゲーション ページ*する必要があります*使用します。

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. DataSource を追加する

削除、`Content`要素置き換え、それを`p:ListDataPage.DataSource`データ ページを設定します。 リモート Json 次の例では、データ ファイルを URL から読み込まれています。

> [!NOTE]
> プレビューでは、データソースの表示ヒントを提供するために、`StyleClass` 属性*が必要です*。 `StyleClass="Events"`プレビューでは事前に定義し、スタイルを含むレイアウトを指す*ハードコード*使用されている JSON データ ソースと一致します。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage"
             Title="Sessions" StyleClass="Events">

    <p:ListDataPage.DataSource>
        <p:JsonDataSource Source="http://demo3143189.mockable.io/sessions" />
    </p:ListDataPage.DataSource>

</p:ListDataPage>
```

**JSON データ**

JSON データの例、[デモ ソース](http://demo3143189.mockable.io/sessions)を次に示します。

```json
[{
  "end": "2016-04-27T18:00:00Z",
  "start": "2016-04-27T17:15:00Z",
  "abstract": "The new Apple TV has been released, and YOU can be one of the first developers to write apps for it. To make things even better, you can build these apps in C#! This session will introduce the basics of how to create a tvOS app with Xamarin, including: differences between tvOS and iOS APIs, TV user interface best practices, responding to user input, as well as the capabilities and limitations of building apps for a television. Grab some popcorn—this is going to be good!",
  "title": "As Seen On TV … Bringing C# to the Living Room",
  "presenter": "Matthew Soucoup",
  "biography": "Matthew is a Xamarin MVP and Certified Xamarin Developer from Madison, WI. He founded his company Code Mill Technologies and started the Madison Mobile .Net Developers Group.  Matt regularly speaks on .Net and Xamarin development at user groups, code camps and conferences throughout the Midwest. Matt gardens hot peppers, rides bikes, and loves Wisconsin micro-brews and cheese.",
  "image": "http://i.imgur.com/ASj60DP.jpg",
  "avatar": "http://i.imgur.com/ASj60DP.jpg",
  "room": "Crick"
}]
```

## <a name="4-run"></a>4. を実行します。

作業のデータ ページで、上記の手順が得られます。

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

これは、事前に構築されたスタイルの **"イベント"** がライトテーマ NuGet パッケージに存在し、データソースに一致するスタイルが定義されているために機能します (例: "title"、"image"、「プレゼンター」)。

「イベント」`StyleClass`ビルドを表示する、`ListDataPage`をカスタム コントロール`CardView`で定義されている Xamarin.Forms.Pages で制御されています。 `CardView`コントロールが 3 つのプロパティ: `ImageSource`、 `Text`、および`Detail`します。 テーマ、データソースの 3 つのフィールド (JSON ファイル) からこれらのプロパティを表示するためにバインドするハードコーディングされています。

## <a name="5-customize"></a>5. カスタマイズ

テンプレートを指定してデータ ソースのバインドを使用して、継承されたスタイルをオーバーライドできます。 次の XAML は、新しい `ListItemControl` および `{p:DataSourceBinding}` の NuGet に含まれている構文を使用して、各行のカスタムテンプレートを宣言**します。**

```xaml
<p:ListDataPage.DefaultItemTemplate>
    <DataTemplate>
        <ViewCell>
            <p:ListItemControl
                Title="{p:DataSourceBinding title}"
                Detail="{p:DataSourceBinding room}"
                ImageSource="{p:DataSourceBinding image}"
                DataSource="{Binding Value}"
                HeightRequest="90"
            >
            </p:ListItemControl>
        </ViewCell>
    </DataTemplate>
</p:ListDataPage.DefaultItemTemplate>
```

提供することで、`DataTemplate`このコードは、`StyleClass`の既定のレイアウトを代わりに使用して、`ListItemControl`します。

[![](get-started-images/custom-sml.png "DataPages Sample Application")](get-started-images/custom.png#lightbox "DataPages Sample Application")

ソース バインドを XAML に c# にはデータを作成できますを好む開発者も (を必ず含めて、`using Xamarin.Forms.Pages;`ステートメント)。

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```

最初からテーマを作成するのはもう少しの作業ですが、今後のプレビューリリースではこれが簡単になります。

## <a name="troubleshooting"></a>トラブルシューティング

<a name="loadtheme" />

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>ファイルまたはアセンブリ 'Xamarin.Forms.Theme.Light' またはその依存関係の 1 つを読み込めませんでした。

プレビュー リリースでは、テーマの実行時に読み込むことがない場合があります。 このエラーを修正するのには、関連するプロジェクトに、以下に示すコードを追加します。

**Android**

**AppDelegate.cs**後に次の行を追加 `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Outlook Web Access (OWA)**

**MainActivity.cs**後に次の行を追加 `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```

## <a name="related-links"></a>関連リンク

- [DataPagesDemo サンプル](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
