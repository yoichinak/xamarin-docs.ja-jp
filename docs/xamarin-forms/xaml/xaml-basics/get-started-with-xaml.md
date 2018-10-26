---
title: 第 1 部です。 XAML の概要
description: Xamarin.Forms アプリケーションで、ほとんどの場合、ページと分離コード ファイルと連携して機能のビジュアル コンテンツを定義するに XAML が使用されます。
ms.prod: xamarin
ms.assetid: 9073FA0E-BD5A-4492-8A93-54C466F6EDB9
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/10/2018
ms.openlocfilehash: f75e49c04ded99b3f7468709926277648f9f3729
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103167"
---
# <a name="part-1-getting-started-with-xaml"></a>第 1 部です。 XAML の概要

_Xamarin.Forms アプリケーションで XAML ページの視覚的内容を定義するために使用がほとんどの場合とで使用できます、C#分離コード ファイル。_

分離コード ファイルは、マークアップのコードのサポートを提供します。 同時に、これら 2 つのファイルは、子ビューとプロパティの初期化が含まれる新しいクラス定義に貢献します。 XAML ファイル内でクラスとプロパティが XML 要素と属性、参照されているし、マークアップとコード間のリンクが確立されています。

## <a name="creating-the-solution"></a>ソリューションの作成

最初の XAML ファイルの編集を開始するには、Visual Studio または Visual Studio for Mac を使用して新しい Xamarin.Forms ソリューションを作成します。 (環境に合わせて下にあるタブを選択します)。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

、Windows で Visual Studio を使用して**ファイル > 新規 > プロジェクト** メニューから。 **新しいプロジェクト**ダイアログ ボックスで、 **Visual C# > クロス プラットフォーム**、左側にあるし、**モバイル アプリ (Xamarin.Forms)** センターの一覧から。 

![](get-started-with-xaml-images/win/newprojectdialog.w157.png "新しいプロジェクト ダイアログ ボックス")

ソリューションの場所を選択の名前を付けます**XamlSamples** (またはに応じて)、キーを押します**OK**します。

次の画面で選択、**空のアプリ**テンプレートと **.NET Standard**コード共有方法。

![](get-started-with-xaml-images/win/newcrossplatformapp.png "新しいアプリのダイアログ")

**[OK]** を押します。 

4 つのプロジェクトがソリューションに作成されます。 **XamlSamples** .NET Standard ライブラリは、 **XamlSamples.Android**、 **XamlSamples.iOS**、およびユニバーサル Windows プラットフォームソリューション、 **XamlSamples.UWP**します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual studio for Mac では、次のように選択します。**ファイル > 新しいソリューション** メニューから。 **新しいプロジェクト**ダイアログ ボックスで、**マルチプラット フォーム > アプリ**、左側にあると**空白フォームのアプリ**(*いない***フォーム アプリ**) テンプレートの一覧から。

![](get-started-with-xaml-images/mac/newprojectdialog1.png "新しいプロジェクト ダイアログ ボックス 1")

キーを押して**次**します。

次のダイアログで、プロジェクトに名前を付けますの**XamlSamples** (またはに応じて)。 確認します、**使用して .NET Standard**オプション ボタンを選択します。

![](get-started-with-xaml-images/mac/newprojectdialog2.png "新しいプロジェクト ダイアログ ボックス 2")

キーを押して**次**します。 

次のダイアログ ボックスで、プロジェクトの場所を選択できます。

![](get-started-with-xaml-images/mac/newprojectdialog3.png "新しいプロジェクト ダイアログ 3")

キーを押して**作成**

3 つのプロジェクトがソリューションに作成されます。 **XamlSamples** .NET Standard ライブラリは、 **XamlSamples.Android**、および**XamlSamples.iOS**します。 

-----

作成した後、 **XamlSamples**ソリューションでは、ソリューションのスタートアップ プロジェクトとしてさまざまなプラットフォーム プロジェクトを選択して、開発環境をテストして、によって作成されたビルドと展開の簡単なアプリケーションphone エミュレーターまたは実際のデバイスのいずれかのプロジェクト テンプレートです。

プラットフォーム固有の共有コードを記述する必要がある場合を除き、 **XamlSamples** .NET Standard ライブラリ プロジェクトは、時間があるほぼすべてのプログラミングにかかる時間。 これらの記事はそのプロジェクトの外部で危険は冒しませんされません。

### <a name="anatomy-of-a-xaml-file"></a>XAML ファイルの構造

内で、 **XamlSamples** .NET Standard ライブラリは次の名前を持つファイルのペア。

- **App.xaml**、XAML ファイルと
- **App.xaml.cs**、 C# *コード ビハインド*XAML ファイルに関連付けられているファイル。

次に、矢印をクリックする必要があります**App.xaml**に分離コード ファイルを参照してください。 

両方**App.xaml**と**App.xaml.cs**という名前のクラスに寄与`App`から派生した`Application`します。 派生したクラスを XAML ファイルとその他のほとんどのクラスが投稿`ContentPage`。 それらのファイルでは、XAML を使用して、ページ全体の視覚的内容を定義します。 これは、他の 2 つのファイルの場合は true、 **XamlSamples**プロジェクト。

- **MainPage.xaml**、XAML ファイルと
- **MainPage.xaml.cs**、C#分離コード ファイル。

**MainPage.xaml** (ただし、書式設定は、少し異なる場合があります) ファイルが次のようになります。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:XamlSamples"
             x:Class="XamlSamples.MainPage">

    <StackLayout>
        <!-- Place new controls here -->
        <Label Text="Welcome to Xamarin Forms!" 
               VerticalOptions="Center" 
               HorizontalOptions="Center" />
    </StackLayout>

</ContentPage>
```

2 つの XML 名前空間 ( `xmlns`) の宣言は、Uri、Xamarin の web サイトの見かけ上 1 つ目と Microsoft's で 1 秒間を参照してください。 これらどのような Uri ポイントを確認する必要はありません。 何もありません。 Xamarin と Microsoft によって所有されている Uri だけあり、基本的にバージョンの識別子として機能します。

最初の XML 名前空間宣言では、プレフィックスのない XAML ファイル内で定義されているタグがたとえば参照、Xamarin.Forms のクラスにことを意味`ContentPage`します。 2 番目の名前空間宣言のプレフィックスを定義する`x`します。 これは、使用のいくつかの要素とは、XAML に固有の属性と自体が XAML の他の実装でサポートされています。 ただし、これらの要素と属性では、URI に埋め込まれた年によって多少異なります。 Xamarin.Forms には、2009 XAML 仕様が、すべてのサポートしています。

`local`名前空間の宣言では、.NET Standard ライブラリ プロジェクトからその他のクラスにアクセスできます。

その最初のタグの最後に、`x`という名前の属性にプレフィックスが使用`Class`します。 ため、この使用`x`プレフィックスはなどの XAML 名前空間で XAML 属性事実上ユニバーサル`Class`としてほとんどの場合に参照されます`x:Class`。

`x:Class`属性を完全修飾 .NET クラス名を指定します。`MainPage`クラス、`XamlSamples`名前空間。 つまり、この XAML ファイルをという名前の新しいクラスを定義する`MainPage`で、`XamlSamples`名前空間から派生した`ContentPage`— をタグ、`x:Class`属性が表示されます。

`x:Class`属性は、派生を定義する XAML ファイルのルート要素にのみ表示C#クラス。 これは、XAML ファイルで定義されている唯一の新しいクラスです。 XAML ファイルに表示される他のすべてが代わりに単に既存のクラスからインスタンス化し、初期化します。

**MainPage.xaml.cs**次のようなファイル (とは別に使用されていない`using`ディレクティブ)。

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

`MainPage`クラスから派生`ContentPage`、ただし、`partial`クラスの定義。 これは示しての別の部分クラス定義が存在する必要があります`MainPage`、それがでしょうか。 新機能および`InitializeComponent`メソッドでしょうか。 

Visual Studio では、プロジェクトをビルド、生成する XAML ファイルを解析するC#コード ファイル。 検索する場合、 **XamlSamples\XamlSamples\obj\Debug**ディレクトリ、という名前のファイルを検索します**XamlSamples.MainPage.xaml.g.cs**します。 "G"は、生成されるを意味します。 これは、その他の部分クラス定義の`MainPage`の定義を格納している、`InitializeComponent`から呼び出されるメソッド、`MainPage`コンス トラクター。 これら 2 つの部分`MainPage`まとめてクラス定義はコンパイルされます。 によって、XAML がコンパイルされたかどうか、XAML ファイルまたは XAML ファイルのバイナリ形式のいずれかが実行可能ファイルに埋め込まれます。

コードの特定のプラットフォーム プロジェクトの呼び出しで、実行時に、`LoadApplication`の新しいインスタンスを渡して、メソッド、 `App` .NET Standard ライブラリのクラス。 `App`クラスのコンス トラクターをインスタンス化`MainPage`します。 そのクラスのコンス トラクターを呼び出す`InitializeComponent`、呼び出す、`LoadFromXaml`メソッドを .NET Standard ライブラリから XAML ファイル (または、コンパイルされたバイナリ) を抽出します。 `LoadFromXaml` XAML ファイルで定義されたすべてのオブジェクトを初期化します、親子関係ですべてをまとめて接続する、イベント、XAML ファイルで設定するためのコードで定義されているイベント ハンドラーをアタッチしますおよびページのコンテンツとしてオブジェクトの結果のツリーを設定します。

通常は、生成されるコード ファイルで多くの時間を費やす必要はありませんが場合がありますランタイム例外が発生しますのコード生成されたファイルのため、これらに精通しておく必要があります。

コンパイルして、このプログラムを実行するときに、`Label`が示すように、XAML 要素は、ページの中央に表示されます。 左から右への 3 つのプラットフォームは、iOS、Android、および UWP には。

[![](get-started-with-xaml-images/xamlsamples.png "既定の Xamarin.Forms 表示")](get-started-with-xaml-images/xamlsamples-large.png#lightbox "Xamarin.Forms の既定の表示")

さらに興味深いビジュアルは、必要な詳細は XAML の興味深いです。

## <a name="adding-new-xaml-pages"></a>新しい XAML ページを追加します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

その他の XAML ベースを追加する`ContentPage`をプロジェクトにクラスを選択、 **XamlSamples** .NET Standard ライブラリ プロジェクトし、呼び出す、**プロジェクト > 新しい項目の追加**メニュー項目。 左側にある、**新しい項目の追加**ダイアログ ボックスで、 **Visual C#** と**Xamarin.Forms**します。 一覧から選択**コンテンツ ページ**(いない**コンテンツ ページ (C#)**、コードのみ ページを作成するまたは**コンテンツ ビュー**、ページではない)。 ページの名前、たとえば、 **HelloXamlPage.xaml**:

![](get-started-with-xaml-images/win/addnewitemdialog.w157.png "新しい項目 ダイアログ ボックスを追加します。")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

その他の XAML ベースを追加する`ContentPage`をプロジェクトにクラスを選択、 **XamlSamples** .NET Standard ライブラリ プロジェクトし、呼び出す、**ファイル > 新しいファイル**メニュー項目。 左側にある、**新しいファイル**ダイアログ ボックスで、**フォーム**、左側にあると**フォーム ContentPage Xaml** (いない**フォーム ContentPage**をコードのみ ページを作成または**コンテンツ ビュー**ページではない)。 ページの名前、たとえば、 **HelloXamlPage**:

![](get-started-with-xaml-images/mac/newfiledialog.png "新しいファイル ダイアログ")

-----

2 つのファイルがプロジェクトに追加された**HelloXamlPage.xaml**と分離コード ファイル**HelloXamlPage.xaml.cs**します。 

## <a name="setting-page-content"></a>ページのコンテンツの設定

編集、 **HelloXamlPage.xaml**唯一のタグは、のようにファイル`ContentPage`と`ContentPage.Content`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage">
    <ContentPage.Content>
        
    </ContentPage.Content>
</ContentPage>
```

`ContentPage.Content`タグは、XAML の固有の構文の一部です。 最初は、無効な XML に表示される可能性がありますは有効です。 期間は、XML の特殊文字ではありません。

`ContentPage.Content`タグが呼び出されます*プロパティ要素*タグ。 `Content` プロパティである`ContentPage`とは、通常 1 つのビューまたは子ビューのレイアウトを設定します。 プロパティが通常、XAML 内の属性になりますが、設定するは困難を`Content`属性を複合オブジェクト。 そのため、プロパティは、クラス名とピリオドで区切ったプロパティ名で構成される XML 要素として表されます。 これで、`Content`間プロパティを設定することができます、`ContentPage.Content`タグは、次のように。

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

この時点では、クラス、プロパティ、および XML 間のリレーションシップを明らかにする必要があります。 A Xamarin.Forms クラス (など`ContentPage`または`Label`) XML 要素としての XAML ファイルに表示されます。 そのクラスのプロパティ-など`Title`で`ContentPage`の 7 つのプロパティと`Label`-通常、XML 属性として表示されます。

これらのプロパティの値を設定する多くのショートカットが存在します。 一部のプロパティは基本データ型: など、`Title`と`Text`型のプロパティは、 `String`、`Rotation`の種類は`Double`と`IsVisible`(は`true`既定でし、ここでのみ設定されます図) は型の`Boolean`します。

`HorizontalTextAlignment`プロパティの型は`TextAlignment`、列挙型であります。 列挙型のプロパティ、メンバー名はすべて指定する必要があります。

複雑な型のプロパティ、ただし、コンバーターが使用、XAML を解析するためです。 これらは、クラスから派生する Xamarin.Forms`TypeConverter`します。 多くは、パブリック クラスですが、一部ができません。 この特定の XAML ファイルのこれらのクラスのいくつかは、バック グラウンドの役割を果たします。

-  `LayoutOptionsConverter` `VerticalOptions`プロパティ
-  `FontSizeConverter` `FontSize`プロパティ
-  `ColorTypeConverter` `TextColor`プロパティ

これらのコンバーターは、プロパティの設定の使用可能な構文を制御します。

`ThicknessTypeConverter` 1 つ、2 つ、またはコンマで区切られた 4 つの数値を処理することができます。 1 つの数値は、指定した場合は、4 辺すべてに適用されます。 2 つの数値を左と右の余白を 1 つ目は、2 番目の上部と下部にあります。 4 つの数値は、注文左、上、右、下です。

`LayoutOptionsConverter`のパブリックな静的フィールドの名前を変換することができます、`LayoutOptions`構造体の型の値を`LayoutOptions`します。

`FontSizeConverter`を処理できる、`NamedSize`メンバーまたは数値のフォント サイズ。

`ColorTypeConverter`のパブリックな静的フィールドの名前を受け入れる、`Color`構造体または 16 進数の RGB 値、アルファ チャネルの有無の先頭に番号記号 (#)。 アルファ チャネルなしの構文を次に示します。

 `TextColor="#rrggbb"`

個々 のほとんどの文字は、16 進数です。 アルファ チャネルが含まれる方法を次に示します。

 `TextColor="#aarrggbb">`

アルファ チャネルの場合は、FF は完全に不透明を 00 は完全に透明に留意してください。

その他の 2 つの形式は、各チャネルの 1 つの 16 進のみを指定することを許可します。

 `TextColor="#rgb"` `TextColor="#argb"`

このような場合は、値に数字が繰り返されます。 たとえば、#CF3 は、RGB カラー CC-FF-33 です。

## <a name="page-navigation"></a>ページ ナビゲーション

実行すると、 **XamlSamples**プログラム、`MainPage`が表示されます。 新しいに`HelloXamlPage`として新しいスタートアップでページを設定するか、 **App.xaml.cs**ファイル、またはから新しいページに移動`MainPage`します。

ナビゲーションを実装するには、最初のコードを変更、 **App.xaml.cs**コンス トラクターを`NavigationPage`オブジェクトが作成されます。

```csharp
public App()
{
    InitializeComponent();
    MainPage = new NavigationPage(new MainPage());
}
```

**MainPage.xaml.cs**コンス トラクターは、単純なを作成する`Button`に移動するイベント ハンドラーを使用して`HelloXamlPage`:

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

設定、 `Content` 、ページのプロパティの設定が置き換えられます、 `Content` XAML ファイルのプロパティ。 コンパイルし、このプログラムの新しいバージョンを展開すると、画面にボタンが表示されます。 移動する、キーを押して`HelloXamlPage`します。 IPhone、Android、および UWP の結果ページを次に示します。

[![](get-started-with-xaml-images/helloxaml1.png "ラベルのテキストを回転")](get-started-with-xaml-images/helloxaml1-large.png#lightbox "ラベルのテキストの回転")

移動できます`MainPage`を使用して、 **< 戻る**ios では、ページの上部にある、または android では、電話の下部にある左向きの矢印を使用して、または Windows 10 ではページの上部にある左向きの矢印を使用してボタンをクリックします。

自由に表示するために別の方法については、XAML で試して、`Label`します。 テキストに任意の Unicode 文字を埋め込む必要がある場合は、標準の XML 構文を使用できます。 たとえば、スマート引用符で囲まれた応答メッセージを配置する次のように使用します。

 `<Label Text="&#x201C;Hello, XAML!&#x201D;" … />`

次のような見た目に示します。

[![](get-started-with-xaml-images/helloxaml2.png "Unicode 文字がラベルのテキストを回転")](get-started-with-xaml-images/helloxaml2-large.png#lightbox "Unicode 文字がラベルのテキストの回転")

## <a name="xaml-and-code-interactions"></a>XAML とコードの相互作用

**HelloXamlPage**サンプルには、1 つだけが含まれています。 `Label`  ページで、これは非常に通常ではありませんが。 ほとんど`ContentPage`派生クラス設定、`Content`プロパティをいくつかのレイアウトを並べ替えるなど、`StackLayout`します。 `Children`のプロパティ、`StackLayout`型定義は`IList<View>`が、型のオブジェクトでは実際に`ElementCollection<View>`コレクションは、複数のビューまたはその他のレイアウトを反映できます。 XAML では、これらの親子リレーションシップが通常の XML 階層で確立されます。 という名前の新しいページの XAML ファイルを示します**XamlPlusCodePage**:。

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

この XAML ファイルが構文的に完了して、外観を次に示します。

[![](get-started-with-xaml-images/xamlpluscode1.png "複数のコントロールをページに")](get-started-with-xaml-images/xamlpluscode1-large.png#lightbox "ページ上の複数のコントロール")

ただし、このプログラムに機能的には不十分なを検討する可能性がしています。 おそらく、`Slider`が発生することになって、`Label`現在の値を表示して、`Button`プログラム内で作業を行うためのものでは、可能性があります。

わかる[パート 4 です。データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)、ジョブの表示、`Slider`値を使用して、`Label`全体を XAML でデータ バインディングで処理できます。 ソリューションのコードに最初に表示すると便利です。 そうであっても、処理、`Button`コードが必要です をクリックします。 つまり、分離コード ファイルを`XamlPlusCodePage`のハンドラーを含める必要があります、`ValueChanged`のイベント、`Slider`と`Clicked`のイベント、`Button`します。 それらを追加しましょう。

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

これらのイベント ハンドラーは、パブリックにする必要はありません。

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

プロパティに値を割り当てることと同じ構文を持つイベントにハンドラーを割り当てることに注意してください。

場合のハンドラー、`ValueChanged`のイベント、`Slider`を使用する、`Label`ハンドラーは、現在の値を表示するコードからそのオブジェクトを参照する必要があります。 `Label` 、名前で指定する必要があります、`x:Name`属性。

```xaml
<Label x:Name="valueLabel"
       Text="A simple Label"
       Font="Large"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand" />
```

`x`のプレフィックス、`x:Name`属性は、この属性が XAML に固有であることを示します。

割り当てる名前、`x:Name`属性が同じルールC#変数名。 たとえば、文字またはアンダー スコアで始まるし、埋め込みスペースを含めずにする必要があります。

これで、`ValueChanged`イベント ハンドラーを設定できます、`Label`を表示、新しい`Slider`値。 新しい値は、イベント引数から入手できます。

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = args.NewValue.ToString("F3");
}
```

または、ハンドラーを入手できます、`Slider`からこのイベントを生成しているオブジェクト、`sender`引数を取得し、`Value`からのプロパティ。

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = ((Slider)sender).Value.ToString("F3");
}
```

最初に、プログラムを実行すると、`Label`が表示されない、`Slider`ため、その値、`ValueChanged`イベントがまだ発生していません。 操作が、`Slider`により値が表示されます。

[![](get-started-with-xaml-images/xamlpluscode2.png "スライダーの値が表示される")](get-started-with-xaml-images/xamlpluscode2-large.png#lightbox "スライダーの値が表示されます")

用に今すぐ、`Button`します。 応答をシミュレーションしてみましょう、`Clicked`イベントのアラートを表示することによって、`Text`ボタンの。 イベント ハンドラーは安全にキャストできます、`sender`への引数、`Button`し、そのプロパティをアクセスします。

```csharp
async void OnButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;
    await DisplayAlert("Clicked!",
        "The button labeled '" + button.Text + "' has been clicked",
        "OK");
}
```

メソッドとは見なさ`async`ため、`DisplayAlert`メソッドは非同期でありで始まる必要があります、`await`演算子、メソッドの完了時に返されます。 このメソッドが取得されるため、`Button`からイベントを発生させる、`sender`引数、複数のボタンの同じハンドラーを使用できます。

XAML で定義されているオブジェクトが、分離コード ファイルで処理されるイベントを起動できることと、分離コード ファイルを使って割り当てられた名前を使用して XAML で定義されているオブジェクトにアクセスできることを確認した、`x:Name`属性。 これらは、コードと XAML を操作する 2 つの基本的な方法です。

XAML の動作を新しく生成されたを調べることで得られる方法にいくつか追加のインサイト**XamlPlusCode.xaml.g.cs ファイル**、するようになったにはいずれかに割り当てられている任意の名前が含まれています`x:Name`プライベート フィールドとして属性。 そのファイルの簡略化されたバージョンを次に示します。

```csharp
public partial class XamlPlusCodePage : ContentPage {

    private Label valueLabel;

    private void InitializeComponent() {
        this.LoadFromXaml(typeof(XamlPlusCodePage));
        valueLabel = this.FindByName<Label>("valueLabel");
    }
}
```

このフィールドの宣言により、変数内で使用される自由に、`XamlPlusCodePage`管轄下の部分クラス ファイル。 実行時に、フィールドは、XAML が解析された後に割り当てられます。 つまり、`valueLabel`フィールドは`null`ときに、`XamlPlusCodePage`コンス トラクターの後に、有効な開始`InitializeComponent`が呼び出されます。

後`InitializeComponent`コンス トラクターにコントロールを返します、ビジュアル、ページの構築が完了した場合と同様をインスタンス化し、コードで初期化されている必要があります。 XAML ファイルは、不要になったクラスのすべての役割を果たします。 ページで、希望どおり、たとえば、ビューを追加することによってこれらのオブジェクトを操作することができます、 `StackLayout`、または設定、`Content`以外に、ページのプロパティだけです。 「について、ツリー」を調べることで、`Content`ページとページ内の項目のプロパティ、`Children`レイアウトのコレクション。 この方法でアクセス ビューのプロパティを設定またはにイベント ハンドラーを動的に割り当てることができます。

かまいません。 ページ、および XAML がそのコンテンツをビルドするツールのみです。

## <a name="summary"></a>まとめ

概要については、このクラスの定義に、XAML ファイルとコード ファイルが協力する方法と、XAML とコード ファイルを操作する方法を説明しました。 XAML も非常に柔軟な方法で使用できるようにする独自の構文機能を備えています。 これらの探索を開始できます[第 2 部です。重要な XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)します。



## <a name="related-links"></a>関連リンク

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [第 2 部基本的な XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [第 3 部XAML マークアップ拡張](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [第 4 部データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [第 5 部MVVM へのデータ バインディングから](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
