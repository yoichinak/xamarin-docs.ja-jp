---
title: Xamarin.Forms の XAML Namespace カスタム スキーマ
description: XAML 名前空間のカスタム スキーマは、カスタムの URL と 1 つまたは複数の CLR 名前空間の間のマッピングを指定する XmlnsDefinitionAttribute クラスを使用して定義できます。 カスタムの名前空間のスキーマは、XAML 名前空間の宣言で使用できます。
ms.prod: xamarin
ms.assetid: FDF201A1-8C35-4569-A728-F9B0A0C5B31A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/21/2018
ms.openlocfilehash: 8167ff00d3e4d7167772f6f5a578da6197c0d72d
ms.sourcegitcommit: 93c9fe61eb2cdfa530960b4253eb85161894c882
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/07/2019
ms.locfileid: "55832234"
---
# <a name="xaml-custom-namespace-schemas-in-xamarinforms"></a>Xamarin.Forms の XAML Namespace カスタム スキーマ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/XAML/CustomNamespaceSchemas/)

ライブラリの型は、共通言語ランタイム (CLR) 名前空間の名前とアセンブリ名を指定する名前空間宣言で、ライブラリの XAML 名前空間を宣言することで、XAML で参照できます。

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:MyCompany.Controls;assembly="MyCompany.Controls">
    ...
</ContentPage>
```

ただしでの CLR 名前空間とアセンブリ名を指定する、`xmlns`定義する厄介なこと、エラーが発生します。 さらに、複数の XAML 名前空間宣言を必要なライブラリには、複数の名前空間の型が含まれている場合があります。

など、カスタムの名前空間スキーマを定義するその他の方法は、 `http://mycompany.com/schemas/controls`、1 つまたは複数の CLR 名前空間にマップされます。 これにより、異なる名前空間内にある場合でも、アセンブリ内のすべての型を参照する 1 つの XAML 名前空間宣言。 また、複数のアセンブリで参照型を 1 つの XAML 名前空間宣言こともできます。

XAML 名前空間の詳細については、次を参照してください。 [Xamarin.Forms の XAML 名前空間](namespaces.md)します。

## <a name="defining-a-custom-namespace-schema"></a>カスタムの名前空間のスキーマを定義します。

サンプル アプリケーションにはなど、いくつかの単純なコントロールを公開するライブラリが含まれています`CircleButton`:

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

ライブラリ内のすべてのコントロールが存在する、`MyCompany.Controls`名前空間。 これらのコントロールは、カスタムの名前空間のスキーマを呼び出し元のアセンブリに公開できます。

カスタムの名前空間のスキーマが定義されている、`XmlnsDefinitionAttribute`クラスは、XAML 名前空間と 1 つまたは複数の CLR 名前空間のマッピングを指定します。 `XmlnsDefinitionAttribute`は 2 つの引数を受け取ります。 XAML 名前空間の名前と CLR 名前空間の名前。 XAML 名前空間の名前が格納されている、`XmlnsDefinitionAttribute.XmlNamespace`にプロパティ、および CLR 名前空間の名前が格納されている、`XmlnsDefinitionAttribute.ClrNamespace`プロパティ。

> [!NOTE]
> `XmlnsDefinitionAttribute`クラスは、という名前のプロパティもあります。 `AssemblyName`、アセンブリの名前を必要に応じて設定します。 これは CLR 名前空間から参照されているときに必要な`XmlnsDefinitionAttribute`は、外部のアセンブリ。

`XmlnsDefinitionAttribute`カスタム名前空間のスキーマにマップされる CLR 名前空間を含むプロジェクトでアセンブリ レベルで定義する必要があります。 次の例は、 **AssemblyInfo.cs**サンプル アプリケーションからのファイル。

```csharp
using Xamarin.Forms;
using MyCompany.Controls;

[assembly: Preserve]
[assembly: XmlnsDefinition("http://mycompany.com/schemas/controls", "MyCompany.Controls")]
```

このコードをマップするカスタムの名前空間のスキーマの作成、`http://mycompany.com/schemas/controls`への URL、 `MyCompany.Controls` CLR 名前空間。 さらに、`Preserve`リンカーがアセンブリ内のすべての型を保存することを確認する、アセンブリで属性を指定します。

> [!IMPORTANT]
> `Preserve`を通じてカスタムの名前空間のスキーマ、マップまたはアセンブリ全体に適用されているアセンブリのクラスに属性を適用する必要があります。

カスタムの名前空間のスキーマは、XAML ファイル内の型解決を使用できます。

## <a name="consuming-a-custom-namespace-schema"></a>カスタムの名前空間のスキーマの使用

カスタムの名前空間のスキーマの型を使用するには、こと、型を使用するアセンブリからの参照をコードがある、アセンブリ、型を定義する XAML コンパイラが必要です。 これを含むクラスを追加することで実現できます、`Init`メソッドを XAML で使用される型を定義するアセンブリ。

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

`Init`メソッドは、カスタムの名前空間のスキーマの型を使用するアセンブリから呼び出す、できます。

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
> カスタムの名前空間のスキーマ型を含むアセンブリを検索することができません、XAML コンパイラ エラー コードの参照を含めることが発生します。

使用する、 `CircleButton` XAML 名前空間が宣言は、コントロール、カスタムの名前空間のスキーマの URL を指定する名前空間宣言で。

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

`CircleButton` インスタンスに追加できる、 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage)でそれらを宣言することで、`controls`名前空間プレフィックス。

カスタムの名前空間スキーマ型を検索する Xamarin.Forms は、参照先アセンブリを検索`XmlnsDefinitionAttribute`インスタンス。 場合、 `xmlns` XAML ファイル内の要素の属性と一致する、`XmlNamespace`でプロパティの値を`XmlnsDefinitionAttribute`、Xamarin.Forms が使用するとき、`XmlnsDefinitionAttribute.ClrNamespace`プロパティ値の型の解決。 その他の一致に基づく型解決を試行する Xamarin.Forms が引き続き型解決が失敗した場合、`XmlnsDefinitionAttribute`インスタンス。

結果は、その 2 つ`CircleButton`インスタンスが表示されます。

![ボタンを circle](custom-namespace-schemas-images/circle-buttons.png "Circle ボタン")

## <a name="related-links"></a>関連リンク

- [カスタムの Namespace スキーマ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/XAML/CustomNamespaceSchemas/)
- [Xamarin.Forms の XAML 名前空間](namespaces.md)
