---
title: "イメージ"
description: "イメージは、Xamarin.Forms を使用したプラットフォーム間で共有することができます、具体的には、各プラットフォーム用に読み込むことができる、または表示をダウンロードすることができます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 440ee997b075b5c89504dcf20171fa3c8713e1ce
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="images"></a>イメージ

_イメージは、Xamarin.Forms を使用したプラットフォーム間で共有することができます、具体的には、各プラットフォーム用に読み込むことができる、または表示をダウンロードすることができます。_

イメージは、アプリケーションのナビゲーション、使いやすさ、およびブランド化の重要な部分です。 Xamarin.Forms アプリケーションは、各プラットフォームでも可能性があるさまざまなイメージを表示では、すべてのプラットフォームでイメージを共有できる必要があります。

プラットフォーム固有のイメージものアイコンとスプラッシュ スクリーン; 必要です。これらは、プラットフォームごとに構成する必要があります。

このドキュメントでは、次のトピックについて説明します。

- [ **ローカル イメージ**](#Local_Images) -iOS Retina または Android の高 DPI バージョンの画像のようなネイティブの解像度の解決を含め、アプリケーション付属のイメージを表示します。
- [ **埋め込み画像**](#Embedded_Images) -アセンブリ リソースとして埋め込まれた画像を表示します。
- [ **イメージをダウンロード**](#Downloading_Images) - ダウンロードし、イメージを表示します。
- [ **アイコンとスプラッシュ スクリーン**](#Icons_and_splashscreens) -プラットフォーム固有のアイコンとスタートアップ イメージ。

## <a name="displaying-images"></a>イメージを表示します。

Xamarin.Forms を使用して、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)画像を表示するページに表示します。 2 つの重要なプロパティがあります。

- [`Source`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) - [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/)インスタンス、ファイル、Uri またはリソースで、表示するイメージを設定します。
- [`Aspect`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) -方法が (stretch、トリミングまたはレター ボックスするかどうか) 内で表示されている境界内の画像のサイズを変更します。

[`ImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) インスタンスは、イメージ ソースの種類ごとに静的メソッドを使用して取得できます。

- [`FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) -ファイル名または各プラットフォームで解決可能なファイル パスが必要です。
- [`FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) -たとえば、Uri オブジェクトが必要です。  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) -アプリケーションまたは、PCL に埋め込む画像ファイルへのリソース識別子が必要です、**ビルド アクション: EmbeddedResource**です。
- [`FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) -イメージ データを提供するストリームが必要です。

[ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/)プロパティは、表示領域に合わせて、イメージをスケーリングする方法を決定します。

- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.Fill/) -完全かつ正確には、表示領域を占めるように画像を拡大します。 これにより、ゆがんで表示されるイメージ可能性があります。
- [`AspectFill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFill/) -を縦横比を維持しながら、表示領域を占めるよう、イメージをクリップ (ie。 ゆがむことなく)。
- [`AspectFit`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFit/) -Letterboxes (必要な場合) イメージ全体のイメージが表示領域に収まるように、かどうかに応じて境界線の上/下に追加される空白の領域で、イメージは幅または高さ。

イメージを読み込むことが、[ローカルファイル](#Local_Images_in_Xaml)、[埋め込みリソース](#embedded_images)、または[ダウンロード](#Downloading_Images)です。

<a name="Local_Images" />

## <a name="local-images"></a>ローカルの画像

イメージ ファイルを各アプリケーション プロジェクトに追加し、Xamarin.Forms 共有コードから参照されていることができます。 すべてのアプリ間で 1 つのイメージを使用する*すべてのプラットフォームで同じファイル名を使用する必要があります*、有効な Android リソース名を指定する必要があります (ie。 小文字のみ、数字、アンダー スコア、および期間が許可されている)。

- **iOS** - 管理し、iOS 9 は、使用するために、イメージをサポートする方法を優先**アセット カタログのイメージ セット**、すべてのさまざまなデバイスをサポートし、規模の要素に必要なイメージのバージョンを含める必要がありますが、アプリケーション。 詳細については、次を参照してください。[資産カタログ イメージ セットに追加するイメージ](~/ios/app-fundamentals/images-icons/displaying-an-image.md)です。
- **Android** -内のイメージを配置、**リソース/描画**ディレクトリが**ビルド アクション: AndroidResource**です。 高 DPI と低のバージョンの画像も提供されます (で適切に名前付き**リソース**などサブディレクトリ**ドロウアブル ldpi**、**ドロウアブル hdpi**、および**ドロウアブル xhdpi**)。
- **Windows Phone** -アプリケーションのルート ディレクトリでイメージを配置**ビルド アクション: コンテンツ**です。
- **ユニバーサル Windows プラットフォーム (UWP)** -アプリケーションのルート ディレクトリでイメージを配置**ビルド アクション: コンテンツ**です。

> [!IMPORTANT]
> IOS 9 の場合は、前にイメージ通常に配置されていた、**リソース**フォルダー**ビルド アクション: BundleResource**です。 ただし、apple の iOS アプリで画像の操作には、このメソッドは廃止されました。 詳細については、次を参照してください。[イメージのサイズとファイル名](~/ios/app-fundamentals/images-icons/displaying-an-image.md)です。

ファイルの名前付けおよび配置についてこれらの規則に準拠するには、次の XAML を読み込むし、すべてのプラットフォームでイメージを表示することができます。

```xaml
<Image Source="waterfront.jpg" />
```

同等の c# コードは次のとおりです。

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

次のスクリーン ショットは、ローカルのイメージを表示する各プラットフォームでの結果を表示します。

[![ローカルの ImageSource](images-images/local-sml.png "サンプル アプリケーションがローカルのイメージを表示する")](images-images/local.png#lightbox "サンプル アプリケーションがローカルのイメージを表示します。")

柔軟性を高めるため、`Device.RuntimePlatform`このコード例で示すように、別のイメージ ファイルまたはパスの一部またはすべてのプラットフォームを選択するプロパティを使用できます。

```csharp
image.Source = Device.RuntimePlatform == Device.Android ? ImageSource.FromFile("waterfront.jpg") : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> すべてのプラットフォームで同じイメージ ファイル名を使用するには、名前はすべてのプラットフォームで有効にする必要があります。 Android ドロウアブル名前付けの制限 – 小文字のアルファベット、数字、アンダー スコア、およびピリオドのみが許可されている – 持ちプラットフォーム間の互換性のためこの従う必要がありますの他のすべてのプラットフォームですぎます。 ファイル名の例**waterfront.png**次の手順では、ルールが無効なファイル名の例は、"水 front.png、""WaterFront.png"を含む"水-front.png"と"wåterfront.png"です。

<a name="Native_Resolutions" />

### <a name="native-resolutions-retina-and-high-dpi"></a>ネイティブの解像度 (Retina と高 DPI)

IOS と Android プラットフォームの両方には、別の画像の解像度、オペレーティング システムが実行時に、デバイスの機能に基づく適切なイメージを選択のサポートが含まれます。 Xamarin.Forms は、場合は、ファイルが正しくという名前し、プロジェクト内に別の解像度を自動的にサポートするために、ローカルのイメージを読み込むためのネイティブ プラットフォームの Api を使用します。

IOS 9 以降のイメージを管理することをお勧め適切な資産カタログ イメージ セットに必要な各解像度のイメージのドラッグを開始します。 詳細については、次を参照してください。[資産カタログ イメージ セットに追加するイメージ](~/ios/app-fundamentals/images-icons/displaying-an-image.md)です。

IOS 9 の場合は前に、retina バージョンのイメージを配置する可能性があります、**リソース**フォルダー - 2 と 3 回の解像度、  **@2x** または **@3x** (ファイル拡張子の前にファイル名のサフィックス **myimage@2x.png**). ただし、apple の iOS アプリで画像の操作には、このメソッドは廃止されました。 詳細については、次を参照してください。[イメージのサイズとファイル名](~/ios/app-fundamentals/images-icons/displaying-an-image.md)です。

Android の代替解像度のイメージを配置する必要があります[特別にという名前のディレクトリ](http://developer.android.com/guide/practices/screens_support.html)Android プロジェクトで、次のスクリーン ショットに示すようにします。

[![Android の複数の解像度のイメージの場所](images-images/xs-highdpisolution-sml.png "Android の複数の解像度のイメージの場所")](images-images/xs-highdpisolution.png#lightbox "Android の複数の解像度のイメージの場所")

### <a name="additional-controls-that-display-images"></a>その他のコントロール イメージを表示します。

一部のコントロールでは、イメージを表示するプロパティがあります。

- [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) -すべてのページから派生する型`Page`が[ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/)と[ `BackgroundImage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.BackgroundImage/)プロパティで、ローカル ファイルの参照を割り当てることができます。 場合など、特定の状況で、 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)が表示されて、 [ `ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)プラットフォームでサポートされている場合、アイコンが表示されます。

  > [!IMPORTANT]
  > Ios の場合、 [ `Page.Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/)アセット カタログのイメージ セット内のイメージからプロパティを設定することはできません。 代わりのアイコン イメージを読み込む、`Page.Icon`プロパティから、**リソース**iOS プロジェクト フォルダーにします。

- [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) は、 [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/)ローカル ファイルの参照を設定できるプロパティです。
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) は、 [ `ImageSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ImageCell.ImageSource/)をイメージに設定できるプロパティですが、ローカル ファイル、埋め込みリソース、または URI から取得します。

<a name="embedded_images" />

## <a name="embedded-images"></a>[埋め込み画像]

埋め込み画像も付属していますが (ローカル イメージの場合) のようなアプリケーションが、各アプリケーションのファイルの構造、イメージのイメージのコピーではなく、ファイルがリソースとしてアセンブリに埋め込まれました。 イメージを配布するには、このメソッドは、イメージがコードにバンドルされているコンポーネントの作成に特に適しています。

プロジェクトで画像を埋め込むには、新しい項目を追加したり、追加する画像/秒を選択して右クリックします。 既定では、イメージになります**ビルド アクション: None**; に設定する必要があります**ビルド アクション: EmbeddedResource**です。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](images-images/vs-buildaction.png "ビルド アクションを設定します埋め込まれたリソース。")

**ビルド アクション**表示および変更、**プロパティ**ファイルのウィンドウ。

この例では、リソース ID は**WorkingWithImages.beach.jpg**です。
IDE によってこの既定値を連結して生成された、**既定 Namespace**ファイル名でこのプロジェクトの各値の間のピリオド (.) を使用します。
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](images-images/xs-buildaction.png "ビルド アクションを設定します埋め込まれたリソース。")

**ビルド アクション**も表示および変更、**プロパティ**ファイルのパッド。
このパッドを示しています、**リソース ID**コード内のリソースを参照するためです。 以下のスクリーン ショットで、**リソース ID**は**WorkingWithImages.beach.jpg**です。
IDE によってこの既定値を連結して生成された、**既定 Namespace**ファイル名でこのプロジェクトの各値の間のピリオド (.) を使用します。
この ID を編集することができます、**プロパティ**パッドがこれらの例の値**WorkingWithImages.beach.jpg**使用されます。

![](images-images/xs-embeddedproperties.png "埋め込まれたリソース プロパティ パッド")

-----

フォルダー名がリソース ID でピリオド (.) で区切られても、プロジェクト内のフォルダーに埋め込み画像を配置する場合 移動、 **beach.jpg**という名前のフォルダーにイメージ**により**のリソース ID になる**WorkingWithImages.MyImages.beach.jpg**

埋め込み画像を読み込むようにコードを渡すだけ、**リソース ID**を[ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/)メソッドを次のように。

```csharp
var embeddedImage = new Image { Source = ImageSource.FromResource("WorkingWithImages.beach.jpg") };
```

現在リソース識別子の暗黙的な変換はありません。 代わりに、使用する必要があります[ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/)または`new ResourceImageSource()`埋め込み画像を読み込めません。

次のスクリーン ショットは、各プラットフォームで埋め込み画像を表示する結果を表示します。

[![ResourceImageSource](images-images/resource-sml.png "サンプル アプリケーションの埋め込み画像を表示する")](images-images/resource.png#lightbox "サンプル アプリケーションの埋め込み画像を表示します。")

<a name="Embedded_Images_in_Xaml" />

### <a name="using-xaml"></a>XAML を使用してください。

組み込み型コンバーターが存在しないため`string`に`ResourceImageSource`、これらの種類のイメージは XAML でネイティブに読み込むことができません。 単純なカスタム XAML マークアップ拡張機能を記述してを使用してイメージを読み込む代わりに、**リソース ID** XAML で指定します。

```csharp
[ContentProperty ("Source")]
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
   var imageSource = ImageSource.FromResource(Source);

   return imageSource;
 }
}
```

この拡張機能を使用するには、追加のカスタム`xmlns`XAML をプロジェクトの名前空間とアセンブリの正しい値を使用します。 この構文を使用して、イメージ ソースを設定することができますし:`{local:ImageResource WorkingWithImages.beach.jpg}`です。 XAML の完全な例を次に示します。

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

### <a name="troubleshooting-embedded-images"></a>埋め込み画像のトラブルシューティング

<a name="Debugging_Embedded_Images" />

#### <a name="debugging-code"></a>コードのデバッグ

特定のイメージ リソースが読み込まれていない理由を理解しにくい場合があります、ため、次のデバッグ コードが、リソースが正しく構成されていることを確認するために、アプリケーションを一時的に追加できます。 特定のアセンブリに埋め込まれた認識されているすべてのリソースを出力は、<span class="UIItem">コンソール</span>リソース読み込みの問題をデバッグする際にします。

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

#### <a name="images-embedded-in-other-projects-dont-appear"></a>他のプロジェクトに埋め込まれた画像が表示されません。

`Image.FromResource` コードの呼び出しと同じアセンブリ内のイメージだけを検索`FromResource`です。 対象のアセンブリが変更することによって、特定のリソースを含むを判断できます上記デバッグ コードを使用して、`typeof()`ステートメントを`Type`各アセンブリ内にあります。

<a name="Downloading_Images" />

## <a name="downloading-images"></a>イメージをダウンロードします。

イメージに自動的にダウンロードできます、表示のため次の XAML で示すように。

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
var webImage = new Image { Source = ImageSource.FromUri(new Uri("https://xamarin.com/content/images/pages/forms/example-app.png")) };
```

[ `ImageSource.FromUri` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/)メソッドが必要な`Uri`オブジェクト、および新しいを返します[ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/)から読み取る、`Uri`です。

暗黙の変換が URI 文字列のため、次の例はでも機能します。

```csharp
webImage.Source = "https://xamarin.com/content/images/pages/forms/example-app.png";
```

次のスクリーン ショットは、各プラットフォームでのリモート イメージを表示する結果を表示します。

[![ダウンロード ImageSource](images-images/download-sml.png "サンプル アプリケーションをダウンロードしたイメージを表示する")](images-images/download.png#lightbox "サンプル アプリケーションをダウンロードしたイメージを表示します。")

<a name="Image_Caching" />

### <a name="downloaded-image-caching"></a>ダウンロードしたイメージのキャッシュ

A [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/)も次のプロパティを通じて、構成、ダウンロードしたイメージのキャッシュをサポートします。

- [`CachingEnabled`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CachingEnabled/) にキャッシュが有効するかどうか (`true`既定)。
- [`CacheValidity`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CacheValidity/) A`TimeSpan`イメージをローカルに保存はどのくらいの期間を定義します。

キャッシュは既定で有効にし、24 時間、ローカルにイメージを格納します。 特定のイメージのキャッシュを無効にするには、次のように イメージ ソースをインスタンス化します。

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri="http://server.com/image" };
```

特定のキャッシュ期間 (たとえば、5 日) を設定するには、次のように 画像のソースをインスタンス化します。

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://xamarin.com/content/images/pages/forms/example-app.png"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

組み込みのキャッシュとで各セルにイメージを設定 (またはできるバインド) イメージの一覧をスクロールするようなシナリオをサポートし、組み込みキャッシュのセルがビューにスクロールするときに、イメージを再読み込みを処理できるように非常に簡単です。

<a name="Icons_and_splashscreens" />

## <a name="icons-and-splashscreens"></a>アイコンとスプラッシュ スクリーン

関連していないときに、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)ビュー、アプリケーションのアイコンとスプラッシュ スクリーンも Xamarin.Forms プロジェクト内のイメージの重要な使い方として。

アイコンと Xamarin.Forms のアプリのスプラッシュ スクリーンの設定は、各アプリケーション プロジェクトで行われます。 これは、iOS、Android、および UWP 用のイメージ サイズが正しく生成することを意味します。 これらのイメージは、という名前し、各プラットフォームの要件に従って配置する必要があります。

## <a name="icons"></a>アイコン

参照してください、 [iOS イメージを操作](~/ios/app-fundamentals/images-icons/index.md)、 [Google 物](http://developer.android.com/design/style/iconography.html)、および[タイルおよびアイコン アセットに関するガイドライン](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)これらのアプリケーション リソースを作成する方法の詳細についてです。

## <a name="splashscreens"></a>スプラッシュ スクリーン

IOS および UWP アプリケーションのみ (スタートアップ画面または既定のイメージとも呼ばれます)、スプラッシュ スクリーンが必要です。

マニュアルを参照してください[iOS イメージを操作](~/ios/app-fundamentals/images-icons/index.md)と[スプラッシュ スクリーン](/windows/uwp/launch-resume/splash-screens/)Windows デベロッパー センターにします。

## <a name="summary"></a>まとめ

Xamarin.Forms では、さまざまなプラットフォーム間で使用する同じイメージまたはプラットフォーム固有のイメージを指定するを許可する、クロスプラット フォームのアプリケーションに画像を挿入するさまざまな方法が用意されています。 ダウンロードしたイメージも自動的にキャッシュされる一般的なコーディングのシナリオを自動化します。

アプリケーションのアイコンとスプラッシュ スクリーンのイメージはセットアップ、プラットフォーム固有のアプリに使用される同じガイダンスに従う Xamarin.Forms 以外のアプリケーションの場合と構成


## <a name="related-links"></a>関連リンク

- [WorkingWithImages (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/)
- [iOS イメージの操作](~/ios/app-fundamentals/images-icons/index.md)
- [Android のアイコン](http://developer.android.com/design/style/iconography.html)
- [タイルおよびアイコン アセットに関するガイドライン](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
