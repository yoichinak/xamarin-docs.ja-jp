---
title: フォント Xamarin.Forms
description: この記事では、アプリケーションにテキストを表示するコントロールのフォント情報を指定する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/04/2020
ms.custom: contperf-fy21q2
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 146c67655c420611b2a0901efa6cc22ef780713e
ms.sourcegitcommit: c5e72d2ca4152b62ab6583f0dbe84b3ba29d8283
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/18/2020
ms.locfileid: "97677538"
---
# <a name="fonts-in-no-locxamarinforms"></a>フォント Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/workingwithfonts)

既定では、は、 Xamarin.Forms 各プラットフォームで定義されたシステムフォントを使用します。 ただし、テキストを表示するコントロールは、このフォントを変更するために使用できるプロパティを定義します。

- `FontAttributes`型の `FontAttributes` 。これは、、、およびの3つのメンバーを持つ列挙体です `None` `Bold` `Italic` 。 このプロパティの既定値は `None` です。
- `double` 型の `FontSize`。
- `string` 型の `FontFamily`。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトが基になっています。つまり、これらは、データ バインディングの対象にすることができ、スタイルを設定できます。

## <a name="set-font-attributes"></a>フォント属性の設定

テキストを表示するコントロールは、プロパティを設定して `FontAttributes` フォント属性を指定できます。

```xaml
<Label Text="Italics"
       FontAttributes="Italic" />
<Label Text="Bold and italics"
       FontAttributes="Bold, Italic" />
```

同等の C# コードを次に示します。

```csharp
Label label1 = new Label
{
    Text = "Italics",
    FontAttributes = FontAttributes.Italic
};

Label label2 = new Label
{
    Text = "Bold and italics",
    FontAttributes = FontAttributes.Bold | FontAttributes.Italic
};    
```

## <a name="set-the-font-size"></a>フォントサイズを設定する

テキストを表示するコントロールは、プロパティを設定して `FontSize` フォントサイズを指定できます。 プロパティは、 `FontSize` 値に直接設定することも `double` 、列挙値によって設定することもでき [`NamedSize`](xref:Xamarin.Forms.NamedSize) ます。

```xaml
<Label Text="Font size 24"
       FontSize="24" />
<Label Text="Large font size"
       FontSize="Large" />
```

同等の C# コードを次に示します。

```csharp
Label label1 = new Label
{
    Text = "Font size 24",
    FontSize = 24
};

Label label2 = new Label
{
    Text = "Large font size",
    FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label))
};
```

`Device.GetNamedSize`もう1つの方法として、2番目の引数をとして指定するオーバーライドがあり [`Element`](xref:Xamarin.Forms.Element) ます。

```csharp
Label myLabel = new Label
{
    Text = "Large font size",
};
myLabel.FontSize = Device.GetNamedSize(NamedSize.Large, myLabel);
```

> [!NOTE]
> `FontSize`として指定した場合の値は、 `double` デバイスに依存しない単位で測定されます。 詳細については、「 [測定単位](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)」を参照してください。

名前付きフォントサイズの詳細については、「 [名前付きフォントサイズ](#understand-named-font-sizes)について」を参照してください。

## <a name="set-the-font-family"></a>フォントファミリの設定

テキストを表示するコントロールでは、 `FontFamily` プロパティをフォントファミリ名 ("Times Roman" など) に設定できます。 ただし、そのフォントファミリが特定のプラットフォームでサポートされている場合にのみ機能します。

プラットフォームで使用できるフォントを派生させるために使用できる手法がいくつかあります。 ただし、ファイルが存在することは、必ずしもフォントファミリを意味するものではなく、アプリケーションでの使用を意図していない TTFs も含まれています。 また、プラットフォームにインストールされているフォントは、プラットフォームのバージョンで変更される場合があります。 そのため、フォントファミリを指定する最も信頼性の高い方法は、カスタムフォントを使用することです。

カスタムフォントは、共有プロジェクトに追加することができ Xamarin.Forms ます。追加の作業を行わなくても、プラットフォームプロジェクトによって使用されます。 その手順は次のとおりです。

1. Xamarin.Forms埋め込みリソースとして共有プロジェクトにフォントを追加します (**ビルドアクション: EmbeddedResource**)。
1. 属性を使用して、 **AssemblyInfo.cs** などのファイル内のアセンブリにフォントファイルを登録し `ExportFont` ます。 オプションのエイリアスを指定することもできます。

次の例は、アセンブリに登録されている Lobster-Regular フォントとエイリアスを示しています。

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf", Alias = "Lobster")]
```

> [!NOTE]
> フォントは、共有プロジェクト内の任意のフォルダーに置くことができます。フォントをアセンブリに登録するときにフォルダー名を指定する必要はありません。
>
> Windows では、フォントファイル名とフォント名が異なる場合があります。 Windows でフォント名を検出するには、[ファイル] を右クリックし、[ **プレビュー**] を選択します。 その後、[プレビュー] ウィンドウからフォント名を決定できます。

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

同等の C# コードを次に示します。

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
> Windows でのリリースビルドの場合は、カスタムフォントを含むアセンブリがメソッドの呼び出しで引数として渡されることを確認し `Forms.Init` ます。 詳細については、「 [トラブルシューティング](~/xamarin-forms/platform/windows/installation/index.md#troubleshooting)」を参照してください。

## <a name="set-font-properties-per-platform"></a>プラットフォームごとのフォントプロパティの設定

[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)クラスと [`On`](xref:Xamarin.Forms.On) クラスを XAML で使用して、プラットフォームごとにフォントプロパティを設定できます。 次の例では、各プラットフォームで異なるフォントファミリとサイズを設定しています。

```xaml
<Label Text="Different font properties on different platforms"
       FontSize="{OnPlatform iOS=20, Android=Medium, UWP=24}">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
            <On Platform="iOS" Value="MarkerFelt-Thin" />
            <On Platform="Android" Value="Lobster-Regular" />
            <On Platform="UWP" Value="ArimaMadurai-Black" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

プロパティは、 [`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#provide-platform-specific-values) プラットフォームごとにフォントプロパティを設定するためにコードで使用できます。

```csharp
Label label = new Label
{
    Text = "Different font properties on different platforms"
};

label.FontSize = Device.RuntimePlatform == Device.iOS ? 20 :
    Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : 24;
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular" : "ArimaMadurai-Black";
```

プラットフォーム固有の値を指定する方法の詳細については、「 [プラットフォーム固有の値を指定](~/xamarin-forms/platform/device.md#provide-platform-specific-values)する」を参照してください。 マークアップ拡張機能の詳細については `OnPlatform` 、「 [onplatform markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)」を参照してください。

## <a name="understand-named-font-sizes"></a>名前付きフォントサイズを理解する

Xamarin.Forms[`NamedSize`](xref:Xamarin.Forms.NamedSize)特定のフォントサイズを表す列挙体のフィールドを定義します。 次の表に、 `NamedSize` iOS、Android、ユニバーサル Windows プラットフォーム (UWP) のメンバーと既定のサイズを示します。

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

サイズ値は、デバイスに依存しない単位で測定されます。 詳細については、「 [測定単位](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)」を参照してください。

> [!NOTE]
> IOS と Android では、オペレーティングシステムのユーザー補助オプションに基づいて、名前付きフォントサイズが自動スケールされます。 この動作は、プラットフォーム固有の iOS では無効にすることができます。 詳細については、「 [iOS での名前付きフォントサイズのアクセシビリティスケーリング](~/xamarin-forms/platform/ios/named-font-size-scaling.md)」を参照してください。

## <a name="display-font-icons"></a>フォント アイコンの表示

アプリケーションでは、オブジェクトのフォントアイコンデータを指定することによって、フォントアイコンを表示でき Xamarin.Forms `FontImageSource` ます。 クラスから派生するこのクラスに [`ImageSource`](xref:Xamarin.Forms.ImageSource) は、次のプロパティがあります。

- `Glyph` –として指定されたフォントアイコンの unicode 文字値 `string` 。
- `Size` –表示される `double` フォントアイコンのサイズをデバイスに依存しない単位で示す値。 既定値は 30 です。 また、このプロパティは、名前付きフォントサイズに設定できます。
- `FontFamily` - `string` フォントアイコンが属するフォントファミリを表す。
- `Color` – [`Color`](xref:Xamarin.Forms.Color) フォントアイコンを表示するときに使用する省略可能な値です。

このデータは、を表示できる任意のビューで表示できる PNG を作成するために使用され `ImageSource` ます。 この方法では、フォントアイコンの表示を、などの1つのテキスト表示ビューに制限するのではなく、複数のビューによって、emojis などのフォントアイコンを表示でき [`Label`](xref:Xamarin.Forms.Label) ます。

> [!IMPORTANT]
> 現在、フォントアイコンは、unicode 文字表現でのみ指定できます。

次の XAML の例では、ビューによって1つのフォントアイコンが表示されてい [`Image`](xref:Xamarin.Forms.Image) ます。

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

このコードは、Ionicons フォントファミリの XBox アイコンをビューに表示 [`Image`](xref:Xamarin.Forms.Image) します。 このアイコンの unicode 文字はであるのに対し `\uf30c` 、XAML でエスケープする必要があるため、になり `&#xf30c;` ます。 同等の C# コードを次に示します。

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

次のスクリーンショットは、 [バインド](/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts) 可能なレイアウトのサンプルから、バインド可能なレイアウトで表示されているいくつかのフォントアイコンを示しています。

![IOS と Android で表示されているフォントアイコンのスクリーンショット](fonts-images/font-image-source.png "画像ビューに表示されているフォントアイコン")

## <a name="related-links"></a>関連リンク

- [フォントのサンプル](/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [Text (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [バインド可能なレイアウト (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [プラットフォーム固有の値を指定する](~/xamarin-forms/platform/device.md#provide-platform-specific-values)
- [OnPlatform マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)
- [バインド可能なレイアウト](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
