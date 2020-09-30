---
title: Xamarin.Forms デバイスクラス
description: この記事では Xamarin.Forms 、デバイスクラスを使用して、プラットフォームごとに機能とレイアウトをきめ細かく制御する方法について説明します。
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8eff115e894f77aeacff0f6c072bfd338fa19844
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91560508"
---
# <a name="no-locxamarinforms-device-class"></a>Xamarin.Forms デバイスクラス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithdevice)

クラスには、 [`Device`](xref:Xamarin.Forms.Device) 開発者がプラットフォームごとにレイアウトと機能をカスタマイズするのに役立つさまざまなプロパティとメソッドが含まれています。

クラスには、特定のハードウェアの種類とサイズでコードをターゲットにするためのメソッドとプロパティに加え `Device` て、バックグラウンドスレッドから UI コントロールを操作するために使用できるメソッドが用意されています。 詳細については、「 [バックグラウンドスレッドから UI を操作する](#interact-with-the-ui-from-background-threads)」を参照してください。

## <a name="provide-platform-specific-values"></a>プラットフォーム固有の値を指定する

2.3.4 より前では、 Xamarin.Forms アプリケーションが実行されていたプラットフォームは、プロパティを調べて、、、の各列挙値と比較することによって取得でき [`Device.OS`](xref:Xamarin.Forms.Device.OS) [`TargetPlatform.iOS`](xref:Xamarin.Forms.TargetPlatform.iOS) [`TargetPlatform.Android`](xref:Xamarin.Forms.TargetPlatform.Android) [`TargetPlatform.WinPhone`](xref:Xamarin.Forms.TargetPlatform.WinPhone) [`TargetPlatform.Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) ます。 同様に、オーバーロードの1つを使用して、 [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) プラットフォーム固有の値をコントロールに提供できます。

ただし、 Xamarin.Forms これらの api の2.3.4 は非推奨とされ、置き換えられました。 クラスには、、、 [`Device`](xref:Xamarin.Forms.Device) [`Device.iOS`](xref:Xamarin.Forms.Device.iOS) [`Device.Android`](xref:Xamarin.Forms.Device.Android) `Device.WinPhone` (非推奨)、 `Device.WinRT` (非推奨) [`Device.UWP`](xref:Xamarin.Forms.Device.UWP) [`Device.macOS`](xref:Xamarin.Forms.Device.macOS) 、、およびの各プラットフォームを識別するパブリック文字列定数が含まれるようになりました。 同様に、  [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) オーバーロードは api と api に置き換えられてい [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) [`On`](xref:Xamarin.Forms.On) ます。

C# では、プロパティにステートメントを作成し、 `switch` [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) `case` 必要なプラットフォームのステートメントを指定することによって、プラットフォーム固有の値を指定できます。

```csharp
double top;
switch (Device.RuntimePlatform)
{
  case Device.iOS:
    top = 20;
    break;
  case Device.Android:
  case Device.UWP:
  default:
    top = 0;
    break;
}
layout.Margin = new Thickness(5, top, 5, 0);
```

[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)クラスと [`On`](xref:Xamarin.Forms.On) クラスは、XAML で同じ機能を提供します。

```xaml
<StackLayout>
  <StackLayout.Margin>
    <OnPlatform x:TypeArguments="Thickness">
      <On Platform="iOS" Value="0,20,0,0" />
      <On Platform="Android, UWP" Value="0,0,0,0" />
    </OnPlatform>
  </StackLayout.Margin>
  ...
</StackLayout>
```

[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)クラスは、 `x:TypeArguments` 対象の型に一致する属性を使用してインスタンス化する必要があるジェネリッククラスです。 クラスでは、 [`On`](xref:Xamarin.Forms.On) [`Platform`](xref:Xamarin.Forms.On.Platform) 属性は単一の `string` 値または複数のコンマ区切り値を受け取ることができ `string` ます。

> [!IMPORTANT]
> `Platform`クラスに正しくない属性値を指定 `On` しても、エラーは発生しません。 代わりに、プラットフォーム固有の値が適用されずにコードが実行されます。

また、 `OnPlatform` XAML でマークアップ拡張機能を使用して、プラットフォームごとに UI の外観をカスタマイズすることもできます。 詳細については、「 [Onplatform Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)」を参照してください。

## <a name="deviceidiom"></a>デバイス. 表現形式

プロパティは、 `Device.Idiom` アプリケーションが実行されているデバイスに応じてレイアウトまたは機能を変更するために使用できます。 列挙には [`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom) 、次の値が含まれます。

- **電話** – IPhone、iPod touch、および Android デバイスが 600 dip ^ よりも狭い
- **タブレット** – IPad、Windows デバイス、および Android デバイスが 600 dip ^ よりも大きい
- **デスクトップ** – windows 10 デスクトップコンピューター上の [UWP アプリ](~/xamarin-forms/platform/windows/installation/index.md) でのみ返されます ( `Phone` 連続性のあるシナリオの場合を含め、モバイル Windows デバイスでを返します)。
- **Tv** – Tizen tv デバイス
- **Watch** – Tizen watch デバイス
- **サポート** されていない–未使用

*^ dip は必ずしも物理ピクセル数ではありません*

プロパティは、次のように、 `Idiom` 大きな画面を利用するレイアウトを作成する場合に特に便利です。

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

クラスは、 [`OnIdiom`](xref:Xamarin.Forms.OnIdiom`1) XAML と同じ機能を提供します。

```xaml
<StackLayout>
    <StackLayout.Margin>
        <OnIdiom x:TypeArguments="Thickness">
            <OnIdiom.Phone>0,20,0,0</OnIdiom.Phone>
            <OnIdiom.Tablet>0,40,0,0</OnIdiom.Tablet>
            <OnIdiom.Desktop>0,60,0,0</OnIdiom.Desktop>
        </OnIdiom>
    </StackLayout.Margin>
    ...
</StackLayout>
```

[`OnIdiom`](xref:Xamarin.Forms.OnPlatform`1)クラスは、 `x:TypeArguments` 対象の型に一致する属性を使用してインスタンス化する必要があるジェネリッククラスです。

また、 `OnIdiom` XAML でマークアップ拡張機能を使用して、アプリケーションが実行されているデバイスの表現方法に基づいて UI の外観をカスタマイズすることもできます。 詳細については、「 [Onidiom のマークアップ拡張](~/xamarin-forms/xaml/markup-extensions/consuming.md#onidiom-markup-extension)」を参照してください。

## <a name="deviceflowdirection"></a>System.windows.flowdirection>

値は、 [`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) [`FlowDirection`](xref:Xamarin.Forms.FlowDirection) デバイスによって使用されている現在のフロー方向を表す列挙値を取得します。 フロー方向とは、ページ上の UI 要素を視覚でスキャンしていく方向のことです。 列挙値は、次のとおりです。

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

XAML では、 [`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) マークアップ拡張機能を使用して値を取得でき `x:Static` ます。

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

C# の同等のコードは次のとおりです。

```csharp
this.FlowDirection = Device.FlowDirection;
```

フローの方向の詳細については、「 [右から左へのローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)」を参照してください。

## <a name="devicestyles"></a>デバイス. スタイル

[ `Styles` プロパティ](~/xamarin-forms/user-interface/styles/index.md)には、いくつかのコントロールのプロパティ (など) に適用できる組み込みのスタイル定義が含まれてい `Label` `Style` ます。 使用できるスタイルは次のとおりです。

- BodyStyle
- CaptionStyle
- ListItemDetailTextStyle
- ListItemTextStyle
- Subタイトルのスタイル
- タイトルのスタイル

## <a name="devicegetnamedsize"></a>デバイス. GetNamedSize

`GetNamedSize` C# コードでを設定するときに使用でき [`FontSize`](~/xamarin-forms/user-interface/text/fonts.md) ます。

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

## <a name="devicegetnamedcolor"></a>Device.GetNamedColor

Xamarin.Forms 4.6 では、名前付きの色のサポートが導入されています。 名前付きの色とは、デバイス上でアクティブになっているシステムモード (たとえば、淡色や濃色) に応じて異なる値を持つ色です。 Android では、名前付きの色には、 [R. Color](https://developer.android.com/reference/android/R.color#constants_2) クラスを使用してアクセスします。 IOS では、名前付きの色は [システムカラー](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/color/#system-colors)と呼ばれます。 ユニバーサル Windows プラットフォームでは、名前付きの色は [XAML テーマリソース](/windows/uwp/design/controls-and-patterns/xaml-theme-resources)と呼ばれます。

メソッドを使用して、 `GetNamedColor` Android、iOS、UWP の名前付きの色を取得できます。 メソッドは引数を受け取り、 `string` を返し [`Color`](xref:Xamarin.Forms.Color) ます。

```csharp
// Retrieve an Android named color
Color color = Device.GetNamedColor(NamedPlatformColor.HoloBlueBright);
```

`Color.Default` は、色の名前が見つからない場合、または `GetNamedColor` サポートされていないプラットフォームでが呼び出された場合に返されます。

> [!NOTE]
> `GetNamedColor`このメソッドは、 `Color` プラットフォームに固有のを返すため、通常はプロパティと組み合わせて使用する必要があり [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) ます。

クラスには、 `NamedPlatformColor` Android、iOS、UWP の名前付きの色を定義する定数が含まれています。

| Android | iOS | macOS | UWP |
| --- | --- | --- | --- |
| `BackgroundDark` | `Label` | `AlternateSelectedControlTextColor` | `SystemAltHighColor` |
| `BackgroundLight` | `Link` | `ControlAccent` | `SystemAltLowColor` |
| `Black` | `OpaqueSeparator` | `ControlBackgroundColor` | `SystemAltMediumColor` |
| `DarkerGray` | `PlaceholderText` | `ControlColor` | `SystemAltMediumHighColor` |
| `HoloBlueBright` | `QuaternaryLabel` | `DisabledControlTextColor` | `SystemAltMediumLowColor` |
| `HoloBlueDark` | `SecondaryLabel` | `FindHighlightColor` | `SystemBaseHighColor` |
| `HoloBlueLight` | `Separator` | `GridColor` | `SystemBaseLowColor` |
| `HoloGreenDark` | `SystemBlue` | `HeaderTextColor` | `SystemBaseMediumColor` |
| `HoloGreenLight` | `SystemGray` | `HighlightColor` | `SystemBaseMediumHighColor` |
| `HoloOrangeDark` | `SystemGray2` | `KeyboardFocusIndicatorColor` | `SystemBaseMediumLowColor` |
| `HoloOrangeLight` | `SystemGray3` | `Label` | `SystemChromeAltLowColor` |
| `HoloPurple` | `SystemGray4` | `LabelColor` | `SystemChromeBlackHighColor` |
| `HoloRedDark` | `SystemGray5` | `Link` | `SystemChromeBlackLowColor` |
| `HoloRedLight` | `SystemGray6` | `LinkColor` | `SystemChromeBlackMediumColor` |
| `TabIndicatorText` | `SystemGreen` | `PlaceholderText` | `SystemChromeBlackMediumLowColor` |
| `Transparent` | `SystemIndigo` | `PlaceholderTextColor` | `SystemChromeDisabledHighColor` |
| `White` | `SystemOrange` | `QuaternaryLabel`| `SystemChromeDisabledLowColor` |
| `WidgetEditTextDark` | `SystemPink` | `QuaternaryLabelColor` | `SystemChromeHighColor` |
| | `SystemPurple` | `SecondaryLabel` | `SystemChromeLowColor` |
| | `SystemRed` | `SecondaryLabelColor` | `SystemChromeMediumColor` |
| | `SystemTeal` | `SelectedContentBackgroundColor` | `SystemChromeMediumLowColor` |
| | `SystemYellow` | `SelectedControlColor` | `SystemChromeWhiteColor` |
| | `TertiaryLabel` | `SelectedControlTextColor` | `SystemListLowColor` |
| | | `SelectedMenuItemTextColor` | `SystemListMediumColor`|
| | | `SelectedTextBackgroundColor` | |
| | | `SelectedTextColor` | |
| | | `Separator` | |
| | | `SeparatorColor` | |
| | | `ShadowColor` | |
| | | `SystemBlue` | |
| | | `SystemGray` | |
| | | `SystemGreen` | |
| | | `SystemIndigo` | |
| | | `SystemOrange` | |
| | | `SystemPink` | |
| | | `SystemPurple` | |
| | | `SystemRed` | |
| | | `SystemTeal` | |
| | | `SystemYellow` | |
| | | `TertiaryLabel` | |
| | | `TertiaryLabelColor` | |
| | | `TextBackgroundColor` | |
| | | `TextColor` | |
| | | `UnderPageBackgroundColor` | |
| | | `UnemphasizedSelectedContentBackgroundColor` | |
| | | `UnemphasizedSelectedTextBackgroundColor` | |
| | | `UnemphasizedSelectedTextColor` | |
| | | `WindowBackgroundColor` | |
| | | `WindowFrameTextColor` | |

## <a name="devicestarttimer"></a>デバイス. StartTimer

`Device`クラスには、 `StartTimer` Xamarin.Forms .NET Standard ライブラリなど、共通コードで動作する時間に依存するタスクをトリガーするための簡単な方法を提供するメソッドもあります。 を渡して `TimeSpan` 間隔を設定し、を返して `true` タイマーを実行したままにするか、 `false` 現在の呼び出しの後に停止します。

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () =>
{
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

タイマー内のコードがユーザーインターフェイスと対話する場合 (のテキストの設定 `Label` やアラートの表示など)、式の内部で実行する必要があり `BeginInvokeOnMainThread` ます (以下を参照)。

> [!NOTE]
> `System.Timers.Timer`クラスと `System.Threading.Timer` クラスは、メソッドを使用するための代替手段として .NET Standard `Device.StartTimer` ます。

## <a name="interact-with-the-ui-from-background-threads"></a>バックグラウンドスレッドから UI を操作する

IOS、Android、およびユニバーサル Windows プラットフォームを含むほとんどのオペレーティングシステムは、ユーザーインターフェイスを含むコードにシングルスレッドモデルを使用します。 このスレッドは、多くの場合、 *メインスレッド* または *UI スレッド*と呼ばれます。 このモデルの結果として、ユーザーインターフェイス要素にアクセスするすべてのコードは、アプリケーションのメインスレッドで実行する必要があります。

アプリケーションでは、バックグラウンドスレッドを使用して、web サービスからデータを取得するなど、長時間実行される可能性がある操作を実行することがあります。 バックグラウンドスレッドで実行されているコードがユーザーインターフェイス要素にアクセスする必要がある場合は、メインスレッドでそのコードを実行する必要があります。

クラスには `Device` 、 `static` バックグラウンドスレッドからユーザーインターフェイス要素を操作するために使用できる次のメソッドが含まれています。

| メソッド | 引数 | 戻り値 | 目的 |
|---|---|---|---|
| `BeginInvokeOnMainThread` | `Action` | `void` | `Action`メインスレッドでを呼び出し、完了するまで待機しません。 |
| `InvokeOnMainThreadAsync<T>` | `Func<T>` | `Task<T>` | メイン スレッド上で `Func<T>` を呼び出し、それが完了するまで待機します。 |
| `InvokeOnMainThreadAsync` | `Action` | `Task` | メイン スレッド上で `Action` を呼び出し、それが完了するまで待機します。 |
| `InvokeOnMainThreadAsync<T>`| `Func<Task<T>>` | `Task<T>` | メイン スレッド上で `Func<Task<T>>` を呼び出し、それが完了するまで待機します。 |
| `InvokeOnMainThreadAsync` | `Func<Task>` | `Task` | メイン スレッド上で `Func<Task>` を呼び出し、それが完了するまで待機します。 |
| `GetMainThreadSynchronizationContextAsync` | | `Task<SynchronizationContext>` | メイン スレッドの `SynchronizationContext` を返します。 |

次のコードは、メソッドの使用例を示してい `BeginInvokeOnMainThread` ます。

```csharp
Device.BeginInvokeOnMainThread (() =>
{
    // interact with UI elements
});
```

## <a name="related-links"></a>関連リンク

- [デバイスのサンプル](/samples/xamarin/xamarin-forms-samples/workingwithdevice)
- [スタイルのサンプル](/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [デバイス API](xref:Xamarin.Forms.Device)