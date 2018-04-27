---
title: パート 1 です。 XAML の概要
description: Xamarin.Forms アプリケーションでは、XAML はページのビジュアルの内容を定義するほとんどの場合に使用されます。 XAML ファイルは、マークアップのコードのサポートを提供する c# コード ファイルに関連付けでは常にします。 同時に、これら 2 つのファイルは、子ビューおよびプロパティの初期化を含む新しいクラス定義に影響します。 XAML ファイル内でクラスとプロパティが、XML 要素と属性、参照されているし、マークアップとコード間のリンクが確立されます。
ms.prod: xamarin
ms.assetid: 9073FA0E-BD5A-4492-8A93-54C466F6EDB9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: f8032966b49f6f023642b0d1338e8c5d740b66e0
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2018
---
# <a name="part-1-getting-started-with-xaml"></a>パート 1 です。 XAML の概要

_Xamarin.Forms アプリケーションでは、XAML はページのビジュアルの内容を定義するほとんどの場合に使用されます。XAML ファイルは、マークアップのコードのサポートを提供する c# コード ファイルに関連付けでは常にします。同時に、これら 2 つのファイルは、子ビューおよびプロパティの初期化を含む新しいクラス定義に影響します。XAML ファイル内でクラスとプロパティが、XML 要素と属性、参照されているし、マークアップとコード間のリンクが確立されます。_

## <a name="creating-the-solution"></a>ソリューションの作成

最初の XAML ファイルの編集を開始するには、Visual Studio または Visual Studio for Mac を使用して新しい Xamarin.Forms ソリューションを作成します。 (お客様の環境に対応するこのページの上部にあるタブを選択します。)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Windows では、Visual Studio を使用して選択**ファイル > 新規 > プロジェクト** メニューからです。 **新しいプロジェクト**ダイアログで、 **Visual c# > Cross Platform** 、左側にあるし、 **(Xamarin.Forms またはネイティブ) のクロス プラットフォーム アプリ**センターの一覧からです。 

![](get-started-with-xaml-images/win/newprojectdialog.png "新しいプロジェクト ダイアログ ボックス")

ソリューションの場所を選択しの名前を付けます**XamlSamples** (または必要に応じて)、キーを押します**OK**です。

次の画面で選択、**空のアプリケーション**、テンプレート、 **Xamarin.Forms** UI テクノロジと**ポータブル クラス ライブラリ (PCL)** コード共有の戦略。

![](get-started-with-xaml-images/win/newcrossplatformapp.png "新しいアプリ ダイアログ ボックス")

**[OK]** を押します。 

4 つのプロジェクトがソリューションに作成されます。 **XamlSamples**ポータブル クラス ライブラリ (PCL) **XamlSamples.Android**、 **XamlSamples.iOS**、およびユニバーサル Windowsプラットフォーム ソリューション**XamlSamples.UWP**です。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 用 Visual Studio で次のように選択します。**ファイル > 新しいソリューション** メニューからです。 **新しいプロジェクト**ダイアログで、**マルチプラット フォーム > アプリ**、左側にあると**空のフォーム アプリケーション**(*いない***フォーム アプリ**) テンプレートの一覧から。

![](get-started-with-xaml-images/mac/newprojectdialog1.png "新しいプロジェクト ダイアログ ボックス 1")

キーを押して**次**です。

[次へ] ダイアログ ボックスで、プロジェクトの名前を付けます**XamlSamples** (または必要に応じて)。 確認して、**ポータブル クラス ライブラリの使用**ラジオ ボタンが選択されていることと**ユーザー インターフェイス ファイルを使用して XAML**がチェックします。

![](get-started-with-xaml-images/mac/newprojectdialog2.png "新しいプロジェクト ダイアログ ボックス 2")

キーを押して**次**です。 

次のダイアログ ボックスでは、プロジェクトの場所を選択できます。

![](get-started-with-xaml-images/mac/newprojectdialog3.png "新しいプロジェクト ダイアログ ボックス 3")

キーを押して**作成**

3 つのプロジェクトがソリューションに作成されます。 **XamlSamples**ポータブル クラス ライブラリ (PCL) **XamlSamples.Android**、および**XamlSamples.iOS**です。 

-----

作成した後、 **XamlSamples**ソリューションでは、ソリューションのスタートアップ プロジェクトとしてさまざまなプラットフォームのプロジェクトを選択することによって、開発環境をテストすることができ、によって作成されたビルドとの簡単なアプリケーションの展開phone エミュレーターまたはデバイスの実際のいずれかのプロジェクト テンプレートです。

共有プラットフォーム固有のコードを記述する必要がない限り**XamlSamples** PCL プロジェクトは、時間がある、プログラミングにかかる時間をほぼすべてです。 これらの記事は、そのプロジェクトの外部で高まるはされません。

### <a name="anatomy-of-a-xaml-file"></a>XAML ファイルの構造

内で、 **XamlSamples**ポータブル クラス ライブラリは、次の名前を持つファイルのペア。

- **App.xaml**、XAML ファイルであると
- **App.xaml.cs**、c#*コード ビハインド*XAML ファイルに関連付けられているファイル。

横の矢印をクリックする必要があります**App.xaml**分離コード ファイルを表示します。 

両方**App.xaml**と**App.xaml.cs**という名前のクラスに影響する`App`から派生した`Application`です。 XAML ファイルに他のほとんどのクラスから派生したクラスに影響する`ContentPage`; それらのファイルでは、XAML を使用して、ページ全体のビジュアルの内容を定義します。 これは、その他の 2 つのファイルの場合は true、 **XamlSamples**プロジェクト。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- **MainPage.xaml**、XAML ファイルであると
- **MainPage.xaml.cs**、c# 分離コード ファイル。

**MainPage.xaml**ファイルは、次のようになります。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage">

    <Label Text="Welcome to Xamarin Forms!" 
           VerticalOptions="Center" 
           HorizontalOptions="Center" />

</ContentPage>
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

- **XamlSamplesPage.xaml**、XAML ファイルであると
- **XamlSamplesPage.xaml.cs**、c# 分離コード ファイル。

**XamlSamplesPage.xaml**ファイルは、次のようになります。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" 
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
             xmlns:local="clr-namespace:XamlSamples" 
             x:Class="XamlSamples.XamlSamplesPage">

    <Label Text="Welcome to Xamarin Forms!" 
           VerticalOptions="Center" 
           HorizontalOptions="Center" />

</ContentPage>
```

-----

2 つの XML 名前空間 ( `xmlns`) の宣言は、Uri、最初の Xamarin の web サイトの見かけ上、および microsoft の 2 つ目を参照してください。 どのような Uri ポイントをチェックする必要はありません。 何もないです。 Xamarin と、Microsoft が所有する Uri だけであり、基本的にはバージョンの識別子として機能します。

最初の XML 名前空間宣言では、プレフィックスのない XAML ファイル内で定義されているタグがたとえば参照、Xamarin.Forms 内のクラスにことを意味`ContentPage`です。 2 番目の名前空間宣言のプレフィックスを定義する`x`です。 これは、使用されるいくつかの要素とは、XAML に固有の属性とそれ自体が XAML の他の実装でサポートされています。 ただし、これらの要素と属性では、URI に埋め込まれた年によって多少異なります。 Xamarin.Forms では、そのすべてではなく、2009 XAML 仕様をサポートします。

`local`名前空間の宣言では、PCL プロジェクトから他のクラスにアクセスすることができます。

その最初のタグの最後に、`x`という名前の属性にプレフィックスが使用`Class`です。 のこの使用`x`プレフィックスは、XAML 名前空間、XAML 属性のほぼユニバーサルなど`Class`はほとんどの場合と呼ば`x:Class`です。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

`x:Class`属性が完全修飾 .NET クラス名を指定します。、`MainPage`クラス内で、`XamlSamples`名前空間。 つまり、この XAML ファイルをという名前の新しいクラスを定義する`MainPage`で、`XamlSamples`から派生した名前空間`ContentPage`— をタグ、`x:Class`属性が表示されます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

`x:Class`属性が完全修飾 .NET クラス名を指定します。、`XamlSamplesPage`クラス内で、`XamlSamples`名前空間。 つまり、この XAML ファイルをという名前の新しいクラスを定義する`XamlSamplesPage`で、`XamlSamples`から派生した名前空間`ContentPage`— をタグ、`x:Class`属性が表示されます。

-----

`x:Class`属性は C# の場合、派生クラスを定義する XAML ファイルのルート要素にのみ表示できます。 これは、XAML ファイルで定義されている唯一の新しいクラスです。 XAML ファイルに表示される他のすべて代わりに単に既存のクラスからインスタンス化および初期化されています。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

**MainPage.xaml.cs**ファイルは、次のようになります (使用されていない場合を除いて`using`ディレクティブ)。

```csharp
using Xamarin.Forms;

namespace XamlSamples
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }
    }
}
```

`MainPage`クラスから派生`ContentPage`、わかりますが、`partial`クラス定義です。 これは、用に別の部分クラス定義が存在する必要がありますを示して`MainPage`が、そのしますか? 新機能を`InitializeComponent`メソッドしますか? 

Visual Studio では、プロジェクトをビルド、c# コード ファイルを生成する XAML ファイルを解析します。 参照する場合、 **XamlSamples\XamlSamples\obj\Debug**ディレクトリ、という名前のファイルにあります**XamlSamples.MainPage.xaml.g.cs**です。 'G' は、生成されるを意味します。 これは、その他の部分クラス定義の`MainPage`の定義を含む、`InitializeComponent`から呼び出されるメソッド、`MainPage`コンス トラクターです。 これら 2 つの部分`MainPage`クラス定義を一緒にコンパイルし、ことができます。 によって、XAML がコンパイルされたかどうか、XAML ファイルまたは XAML ファイルのバイナリ形式のいずれかが実行可能ファイルに埋め込まれます。

特定のプラットフォーム プロジェクト呼び出し内のコードを実行時に、`LoadApplication`の新しいインスタンスを渡す方法、 `App` PCL にクラスです。 `App`クラスのコンス トラクターをインスタンス化`MainPage`です。 そのクラスのコンス トラクターを呼び出す`InitializeComponent`、呼び出す、 `LoadFromXaml` PCL から XAML ファイル (または、コンパイルされたバイナリ) を抽出する方法です。 `LoadFromXaml` XAML ファイルで定義されているすべてのオブジェクトを初期化、親子関係ですべて同時に接続する、イベント、XAML ファイルで設定するためのコードで定義されたイベント ハンドラーをアタッチおよびページのコンテンツとしてオブジェクトの結果ツリーを設定します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

**XamlSamplesPage.xaml.cs**ファイルは、次のようになります。

```csharp
using Xamarin.Forms;

namespace XamlSamples
{
    public partial class XamlSamplesPage : ContentPage
    {
        public XamlSamplesPage()
        {
            InitializeComponent();
        }
    }
}
```

`XamlSamplesPage`クラスから派生`ContentPage`、わかりますが、`partial`クラス定義です。 C# の別のファイルに別の部分クラス定義が存在する必要がありますをある提案`XamlSamplesPage`が、そのしますか? 新機能を`InitializeComponent`メソッドしますか?

Visual Studio for Mac は、プロジェクトをビルド、c# コード ファイルを生成する XAML ファイルを解析します。 参照する場合、 **XamlSamples\XamlSamples\obj\Debug**ディレクトリ、という名前のファイルにあります**XamlSamples.XamlSamplesPage.xaml.g.cs**です。 'G' は、生成されるを意味します。 これは、その他の部分クラス定義の`XamlSamplesPage`の定義を含む、`InitializeComponent`から呼び出されるメソッド、`XamlSamplesPage`コンス トラクターです。  これら 2 つの部分`XamlSamplesPage`クラス定義を一緒にコンパイルし、ことができます。 によって、XAML がコンパイルされたかどうか、XAML ファイルまたは XAML ファイルのバイナリ形式のいずれかが実行可能ファイルに埋め込まれます。

特定のプラットフォーム プロジェクト呼び出し内のコードを実行時に、`LoadApplication`の新しいインスタンスを渡す方法、 `App` PCL にクラスです。 `App`クラスのコンス トラクターをインスタンス化`XamlSamplesPage`です。 そのクラスのコンス トラクターを呼び出す`InitializeComponent`、呼び出す、 `LoadFromXaml` PCL から XAML ファイル (または、コンパイルされたバイナリ) を抽出する方法です。 `LoadFromXaml` XAML ファイルで定義されているすべてのオブジェクトを初期化、親子関係ですべて同時に接続する、イベント、XAML ファイルで設定するためのコードで定義されたイベント ハンドラーをアタッチおよびページのコンテンツとしてオブジェクトの結果ツリーを設定します。

-----

通常は、生成されたコード ファイルに多くの時間を費やす必要はありません、ことがありますランタイム例外が発生、生成されたファイル内のコードに慣れておく必要がありますのでします。

コンパイルして、このプログラムを実行するときに、`Label`が示すように、XAML 要素は、ページの中央に表示されます。 左から右への 3 つのプラットフォームは、iOS、Android、および UWP には。

[![](get-started-with-xaml-images/xamlsamples.png "既定の Xamarin.Forms 表示")](get-started-with-xaml-images/xamlsamples-large.png#lightbox "Xamarin.Forms の既定の表示")

さらに興味深いビジュアルは、必要な詳細は XAML の興味深いです。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

## <a name="preliminaries"></a>準備

名前を変更するファイル名 Visual Studio で Mac 用 Windows で実行されている Visual Studio によって作成されたファイルと一貫性のある、 **XamlSamplesPage.xaml**に**MainPage.xaml**、および**XamlSamplesPage.xaml.cs**に**MainPage.xaml.cs**です。 内で、 **XamlSamplesPage.xaml**ファイルを変更、`XamlSamplesPage`に`MainPage`です。 内で、 **XamlSamplesPage.xaml.cs**ファイルを 2 回の出現の変更は、`XamlSamplesPage`に`MainPage`です。 内で、 **App.xaml.cs**ファイル、ステートメントを変更

```csharp
MainPage = new XamlSamplesPage();
```

この行を次のように変更します。

```csharp
MainPage = new MainPage();
```

-----

プログラムをコンパイルし、続行する前に配置するテストです。

## <a name="adding-new-xaml-pages"></a>新しい XAML ページを追加します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

その他の XAML ベースの追加を`ContentPage`をプロジェクトにクラスを選択、 **XamlSamples** PCL プロジェクトを起動、**プロジェクト > 新しい項目の追加**メニュー項目。 左側にある、**新しい項目の追加**ダイアログで、 **Visual c#** と**Xamarin.Forms**です。 リストから選択**コンテンツ ページ**(されません**コンテンツ ページ (c#)**、これは、コードのみ ページで、作成または**コンテンツ ビュー**ページではない)。 ページの名前、たとえば、 **HelloXamlPage.xaml**:

![](get-started-with-xaml-images/win/addnewitemdialog.png "新しい項目 ダイアログ ボックスを追加します。")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

その他の XAML ベースの追加を`ContentPage`をプロジェクトにクラスを選択、 **XamlSamples** PCL プロジェクトを起動、**ファイル > 新しいファイル**メニュー項目。 左側にある、**新しいファイル**ダイアログで、**フォーム**、左側にあると**フォーム コンテンツ ページの Xaml** (いない**フォーム コンテンツ ページ**、コード専用のページを作成または**コンテンツ ビュー**、これは、ページではありません)。 ページの名前、たとえば、 **HelloXamlPage**:

![](get-started-with-xaml-images/mac/newfiledialog.png "新しいファイル ダイアログ ボックス")

-----

2 つのファイルがプロジェクトに追加された**HelloXamlPage.xaml**と分離コード ファイル**HelloXamlPage.xaml.cs**です。 

## <a name="setting-page-content"></a>設定ページの内容

編集、 **HelloXamlPage.xaml**ようにするためのタグだけがファイル`ContentPage`と`ContentPage.Content`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage">
    <ContentPage.Content>
        
    </ContentPage.Content>
</ContentPage>
```

`ContentPage.Content`タグは XAML の一意の構文の一部です。 最初は、無効な xml か、表示される可能性がありますが、無効です。 期間は、XML の特殊文字ではありません。

`ContentPage.Content`タグが呼び出されます*プロパティ要素*タグ。 `Content` プロパティは、 `ContentPage`、1 つのビューまたは子ビューのレイアウトに一般的に設定されているとします。 プロパティが通常 XAML では、属性になりますが、設定が難しくなります、`Content`複雑なオブジェクトへの属性です。 そのため、プロパティは、クラス名とピリオドで区切ったプロパティ名で構成される XML 要素として表されます。 今すぐ、`Content`間でプロパティを設定することができます、`ContentPage.Content`タグは、次のようにします。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage"
             Title="Hello XAML Page">
    <ContentPage.Content>

        <Label Text="Hello, XAML!"
               VerticalOptions="Center"
               HorizontalTextAlignment="Center"
               Rotation="-15"
               IsVisible="true"
               FontSize="Large"
               FontAttributes="Bold"
               TextColor="Blue" />

    </ContentPage.Content>
</ContentPage>
```

また、`Title`ルート タグに属性が設定されています。

この時点では、クラス、プロパティ、および XML の関係を明らかにする必要があります: A Xamarin.Forms クラス (など`ContentPage`または`Label`) XML 要素としての XAML ファイルに表示されます。 そのクラスのプロパティ-など`Title`で`ContentPage`の 7 つのプロパティと`Label`— 通常の XML 属性として表示されます。

これらのプロパティの値を設定する多くのショートカットが存在します。 いくつかのプロパティは、基本データ型: など、`Title`と`Text`型のプロパティは、 `String`、`Rotation`の型は`Double`と`IsVisible`(は`true`既定では、ここでのみ設定と図) は、型の`Boolean`します。

`HorizontalTextAlignment`プロパティの型は`TextAlignment`、これは、列挙型。 列挙体の型のプロパティで指定する必要がありますすべては、メンバー名です。

より複雑な型のプロパティ、ただし、コンバーターが使用、XAML を解析するためです。 これらは、クラスから派生する xamarin.forms`TypeConverter`です。 多くは、パブリック クラスですが、いくつかはできません。 この特定の XAML ファイルのこれらのクラスのいくつかは、バック グラウンドで役割を果たします。

-  `LayoutOptionsConverter` `VerticalOptions`プロパティ
-  `FontSizeConverter` `FontSize`プロパティ
-  `ColorTypeConverter` `TextColor`プロパティ

これらのコンバーターは、プロパティの設定の使用可能な構文を制御します。

`ThicknessTypeConverter` 1 つ、2 つ、またはコンマで区切られた 4 つの数値を処理できます。 1 つの数値を指定する場合は、次の 4 つの辺をすべてに適用されます。 2 つの数値の 1 つは、左と右の余白を 2 番目の上部と下部です。 4 つの数字は、注文の左、上、右、および下でです。

`LayoutOptionsConverter`のパブリック静的フィールドの名前を変換することができます、`LayoutOptions`構造型の値を`LayoutOptions`です。

`FontSizeConverter`を処理できる、`NamedSize`メンバー、または数値のフォント サイズ。

`ColorTypeConverter`のパブリック静的フィールドの名前を受け入れる、`Color`構造または 16 進数の RGB 値、アルファ チャネルの有無の前にシャープ記号 (#)。 アルファ チャネルせず構文を次に示します。

 `TextColor="#rrggbb"`

個々 のほとんどの文字は、16 進数です。 アルファ チャネルが含まれる方法を次に示します。

 `TextColor="#aarrggbb">`

アルファ チャネルは、FF は完全に不透明であり、00 は完全に透過的にしてください。

その他の 2 つの形式では、チャネルごとに 1 つ、16 進数字のみを指定できます。

 `TextColor="#rgb"` `TextColor="#argb"`

このような場合は、その数字が値に繰り返されます。 たとえば、#CF3、RGB カラー CC-FF-33 です。

## <a name="page-navigation"></a>ページ ナビゲーション

実行すると、 **XamlSamples** 、プログラム、`MainPage`が表示されます。 新しいを表示する`HelloXamlPage`設定するか新しい起動されるページで、 **App.xaml.cs**ファイル、またはから新しいページに移動`MainPage`です。

ナビゲーションを実装するには、最初のコードを変更、 **App.xaml.cs**コンス トラクターできるように、`NavigationPage`オブジェクトを作成します。

```csharp
public App()
{
    InitializeComponent();
    MainPage = new NavigationPage(new MainPage());
}
```

**MainPage.xaml.cs**コンス トラクターを作成、単純な`Button`に移動する、イベント ハンドラーを使用して`HelloXamlPage`:

```csharp
public MainPage()
{
    InitializeComponent();

    Button button = new Button
    {
        Text = "Navigate!",
        HorizontalOptions = LayoutOptions.Center,
        VerticalOptions = LayoutOptions.Center
    };

    button.Clicked += async (sender, args) =>
    {
        await Navigation.PushAsync(new HelloXamlPage());
    };

    Content = button;
}
```

設定、`Content`ページのプロパティの設定の置換、 `Content` XAML ファイルのプロパティです。 コンパイルして、このプログラムの新しいバージョンを配置するときに画面にボタンが表示されます。 移動を押しても`HelloXamlPage`します。 IPhone、Android、および UWP の結果ページを次に示します。

[![](get-started-with-xaml-images/helloxaml1.png "ラベルのテキストを回転")](get-started-with-xaml-images/helloxaml1-large.png#lightbox "ラベルのテキストの回転")

場所に戻ることができます`MainPage`を使用して、 **< 戻る**ios の場合、ページの上部にある、または android で電話の下部にある左向きの矢印を使用して、または Windows 10 で、ページの上部にある左向きの矢印を使用してボタンをクリックします。

表示するために別の方法については、XAML を試して、`Label`です。 テキストに含まれる Unicode 文字を埋め込む必要がある場合は、標準の XML 構文を使用することができます。 たとえば、次のように使用すると、スマート引用符で囲まれた応答メッセージを配置します。

 `<Label Text="&#x201C;Hello, XAML!&#x201D;" … />`

外観を次に示します。

[![](get-started-with-xaml-images/helloxaml2.png "Unicode 文字がラベルのテキストを回転")](get-started-with-xaml-images/helloxaml2-large.png#lightbox "Unicode 文字がラベルのテキストの回転")

## <a name="xaml-and-code-interactions"></a>XAML およびコードの相互作用

**HelloXamlPage**サンプルには、1 つだけが含まれています`Label` ページで、これは非常に通常ではありませんが、します。 ほとんど`ContentPage`派生型のセット、`Content`をいくつかのレイアウト プロパティを並べ替えるなど、`StackLayout`です。 `Children`のプロパティ、`StackLayout`として定義されている型の`IList<View>`が実際には型のオブジェクトは`ElementCollection<View>`コレクションが複数のビューまたはその他のレイアウトを値が設定されるとします。 XAML では、これらの親子関係は通常の XML 階層を確立します。 ここでは、という名前の新しいページの XAML ファイル**XamlPlusCodePage**:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="CenterAndExpand" />

        <Label Text="A simple Label"
               Font="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

この XAML ファイルが構文的に完了しての外観を次に示します。

[![](get-started-with-xaml-images/xamlpluscode1.png "ページ上の複数のコントロール")](get-started-with-xaml-images/xamlpluscode1-large.png#lightbox "ページ上の複数のコントロール")

ただしが不十分な機能的にするには、このプログラムを検討する可能性があります。 おそらく、`Slider`が発生することになって、`Label`を現在の値を表示して、`Button`プログラム内で処理を行うためのものが可能性があります。

わかる[パート 4 です。データのバインドの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)、ジョブの表示、`Slider`値を使用して、 `Label` XAML で、データ バインディングで処理できます。 ソリューションのコードを最初に表示すると便利です。 それでも、処理、`Button`コード必要があります をクリックします。 つまり、分離コード ファイルを`XamlPlusCodePage`のハンドラーを含める必要があります、`ValueChanged`のイベント、`Slider`と`Clicked`のイベント、`Button`です。 説明を追加します。

```csharp
namespace XamlSamples
{
    public partial class XamlPlusCodePage
    {
        public XamlPlusCodePage()
        {
            InitializeComponent();
        }

        void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
        {

        }

        void OnButtonClicked(object sender, EventArgs args)
        {

        }
    }
}
```

これらのイベント ハンドラーは、パブリックである必要はありません。

XAML ファイルに戻り、`Slider`と`Button`タグの属性を含める必要があります、`ValueChanged`と`Clicked`これらのハンドラーを参照するイベント。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.XamlPlusCodePage"
             Title="XAML + Code Page">
    <StackLayout>
        <Slider VerticalOptions="CenterAndExpand"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="A simple Label"
               Font="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Button Text="Click Me!"
                HorizontalOptions="Center"
                VerticalOptions="CenterAndExpand"
                Clicked="OnButtonClicked" />
    </StackLayout>
</ContentPage>
```

イベントのハンドラーを割り当てることが、プロパティに値を割り当てることと同じ構文に注意してください。

場合のハンドラーを`ValueChanged`のイベント、`Slider`を使用する、`Label`ハンドラーを表示するには、現在の値は、コードからそのオブジェクトを参照する必要があります。 `Label`で指定される名前が必要、`x:Name`属性。

```xaml
<Label x:Name="valueLabel"
       Text="A simple Label"
       Font="Large"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand" />
```

`x`のプレフィックス、`x:Name`属性は、この属性が XAML に固有であることを示します。

割り当てる名前、`x:Name`属性が c# 変数名と同じルールです。 たとえば、文字またはアンダー スコアで始まるおよび埋め込み型スペースが含まれていない必要があります。

これで、`ValueChanged`イベント ハンドラーを設定できます、`Label`を表示、新しい`Slider`値。 新しい値は、イベントの引数から入手できます。

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = args.NewValue.ToString("F3");
}
```

または、ハンドラーを取得できなかった、`Slider`からこのイベントを生成しているオブジェクト、`sender`引数を取得し、`Value`のプロパティ。

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = ((Slider)sender).Value.ToString("F3");
}
```

最初に、プログラムを実行すると、`Label`が表示されない、`Slider`値のため、`ValueChanged`イベントが発生していないまだです。 操作ですが、`Slider`により表示される値。

[![](get-started-with-xaml-images/xamlpluscode2.png "表示される値をスライダー")](get-started-with-xaml-images/xamlpluscode2-large.png#lightbox "スライダーの値が表示されます")

ようになりました、`Button`です。 応答をシミュレートしてみましょう、`Clicked`イベントのアラートを表示することによって、`Text`ボタンのです。 イベント ハンドラーは安全にキャストできます、`sender`への引数、`Button`し、そのプロパティにアクセスします。

```csharp
async void OnButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;
    await DisplayAlert("Clicked!",
        "The button labeled '" + button.Text + "' has been clicked",
        "OK");
}
```

メソッドとは見なさ`async`ため、`DisplayAlert`メソッドは非同期でありで始まる必要があります、`await`演算子で、メソッドが終了したときに返されます。 このメソッドが取得されるため、`Button`からイベントを発生させる、`sender`引数、複数のボタンの同じハンドラーを使用できます。

XAML で定義されているオブジェクトが分離コード ファイルで処理されるイベントを起動できることと、分離コード ファイルを使って割り当てられた名前を使用して、XAML で定義されているオブジェクトにアクセスできることを確認した、`x:Name`属性。 これらは、コードと XAML の相互作用する 2 つの基本的な方法です。

XAML の動作を確認するには、新しく生成された収集されない方法にいくつか追加のインサイト**XamlPlusCode.xaml.g.cs ファイル**、するようになったには、いずれかに割り当てられている任意の名前が含まれています。`x:Name`プライベート フィールドとして属性。 次にそのファイルの簡略化されたバージョンを示します。

```csharp
public partial class XamlPlusCodePage : ContentPage {

    private Label valueLabel;

    private void InitializeComponent() {
        this.LoadFromXaml(typeof(XamlPlusCodePage));
        valueLabel = this.FindByName<Label>("valueLabel");
    }
}
```

このフィールドの宣言により、変数内で使用される自由に、`XamlPlusCodePage`管轄の下の部分クラス ファイルです。 実行時に、XAML が解析された後、フィールドが割り当てられます。 つまり、`valueLabel`フィールドは`null`ときに、`XamlPlusCodePage`コンス トラクターの後に、有効な開始`InitializeComponent`と呼びます。

後に`InitializeComponent`コンス トラクターに制御を返します、ページのビジュアル構築が完了した場合と同様されてインスタンス化され、コード内で初期化します。 XAML ファイルは、不要になったクラス内の役割を果たします。 ページをたとえば、ビューを追加することで任意の方法でこれらのオブジェクトを操作することができます、 `StackLayout`、または設定、`Content`以外に、ページのプロパティだけです。 行うことができます"ツリー"を調べることによって、`Content`ページとページ内の項目のプロパティ、`Children`レイアウトのコレクション。 この方法でアクセス ビューのプロパティを設定またはにイベント ハンドラーを動的に割り当てることができます。

自由にできます。 これは、ページと、XAML は、そのコンテンツを作成するツールのみです。

## <a name="summary"></a>まとめ

この概要では、クラス定義に、XAML ファイルとコード ファイルの影響し、XAML およびコード ファイルの相互作用を見てきました。 XAML は、非常に柔軟な方法で使用できるようにする独自の構文機能もあります。 これらでの探索を開始できる[パート 2 です。重要な XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)です。



## <a name="related-links"></a>関連リンク

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [第 2 部基本的な XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [第 3 部XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [第 4 部データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [第 5 部MVVM へのデータ バインディング](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
