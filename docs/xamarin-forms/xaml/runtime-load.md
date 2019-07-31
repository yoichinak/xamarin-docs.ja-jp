---
title: 実行時の Xamarin 形式での XAML の読み込み
description: XAML は、LoadFromXaml 拡張メソッドを使用して実行時に読み込んで解析することができます。
ms.prod: xamarin
ms.assetid: 25F73FBF-2DD3-468E-A2D8-0897414F0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/12/2018
ms.openlocfilehash: e253d2ba949a94637d7773fdc50b479679fd3f41
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657248"
---
# <a name="loading-xaml-at-runtime-in-xamarinforms"></a>実行時の Xamarin 形式での XAML の読み込み

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)

名前空間には[`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) 、実行時に XAML を読み込んで解析するために使用できる2つの拡張メソッドが含まれています。 [`Xamarin.Forms.Xaml`](xref:Xamarin.Forms.Xaml)

## <a name="background"></a>背景

Xamarin の XAML クラスが構築されると、 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)メソッドは間接的に呼び出されます。 このエラーは、XAML クラスの分離コードファイルがコンストラクターからメソッド`InitializeComponent`を呼び出すことが原因で発生します。

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

Visual Studio は、xaml ファイルを含むプロジェクトをビルドするときに、 C# `InitializeComponent`メソッドの定義を含むコードファイル (たとえば、 **MainPage.xaml.g.cs**) を生成するために xaml ファイルを解析します。

```csharp
private void InitializeComponent()
{
    global::Xamarin.Forms.Xaml.Extensions.LoadFromXaml(this, typeof(MainPage));
    ...
}
```

メソッド`InitializeComponent`は、 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)メソッドを呼び出して、XAML ファイル (またはコンパイルされたバイナリ) を .NET Standard ライブラリから抽出します。 抽出後、XAML ファイルで定義されているすべてのオブジェクトを初期化し、それらを親子のリレーションシップですべて連結し、コードで定義されているイベントハンドラーを XAML ファイルに設定されたイベントにアタッチし、オブジェクトの結果ツリーをのコンテンツとして設定します。改.

## <a name="loading-xaml-at-runtime"></a>実行時の XAML の読み込み

これらの`public`メソッドはであるため、Xamarin. Forms アプリケーションから呼び出して、実行時に XAML を読み込んで解析することができます。 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) これにより、アプリケーションが web サービスから XAML をダウンロードし、XAML から必要なビューを作成して、アプリケーションで表示するなどのシナリオが可能になります。

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

この例[`Button`](xref:Xamarin.Forms.Button)では、インスタンスが作成されます。 [`Text`](xref:Xamarin.Forms.Button.Text)このインスタンスのプロパティ値は、 `string`で定義されている XAML から設定されます。 `Button` [次`StackLayout`](xref:Xamarin.Forms.StackLayout)に、が、ページの XAML で定義されているに追加されます。

> [!NOTE]
> 拡張[`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)メソッドを使用すると、ジェネリック型引数を指定できます。 ただし、型引数を指定する必要はほとんどありません。これは、操作対象のインスタンスの型から推論されるためです。

メソッドを使用すると、次の例の拡大を[`ContentPage`](xref:Xamarin.Forms.ContentPage)使用して XAML を展開できます。 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)

```csharp
using Xamarin.Forms.Xaml;
...

// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n</ContentPage>";

ContentPage page = new ContentPage().LoadFromXaml(pageXAML);
await Navigation.PushAsync(page);
```

## <a name="accessing-elements"></a>アクセス (要素に)

メソッドを使用して実行[`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)時に xaml を読み込むと、(を使用して`x:Name`) 指定されたランタイムオブジェクト名を持つ xaml 要素への厳密に型指定されたアクセスが許可されません。 ただし、これらの XAML 要素は、 [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*)メソッドを使用して取得し、必要に応じてアクセスできます。

```csharp
// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n<StackLayout>\n<Label x:Name=\"monkeyName\"\n />\n</StackLayout>\n</ContentPage>";
ContentPage page = new ContentPage().LoadFromXaml(pageXAML);

Label monkeyLabel = page.FindByName<Label>("monkeyName");
monkeyLabel.Text = "Seated Monkey";
...
```

この例では、の[`ContentPage`](xref:Xamarin.Forms.ContentPage) XAML が大きくなっています。 この XAML には[`Label`](xref:Xamarin.Forms.Label) 、 `monkeyName` [`Text`](xref:Xamarin.Forms.Label.Text)プロパティが設定される前[`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*)に、メソッドを使用して取得されるという名前のが含まれています。

## <a name="related-links"></a>関連リンク

- [LoadRuntimeXAML (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)
