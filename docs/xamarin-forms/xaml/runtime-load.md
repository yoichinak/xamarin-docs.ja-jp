---
title: 実行時の Xamarin 形式での XAML の読み込み
description: XAML は、LoadFromXaml 拡張メソッドを使用して実行時に読み込んで解析することができます。
ms.prod: xamarin
ms.assetid: 25F73FBF-2DD3-468E-A2D8-0897414F0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/12/2018
ms.openlocfilehash: 71f510cd37d4bed2a5a6077ed63f748ce9894725
ms.sourcegitcommit: ae5557c5024d4b7bd52b2f33cb96114ce2b8e086
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2020
ms.locfileid: "77045072"
---
# <a name="loading-xaml-at-runtime-in-xamarinforms"></a>実行時の Xamarin 形式での XAML の読み込み

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)

[`Xamarin.Forms.Xaml`](xref:Xamarin.Forms.Xaml)名前空間には、実行時に XAML を読み込み、解析するために使用できる2つの[`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)拡張メソッドが含まれています。

## <a name="background"></a>バックグラウンド

Xamarin の XAML クラスが構築されると、 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)メソッドが間接的に呼び出されます。 これは、XAML クラスの分離コードファイルがコンストラクターから `InitializeComponent` メソッドを呼び出すことが原因で発生します。

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

Visual Studio は、XAML ファイルを含むプロジェクトをビルドするときに、XAML ファイルを解析C#して、`InitializeComponent` メソッドの定義を含むコードファイル (たとえば、 **MainPage.xaml.g.cs**) を生成します。

```csharp
private void InitializeComponent()
{
    global::Xamarin.Forms.Xaml.Extensions.LoadFromXaml(this, typeof(MainPage));
    ...
}
```

`InitializeComponent` メソッドは、 [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)メソッドを呼び出して、.NET Standard ライブラリから XAML ファイル (またはコンパイルされたバイナリ) を抽出します。 抽出後、XAML ファイルで定義されているすべてのオブジェクトを初期化し、それらを親子のリレーションシップですべて連結し、コードで定義されているイベントハンドラーを XAML ファイルに設定されたイベントにアタッチし、オブジェクトの結果ツリーをのコンテンツとして設定します。改.

## <a name="loading-xaml-at-runtime"></a>実行時の XAML の読み込み

[`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)メソッドは `public`であるため、Xamarin. フォームアプリケーションから呼び出して、実行時に XAML を読み込んで解析することができます。 これにより、アプリケーションが web サービスから XAML をダウンロードし、XAML から必要なビューを作成して、アプリケーションで表示するなどのシナリオが可能になります。

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

この例では、 [`Button`](xref:Xamarin.Forms.Button)インスタンスが作成され、 [`Text`](xref:Xamarin.Forms.Button.Text)プロパティ値が `string`で定義されている XAML から設定されます。 次に、ページの XAML で定義されている[`StackLayout`](xref:Xamarin.Forms.StackLayout)に `Button` が追加されます。

> [!NOTE]
> [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)拡張メソッドを使用すると、ジェネリック型引数を指定できます。 ただし、型引数を指定する必要はほとんどありません。これは、操作対象のインスタンスの型から推論されるためです。

[`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)メソッドを使用すると、次の例のように[`ContentPage`](xref:Xamarin.Forms.ContentPage)を拡大してから、その XAML に移動できます。

```csharp
using Xamarin.Forms.Xaml;
...

// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n</ContentPage>";

ContentPage page = new ContentPage().LoadFromXaml(pageXAML);
await Navigation.PushAsync(page);
```

## <a name="accessing-elements"></a>アクセス (要素に)

[`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)メソッドを使用して実行時に xaml を読み込むと、ランタイムオブジェクト名 (`x:Name`を使用) を指定した xaml 要素への厳密に型指定されたアクセスが許可されません。 ただし、これらの XAML 要素は[`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*)メソッドを使用して取得し、必要に応じてアクセスできます。

```csharp
// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n<StackLayout>\n<Label x:Name=\"monkeyName\"\n />\n</StackLayout>\n</ContentPage>";
ContentPage page = new ContentPage().LoadFromXaml(pageXAML);

Label monkeyLabel = page.FindByName<Label>("monkeyName");
monkeyLabel.Text = "Seated Monkey";
...
```

この例では、 [`ContentPage`](xref:Xamarin.Forms.ContentPage)の XAML が大きくなっています。 この XAML には、 [`Text`](xref:Xamarin.Forms.Label.Text)プロパティが設定される前に、 [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*)メソッドを使用して取得される `monkeyName`という名前の[`Label`](xref:Xamarin.Forms.Label)が含まれています。

## <a name="related-links"></a>関連リンク

- [LoadRuntimeXAML (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)
