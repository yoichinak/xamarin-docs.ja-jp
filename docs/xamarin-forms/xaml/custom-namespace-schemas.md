---
title: Xamarin 形式の XAML カスタム名前空間スキーマ
description: XAML カスタム名前空間スキーマは、XmlnsDefinitionAttribute クラスを使用して定義できます。これは、カスタム URL と1つ以上の CLR 名前空間の間のマッピングを指定します。 その後、カスタム名前空間スキーマを XAML 名前空間宣言で使用できます。
ms.prod: xamarin
ms.assetid: FDF201A1-8C35-4569-A728-F9B0A0C5B31A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/21/2018
ms.openlocfilehash: d76b5eefcaf0edeb12f128c60e9b8fffff8bcf3b
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644708"
---
# <a name="xaml-custom-namespace-schemas-in-xamarinforms"></a>Xamarin 形式の XAML カスタム名前空間スキーマ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-customnamespaceschemas)

ライブラリ内の型は、XAML でライブラリの XAML 名前空間を宣言することで参照できます。このとき、共通言語ランタイム (CLR) の名前空間名とアセンブリ名を指定する名前空間宣言を使用します。

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:MyCompany.Controls;assembly="MyCompany.Controls">
    ...
</ContentPage>
```

ただし、CLR 名前空間とアセンブリ名を`xmlns`定義に指定すると、厄介でエラーが発生しやすくなります。 また、ライブラリに複数の名前空間の型が含まれている場合は、複数の XAML 名前空間宣言が必要になることがあります。

別の方法として、1つまたは複数の`http://mycompany.com/schemas/controls`CLR 名前空間にマップされるカスタム名前空間スキーマ (など) を定義することもできます。 これにより、単一の XAML 名前空間宣言で、アセンブリ内のすべての型を参照できます。これは、異なる名前空間にある場合でも同様です。 また、単一の XAML 名前空間宣言で、複数のアセンブリ内の型を参照することもできます。

XAML 名前空間の詳細については、「 [Xamarin. Forms の Xaml 名前空間](namespaces.md)」を参照してください。

## <a name="defining-a-custom-namespace-schema"></a>カスタム名前空間スキーマの定義

サンプルアプリケーションには、 `CircleButton`次のような単純なコントロールを公開するライブラリが含まれています。

```csharp
using Xamarin.Forms;

namespace MyCompany.Controls
{
    public class CircleButton : Button
    {
        ...
    }
}
```

ライブラリ内のすべてのコントロールは、 `MyCompany.Controls`名前空間に存在します。 これらのコントロールは、カスタム名前空間スキーマを使用して、呼び出し元アセンブリに公開できます。

カスタム名前空間スキーマは、 `XmlnsDefinitionAttribute`クラスを使用して定義されます。これは、XAML 名前空間と1つ以上の CLR 名前空間の間のマッピングを指定します。 は`XmlnsDefinitionAttribute` 、XAML 名前空間名と CLR 名前空間名の2つの引数を受け取ります。 XAML 名前空間名は`XmlnsDefinitionAttribute.XmlNamespace`プロパティに格納され、CLR 名前空間名は`XmlnsDefinitionAttribute.ClrNamespace`プロパティに格納されます。

> [!NOTE]
> クラスには、という名前`AssemblyName`のプロパティもあります。これはオプションでアセンブリの名前に設定できます。 `XmlnsDefinitionAttribute` これは、 `XmlnsDefinitionAttribute`から参照される CLR 名前空間が外部アセンブリにある場合にのみ必要です。

は`XmlnsDefinitionAttribute` 、カスタム名前空間スキーマでマップされる CLR 名前空間を含むプロジェクトのアセンブリレベルで定義する必要があります。 次の例は、サンプルアプリケーションの**AssemblyInfo.cs**ファイルを示しています。

```csharp
using Xamarin.Forms;
using MyCompany.Controls;

[assembly: Preserve]
[assembly: XmlnsDefinition("http://mycompany.com/schemas/controls", "MyCompany.Controls")]
```

このコードは、 `http://mycompany.com/schemas/controls` URL を`MyCompany.Controls` CLR 名前空間にマップするカスタム名前空間スキーマを作成します。 また、 `Preserve`アセンブリに属性を指定して、リンカーがアセンブリ内のすべての型を確実に保持するようにします。

> [!IMPORTANT]
> 属性`Preserve`は、カスタム名前空間スキーマを通じて割り当てられるか、アセンブリ全体に適用されるアセンブリ内のクラスに適用する必要があります。

その後、カスタム名前空間スキーマを XAML ファイルの型解決に使用できます。

## <a name="consuming-a-custom-namespace-schema"></a>カスタム名前空間スキーマの使用

カスタム名前空間スキーマの型を使用するには、XAML コンパイラで、型を使用するアセンブリから、型を定義するアセンブリへのコード参照が必要です。 これを実現するには、 `Init`メソッドを含むクラスを、XAML で使用される型を定義するアセンブリに追加します。

```csharp
namespace MyCompany.Controls
{
    public static class Controls
    {
        public static void Init()
        {
        }
    }
}
```

この`Init`メソッドは、カスタム名前空間スキーマの型を使用するアセンブリから呼び出すことができます。

```csharp
using Xamarin.Forms;
using MyCompany.Controls;

namespace CustomNamespaceSchemaDemo
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            Controls.Init();
            InitializeComponent();
        }
    }
}
```

> [!WARNING]
> このようなコード参照を含めないと、XAML コンパイラはカスタム名前空間スキーマ型を含むアセンブリを見つけることができなくなります。

`CircleButton`コントロールを使用するには、名前空間の宣言でカスタム名前空間スキーマの URL を指定して、XAML 名前空間を宣言します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:controls="http://mycompany.com/schemas/controls"
             x:Class="CustomNamespaceSchemaDemo.MainPage">
    <StackLayout Margin="20,35,20,20">
        ...
        <controls:CircleButton Text="+"
                               BackgroundColor="Fuchsia"
                               BorderColor="Black"
                               CircleDiameter="100" />
        <controls:CircleButton Text="-"
                               BackgroundColor="Teal"
                               BorderColor="Silver"
                               CircleDiameter="70" />
        ...
    </StackLayout>
</ContentPage>
```

`CircleButton`インスタンスをに追加するには[`ContentPage`](xref:Xamarin.Forms.ContentPage) 、 `controls`名前空間プレフィックスを使用してインスタンスを宣言します。

カスタム名前空間スキーマ型を検索するために、Xamarin. Forms は参照`XmlnsDefinitionAttribute`アセンブリのインスタンスを検索します。 XAML ファイル`xmlns`内の要素の属性がの`XmlNamespace` `XmlnsDefinitionAttribute`プロパティ値と一致する場合、 `XmlnsDefinitionAttribute.ClrNamespace` Xamarin はプロパティ値を使用して型の解決を試みます。 型の解決に失敗した場合、Xamarin は、その他の一致する`XmlnsDefinitionAttribute`インスタンスに基づいて型の解決を試行し続けます。

結果として、 `CircleButton` 2 つのインスタンスが表示されます。

![円ボタン](custom-namespace-schemas-images/circle-buttons.png "円ボタン")

## <a name="related-links"></a>関連リンク

- [カスタム名前空間スキーマ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/xaml-customnamespaceschemas)
- [XAML 名前空間で推奨されるプレフィックス](custom-prefix.md)
- [Xamarin. Forms の XAML 名前空間](namespaces.md)
