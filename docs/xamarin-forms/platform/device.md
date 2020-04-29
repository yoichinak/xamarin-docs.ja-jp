---
title: Xamarin. Forms デバイスクラス
description: この記事では、Xamarin のデバイスクラスを使用して、プラットフォームごとに機能とレイアウトをきめ細かく制御する方法について説明します。
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/17/2020
ms.openlocfilehash: d0f0fa7dd68e8852dd7a72486c155ec064540644
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517068"
---
# <a name="xamarinforms-device-class"></a>Xamarin. Forms デバイスクラス

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithdevice)

クラス[`Device`](xref:Xamarin.Forms.Device)には、開発者がプラットフォームごとにレイアウトと機能をカスタマイズするのに役立つさまざまなプロパティとメソッドが含まれています。

`Device`クラスには、特定のハードウェアの種類とサイズでコードをターゲットにするためのメソッドとプロパティに加えて、バックグラウンドスレッドから UI コントロールを操作するために使用できるメソッドが用意されています。 詳細については、「[バックグラウンドスレッドから UI を操作する](#interact-with-the-ui-from-background-threads)」を参照してください。

## <a name="provide-platform-specific-values"></a>プラットフォーム固有の値を指定する

Xamarin [`Device.OS`](xref:Xamarin.Forms.Device.OS) 2.3.4 より前では、アプリケーションが実行されていたプラットフォームは、プロパティを調べて[`TargetPlatform.iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)、 [`TargetPlatform.Android`](xref:Xamarin.Forms.TargetPlatform.Android)、、 [`TargetPlatform.WinPhone`](xref:Xamarin.Forms.TargetPlatform.WinPhone)の各[`TargetPlatform.Windows`](xref:Xamarin.Forms.TargetPlatform.Windows)列挙値と比較することによって取得できます。 同様に、オーバーロードの[`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) 1 つを使用して、プラットフォーム固有の値をコントロールに提供できます。

ただし、2.3.4 はこれらの Api を非推奨と置き換えたため、 クラス[`Device`](xref:Xamarin.Forms.Device)には、、、 `Device.WinPhone`( `Device.WinRT`非推奨)、(非推奨) [`Device.UWP`](xref:Xamarin.Forms.Device.UWP)、、 [`Device.macOS`](xref:Xamarin.Forms.Device.macOS)およびの各プラットフォーム[`Device.iOS`](xref:Xamarin.Forms.Device.iOS) [`Device.Android`](xref:Xamarin.Forms.Device.Android)を識別するパブリック文字列定数が含まれるようになりました。 同様に、 [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action))オーバーロードは[`On`](xref:Xamarin.Forms.On) api と api に[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)置き換えられています。

C# では、 `switch` [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform)プロパティにステートメントを作成し、必要なプラットフォームのステートメントを指定`case`することによって、プラットフォーム固有の値を指定できます。

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

クラス[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)と[`On`](xref:Xamarin.Forms.On)クラスは、XAML で同じ機能を提供します。

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

[`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1)クラスは、対象の型に一致する`x:TypeArguments`属性を使用してインスタンス化する必要があるジェネリッククラスです。 [`On`](xref:Xamarin.Forms.On)クラスでは、 [`Platform`](xref:Xamarin.Forms.On.Platform)属性は単一`string`の値または複数のコンマ区切り`string`値を受け取ることができます。

> [!IMPORTANT]
> `On`クラスに正しくない`Platform`属性値を指定しても、エラーは発生しません。 代わりに、プラットフォーム固有の値が適用されずにコードが実行されます。

また、XAML `OnPlatform`でマークアップ拡張機能を使用して、プラットフォームごとに UI の外観をカスタマイズすることもできます。 詳細については、「 [Onplatform Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform)」を参照してください。

## <a name="deviceidiom"></a>デバイス. 表現形式

プロパティ`Device.Idiom`は、アプリケーションが実行されているデバイスに応じてレイアウトまたは機能を変更するために使用できます。 列挙[`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom)には、次の値が含まれます。

- **電話**– IPhone、iPod touch、および Android デバイスが 600 dip ^ よりも狭い
- **タブレット**– IPad、Windows デバイス、および Android デバイスが 600 dip ^ よりも大きい
- **デスクトップ**– windows 10 デスクトップコンピューター上の[UWP アプリ](~/xamarin-forms/platform/windows/installation/index.md)でのみ返され`Phone`ます (連続性のあるシナリオの場合を含め、モバイル Windows デバイスでを返します)。
- **Tv** – Tizen tv デバイス
- **Watch** – Tizen watch デバイス
- **サポート**されていない–未使用

*^ dip は必ずしも物理ピクセル数ではありません*

`Idiom`プロパティは、次のように、大きな画面を利用するレイアウトを作成する場合に特に便利です。

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

クラス[`OnIdiom`](xref:Xamarin.Forms.OnIdiom`1)は、XAML と同じ機能を提供します。

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

[`OnIdiom`](xref:Xamarin.Forms.OnPlatform`1)クラスは、対象の型に一致する`x:TypeArguments`属性を使用してインスタンス化する必要があるジェネリッククラスです。

また、XAML `OnIdiom`でマークアップ拡張機能を使用して、アプリケーションが実行されているデバイスの表現方法に基づいて UI の外観をカスタマイズすることもできます。 詳細については、「 [Onidiom のマークアップ拡張](~/xamarin-forms/xaml/markup-extensions/consuming.md#onidiom)」を参照してください。

## <a name="deviceflowdirection"></a>System.windows.flowdirection>

値[`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)は、デバイス[`FlowDirection`](xref:Xamarin.Forms.FlowDirection)によって使用されている現在のフロー方向を表す列挙値を取得します。 フロー方向とは、ページ上の UI 要素を視覚でスキャンしていく方向のことです。 列挙値は、次のとおりです。

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

XAML では、 [`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) `x:Static`マークアップ拡張機能を使用して値を取得できます。

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

C# の同等のコードは次のとおりです。

```csharp
this.FlowDirection = Device.FlowDirection;
```

フローの方向の詳細については、「[右から左へのローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)」を参照してください。

## <a name="devicestyles"></a>デバイス. スタイル

[ `Styles`プロパティ](~/xamarin-forms/user-interface/styles/index.md)には、いくつかのコントロールのプロパティ (など`Label`) `Style`に適用できる組み込みのスタイル定義が含まれています。 使用できるスタイルは次のとおりです。

- BodyStyle
- CaptionStyle
- ListItemDetailTextStyle
- ListItemTextStyle
- Subタイトルのスタイル
- タイトルのスタイル

## <a name="devicegetnamedsize"></a>デバイス. GetNamedSize

`GetNamedSize`C# コードでを設定[`FontSize`](~/xamarin-forms/user-interface/text/fonts.md)するときに使用できます。

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

## <a name="devicegetnamedcolor"></a>デバイス. GetNamedColor

Xamarin. Forms 4.6 では、名前付きの色のサポートが導入されています。 名前付きの色とは、デバイス上でアクティブになっているシステムモード (たとえば、淡色や濃色) に応じて異なる値を持つ色です。 Android では、名前付きの色には、 [R. Color](https://developer.android.com/reference/android/R.color#constants_2)クラスを使用してアクセスします。 IOS では、名前付きの色は[システムカラー](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/color/#system-colors)と呼ばれます。 ユニバーサル Windows プラットフォームでは、名前付きの色は[XAML テーマリソース](/windows/uwp/design/controls-and-patterns/xaml-theme-resources)と呼ばれます。

`GetNamedColor`メソッドを使用して、Android、IOS、UWP の名前付きの色を取得できます。 メソッドは引数を`string`受け取り、を[`Color`](xref:Xamarin.Forms.Color)返します。

```csharp
// Retrieve an Android named color
Color color = Device.GetNamedColor(NamedPlatformColor.HoloBlueBright);
```

`Color.Default`は、色の名前が見つからない場合、またはサポート`GetNamedColor`されていないプラットフォームでが呼び出された場合に返されます。

> [!NOTE]
> このメソッド`GetNamedColor`は、プラットフォーム`Color`に固有のを返すため、通常は[`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform)プロパティと組み合わせて使用する必要があります。

クラス`NamedPlatformColor`には、Android、IOS、UWP の名前付きの色を定義する定数が含まれています。

| Android | iOS | UWP |
| --- | --- | --- |
| `BackgroundDark` | `Label` | `SystemAltHighColor` |
| `BackgroundLight` | `Link` | `SystemAltLowColor` |
| `Black` | `OpaqueSeparator` | `SystemAltMediumColor` |
| `DarkerGray` | `PlaceholderText` | `SystemAltMediumHighColor` |
| `HoloBlueBright` | `QuaternaryLabel` | `SystemAltMediumLowColor` |
| `HoloBlueDark` | `SecondaryLabel` | `SystemBaseHighColor` |
| `HoloBlueLight` | `Separator` | `SystemBaseLowColor` |
| `HoloGreenDark` | `SystemBlue` | `SystemBaseMediumColor` |
| `HoloGreenLight` | `SystemGray` | `SystemBaseMediumHighColor` |
| `HoloOrangeDark` | `SystemGray2` | `SystemBaseMediumLowColor` |
| `HoloOrangeLight` | `SystemGray3` | `SystemChromeAltLowColor` |
| `HoloPurple` | `SystemGray4` | `SystemChromeBlackHighColor` |
| `HoloRedDark` | `SystemGray5` | `SystemChromeBlackLowColor` |
| `HoloRedLight` | `SystemGray6` | `SystemChromeBlackMediumColor` |
| `TabIndicatorText` | `SystemGreen` | `SystemChromeBlackMediumLowColor` |
| `Transparent` | `SystemIndigo` | `SystemChromeDisabledHighColor` |
| `White` | `SystemListLowColor` | `SystemChromeDisabledLowColor` |
| `WidgetEditTextDark` | `SystemListMediumColor` | `SystemChromeHighColor` |
| | `SystemPink` | `SystemChromeLowColor` |
| | `SystemPurple` | `SystemChromeMediumColor` |
| | `SystemRed` | `SystemChromeMediumLowColor` |
| | `SystemTeal` | `SystemChromeWhiteColor` |
| | `SystemYellow` |
| | `TertiaryLabel` |

## <a name="devicestarttimer"></a>デバイス. StartTimer

クラス`Device`には、.NET Standard `StartTimer`ライブラリを含む、Xamarin. Forms 共通コードで動作する時間に依存するタスクをトリガーするための簡単な方法を提供するメソッドもあります。 を`TimeSpan`渡して間隔を設定し、 `true`を返してタイマーを実行`false`したままにするか、現在の呼び出しの後に停止します。

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () =>
{
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

タイマー内のコードがユーザーインターフェイスと対話する場合 (のテキストの設定やアラートの表示`Label`など)、式の`BeginInvokeOnMainThread`内部で実行する必要があります (以下を参照)。

> [!NOTE]
> クラス`System.Timers.Timer`と`System.Threading.Timer`クラスは、 `Device.StartTimer`メソッドを使用するための代替手段として .NET Standard ます。

## <a name="interact-with-the-ui-from-background-threads"></a>バックグラウンドスレッドから UI を操作する

IOS、Android、およびユニバーサル Windows プラットフォームを含むほとんどのオペレーティングシステムは、ユーザーインターフェイスを含むコードにシングルスレッドモデルを使用します。 このスレッドは、多くの場合、*メインスレッド*または*UI スレッド*と呼ばれます。 このモデルの結果として、ユーザーインターフェイス要素にアクセスするすべてのコードは、アプリケーションのメインスレッドで実行する必要があります。

アプリケーションでは、バックグラウンドスレッドを使用して、web サービスからデータを取得するなど、長時間実行される可能性がある操作を実行することがあります。 バックグラウンドスレッドで実行されているコードがユーザーインターフェイス要素にアクセスする必要がある場合は、メインスレッドでそのコードを実行する必要があります。

クラス`Device`には、バックグラウンド`static`スレッドからユーザーインターフェイス要素を操作するために使用できる次のメソッドが含まれています。

| Method | 引数 | 戻り値 | 目的 |
|---|---|---|---|
| `BeginInvokeOnMainThread` | `Action` | `void` | メインスレッド`Action`でを呼び出し、完了するまで待機しません。 |
| `InvokeOnMainThreadAsync<T>` | `Func<T>` | `Task<T>` | メイン スレッド上で `Func<T>` を呼び出し、それが完了するまで待機します。 |
| `InvokeOnMainThreadAsync` | `Action` | `Task` | メイン スレッド上で `Action` を呼び出し、それが完了するまで待機します。 |
| `InvokeOnMainThreadAsync<T>`| `Func<Task<T>>` | `Task<T>` | メイン スレッド上で `Func<Task<T>>` を呼び出し、それが完了するまで待機します。 |
| `InvokeOnMainThreadAsync` | `Func<Task>` | `Task` | メイン スレッド上で `Func<Task>` を呼び出し、それが完了するまで待機します。 |
| `GetMainThreadSynchronizationContextAsync` | | `Task<SynchronizationContext>` | メイン スレッドの `SynchronizationContext` を返します。 |

次のコードは、 `BeginInvokeOnMainThread`メソッドの使用例を示しています。

```csharp
Device.BeginInvokeOnMainThread (() =>
{
    // interact with UI elements
});
```

## <a name="related-links"></a>関連リンク

- [デバイスのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithdevice)
- [スタイルのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [デバイス API](xref:Xamarin.Forms.Device)
