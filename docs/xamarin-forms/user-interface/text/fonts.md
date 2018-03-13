---
title: "フォント"
description: "Xamarin.Forms でフォントの設定"
ms.topic: article
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 771e1607bc4e6be8f0991e159b5d34f6d4ea9c02
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="fonts"></a>フォント

この記事は、Xamarin.Forms を使用 (太さやサイズなど) のフォント属性を指定する方法を説明テキストを表示するコントロールのします。 フォント情報は、[コードで指定された](#Setting_Font_in_Code)または[Xaml で指定した](#Setting_Font_in_Xaml)です。
使用することも、[カスタム フォント](#Using_a_Custom_Font)です。

<a name="Setting_Font_in_Code" />

## <a name="setting-font-in-code"></a>コードでフォントの設定

3 つのテキストを表示するコントロールのフォントに関連するプロパティを使用します。

- **FontFamily** &ndash; 、`string`フォントの名前。
- **FontSize** &ndash;としてフォント サイズ、`double`です。
- **FontAttributes** &ndash;などのスタイル情報を指定する文字列*斜体*と**太字**(を使用して、`FontAttributes`列挙型 (C#))。

このコードは、ラベルを作成し、フォント サイズと表示に適用する重みを指定する方法を示します。

```csharp
var about = new Label {
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>フォント サイズ

`FontSize`プロパティのインスタンスは double 値に設定できます。

```csharp
label.FontSize = 24;
```

使用することも、`NamedSize`列挙型を持つ 4 つのビルトイン オプションです。Xamarin.Forms は各プラットフォームの最適なサイズを選択します。

-  **Micro**
-  **小さな**
-  **[中]**
-  **大規模です**


`NamedSize`列挙型になる任意の場所に使用される、`FontSize`を使用して指定できます、`Device.GetNamedSize`する値を変換する方法、 `double`:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

<a name="FontAttributes" />

### <a name="font-attributes"></a>フォントの属性

フォント スタイルなど**太字**と*斜体*に対して設定できる、`FontAttributes`プロパティです。 次の値は現在サポートされています。

-  **None**
-  **太字**
-  **Italic**

`FontAttribute`列挙体は次のように使用することができます (1 つの属性を指定することができますか`OR`それら)。

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="formattedstring"></a>FormattedString

一部の Xamarin.Forms コントロール (など`Label`) を使用して、文字列内で異なるフォント属性をサポートしても、`FormattedString`クラスです。 A `FormattedString` 1 またはそれ以上から成る`Span`s、それぞれが独自の書式設定属性を持つことができます。

`Span`クラスには、次の属性。

* **テキスト**&ndash;表示する値
* **FontFamily** &ndash;フォント名
* **FontSize** &ndash;フォント サイズ
* **FontAttributes** &ndash;などのスタイル情報*斜体*と**太字**
* **ForegroundColor** &ndash;テキストの色
* **BackgroundColor** &ndash;背景色

作成と表示の例、`FormattedString`次に示す&ndash;ラベルに割り当てられることに注意してください`FormattedText`プロパティおよび not、`Text`プロパティです。

```csharp
var labelFormatted = new Label ();
var fs = new FormattedString ();
fs.Spans.Add (new Span { Text="Red, ", ForegroundColor = Color.Red, FontSize = 20, FontAttributes = FontAttributes.Italic });
fs.Spans.Add (new Span { Text=" blue, ", ForegroundColor = Color.Blue, FontSize = 32 });
fs.Spans.Add (new Span { Text=" and green!", ForegroundColor = Color.Green, FontSize = 12 });
labelFormatted.FormattedText = fs;
```


### <a name="setting-font-info-per-platform"></a>プラットフォームごとのフォント情報の設定

または、`Device.RuntimePlatform`このコードに示すように、プラットフォームごとに異なるフォント名を設定するプロパティを使用できます。

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

IOS 用のフォント情報の適切なソースが[iosfonts.com](http://iosfonts.com)です。

<a name="Setting_Font_in_Xaml" />

## <a name="setting-the-font-in-xaml"></a>Xaml でフォントの設定

Xamarin.Forms は制御がすべて表示テキスト、 `Font` Xaml で設定できるプロパティです。 Xaml でフォントを設定する最も簡単な方法は、この例で示すように、名前付きサイズ列挙値を使用するは。

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

組み込みのコンバーターがある、`Font`プロパティを使用して Xaml 内の文字列値として表すことのすべてのフォント設定です。 次の例では、Xaml でフォント属性およびサイズを指定する方法を示します。

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

倍数を指定する`Font`の設定は、1 つのフォント属性の文字列に必要な設定を組み合わせています。 フォント属性文字列の形式でなければなりません`"[font-face],[attributes],[size]"`です。 パラメーターの順序は重要なすべてのパラメーターは省略可能、および複数`attributes`を指定できます、たとえば。

```xaml
<Label Text="Small bold text" FontAttributes="Bold" FontSize="Micro" />
<Label Text="Really big italic text" FontAttributes="Italic" FontSize="72" />
```

`FormattedString`次に示すように、クラスを XAML では、使用もできます。

```xaml
<Label>
    <Label.FormattedText>
        <FormattedString>
            <FormattedString.Spans>
                <Span Text="Red, " ForegroundColor="Red" FontAttributes="Italic" FontSize="20" />
                <Span Text=" blue, " ForegroundColor="Blue" FontSize="32" />
                <Span Text=" and green! " ForegroundColor="Green" FontSize="12"/>
            </FormattedString.Spans>
        </FormattedString>
    </Label.FormattedText>
</Label>
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values) 各プラットフォームで、別のフォントを表示するために XAML にも使用できます。 次の例は、iOS でカスタム フォント フェイスを使用して (<span style="font-family:MarkerFelt-Thin">MarkerFelt シン</span>) し、他のプラットフォームでサイズ/属性のみを指定します。

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP, WinRT, WinPhone" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

使用することをお勧めは常にカスタム フォントを指定する場合`OnPlatform`、すべてのプラットフォームで利用可能なフォントを検索することは困難です。

<a name="Using_a_Custom_Font" />

## <a name="using-a-custom-font"></a>カスタム フォントを使用してください。

組み込みの書体以外のフォントを使用するには、プラットフォーム固有のコーディングのいくつかが必要です。 このスクリーン ショットは、カスタム フォントを示しています。**ロブスター**から[Google のオープン ソース フォント](https://www.google.com/fonts)iOS、Android、および Xamarin.Forms を使用して Windows Phone 上に描画します。

 [![IOS および Android でのカスタム フォント](fonts-images/custom-sml.png "カスタム フォント例")](fonts-images/custom.png#lightbox "カスタム フォントの例")

各プラットフォームに必要な手順は次のとおりです。 アプリケーションにカスタム フォント ファイルを含める場合は、フォントのライセンス許可配布していることを確認することを確認します。

### <a name="ios"></a>iOS

最初にそれが読み込まれている、ことを確認し、Xamarin.Forms を使用して名前で参照することによってカスタム フォントを表示することは`Font`メソッドです。
指示に従って[このブログの投稿](http://blog.xamarin.com/custom-fonts-in-ios/):

1. 使用して、フォント ファイルを追加**ビルド アクション: BundleResource**、および
2. 更新プログラム、 **Info.plist**ファイル (**アプリケーションによって提供されるフォント**、または`UIAppFonts`キー、)、し、
3. 参照している名前で Xamarin.Forms でフォントを定義する場所です。

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Android 用の Xamarin.Forms では、次の名前付け基準を特定して、プロジェクトに追加されたカスタム フォントを参照できます。 最初にフォント ファイルを追加、**資産**アプリケーション プロジェクトとセット内のフォルダー*ビルド アクション: AndroidAsset*です。 完全なパスを使用し、*フォント名*次のコード スニペットに示すように、Xamarin.Forms でフォント名としてハッシュ (#) で区切られました。

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Windows プラットフォーム用の Xamarin.Forms では、次の名前付け基準を特定して、プロジェクトに追加されたカスタム フォントを参照できます。 最初にフォント ファイルを追加、 **/Assets フォント/**アプリケーション プロジェクトとセット内のフォルダー、<span class="UIItem">ビルド アクション: コンテンツ</span>です。 完全パスとフォント ファイル名、後にハッシュ (#) を使用して、<span class="UIItem">フォント名</span>次のコード スニペットで示すように、します。

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> フォント ファイル名とフォント名異なる場合がありますに注意してください。 Windows 上のフォント名を探索するには、.ttf ファイルを右クリックし、選択**プレビュー**です。 フォント名は、プレビュー ウィンドウから確認できます。

これでアプリケーション共通のコードが完成しました。 これでプラットフォーム固有の電話ダイヤル コードが [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) として実装されます。

### <a name="xaml"></a>XAML

使用することも[ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#providing-platform-values)カスタム フォントを表示するために XAML で。

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="Lobster-Regular" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP, WinRT, WinPhone" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

<a name="Summary" />

## <a name="summary"></a>まとめ

Xamarin.Forms を使用する単純な既定の設定を提供するすべてのサポートされているプラットフォームについて簡単にテキストのサイズします。 フォントのフォント フェイスとサイズを指定することもできます&ndash;各プラットフォームにも異なる&ndash;詳細に制御が必要な場合です。 `FormattedString`クラスを使用して別のフォントの仕様を含む文字列を構築するために使用できます、`Span`クラスです。

フォントの詳細については、正しく書式設定されたフォント属性を使用して、Xaml で指定することも、または`FormattedString`を持つ要素`Span`子。


## <a name="related-links"></a>関連リンク

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [テキスト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
