---
title: "はじめに with DataPages" description: "この記事では、DataPages を使用して単純なデータドリブンページの構築を開始する方法について説明 Xamarin.Forms します。"
ms BAA1: xamarin ms assetid: 6416E5FA-6384-4298-A89381E47210: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 12/01/2017 no loc: [ Xamarin.Forms ,] を指定します。 Xamarin.Essentials
---

# <a name="getting-started-with-datapages"></a>DataPages でのはじめに

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)

![](~/media/shared/preview.png "This API is currently in preview")

> [!IMPORTANT]
> DataPages Xamarin.Forms を表示するには、テーマ参照が必要です。 これには、のインストールが含ま[ Xamarin.Forms れます。Theme。](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/)プロジェクトの基本となる NuGet パッケージの後に、[のいずれかが続きます。 Xamarin.FormsTheme](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/)または[ Xamarin.Forms 。Theme. ダーク](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/)NuGet パッケージ。

DataPages Preview を使用して単純なデータドリブンページの構築を開始するには、次の手順に従います。 このデモでは、コード内の特定の JSON 形式でのみ機能する、プレビュービルドでハードコーディングされたスタイル ("イベント") を使用します。

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

## <a name="1-add-nuget-packages"></a>1. NuGet パッケージを追加する

これらの NuGet パッケージを Xamarin.Forms .NET Standard ライブラリおよびアプリケーションプロジェクトに追加します。

- Xamarin.Forms.トピック
- Xamarin.Forms.Theme. Base
- テーマ実装 NuGet (例 Xamarin.Forms.Theme)

## <a name="2-add-theme-reference"></a>2. テーマ参照を追加する

**App.xaml**ファイルで、テーマのカスタムを追加 `xmlns:mytheme` し、テーマがアプリケーションのリソースディクショナリにマージされていることを確認します。

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
> また、iOS および Android に定型コードを追加して、[テーマアセンブリ (下記) を読み込む](#troubleshooting)手順にも従う必要があり `AppDelegate` `MainActivity` ます。 これは、今後のプレビューリリースで改善される予定です。

## <a name="3-add-a-xaml-page"></a>3. XAML ページを追加する

新しい XAML ページをアプリケーションに追加 Xamarin.Forms し、*基本クラス*をからに変更 `ContentPage` し `Xamarin.Forms.Pages.ListDataPage` ます。 これは、C# と XAML の両方で実行する必要があります。

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

ルート要素を `<p:ListDataPage>` のカスタム名前空間に変更することに加えて、 `xmlns:p` も追加する必要があります。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage">

    <ContentPage.Content></ContentPage.Content>

</p:ListDataPage>
```

**Application サブクラス**

クラスコンストラクターを変更して `App` 、 `MainPage` が新しいを含むに設定されるようにし `NavigationPage` `SessionDataPage` ます。 ナビゲーションページを使用する*必要があり*ます。

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. DataSource を追加する

要素を削除 `Content` し、をに置き換えて、 `p:ListDataPage.DataSource` ページにデータを設定します。 次の例では、リモート Json データファイルが URL から読み込まれています。

> [!NOTE]
> プレビューで*requires*は、 `StyleClass` データソースの表示ヒントを提供するために属性が必要です。 は、 `StyleClass="Events"` プレビューで事前に定義されているレイアウトを参照し、使用されている JSON データソースと一致するように*ハードコーディング*されたスタイルを含みます。

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

デモソースからの JSON データの例を次に示します。

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

上記の手順を実行すると、作業データページが生成されます。

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

これは、事前に構築されたスタイルの **"イベント"** がライトテーマ NuGet パッケージに存在し、データソースに一致するスタイルが定義されているために機能します (例: "title"、"image"、"プレゼンター")。

"Events" は、で定義されている `StyleClass` カスタムコントロールを使用してコントロールを表示するように構築されてい `ListDataPage` `CardView` Xamarin.Forms ます。トピック. `CardView`コントロールには `ImageSource` 、、、およびの3つのプロパティがあり `Text` `Detail` ます。 このテーマは、データソースの3つのフィールド (JSON ファイルから) をこれらのプロパティにバインドして表示するようにハードコーディングされています。

## <a name="5-customize"></a>5. カスタマイズ

継承されたスタイルは、テンプレートを指定し、データソースバインドを使用することによってオーバーライドできます。 次の XAML は、に含まれている新しいと構文を使用して、各行のカスタムテンプレートを宣言し `ListItemControl` `{p:DataSourceBinding}` ** Xamarin.Forms ます。ページ**NuGet:

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

このコードを指定することにより `DataTemplate` 、をオーバーライド `StyleClass` し、代わりにの既定のレイアウトを使用し `ListItemControl` ます。

[![](get-started-images/custom-sml.png "DataPages Sample Application")](get-started-images/custom.png#lightbox "DataPages Sample Application")

C# を XAML にする開発者は、データソースのバインドも作成できます (ステートメントを含めることを忘れないでください `using Xamarin.Forms.Pages;` )。

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```

最初からテーマを作成するのはもう少しの作業ですが、今後のプレビューリリースではこれが簡単になります。

## <a name="troubleshooting"></a>トラブルシューティング

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>ファイルまたはアセンブリ ' を読み込むことができませんでした Xamarin.Forms 。Theme (またはその依存関係の1つ)

プレビューリリースでは、実行時にテーマを読み込むことができない可能性があります。 このエラーを修正するには、以下に示すコードを関連するプロジェクトに追加します。

**Android**

**AppDelegate.cs**で、の後に次の行を追加します。`LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

**MainActivity.cs**で、の後に次の行を追加します。`LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```

## <a name="related-links"></a>関連リンク

- [DataPagesDemo サンプル](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
