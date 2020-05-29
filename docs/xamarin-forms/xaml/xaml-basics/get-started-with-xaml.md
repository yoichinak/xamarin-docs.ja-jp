---
title: "パート 1. XAML の "説明:" のはじめに Xamarin.Forms アプリケーションでは、xaml は主にページのビジュアルコンテンツを定義するために使用され、分離コードファイルと連動します。 "
ms. 製品: xamarin ms. assetid: 9073FA0E-BD5A-4492-8A93-54C466F6EDB9: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 09/30/2019 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="part-1-getting-started-with-xaml"></a>第 1 部 XAML の概要

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)

_アプリケーションで Xamarin.Forms は、XAML はほとんどの場合、ページのビジュアルコンテンツを定義するために使用され、C# 分離コードファイルと連携します。_

分離コードファイルは、マークアップのコードサポートを提供します。 これらの2つのファイルは、子ビューとプロパティの初期化を含む新しいクラス定義に寄与します。 XAML ファイル内では、クラスとプロパティは XML 要素と属性を使用して参照され、マークアップとコードの間のリンクが確立されます。

## <a name="creating-the-solution"></a>ソリューションの作成

最初の XAML ファイルの編集を開始するには、Visual Studio または Visual Studio for Mac を使用して、新しいソリューションを作成 Xamarin.Forms します。 (環境に応じた下のタブを選択してください。)

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Windows で Visual Studio 2019 を起動し、[スタート] ウィンドウで [**新しいプロジェクトの作成**] をクリックして新しいプロジェクトを作成します。

![新しいソリューションウィンドウ](get-started-with-xaml-images/win/new-solution-2019.png)

[**新しいプロジェクトの作成**] ウィンドウで、[プロジェクトの**種類**] ボックスの一覧の [ **mobile** ] を選択し、[**モバイルアプリ ( Xamarin.Forms )** ] テンプレートを選択して、[**次へ**] ボタンをクリックします。

![[新しいプロジェクト] ウィンドウ](get-started-with-xaml-images/win/new-project-2019.png)

[**新しいプロジェクトの構成**] ウィンドウで、**プロジェクト名**を**xamlsamples** (または任意のもの) に設定し、[**作成**] ボタンをクリックします。

[**新しいクロスプラットフォームアプリ**] ダイアログで、[**空白**] をクリックし、[ **OK** ] ボタンをクリックします。

![新しいアプリダイアログ](get-started-with-xaml-images/win/new-cross-platform-app.png)

このソリューションには、次の4つのプロジェクトが作成されます。 **xamlsamples** .NET Standard library、Xamlsamples **.** **IOS**、ユニバーサル WINDOWS プラットフォームソリューション、 **xamlsamples。 UWP**。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac で、メニューから [**ファイル > 新しいソリューション**] を選択します。 [**新しいプロジェクト**] ダイアログボックスで、左側にある [**マルチプラットフォーム > アプリ**] を選択し、テンプレートの一覧から**空のフォームアプリ**(**フォームアプリ**では*ありません*) を選択します。

![[新しいプロジェクト] ダイアログ1](get-started-with-xaml-images/mac/newprojectdialog1.png)

[**次へ**] をクリックします。

次のダイアログボックスで、プロジェクトに**Xamlsamples**の名前 (または任意のもの) を指定します。 [ **.NET Standard を使用**する] オプションボタンが選択されていることを確認します。

![[新しいプロジェクト] ダイアログ2](get-started-with-xaml-images/mac/newprojectdialog2.png)

[**次へ**] をクリックします。

次のダイアログボックスで、プロジェクトの場所を選択できます。

![[新しいプロジェクト] ダイアログ3](get-started-with-xaml-images/mac/newprojectdialog3.png)

[**作成**

ソリューション内に3つのプロジェクトが作成されます。 **xamlsamples** .NET Standard Library、 **Xamlsamples. Android**、および**xamlsamples です。**

-----

**Xamlsamples**ソリューションを作成した後、ソリューションのスタートアッププロジェクトとしてさまざまなプラットフォームプロジェクトを選択し、プロジェクトテンプレートによって作成された単純なアプリケーションを phone エミュレーターまたは実際のデバイスに構築してデプロイすることで、開発環境をテストできます。

プラットフォーム固有のコードを記述する必要がある場合を除いて、共有されている**Xamlsamples** .NET Standard ライブラリプロジェクトには、ほぼすべてのプログラミング時間が費やされます。 これらの記事は、そのプロジェクトの外部では実行されません。

### <a name="anatomy-of-a-xaml-file"></a>XAML ファイルの構造

**Xamlsamples** .NET Standard ライブラリには、次の名前を持つファイルのペアが含まれています。

- App.xaml **、xaml ファイル、** そして
- **App.xaml.cs**。 xaml ファイルに関連付けられている C#*分離コード*ファイルです。

[ **App.xaml** ] の横にある矢印をクリックすると、分離コードファイルが表示されます。

**App.xaml**と**App.xaml.cs**はどちらも、から派生したという名前のクラスに寄与し `App` `Application` ます。 XAML ファイルを持つその他のほとんどのクラスは、から派生するクラスに寄与します。これらのファイルは、XAML を使用して `ContentPage` ページ全体のビジュアルコンテンツを定義します。 これは、 **Xamlsamples**プロジェクト内の他の2つのファイルに当てはまります。

- **Mainpage.xaml。** xaml ファイルそして
- **MainPage.xaml.cs**は、C# 分離コードファイルです。

**Mainpage.xaml**ファイルは次のようになります (ただし、書式設定は少し異なる場合があります)。

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

2つの XML 名前空間 ( `xmlns` ) 宣言は、最初は Xamarin の web サイトで、もう1つは Microsoft の uri を参照します。 これらの Uri がどのようなものかを確認することは避けてください。 何もありません。 これらは、Xamarin と Microsoft が所有する単なる Uri であり、基本的にはバージョン識別子として機能します。

最初の XML 名前空間宣言では、XAML ファイル内でプレフィックスなしのタグがを参照していることを意味し Xamarin.Forms ます。たとえば、のように `ContentPage` なります。 2番目の名前空間宣言は、のプレフィックスを定義し `x` ます。 これは、XAML 自体に固有であり、XAML の他の実装でサポートされているいくつかの要素と属性に使用されます。 ただし、これらの要素と属性は、URI に埋め込まれている年によって若干異なります。 Xamarin.Formsでは、2009 XAML 仕様がサポートされていますが、そのすべてはサポートされていません。

`local`名前空間の宣言を使用すると、.NET Standard ライブラリプロジェクトから他のクラスにアクセスできます。

最初のタグの最後に、 `x` という名前の属性にプレフィックスが使用され `Class` ます。 このプレフィックスの使用は、 `x` xaml 名前空間では実質的に汎用的であるため、などの xaml 属性は、ほとんどの場合、と `Class` 呼ばれ `x:Class` ます。

属性は、 `x:Class` 完全修飾 .net クラス名 `MainPage` (名前空間のクラス) を指定します `XamlSamples` 。 つまり、この XAML ファイルは、から派生した名前空間内のという名前の新しいクラスを定義します。これは、 `MainPage` `XamlSamples` 属性が `ContentPage` 表示されるタグ `x:Class` です。

属性は、 `x:Class` 派生 C# クラスを定義するために、XAML ファイルのルート要素でのみ使用できます。 これは、XAML ファイルで定義されている唯一の新しいクラスです。 XAML ファイルに表示されるその他のものはすべて、単に既存のクラスからインスタンス化され、初期化されます。

**MainPage.xaml.cs**ファイルは次のようになります (未使用の `using` ディレクティブ以外)。

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

`MainPage`クラスはから派生し `ContentPage` ますが、クラス定義に注意して `partial` ください。 これは、の別の部分クラス定義が必要であることを示してい `MainPage` ますが、ここではを使用します。 その方法を教えて `InitializeComponent` ください。

Visual Studio によってプロジェクトがビルドされると、XAML ファイルが解析され、C# コードファイルが生成されます。 **XamlSamples\XamlSamples\obj\Debug**ディレクトリを調べると、 **XamlSamples.MainPage.xaml.g.cs**という名前のファイルが表示されます。 ' G ' は、生成されたを表します。 これは、 `MainPage` `InitializeComponent` コンストラクターから呼び出されたメソッドの定義を含むのその他の部分クラス定義です `MainPage` 。 これらの2つの部分 `MainPage` クラス定義を一緒にコンパイルできます。 Xaml がコンパイルされているかどうかによって、xaml ファイルまたはバイナリ形式の XAML ファイルが実行可能ファイルに埋め込まれます。

実行時に、特定のプラットフォームプロジェクトのコードは `LoadApplication` メソッドを呼び出し、.NET Standard ライブラリのクラスの新しいインスタンスに渡し `App` ます。 `App`クラスコンストラクターがインスタンス化し `MainPage` ます。 このクラスのコンストラクターは `InitializeComponent` 、を呼び出し、 `LoadFromXaml` XAML ファイル (またはコンパイルされたバイナリ) を抽出するメソッドを .NET Standard ライブラリから呼び出します。 `LoadFromXaml`XAML ファイルで定義されているすべてのオブジェクトを初期化し、それらを親子のリレーションシップですべて連結し、コードで定義されているイベントハンドラーを XAML ファイルに設定されたイベントにアタッチし、オブジェクトの結果ツリーをページの内容として設定します。

通常は、生成されたコードファイルを使用する必要はあまりありませんが、ランタイム例外が生成されたファイルのコードに対して発生することがあるので、理解しておく必要があります。

このプログラムをコンパイルして実行すると、 `Label` XAML が示すように、要素がページの中央に表示されます。

[![既定の Xamarin.Forms 表示](get-started-with-xaml-images/xamlsamples.png)](get-started-with-xaml-images/xamlsamples-large.png#lightbox)

より興味深いビジュアルについては、さらに興味深い XAML が必要です。

## <a name="adding-new-xaml-pages"></a>新しい XAML ページの追加

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

他の XAML ベースのクラスをプロジェクトに追加するに `ContentPage` は、 **xamlsamples** .NET Standard ライブラリプロジェクトを選択し、右クリックして、[ **> 新しい項目の追加**] を選択します。[**新しい項目の追加**] ダイアログボックスで、[ **Visual C# 項目 > Xamarin.Forms > コンテンツページ**( **C#)**] を選択します。このページでは、コードのみのページを作成するか、ページではない**コンテンツビュー**を作成します。 ページに名前を付けます。たとえば、 **HelloXamlPage**のように指定します。

![[新しい項目の追加] ダイアログ](get-started-with-xaml-images/win/add-new-item-dialog-2019.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

他の XAML ベースのクラスをプロジェクトに追加するには、 `ContentPage` **xamlsamples** .NET Standard ライブラリプロジェクトを選択し、[**ファイル > 新しいファイル**] メニュー項目を呼び出します。 [**新しいファイル**] ダイアログの左側で、左側にある [**フォーム**] を選択します。フォーム**ContentPage Xaml** (コードのみのページを作成するか、ページで**はない****コンテンツビュー**) を選択します。 ページに名前を付けます。たとえば、 **HelloXamlPage**のように指定します。

![[新しいファイル] ダイアログ](get-started-with-xaml-images/mac/newfiledialog.png)

-----

2つのファイルがプロジェクト**HelloXamlPage**と分離コードファイル**HelloXamlPage.xaml.cs**に追加されます。

## <a name="setting-page-content"></a>ページコンテンツの設定

**HelloXamlPage**ファイルを編集して、タグだけがおよびのタグになるようにします `ContentPage` `ContentPage.Content` 。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="XamlSamples.HelloXamlPage">
    <ContentPage.Content>

    </ContentPage.Content>
</ContentPage>
```

タグは、 `ContentPage.Content` XAML の一意の構文の一部です。 最初は無効な XML であるように見えるかもしれませんが、有効であると思われます。 ピリオドは XML の特殊文字ではありません。

`ContentPage.Content`タグは、*プロパティ要素*タグと呼ばれます。 `Content`はのプロパティであり `ContentPage` 、通常は1つのビューまたは子ビューを持つレイアウトに設定されます。 通常、プロパティは XAML の属性になりますが、 `Content` 複雑なオブジェクトに属性を設定するのは困難です。 そのため、プロパティは、クラス名とプロパティ名で構成される、ピリオドで区切られた XML 要素として表されます。 これで、次の `Content` ようにタグ間にプロパティを設定でき `ContentPage.Content` ます。

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

また `Title` 、ルートタグに属性が設定されていることにも注意してください。

現時点では、クラス、プロパティ、および XML 間のリレーションシップが明らかになります。 Xamarin.Forms クラス (やなど) は、 `ContentPage` `Label` XAML ファイルの xml 要素として表示されます。 このクラスのプロパティ ( `Title` `ContentPage` およびの7つのプロパティを含む) `Label` は、通常、XML 属性として表示されます。

多くのショートカットは、これらのプロパティの値を設定するために用意されています。 プロパティには基本的なデータ型があります。たとえば、プロパティとプロパティは型で、は型であり、(既定ではこのように設定されてい `Title` `Text` `String` `Rotation` `Double` `IsVisible` `true` ます) は型 `Boolean` です。

`HorizontalTextAlignment`プロパティは、列挙体である型です `TextAlignment` 。 任意の列挙型のプロパティの場合は、メンバー名だけを指定する必要があります。

ただし、より複雑な型のプロパティの場合は、XAML を解析するためにコンバーターが使用されます。 これらは、 Xamarin.Forms から派生するのクラスです `TypeConverter` 。 多くはパブリッククラスですが、一部はパブリッククラスではありません。 この特定の XAML ファイルでは、これらのクラスのいくつかがバックグラウンドで役割を果たします。

- `LayoutOptionsConverter`プロパティの場合 `VerticalOptions`
- `FontSizeConverter`プロパティの場合 `FontSize`
- `ColorTypeConverter`プロパティの場合 `TextColor`

これらのコンバーターは、プロパティ設定の使用可能な構文を制御します。

は、 `ThicknessTypeConverter` コンマで区切られた1つ、2つ、または4つの数値を処理できます。 1つの数値を指定した場合は、4辺すべてに適用されます。 2つの数値を使用した場合、最初の余白は左および右になり、2番目の数値は上下になります。 4つの数値は、左、上、右、下の順序で並べ替えられます。

は、 `LayoutOptionsConverter` 構造体のパブリック静的フィールドの名前 `LayoutOptions` を型の値に変換でき `LayoutOptions` ます。

は、 `FontSizeConverter` `NamedSize` メンバーまたは数値のフォントサイズを処理できます。

は、 `ColorTypeConverter` 構造体または16進数の RGB 値のパブリック静的フィールドの名前を受け取り `Color` ます。アルファチャネルの有無は、前にシャープ記号 (#) が付きます。 次に、アルファチャネルのない構文を示します。

 `TextColor="#rrggbb"`

各短い文字は、16進数の数字です。 アルファチャネルを含める方法を次に示します。

 `TextColor="#aarrggbb">`

アルファチャネルの場合、FF は完全に不透明であり、00は完全に透明であることに注意してください。

他の2つの形式では、各チャネルに対して1つの16進数字のみを指定できます。

 `TextColor="#rgb"` `TextColor="#argb"`

このような場合は、数字が繰り返されて値が形成されます。 たとえば、#CF3 は RGB カラー (CC-FF-33) です。

## <a name="page-navigation"></a>ページのナビゲーション

**Xamlsamples**プログラムを実行すると、 `MainPage` が表示されます。 新しいを表示するには、 `HelloXamlPage` **App.xaml.cs**ファイルの新しいスタートアップページとして設定するか、から新しいページに移動し `MainPage` ます。

ナビゲーションを実装するには、まず**App.xaml.cs**コンストラクターのコードを変更して、 `NavigationPage` オブジェクトが作成されるようにします。

```csharp
public App()
{
    InitializeComponent();
    MainPage = new NavigationPage(new MainPage());
}
```

**MainPage.xaml.cs**コンストラクターでは、単純なを作成 `Button` し、イベントハンドラーを使用してに移動でき `HelloXamlPage` ます。

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

ページのプロパティを設定すると、 `Content` XAML ファイル内のプロパティの設定が置き換えられ `Content` ます。 このプログラムの新しいバージョンをコンパイルして展開すると、ボタンが画面に表示されます。 キーを押すと、に移動 `HelloXamlPage` します。 IPhone、Android、UWP の結果ページを次に示します。

[![回転したラベルのテキスト](get-started-with-xaml-images/helloxaml1.png)](get-started-with-xaml-images/helloxaml1-large.png#lightbox)

`MainPage`IOS の [ **< 戻る**] ボタンを使用して、ページの上部または Android の電話の下部にある左矢印を使用するか、Windows 10 のページの上部にある左矢印を使用すると、に戻ることができます。

を表示するさまざまな方法について、XAML を自由に試してみて `Label` ください。 Unicode 文字をテキストに埋め込む必要がある場合は、標準の XML 構文を使用できます。 たとえば、あいさつをスマート引用符で囲むには、次のように入力します。

 `<Label Text="&#x201C;Hello, XAML!&#x201D;" … />`

次のようになります。

[![Unicode 文字を使用したラベルテキストの回転](get-started-with-xaml-images/helloxaml2.png)](get-started-with-xaml-images/helloxaml2-large.png#lightbox)

## <a name="xaml-and-code-interactions"></a>XAML とコードの相互作用

**HelloXamlPage**サンプルにはページのが1つだけ含まれてい `Label` ますが、これは非常にまれです。 ほとんど `ContentPage` の派生物では、プロパティは、など、 `Content` 何らかの種類のレイアウトに設定され `StackLayout` ます。 `Children`のプロパティ `StackLayout` は型として定義されていますが、実際には型のオブジェクトであり、 `IList<View>` `ElementCollection<View>` そのコレクションには複数のビューまたはその他のレイアウトを設定できます。 XAML では、これらの親子リレーションシップは通常の XML 階層を使用して確立されます。 **XamlPlusCodePage**という名前の新しいページの XAML ファイルを次に示します。

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

この XAML ファイルは構文的に完成しています。次のようになります。

[![ページ上の複数のコントロール](get-started-with-xaml-images/xamlpluscode1.png)](get-started-with-xaml-images/xamlpluscode1-large.png#lightbox)

しかし、このプログラムは機能的に短さされると考えられます。 は、が現在の値を表示すること `Slider` を想定 `Label` しています。また、は、 `Button` プログラム内で何らかの処理を行うことが意図されています。

(パート4を参照) [。データバインディングの基本](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)、を使用して値を表示するジョブは、 `Slider` データバインディングを使用して `Label` XAML で完全に処理できます。 しかし、code ソリューションを最初に確認すると便利です。 そのような場合でも、クリックを確実に処理するには、 `Button` コードが必要です。 これは、の分離コードファイルに、の `XamlPlusCodePage` イベントおよびのイベントのハンドラーが含まれている必要があることを意味し `ValueChanged` `Slider` `Clicked` `Button` ます。 次のように追加してみましょう。

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

これらのイベントハンドラーは、パブリックである必要はありません。

XAML ファイルに戻り、タグとタグには、 `Slider` `Button` これらのハンドラーを `ValueChanged` 参照するイベントとイベントの属性を含める必要があり `Clicked` ます。

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

イベントにハンドラーを割り当てることは、プロパティに値を割り当てるのと同じ構文を持つことに注意してください。

のイベントのハンドラーが `ValueChanged` 現在の値を表示するためにを `Slider` 使用する場合 `Label` 、ハンドラーはそのオブジェクトをコードから参照する必要があります。 には、 `Label` 属性で指定される名前が必要 `x:Name` です。

```xaml
<Label x:Name="valueLabel"
       Text="A simple Label"
       Font="Large"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand" />
```

`x`属性のプレフィックスは、 `x:Name` この属性が XAML に固有であることを示します。

属性に割り当てる名前には、 `x:Name` C# の変数名と同じ規則があります。 たとえば、文字またはアンダースコアで始まり、空白を埋め込むことはできません。

これで `ValueChanged` 、イベントハンドラーは、 `Label` 新しい値を表示するようにを設定でき `Slider` ます。 新しい値は、イベント引数から取得できます。

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = args.NewValue.ToString("F3");
}
```

または、ハンドラーは、 `Slider` このイベントを生成しているオブジェクトを `sender` 引数から取得し、そのプロパティを取得することもでき `Value` ます。

```csharp
void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
{
    valueLabel.Text = ((Slider)sender).Value.ToString("F3");
}
```

プログラムを最初に実行したときに、 `Label` `Slider` イベントがまだ発生していないため、には値が表示されません `ValueChanged` 。 ただし、の操作 `Slider` によって値が表示されます。

[![表示されるスライダーの値](get-started-with-xaml-images/xamlpluscode2.png)](get-started-with-xaml-images/xamlpluscode2-large.png#lightbox)

`Button`では、 `Clicked`ボタンのを使用してアラートを表示することにより、イベントへの応答をシミュレートし `Text` ます。 イベントハンドラーは、引数をに安全にキャストし、 `sender` `Button` そのプロパティにアクセスできます。

```csharp
async void OnButtonClicked(object sender, EventArgs args)
{
    Button button = (Button)sender;
    await DisplayAlert("Clicked!",
        "The button labeled '" + button.Text + "' has been clicked",
        "OK");
}
```

メソッドはとして定義されています。これは、メソッドが非同期であり、 `async` `DisplayAlert` `await` メソッドの完了時にを返す演算子を使用する必要があるためです。 このメソッドは、引数からイベントを発生させるを取得するため `Button` `sender` 、複数のボタンに同じハンドラーを使用できます。

XAML で定義されたオブジェクトが分離コードファイルで処理されるイベントを発生させることができ、分離コードファイルは、属性を使用して割り当てられた名前を使用して、XAML で定義されたオブジェクトにアクセスできることを確認しました `x:Name` 。 これらは、コードと XAML がやり取りする2つの基本的な方法です。

新たに生成された**XamlPlusCode.xaml.g.cs ファイル**を調べることで、XAML の動作方法に関する追加の洞察を得ることができます。これには、任意の `x:Name` 属性にプライベートフィールドとして割り当てられた任意の名前が含まれるようになりました。 このファイルの簡略化されたバージョンを次に示します。

```csharp
public partial class XamlPlusCodePage : ContentPage {

    private Label valueLabel;

    private void InitializeComponent() {
        this.LoadFromXaml(typeof(XamlPlusCodePage));
        valueLabel = this.FindByName<Label>("valueLabel");
    }
}
```

このフィールドを宣言すると、管轄区域の部分クラスファイル内の任意の場所で自由に変数を使用でき `XamlPlusCodePage` ます。 実行時には、XAML が解析された後にフィールドが割り当てられます。 これは、 `valueLabel` フィールドが `null` コンストラクターの開始時ですが、 `XamlPlusCodePage` が呼び出された後で有効になることを意味し `InitializeComponent` ます。

が `InitializeComponent` 制御をコンストラクターに戻した後、ページのビジュアルは、コードでインスタンス化および初期化された場合と同様に構築されています。 XAML ファイルは、クラス内のどのロールも再生されなくなりました。 ページでこれらのオブジェクトを操作するには、にビューを追加する `StackLayout` か、ページのプロパティを全体に設定するなど、任意の方法を使用でき `Content` ます。 `Content`ページのプロパティとレイアウトのコレクション内の項目を調べて、ツリーをウォークすることができ `Children` ます。 この方法でアクセスされたビューのプロパティを設定したり、イベントハンドラーを動的に割り当てたりすることができます。

自由にお使いいただけます。 これはページです。 XAML は、そのコンテンツを構築するためのツールにすぎません。

## <a name="summary"></a>まとめ

この概要では、XAML ファイルとコードファイルがクラス定義にどのように影響するか、および XAML とコードファイルがどのように対話するかを説明しました。 しかし、XAML には、非常に柔軟な方法で使用できる独自の一意の構文機能もあります。 これらの操作については、[第2部で開始できます。必須の XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)。

## <a name="related-links"></a>関連リンク

- [XamlSamples](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xamlsamples)
- [第2部。必須の XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
- [パート3。XAML マークアップ拡張機能](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [パート4。データバインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
- [パート5。データバインドから MVVM へ](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)
