---
title: "実行時の XAML の読み込み Xamarin.Forms " 説明: "xaml は、LoadFromXaml 拡張メソッドを使用して実行時に読み込んで解析できます。"
ms. 製品: xamarin ms. assetid: 25F73FBF-2DD3-468E-A2D8-0897414F0F4A: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 12/12/2018 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="loading-xaml-at-runtime-in-xamarinforms"></a>実行時の XAML の読み込みXamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)

名前空間には、 [`Xamarin.Forms.Xaml`](xref:Xamarin.Forms.Xaml) [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 実行時に XAML を読み込んで解析するために使用できる2つの拡張メソッドが含まれています。

## <a name="background"></a>背景

Xamarin.FormsXAML クラスが構築されると、 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) メソッドは間接的に呼び出されます。 このエラーは、XAML クラスの分離コードファイルがコンストラクターからメソッドを呼び出すことが原因で発生し `InitializeComponent` ます。

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

Visual Studio は、XAML ファイルを含むプロジェクトをビルドするときに、メソッドの定義を含む C# コードファイル (たとえば、 **MainPage.xaml.g.cs**) を生成するために xaml ファイルを解析し `InitializeComponent` ます。

```csharp
private void InitializeComponent()
{
    global::Xamarin.Forms.Xaml.Extensions.LoadFromXaml(this, typeof(MainPage));
    ...
}
```

メソッドは、メソッドを呼び出して、 `InitializeComponent` [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) XAML ファイル (またはコンパイルされたバイナリ) を .NET Standard ライブラリから抽出します。 抽出後、XAML ファイルで定義されているすべてのオブジェクトを初期化し、それらを親子のリレーションシップですべて連結し、コードで定義されているイベントハンドラーを XAML ファイルに設定されたイベントにアタッチし、オブジェクトの結果ツリーをページのコンテンツとして設定します。

## <a name="loading-xaml-at-runtime"></a>実行時の XAML の読み込み

これらの [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) メソッドは `public` であるため、アプリケーションから呼び出して、 Xamarin.Forms 実行時に XAML を読み込んで解析することができます。 これにより、アプリケーションが web サービスから XAML をダウンロードし、XAML から必要なビューを作成して、アプリケーションで表示するなどのシナリオが可能になります。

> [!WARNING]
> 実行時の XAML の読み込みにはパフォーマンスコストが非常に高く、通常は避ける必要があります。

次のコード例は、簡単な使用方法を示しています。

```csharp
using Xamarin.Forms.Xaml;
...

string navigationButtonXAML = "<Button Text=\"Navigate\" />";
Button navigationButton = new Button().LoadFromXaml(navigationButtonXAML);
...
_stackLayout.Children.Add(navigationButton);
```

この例では、 [`Button`](xref:Xamarin.Forms.Button) インスタンスが作成され、その [`Text`](xref:Xamarin.Forms.Button.Text) プロパティ値がで定義された XAML から設定され `string` ます。 `Button`次に、が、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) ページの XAML で定義されているに追加されます。

> [!NOTE]
> 拡張メソッドを使用すると、 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) ジェネリック型引数を指定できます。 ただし、型引数を指定する必要はほとんどありません。これは、操作対象のインスタンスの型から推論されるためです。

メソッドを使用すると、 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 次の例の拡大を使用して XAML を展開でき [`ContentPage`](xref:Xamarin.Forms.ContentPage) ます。

```csharp
using Xamarin.Forms.Xaml;
...

// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n</ContentPage>";

ContentPage page = new ContentPage().LoadFromXaml(pageXAML);
await Navigation.PushAsync(page);
```

## <a name="accessing-elements"></a>アクセス (要素に)

メソッドを使用して実行時に XAML を読み込むと、 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) (を使用して) 指定されたランタイムオブジェクト名を持つ xaml 要素への厳密に型指定されたアクセスが許可されません `x:Name` 。 ただし、これらの XAML 要素は、メソッドを使用して取得 [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) し、必要に応じてアクセスできます。

```csharp
// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n<StackLayout>\n<Label x:Name=\"monkeyName\"\n />\n</StackLayout>\n</ContentPage>";
ContentPage page = new ContentPage().LoadFromXaml(pageXAML);

Label monkeyLabel = page.FindByName<Label>("monkeyName");
monkeyLabel.Text = "Seated Monkey";
...
```

この例では、の XAML が大きくなってい [`ContentPage`](xref:Xamarin.Forms.ContentPage) ます。 この XAML には [`Label`](xref:Xamarin.Forms.Label) 、 `monkeyName` [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) プロパティが設定される前に、メソッドを使用して取得されるという名前のが含まれてい [`Text`](xref:Xamarin.Forms.Label.Text) ます。

## <a name="related-links"></a>関連リンク

- [LoadRuntimeXAML (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)
