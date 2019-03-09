---
title: Xamarin.Forms でのフォント
description: この記事では、Xamarin.Forms を使用して、テキストを表示するコントロールにフォント属性（太さやサイズを含む）を指定する方法について説明します。
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/04/2019
ms.openlocfilehash: 530fcf638454373ae68391e4e11bca85dd2fff63
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669558"
---
# <a name="fonts-in-xamarinforms"></a>Xamarin.Forms でのフォント

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)

この記事では、Xamarin.Forms を使用して、テキストを表示するコントロールにフォント属性（太さやサイズを含む）を指定する方法について説明します。 フォント情報は、[コードでフォントを指定する](#Setting_Font_in_Code)または[XAML でコードを指定する](#Setting_Font_in_Xaml)をご参照ください。 ' を使用することもできます、[カスタム フォント](#Using_a_Custom_Font)、および[フォント アイコンを表示](#display-font-icons)します。

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>コードでフォントを設定します。

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

4つの組み込みオプションを持つ `NamedSize` 列挙体を使用することもできます。 Xamarin.Forms は、各プラットフォームに最適なサイズを選択します。

-  **Micro**
-  **Small**
-  **[Medium]**
-  **Large**

`FontSize`は、`Device.GetNamedSize`メソッドを使用して、`NamedSize`列挙体の値を`double`に変換することで指定できます:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

<a name="FontAttributes" />

### <a name="font-attributes"></a>フォントの属性

**太字**や*斜体*などのフォントスタイルは、`FontAttributes`プロパティで設定できます。 現在サポートされている値は次のとおりです。

-  **None**
-  **Bold**
-  **Italic**

`FontAttribute`列挙体は、次のように使用できます（単一の属性を指定するか、またはそれらを`OR`して一緒にすることができます）。

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>プラットフォームごとのフォント情報のセット

別の方法として、次のコードに示すように、`Device.RuntimePlatform`プロパティを使用して各プラットフォームに異なるフォント名を設定することもできます。

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

iOS 用のフォント情報の良い情報源は[iosfonts.com](http://iosfonts.com)です。

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>XAML でフォントを設定します。

テキストを表示する Xamarin.Forms コントロールはすべて XAML で設定できる`FontSize`プロパティを持ちます。 XAML でフォントを設定する最も簡単な方法は、次の例に示すように、名前付きサイズ列挙値を使用することです。

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

XAML には、すべてのフォント設定を文字列値として表現できるようにする、`FontSize`プロパティの組み込みコンバーターがあります。 さらに、`FontAttributes`フォント属性を指定するプロパティを使用できます。

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values)を XAML で使用して、各プラットフォームで異なるフォントをレンダリングすることもできます。 以下の例では、iOS（<span style="font-family:MarkerFelt-Thin">MarkerFelt-Thin</span>）ではカスタムフォントフェイスを使用し、他のプラットフォームではサイズと属性のみを指定しています。

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

<a name="Using_a_Custom_Font" />

## <a name="use-a-custom-font"></a>カスタム フォントを使用します。

組み込みの書体以外のフォントを使用するには、プラットフォーム固有のコーディングが必要です。 このスクリーンショットは、Xamarin.Forms を使用してレンダリングされた[Google のオープンソースのフォント](https://www.google.com/fonts)からのカスタムフォント**Lobster**を示しています。

 [![iOS と Android でのカスタム フォント](fonts-images/custom-sml.png "カスタム フォントの例")](fonts-images/custom.png#lightbox "カスタム フォントの例")

各プラットフォームに必要な手順は次のとおりです。 アプリケーションにカスタム フォント ファイルを含める場合は、そのフォントのライセンスが配布を許可していることを確認してください。

### <a name="ios"></a>iOS

カスタムフォントが最初に読み込まれ、Xamarin.Forms の`Font`メソッドを使用して名前参照できることを確認してください。
[このブログの投稿](https://blog.xamarin.com/custom-fonts-in-ios/)の指示に従ってください:

1. フォント ファイルを追加**ビルド アクション。BundleResource**と
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

Android 用 Xamarin.Forms は、特定の命名基準に従ってプロジェクトに追加されたカスタムフォントを参照できます。 最初にフォント ファイルを追加、**資産**アプリケーション プロジェクトとセット内のフォルダー*ビルド アクション。AndroidAsset*します。 次に、フルパスとフォント名を Xamarin.Forms の*フォント名*としてハッシュ（＃）で区切って使用します。以下にコードスニペットを示します：

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Windows プラットフォーム用の Xamarin.Forms は、特定の命名標準に従ってプロジェクトに追加されたカスタム フォントを参照できます。 最初にフォント ファイルをアプリケーション プロジェクトの **/Assets/Fonts/** フォルダに追加し、<span class="UIItem">ビルドアクション：Content</span>を設定します。 次に、フルパスとフォントファイル名を使用し、その後にハッシュ（＃）と<span class="UIItem">フォント名</span>を続けます。以下にコードスニペットを示します：

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

XAML で[ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#providing-platform-values)を使用してカスタムフォントをレンダリングすることもできます。

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

## <a name="display-font-icons"></a>フォント アイコンを表示します。

フォント アイコンのデータを指定することで、Xamarin.Forms アプリケーションでフォント アイコンを表示することができます、`FontImageSource`オブジェクト。 このクラスから派生した、 [ `ImageSource` ](xref:Xamarin.Forms.ImageSource)クラスでは、次のプロパティ。

- `Glyph` – として指定された、[フォント] アイコンの unicode 文字値、`string`します。
- `Size` –、`double`デバイス非依存単位、フォントが表示されるアイコンのサイズを示す値。 既定値は、30 です。
- `FontFamily` –、 `string` [フォント] アイコンが所属するフォント ファミリを表します。
- `Color` – 省略可能な[ `Color` ](xref:Xamarin.Forms.Color) [フォント] アイコンを表示するときに使用する値。

このデータを使用して表示できる任意のビューで表示できるよう、PNG、作成、`ImageSource`します。 このアプローチにより、絵文字などのフォントのアイコンなどのビューを表示する 1 つのテキストにフォント アイコンの表示を制限することではなく、複数のビューに表示する、 [ `Label`](xref:Xamarin.Forms.Label)します。

> [!IMPORTANT]
> フォント アイコンは、unicode 文字表現によって現在のみ指定できます。

次の XAML の例がにより表示される 1 つのフォントのアイコン、 [ `Image` ](xref:Xamarin.Forms.Image)ビュー。

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

このコードで Ionicons フォント ファミリから、XBox アイコンが表示されます、 [ `Image` ](xref:Xamarin.Forms.Image)ビュー。 Unicode 文字のこのアイコンは、のことに注目`\uf30c`、XAML でエスケープする必要があり、したがってようになります`&#xf30c;`します。 同等の C# コードに示します。

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

次のスクリーン ショットから、[バインド可能なレイアウト](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindableLayouts/)サンプルは、バインド可能なレイアウトで表示されているいくつかのフォントのアイコンを表示します。

![IOS と Android で、表示されているフォント アイコンのスクリーン ショット](fonts-images/font-image-source.png "イメージのビューに表示されているフォント アイコン")

## <a name="related-links"></a>関連リンク

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [テキスト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
- [バインド可能なレイアウト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindableLayouts/)
- [バインド可能なレイアウト](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
