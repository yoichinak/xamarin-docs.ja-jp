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
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490167"
---
# <a name="images-in-xamarinforms"></a>Xamarin.Forms でのイメージ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages)

_イメージは、Xamarin.Forms のプラットフォームで共有できる、具体的には、各プラットフォーム用に読み込むことができるまたは表示をダウンロードすることができます。_

イメージは、アプリケーションのナビゲーション、ユーザビリティ、およびブランド化の重要な部分です。 Xamarin.Forms アプリケーションは、すべてのプラットフォームでは、イメージを共有するが、各プラットフォームでも可能性があるさまざまなイメージを表示することができる必要があります。

プラットフォーム固有のイメージもアイコンとスプラッシュ スクリーン; に必要です。これらは、プラットフォームごとに構成する必要があります。

## <a name="display-images"></a>画像の表示

Xamarin.Forms を使用して、 [ `Image` ](xref:Xamarin.Forms.Image)をページにイメージを表示するビュー。 2 つの重要なプロパティがあります。

- [`Source`](xref:Xamarin.Forms.Image.Source) - [ `ImageSource` ](xref:Xamarin.Forms.ImageSource)インスタンス、ファイル、Uri またはリソースで、表示するイメージを設定します。
- [`Aspect`](xref:Xamarin.Forms.Image.Aspect) -(Stretch、トリミングまたはレター ボックスかどうか) 内で表示されている範囲内のイメージのサイズを変更する方法。

[`ImageSource`](xref:Xamarin.Forms.ImageSource) インスタンスは、画像ソースの種類ごとに静的メソッドを使用して取得できます。

- [`FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) -ファイル名または各プラットフォームで解決可能なファイル パスが必要です。
- [`FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) -次のような Uri オブジェクトが必要です。  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) -で、アプリケーションまたは .NET Standard ライブラリ プロジェクトに埋め込まれた画像ファイルへのリソース識別子が必要です、**ビルド アクション: EmbeddedResource**します。
- [`FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) -イメージ データを提供するストリームが必要です。

[ `Aspect` ](xref:Xamarin.Forms.Image.Aspect)プロパティが表示領域に合わせて、イメージをスケーリングする方法を決定します。

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) -完全かつ正確には、表示領域を塗りつぶすイメージを拡大します。 これにより、イメージ遠近法されている可能性があります。
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) -縦横比を維持しながら、表示領域を塗りつぶすようにイメージをクリップ (ie。 ゆがむことなく)。
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) -レター ボックス (必須) の場合は、イメージ全体のイメージが表示領域に収まるように空白かどうかに応じて境界線の上/下に追加で、イメージは幅または高さ。

イメージは、[ローカルファイル](#local-images)、[埋め込みリソース](#embedded-images)、[ダウンロード](#download-images)、またはストリームから読み込むことができます。 また、`FontImageSource` オブジェクトのフォントアイコンデータを指定することによって、 [`Image`](xref:Xamarin.Forms.Image)ビューでフォントアイコンを表示できます。 詳細については、「[フォント](~/xamarin-forms/user-interface/text/fonts.md)ガイド」の「[フォントアイコンの表示](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons)」を参照してください。

## <a name="local-images"></a>ローカルイメージ

イメージ ファイルを各アプリケーション プロジェクトに追加し、Xamarin.Forms の共有コードから参照します。 このイメージ配布方法は、イメージがプラットフォーム固有の場合に必要になります。例えば、異なるプラットフォームで異なる解像度を使用する場合や、わずかに異なるデザインを使用する場合などです。

すべてのアプリ間で 1 つのイメージを使用する*すべてのプラットフォームで同じファイル名を使用する必要があります*、有効な Android のリソース名を指定する必要があります (つまり。 のみ小文字、数字、アンダー スコア、および期間が許可されている)。

- **iOS** - 管理し、iOS 9 は、使用するために、イメージをサポートする方法を優先**資産カタログの画像セット**、すべてのスケール ファクターのさまざまなデバイスをサポートするために必要なイメージのバージョンを含める必要がありますが、アプリケーション。 詳細については、次を参照してください。[資産カタログ イメージ セットに追加するイメージ](~/ios/app-fundamentals/images-icons/displaying-an-image.md)します。
- **Android** -内のイメージを配置、**リソース/drawable**ディレクトリが**ビルド アクション: AndroidResource**します。 高 DPI と低いバージョンの画像が指定することもできます (で適切に名前付き**リソース**など、サブディレクトリ**ldpi drawable**、 **drawable hdpi**と**drawable xhdpi**)。
- **ユニバーサル Windows プラットフォーム (UWP)** -アプリケーションのルート ディレクトリにイメージを配置**ビルド アクション: コンテンツ**します。

> [!IMPORTANT]
> IOS 9 の場合は、前にイメージが通常の配置、**リソース**フォルダー**ビルド アクション: BundleResource**します。 ただし、apple の iOS アプリで画像の操作には、このメソッドは廃止されました。 詳細については、次を参照してください。[画像のサイズとファイル名](~/ios/app-fundamentals/images-icons/displaying-an-image.md)します。

ファイルの名前付けおよび配置についてこれらの規則に準拠するには、次の XAML を読み込み、すべてのプラットフォーム イメージを表示することができます。

```xaml
<Image Source="waterfront.jpg" />
```

同等の c# コードは次のとおりです。

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

次のスクリーン ショットは、ローカル イメージを表示する各プラットフォームでの結果を表示します。

[![ローカルイメージを表示する サンプルアプリケーション](images-images/local-sml.png)](images-images/local.png#lightbox)

柔軟性を高めるため、`Device.RuntimePlatform`このコード例で示すように、別のイメージ ファイルまたはパスの一部またはすべてのプラットフォームを選択するプロパティを使用できます。

```csharp
image.Source = Device.RuntimePlatform == Device.Android 
                ? ImageSource.FromFile("waterfront.jpg") 
                : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> すべてのプラットフォームで同じイメージ ファイル名を使用するには、名前は、すべてのプラットフォームでは有効である必要があります。 ドローアブルの android では、名前付けに関する制約がある-小文字アルファベット、数字、アンダー スコア、およびピリオドのみが許可されている – とクロス プラットフォームの互換性のためこの従う必要があるその他のすべてのプラットフォームでも。 ファイル名の例**waterfront.png**規則に従いますが、無効なファイル名の例は、"water front.png、""WaterFront.png"、"water-front.png"と"wåterfront.png"。

### <a name="native-resolutions-retina-and-high-dpi"></a>ネイティブの解像度 (retina と高 DPI)

iOS、Android、および UWP には、オペレーティング システムがデバイスの機能に基づいて実行時に適切なイメージを選択、さまざまな画像の解像度のサポートが含まれます。 Xamarin.Forms では、ローカルのイメージを読み込み、ファイルが正しくという名前し、プロジェクト内にある場合に自動的に代替の解像度をサポートに、ネイティブ プラットフォームの Api を使用します。

IOS 9 以降のイメージの管理の推奨される方法は、適切な資産カタログの画像セットに必要な各解像度のイメージをドラッグすることです。 詳細については、次を参照してください。[資産カタログ イメージ セットに追加するイメージ](~/ios/app-fundamentals/images-icons/displaying-an-image.md)します。

Retina のバージョンのイメージは、iOS 9 より前に配置する可能性があります、 **リソース** フォルダー - 2 と 3 回の解像度、 **@2x** または **@3x** (例: ファイル拡張子の前に、ファイル名のサフィックス **myimage@2x.png** ). ただし、apple の iOS アプリで画像の操作には、このメソッドは廃止されました。 詳細については、次を参照してください。[画像のサイズとファイル名](~/ios/app-fundamentals/images-icons/displaying-an-image.md)します。

Android の代替解像度のイメージを配置する必要があります[特別という名前のディレクトリ](https://developer.android.com/guide/practices/screens_support.html)に次のスクリーン ショットに示すように、Android プロジェクトで。

[![Android の複数解像度の画像の場所を](images-images/xs-highdpisolution-sml.png)](images-images/xs-highdpisolution.png#lightbox)

イメージ ファイルの名前を UWP[付くことができます`.scale-xxx`ファイル拡張子の前に](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast)ここで、`xxx`など、資産に適用されるスケーリングの割合をパーセント**myimage.scale 200.png**。 イメージは、コードまたは XAML スケール修飾子を指定せず、だけなどに参照する**myimage.png**します。 プラットフォームでは、ディスプレイの現在の DPI に基づいて最も近い適切な資産のスケールを選択します。

### <a name="additional-controls-that-display-images"></a>画像を表示する追加のコントロール

一部のコントロールでは、イメージを表示するプロパティがあります。

- [`Button`](xref:Xamarin.Forms.Button)には、`Button`に表示されるビットマップイメージに設定できる[`ImageSource`](xref:Xamarin.Forms.Button.ImageSource)のプロパティがあります。 詳細については、「[ボタンを使用したビットマップの使用](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)」を参照してください。
- [`ImageButton`](xref:Xamarin.Forms.Button)には、`ImageButton`に表示するイメージに設定できる[`Source`](xref:Xamarin.Forms.ImageButton.Source)のプロパティがあります。 詳細については、「[イメージソースの設定](~/xamarin-forms/user-interface/imagebutton.md#setting-the-image-source)」を参照してください。
- [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)には、ファイル、埋め込みリソース、URI、またはストリームから読み込まれたイメージに設定できる[`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource)のプロパティがあります。
- [`ImageCell`](xref:Xamarin.Forms.ImageCell)には、ファイル、埋め込みリソース、URI、またはストリームから取得したイメージに設定できる[`ImageSource`](xref:Xamarin.Forms.ImageCell.ImageSource)プロパティがあります。
- [`Page`](xref:Xamarin.Forms.Page). `Page` から派生するすべてのページの種類には[`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource)と[`BackgroundImageSource`](xref:Xamarin.Forms.Page.BackgroundImageSource)のプロパティがあります。このプロパティには、ファイル、埋め込みリソース、URI、またはストリームを割り当てることができます。 場合など、特定の状況で、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)が表示されて、 [ `ContentPage`](xref:Xamarin.Forms.ContentPage)プラットフォームでサポートされている場合、アイコンが表示されます。

  > [!IMPORTANT]
  > Ios では、 [ `Page.IconImageSource` ](xref:Xamarin.Forms.Page.IconImageSource)資産カタログの画像セット内のイメージからプロパティを設定することはできません。 代わりに、ファイル、埋め込みリソース、URI、またはストリームから `Page.IconImageSource` プロパティのアイコンイメージを読み込みます。

## <a name="embedded-images"></a>埋め込み画像

埋め込み画像は (ローカルのイメージ) のようなアプリケーションにも付属しますが、各アプリケーションのファイル構造、イメージ、イメージのコピーではなく、ファイルがリソースとしてアセンブリに埋め込まれました。 イメージの配布には、このメソッドは、各プラットフォームで同じイメージを使用する場合に推奨し、コードにバンドルされているイメージは、コンポーネントの作成に特に適しています。

プロジェクトには、画像を埋め込む、新しい項目を追加し、追加するイメージ/秒を選択して右クリックします。 既定では、イメージに**ビルド アクション: None**; に設定する必要があります**ビルド アクション: EmbeddedResource**します。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![ビルドアクションを埋め込みリソースに設定](images-images/vs-buildaction-sml.png)](images-images/vs-buildaction.png#lightbox)

**ビルド アクション**表示および変更できる、**プロパティ**ファイル ウィンドウ。

この例では、リソース ID は**WorkingWithImages.beach.jpg**します。
IDE によってこの既定値を連結して生成された、**既定 Namespace**ファイル名では、このプロジェクトの各値の間のピリオド (.) を使用します。
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](images-images/xs-buildaction.png "Set Build Action: EmbeddedResource")

**ビルド アクション**表示および変更できるようも、**プロパティ**ファイルのパッド。
このパッドを示しています、**リソース ID**コード内のリソースの参照に使用します。 次のスクリーン ショット、**リソース ID**は**WorkingWithImages.beach.jpg**します。
IDE によってこの既定値を連結して生成された、**既定 Namespace**ファイル名では、このプロジェクトの各値の間のピリオド (.) を使用します。
この ID を編集できます、**プロパティ**パッドがこれらの例の値**WorkingWithImages.beach.jpg**使用されます。

[![埋め込みリソースプロパティパッドの](images-images/xs-embeddedproperties-sml.png)](images-images/xs-embeddedproperties.png#lightbox)

-----

リソース ID では、ピリオド (.) で、フォルダー名も区切られた、プロジェクト内にフォルダーに埋め込まれた画像を配置する場合 移動、 **beach.jpg**という名前のフォルダーにイメージ**MyImages**のリソース ID になる**WorkingWithImages.MyImages.beach.jpg**

埋め込み画像を読み込むためのコードを渡すだけ、**リソース ID**を[ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource*)メソッドを次に示すよう。

```csharp
var embeddedImage = new Image { 
      Source = ImageSource.FromResource(
        "WorkingWithImages.beach.jpg", 
        typeof(EmbeddedImages).GetTypeInfo().Assembly
      ) };
```

> [!NOTE]
> オーバー ロードを使用する必要はユニバーサル Windows プラットフォームでのリリース モードでの埋め込み画像の表示をサポートする`ImageSource.FromResource`のイメージを検索するためのソース アセンブリを指定します。

現在のリソース識別子の暗黙的な変換ではありません。 代わりに、使用する必要があります[ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource*)または`new ResourceImageSource()`埋め込み画像を読み込めません。

次のスクリーン ショットは、埋め込み画像を表示する各プラットフォームでの結果を表示します。

[![埋め込み画像を表示する サンプルアプリケーション](images-images/resource-sml.png)](images-images/resource.png#lightbox)

### <a name="xaml"></a>XAML

組み込み型コンバーターがないため`string`に`ResourceImageSource`、XAML によって、これらの種類のイメージをネイティブに読み込むことができません。 使用してイメージを読み込む代わりに、単純なカスタム XAML マークアップ拡張機能を記述できます、**リソース ID** XAML で指定します。

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
> オーバー ロードを使用する必要はユニバーサル Windows プラットフォームでのリリース モードでの埋め込み画像の表示をサポートする`ImageSource.FromResource`のイメージを検索するためのソース アセンブリを指定します。

この拡張機能を使用するには、追加のカスタム`xmlns`XAML をプロジェクトの名前空間とアセンブリの正しい値を使用します。 この構文を使用して、イメージのソースを設定することができますし:`{local:ImageResource WorkingWithImages.beach.jpg}`します。 完全な XAML の例は、以下に示します。

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

特定のイメージ リソースが読み込まれていない理由を理解しにくい場合があります、ため、次のコードのデバッグが、リソースが正しく構成されていることを確認するためのアプリケーションを一時的に追加できます。 特定のアセンブリに埋め込まれているすべての既知のリソースの出力には、**コンソール**リソース読み込みの問題をデバッグする際にします。

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

既定で、`ImageSource.FromResource`だけメソッドを呼び出すコードと同じアセンブリ内のイメージの検索、`ImageSource.FromResource`メソッド。 どのアセンブリは、変更することで、特定のリソースを含めることが確認できます、デバッグ コードを使用して、`typeof()`ステートメントを`Type`各アセンブリ内にあります。

ただし、ソース アセンブリの埋め込み画像の検索対象をへの引数として指定できます、`ImageSource.FromResource`メソッド。

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

[ `ImageSource.FromUri` ](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri))メソッドが必要です、`Uri`オブジェクト、および新しいを返します[ `UriImageSource` ](xref:Xamarin.Forms.UriImageSource)から読み取る、`Uri`します。

暗黙の変換が、URI 文字列のため、次の例は、動作も。

```csharp
webImage.Source = "https://xamarin.com/content/images/pages/forms/example-app.png";
```

次のスクリーン ショットは、リモート イメージを表示する各プラットフォームでの結果を表示します。

[![ダウンロードしたイメージを表示する サンプルアプリケーション](images-images/download-sml.png)](images-images/download.png#lightbox)

### <a name="downloaded-image-caching"></a>ダウンロードされたイメージのキャッシュ

A [ `UriImageSource` ](xref:Xamarin.Forms.UriImageSource)も、次のプロパティを使用して構成、ダウンロードしたイメージのキャッシュをサポートしています。

- [`CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) のキャッシュが有効なかどうか (`true`既定)。
- [`CacheValidity`](xref:Xamarin.Forms.UriImageSource.CacheValidity) は、`TimeSpan`イメージをローカルに格納するはどのくらいの期間を定義します。

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

関連していないときに、 [ `Image` ](xref:Xamarin.Forms.Image)ビュー、アプリケーションのアイコンとスプラッシュ スクリーンの Xamarin.Forms プロジェクト内のイメージの重要な使用もします。

アイコンとスプラッシュ スクリーン Xamarin.Forms アプリの設定は、各アプリケーション プロジェクトで行われます。 これは、サイズの iOS、Android、および UWP 用のイメージを正しく生成することを意味します。 これらのイメージは、各プラットフォームの要件に従って配置してという名前をする必要があります。

## <a name="icons"></a>アイコン

これらのアプリケーションリソースの作成の詳細については、「IOS での[イメージ](~/ios/app-fundamentals/images-icons/index.md)の使用」、「 [Google Iconography](https://developer.android.com/design/style/iconography.html)」、および「[タイルとアイコンの資産に関する UWP ガイドライン](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)」を参照してください。

また、`FontImageSource` オブジェクトのフォントアイコンデータを指定することによって、 [`Image`](xref:Xamarin.Forms.Image)ビューでフォントアイコンを表示できます。 詳細については、「[フォント](~/xamarin-forms/user-interface/text/fonts.md)ガイド」の「[フォントアイコンの表示](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons)」を参照してください。

## <a name="splash-screens"></a>スプラッシュ スクリーン

IOS と UWP アプリケーションのみ (スタートアップ画面または既定のイメージとも呼ばれます) のスプラッシュ スクリーンが必要です。

ドキュメントを参照してください[iOS イメージを操作](~/ios/app-fundamentals/images-icons/index.md)と[スプラッシュ スクリーン](/windows/uwp/launch-resume/splash-screens/)Windows デベロッパー センターでします。

## <a name="related-links"></a>関連リンク

- [WorkingWithImages (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithimages)
- [iOS 画像の操作](~/ios/app-fundamentals/images-icons/index.md)
- [Android のアイコン](https://developer.android.com/design/style/iconography.html)
- [タイルおよびアイコンのアセットのガイドライン](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
