---
title: 第 13 章の概要です。 ビットマップ
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 13 章の概要。 ビットマップ'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: charlespetzold
ms.author: chape
ms.date: 07/18/2018
ms.openlocfilehash: d863ce1c6195ddaef164c3a15817a4ff87a3c332
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156627"
---
# <a name="summary-of-chapter-13-bitmaps"></a>第 13 章の概要です。 ビットマップ

> [!NOTE] 
> このページに関する注意事項は、この本で説明されている内容が Xamarin.Forms が異なっている領域を示しています。

Xamarin.Forms [ `Image` ](xref:Xamarin.Forms.Image)要素には、ビットマップが表示されます。 すべての Xamarin.Forms プラットフォームは、JPEG、PNG、GIF、および BMP ファイル形式をサポートします。

ビットマップを Xamarin.Forms では、4 つの場所から取得されます。

- URL で指定された web 経由で
- 共有ライブラリ内のリソースとして埋め込まれています。
- プラットフォームのアプリケーション プロジェクトにリソースとして埋め込まれて
- .NET で参照できる任意の場所から`Stream`を含むオブジェクト。 `MemoryStream`

共有ライブラリでビットマップ リソースはプラットフォームに依存せず、プラットフォーム プロジェクトでビットマップ リソースがプラットフォームに固有です。

> [!NOTE] 
> 書籍のテキストでは、.NET Standard ライブラリに置き換えられているポータブル クラス ライブラリへの参照。 .NET standard ライブラリを使用するブックからのすべてのサンプル コードが変換されました。

設定して、ビットマップが指定されて、 [ `Source` ](xref:Xamarin.Forms.Image.Source)プロパティの`Image`型のオブジェクトに[ `ImageSource` ](xref:Xamarin.Forms.ImageSource)、3 つの派生クラスで抽象クラス。

- [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) 基づく web 経由でビットマップにアクセスするため、`Uri`オブジェクトに設定してその[ `Uri` ](xref:Xamarin.Forms.UriImageSource.Uri)プロパティ
- [`FileImageSource`](xref:Xamarin.Forms.FileImageSource) プラットフォームのアプリケーション プロジェクトに格納されているビットマップにアクセスするために設定フォルダーとファイル パスに基づくその[ `File` ](xref:Xamarin.Forms.FileImageSource.File)プロパティ
- [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource) .NET を使用してビットマップを読み込むため`Stream`を返すことによって指定されたオブジェクトを`Stream`から、`Func`に設定、 [ `Stream` ](xref:Xamarin.Forms.StreamImageSource.Stream)プロパティ

または (およびより一般的な) の次の静的メソッドを使用することができます、`ImageSource`を返しますのすべてのクラス`ImageSource`オブジェクト。

- [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) 基づく web 経由でビットマップにアクセスするため、`Uri`オブジェクト
- [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) PCL は、アプリケーション内の埋め込みリソースとして格納されているビットマップにアクセスするため[ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Type))または[ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Reflection.Assembly))別のソース アセンブリ内のビットマップにアクセスするには
- [`ImageSource.FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) プラットフォームのアプリケーション プロジェクトからビットマップにアクセスします。
- [`ImageSource.FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) 基づくビットマップを読み込むため、`Stream`オブジェクト

クラスのメソッドは、`Image.FromResource`メソッド。 `UriImageSource`クラスは、キャッシュを制御する必要がある場合に便利です。 `FileImageSource`クラスは XAML で便利です。 `StreamImageSource` 非同期読み込みの役に立ちます`Stream`オブジェクトの場合、一方`ImageSource.FromStream`が同期されます。

## <a name="platform-independent-bitmaps"></a>プラットフォームに依存しないビットマップ

[ **WebBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode)プロジェクトでは、ビットマップを読み込みを使用して、web 経由で`ImageSource.FromUri`します。 `Image`要素に設定されて、`Content`のプロパティ、`ContentPage`ので、ページのサイズに制限されます。 ビットマップのサイズの制約に関係なく`Image`要素は、コンテナーのサイズに拡大し、ビットマップ内の最大サイズを表示する、`Image`ビットマップの縦横比を維持しながら要素。 領域、`Image`以外にも、ビットマップを色付きの[ `BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)します。

[ **WebBitmapXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml)サンプルと似ていますが、単に設定、`Source`プロパティに、URL。 変換が処理されますが、 [ `ImageSourceConverter` ](xref:Xamarin.Forms.ImageSourceConverter)クラス。

### <a name="fit-and-fill"></a>調整と塗りつぶし

設定して、ビットマップを拡大する方法を制御することができます、 [ `Aspect` ](xref:Xamarin.Forms.Image.Aspect)のプロパティ、`Image`の次のメンバーのいずれかに、 [ `Aspect` ](xref:Xamarin.Forms.Aspect)列挙体。

- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit): 縦横比 (既定値) の尊重
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill): 領域で埋め、縦横比が適用されません
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill): 領域が埋められますが、縦横比、ビットマップの一部をトリミングすることによって実現を尊重

### <a name="embedded-resources"></a>埋め込みリソース

ビットマップ ファイルは、PCL、または、PCL のフォルダーに追加できます。 付けます、**ビルド アクション**の**EmbeddedResource**します。 [ **ResourceBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode)サンプルを使用する方法を示します`ImageSource.FromResource`ファイルを読み込めません。 メソッドに渡されるリソース名は、アセンブリ名の後にドットの後に省略可能なフォルダー名とファイル名に続けて、ドットで構成されます。

プログラムのセット、`VerticalOptions`と`HorizontalOptions`のプロパティ、`Image`に`LayoutOptions.Center`、これにより、`Image`制約なしの要素。 `Image`ビットマップは同じサイズ。

- IOS と Android での`Image`はビットマップのピクセル サイズです。 ビットマップのピクセルと画面ピクセルの間の一対一のマッピングがあります。
- ユニバーサル Windows プラットフォームで、`Image`はデバイスに依存しない単位で、ビットマップのピクセル サイズです。 ほとんどのデバイスでは、各ビットマップのピクセルは、複数の画面のピクセルを占有します。

[ **StackedBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) put のサンプル、`Image`垂直で`StackLayout`XAML でします。 という名前のマークアップ拡張[ `ImageResourceExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) XAML 内の埋め込みリソースを参照するために役立ちます。 このクラスはのみ、そのアセンブリからリソースを読み込みますが配置されている、ため、ライブラリ内に配置することはできません。

### <a name="more-on-sizing"></a>詳細については、サイズ変更

多くの場合、サイズのビットマップのすべてのプラットフォーム間で一貫する必要があります。
みる**StackedBitmap**、設定することができます、`WidthRequest`で、`Image`垂直方向に要素`StackLayout`が、プラットフォーム間で一貫したサイズにするのにだけサイズを縮小できます、この手法を使用します。

設定することも、`HeightRequest`イメージを作成するサイズのプラットフォームで一貫性のあるが、この手法の多用性をビットマップの幅が制限されます。 垂直方向に、イメージの`StackLayout`、`HeightRequest`避ける必要があります。

デバイス非依存単位 phone 幅より広いビットマップを開始し、設定するが最善のアプローチ`WidthRequest`デバイスに依存しない単位で必要な幅にします。 これは、方法については、 [ **DeviceIndBitmapSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize)サンプル。

[ **MadTeaParty** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty)ルイス キャロルの第 7 章が表示されます*景色 Alice の冒険*John Tenniel によって元の図。

[![Mad 紅茶のパーティのスクリーン ショットをトリプル](images/ch13fg16-small.png "Mad Hatters 紅茶のパーティの書籍のテキスト")](images/ch13fg16-large.png#lightbox "Mad Hatters 紅茶のパーティの書籍のテキスト")

### <a name="browsing-and-waiting"></a>参照して、待機しています

[ **ImageBrowser** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser)サンプルには、Xamarin の web サイトに格納されているストック イメージをブラウズできます。 .NET を使用して[ `WebRequest` ](xref:System.Net.WebRequest)ビットマップの一覧を含む JSON ファイルをダウンロードするクラス。

> [!NOTE]
> Xamarin.Forms のプログラムを使用する必要があります[ `HttpClient` ](xref:System.Net.Http.HttpClient)なく[ `WebRequest` ](xref:System.Net.WebRequest)インターネット経由でファイルにアクセスするためです。 

プログラムを使用して、 [ `ActivityIndicator` ](xref:Xamarin.Forms.ActivityIndicator)を処理が行われているかを示します。 各ビットマップを読み込み、読み取り専用として[ `IsLoading` ](xref:Xamarin.Forms.Image.IsLoading)プロパティの`Image`は`true`します。 `IsLoading`ためプロパティは、バインド可能なプロパティによって支えられて、`PropertyChanged`そのプロパティを変更するときに発生します。 プログラムは、このイベントにハンドラーをアタッチしの現在の設定を使用して`IsLoaded`を設定する、 [ `IsRunning` ](https://api/property/Xamarin.Forms.ActivityIndicator.IsRunning/)のプロパティ、`ActivityIndicator`します。

## <a name="streaming-bitmaps"></a>ビットマップのストリーミング

`ImageSource.FromStream`メソッドを作成、 `ImageSource` .NET に基づいて`Stream`します。 メソッドに渡す必要がある、`Func`オブジェクトを返す、`Stream`オブジェクト。

### <a name="accessing-the-streams"></a>ストリームへのアクセス

[ **BitmapStreams** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams)サンプルを使用する方法を示して、`ImaageSource.FromStream`メソッドに埋め込みリソースとして格納されているビットマップを読み込むと、web 上で、ビットマップを読み込めません。

### <a name="generating-bitmaps-at-run-time"></a>実行時にビットマップを生成します。

簡単にコードで構築してに格納し、圧縮されていない BMP ファイル形式をサポートしてすべての Xamarin.Forms プラットフォーム、`MemoryStream`します。 この手法により、アルゴリズム、実行時にビットマップの作成に実装されている、 [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)クラス、 **Xamrin.FormsBook.Toolkit**ライブラリ。

「は、自分で」 [ **DiyGradientBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap)サンプルの使用を示します`BmpMaker`グラデーション、イメージ、ビットマップを作成します。

## <a name="platform-specific-bitmaps"></a>プラットフォーム固有のビットマップ

すべての Xamarin.Forms プラットフォームは、プラットフォームのアプリケーション アセンブリにビットマップを格納するを許可します。 これらのプラットフォームのビットマップがの型は、Xamarin.Forms アプリケーションで取得するとき[ `FileImageSource`](xref:Xamarin.Forms.FileImageSource)します。 使用すること。

- [ `Icon` ](xref:Xamarin.Forms.MenuItem.Icon)のプロパティ [`MenuItem`](xref:Xamarin.Forms.MenuItem)
- [ `Icon` ](xref:Xamarin.Forms.MenuItem.Icon)のプロパティ [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)
- [ `Image` ](xref:Xamarin.Forms.Button)のプロパティ `Button`

プラットフォーム アセンブリには、アイコンとスプラッシュ スクリーン用のビットマップには既にが含まれます。

- IOS プロジェクトでの**リソース**フォルダー
- Android プロジェクトのサブフォルダーで、**リソース**フォルダー
- Windows プロジェクトでの**資産**フォルダー (ただし、Windows プラットフォームでは、そのフォルダーへのビットマップの使用は制限しない)

[ **PlatformBitmaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps)サンプルでは、コードを使用して、プラットフォームのアプリケーション プロジェクトからアイコンを表示します。

### <a name="bitmap-resolutions"></a>ビットマップの解決策

すべてのプラットフォームでは、複数のバージョンの別のデバイスの解像度のビットマップ イメージの保存を許可します。 実行時に適切なバージョンがデバイスの画面の解像度に基づいて読み込まれます。

Ios では、これらのビットマップは、ファイル名にサフィックスによって区別されます。

- 160 DPI デバイス (1 ピクセル デバイスに依存しない単位) のサフィックスなし
- '@2x' 320 DPI デバイス (2 の単位は DIU をピクセル単位) のサフィックス
- '@3x' 480 DPI デバイス (、単位は DIU に 3 つのピクセル単位) のサフィックス

次の 3 つのバージョンでは、1 インチの四角形として表示するためのビットマップが存在します。

- 160 ピクセルの正方形に MyImage.jpg
- MyImage@2x.jpg 320 ピクセルの正方形で
- MyImage@3x.jpg 480 ピクセルの正方形に

このビットマップとして MyImage.jpg を参照して、プログラムが、画面の解像度に基づいて実行時に適切なバージョンが取得されます。 制約なし、ときに、ビットマップは 160 のデバイスに依存しない単位で常に表示されます。

さまざまなサブフォルダーに android では、ビットマップが格納されている、**リソース**フォルダー。

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

いくつかのパーセンテージは有効です。 この本のサンプル プログラムでイメージのみを含める**スケール 200** 、サフィックスが現在の Xamarin.Forms ソリューション テンプレートを含める**スケール 100**、**スケール 125**、**スケール ~ 150 台**、および**スケール 400**します。

プラットフォームのプロジェクトにビットマップを追加するときに、**ビルド アクション**する必要があります。

- iOS: **BundleResource**
- Android: **AndroidResource**
- UWP:**コンテンツ**

[ **ImageTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap)サンプルから成る 2 つのボタンのようなオブジェクトを作成します`Image`を持つ要素を`TapGestureRecognizer`をインストールします。 オブジェクトが 1 インチの四角形をすることが目的です。 `Source`プロパティの`Image`を使用して設定されている`OnPlatform`と`On`プラットフォームで可能性のあるさまざまなファイル名を参照するオブジェクト。 ビットマップ イメージには、どのサイズ ビットマップが取得され、表示を表示できるように、ピクセル サイズを示す番号が含まれます。

### <a name="toolbars-and-their-icons"></a>ツールバーとアイコン

Xamarin.Forms のツールバーでは、構築するには、プラットフォーム固有のビットマップの主な用途の 1 つ[ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem)オブジェクトを[ `ToolbarItems` ](xref:Xamarin.Forms.Page.ToolbarItems) により定義されたコレクション`Page`. `ToobarItem` 派生した[ `MenuItem` ](xref:Xamarin.Forms.MenuItem)元である、いくつかのプロパティを継承します。

最も重要な`ToolbarItem`のプロパティします。

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) プラットフォームによって表示されるテキストと `Order`
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) 型の`FileImageSource`プラットフォームによって表示されるイメージと `Order`
- [`Order`](xref:Xamarin.Forms.ToolbarItem.Order) 型の[ `ToolbarItemOrder` ](xref:Xamarin.Forms.ToolbarItemOrder)、3 つのメンバーを持つ列挙[ `Default` ](xref:Xamarin.Forms.ToolbarItemOrder.Default)、 [ `Primary` ](xref:Xamarin.Forms.ToolbarItemOrder.Primary)、および[ `Secondary` ](xref:Xamarin.Forms.ToolbarItemOrder.Secondary).

数`Primary`項目 3 または 4 に限定されます。 含める必要があります、`Text`のすべての項目を設定します。 ほとんどのプラットフォームでのみ、`Primary`項目が必要です、 `Icon` Windows 8.1 が必要ですが、`Icon`のすべての項目。 32 のデバイスに依存しない単位正方形のアイコンがあります。 `FileImageSource`型では、プラットフォーム固有であることを示します。

`ToolbarItem`発生、 [ `Clicked` ](xref:Xamarin.Forms.MenuItem.Clicked)非常によく似たタップしたときにイベントを`Button`します。 `ToolbarItem` サポートしています[ `Command` ](xref:Xamarin.Forms.MenuItem.Command)と[ `CommandParameter` ](xref:Xamarin.Forms.MenuItem.CommandParameter) MVVM 関連多くの場合に使用されるプロパティ。 (を参照してください[第 18 章、MVVM](chapter18.md))。

IOS と Android の両方は、ツールバーを表示するページである必要があります、 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage)またはに移動 ページ、`NavigationPage`します。 [ **ToolbarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo)プログラムのセット、`MainPage`プロパティの`App`クラスを[`NavigationPage`コンス トラクター](xref:Xamarin.Forms.NavigationPage.%23ctor(Xamarin.Forms.Page))で、 `ContentPage`引数は、ツールバーの構築とイベント ハンドラーを示しています。

### <a name="button-images"></a>ボタンのイメージ

設定する、プラットフォーム固有のビットマップを使用することもできます、 [ `Image` ](xref:Xamarin.Forms.Button.Image)プロパティの`Button`に示すようには、32 のデバイスに依存しない単位正方形のビットマップに、 [ **ButtonImage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage)サンプル。

> [!NOTE]
> ボタンのイメージの使用が強化されました。 参照してください[ボタンにビットマップを使用して](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)します。

## <a name="related-links"></a>関連リンク

- [第 13 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [第 13 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [イメージの処理](~/xamarin-forms/user-interface/images.md)
- [ボタンにビットマップを使用します。](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)
