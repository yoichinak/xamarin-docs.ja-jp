---
title: Xamarin.Forms のフォント
description: この資料では、Xamarin.Forms アプリケーションでテキストを表示するコントロールのフォント情報を指定する方法について説明します。
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2020
ms.openlocfilehash: ca4d3b242fcc73bb73e8d6ab1f817eefcc2ade4d
ms.sourcegitcommit: 89b3e383a37db5b940f0c63bbfe9cb806dc7d5d1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/15/2020
ms.locfileid: "81388801"
---
# <a name="fonts-in-xamarinforms"></a>Xamarin.Forms のフォント

[![サンプルの](~/media/shared/download.png)ダウンロード サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

この資料では、Xamarin.Forms でテキストを表示するコントロールのフォント属性 (太さやサイズを含む) を指定する方法について説明します。 フォント情報は[、コードで指定](#Setting_Font_in_Code)するか[、XAML で指定](#Setting_Font_in_Xaml)できます。 [カスタム フォント](#use-a-custom-font)を使用して、フォント アイコン を[表示](#display-font-icons)することもできます。

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>コードでフォントを設定する

テキストを表示するコントロールの 3 つのフォント関連プロパティを使用します。

- **フォントファミリ**&ndash;フォント`string`名。
- **フォントサイズ**&ndash;を`double`.
- **フォント属性**&ndash;(C# の列挙体を使用して)*斜体*や**太字**などのスタイル情報を指定する`FontAttributes`文字列です。

次のコードは、ラベルを作成し、表示するフォント サイズと太さを指定する方法を示しています。

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

プロパティ`FontSize`は、次の例に示す倍精度浮動小数点値に設定できます。

```csharp
label.FontSize = 24;
```

サイズ値は、デバイスに依存しない単位で測定されます。 詳細については、「[計測単位](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)」を参照してください。

Xamarin.Forms では、特定の[`NamedSize`](xref:Xamarin.Forms.NamedSize)フォント サイズを表す列挙体のフィールドも定義します。 名前付きフォント サイズの詳細については、「[名前付きフォント サイズ](#named-font-sizes)」を参照してください。

<a name="FontAttributes" />

### <a name="font-attributes"></a>フォント属性

太字や*斜体*などのフォント スタイルは、プロパティで設定できます。 **bold** `FontAttributes` 現在サポートされている値は次のとおりです。

- **なし**
- **太字**
- **斜体**

列挙`FontAttribute`体は、次のように使用できます (単一の属性を指定`OR`することも、一緒に指定することもできます)。

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>プラットフォームごとのフォント情報の設定

または、この`Device.RuntimePlatform`コードに示すように、このプロパティを使用して、プラットフォームごとに異なるフォント名を設定できます。

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

iOS のフォント情報の良いソースは[iosfonts.com](http://iosfonts.com)です。

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>XAML でフォントを設定する

テキストを表示する Xamarin.Forms コントロール`FontSize`には、XAML で設定できるプロパティがあります。 XAML でフォントを設定する最も簡単な方法は、次の例に示すように、名前付きサイズの列挙値を使用することです。

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

XAML ですべてのフォント設定を`FontSize`文字列値として表現できるプロパティ用の組み込みのコンバーターがあります。 さらに、このプロパティ`FontAttributes`を使用してフォント属性を指定できます。

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

この[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-specific-values)プロパティは、XAML で使用して、プラットフォームごとに異なるフォントをレンダリングすることもできます。 次の例では、プラットフォームごとに異なるフォントを使用しています。

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

Xamarin.Forms は、特定[`NamedSize`](xref:Xamarin.Forms.NamedSize)のフォント サイズを表す列挙体のフィールドを定義します。 次の表に`NamedSize`、iOS、Android、およびユニバーサル Windows プラットフォーム (UWP) のメンバーと既定のサイズを示します。

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

サイズ値は、デバイスに依存しない単位で測定されます。 詳細については、「[計測単位](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement)」を参照してください。

名前付きフォント サイズは、XAML とコードの両方を使用して設定できます。 さらに、このメソッド`Device.GetNamedSize`を呼び出して、名前`double`付きフォント サイズを表す を返すことができます。

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> iOS と Android では、名前付きフォント サイズは、オペレーティング システムのユーザー補助オプションに基づいて自動的にスケーリングされます。 この動作は、プラットフォーム固有の iOS で無効にすることができます。 詳細については、「 [iOS での名前付きフォント サイズのアクセシビリティスケーリング](~/xamarin-forms/platform/ios/named-font-size-scaling.md)」を参照してください。

## <a name="use-a-custom-font"></a>カスタム フォントを使用する

カスタム フォントは、Xamarin.Forms 共有プロジェクトに追加でき、追加の作業をしなくてもプラットフォーム プロジェクトで使用できます。 その手順は次のとおりです。

1. フォントを Xamarin.Forms 共有プロジェクトに埋め込みリソースとして追加します (**ビルド アクション: EmbeddedResource**)。
1. 属性を使用して、フォント ファイル AssemblyInfo.csをアセンブリに登録します。 **AssemblyInfo.cs** `ExportFont` オプションの別名も指定できます。

> [!IMPORTANT]
> 埋め込みフォントでは、Xamarin.Forms 4.5.0.530 以上を使用する必要があります。

次の例は、アセンブリに登録されているロブスター標準フォントとエイリアスを示しています。

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf", Alias = "Lobster")]
```

> [!NOTE]
> フォントは、アセンブリにフォントを登録するときにフォルダー名を指定しなくても、共有プロジェクト内の任意のフォルダーに配置できます。

その後、ファイル拡張子を付けずに、名前を参照することで、各プラットフォームでフォントを使用できます。

```xaml
<!-- Use font name -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster-Regular" />
```

または、エイリアスを参照することで、各プラットフォームで使用できます。

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

次のスクリーンショットは、カスタム フォントを示しています。

[![iOS とアンドロイドのカスタム フォント](fonts-images/custom-sml.png "カスタム フォントの例")](fonts-images/custom.png#lightbox "カスタム フォントの例")

> [!IMPORTANT]
> Windows では、フォント ファイル名とフォント名が異なる場合があります。 Windows でフォント名を検出するには、.ttf ファイルを右クリックし、[**プレビュー**] を選択します。 フォント名はプレビューウィンドウから決定できます。

## <a name="display-font-icons"></a>フォント アイコンの表示

フォント アイコンは`FontImageSource`、Xamarin.Forms アプリケーションでオブジェクトのフォント アイコン データを指定して表示できます。 このクラスは、クラスから派生し[`ImageSource`](xref:Xamarin.Forms.ImageSource)、次のプロパティを持ちます。

- `Glyph`– フォントアイコンのユニコード文字値で、 として指定`string`されます。
- `Size`–`double`レンダリングされたフォント アイコンのサイズをデバイスに依存しない単位で示す値。 既定値は 30 です。 また、このプロパティは、名前付きフォント サイズに設定できます。
- `FontFamily`–`string`フォントアイコンが属するフォントファミリを表す。
- `Color`– フォント[`Color`](xref:Xamarin.Forms.Color)アイコンを表示するときに使用するオプションの値。

このデータは、PNG を作成するために使用され`ImageSource`、. この方法では、絵文字などのフォントアイコンを複数のビューで表示できます[`Label`](xref:Xamarin.Forms.Label)。

> [!IMPORTANT]
> フォントアイコンは、現在 Unicode 文字表現によってのみ指定できます。

次の XAML の例では、ビューで表示されている[`Image`](xref:Xamarin.Forms.Image)1 つのフォント アイコンがあります。

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

このコードでは、Ionicons フォント ファミリの XBox アイコンが[`Image`](xref:Xamarin.Forms.Image)ビューに表示されます。 このアイコンの Unicode 文字は`\uf30c`、XAML でエスケープする必要があるので、 . `&#xf30c;` 該当の C# コードを次に示します。

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

次のスクリーンショットは、[バインド可能なレイアウト](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)サンプルから、バインド可能なレイアウトによって表示されている複数のフォント アイコンを示しています。

![iOSとAndroidで表示されているフォントアイコンのスクリーンショット](fonts-images/font-image-source.png "イメージ ビューに表示されるフォント アイコン")

## <a name="related-links"></a>関連リンク

- [フォントサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [テキスト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [バインド可能なレイアウト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [バインド可能なレイアウト](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
