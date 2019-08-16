---
title: Xamarin.Forms でのフォント
description: この記事では、Xamarin.Forms を使用して、テキストを表示するコントロールにフォント属性（太さやサイズを含む）を指定する方法について説明します。
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/28/2019
ms.openlocfilehash: c18c4e63831a03cbe28accfe10f4c7da31130803
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529309"
---
# <a name="fonts-in-xamarinforms"></a>Xamarin.Forms でのフォント

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

この記事では、Xamarin.Forms を使用して、テキストを表示するコントロールにフォント属性（太さやサイズを含む）を指定する方法について説明します。 フォント情報は、[コードでフォントを指定する](#Setting_Font_in_Code)または[XAML でコードを指定する](#Setting_Font_in_Xaml)をご参照ください。 また、[カスタムフォント](#Using_a_Custom_Font)を使用したり、[フォントアイコンを表示](#display-font-icons)したりすることもできます。

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>コードでのフォントの設定

テキストを表示するコントロールは、3 つのフォント関連プロパティを使用します。

- **FontFamily** &ndash; 、フォント名 `string`型。
- **FontSize** &ndash;フォント サイズ `double`型。
- **FontAttributes** &ndash;*斜体*や**太字**などのスタイル情報を指定する文字列（C＃の`FontAttributes`列挙型を使用）。

このコードは、ラベルを作成して、表示するフォントサイズと太さを指定する方法を示しています。

```csharp
var about = new Label {
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>Font Size

`FontSize`プロパティは、たとえば double 値に設定できます。

```csharp
label.FontSize = 24;
```

また、Xamarin は、 [`NamedSize`](xref:Xamarin.Forms.NamedSize)特定のフォントサイズを表す列挙型のフィールドも定義します。 名前付きフォントサイズの詳細については、「[名前付きフォントサイズ](#named-font-sizes)」を参照してください。

<a name="FontAttributes" />

### <a name="font-attributes"></a>フォント属性

**太字**や*斜体*などのフォントスタイルは、`FontAttributes`プロパティで設定できます。 現在サポートされている値は次のとおりです。

- **None**
- **Bold**
- **Italic**

`FontAttribute`列挙体は、次のように使用できます（単一の属性を指定するか、またはそれらを`OR`して一緒にすることができます）。

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>プラットフォームごとのフォント情報の設定

別の方法として、次のコードに示すように、`Device.RuntimePlatform`プロパティを使用して各プラットフォームに異なるフォント名を設定することもできます。

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

iOS 用のフォント情報の良い情報源は[iosfonts.com](http://iosfonts.com)です。

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>XAML でのフォントの設定

テキストを表示する Xamarin.Forms コントロールはすべて XAML で設定できる`FontSize`プロパティを持ちます。 XAML でフォントを設定する最も簡単な方法は、次の例に示すように、名前付きサイズ列挙値を使用することです。

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

XAML には、すべてのフォント設定を文字列値として表現できるようにする、`FontSize`プロパティの組み込みコンバーターがあります。 また、プロパティを`FontAttributes`使用してフォント属性を指定することもできます。

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

カスタムフォントフェースを指定するときは、すべてのプラットフォームで利用可能なフォントを見つけるのが難しいため、常に`OnPlatform`を使用することをお勧めします。

## <a name="named-font-sizes"></a>名前付きフォントサイズ

Xamarin は、 [`NamedSize`](xref:Xamarin.Forms.NamedSize)特定のフォントサイズを表す列挙型のフィールドを定義します。 次の表に、 `NamedSize` iOS、Android、ユニバーサル Windows プラットフォーム (UWP) のメンバーと既定のサイズを示します。

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

名前付きフォントサイズは、XAML とコードの両方で設定できます。 また、 `Device.GetNamedSize`メソッドを呼び出して、名前付きフォントサイズ`double`を表すを返すこともできます。

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> IOS と Android では、オペレーティングシステムのユーザー補助オプションに基づいて、名前付きフォントサイズが自動スケールされます。 この動作は、プラットフォーム固有の iOS では無効にすることができます。 詳細については、「 [iOS での名前付きフォントサイズのアクセシビリティスケーリング](~/xamarin-forms/platform/ios/named-font-size-scaling.md)」を参照してください。

<a name="Using_a_Custom_Font" />

## <a name="use-a-custom-font"></a>カスタムフォントを使用する

組み込みの書体以外のフォントを使用するには、プラットフォーム固有のコーディングが必要です。 このスクリーンショットは、Xamarin.Forms を使用してレンダリングされた[Google のオープンソースのフォント](https://www.google.com/fonts)からのカスタムフォント**Lobster**を示しています。

 [![iOS と Android でのカスタム フォント](fonts-images/custom-sml.png "カスタム フォントの例")](fonts-images/custom.png#lightbox "カスタム フォントの例")

各プラットフォームに必要な手順は次のとおりです。 アプリケーションにカスタム フォント ファイルを含める場合は、そのフォントのライセンスが配布を許可していることを確認してください。

### <a name="ios"></a>iOS

カスタムフォントが最初に読み込まれ、Xamarin.Forms の`Font`メソッドを使用して名前参照できることを確認してください。
[このブログの投稿](https://blog.xamarin.com/custom-fonts-in-ios/)の指示に従ってください:

1. ビルドアクションを含む**フォントファイルを追加します。BundleResource**、
2. **Info.plist**ファイル（**アプリケーションから提供されたフォント**、または`UIAppFonts`、キー）を更新し、
3. Xamarin.Forms でフォントを定義する場合は必ず名前で参照してください。

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Android 用 Xamarin.Forms は、特定の命名基準に従ってプロジェクトに追加されたカスタムフォントを参照できます。 まず、アプリケーションプロジェクトの**Assets**フォルダーにフォントファイルを追加し、ビルド*アクションを設定します。AndroidAsset*。 次に、フルパスとフォント名を Xamarin.Forms の*フォント名*としてハッシュ（＃）で区切って使用します。以下にコードスニペットを示します：

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
> フォント名とフォント ファイルの名前が異なることに注意してください。 Windows 上のフォント名を検出するには、.ttf ファイルを右クリックして**プレビュー**を選択します。 フォント名は、プレビュー ウィンドウから確認できます。

アプリケーション共通のコードが完成しました。 これでプラットフォーム固有の電話ダイヤルコードが [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) として実装されます。

### <a name="xaml"></a>XAML

XAML で[ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#interact-with-the-ui-from-background-threads)を使用してカスタムフォントをレンダリングすることもできます。

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

フォントアイコンは、 `FontImageSource`オブジェクトのフォントアイコンデータを指定することによって、Xamarin アプリケーションで表示できます。 [`ImageSource`](xref:Xamarin.Forms.ImageSource)クラスから派生するこのクラスには、次のプロパティがあります。

- `Glyph`–として`string`指定されたフォントアイコンの unicode 文字値。
- `Size`–表示されるフォントアイコンのサイズをデバイスに依存しない単位で示す値。`double` 既定値は、30 です。
- `FontFamily`-フォントアイコンが属するフォントファミリを表す。`string`
- `Color`–フォントアイコン[`Color`](xref:Xamarin.Forms.Color)を表示するときに使用する省略可能な値です。

このデータは、を表示`ImageSource`できる任意のビューで表示できる PNG を作成するために使用されます。 この方法では、フォントアイコンの表示を、などの[`Label`](xref:Xamarin.Forms.Label)1 つのテキスト表示ビューに制限するのではなく、複数のビューによって、emojis などのフォントアイコンを表示できます。

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

このコードは、Ionicons フォントファミリの XBox アイコンを[`Image`](xref:Xamarin.Forms.Image)ビューに表示します。 このアイコンの unicode 文字は`\uf30c`であるのに対し、XAML でエスケープする必要があるため、になり`&#xf30c;`ます。 同等のコードをC#で示します。

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
