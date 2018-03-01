---
title: "DataPages の概要"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6416E5FA-6384-4298-BAA1-A89381E47210
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 79ce4a1393b2affade3422f73e1c3d4680d199df
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="getting-started-with-datapages"></a>DataPages の概要

![](~/media/shared/preview.png "この API は現在プレビュー中")

> [!IMPORTANT]
> DataPages が必要です、 [Xamarin.Forms テーマ](~/xamarin-forms/user-interface/themes/index.md)レンダリングへの参照。


作業を開始する DataPages Preview を使用して単純なデータ ドライブのページを作成する、次の手順に従います。 プレビューにハードコードされたスタイル (「イベント」) を構築するこのデモで使用は、コード内の特定の JSON 形式でのみ機能します。

[ ![](get-started-images/demo-sml.png "DataPages サンプル アプリケーション")](get-started-images/demo.png "DataPages サンプル アプリケーション")

## <a name="1-add-nuget-packages"></a>1.NuGet パッケージを追加します。

Xamarin.Forms PCL およびアプリケーションのプロジェクトには、これらの Nuget パッケージを追加します。

* Xamarin.Forms.Pages
* Xamarin.Forms.Theme.Base
* テーマの実装 (Nuget Xamarin.Forms.Themes.Light)

## <a name="2-add-theme-reference"></a>2.テーマの参照を追加します。

**App.xaml**ファイルに追加し、カスタム`xmlns:mytheme`のテーマをテーマがアプリケーションのリソース ディクショナリにマージを確認してください。

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

**重要:**する手順を行う必要があります[テーマ アセンブリ (下記) を読み込む](#loadtheme)iOS に定型コードを追加することによって`AppDelegate`および Android`MainActivity`です。 これが、将来のプレビュー リリースでは改善されます。


## <a name="3-add-a-xaml-page"></a>3.XAML ページを追加します。

Xamarin.Forms アプリケーションに新しい XAML ページを追加し、*基本クラスを変更*から`ContentPage`に`Xamarin.Forms.Pages.ListDataPage`です。 これは、c# と XAML の両方で実行するのには。

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

変更するルート要素だけでなく`<p:ListDataPage>`カスタム名前空間を`xmlns:p`も追加する必要があります。

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

変更、`App`クラスのコンス トラクターを`MainPage`に設定されている、`NavigationPage`を含む新しい`SessionDataPage`です。 ナビゲーション ページ*必要があります*使用します。

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3.データ ソースを追加します。

削除、`Content`要素置き換えます、`p:ListDataPage.DataSource`データ ページを設定します。 リモート Json 次の例では、データ ファイルを URL から読み込まれています。

**注:**プレビュー*が必要です*、`StyleClass`属性をデータ ソースのレンダリングのヒントを提供します。 `StyleClass="Events"`プレビューでは事前に定義し、スタイルを含むレイアウトを指す*ハードコードされた*使用されている JSON のデータ ソースを一致するようにします。

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

## <a name="4-run"></a>4.実行です。

上記の手順は、作業のデータ ページになります。

[ ![](get-started-images/demo-sml.png "DataPages サンプル アプリケーション")](get-started-images/demo.png "DataPages サンプル アプリケーション")

これは機能するため、構築済みのスタイル**「イベント」**ライト テーマの Nuget パッケージに存在し、(データ ソースに一致するように定義されているスタイルを持つ "title", "image", "presenter").

「イベント」`StyleClass`ビルドを表示する、`ListDataPage`をカスタム コントロール`CardView`で定義されている Xamarin.Forms.Pages で制御されています。 `CardView`コントロールが 3 つのプロパティ: `ImageSource`、 `Text`、および`Detail`です。 データ ソースの 3 つのフィールド (JSON ファイル) からこれらのプロパティを表示するためにバインドするはハードコードされています。

## <a name="5-customize"></a>5.カスタマイズ

継承されたスタイルは、テンプレートを指定して、データ ソースのバインドを使用してオーバーライドできます。 次の XAML を new を使用して行ごとにカスタム テンプレートを宣言`ListItemControl`と`{p:DataSourceBinding}`に含まれている構文、 **Xamarin.Forms.Pages** Nuget:

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

提供することによって、`DataTemplate`このコードは、`StyleClass`代わりに既定のレイアウトを使用して、`ListItemControl`です。

[ ![](get-started-images/custom-sml.png "DataPages サンプル アプリケーション")](get-started-images/custom.png "DataPages サンプル アプリケーション")

C# XAML にデータを作成する開発者はソース バインドすぎます (を必ず含めて、`using Xamarin.Forms.Pages;`ステートメント)。

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```


最初からテーマを作成する、もう少しの作業が (を参照してください、[テーマ ガイド](~/xamarin-forms/user-interface/themes/index.md)) が、将来のプレビュー リリースは簡単に作成を行うには。


## <a name="troubleshooting"></a>トラブルシューティング

<a name="loadtheme"/>

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>ファイルまたはアセンブリ 'Xamarin.Forms.Theme.Light' またはその依存関係の 1 つを読み込めませんでした。

プレビュー リリースでテーマの実行時に読み込むこといない場合があります。 このエラーを解決するのには、関連するプロジェクトに、以下に示すコードを追加します。

**iOS**

**<code>appdelegate.cs</code>**後に次の行を追加 `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

**MainActivity.cs**後に次の行を追加 `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```



## <a name="related-links"></a>関連リンク

- [DataPagesDemo サンプル](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
