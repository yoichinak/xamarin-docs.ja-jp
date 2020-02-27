---
title: Xamarin.Forms でのフォント
description: この記事では、Xamarin.Forms アプリケーションでテキストを表示するコントロールのフォント情報を指定する方法について説明します。
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/20/2020
ms.openlocfilehash: 75cf5acdb862a15d722075269b2f736264be680f
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77636169"
---
# <a name="fonts-in-xamarinforms"></a>Xamarin.Forms でのフォント

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

この記事では、Xamarin.Forms を使用 (重みとサイズを含む) のフォント属性を指定する方法を説明テキストを表示するコントロールにします。 フォント情報は、[コードで指定](#Setting_Font_in_Code)することも、 [XAML で指定](#Setting_Font_in_Xaml)することもできます。 また、[カスタムフォント](#Using_a_Custom_Font)を使用したり、[フォントアイコンを表示](#display-font-icons)したりすることもできます。

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>コードでのフォントの設定

3 つのテキストを表示する任意のコントロールのフォント関連プロパティを使用します。

- **FontFamily** `string` フォント名 &ndash; ます。
- **FontSize** &ndash; `double`としてフォントサイズを変更します。
- **Fontattributes** &ndash;*斜体*や**太字**などのスタイル情報を指定する文字列です (でC#`FontAttributes` 列挙を使用します)。

このコードは、ラベルを作成し、重みを表示するフォント サイズを指定する方法を示します。

```csharp
var about = new Label
{
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>[フォント サイズ]

`FontSize` プロパティは、次のように double 値に設定できます。

```csharp
label.FontSize = 24;
```

サイズ値は、デバイスに依存しない単位で測定されます。 詳細については、「[測定単位](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)」を参照してください。

また、Xamarin. Forms は、特定のフォントサイズを表す[`NamedSize`](xref:Xamarin.Forms.NamedSize)列挙型のフィールドも定義します。 名前付きフォントサイズの詳細については、「[名前付きフォントサイズ](#named-font-sizes)」を参照してください。

<a name="FontAttributes" />

### <a name="font-attributes"></a>フォント属性

**太字**や*斜体*などのフォントスタイルは、`FontAttributes` プロパティで設定できます。 次の値は現在サポートされています。

- **なし**
- **太字**
- **<**

`FontAttribute` 列挙体は、次のように使用できます (1 つの属性を指定することも、複数の属性を `OR` することもできます)。

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>プラットフォームごとのフォント情報の設定

また、次のコードに示すように、`Device.RuntimePlatform` プロパティを使用して各プラットフォームで異なるフォント名を設定することもできます。

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

IOS のフォント情報の適切なソースは[iosfonts.com](http://iosfonts.com)です。

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>XAML でのフォントの設定

テキストを表示する Xamarin. Forms コントロールには、XAML で設定できる `FontSize` のプロパティがあります。 XAML でフォントを設定する最も簡単な方法は、この例で示すように、名前付きサイズ、列挙値を使用します。

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

XAML では、すべてのフォント設定を文字列値として表すことができる、`FontSize` プロパティ用の組み込みのコンバーターが用意されています。 また、`FontAttributes` プロパティを使用して、フォント属性を指定することもできます。

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

XAML では、 [`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-specific-values)プロパティを使用して、各プラットフォームで別のフォントを表示することもできます。 次の例では、各プラットフォームで異なるフォントを使用します。

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

## <a name="named-font-sizes"></a>名前付きフォントサイズ

Xamarin は、特定のフォントサイズを表す[`NamedSize`](xref:Xamarin.Forms.NamedSize)列挙型のフィールドを定義します。 次の表は、`NamedSize` のメンバーと、iOS、Android、ユニバーサル Windows プラットフォーム (UWP) の既定のサイズを示しています。

| メンバー | iOS | Android | UWP |
| --- | --- | --- | --- |
| `Default` | 16 | 14 | 14 |
| `Micro` | 11 | 10 | 15.667 |
| `Small` | 13 | 14 | 18.667 |
| `Medium` | 16 | 17 | 22.667 |
| `Large` | 20 | 22 | 32 |
| `Body` | 17 | 16 | 14 |
| `Header` | 17 | 96 | 46 |
| `Title` | 28 | 24 | 24 |
| `Subtitle` | 22 | 16 | 20 |
| `Caption` | 12 | 12 | 12 |

サイズ値は、デバイスに依存しない単位で測定されます。 詳細については、「[測定単位](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)」を参照してください。

名前付きフォントサイズは、XAML とコードの両方で設定できます。 また、`Device.GetNamedSize` メソッドを呼び出して、名前付きフォントサイズを表す `double` を返すこともできます。

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> IOS と Android では、オペレーティングシステムのユーザー補助オプションに基づいて、名前付きフォントサイズが自動スケールされます。 この動作は、プラットフォーム固有の iOS では無効にすることができます。 詳細については、「 [iOS での名前付きフォントサイズのアクセシビリティスケーリング](~/xamarin-forms/platform/ios/named-font-size-scaling.md)」を参照してください。

<a name="Using_a_Custom_Font" />

## <a name="use-a-custom-font"></a>カスタムフォントを使用する

カスタムフォントは、Xamarin. フォーム共有プロジェクトに追加できます。また、追加の作業を行わずに、プラットフォームプロジェクトによって使用されます。 その手順は次のとおりです。

1. 埋め込みリソースとして Xamarin. Forms 共有プロジェクトにフォントを追加します (**ビルドアクション: EmbeddedResource**)。
1. `ExportFont` 属性を使用して、 **AssemblyInfo.cs**などのファイル内のアセンブリにフォントファイルを登録します。

次の例は、アセンブリに登録されている Lobster 標準フォントを示しています。

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf")]
```

> [!NOTE]
> フォントは、共有プロジェクト内の任意のフォルダーに置くことができます。フォントをアセンブリに登録するときにフォルダー名を指定する必要はありません。

このフォントは、ファイル拡張子のない名前を参照することで、各プラットフォームで使用できます。

```xaml
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster-Regular" />
```

同等の C# コードを次に示します。

```csharp
Label label = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster-Regular"
};
```

次のスクリーンショットはカスタムフォントを示しています。

[![IOS と Android でのカスタムフォント](fonts-images/custom-sml.png "カスタムフォントの例")](fonts-images/custom.png#lightbox "カスタムフォントの例")

## <a name="display-font-icons"></a>フォントアイコンの表示

`FontImageSource` オブジェクトでフォントアイコンデータを指定することによって、Xamarin アプリケーションでフォントアイコンを表示できます。 [`ImageSource`](xref:Xamarin.Forms.ImageSource)クラスから派生するこのクラスには、次のプロパティがあります。

- `Glyph` – `string`として指定されたフォントアイコンの unicode 文字値。
- `Size` –表示されるフォントアイコンのサイズをデバイスに依存しない単位で示す `double` 値です。 既定値は、30 です。 また、このプロパティは、名前付きフォントサイズに設定できます。
- `FontFamily` –フォントアイコンが属するフォントファミリを表す `string` です。
- `Color` –フォントアイコンを表示するときに使用する省略可能な[`Color`](xref:Xamarin.Forms.Color)値です。

このデータは、`ImageSource`を表示できる任意のビューで表示できる PNG を作成するために使用されます。 この方法では、フォントアイコンの表示を、 [`Label`](xref:Xamarin.Forms.Label)などの1つのテキスト表示ビューに制限するのではなく、複数のビューで、emojis などのフォントアイコンを表示できます。

> [!IMPORTANT]
> 現在、フォントアイコンは、unicode 文字表現でのみ指定できます。

次の XAML の例には、 [`Image`](xref:Xamarin.Forms.Image)ビューによって表示される1つのフォントアイコンがあります。

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

このコードにより、Ionicons フォントファミリの XBox アイコンが[`Image`](xref:Xamarin.Forms.Image)ビューに表示されます。 このアイコンの unicode 文字は `\uf30c`ますが、XAML でエスケープする必要があるため、`&#xf30c;`になります。 同等の C# コードを次に示します。

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

次のスクリーンショットは、[バインド](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)可能なレイアウトのサンプルから、バインド可能なレイアウトで表示されているいくつかのフォントアイコンを示しています。

![IOS と Android で表示されているフォントアイコンのスクリーンショット](fonts-images/font-image-source.png "画像ビューに表示されているフォントアイコン")

## <a name="related-links"></a>関連リンク

- [フォントのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [Text (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [バインド可能なレイアウト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [バインド可能なレイアウト](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
