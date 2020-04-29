---
title: Xamarin 形式のフォント
description: この記事では、Xamarin. フォームアプリケーションでテキストを表示するコントロールのフォント情報を指定する方法について説明します。
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2020
ms.openlocfilehash: 160cbbfa99114d74fa5fdaa5f92b8397ea7d3367
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82516476"
---
# <a name="fonts-in-xamarinforms"></a>Xamarin 形式のフォント

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

この記事では、テキストを表示するコントロールのフォント属性 (ウエイトやサイズなど) を指定する方法について説明します。 フォント情報は、[コードで指定](#Setting_Font_in_Code)することも、 [XAML で指定](#Setting_Font_in_Xaml)することもできます。 また、[カスタムフォント](#use-a-custom-font)を使用したり、[フォントアイコンを表示](#display-font-icons)したりすることもできます。

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>コードでのフォントの設定

テキストを表示するコントロールのフォントに関連する3つのプロパティを使用します。

- フォント&ndash;名`string` **を指定します**。
- フォントサイズ**をと**して`double`フォントを変更します。 &ndash;
- **Fontattributes** &ndash; *斜体*や**太字**などのスタイル情報を指定する文字列。 `FontAttributes` C# では、列挙体を使用します。

このコードは、ラベルを作成し、表示するフォントサイズとウエイトを指定する方法を示しています。

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

プロパティ`FontSize`は、次のように double 値に設定できます。

```csharp
label.FontSize = 24;
```

サイズ値は、デバイスに依存しない単位で測定されます。 詳細については、「[測定単位](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)」を参照してください。

また、Xamarin は、 [`NamedSize`](xref:Xamarin.Forms.NamedSize)特定のフォントサイズを表す列挙型のフィールドも定義します。 名前付きフォントサイズの詳細については、「[名前付きフォントサイズ](#named-font-sizes)」を参照してください。

<a name="FontAttributes" />

### <a name="font-attributes"></a>フォント属性

**太字**や*斜体*などのフォントスタイルは、 `FontAttributes`プロパティで設定できます。 現在、次の値がサポートされています。

- **なし**
- **縞**
- **<**

列挙`FontAttribute`体は、次のように使用できます (1 つの`OR`属性を指定することも、一緒に指定することもできます)。

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>プラットフォームごとのフォント情報の設定

また、次`Device.RuntimePlatform`のコードに示すように、プロパティを使用して各プラットフォームで異なるフォント名を設定することもできます。

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

IOS のフォント情報の適切なソースは[iosfonts.com](http://iosfonts.com)です。

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>XAML でのフォントの設定

テキストを表示する Xamarin. フォームコントロールには`FontSize` 、XAML で設定できるプロパティがあります。 XAML でフォントを設定する最も簡単な方法は、次の例に示すように、名前付きサイズの列挙値を使用することです。

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

XAML では、すべてのフォント設定を`FontSize`文字列値として表すことができる、プロパティの組み込みのコンバーターが用意されています。 また、プロパティを`FontAttributes`使用してフォント属性を指定することもできます。

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

プロパティ[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#provide-platform-specific-values)は、XAML で使用して、各プラットフォームで別のフォントを表示することもできます。 次の例では、各プラットフォームで異なるフォントを使用します。

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

## <a name="named-font-sizes"></a>名前付きフォント サイズ

Xamarin は、 [`NamedSize`](xref:Xamarin.Forms.NamedSize)特定のフォントサイズを表す列挙型のフィールドを定義します。 次の表に、 `NamedSize` IOS、Android、ユニバーサル WINDOWS プラットフォーム (UWP) のメンバーと既定のサイズを示します。

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

名前付きフォントサイズは、XAML とコードの両方で設定できます。 また、 `Device.GetNamedSize`メソッドを呼び出して、名前付きフォントサイズ`double`を表すを返すこともできます。

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> IOS と Android では、オペレーティングシステムのユーザー補助オプションに基づいて、名前付きフォントサイズが自動スケールされます。 この動作は、プラットフォーム固有の iOS では無効にすることができます。 詳細については、「 [iOS での名前付きフォントサイズのアクセシビリティスケーリング](~/xamarin-forms/platform/ios/named-font-size-scaling.md)」を参照してください。

## <a name="use-a-custom-font"></a>カスタムフォントを使用する

カスタムフォントは、Xamarin. フォーム共有プロジェクトに追加できます。また、追加の作業を行わずに、プラットフォームプロジェクトによって使用されます。 その手順は次のとおりです。

1. 埋め込みリソースとして Xamarin. Forms 共有プロジェクトにフォントを追加します (**ビルドアクション: EmbeddedResource**)。
1. 属性を使用して、AssemblyInfo.cs などのファイル内のアセンブリにフォントファイルを登録します。 **AssemblyInfo.cs** `ExportFont` オプションのエイリアスを指定することもできます。

> [!IMPORTANT]
> 埋め込みフォントを使用するには、4.5.0.530 以上を使用する必要があります。

次の例は、エイリアスと共に、アセンブリに登録されている標準フォント Lobster を示しています。

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf", Alias = "Lobster")]
```

> [!NOTE]
> フォントは、共有プロジェクト内の任意のフォルダーに置くことができます。フォントをアセンブリに登録するときにフォルダー名を指定する必要はありません。

このフォントは、ファイル拡張子のない名前を参照することで、各プラットフォームで使用できます。

```xaml
<!-- Use font name -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster-Regular" />
```

または、そのエイリアスを参照することによって、各プラットフォームで使用することもできます。

```xaml
<!-- Use font alias -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster" />
```

該当の C# コードを次に示します。

```csharp
// Use font name
Label label1 = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster-Regular"
};

// Use font alias
Label label2 = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster"
};
```

次のスクリーンショットはカスタムフォントを示しています。

[![IOS と Android でのカスタムフォント](fonts-images/custom-sml.png "カスタムフォントの例")](fonts-images/custom.png#lightbox "カスタムフォントの例")

> [!IMPORTANT]
> Windows では、フォントファイル名とフォント名が異なる場合があります。 Windows でフォント名を検出するには、[ファイル] を右クリックし、[**プレビュー**] を選択します。 その後、[プレビュー] ウィンドウからフォント名を決定できます。

## <a name="display-font-icons"></a>フォント アイコンの表示

フォントアイコンは、 `FontImageSource`オブジェクトのフォントアイコンデータを指定することによって、Xamarin アプリケーションで表示できます。 [`ImageSource`](xref:Xamarin.Forms.ImageSource)クラスから派生するこのクラスには、次のプロパティがあります。

- `Glyph`–として指定されたフォントアイコンの unicode 文字`string`値。
- `Size`–表示`double`されるフォントアイコンのサイズをデバイスに依存しない単位で示す値。 既定値は 30 です。 また、このプロパティは、名前付きフォントサイズに設定できます。
- `FontFamily`-フォント`string`アイコンが属するフォントファミリを表す。
- `Color`–フォントアイコン[`Color`](xref:Xamarin.Forms.Color)を表示するときに使用する省略可能な値です。

このデータは、を表示できる任意のビューで表示できる PNG を作成するために使用`ImageSource`されます。 この方法では、フォントアイコンの表示を、などの[`Label`](xref:Xamarin.Forms.Label)1 つのテキスト表示ビューに制限するのではなく、複数のビューによって、emojis などのフォントアイコンを表示できます。

> [!IMPORTANT]
> 現在、フォントアイコンは、unicode 文字表現でのみ指定できます。

次の XAML の例では、 [`Image`](xref:Xamarin.Forms.Image)ビューによって1つのフォントアイコンが表示されています。

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

このコードは、Ionicons フォントファミリの XBox アイコンを[`Image`](xref:Xamarin.Forms.Image)ビューに表示します。 このアイコンの unicode 文字は`\uf30c`であるのに対し、XAML でエスケープする必要があるため、 `&#xf30c;`になります。 該当の C# コードを次に示します。

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
