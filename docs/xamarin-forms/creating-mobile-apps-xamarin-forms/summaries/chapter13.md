---
title: 第 13 章の概要です。 ビットマップ
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 13 章の概要。 ビットマップ'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: e4746ed94a008d382ce15bb9cd7c52365d9ba574
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725529"
---
# <a name="summary-of-chapter-13-bitmaps"></a>第 13 章の概要です。 ビットマップ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)

> [!NOTE]
> このページに関する注意事項は、この本で説明されている内容が Xamarin.Forms が異なっている領域を示しています。

Xamarin. Forms [`Image`](xref:Xamarin.Forms.Image)要素にはビットマップが表示されます。 すべての Xamarin.Forms プラットフォームは、JPEG、PNG、GIF、および BMP ファイル形式をサポートします。

ビットマップを Xamarin.Forms では、4 つの場所から取得されます。

- URL で指定された web 経由で
- 共有ライブラリ内のリソースとして埋め込まれています。
- プラットフォームのアプリケーション プロジェクトにリソースとして埋め込まれて
- .NET `Stream` オブジェクトで参照できる任意の場所から (を含む `MemoryStream`)

共有ライブラリでビットマップ リソースはプラットフォームに依存せず、プラットフォーム プロジェクトでビットマップ リソースがプラットフォームに固有です。

> [!NOTE]
> 書籍のテキストでは、.NET Standard ライブラリに置き換えられているポータブル クラス ライブラリへの参照。 .NET standard ライブラリを使用するブックからのすべてのサンプル コードが変換されました。

ビットマップは、`Image` の[`Source`](xref:Xamarin.Forms.Image.Source)プロパティを[`ImageSource`](xref:Xamarin.Forms.ImageSource)型のオブジェクトに設定することによって指定されます。これは、次の3つの派生クラスを持つ抽象クラスです。

- [`Uri`](xref:Xamarin.Forms.UriImageSource.Uri)プロパティに設定された `Uri` オブジェクトに基づいて、web 経由でビットマップにアクセスするための[`UriImageSource`](xref:Xamarin.Forms.UriImageSource)
- [`File`](xref:Xamarin.Forms.FileImageSource.File)プロパティに設定されたフォルダーおよびファイルパスに基づいてプラットフォームアプリケーションプロジェクトに格納されているビットマップにアクセスするための[`FileImageSource`](xref:Xamarin.Forms.FileImageSource)
- [`Stream`](xref:Xamarin.Forms.StreamImageSource.Stream)プロパティに設定された `Func` から `Stream` を返すことによって指定された .net `Stream` オブジェクトを使用してビットマップを読み込む[`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource)

別の方法として、`ImageSource` クラスの次の静的メソッドを使用することもできます。これらはすべて `ImageSource` オブジェクトを返します。

- `Uri` オブジェクトに基づいて web 上のビットマップにアクセスするための[`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri))
- アプリケーション PCL に埋め込まれたリソースとして格納されているビットマップにアクセスするための[`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) 。別のソースアセンブリのビットマップにアクセスするための[`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Type))または[`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Reflection.Assembly))
- プラットフォームアプリケーションプロジェクトからビットマップにアクセスするための[`ImageSource.FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String))
- `Stream` オブジェクトに基づいてビットマップを読み込むための[`ImageSource.FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream}))

`Image.FromResource` メソッドに相当するクラスはありません。 `UriImageSource` クラスは、キャッシュを制御する必要がある場合に便利です。 `FileImageSource` クラスは、XAML で役に立ちます。 `StreamImageSource` は `Stream` オブジェクトの非同期読み込みに便利ですが、`ImageSource.FromStream` は同期です。

## <a name="platform-independent-bitmaps"></a>プラットフォームに依存しないビットマップ

[**Webbitmapcode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode)プロジェクトは、`ImageSource.FromUri`を使用して web 上にビットマップを読み込みます。 `Image` 要素は `ContentPage`の `Content` プロパティに設定されているため、ページのサイズに制限されます。 ビットマップのサイズに関係なく、制約付きの `Image` 要素はコンテナーのサイズに拡張され、ビットマップの縦横比を維持したまま、`Image` 要素内の最大サイズでビットマップが表示されます。 ビットマップを超えて `Image` の領域は、 [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)で色付けできます。

[**Webbitmapxaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml)サンプルも似ていますが、単純に `Source` プロパティを URL に設定します。 変換は、 [`ImageSourceConverter`](xref:Xamarin.Forms.ImageSourceConverter)クラスによって処理されます。

### <a name="fit-and-fill"></a>調整と塗りつぶし

`Image` の[`Aspect`](xref:Xamarin.Forms.Image.Aspect)プロパティを[`Aspect`](xref:Xamarin.Forms.Aspect)列挙体の次のいずれかのメンバーに設定することにより、ビットマップの拡張方法を制御できます。

- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit): 縦横比を尊重します (既定)。
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill): 塗りつぶし領域、縦横比は考慮されません
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill): 塗りつぶし領域ですが、ビットマップの一部をトリミングすることで達成される縦横比を尊重します。

### <a name="embedded-resources"></a>埋め込みリソース

ビットマップ ファイルは、PCL、または、PCL のフォルダーに追加できます。 **EmbeddedResource**の**ビルドアクション**を指定します。 [**Resourcebitmapcode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode)サンプルは、`ImageSource.FromResource` を使用してファイルを読み込む方法を示しています。 メソッドに渡されるリソース名は、アセンブリ名の後にドットの後に省略可能なフォルダー名とファイル名に続けて、ドットで構成されます。

プログラムによって、`Image` の `VerticalOptions` と `HorizontalOptions` のプロパティが `LayoutOptions.Center`に設定されます。これにより、`Image` 要素に制約が適用されません。 `Image` とビットマップのサイズは同じです。

- IOS と Android では、`Image` はビットマップのピクセルサイズです。 ビットマップのピクセルと画面ピクセルの間の一対一のマッピングがあります。
- ユニバーサル Windows プラットフォームの場合、`Image` は、デバイスに依存しない単位でのビットマップのピクセルサイズです。 ほとんどのデバイスでは、各ビットマップのピクセルは、複数の画面のピクセルを占有します。

[**StackedBitmap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap)サンプルでは、`Image` を XAML の垂直 `StackLayout` に配置します。 [`ImageResourceExtension`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs)という名前のマークアップ拡張機能は、XAML の埋め込みリソースを参照するのに役立ちます。 このクラスはのみ、そのアセンブリからリソースを読み込みますが配置されている、ため、ライブラリ内に配置することはできません。

### <a name="more-on-sizing"></a>詳細については、サイズ変更

多くの場合、サイズのビットマップのすべてのプラットフォーム間で一貫する必要があります。
**StackedBitmap**を試してみると、`Image` 要素の `WidthRequest` を垂直 `StackLayout` に設定して、プラットフォーム間でサイズを統一することができますが、この方法を使用してサイズを減らすことしかできません。

また、`HeightRequest` を設定して、イメージのサイズをプラットフォームに合わせて調整することもできますが、ビットマップの制限幅によって、この手法の汎用性が制限されます。 縦 `StackLayout`の画像の場合、`HeightRequest` を避ける必要があります。

最適な方法は、デバイスに依存しない単位で電話の幅よりも大きいビットマップを使用し、デバイスに依存しない単位で `WidthRequest` を目的の幅に設定することです。 これは、 [**Deviceindbitmapsize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize)サンプルに示されています。

[**MadTeaParty**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty)は、Wonderland におけるルイス Carroll の*Alice の冒険*の第7章を、John が元の図と一緒に表示します。

[![Mad ティーパーティーのトリプルスクリーンショット](images/ch13fg16-small.png "Mad Hatters 茶パーティーの書籍のテキスト")](images/ch13fg16-large.png#lightbox "Mad Hatters 茶パーティーの書籍のテキスト")

### <a name="browsing-and-waiting"></a>参照して、待機しています

[**Imagebrowser**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser)サンプルを使用すると、ユーザーは Xamarin web サイトに格納されているストックイメージを参照できます。 .NET [`WebRequest`](xref:System.Net.WebRequest)クラスを使用して、ビットマップの一覧を含む JSON ファイルをダウンロードします。

> [!NOTE]
> Xamarin. Forms プログラムは、インターネット経由でファイルにアクセスするために[`WebRequest`](xref:System.Net.WebRequest)ではなく[`HttpClient`](xref:System.Net.Http.HttpClient)を使用する必要があります。

プログラムは、何かが起こっていることを示すために[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)を使用します。 各ビットマップが読み込まれると、`Image` の読み取り専用の[`IsLoading`](xref:Xamarin.Forms.Image.IsLoading)プロパティが `true`ます。 `IsLoading` プロパティは、バインド可能なプロパティによってサポートされているため、プロパティが変更されたときに `PropertyChanged` イベントが発生します。 プログラムは、このイベントにハンドラーをアタッチし、`IsLoaded` の現在の設定を使用して `ActivityIndicator`の[`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning)プロパティを設定します。

## <a name="streaming-bitmaps"></a>ビットマップのストリーミング

`ImageSource.FromStream` メソッドは、.NET `Stream`に基づいて `ImageSource` を作成します。 メソッドには、`Stream` オブジェクトを返す `Func` オブジェクトを渡す必要があります。

### <a name="accessing-the-streams"></a>ストリームへのアクセス

[**Bitmapstreams**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams)サンプルでは、`ImaageSource.FromStream` メソッドを使用して、埋め込みリソースとして格納されているビットマップを読み込んで、web 上にビットマップを読み込む方法を示します。

### <a name="generating-bitmaps-at-run-time"></a>実行時にビットマップを生成します。

すべての Xamarin. Forms プラットフォームは、圧縮されていない BMP ファイル形式をサポートしています。これは、コードで簡単に作成し、`MemoryStream`に格納することができます。 この手法を使用する**と、アルゴリズムライブラリの** [`BmpMaker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)クラスに実装されているように、実行時にビットマップを作成できます。

"自分で実行する" [**DiyGradientBitmap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap)サンプルは、`BmpMaker` を使用して、グラデーションイメージでビットマップを作成する方法を示しています。

## <a name="platform-specific-bitmaps"></a>プラットフォーム固有のビットマップ

すべての Xamarin.Forms プラットフォームは、プラットフォームのアプリケーション アセンブリにビットマップを格納するを許可します。 Xamarin. Forms アプリケーションによって取得されると、これらのプラットフォームビットマップは[`FileImageSource`](xref:Xamarin.Forms.FileImageSource)型になります。 使用すること。

- の[`Icon`](xref:Xamarin.Forms.MenuItem.Icon)プロパティ[`MenuItem`](xref:Xamarin.Forms.MenuItem)
- の[`Icon`](xref:Xamarin.Forms.MenuItem.Icon)プロパティ[`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)
- の[`Image`](xref:Xamarin.Forms.Button)プロパティ `Button`

プラットフォーム アセンブリには、アイコンとスプラッシュ スクリーン用のビットマップには既にが含まれます。

- IOS プロジェクトの**Resources**フォルダー
- Android プロジェクトでは、 **Resources**フォルダーのサブフォルダーにあります。
- Windows プロジェクトでは、 **Assets**フォルダーにあります (ただし、windows プラットフォームではビットマップがそのフォルダーに制限されません)。

[**Platformbitmaps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps)サンプルでは、コードを使用して、プラットフォームアプリケーションプロジェクトのアイコンを表示します。

### <a name="bitmap-resolutions"></a>ビットマップの解決策

すべてのプラットフォームでは、複数のバージョンの別のデバイスの解像度のビットマップ イメージの保存を許可します。 実行時に適切なバージョンがデバイスの画面の解像度に基づいて読み込まれます。

Ios では、これらのビットマップは、ファイル名にサフィックスによって区別されます。

- 160 DPI デバイス (1 ピクセル デバイスに依存しない単位) のサフィックスなし
- 320 DPI デバイスの '@2x' サフィックス (2 ピクセル x DIU)
- 480 DPI デバイスの '@3x' サフィックス (3 ピクセル x DIU)

次の 3 つのバージョンでは、1 インチの四角形として表示するためのビットマップが存在します。

- 160 ピクセルの正方形に MyImage.jpg
- 320ピクセル四方の MyImage@2x.jpg
- 480ピクセル四方の MyImage@3x.jpg

このビットマップとして MyImage.jpg を参照して、プログラムが、画面の解像度に基づいて実行時に適切なバージョンが取得されます。 制約なし、ときに、ビットマップは 160 のデバイスに依存しない単位で常に表示されます。

Android の場合、ビットマップは**Resources**フォルダーのさまざまなサブフォルダーに格納されます。

- drawable-ldpi (低 DPI) 120 DPI デバイス (0.75 の単位は DIU をピクセル単位)
- drawable-mdpi (中) の 160 DPI デバイス (1 ピクセル、単位は DIU を)
- drawable-hdpi (高) の 240 DPI デバイス (1.5 の単位は DIU をピクセル単位)
- drawable-xhdpi (超高) 320 DPI デバイス (2 の単位は DIU をピクセル単位)
- drawable-xxhdpi (場合によっては超高余分な) 480 DPI デバイス (、単位は DIU に 3 つのピクセル単位)
- drawable-xxxhdpi (3 つの余分な高値) 640 DPI デバイス (4 ピクセル、単位は DIU を)

ビットマップのさまざまなバージョンは正方形の 1 インチで表示するためのもので、ビットマップの名前が同じで別のサイズになり、これらのフォルダーに格納します。

- drawable-ldpi/MyImage.jpg 120 ピクセルの正方形に
- drawable-mdpi/MyImage.jpg 160 ピクセルの正方形に
- drawable-hdpi/MyImage.jpg 240 ピクセルの正方形に
- drawable-xhdpi/MyImage.jpg 320 ピクセルの正方形で
- drawable-xxhdpi/MyImage.jpg 480 ピクセルの正方形に
- drawable-xxxhdpi/MyImage.jpg 640 ピクセルの正方形で

ビットマップは、160 のデバイスに依存しない単位で常に表示されます。 (標準の Xamarin.Forms ソリューション テンプレートのみを含む hdpi、xhdpi、および xxhdpi フォルダー)

UWP プロジェクトには、ビットマップの名前付けスキームで構成されているデバイスに依存しない単位あたりのピクセル単位でスケール ファクターのパーセンテージなどがサポートされています。

- MyImage.scale-200.jpg 320 ピクセルの正方形で

いくつかのパーセンテージは有効です。 このブックのサンプルプログラムには、**スケール 200**のサフィックスを持つイメージのみが含まれていますが、現在の Xamarin. Forms ソリューションテンプレートには、**スケール-100**、**スケール-125**、**スケール-150**、および**スケール-400**が含まれています。

プラットフォームプロジェクトにビットマップを追加する場合、**ビルドアクション**は次のようになります。

- iOS: **BundleResource**
- Android: **Androidresource**
- UWP:**コンテンツ**

[**Imagetap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap)サンプルでは、`TapGestureRecognizer` がインストールされた `Image` 要素で構成される、ボタンに似た2つのオブジェクトを作成します。 オブジェクトが 1 インチの四角形をすることが目的です。 `Image` の `Source` プロパティは `OnPlatform` および `On` オブジェクトを使用して設定され、プラットフォーム上の異なるファイル名を参照します。 ビットマップ イメージには、どのサイズ ビットマップが取得され、表示を表示できるように、ピクセル サイズを示す番号が含まれます。

### <a name="toolbars-and-their-icons"></a>ツールバーとアイコン

プラットフォーム固有のビットマップの主な用途の1つは、`Page`で定義された[`ToolbarItems`](xref:Xamarin.Forms.Page.ToolbarItems)コレクションに[`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)オブジェクトを追加することによって構築される Xamarin. Forms ツールバーです。 `ToobarItem` は、一部のプロパティを継承する[`MenuItem`](xref:Xamarin.Forms.MenuItem)から派生します。

最も重要な `ToolbarItem` プロパティは次のとおりです。

- プラットフォームおよび `Order` によって表示される可能性があるテキストの[`Text`](xref:Xamarin.Forms.MenuItem.Text)
- プラットフォームと `Order` によって表示されるイメージの `FileImageSource` 型の[`Icon`](xref:Xamarin.Forms.MenuItem.Icon)
- [`ToolbarItemOrder`](xref:Xamarin.Forms.ToolbarItemOrder)型の[`Order`](xref:Xamarin.Forms.ToolbarItem.Order) 、 [`Default`](xref:Xamarin.Forms.ToolbarItemOrder.Default)、 [`Primary`](xref:Xamarin.Forms.ToolbarItemOrder.Primary)、および[`Secondary`](xref:Xamarin.Forms.ToolbarItemOrder.Secondary)の3つのメンバーを持つ列挙体です。

`Primary` 項目の数は 3 ~ 4 に制限する必要があります。 すべての項目に対して `Text` 設定を含める必要があります。 ほとんどのプラットフォームでは、`Primary` 項目だけが `Icon` を必要としますが、Windows 8.1 にはすべての項目に対して `Icon` が必要です。 32 のデバイスに依存しない単位正方形のアイコンがあります。 `FileImageSource` 型は、それらがプラットフォーム固有であることを示します。

`ToolbarItem` は、`Button`のように、タップしたときに[`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked)イベントを発生させます。 `ToolbarItem` では、MVVM との接続によく使用される[`Command`](xref:Xamarin.Forms.MenuItem.Command)と[`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter)プロパティもサポートしています。 ( [MVVM の章を](chapter18.md)参照してください)。

IOS と Android のどちらでも、ツールバーを表示するページが[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)か、`NavigationPage`によって移動されるページである必要があります。 [**Toolbardemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo)プログラムは、`App` クラスの `MainPage` プロパティを、`ContentPage` 引数を使用して[`NavigationPage` コンストラクター](xref:Xamarin.Forms.NavigationPage.%23ctor(Xamarin.Forms.Page))に設定し、ツールバーの構築とイベントハンドラーを示します。

### <a name="button-images"></a>ボタンのイメージ

また、 [**Buttonimage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage)サンプルで示すように、プラットフォーム固有のビットマップを使用して、`Button` の[`Image`](xref:Xamarin.Forms.Button.Image)プロパティを、デバイスに依存しない32の正方形のビットマップに設定することもできます。

> [!NOTE]
> ボタンのイメージの使用が強化されました。 「[ボタンを使用したビットマップの使用」を](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)参照してください。

## <a name="related-links"></a>関連リンク

- [第13章フルテキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [第13章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [イメージの処理](~/xamarin-forms/user-interface/images.md)
- [ボタンを使用したビットマップの使用](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)
