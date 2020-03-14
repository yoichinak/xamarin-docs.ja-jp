---
title: Xamarin.Forms でのイメージ
description: イメージは、Xamarin.Forms のプラットフォームで共有できる、具体的には、各プラットフォーム用に読み込むことができるまたは表示をダウンロードすることができます。
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
ms.openlocfilehash: 255d3f2f532e4899b1a890405af942a7ca2da8ea
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79305709"
---
# <a name="images-in-xamarinforms"></a>Xamarin.Forms でのイメージ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages)

_イメージは、Xamarin. Forms を使用してプラットフォーム間で共有できます。また、プラットフォームごとに個別に読み込むことも、表示用にダウンロードすることもできます。_

イメージは、アプリケーションのナビゲーション、ユーザビリティ、およびブランド化の重要な部分です。 Xamarin.Forms アプリケーションは、すべてのプラットフォームでは、イメージを共有するが、各プラットフォームでも可能性があるさまざまなイメージを表示することができる必要があります。

プラットフォーム固有のイメージもアイコンとスプラッシュ スクリーン; に必要です。これらは、プラットフォームごとに構成する必要があります。

## <a name="display-images"></a>画像の表示

Xamarin では、 [`Image`](xref:Xamarin.Forms.Image)ビューを使用して、ページに画像を表示します。 2 つの重要なプロパティがあります。

- [`Source`](xref:Xamarin.Forms.Image.Source) - [`ImageSource`](xref:Xamarin.Forms.ImageSource)インスタンス。ファイル、Uri、リソースのいずれかで、表示するイメージを設定します。
- [`Aspect`](xref:Xamarin.Forms.Image.Aspect) -表示される境界内のイメージのサイズを変更する方法 (ストレッチ、トリミング、またはレターレター)。

[`ImageSource`](xref:Xamarin.Forms.ImageSource)インスタンスは、イメージソースの種類ごとに静的メソッドを使用して取得できます。

- [`FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) -各プラットフォームで解決できるファイル名またはファイルパスが必要です。
- [`FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) -Uri オブジェクトが必要です。例:  [`new Uri("http://server.com/image.jpg")`]。
- [`FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) -**ビルドアクション EmbeddedResource**を使用して、アプリケーションまたは .NET Standard ライブラリプロジェクトに埋め込まれたイメージファイルのリソース識別子が必要です。
- [`FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) -イメージデータを提供するストリームが必要です。

[`Aspect`](xref:Xamarin.Forms.Image.Aspect)プロパティは、表示領域に合わせてイメージをどのように拡大縮小するかを決定します。

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) -画像を完全に拡大し、表示領域を正確に塗りつぶします。 これにより、イメージ遠近法されている可能性があります。
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) -画像をクリップして、縦横比を維持しながら表示領域を塗りつぶすようにします (ひずみはありません)。
- イメージ全体が表示領域に収まるように (必要に応じて) イメージを Letterboxes します。これにより、画像の幅が広いか高さに応じて、上下または横に空白が追加されます。 [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit)

イメージは、[ローカルファイル](#local-images)、[埋め込みリソース](#embedded-images)、[ダウンロード](#download-images)、またはストリームから読み込むことができます。 また、`FontImageSource` オブジェクトのフォントアイコンデータを指定することによって、 [`Image`](xref:Xamarin.Forms.Image)ビューでフォントアイコンを表示できます。 詳細については、「[フォント](~/xamarin-forms/user-interface/text/fonts.md)ガイド」の「[フォントアイコンの表示](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons)」を参照してください。

## <a name="local-images"></a>ローカルイメージ

イメージ ファイルを各アプリケーション プロジェクトに追加し、Xamarin.Forms の共有コードから参照します。 このイメージ配布方法は、イメージがプラットフォーム固有の場合に必要になります。例えば、異なるプラットフォームで異なる解像度を使用する場合や、わずかに異なるデザインを使用する場合などです。

すべてのアプリで1つのイメージを使用するには、すべての*プラットフォームで同じファイル名を使用する必要が*あります。また、有効な Android リソース名を指定する必要があります (つまり、小文字、数字、アンダースコア、ピリオドのみを使用できます)。

- **ios** -ios 9 以降でイメージを管理およびサポートする場合は、**資産カタログのイメージセット**を使用することをお勧めします。これには、アプリケーションのさまざまなデバイスとスケールファクターをサポートするために必要なイメージのすべてのバージョンが含まれている必要があります。 詳細については、「[アセットカタログイメージセットへのイメージの追加](~/ios/app-fundamentals/images-icons/displaying-an-image.md)」を参照してください。
- **Android** -ビルドアクションを使用して**リソース/** 作成ディレクトリにイメージを配置します **: androidresource**。 イメージの高および低 DPI バージョンも提供できます (適切な名前が付けられた**リソース**サブディレクトリ **(たとえば、** 描画可能**な-ldpi**、 **xhdpi**)。
- **ユニバーサル Windows プラットフォーム (UWP)** -**ビルドアクション**を使用して、アプリケーションのルートディレクトリにイメージを配置します。コンテンツ。

> [!IMPORTANT]
> IOS 9 より前の場合、イメージは通常、 **Build Action: BundleResource**を使用して**Resources**フォルダーに配置されていました。 ただし、apple の iOS アプリで画像の操作には、このメソッドは廃止されました。 詳細については、「[イメージのサイズとファイル名](~/ios/app-fundamentals/images-icons/displaying-an-image.md)」を参照してください。

ファイルの名前付けおよび配置についてこれらの規則に準拠するには、次の XAML を読み込み、すべてのプラットフォーム イメージを表示することができます。

```xaml
<Image Source="waterfront.jpg" />
```

同等の c# コードは次のとおりです。

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

次のスクリーン ショットは、ローカル イメージを表示する各プラットフォームでの結果を表示します。

[ローカルイメージを表示する ![サンプルアプリケーション](images-images/local-sml.png)](images-images/local.png#lightbox)

柔軟性を高めるために、次のコード例に示すように、`Device.RuntimePlatform` プロパティを使用して、一部またはすべてのプラットフォームに対して異なるイメージファイルまたはパスを選択できます。

```csharp
image.Source = Device.RuntimePlatform == Device.Android 
                ? ImageSource.FromFile("waterfront.jpg") 
                : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> すべてのプラットフォームで同じイメージ ファイル名を使用するには、名前は、すべてのプラットフォームでは有効である必要があります。 ドローアブルの android では、名前付けに関する制約がある-小文字アルファベット、数字、アンダー スコア、およびピリオドのみが許可されている – とクロス プラットフォームの互換性のためこの従う必要があるその他のすべてのプラットフォームでも。 例のファイル名ウォーターフロントは規則に従い**ます**が、無効なファイル名の例としては、"ウォーターフロント"、"water-front"、"wåterfront" などがあります。

### <a name="native-resolutions-retina-and-high-dpi"></a>ネイティブの解像度 (retina と高 DPI)

iOS、Android、および UWP には、オペレーティング システムがデバイスの機能に基づいて実行時に適切なイメージを選択、さまざまな画像の解像度のサポートが含まれます。 Xamarin.Forms では、ローカルのイメージを読み込み、ファイルが正しくという名前し、プロジェクト内にある場合に自動的に代替の解像度をサポートに、ネイティブ プラットフォームの Api を使用します。

IOS 9 以降のイメージの管理の推奨される方法は、適切な資産カタログの画像セットに必要な各解像度のイメージをドラッグすることです。 詳細については、「[アセットカタログイメージセットへのイメージの追加](~/ios/app-fundamentals/images-icons/displaying-an-image.md)」を参照してください。

IOS 9 より前では、retina バージョンのイメージは、ファイル拡張子の前に、ファイル名に **@2x** または **@3x** サフィックスを使用して、**リソース**フォルダー-2 ~ 3 倍の解像度で配置できます。 **myimage@2x.png** )。 ただし、apple の iOS アプリで画像の操作には、このメソッドは廃止されました。 詳細については、「[イメージのサイズとファイル名](~/ios/app-fundamentals/images-icons/displaying-an-image.md)」を参照してください。

Android の代替解像度のイメージは、次のスクリーンショットに示すように、Android プロジェクトの[特別に名前が付け](https://developer.android.com/guide/practices/screens_support.html)られたディレクトリに配置する必要があります。

[Android の複数解像度の画像の場所を ![](images-images/xs-highdpisolution-sml.png)](images-images/xs-highdpisolution.png#lightbox)

UWP イメージファイル名の[末尾には、ファイル拡張子の前に `.scale-xxx` を付けることができ](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast)ます。 `xxx` は、資産に適用されるスケーリングの比率 (例: **scale-200**) です。 イメージは、コードまたは XAML で、スケール修飾子を使用せずに参照できます (例: **myimage .png**)。 プラットフォームでは、ディスプレイの現在の DPI に基づいて最も近い適切な資産のスケールを選択します。

### <a name="additional-controls-that-display-images"></a>画像を表示する追加のコントロール

一部のコントロールでは、イメージを表示するプロパティがあります。

- [`Button`](xref:Xamarin.Forms.Button)には、`Button`に表示されるビットマップイメージに設定できる[`ImageSource`](xref:Xamarin.Forms.Button.ImageSource)のプロパティがあります。 詳細については、「[ボタンを使用したビットマップの使用](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)」を参照してください。
- [`ImageButton`](xref:Xamarin.Forms.Button)には、`ImageButton`に表示するイメージに設定できる[`Source`](xref:Xamarin.Forms.ImageButton.Source)のプロパティがあります。 詳細については、「[イメージソースの設定](~/xamarin-forms/user-interface/imagebutton.md#setting-the-image-source)」を参照してください。
- [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)には、ファイル、埋め込みリソース、URI、またはストリームから読み込まれたイメージに設定できる[`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)のプロパティがあります。
- [`ImageCell`](xref:Xamarin.Forms.ImageCell)には、ファイル、埋め込みリソース、URI、またはストリームから取得したイメージに設定できる[`ImageSource`](xref:Xamarin.Forms.ImageCell.ImageSource)プロパティがあります。
- [`Page`](xref:Xamarin.Forms.Page)」を参照してください。 `Page` から派生するすべてのページの種類には[`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource)と[`BackgroundImageSource`](xref:Xamarin.Forms.Page.BackgroundImageSource)のプロパティがあります。このプロパティには、ファイル、埋め込みリソース、URI、またはストリームを割り当てることができます。 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)が[`ContentPage`](xref:Xamarin.Forms.ContentPage)を表示しているときなど、特定の状況下では、プラットフォームでサポートされている場合はアイコンが表示されます。

  > [!IMPORTANT]
  > IOS では、 [`Page.IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource)プロパティを、アセットカタログイメージセット内のイメージから設定することはできません。 代わりに、ファイル、埋め込みリソース、URI、またはストリームから `Page.IconImageSource` プロパティのアイコンイメージを読み込みます。

## <a name="embedded-images"></a>埋め込み画像

埋め込み画像は (ローカルのイメージ) のようなアプリケーションにも付属しますが、各アプリケーションのファイル構造、イメージ、イメージのコピーではなく、ファイルがリソースとしてアセンブリに埋め込まれました。 イメージの配布には、このメソッドは、各プラットフォームで同じイメージを使用する場合に推奨し、コードにバンドルされているイメージは、コンポーネントの作成に特に適しています。

プロジェクトには、画像を埋め込む、新しい項目を追加し、追加するイメージ/秒を選択して右クリックします。 既定では、イメージに**ビルドアクション**はありません。**ビルドアクション: EmbeddedResource**に設定する必要があります。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[ビルドアクションを埋め込みリソースに設定 ![](images-images/vs-buildaction-sml.png)](images-images/vs-buildaction.png#lightbox)

**ビルドアクション**は、ファイルの **[プロパティ]** ウィンドウで表示および変更できます。

この例では、リソース ID は**WorkingWithImages**です。
IDE では、このプロジェクトの既定の**名前空間**とファイル名を連結することによって、この既定の名前空間を生成しました。各値の間にピリオド (.) を使用します。
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![](images-images/xs-buildaction.png "Set Build Action: EmbeddedResource")

**ビルドアクション**は、ファイルの**プロパティ**パッドで表示および変更することもできます。
このパッドには、コードでリソースを参照するために使用される**リソース ID**が表示されます。 次のスクリーンショットでは、**リソース ID**は**WorkingWithImages**です。
IDE では、このプロジェクトの既定の**名前空間**とファイル名を連結することによって、この既定の名前空間を生成しました。各値の間にピリオド (.) を使用します。
この ID は**プロパティ**パッドで編集できますが、これらの例では、値**WorkingWithImages**が使用されます。

[埋め込みリソースプロパティパッドの ![](images-images/xs-embeddedproperties-sml.png)](images-images/xs-embeddedproperties.png#lightbox)

-----

リソース ID では、ピリオド (.) で、フォルダー名も区切られた、プロジェクト内にフォルダーに埋め込まれた画像を配置する場合 **ビーチ .jpg**イメージを**myimages**という名前のフォルダーに移動すると、 **WorkingWithImages**のリソース ID が生成されます。

埋め込み画像を読み込むコードは、次に示すように、単に**リソース ID**を[`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*)メソッドに渡します。

```csharp
var embeddedImage = new Image { 
      Source = ImageSource.FromResource(
        "WorkingWithImages.beach.jpg", 
        typeof(EmbeddedImages).GetTypeInfo().Assembly
      ) };
```

> [!NOTE]
> ユニバーサル Windows プラットフォームでのリリースモードでの埋め込み画像の表示をサポートするには、イメージを検索するソースアセンブリを指定する `ImageSource.FromResource` のオーバーロードを使用する必要があります。

現在のリソース識別子の暗黙的な変換ではありません。 代わりに、 [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*)または `new ResourceImageSource()` を使用して埋め込み画像を読み込む必要があります。

次のスクリーン ショットは、埋め込み画像を表示する各プラットフォームでの結果を表示します。

[埋め込み画像を表示する ![サンプルアプリケーション](images-images/resource-sml.png)](images-images/resource.png#lightbox)

### <a name="xaml"></a>XAML

`string` から `ResourceImageSource`に組み込まれている型コンバーターがないため、これらの種類のイメージを XAML でネイティブに読み込むことはできません。 代わりに、単純なカスタム XAML マークアップ拡張機能を記述して、XAML で指定された**リソース ID**を使用してイメージを読み込むことができます。

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
> ユニバーサル Windows プラットフォームでのリリースモードでの埋め込み画像の表示をサポートするには、イメージを検索するソースアセンブリを指定する `ImageSource.FromResource` のオーバーロードを使用する必要があります。

この拡張機能を使用するには、プロジェクトの正しい名前空間とアセンブリ値を使用して、カスタム `xmlns` を XAML に追加します。 イメージソースは、次の構文を使用して設定できます: `{local:ImageResource WorkingWithImages.beach.jpg}`。 完全な XAML の例は、以下に示します。

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

特定のイメージ リソースが読み込まれていない理由を理解しにくい場合があります、ため、次のコードのデバッグが、リソースが正しく構成されていることを確認するためのアプリケーションを一時的に追加できます。 このメソッドは、指定されたアセンブリに埋め込まれているすべての既知のリソースを**コンソール**に出力し、リソースの読み込みに関する問題を解決します。

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(EmbeddedImages).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

#### <a name="images-embedded-in-other-projects"></a>他のプロジェクトに埋め込まれているイメージ

既定では、`ImageSource.FromResource` メソッドは、`ImageSource.FromResource` メソッドを呼び出すコードと同じアセンブリ内のイメージのみを検索します。 上記のデバッグコードを使用して、`typeof()` ステートメントを各アセンブリに存在することがわかっている `Type` に変更することで、特定のリソースを含むアセンブリを特定できます。

ただし、埋め込み画像を検索するソースアセンブリは、`ImageSource.FromResource` メソッドの引数として指定できます。

```csharp
var imageSource = ImageSource.FromResource("filename.png", 
            typeof(MyClass).GetTypeInfo().Assembly);
```

## <a name="download-images"></a>イメージのダウンロード

イメージに自動的にダウンロードできます、表示のための次の XAML に示すよう。

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
       x:Class="WorkingWithImages.DownloadImagesXaml">
  <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
    <Label Text="Image UriSource Xaml" />
    <Image Source="https://xamarin.com/content/images/pages/forms/example-app.png" />
    <Label Text="example-app.png gets downloaded from xamarin.com" />
  </StackLayout>
</ContentPage>
```

同等の c# コードは次のとおりです。

```csharp
var webImage = new Image { 
     Source = ImageSource.FromUri(
        new Uri("https://xamarin.com/content/images/pages/forms/example-app.png")
     ) };
```

[`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri))メソッドには `Uri` オブジェクトが必要で、`Uri`から読み取る新しい[`UriImageSource`](xref:Xamarin.Forms.UriImageSource)が返されます。

暗黙の変換が、URI 文字列のため、次の例は、動作も。

```csharp
webImage.Source = "https://xamarin.com/content/images/pages/forms/example-app.png";
```

次のスクリーン ショットは、リモート イメージを表示する各プラットフォームでの結果を表示します。

[ダウンロードしたイメージを表示する ![サンプルアプリケーション](images-images/download-sml.png)](images-images/download.png#lightbox)

### <a name="downloaded-image-caching"></a>ダウンロードされたイメージのキャッシュ

[`UriImageSource`](xref:Xamarin.Forms.UriImageSource)は、次のプロパティを使用して構成されたダウンロードイメージのキャッシュもサポートしています。

- [`CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) -キャッシュが有効になっているかどうか (既定では`true`)。
- [`CacheValidity`](xref:Xamarin.Forms.UriImageSource.CacheValidity) -イメージをローカルに保存する期間を定義する `TimeSpan`。

キャッシュは既定で有効になっているし、24 時間のローカルでイメージを格納します。 特定のイメージのキャッシュを無効にするには、イメージ ソースを次のようにインスタンス化します。

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri = new Uri("http://server.com/image") };
```

特定のキャッシュ期間 (たとえば、5 日) を設定するには、次のように イメージ ソースをインスタンス化します。

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://xamarin.com/content/images/pages/forms/example-app.png"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

組み込みキャッシュすると、各セルにイメージを設定 (またはできるバインド) イメージの一覧をスクロールなどのシナリオをサポートし、組み込みのキャッシュのセルがビューにスクロールされたとき、イメージの再読み込みを処理できるようにする非常に簡単になります。

## <a name="animated-gifs"></a>アニメーション Gif

Xamarin. フォームには、小さいアニメーション Gif を表示するためのサポートが含まれています。 これを行うには、 [`Image.Source`](xref:Xamarin.Forms.Image.Source)プロパティをアニメーション GIF ファイルに設定します。

```xaml
<Image Source="demo.gif" />
```

> [!IMPORTANT]
> Xamarin のアニメーション GIF サポートにはファイルをダウンロードする機能が含まれていますが、アニメーション Gif のキャッシュまたはストリーミングはサポートされていません。

既定では、アニメーション GIF が読み込まれると、再生されません。 これは、アニメーション GIF を再生するか停止するかを制御する `IsAnimationPlaying` プロパティに、既定値の `false`が設定されているためです。 `bool`型のこのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。これは、データバインディングのターゲットにすることができ、スタイルを設定できることを意味します。

したがって、アニメーション GIF が読み込まれると、`IsAnimationPlaying` プロパティが `true`に設定されるまで再生されません。 再生を停止するには、`IsAnimationPlaying` プロパティを `false`に設定します。 このプロパティは、非 GIF イメージソースを表示する場合には効果がないことに注意してください。

> [!NOTE]
> Android では、アニメーション GIF のサポートによって、アプリケーションが高速レンダラーを使用する必要があり、レガシレンダラーを使用することを選択した場合には機能しません。
> UWP では、アニメーション GIF のサポートには、Windows 10 記念日更新プログラム (バージョン 1607) の最小リリースが必要です。

## <a name="icons-and-splash-screens"></a>アイコンとスプラッシュ スクリーン

[`Image`](xref:Xamarin.Forms.Image)ビューには関連していませんが、アプリケーションアイコンとスプラッシュスクリーンは、Xamarin. Forms プロジェクトのイメージの重要な使用方法でもあります。

アイコンとスプラッシュ スクリーン Xamarin.Forms アプリの設定は、各アプリケーション プロジェクトで行われます。 これは、サイズの iOS、Android、および UWP 用のイメージを正しく生成することを意味します。 これらのイメージは、各プラットフォームの要件に従って配置してという名前をする必要があります。

## <a name="icons"></a>[小さいアイコン]

これらのアプリケーションリソースの作成の詳細については、「IOS での[イメージ](~/ios/app-fundamentals/images-icons/index.md)の使用」、「 [Google Iconography](https://developer.android.com/design/style/iconography.html)」、および「[タイルとアイコンの資産に関する UWP ガイドライン](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)」を参照してください。

また、`FontImageSource` オブジェクトのフォントアイコンデータを指定することによって、 [`Image`](xref:Xamarin.Forms.Image)ビューでフォントアイコンを表示できます。 詳細については、「[フォント](~/xamarin-forms/user-interface/text/fonts.md)ガイド」の「[フォントアイコンの表示](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons)」を参照してください。

## <a name="splash-screens"></a>スプラッシュ スクリーン

IOS と UWP アプリケーションのみ (スタートアップ画面または既定のイメージとも呼ばれます) のスプラッシュ スクリーンが必要です。

Windows デベロッパーセンターで、 [iOS のイメージ](~/ios/app-fundamentals/images-icons/index.md)と[スプラッシュスクリーン](/windows/uwp/launch-resume/splash-screens/)を操作する方法については、こちらのドキュメントを参照してください。

## <a name="related-links"></a>関連リンク

- [WorkingWithImages (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages)
- [iOS でのイメージの使用](~/ios/app-fundamentals/images-icons/index.md)
- [Android Iconography](https://developer.android.com/design/style/iconography.html)
- [タイルとアイコン資産のガイドライン](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
