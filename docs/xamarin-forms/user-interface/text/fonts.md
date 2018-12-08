---
title: Xamarin.Forms でのフォント
description: この記事では、Xamarin.Forms アプリケーションでテキストを表示するコントロールのフォント情報を指定する方法について説明します。
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 1b90a3184b89ba9147525a87b52e048bbb59f5af
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53061153"
---
# <a name="fonts-in-xamarinforms"></a>Xamarin.Forms でのフォント

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)

この記事では、Xamarin.Forms を使用 (重みとサイズを含む) のフォント属性を指定する方法を説明テキストを表示するコントロールにします。 フォント情報は、[コードで指定された](#Setting_Font_in_Code)または[XAML で指定された](#Setting_Font_in_Xaml)します。
使用することも、[カスタム フォント](#Using_a_Custom_Font)します。

<a name="Setting_Font_in_Code" />

## <a name="setting-font-in-code"></a>コードでのフォントの設定

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

### <a name="font-size"></a>フォント サイズ

`FontSize`プロパティをたとえば、double 値に設定できます。

```csharp
label.FontSize = 24;
```

使用することも、`NamedSize`を 4 つの組み込みオプション; を持つ列挙型Xamarin.Forms は、各プラットフォーム用に最適なサイズを選択します。

-  **Micro**
-  **小さな**
-  **[中]**
-  **大規模です**

`NamedSize`列挙型になる任意の場所に使用される、`FontSize`を使用して指定できます、`Device.GetNamedSize`値に変換するメソッド、 `double`:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

<a name="FontAttributes" />

### <a name="font-attributes"></a>フォントの属性

フォントのスタイルなど**太字**と*斜体*に設定することができます、`FontAttributes`プロパティ。 次の値は現在サポートされています。

-  **None**
-  **太字**
-  **斜体**

`FontAttribute`列挙体は、次のように使用できます (1 つの属性を指定するまたは`OR`それら)。

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="setting-font-info-per-platform"></a>プラットフォームごとのフォント情報の設定

または、`Device.RuntimePlatform`このコードに示すように、プラットフォームごとに異なるフォント名を設定するプロパティを使用できます。

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

IOS 用のフォント情報の適切なソースが[iosfonts.com](http://iosfonts.com)します。

<a name="Setting_Font_in_Xaml" />

## <a name="setting-the-font-in-xaml"></a>XAML でのフォントの設定

Xamarin.Forms コントロールがすべて表示テキスト、 `Font` XAML で設定できるプロパティです。 XAML でフォントを設定する最も簡単な方法は、この例で示すように、名前付きサイズ、列挙値を使用します。

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

組み込みのコンバーターが、`Font`プロパティにより、XAML 内の文字列値として表現するすべてのフォント設定です。 次の例では、XAML の属性のフォントとサイズを指定する方法を表示します。

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

複数の指定に`Font`の設定は、1 つに、必要な設定を組み合わせて`Font`属性の文字列。 フォントの属性の文字列として書式設定する必要があります`"[font-face],[attributes],[size]"`します。 パラメーターの順序は重要なすべてのパラメーターは省略可能、および複数`attributes`指定できますが、たとえば。

```xaml
<Label Text="Small bold text" Font="Bold, Micro" />
<Label Text="Medium custom font" Font="MarkerFelt-Thin, 42" />
<Label Text="Really big bold and italic text" Font="Bold, Italic, 72"  />
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values) 各プラットフォームで別のフォントを表示するために XAML にも使用できます。 次の例は、カスタムのフォント フェイスを使用して iOS で (<span style="font-family:MarkerFelt-Thin">MarkerFelt シン</span>) し、その他のプラットフォームでのみサイズと属性を指定します。

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

<a name="Using_a_Custom_Font" />

## <a name="using-a-custom-font"></a>カスタム フォントを使用します。

組み込みのタイプフェイス以外のフォントを使用してでは、いくつかのプラットフォームに固有のコーディングが必要です。 このスクリーン ショットは、カスタム フォント**Lobster**から[Google のオープン ソースのフォント](https://www.google.com/fonts)Xamarin.Forms を使用して表示します。

 [![IOS と Android でのカスタム フォント](fonts-images/custom-sml.png "カスタム フォントの例")](fonts-images/custom.png#lightbox "カスタム フォントの例")

各プラットフォームに必要な手順は次のとおりです。 アプリケーションにカスタム フォント ファイルを含む、ときに必ずフォントのライセンスは、配布することを確認してください。

### <a name="ios"></a>iOS

最初に読み込まれるをことを確認し、Xamarin.Forms を使用して名前で参照することによってカスタム フォントを表示することは`Font`メソッド。
指示に従って[このブログの投稿](http://blog.xamarin.com/custom-fonts-in-ios/):

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

Android 用の Xamarin.Forms には、特定の名前付け標準に従って、プロジェクトに追加されているカスタム フォントを参照できます。 最初にフォント ファイルを追加、**資産**アプリケーション プロジェクトとセット内のフォルダー*ビルド アクション: AndroidAsset*します。 完全パスを使用し、*フォント名*次のコード スニペットに示すように、Xamarin.Forms でのフォント名としてハッシュ (#) で区切られました。

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows

Windows プラットフォーム用の Xamarin.Forms には、特定の名前付け標準に従って、プロジェクトに追加されているカスタム フォントを参照できます。 最初にフォント ファイルを追加、 **/Assets/フォント/** アプリケーション プロジェクトとセット内のフォルダー、<span class="UIItem">ビルド アクション: コンテンツ</span>します。 完全なパスとフォント ファイル名を使用後にハッシュ (#)、<span class="UIItem">フォント名</span>、次のコード スニペットを示します。

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> フォント名とフォント ファイルの名前が異なるあることに注意してください。 Windows 上のフォント名を検出するには、.ttf ファイルを右クリックして**プレビュー**します。 フォント名は、プレビュー ウィンドウから確認できます。

これでアプリケーション共通のコードが完成しました。 これでプラットフォーム固有の電話ダイヤル コードが [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) として実装されます。

### <a name="xaml"></a>XAML

使用することも[ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#providing-platform-values)カスタム フォントを表示するために XAML で。

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

<a name="Summary" />

## <a name="summary"></a>まとめ

Xamarin.Forms を使用する単純な既定の設定を提供するすべてのサポートされているプラットフォームについて簡単にテキストのサイズします。 フォント フェイスとサイズを指定することもできます&ndash;各プラットフォームにも異なる&ndash;の細かい制御が必要な場合。

フォント情報は、正しく書式設定されたフォント属性を使用して XAML でも指定することができます。

## <a name="related-links"></a>関連リンク

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [テキスト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
