---
title: 画像Xamarin.Forms
description: イメージは、を使用してプラットフォーム間で共有でき Xamarin.Forms 、プラットフォームごとに個別に読み込むことも、表示用にダウンロードすることもできます。
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/19/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3ad0981c0249bc81a97d5c48489167d81a1523de
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938469"
---
# <a name="images-in-xamarinforms"></a>画像Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages)

_イメージは、を使用してプラットフォーム間で共有でき Xamarin.Forms 、プラットフォームごとに個別に読み込むことも、表示用にダウンロードすることもできます。_

イメージは、アプリケーションのナビゲーション、使いやすさ、およびブランド化の重要な部分です。 Xamarin.Formsアプリケーションでは、すべてのプラットフォーム間でイメージを共有できる必要がありますが、プラットフォームごとに異なるイメージが表示される可能性もあります。

アイコンおよびスプラッシュスクリーンにもプラットフォーム固有のイメージが必要です。これらは、プラットフォームごとに構成する必要があります。

## <a name="display-images"></a>画像の表示

Xamarin.Forms[`Image`](xref:Xamarin.Forms.Image)ビューを使用して、ページ上にイメージを表示します。 これにはいくつかの重要なプロパティがあります。

- [`Source`](xref:Xamarin.Forms.Image.Source)- [`ImageSource`](xref:Xamarin.Forms.ImageSource) ファイル、Uri、またはリソースのいずれかのインスタンス。表示するイメージを設定します。
- [`Aspect`](xref:Xamarin.Forms.Image.Aspect)-イメージが表示されている境界内でイメージのサイズを変更する方法 (伸縮するか、またはレターレターにするか)。

[`ImageSource`](xref:Xamarin.Forms.ImageSource)インスタンスは、イメージソースの種類ごとに静的メソッドを使用して取得できます。

- [`FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String))-各プラットフォームで解決できるファイル名またはファイルパスが必要です。
- [`FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri))-Uri オブジェクトが必要です。例:  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*)-**ビルドアクション EmbeddedResource**を使用して、アプリケーションまたは .NET Standard ライブラリプロジェクトに埋め込まれているイメージファイルのリソース識別子が必要です。
- [`FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream}))-イメージデータを提供するストリームが必要です。

プロパティは、 [`Aspect`](xref:Xamarin.Forms.Image.Aspect) 表示領域に合わせてイメージをスケーリングする方法を決定します。

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill)-画像を完全に拡大し、表示領域を正確に塗りつぶします。 これにより、イメージがゆがんでしまう可能性があります。
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill)-画像をクリップして、縦横比を維持しながら、表示領域がいっぱいになるようにします (ひずみはありません)。
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit)-イメージ全体が表示領域に収まるように (必要に応じて) イメージを Letterboxes します。これにより、画像の幅が広いか高さに応じて、上下または横に空白が追加されます。

イメージは、[ローカルファイル](#local-images)、[埋め込みリソース](#embedded-images)、[ダウンロード](#download-images)、またはストリームから読み込むことができます。 また、 [`Image`](xref:Xamarin.Forms.Image) オブジェクトのフォントアイコンデータを指定することで、ビューによってフォントアイコンを表示することもでき `FontImageSource` ます。 詳細については、「[フォント](~/xamarin-forms/user-interface/text/fonts.md)ガイド」の「[フォントアイコンの表示](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons)」を参照してください。

## <a name="local-images"></a>ローカルイメージ

イメージファイルは、各アプリケーションプロジェクトに追加し、共有コードから参照でき Xamarin.Forms ます。 このイメージ配布方法は、イメージがプラットフォーム固有の場合に必要になります。例えば、異なるプラットフォームで異なる解像度を使用する場合や、わずかに異なるデザインを使用する場合などです。

すべてのアプリで1つのイメージを使用するには、すべての*プラットフォームで同じファイル名を使用する必要が*あります。また、有効な Android リソース名を指定する必要があります (つまり、小文字、数字、アンダースコア、ピリオドのみを使用できます)。

- **ios** -ios 9 以降でイメージを管理およびサポートする場合は、**資産カタログのイメージセット**を使用することをお勧めします。これには、アプリケーションのさまざまなデバイスとスケールファクターをサポートするために必要なイメージのすべてのバージョンが含まれている必要があります。 詳細については、「[アセットカタログイメージセットへのイメージの追加](~/ios/app-fundamentals/images-icons/displaying-an-image.md)」を参照してください。
- **Android** -ビルドアクションを使用して**リソース/** 作成ディレクトリにイメージを配置します **: androidresource**。 イメージの高および低 DPI バージョンも提供できます (適切な名前が付けられた**リソース**サブディレクトリ **(たとえば、** 描画可能**な-ldpi**、 **xhdpi**)。
- **ユニバーサル Windows プラットフォーム (UWP)** -既定では、イメージはアプリケーションのルートディレクトリに**ビルドアクション: コンテンツ**と共に配置する必要があります。 また、イメージを別のディレクトリに配置して、プラットフォーム固有ので指定することもできます。 詳細については、「 [Windows の既定のイメージディレクトリ](~/xamarin-forms/platform/windows/default-image-directory.md)」を参照してください。

> [!IMPORTANT]
> IOS 9 より前の場合、イメージは通常、 **Build Action: BundleResource**を使用して**Resources**フォルダーに配置されていました。 ただし、iOS アプリでイメージを操作するこの方法は、Apple によって非推奨とされています。 詳細については、「[イメージのサイズとファイル名](~/ios/app-fundamentals/images-icons/displaying-an-image.md)」を参照してください。

これらの規則をファイルの名前付けと配置に準拠させることで、次の XAML がすべてのプラットフォームにイメージを読み込んで表示できるようになります。

```xaml
<Image Source="waterfront.jpg" />
```

同等の C# コードは次のとおりです。

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

次のスクリーンショットは、各プラットフォームでローカルイメージを表示した結果を示しています。

[![ローカルイメージを表示するサンプルアプリケーション](images-images/local-sml.png)](images-images/local.png#lightbox)

柔軟性を高めるために、次の `Device.RuntimePlatform` コード例に示すように、プロパティを使用して、一部またはすべてのプラットフォームに対して異なるイメージファイルまたはパスを選択できます。

```csharp
image.Source = Device.RuntimePlatform == Device.Android
                ? ImageSource.FromFile("waterfront.jpg")
                : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> すべてのプラットフォームで同じイメージファイル名を使用するには、すべてのプラットフォームで名前が有効である必要があります。 Android の drawables には名前付けの制限があります。これは、小文字、数字、アンダースコア、ピリオドのみを使用できます。また、クロスプラットフォームの互換性のためには、他のすべてのプラットフォームでも従う必要があります。 例のファイル名**waterfront.png**は規則に従いますが、無効なファイル名の例としては、"水 front.png"、"WaterFront.png"、"water-front.png"、"wåterfront.png" などがあります。

### <a name="native-resolutions-retina-and-high-dpi"></a>ネイティブの解像度 (retina と高 DPI)

iOS、Android、UWP には、さまざまなイメージの解像度がサポートされています。オペレーティングシステムは、デバイスの機能に基づいて実行時に適切なイメージを選択します。 Xamarin.Formsは、ネイティブプラットフォームの Api を使用してローカルイメージを読み込みます。そのため、ファイルの名前が正しく、プロジェクトに配置されている場合は、代替の解決策を自動的にサポートします。

IOS 9 以降でイメージを管理する場合は、適切な資産カタログのイメージセットに必要な各解像度のイメージをドラッグすることをお勧めします。 詳細については、「[アセットカタログイメージセットへのイメージの追加](~/ios/app-fundamentals/images-icons/displaying-an-image.md)」を参照してください。

IOS 9 より前では、retina バージョンのイメージは、 **Resources** **@2x** **@3x** ファイル拡張子の前にファイル名にまたはサフィックスを付けて、リソースフォルダー-2 ~ 3 倍の解像度で配置できます。 **myimage@2x.png**). ただし、iOS アプリでイメージを操作するこの方法は、Apple によって非推奨とされています。 詳細については、「[イメージのサイズとファイル名](~/ios/app-fundamentals/images-icons/displaying-an-image.md)」を参照してください。

Android の代替解像度のイメージは、次のスクリーンショットに示すように、Android プロジェクトの[特別に名前が付け](https://developer.android.com/guide/practices/screens_support.html)られたディレクトリに配置する必要があります。

[![Android の複数解像度の画像の場所](images-images/xs-highdpisolution-sml.png)](images-images/xs-highdpisolution.png#lightbox)

UWP イメージファイル名は、 [ `.scale-xxx` ファイル拡張子の前にサフィックスを付けることができ](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast)ます。ここで、 `xxx` は資産に適用されるスケーリングの比率 ( **myimage.scale-200.png**など) です。 イメージは、スケール修飾子を使用せずにコードまたは XAML で参照できます。たとえば、 **myimage.png**だけです。 プラットフォームは、ディスプレイの現在の DPI に基づいて、最も近い適切な資産スケールを選択します。

### <a name="additional-controls-that-display-images"></a>画像を表示する追加のコントロール

一部のコントロールには、次のような画像を表示するプロパティがあります。

- [`Button`](xref:Xamarin.Forms.Button)には [`ImageSource`](xref:Xamarin.Forms.Button.ImageSource) 、に表示されるビットマップイメージに設定できるプロパティがあり `Button` ます。 詳細については、「[ボタンを使用したビットマップの使用](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)」を参照してください。
- [`ImageButton`](xref:Xamarin.Forms.Button)には [`Source`](xref:Xamarin.Forms.ImageButton.Source) 、に表示するイメージに設定できるプロパティがあり `ImageButton` ます。 詳細については、「[イメージソースの設定](~/xamarin-forms/user-interface/imagebutton.md#setting-the-image-source)」を参照してください。
- [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)には、 [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) ファイル、埋め込みリソース、URI、またはストリームから読み込まれたイメージに設定できるプロパティがあります。
- [`ImageCell`](xref:Xamarin.Forms.ImageCell)には、 [`ImageSource`](xref:Xamarin.Forms.ImageCell.ImageSource) ファイル、埋め込みリソース、URI、またはストリームから取得したイメージに設定できるプロパティがあります。
- [`Page`](xref:Xamarin.Forms.Page). から派生するすべてのページの種類 `Page` には [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) プロパティとプロパティがあり [`BackgroundImageSource`](xref:Xamarin.Forms.Page.BackgroundImageSource) 、ファイル、埋め込みリソース、URI、またはストリームを割り当てることができます。 でが表示されている場合など、特定の状況下では、 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) [`ContentPage`](xref:Xamarin.Forms.ContentPage) プラットフォームでサポートされている場合はアイコンが表示されます。

  > [!IMPORTANT]
  > IOS では、 [`Page.IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) 資産カタログのイメージセット内のイメージからプロパティを設定することはできません。 代わりに、 `Page.IconImageSource` ファイル、埋め込みリソース、URI、またはストリームから、プロパティのアイコンイメージを読み込みます。

## <a name="embedded-images"></a>埋め込み画像

埋め込み画像もアプリケーション (ローカルイメージなど) に付属していますが、各アプリケーションのファイル構造に画像のコピーが含まれているのではなく、イメージファイルがリソースとしてアセンブリに埋め込まれています。 このイメージの配布方法は、各プラットフォームで同じイメージを使用する場合に推奨されます。イメージはコードにバンドルされるため、コンポーネントの作成に特に適しています。

プロジェクトにイメージを埋め込むには、右クリックして新しい項目を追加し、追加するイメージ/s を選択します。 既定では、イメージに**ビルドアクション**はありません。**ビルドアクション: EmbeddedResource**に設定する必要があります。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![ビルドアクションを埋め込みリソースに設定](images-images/vs-buildaction-sml.png)](images-images/vs-buildaction.png#lightbox)

**ビルドアクション**は、ファイルの [**プロパティ**] ウィンドウで表示および変更できます。

この例では、リソース ID は**WorkingWithImages.beach.jpg**です。
IDE では、このプロジェクトの既定の**名前空間**とファイル名を連結することによって、この既定の名前空間を生成しました。各値の間にピリオド (.) を使用します。
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![ビルドアクションの設定: EmbeddedResource](images-images/xs-buildaction.png)

**ビルドアクション**は、ファイルの**プロパティ**パッドで表示および変更することもできます。
このパッドには、コードでリソースを参照するために使用される**リソース ID**が表示されます。 次のスクリーンショットでは、**リソース ID**は**WorkingWithImages.beach.jpg**しています。
IDE では、このプロジェクトの既定の**名前空間**とファイル名を連結することによって、この既定の名前空間を生成しました。各値の間にピリオド (.) を使用します。
この ID は**プロパティ**パッドで編集できますが、これらの例では**WorkingWithImages.beach.jpg**値が使用されます。

[![埋め込みリソースプロパティパッド](images-images/xs-embeddedproperties-sml.png)](images-images/xs-embeddedproperties.png#lightbox)

-----

埋め込み画像をプロジェクト内のフォルダーに配置する場合、フォルダー名もリソース ID のピリオド (.) で区切られます。 **beach.jpg**イメージを**myimages**という名前のフォルダーに移動すると、リソース ID がになり**WorkingWithImages.MyImages.beach.jpg**

埋め込み画像を読み込むコードは、次に示すように、**リソース ID**をメソッドに渡し [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) ます。

```csharp
Image embeddedImage = new Image
{
    Source = ImageSource.FromResource("WorkingWithImages.beach.jpg", typeof(MyClass).GetTypeInfo().Assembly)
};
```

> [!NOTE]
> ユニバーサル Windows プラットフォームでのリリースモードでの埋め込み画像の表示をサポートするには、 `ImageSource.FromResource` イメージを検索するソースアセンブリを指定するのオーバーロードを使用する必要があります。

現時点では、リソース識別子に対して暗黙的な変換は行われません。 代わりに、 [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) またはを使用して `new ResourceImageSource()` 埋め込み画像を読み込む必要があります。

次のスクリーンショットは、各プラットフォームに埋め込まれたイメージを表示した結果を示しています。

[![埋め込み画像を表示するサンプルアプリケーション](images-images/resource-sml.png)](images-images/resource.png#lightbox)

### <a name="xaml"></a>XAML

からへの組み込み型コンバーターがないため `string` `ResourceImageSource` 、これらの種類のイメージを XAML でネイティブに読み込むことはできません。 代わりに、単純なカスタム XAML マークアップ拡張機能を記述して、XAML で指定された**リソース ID**を使用してイメージを読み込むことができます。

```csharp
[ContentProperty (nameof(Source))]
public class ImageResourceExtension : IMarkupExtension
{
 public string Source { get; set; }

 public object ProvideValue (IServiceProvider serviceProvider)
 {
   if (Source == null)
   {
     return null;
   }

   // Do your translation lookup here, using whatever method you require
   var imageSource = ImageSource.FromResource(Source, typeof(ImageResourceExtension).GetTypeInfo().Assembly);

   return imageSource;
 }
}
```

> [!NOTE]
> ユニバーサル Windows プラットフォームでのリリースモードでの埋め込み画像の表示をサポートするには、 `ImageSource.FromResource` イメージを検索するソースアセンブリを指定するのオーバーロードを使用する必要があります。

この拡張機能を使用するには、 `xmlns` プロジェクトの正しい名前空間とアセンブリ値を使用して、カスタムを XAML に追加します。 この構文を使用してイメージソースを設定でき `{local:ImageResource WorkingWithImages.beach.jpg}` ます。 完全な XAML の例を次に示します。

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage
   xmlns="http://xamarin.com/schemas/2014/forms"
   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
   xmlns:local="clr-namespace:WorkingWithImages;assembly=WorkingWithImages"
   x:Class="WorkingWithImages.EmbeddedImagesXaml">
 <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
   <!-- use a custom Markup Extension -->
   <Image Source="{local:ImageResource WorkingWithImages.beach.jpg}" />
 </StackLayout>
</ContentPage>
```

### <a name="troubleshoot-embedded-images"></a>埋め込み画像のトラブルシューティング

#### <a name="debug-code"></a>コードのデバッグ

特定のイメージリソースが読み込まれていない理由を理解することは難しい場合があるため、次のデバッグコードをアプリケーションに一時的に追加して、リソースが正しく構成されていることを確認することができます。 このメソッドは、指定されたアセンブリに埋め込まれているすべての既知のリソースを**コンソール**に出力し、リソースの読み込みに関する問題を解決します。

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(MyClass).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

#### <a name="images-embedded-in-other-projects"></a>他のプロジェクトに埋め込まれているイメージ

既定では、 `ImageSource.FromResource` メソッドは、メソッドを呼び出すコードと同じアセンブリ内のイメージのみを検索し `ImageSource.FromResource` ます。 上記のデバッグコードを使用して、 `typeof()` ステートメントを `Type` 各アセンブリ内の既知のに変更することで、特定のリソースを含むアセンブリを特定できます。

ただし、埋め込み画像を検索するソースアセンブリは、メソッドの引数として指定でき `ImageSource.FromResource` ます。

```csharp
var imageSource = ImageSource.FromResource("filename.png",
            typeof(MyClass).GetTypeInfo().Assembly);
```

## <a name="download-images"></a>イメージのダウンロード

次の XAML に示すように、イメージを自動的にダウンロードして表示することができます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
       x:Class="WorkingWithImages.DownloadImagesXaml">
  <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
    <Label Text="Image UriSource Xaml" />
    <Image Source="https://aka.ms/campus.jpg" />
    <Label Text="campus.jpg gets downloaded from microsoft.com" />
  </StackLayout>
</ContentPage>
```

同等の C# コードは次のとおりです。

```csharp
var webImage = new Image {
     Source = ImageSource.FromUri(
        new Uri("https://aka.ms/campus.jpg")
     ) };
```

メソッドは、 [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) オブジェクトを必要とし、 `Uri` [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) から読み取る新しいを返し `Uri` ます。

URI 文字列にも暗黙的な変換が行われるため、次の例も機能します。

```csharp
webImage.Source = "https://aka.ms/campus.jpg";
```

次のスクリーンショットは、各プラットフォームでリモートイメージを表示した結果を示しています。

[![ダウンロードしたイメージを表示するサンプルアプリケーション](images-images/download-sml.png)](images-images/download.png#lightbox)

### <a name="downloaded-image-caching"></a>ダウンロードされたイメージのキャッシュ

は、 [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) 次のプロパティを使用して構成されたダウンロードイメージのキャッシュもサポートしています。

- [`CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled)-キャッシュが有効になっているかどうか ( `true` 既定)。
- [`CacheValidity`](xref:Xamarin.Forms.UriImageSource.CacheValidity)- `TimeSpan` イメージをローカルに保存する期間を定義する。

キャッシュは既定で有効になり、24時間ローカルにイメージを保存します。 特定のイメージのキャッシュを無効にするには、次のようにイメージソースをインスタンス化します。

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri = new Uri("https://server.com/image") };
```

特定のキャッシュ期間 (たとえば、5日) を設定するには、次のようにイメージソースをインスタンス化します。

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://aka.ms/campus.jpg"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

組み込みのキャッシュ機能を使用すると、イメージの一覧のスクロールなどのシナリオを非常に簡単にサポートできるようになります。この場合、各セルにイメージを設定 (またはバインド) し、セルがスクロールして表示されるときに、組み込みのキャッシュでイメージの再読み込みを行うことができます。

## <a name="animated-gifs"></a>アニメーション Gif

Xamarin.Forms小さいアニメーション Gif を表示するためのサポートが含まれています。 これを行うには、 [`Image.Source`](xref:Xamarin.Forms.Image.Source) プロパティをアニメーション GIF ファイルに設定します。

```xaml
<Image Source="demo.gif" />
```

> [!IMPORTANT]
> のアニメーション GIF サポートには Xamarin.Forms ファイルをダウンロードする機能が含まれていますが、アニメーション gif のキャッシュまたはストリーミングはサポートされていません。

既定では、アニメーション GIF が読み込まれると、再生されません。 これは、 `IsAnimationPlaying` アニメーション gif が再生中か停止中かを制御するプロパティには、既定値が設定されているためです `false` 。 型のこのプロパティ `bool` は、オブジェクトによって支えられています。これは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) データバインディングのターゲットとスタイルを設定できることを意味します。

したがって、アニメーション GIF が読み込まれると、 `IsAnimationPlaying` プロパティがに設定されるまで再生されません `true` 。 再生を停止するには、 `IsAnimationPlaying` プロパティをに設定し `false` ます。 このプロパティは、非 GIF イメージソースを表示する場合には効果がないことに注意してください。

> [!NOTE]
> Android では、アニメーション GIF のサポートによって、アプリケーションが高速レンダラーを使用する必要があり、レガシレンダラーを使用することを選択した場合には機能しません。
> UWP では、アニメーション GIF のサポートには、Windows 10 記念日更新プログラム (バージョン 1607) の最小リリースが必要です。

## <a name="icons-and-splash-screens"></a>アイコンとスプラッシュスクリーン

ビューに関連付けられていません [`Image`](xref:Xamarin.Forms.Image) が、アプリケーションアイコンとスプラッシュスクリーンは、プロジェクト内の画像の重要な使用方法でもあり Xamarin.Forms ます。

アプリのアイコンとスプラッシュスクリーンの設定 Xamarin.Forms は、各アプリケーションプロジェクトで実行されます。 これは、iOS、Android、UWP 用に適切にサイズ設定されたイメージを生成することを意味します。 これらのイメージは、各プラットフォームの要件に従って名前が付けられ、配置されている必要があります。

## <a name="icons"></a>アイコン

これらのアプリケーションリソースの作成の詳細については、「IOS での[イメージ](~/ios/app-fundamentals/images-icons/index.md)の使用」、「 [Google Iconography](https://developer.android.com/design/style/iconography.html)」、および「[タイルとアイコンの資産に関する UWP ガイドライン](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)」を参照してください。

また、 [`Image`](xref:Xamarin.Forms.Image) オブジェクトのフォントアイコンデータを指定することで、ビューによってフォントアイコンを表示することもでき `FontImageSource` ます。 詳細については、「[フォント](~/xamarin-forms/user-interface/text/fonts.md)ガイド」の「[フォントアイコンの表示](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons)」を参照してください。

## <a name="splash-screens"></a>スプラッシュ画面

スプラッシュスクリーン (起動画面または既定のイメージとも呼ばれます) を必要とするのは、iOS アプリケーションと UWP アプリケーションのみです。

Windows デベロッパーセンターで、 [iOS のイメージ](~/ios/app-fundamentals/images-icons/index.md)と[スプラッシュスクリーン](/windows/uwp/launch-resume/splash-screens/)を操作する方法については、こちらのドキュメントを参照してください。

## <a name="related-links"></a>関連リンク

- [WorkingWithImages (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages)
- [iOS でのイメージの使用](~/ios/app-fundamentals/images-icons/index.md)
- [Android Iconography](https://developer.android.com/design/style/iconography.html)
- [タイルとアイコン アセットのガイドライン](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
