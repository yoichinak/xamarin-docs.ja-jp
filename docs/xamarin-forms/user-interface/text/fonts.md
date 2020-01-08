---
title: Xamarin.Forms でのフォント
description: この記事では、Xamarin.Forms アプリケーションでテキストを表示するコントロールのフォント情報を指定する方法について説明します。
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/04/2019
ms.openlocfilehash: 62bc5d2012df4880e9257ac70d0fc2e0fca4af64
ms.sourcegitcommit: 619b32f4f96a255140963afcf87629ca690af93d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/27/2019
ms.locfileid: "75500448"
---
# <a name="fonts-in-xamarinforms"></a>Xamarin.Forms でのフォント

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

この記事では、Xamarin.Forms を使用 (重みとサイズを含む) のフォント属性を指定する方法を説明テキストを表示するコントロールにします。 フォント情報は、[コードで指定された](#Setting_Font_in_Code)または[XAML で指定された](#Setting_Font_in_Xaml)します。 また、[カスタムフォント](#Using_a_Custom_Font)を使用したり、[フォントアイコンを表示](#display-font-icons)したりすることもできます。

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>コードでのフォントの設定

3 つのテキストを表示する任意のコントロールのフォント関連プロパティを使用します。

- **FontFamily** &ndash; 、`string`フォント名。
- **FontSize** &ndash;としてのフォント サイズ、`double`します。
- **FontAttributes** &ndash;などのスタイル情報を指定する文字列*斜体*と**太字**(を使用して、`FontAttributes`列挙型 (C#))。

このコードは、ラベルを作成し、重みを表示するフォント サイズを指定する方法を示します。

```csharp
var about = new Label {
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>Font Size

`FontSize`プロパティをたとえば、double 値に設定できます。

```csharp
label.FontSize = 24;
```

サイズ値は、デバイスに依存しない単位で測定されます。 詳細については、「[測定単位](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)」を参照してください。

また、Xamarin. Forms は、特定のフォントサイズを表す[`NamedSize`](xref:Xamarin.Forms.NamedSize)列挙型のフィールドも定義します。 名前付きフォントサイズの詳細については、「[名前付きフォントサイズ](#named-font-sizes)」を参照してください。

<a name="FontAttributes" />

### <a name="font-attributes"></a>フォント属性

フォントのスタイルなど**太字**と*斜体*に設定することができます、`FontAttributes`プロパティ。 次の値は現在サポートされています。

- **None**
- **太字**
- **斜体**

`FontAttribute`列挙体は、次のように使用できます (1 つの属性を指定するまたは`OR`それら)。

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>プラットフォームごとのフォント情報の設定

または、`Device.RuntimePlatform`このコードに示すように、プラットフォームごとに異なるフォント名を設定するプロパティを使用できます。

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

IOS 用のフォント情報の適切なソースが[iosfonts.com](http://iosfonts.com)します。

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>XAML でのフォントの設定

Xamarin.Forms コントロールがすべて表示テキスト、 `FontSize` XAML で設定できるプロパティです。 XAML でフォントを設定する最も簡単な方法は、この例で示すように、名前付きサイズ、列挙値を使用します。

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

組み込みのコンバーターが、`FontSize`プロパティにより、XAML 内の文字列値として表現するすべてのフォント設定です。 また、`FontAttributes` プロパティを使用して、フォント属性を指定することもできます。

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-specific-values)を XAML で使用して、各プラットフォームで異なるフォントをレンダリングすることもできます。 次の例では、iOS でカスタムフォントフェイス (MarkerFelt) を使用して、他のプラットフォームのサイズと属性のみを指定しています。

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

使用することをお勧めは常にカスタム フォントを指定するときに`OnPlatform`、すべてのプラットフォームで利用可能なフォントを検索することは困難です。

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

組み込みのタイプフェイス以外のフォントを使用してでは、いくつかのプラットフォームに固有のコーディングが必要です。 このスクリーン ショットは、カスタム フォント**Lobster**から[Google のオープン ソースのフォント](https://www.google.com/fonts)Xamarin.Forms を使用して表示します。

 [![IOS と Android でのカスタムフォント](fonts-images/custom-sml.png "カスタムフォントの例")](fonts-images/custom.png#lightbox "カスタムフォントの例")

各プラットフォームに必要な手順は次のとおりです。 アプリケーションにカスタム フォント ファイルを含む、ときに必ずフォントのライセンスは、配布することを確認してください。

### <a name="ios"></a>iOS

最初に読み込まれるをことを確認し、Xamarin.Forms を使用して名前で参照することによってカスタム フォントを表示することは`Font`メソッド。
指示に従って[このブログの投稿](https://devblogs.microsoft.com/xamarin/custom-fonts-in-ios/):

1. フォント ファイルを追加**ビルド アクション: BundleResource**と
2. 更新プログラム、 **Info.plist**ファイル (**アプリケーションによって提供されるフォント**、または`UIAppFonts`キー、)、し、
3. Xamarin.Forms でフォントを定義する場所に名前を参照します。

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Android 用 Xamarin.Forms は、特定の命名基準に従ってプロジェクトに追加されたカスタムフォントを参照できます。 最初にフォント ファイルをアプリケーション プロジェクトの**Assetsフォルダ**に追加し、*ビルドアクション：AndroidAsset*を設定します。 次に、フルパスとフォント名を Xamarin.Forms の*フォント名*としてハッシュ（＃）で区切って使用します。以下にコードスニペットを示します：

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Windows プラットフォーム用の Xamarin.Forms は、特定の命名標準に従ってプロジェクトに追加されたカスタム フォントを参照できます。 最初にフォント ファイルをアプリケーション プロジェクトの **/Assets/Fonts/** フォルダに追加し、**ビルドアクション：Content**を設定します。 次に、フルパスとフォントファイル名を使用し、その後にハッシュ（＃）と**フォント名**を続けます。以下にコードスニペットを示します：

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> フォント名とフォント ファイルの名前が異なるあることに注意してください。 Windows 上のフォント名を検出するには、.ttf ファイルを右クリックして**プレビュー**します。 フォント名は、プレビュー ウィンドウから確認できます。

### <a name="xaml"></a>XAML

使用することも[ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#interact-with-the-ui-from-background-threads)カスタム フォントを表示するために XAML で。

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="Lobster-Regular" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

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

このコードにより、Ionicons フォントファミリの XBox アイコンが[`Image`](xref:Xamarin.Forms.Image)ビューに表示されます。 このアイコンの unicode 文字は `\uf30c`ますが、XAML でエスケープする必要があるため、`&#xf30c;`になります。 該当の C# コードを次に示します。

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

- [FontsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [テキスト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [バインド可能なレイアウト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [バインド可能なレイアウト](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
