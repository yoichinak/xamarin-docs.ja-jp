---
title: Xamarin.Forms での実行時に XAML の読み込み
description: XAML は読み込まれ、LoadFromXaml 拡張メソッドで実行時に解析できます。
ms.prod: xamarin
ms.assetid: 25F73FBF-2DD3-468E-A2D8-0897414F0F4A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/12/2018
ms.openlocfilehash: ce8ba32a1a6a1f69033615558c7ebf15d41e70fe
ms.sourcegitcommit: f890b5ec9b7c2702875070859e1a8cbf6e870e46
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/28/2018
ms.locfileid: "53814100"
---
# <a name="loading-xaml-at-runtime-in-xamarinforms"></a>Xamarin.Forms での実行時に XAML の読み込み

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/XAML/LoadRuntimeXAML/)

[ `Xamarin.Forms.Xaml` ](xref:Xamarin.Forms.Xaml)名前空間は、2 つ[ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)読み込むには、使用でき、実行時に XAML を解析する拡張メソッド。

## <a name="background"></a>背景

Xamarin.Forms XAML クラスが作成されるときに、 [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)メソッドが直接呼び出されません。 これは XAML の分離コード ファイル クラスが呼び出すのために発生、`InitializeComponent`メソッド、コンス トラクターから。

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

Visual Studio では、XAML ファイルを含むプロジェクトをビルド、生成する XAML ファイルを解析、C#コード ファイル (たとえば、 **MainPage.xaml.g.cs**) の定義を格納している、`InitializeComponent`メソッド。

```csharp
private void InitializeComponent()
{
    global::Xamarin.Forms.Xaml.Extensions.LoadFromXaml(this, typeof(MainPage));
    ...
}
```

`InitializeComponent`メソッドの呼び出し、 [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)メソッドを .NET Standard ライブラリから XAML ファイル (または、コンパイルされたバイナリ) を抽出します。 抽出後に、すべての XAML ファイルで定義されているオブジェクトを初期化します、それらすべてをまとめて、親子関係で接続、イベント、XAML ファイルで設定するためのコードで定義されているイベント ハンドラーをアタッチしますおよびのコンテンツとしてオブジェクトの結果のツリーを設定しますページです。

## <a name="loading-xaml-at-runtime"></a>実行時に XAML の読み込み

[ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)メソッドは`public`、およびそのため読み込むには、Xamarin.Forms アプリケーションから呼び出すことができ、実行時に XAML を解析します。 これにより、XAML から必要なビューを作成して、アプリケーションに表示することで、web サービスから XAML をダウンロードするアプリケーションなどのシナリオです。

> [!WARNING]
> 実行時に XAML を読み込むと、コスト、パフォーマンスがかなり、一般に避ける必要があります。

次のコード例では、単純な使用法を示します。

```csharp
using Xamarin.Forms.Xaml;
...

string navigationButtonXAML = "<Button Text=\"Navigate\" />";
Button navigationButton = new Button().LoadFromXaml(navigationButtonXAML);
...
_stackLayout.Children.Add(navigationButton);
```

この例で、 [ `Button` ](xref:Xamarin.Forms.Button)みると、インスタンスが作成されます[ `Text` ](xref:Xamarin.Forms.Button.Text)で、XAML から設定されるプロパティの値が定義されている、`string`します。 `Button`に追加し、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)ページの XAML で定義されています。

> [!NOTE]
> [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)拡張メソッドを指定するジェネリック型引数を許可します。 型引数を指定することはほとんどありません必要になるため、ただしで動作してそのインスタンスの型から推論されます。

[ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)膨張させること次の例で任意の XAML を拡張メソッドを使用できます、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)に移動し。

```csharp
using Xamarin.Forms.Xaml;
...

// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n</ContentPage>";

ContentPage page = new ContentPage().LoadFromXaml(pageXAML);
await Navigation.PushAsync(page);
```

## <a name="accessing-elements"></a>要素にアクセスします。

実行時に XAML を読み込み、 [ `LoadFromXaml` ](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*)メソッドはランタイム オブジェクトの名前を指定した XAML 要素への厳密に型指定されたアクセスを許可しない (を使用して`x:Name`)。 使用してこれらの XAML 要素を取得できますが、 [ `FindByName` ](xref:Xamarin.Forms.NameScopeExtensions.FindByName*)メソッド、必要に応じて、アクセスと。

```csharp
// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n<StackLayout>\n<Label x:Name=\"monkeyName\"\n />\n</StackLayout>\n</ContentPage>";
ContentPage page = new ContentPage().LoadFromXaml(pageXAML);

Label monkeyLabel = page.FindByName<Label>("monkeyName");
monkeyLabel.Text = "Seated Monkey";
...
```

この例では、XAML では、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)膨らみます。 この XAML が含まれています、 [ `Label` ](xref:Xamarin.Forms.Label)という`monkeyName`を使用して取得するが、 [ `FindByName` ](xref:Xamarin.Forms.NameScopeExtensions.FindByName*)メソッドを設定する前に[ `Text` ](xref:Xamarin.Forms.Label.Text)プロパティが設定されます。

## <a name="related-links"></a>関連リンク

- [LoadRuntimeXAML (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/XAML/LoadRuntimeXAML/)
