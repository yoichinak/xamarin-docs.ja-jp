---
title: の XAML カスタム名前空間スキーマ Xamarin.Forms
description: XAML カスタム名前空間スキーマは、XmlnsDefinitionAttribute クラスを使用して定義できます。これは、カスタム URL と1つ以上の CLR 名前空間の間のマッピングを指定します。 その後、カスタム名前空間スキーマを XAML 名前空間宣言で使用できます。
ms.prod: xamarin
ms.assetid: FDF201A1-8C35-4569-A728-F9B0A0C5B31A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/21/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f950db0694b21239b742867d519e893d9a62384c
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374070"
---
# <a name="xaml-custom-namespace-schemas-in-xamarinforms"></a>の XAML カスタム名前空間スキーマ Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/xaml-customnamespaceschemas)

ライブラリ内の型は、XAML でライブラリの XAML 名前空間を宣言することで参照できます。このとき、共通言語ランタイム (CLR) の名前空間名とアセンブリ名を指定する名前空間宣言を使用します。

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:MyCompany.Controls;assembly=MyCompany.Controls">
    ...
</ContentPage>
```

ただし、CLR 名前空間とアセンブリ名を定義に指定する `xmlns` と、厄介でエラーが発生しやすくなります。 また、ライブラリに複数の名前空間の型が含まれている場合は、複数の XAML 名前空間宣言が必要になることがあります。

別の方法として、 `http://mycompany.com/schemas/controls` 1 つまたは複数の CLR 名前空間にマップされるカスタム名前空間スキーマ (など) を定義することもできます。 これにより、単一の XAML 名前空間宣言で、アセンブリ内のすべての型を参照できます。これは、異なる名前空間にある場合でも同様です。 また、単一の XAML 名前空間宣言で、複数のアセンブリ内の型を参照することもできます。

XAML 名前空間の詳細については、「 [」の Xamarin.Forms 「Xaml 名前空間](namespaces.md)」を参照してください。

## <a name="defining-a-custom-namespace-schema"></a>カスタム名前空間スキーマの定義

サンプルアプリケーションには、次のような単純なコントロールを公開するライブラリが含まれてい `CircleButton` ます。

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

ライブラリ内のすべてのコントロールは、名前空間に存在し `MyCompany.Controls` ます。 これらのコントロールは、カスタム名前空間スキーマを使用して、呼び出し元アセンブリに公開できます。

カスタム名前空間スキーマは、クラスを使用して定義され `XmlnsDefinitionAttribute` ます。これは、XAML 名前空間と1つ以上の CLR 名前空間の間のマッピングを指定します。 は、 `XmlnsDefinitionAttribute` XAML 名前空間名と CLR 名前空間名の2つの引数を受け取ります。 XAML 名前空間名はプロパティに格納され、 `XmlnsDefinitionAttribute.XmlNamespace` CLR 名前空間名はプロパティに格納され `XmlnsDefinitionAttribute.ClrNamespace` ます。

> [!NOTE]
> クラスには `XmlnsDefinitionAttribute` 、という名前のプロパティもあります `AssemblyName` 。これはオプションでアセンブリの名前に設定できます。 これは、から参照される CLR 名前空間 `XmlnsDefinitionAttribute` が外部アセンブリにある場合にのみ必要です。

は、 `XmlnsDefinitionAttribute` カスタム名前空間スキーマでマップされる CLR 名前空間を含むプロジェクトのアセンブリレベルで定義する必要があります。 次の例は、サンプルアプリケーションの **AssemblyInfo.cs** ファイルを示しています。

```csharp
using Xamarin.Forms;
using MyCompany.Controls;

[assembly: Preserve]
[assembly: XmlnsDefinition("http://mycompany.com/schemas/controls", "MyCompany.Controls")]
```

このコードは、 `http://mycompany.com/schemas/controls` URL を CLR 名前空間にマップするカスタム名前空間スキーマを作成し `MyCompany.Controls` ます。 また、アセンブリに `Preserve` 属性を指定して、リンカーがアセンブリ内のすべての型を確実に保持するようにします。

> [!IMPORTANT]
> 属性は、 `Preserve` カスタム名前空間スキーマを通じて割り当てられるか、アセンブリ全体に適用されるアセンブリ内のクラスに適用する必要があります。

その後、カスタム名前空間スキーマを XAML ファイルの型解決に使用できます。

## <a name="consuming-a-custom-namespace-schema"></a>カスタム名前空間スキーマの使用

カスタム名前空間スキーマの型を使用するには、XAML コンパイラで、型を使用するアセンブリから、型を定義するアセンブリへのコード参照が必要です。 これを実現するには、メソッドを含むクラスを、 `Init` XAML で使用される型を定義するアセンブリに追加します。

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

この `Init` メソッドは、カスタム名前空間スキーマの型を使用するアセンブリから呼び出すことができます。

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

コントロールを使用するには、名前空間の `CircleButton` 宣言でカスタム名前空間スキーマの URL を指定して、XAML 名前空間を宣言します。

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

`CircleButton` インスタンスをに追加するには、 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 名前空間プレフィックスを使用してインスタンスを宣言し `controls` ます。

では、カスタム名前空間スキーマ型を検索するために、 Xamarin.Forms 参照されたアセンブリのインスタンスを検索 `XmlnsDefinitionAttribute` します。 `xmlns`XAML ファイル内の要素の属性がのプロパティ値と一致する場合 `XmlNamespace` `XmlnsDefinitionAttribute` 、 Xamarin.Forms は `XmlnsDefinitionAttribute.ClrNamespace` 型の解決のためにプロパティ値を使用しようとします。 型の解決が失敗した場合、 Xamarin.Forms は、その他の一致するインスタンスに基づいて、型の解決を引き続き試行し `XmlnsDefinitionAttribute` ます。

結果として、2つのインスタンスが表示され `CircleButton` ます。

![円ボタン](custom-namespace-schemas-images/circle-buttons.png "円ボタン")

## <a name="related-links"></a>関連リンク

- [カスタム名前空間スキーマ (サンプル)](/samples/xamarin/xamarin-forms-samples/xaml-customnamespaceschemas)
- [XAML 名前空間で推奨されるプレフィックス](custom-prefix.md)
- [での XAML 名前空間 Xamarin.Forms](namespaces.md)