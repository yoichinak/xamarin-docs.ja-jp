---
title: '第 13 章の概要: ビットマップ'
description: 'Xamarin.Forms でモバイル アプリを作成する: 第 13 章の概要: ビットマップ'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: e4746ed94a008d382ce15bb9cd7c52365d9ba574
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "76725529"
---
# <a name="summary-of-chapter-13-bitmaps"></a>第 13 章の概要: ビットマップ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)

> [!NOTE]
> このページのメモでは、Xamarin.Forms が書籍に記載されている資料と異なる部分が示されています。

Xamarin.Forms の [`Image`](xref:Xamarin.Forms.Image) 要素にはビットマップが表示されます。 すべての Xamarin.Forms プラットフォームは、JPEG、PNG、GIF、および BMP ファイル形式をサポートしています。

Xamarin.Forms のビットマップは、次の 4 つの場所から取得されます。

- URL で指定された Web 経由
- 共有ライブラリにリソースとして埋め込まれている
- プラットフォーム アプリケーション プロジェクトのリソースとして埋め込まれている
- `MemoryStream` など、.NET `Stream` オブジェクトから参照できる任意の場所から

共有ライブラリのビットマップ リソースはプラットフォームに依存しませんが、プラットフォーム プロジェクトのビットマップ リソースはプラットフォーム固有です。

> [!NOTE]
> 本書の説明では、ポータブル クラス ライブラリについて触れますが、これは .NET Standard ライブラリに置き換えられています。 本書のすべてのサンプル コードは、.NET Standard ライブラリを使用するように変換されています。

ビットマップを指定するには、`Image` の [`Source`](xref:Xamarin.Forms.Image.Source) プロパティを、3 つの派生クラスを持つ抽象クラスである [`ImageSource`](xref:Xamarin.Forms.ImageSource) 型のオブジェクトに設定します。

- [`Uri`](xref:Xamarin.Forms.UriImageSource.Uri) プロパティに設定された `Uri` オブジェクトに基づいて、Web 経由でビットマップにアクセスするための [`UriImageSource`](xref:Xamarin.Forms.UriImageSource)
- [`File`](xref:Xamarin.Forms.FileImageSource.File) プロパティに設定されたフォルダーとファイルのパスに基づいて、プラットフォーム アプリケーション プロジェクトに格納されているビットマップにアクセスするための [`FileImageSource`](xref:Xamarin.Forms.FileImageSource)
- [`Stream`](xref:Xamarin.Forms.StreamImageSource.Stream) プロパティに設定された `Func` から `Stream` が返され、それによって指定された .NET `Stream` オブジェクトを使用して、ビットマップを読み込むための [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource)

または (より一般的な方法として)、`ImageSource` クラスの次の静的メソッドを使用できます。これらのいずれからも `ImageSource` オブジェクトが返されます。

- `Uri` オブジェクトに基づいて Web 経由でビットマップにアクセスするための [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri))
- アプリケーション PCL に埋め込みリソースとして格納されているビットマップにアクセスするための [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) (別のソース アセンブリのビットマップにアクセスするには [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Type)) または [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Reflection.Assembly)))
- プラットフォーム アプリケーション プロジェクトからビットマップにアクセスするための [`ImageSource.FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String))
- `Stream` オブジェクトに基づいてビットマップを読み込むための [`ImageSource.FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream}))

`Image.FromResource` メソッドに相当するクラスはありません。 `UriImageSource` クラスは、キャッシュを制御する必要がある場合に役立ちます。 `FileImageSource` クラスは XAML で役立ちます。 `StreamImageSource` は `Stream` オブジェクトの非同期読み込みに、`ImageSource.FromStream` は同期読み込みに役立ちます。

## <a name="platform-independent-bitmaps"></a>プラットフォームに依存しないビットマップ

[**WebBitmapCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) プロジェクトでは、`ImageSource.FromUri` を使用して Web 経由でビットマップを読み込みます。 `Image` 要素は `ContentPage` の `Content` プロパティに設定されているため、ページのサイズに制限されます。 ビットマップのサイズに関係なく、制約付きの `Image` 要素はそのコンテナーのサイズに拡張され、ビットマップの縦横比を維持しながら、`Image` 要素内の最大サイズでビットマップが表示されます。 ビットマップ以外の `Image` の領域は、[`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) を使用して色付けできます。

[**WebBitmapXaml**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) サンプルは似ていますが、単に `Source` プロパティを URL に設定したものです。 変換は [`ImageSourceConverter`](xref:Xamarin.Forms.ImageSourceConverter) クラスによって処理されます。

### <a name="fit-and-fill"></a>自動調整と塗りつぶし

`Image` の [`Aspect`](xref:Xamarin.Forms.Image.Aspect) プロパティを [`Aspect`](xref:Xamarin.Forms.Aspect) 列挙体の次のメンバーのいずれかに設定することで、ビットマップの拡大方法を制御できます。

- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit): 縦横比を維持します (既定値)。
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill): 縦横比を考慮せず、領域全体に拡大します
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill): ビットマップの一部をトリミングすることで、縦横比を維持しながら領域全体に拡大します

### <a name="embedded-resources"></a>埋め込まれたリソース

ビットマップ ファイルを PCL、または PCL のフォルダーに追加できます。 **EmbeddedResource** の **[ビルド アクション]** を指定します。 [**ResourceBitmapCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) サンプルは、`ImageSource.FromResource` を使用してファイルを読み込む方法を示しています。 メソッドに渡されるリソース名は、アセンブリ名、それに続くドット、それに続く省略可能なフォルダー名とドット、それに続くファイル名で構成されます。

プログラムによって、`Image` の `VerticalOptions` および `HorizontalOptions` プロパティが `LayoutOptions.Center` に設定されます。これにより、`Image` 要素が制約なしになります。 `Image` とビットマップは同じサイズです。

- iOS と Android では、`Image` はビットマップのピクセル サイズです。 ビットマップ ピクセルと画面ピクセルの間には 1 対 1 のマッピングがあります。
- ユニバーサル Windows プラットフォームでは、`Image` はデバイスに依存しない単位のビットマップのピクセル サイズです。 ほとんどのデバイスでは、各ビットマップ ピクセルは複数の画面ピクセルを占有します。

[**StackedBitmap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) サンプルでは、XAML で縦の `StackLayout` に `Image` が配置されます。 [`ImageResourceExtension`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) という名前のマークアップ拡張機能は、XAML の埋め込みリソースを参照するために役立ちます。 このクラスでは、それが配置されているアセンブリからのみリソースを読み込むため、ライブラリに配置することはできません。

### <a name="more-on-sizing"></a>サイズ変更の詳細

多くの場合、すべてのプラットフォーム間でビットマップのサイズを統一することをお勧めします。
**StackedBitmap** を試して、縦の `StackLayout` の `Image` 要素に `WidthRequest` を設定して、プラットフォーム間でサイズを統一することができますが、この手法では、サイズを小さくすることしかできません。

また、`HeightRequest` を設定して、プラットフォームで画像サイズを統一することもできますが、ビットマップの幅が制約されているため、この手法の汎用性が制限されます。 縦の `StackLayout` の画像では、`HeightRequest` を使用しないでください。

最適な方法は、デバイスに依存しない単位で電話の幅よりも広いビットマップから始め、デバイスに依存しない単位で `WidthRequest` を目的の幅に設定することです。 これは [**DeviceIndBitmapSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) サンプルで示されています。

[**MadTeaParty**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) を使うと、ルイス・キャロルの『*不思議の国のアリス*』の第 7 章と、ジョン・テニエルのオリジナルの挿絵が表示されます。

[![マッド ティー パーティーのトリプル スクリーンショット](images/ch13fg16-small.png "マッドハッター ティー パーティーの書籍のテキスト")](images/ch13fg16-large.png#lightbox "マッドハッター ティー パーティーの書籍のテキスト")

### <a name="browsing-and-waiting"></a>閲覧と待機

[**ImageBrowser**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) サンプルを使用すると、ユーザーは Xamarin Web サイトに格納されているストック画像を閲覧できます。 .NET [`WebRequest`](xref:System.Net.WebRequest) クラスを使用して、ビットマップの一覧を含む JSON ファイルをダウンロードします。

> [!NOTE]
> Xamarin.Forms プログラムでは、インターネット経由でファイルにアクセスするために、[`WebRequest`](xref:System.Net.WebRequest) ではなく [`HttpClient`](xref:System.Net.Http.HttpClient) を使用する必要があります。

プログラムでは、何かが進行中であることを示すために [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) が使用されます。 各ビットマップが読み込まれているとき、`Image` の読み取り専用の [`IsLoading`](xref:Xamarin.Forms.Image.IsLoading) プロパティは `true` になります。 `IsLoading` プロパティはバインド可能なプロパティによってサポートされているため、そのプロパティが変更されると `PropertyChanged` イベントが発生します。 プログラムによって、このイベントにハンドラーがアタッチされ、`IsLoaded` の現在の設定が使用され、`ActivityIndicator` の [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) プロパティが設定されます。

## <a name="streaming-bitmaps"></a>ストリーミング ビットマップ

`ImageSource.FromStream` メソッドを使うと、.NET `Stream` に基づいて `ImageSource` が作成されます。 メソッドには、`Stream` オブジェクトを返す `Func` オブジェクトを渡す必要があります。

### <a name="accessing-the-streams"></a>ストリームへのアクセス

[**BitmapStreams**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) サンプルは、`ImaageSource.FromStream` メソッドを使用して、埋め込みリソースとして格納されているビットマップを読み込み、Web 経由でビットマップを読み込む方法を示しています。

### <a name="generating-bitmaps-at-run-time"></a>実行時のビットマップの生成

すべての Xamarin.Forms プラットフォームは、非圧縮の BMP ファイル形式をサポートしています。これは、コードで簡単に構築し、`MemoryStream` に格納することができます。 **Xamrin.FormsBook.Toolkit** ライブラリの [`BmpMaker`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) クラスに実装されているように、この手法により、実行時にビットマップをアルゴリズム的に作成できます。

"Do It Yourself" [**DiyGradientBitmap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) サンプルは、`BmpMaker` を使用してグラデーション画像でビットマップを作成する方法を示しています。

## <a name="platform-specific-bitmaps"></a>プラットフォーム固有のビットマップ

すべての Xamarin.Forms プラットフォームでは、プラットフォーム アプリケーション アセンブリにビットマップを格納できます。 Xamarin.Forms アプリケーションによって取得された場合、これらのプラットフォーム ビットマップは [`FileImageSource`](xref:Xamarin.Forms.FileImageSource) 型です。 それらを以下に使用します。

- [`MenuItem`](xref:Xamarin.Forms.MenuItem) の [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) プロパティ
- [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) の [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) プロパティ
- `Button` の [`Image`](xref:Xamarin.Forms.Button) プロパティ

プラットフォーム アセンブリには、アイコンとスプラッシュ スクリーンのビットマップが既に含まれています。

- iOS プロジェクトの **Resources** フォルダー
- Android プロジェクトの **Resources** フォルダーのサブフォルダー
- Windows プロジェクトでは、**Assets** フォルダー内 (ただし、Windows プラットフォームではビットマップはそのフォルダーに限定されません)

[**PlatformBitmaps**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) サンプルでは、コードを使用してプラットフォーム アプリケーション プロジェクトのアイコンを表示します。

### <a name="bitmap-resolutions"></a>ビットマップの解像度

すべてのプラットフォームで、さまざまなデバイス解像度用にビットマップ画像の複数のバージョンを保存できます。 実行時に、画面のデバイス解像度に基づいて適切なバージョンが読み込まれます。

iOS では、これらのビットマップはファイル名のサフィックスによって区別されます。

- 160 DPI デバイスの場合はサフィックスなし (デバイスに依存しない単位に対して 1 ピクセル)
- 320 DPI デバイスの場合は "@2x" のサフィックス (DIU に対して 2 ピクセル)
- 480 DPI デバイスの場合は "@3x" のサフィックス (DIU に対して 3 ピクセル)

1 インチ四方として表示することを目的としたビットマップは、次の 3 つのバージョンに存在します。

- 160 ピクセル四方の MyImage.jpg
- 320 ピクセル四方の MyImage@2x.jpg
- 480 ピクセル四方の MyImage@3x.jpg

このプログラムでは、このビットマップを MyImage.jpg として参照しますが、適切なバージョンは画面の解像度に基づいて実行時に取得されます。 制約なしの場合、ビットマップは常に 160 のデバイスに依存しない単位でレンダリングされます。

Android の場合、ビットマップは **Resources** フォルダーのさまざまなサブフォルダーに格納されます。

- 120 DPI デバイスの場合は drawable-ldpi (低い DPI) (DIU に対して 0.75 ピクセル)
- 160 DPI デバイスの場合は drawable-mdpi (中) (DIU に対して 1 ピクセル)
- 240 DPI デバイスの場合は drawable-hdpi (高い) (DIU に対して 1.5 ピクセル)
- 320 DPI デバイスの場合は drawable-xhdpi (非常に高い) (DIU に対して 2 ピクセル)
- 480 DPI デバイスの場合は drawable-xxhdpi (2 倍の高さ) (DIU に対して 3 ピクセル)
- 640 DPI デバイスの場合は drawable-xxxhdpi (3 倍の高さ) (DIU に対して 4 ピクセル)

1 インチ四方でレンダリングされることを目的としたビットマップの場合、ビットマップのさまざまなバージョンは名前は同じですが、サイズが異なり、これらのフォルダーに格納されます。

- 120 ピクセル四方の drawable-ldpi/MyImage.jpg
- 160 ピクセル四方の drawable-mdpi/MyImage.jpg
- 240 ピクセル四方 drawable-hdpi/MyImage.jpg
- 320 ピクセル四方の drawable-xhdpi/MyImage.jpg
- 480 ピクセル四方の drawable-xxhdpi/MyImage.jpg
- 640 ピクセル四方の drawable-xxxhdpi/MyImage.jpg

ビットマップは常に 160 のデバイスに依存しない単位でレンダリングされます (標準の Xamarin.Forms ソリューション テンプレートには、hdpi、xhdpi、xxhdpi フォルダーのみが含まれます)。

UWP プロジェクトでは、デバイスに依存しない単位ごとのピクセル単位のスケール ファクター (%) で構成されるビットマップ名前付け規則をサポートしています。たとえば、次のとおりです。

- 320 ピクセル四方の MyImage.scale-200.jpg

一部のパーセンテージのみが有効です。 本書のサンプル プログラムには、サフィックスが **scale-200** の画像のみが含まれていますが、現在の Xamarin.Forms ソリューション テンプレートには、**scale-100**、**scale-125**、**scale-150**、**scale-400** が含まれています。

プラットフォーム プロジェクトにビットマップを追加する場合、 **[ビルド アクション]** は次のようになります。

- iOS:**BundleResource**
- Android:**AndroidResource**
- UWP:**コンテンツ**

[**ImageTap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) サンプルでは、`TapGestureRecognizer` がインストールされた `Image` 要素で構成される 2 つのボタンのようなオブジェクトを作成します。 これは、オブジェクトが 1 インチ四方であることが意図されています。 `Image` の `Source` プロパティは、プラットフォーム上の異なる可能性のあるファイル名を参照するために、`OnPlatform` および `On` オブジェクトを使用して設定されます。 ビットマップ画像にはピクセル サイズを示す数字が含まれているため、ビットマップが取得およびレンダリングされるサイズを確認できます。

### <a name="toolbars-and-their-icons"></a>ツールバーとそのアイコン

プラットフォーム固有のビットマップの主な用途の 1 つは、Xamarin.Forms ツールバーです。これを構築するには、[`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) オブジェクトを `Page` で定義された [`ToolbarItems`](xref:Xamarin.Forms.Page.ToolbarItems) コレクションに追加します。 `ToobarItem` は [`MenuItem`](xref:Xamarin.Forms.MenuItem) から派生し、そこからいくつかのプロパティを継承します。

最も重要な `ToolbarItem` プロパティは、次のとおりです。

- プラットフォームと `Order` に応じて表示されるテキストの場合は [`Text`](xref:Xamarin.Forms.MenuItem.Text)
- プラットフォームと `Order` に応じて表示される可能性のある画像の場合は型 `FileImageSource` の [`Icon`](xref:Xamarin.Forms.MenuItem.Icon)
- [`Default`](xref:Xamarin.Forms.ToolbarItemOrder.Default)、[`Primary`](xref:Xamarin.Forms.ToolbarItemOrder.Primary)、および [`Secondary`](xref:Xamarin.Forms.ToolbarItemOrder.Secondary) という 3 つのメンバーを持つ列挙体である型 [`ToolbarItemOrder`](xref:Xamarin.Forms.ToolbarItemOrder) の [`Order`](xref:Xamarin.Forms.ToolbarItem.Order)

`Primary` 項目の数は 3 つまたは 4 つに制限する必要があります。 すべての項目に `Text` 設定を含める必要があります。 ほとんどのプラットフォームでは、`Primary` 項目のみに `Icon` が必要ですが、Windows 8.1 ではすべての項目に `Icon` が必要です。 アイコンは、デバイスに依存しない 32 単位四方にする必要があります。 `FileImageSource` 型は、それらがプラットフォーム固有であることを示します。

`ToolbarItem` を使うと、タップしたときに `Button` のように [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) イベントが発生します。 `ToolbarItem` では、MVVM に関連してよく使用される [`Command`](xref:Xamarin.Forms.MenuItem.Command) および [`CommandParameter`](xref:Xamarin.Forms.MenuItem.CommandParameter) プロパティもサポートされます (「[第 18 章 MVVM](chapter18.md)」を参照してください)。

iOS と Android のいずれでも、ツールバーを表示するページは [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) であるか、`NavigationPage` によってナビゲートされるページである必要があります。 [**ToolbarDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) プログラムによって、`ContentPage` 引数を使用して、`App` クラスの `MainPage` プロパティが [`NavigationPage` コンストラクター](xref:Xamarin.Forms.NavigationPage.%23ctor(Xamarin.Forms.Page))に設定されます。これは、ツール バーの構築とイベント ハンドラーを示すものです。

### <a name="button-images"></a>ボタンの画像

[**ButtonImage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) サンプルに示されているように、プラットフォーム固有のビットマップを使用して、`Button` の [`Image`](xref:Xamarin.Forms.Button.Image) プロパティを、デバイスに依存しない 32 単位四方のビットマップに設定することもできます。

> [!NOTE]
> ボタンでの画像の使用が強化されました。 「[ボタンにビットマップを使用する](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)」を参照してください。

## <a name="related-links"></a>関連リンク

- [第 13 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [第 13 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [イメージの処理](~/xamarin-forms/user-interface/images.md)
- [ボタンにビットマップを使用する](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)
