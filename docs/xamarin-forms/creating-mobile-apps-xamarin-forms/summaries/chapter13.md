---
title: 第 13 章の概要です。 ビットマップ
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 76551057abc1abdd150591c0a1be39e9f68c4278
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-13-bitmaps"></a>第 13 章の概要です。 ビットマップ

Xamarin.Forms [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)要素には、ビットマップが表示されます。 Xamarin.Forms のすべてのプラットフォームでは、JPEG、PNG、GIF、BMP ファイル形式をサポートします。

ビットマップを Xamarin.Forms では、4 つの場所から取得されます。

- URL で指定された web 経由で
- 一般的なポータブル クラス ライブラリ内のリソースとして埋め込まれます
- プラットフォームのアプリケーション プロジェクトにリソースとして埋め込まれました。
- .NET で参照できる任意の場所から`Stream`オブジェクトを含む `MemoryStream`

PCL にビットマップのリソースがプラットフォームに依存しない、プラットフォームのプロジェクトでのビットマップ リソースはプラットフォームに固有です。

設定して、ビットマップを指定、 [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/)プロパティの`Image`型のオブジェクトを[ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/)、3 つの派生クラスを持つ抽象クラス。

- [`UriImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) ビットマップにに基づいて web 経由でアクセスするため、`Uri`オブジェクトに設定してその[ `Uri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.Uri/)プロパティ
- [`FileImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/) プラットフォームのアプリケーション プロジェクトに格納されているビットマップにアクセスするために設定フォルダーとファイル パスに基づくその[ `File` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FileImageSource.File/)プロパティ
- [`StreamImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/) .NET を使用してビットマップを読み込むための`Stream`を返すことによって指定されたオブジェクト、`Stream`から、`Func`設定その[ `Stream` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StreamImageSource.Stream/)プロパティ

または (およびより一般的) の次の静的メソッドを使用することができます、`ImageSource`クラスをすべて返す`ImageSource`オブジェクト。

- [`ImageSource.FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) ビットマップにに基づいて web 経由でアクセスするため、`Uri`オブジェクト
- [`ImageSource.FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) PCL、アプリケーション内の埋め込みリソースとして格納されているビットマップにアクセスするため、または[ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Type/)または[ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Reflection.Assembly/)別のソース アセンブリ内のビットマップにアクセスするには
- [`ImageSource.FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) プラットフォームのアプリケーション プロジェクトからビットマップにアクセスします。
- [`ImageSource.FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) 基づくビットマップを読み込むための`Stream`オブジェクト

クラスの該当するショートカットはありません、`Image.FromResource`メソッドです。 `UriImageSource`クラスは、キャッシュを制御する必要がある場合に便利です。 `FileImageSource`クラスは XAML で便利です。 `StreamImageSource` 非同期の読み込みに役に立ちます`Stream`オブジェクト、一方`ImageSource.FromStream`は同期型です。

## <a name="platform-independent-bitmaps"></a>プラットフォームに依存しないビットマップ

[ **WebBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode)プロジェクトでは、ビットマップを読み込みを使用して、web 経由で`ImageSource.FromUri`です。 `Image`要素に設定されて、`Content`のプロパティ、`ContentPage`ので、ページのサイズに制限されます。 ビットマップのサイズを制約に関係なく`Image`要素は、コンテナーのサイズに合わせて引き伸ばされ、ビットマップ内の最大サイズを表示する、`Image`ビットマップの縦横比を維持しながら要素。 領域、`Image`ビットマップを色付きの越えて[ `BackgroundColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/)です。

[ **WebBitmapXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml)サンプルと似ていますが、単に設定、 `Source` URL にプロパティです。 変換がによって処理される、 [ `ImageSourceConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSourceConverter/)クラスです。

### <a name="fit-and-fill"></a>調整と塗りつぶし

設定して、ビットマップを拡大する方法を制御することができます、 [ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/)のプロパティ、`Image`の次のメンバーのいずれかに、 [ `Aspect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect/)列挙します。

- [`AspectFit`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFit/): 縦横比 (既定値) を尊重
- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.Fill/): 領域に、縦横比を考慮せず
- [`AspectFill`](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect.AspectFill/): 領域が埋められますが、縦横比、ビットマップの一部をトリミングして実現を尊重

### <a name="embedded-resources"></a>埋め込みリソース

PCL にするか、フォルダー、PCL にビットマップ ファイルを追加することができます。 付けます、**ビルド アクション**の**EmbeddedResource**です。 [ **ResourceBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode)サンプルを使用する方法を示します`ImageSource.FromResource`ファイルを読み込めません。 メソッドに渡されるリソース名は、省略可能なフォルダー名を続けて、ドットとドットの後に、ファイル名を続けて、アセンブリ名で構成されます。

プログラム設定、`VerticalOptions`と`HorizontalOptions`のプロパティ、`Image`に`LayoutOptions.Center`、これにより、`Image`制約なしの要素。 `Image`ビットマップは同じサイズ。

- IOS および Android では、上、`Image`ビットマップのピクセル サイズです。 ビットマップのピクセルと画面のピクセルの間の一対一のマッピングがあります。
- Windows ランタイムのプラットフォームで、`Image`デバイス非依存単位にビットマップのピクセル サイズです。 ほとんどのデバイスでは、各ビットマップ ピクセルは、複数の画面のピクセルを占有します。

[ **StackedBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) puts のサンプル、`Image`垂直方向に`StackLayout`XAML でします。 マークアップ拡張機能が名前付き[ `ImageResourceExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) XAML 内の埋め込みリソースを参照するために役立ちます。 このクラスはのみでアセンブリからリソースを読み込みますが配置されて、ライブラリに格納することができないようにします。

### <a name="more-on-sizing"></a>詳細については、サイズ変更

多くの場合、すべてのプラットフォーム間で一貫してサイズ ビットマップする必要があります。
試み**StackedBitmap**、設定することができます、`WidthRequest`上、`Image`垂直方向に要素`StackLayout`サイズが、プラットフォーム間で一貫してのみ、サイズを小さくこの手法を使用します。

設定することも、`HeightRequest`イメージを作成する、プラットフォームでは、一貫性のあるサイズが、この手法の用途のためをビットマップの幅が制限されます。 垂直方向の画像の`StackLayout`、`HeightRequest`避ける必要があります。

最良の手法がデバイスに依存しない単位 phone 幅より広いビットマップで開始し、設定には`WidthRequest`デバイス非依存単位で必要な幅にします。 これに示されている、 [ **DeviceIndBitmapSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize)サンプルです。

[ **MadTeaParty** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty)ルイス Carroll の第 7 章が表示されます*景色 Alice の冒険*John Tenniel によって元の図で。

[![Mad 紅茶のパーティのスクリーン ショットをトリプル](images/ch13fg16-small.png "Mad Hatters 紅茶のパーティの書籍のテキスト")](images/ch13fg16-large.png#lightbox "Mad Hatters 紅茶のパーティの書籍のテキスト")

### <a name="browsing-and-waiting"></a>参照と、待機しています

[ **ImageBrowser** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser)サンプルは、Xamarin の web サイトに格納されているストック イメージを参照するユーザーを許可します。 .NET を使用して`WebRequest`ビットマップの一覧が JSON ファイルをダウンロードするクラス。

プログラムを使用して、 [ `ActivityIndicator` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/)をするものが起こっているかを示します。 各ビットマップを読み込んで、読み取り専用と[ `IsLoading` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.IsLoading/)プロパティ`Image`は`true`します。 `IsLoading`プロパティはそのため、バインド可能なプロパティによってバックアップ、`PropertyChanged`イベントは、そのプロパティが変更されたときに発生します。 プログラムは、このイベントにハンドラーをアタッチしの現在の設定を使用して`IsLoaded`を設定する、 [ `IsRunning` ](https://api/property/Xamarin.Forms.ActivityIndicator.IsRunning/)のプロパティ、`ActivityIndicator`です。

## <a name="streaming-bitmaps"></a>ビットマップのストリーミング

`ImageSource.FromStream`メソッドを作成、 `ImageSource` .NET に基づいて`Stream`です。 メソッドを渡す必要があります、`Func`オブジェクトを返す、`Stream`オブジェクト。

### <a name="accessing-the-streams"></a>ストリームへのアクセス

[ **BitmapStreams** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams)サンプルを使用する方法を示します、`ImaageSource.FromStream`メソッドに、埋め込みリソースとして格納されているビットマップを読み込みし、web 全体で、ビットマップをロードします。

### <a name="generating-bitmaps-at-run-time"></a>実行時にビットマップを生成します。

Xamarin.Forms のすべてのプラットフォームは簡単でコードを構築し、保管に圧縮されていない BMP ファイル形式をサポートして、`MemoryStream`です。 この手法により、アルゴリズムによって、実行時にビットマップの作成に実装されている、 [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs)クラス内で、 **Xamrin.FormsBook.Toolkit**ライブラリです。

「は、自分で」 [ **DiyGradientBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap)サンプルでの使用`BmpMaker`グラデーション イメージを含むビットマップを作成します。

## <a name="platform-specific-bitmaps"></a>プラットフォーム固有のビットマップ

Xamarin.Forms のすべてのプラットフォームでは、プラットフォームのアプリケーション アセンブリにビットマップを格納することを許可します。 Xamarin.Forms アプリケーションによって取得された、ときにこれらのプラットフォームのビットマップが型[ `FileImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/)です。 使用するには。

- [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/)のプロパティ [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/)
- [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/)のプロパティ [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/)
- [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)のプロパティ `Button`

プラットフォーム アセンブリには、アイコンとスプラッシュ スクリーン用のビットマップには既にが含まれます。

- IOS プロジェクトで、**リソース**フォルダー
- サブフォルダーに、Android プロジェクトで、**リソース**フォルダー
- プロジェクトでは、Windows で、**資産**フォルダー (Windows プラットフォームでは、そのフォルダーへのビットマップの使用は制限しない) には

[ **PlatformBitmaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps)サンプルでは、コードを使用して、プラットフォームのアプリケーション プロジェクトからアイコンを表示します。

### <a name="bitmap-resolutions"></a>ビットマップの解決策

すべてのプラットフォームでは、別のデバイスの解像度用のビットマップ イメージの複数のバージョンの保存を許可します。 実行時に、適切なバージョンがデバイスの画面の解像度に基づいて読み込まれます。

Ios の場合は、これらのビットマップは、ファイル名にサフィックスによって区別されています。

- 160 DPI デバイス (デバイス非依存単位に 1 ピクセル) のサフィックスなし
- '@2x' 320 DPI デバイス (2 の単位は DIU をピクセル単位) のサフィックス
- '@3x' 480 DPI デバイス (、単位は DIU を 3 ピクセル単位) のサフィックス

1 インチの四角形として表示するためのビットマップは、3 つのバージョンに存在します。

- 160 ピクセルの四角形に MyImage.jpg
- MyImage@2x.jpg 320 ピクセルの四角形で
- MyImage@3x.jpg 480 ピクセルの四角形に

このビットマップとして MyImage.jpg を参照して、プログラムが、画面の解像度に基づいて実行時に適切なバージョンを取得します。 制約なし、ときに、ビットマップは 160 デバイス非依存単位で常に表示されます。

さまざまなサブフォルダーに android では、ビットマップが格納されている、**リソース**フォルダー。

- ドロウアブル-ldpi (低 DPI) 120 DPI デバイス (0.75 ピクセル、単位は DIU を)
- ドロウアブル-mdpi (中) の形式で 160 DPI デバイス (1 ピクセル、単位は DIU を)
- ドロウアブル-hdpi (高) 240 DPI デバイス (1.5 の単位は DIU をピクセル単位)
- ドロウアブル-xhdpi (余分な高い) 320 DPI デバイス (2 の単位は DIU をピクセル単位)
- ドロウアブル-xxhdpi (場合によっては余分な高い余分な) 480 DPI デバイス (、単位は DIU を 3 ピクセル単位)
- ドロウアブル-xxxhdpi (3 つの余分な高) 640 DPI デバイス (4 ピクセル、単位は DIU を)

正方形の 1 インチに表示されるもので、ビットマップのビットマップのさまざまなバージョンは、さまざまなサイズは、同じ名前になり、これらのフォルダーに格納します。

- ドロウアブル-ldpi/MyImage.jpg 120 ピクセルの四角形に
- ドロウアブル-mdpi/MyImage.jpg 160 ピクセルの四角形に
- ドロウアブル-hdpi/MyImage.jpg 240 ピクセルの四角形に
- ドロウアブル-xhdpi/MyImage.jpg 320 ピクセルの四角形で
- ドロウアブル-xxhdpi/MyImage.jpg 480 ピクセルの四角形に
- ドロウアブル-xxxhdpi/MyImage.jpg 640 ピクセルの四角形で

ビットマップは、160 個のデバイスに依存しない単位で常に表示されます。 (標準の Xamarin.Forms ソリューション テンプレートのみを含む hdpi、xhdpi、および xxhdpi フォルダー)

Windows ランタイム プロジェクトは、名前付けスキームで構成されているデバイスに依存しない単位あたりのピクセルで、スケール ファクターのパーセンテージ、たとえばビットマップをサポートします。

- MyImage.scale-200.jpg at 320 pixels square

一部の割合のみが有効です。 このドキュメントのサンプル プログラムのみを使用してイメージを含める**スケール 200** 、サフィックスが含む Xamarin.Forms ソリューション テンプレートを現在**スケール 100**、**スケール 125**、**スケール 150**、および**スケール 400**です。 

プラットフォームのプロジェクトにビットマップを追加するときに、**ビルド アクション**する必要があります。

- iOS: **BundleResource**
- Android: **AndroidResource**
- Windows ランタイム:**コンテンツ**

[ **ImageTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap)サンプルから成る 2 つのボタンのようなオブジェクトを作成する`Image`を持つ要素を`TapGestureRecognizer`をインストールします。 オブジェクトの 1 インチの正方形であるものです。 `Source`プロパティ`Image`を使用して設定されている`OnPlatform`と`On`プラットフォームでは、可能性のあるさまざまなファイル名を参照するオブジェクト。 ビットマップ イメージには、どのサイズ ビットマップが取得され、表示を確認できるように、そのピクセルのサイズを示す番号が含まれます。

### <a name="toolbars-and-their-icons"></a>ツールバーとアイコン

プラットフォーム固有のビットマップの主な用途の 1 つは Xamarin.Forms ツールバーで、追加することによって構築された[ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/)オブジェクトを[ `ToolbarItems` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.ToolbarItems/) により定義されたコレクション`Page`. `ToobarItem` 派生した[ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/)元であるいくつかのプロパティを継承します。

最も重要な`ToolbarItem`のプロパティします。

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) プラットフォームによって表示されるテキストと `Order`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) 型の`FileImageSource`プラットフォームによって表示されるイメージと `Order`
- [`Order`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Order/) 型の[ `ToolbarItemOrder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItemOrder/)、3 つのメンバーを持つ列挙[ `Default` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Default/)、 [ `Primary` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Primary/)、および[ `Secondary` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Secondary/).

数`Primary`項目が 3 または 4 に制限する必要があります。 含める必要があります、`Text`すべての項目を設定します。 ほとんどのプラットフォームでのみ、`Primary`が必要なアイテム、 `Icon` Windows 8.1 が必要ですが、`Icon`すべての項目。 アイコンは、32 のデバイスに依存しない単位正方形にする必要があります。 `FileImageSource`型では、プラットフォーム固有であることを示します。

`ToolbarItem`発生、 [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/)タップすると、非常によく似たしたときにイベントを`Button`です。 `ToolbarItem` サポートしています[ `Command` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Command/)と[ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.CommandParameter/) MVVM 関連よく使用されるプロパティです。 (を参照してください[第 18 章、MVVM](chapter18.md))。

IOS および Android の両方のツールバーを表示するページがあることが必要な[ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)またはで移動する先のページ、`NavigationPage`です。 [ **ToolbarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo)プログラムのセット、`MainPage`のプロパティ、`App`クラスを[`NavigationPage`コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.NavigationPage.NavigationPage/p/Xamarin.Forms.Page/)で、 `ContentPage`引数、ツールバーの構築とイベント ハンドラーを示しています。

### <a name="button-images"></a>ボタン イメージ

設定する、プラットフォーム固有のビットマップを使用することもできます、 [ `Image` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Image/)プロパティ`Button`に示すように 32 のデバイスに依存しない単位の四角形のビットマップを、 [ **ButtonImage。**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage)サンプルです。



## <a name="related-links"></a>関連リンク

- [第 13 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [サンプルの第 13 章](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [イメージの処理](~/xamarin-forms/user-interface/images.md)
