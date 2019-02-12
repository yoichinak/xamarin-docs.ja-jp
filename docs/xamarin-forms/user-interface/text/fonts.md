---
title: Xamarin.Forms でのフォント
description: この記事では、Xamarin.Forms を使用して、テキストを表示するコントロールにフォント属性（太さやサイズを含む）を指定する方法について説明します。
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

この記事では、Xamarin.Forms を使用して、テキストを表示するコントロールにフォント属性（太さやサイズを含む）を指定する方法について説明します。 フォント情報は、[コードでフォントを指定する](#Setting_Font_in_Code)または[XAML でコードを指定する](#Setting_Font_in_Xaml)をご参照ください。
[カスタム フォント](#Using_a_Custom_Font)を使用することもできます。

<a name="Setting_Font_in_Code" />

## <a name="setting-font-in-code"></a>コードでのフォントの設定

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

### <a name="font-size"></a>フォントサイズ

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

**太字**や*斜体*などのフォントスタイルは、`FontAttributes`プロパティで設定できます。現在サポートされている値は次のとおりです。

-  **None**
-  **Bold**
-  **Italic**

`FontAttribute`列挙体は、次のように使用できます（単一の属性を指定するか、またはそれらを`OR`して一緒にすることができます）。

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="setting-font-info-per-platform"></a>プラットフォームごとのフォント情報の設定

別の方法として、次のコードに示すように、`Device.RuntimePlatform`プロパティを使用して各プラットフォームに異なるフォント名を設定することもできます。

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

iOS 用のフォント情報の良い情報源は[iosfonts.com](http://iosfonts.com)です。

<a name="Setting_Font_in_Xaml" />

## <a name="setting-the-font-in-xaml"></a>XAML でのフォントの設定

テキストを表示する Xamarin.Forms コントロールはすべて XAML で設定できる`Font`プロパティを持ちます。 XAML でフォントを設定する最も簡単な方法は、次の例に示すように、名前付きサイズ列挙値を使用することです。

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

XAML には、すべてのフォント設定を文字列値として表現できるようにする、`Font`プロパティの組み込みコンバーターがあります。次の例は、XAML でフォントの属性とサイズを指定する方法を示しています。

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```


複数の`Font`設定を指定するには、必要な設定を単一のフォント属性文字列にまとめます。フォント属性文字列は、 `"[font-face]、[attributes]、[size]"`の形式でなければなりません。パラメータの順序は重要です。すべてのパラメータはオプションであり、複数の属性を指定できます。次に例を示します。

```xaml
<Label Text="Small bold text" Font="Bold, Micro" />
<Label Text="Medium custom font" Font="MarkerFelt-Thin, 42" />
<Label Text="Really big bold and italic text" Font="Bold, Italic, 72"  />
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values)を XAML で使用して、各プラットフォームで異なるフォントをレンダリングすることもできます。以下の例では、iOS（MarkerFelt-Thin）ではカスタムフォントフェイスを使用し、他のプラットフォームではサイズと属性のみを指定しています。

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

## <a name="using-a-custom-font"></a>カスタム フォントを使用します。


組み込みの書体以外のフォントを使用するには、プラットフォーム固有のコーディングが必要です。このスクリーンショットは、Xamarin.Formsを使用してレンダリングされた[Google のオープンソースのフォント](https://www.google.com/fonts)からのカスタムフォント**Lobster**を示しています。

 [![iOS と Android でのカスタム フォント](fonts-images/custom-sml.png "カスタムフォントの例")](fonts-images/custom.png#lightbox "カスタムフォントの例")

各プラットフォームに必要な手順は次のとおりです。アプリケーションにカスタム フォント ファイルを含める場合は、そのフォントのライセンスが配布を許可していることを確認してください。

### <a name="ios"></a>iOS

カスタムフォントが最初に読み込まれ、Xamarin.Forms の`Font`メソッドを使用して名前参照できることを確認してください。
[このブログの投稿](http://blog.xamarin.com/custom-fonts-in-ios/)の指示に従ってください:

1. **ビルドアクション：BundleResource**で、フォント ファイルを追加し
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

Android 用 Xamarin.Forms は、特定の命名基準に従ってプロジェクトに追加されたカスタムフォントを参照できます。最初にフォント ファイルをアプリケーション プロジェクトの**Assetsフォルダ**に追加し、*ビルドアクション：AndroidAsset*を設定します。次に、フルパスとフォント名を Xamarin.Forms のフォント名としてハッシュ（＃）で区切って使用します。以下にコードスニペットを示します：

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Windows


Windows プラットフォーム用の Xamarin.Forms は、特定の命名標準に従ってプロジェクトに追加されたカスタム フォントを参照できます。最初にフォント ファイルをアプリケーション プロジェクトの **/Assets/Fonts/** フォルダに追加し、<span class="UIItem">ビルドアクション：Content</span>を設定します。次に、フルパスとフォントファイル名を使用し、その後にハッシュ（＃）と<span class="UIItem">フォント名</span>を続けます。以下にコードスニペットを示します：

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

<a name="Summary" />

## <a name="summary"></a>まとめ


Xamarin.Forms はサポートされているすべてのプラットフォームでテキストのサイズを簡単に設定できるようにシンプルなデフォルト設定を提供します。さらに細かい制御が必要な場合は、フォントの種類やサイズを（プラットフォームごとに異なる場合も）指定できます。

フォント情報は、正しくフォーマットされたフォント属性を使用して XAML で指定することもできます

## <a name="related-links"></a>関連リンク

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [テキスト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
