---
title: Xamarin. Forms デバイスクラス
description: この記事では、Xamarin のデバイスクラスを使用して、プラットフォームごとに機能とレイアウトをきめ細かく制御する方法について説明します。
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/12/2019
ms.openlocfilehash: 25ddbea75d0fd6858f848499281da5d5f0b68171
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697200"
---
# <a name="xamarinforms-device-class"></a>Xamarin. Forms デバイスクラス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithdevice)

[@No__t_1](xref:Xamarin.Forms.Device)クラスには、開発者がプラットフォームごとにレイアウトと機能をカスタマイズするのに役立つさまざまなプロパティとメソッドが含まれています。

@No__t_0 クラスには、特定のハードウェアの種類とサイズでコードをターゲットにするためのメソッドとプロパティに加えて、バックグラウンドスレッドから UI コントロールを操作するために使用できるメソッドが含まれています。 詳細については、「[バックグラウンドスレッドから UI を操作する](#interact-with-the-ui-from-background-threads)」を参照してください。

## <a name="providing-platform-specific-values"></a>プラットフォーム固有の値の指定

2\.3.4 の前に、アプリケーションが実行されていたプラットフォームを取得するには、 [`Device.OS`](xref:Xamarin.Forms.Device.OS)プロパティを調べ、それを[`TargetPlatform.iOS`](xref:Xamarin.Forms.TargetPlatform.iOS)、 [`TargetPlatform.Android`](xref:Xamarin.Forms.TargetPlatform.Android)、 [`TargetPlatform.WinPhone`](xref:Xamarin.Forms.TargetPlatform.WinPhone)、および[`TargetPlatform.Windows`](xref:Xamarin.Forms.TargetPlatform.Windows)列挙型と比較します。値. 同様に、 [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action))のオーバーロードの1つを使用して、プラットフォーム固有の値をコントロールに提供できます。

ただし、2.3.4 はこれらの Api を非推奨と置き換えたため、 [@No__t_1](xref:Xamarin.Forms.Device)クラスには、プラットフォーム ( [`Device.iOS`](xref:Xamarin.Forms.Device.iOS)、 [`Device.Android`](xref:Xamarin.Forms.Device.Android)、`Device.WinPhone` (非推奨)、`Device.WinRT` (非推奨[)、`Device.UWP`、および](xref:Xamarin.Forms.Device.UWP) [`Device`1](xref:Xamarin.Forms.Device.macOS)を識別するパブリック文字列定数が含まれるようになりました。 同様に、 [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action))のオーバーロードは、 [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) api と[`On`](xref:Xamarin.Forms.On) api に置き換えられました。

でC#は、 [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform)プロパティに `switch` ステートメントを作成し、必要なプラットフォームに対して `case` ステートメントを指定することによって、プラットフォーム固有の値を指定できます。

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

[@No__t_1](xref:Xamarin.Forms.OnPlatform`1)クラスと[`On`](xref:Xamarin.Forms.On)クラスは、XAML と同じ機能を提供します。

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

[@No__t_1](xref:Xamarin.Forms.OnPlatform`1)クラスは、ターゲットの型に一致する `x:TypeArguments` 属性を使用してインスタンス化する必要があるジェネリッククラスです。 [@No__t_1](xref:Xamarin.Forms.On)クラスでは、 [`Platform`](xref:Xamarin.Forms.On.Platform)属性は1つの `string` 値、または複数のコンマ区切り `string` 値を受け取ることができます。

> [!IMPORTANT]
> @No__t_1 クラスに正しくない `Platform` 属性値を指定しても、エラーは発生しません。 代わりに、プラットフォーム固有の値が適用されずにコードが実行されます。

また、XAML で `OnPlatform` マークアップ拡張機能を使用して、プラットフォームごとに UI の外観をカスタマイズすることもできます。 詳細については、「 [Onplatform Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform)」を参照してください。

## <a name="deviceidiom"></a>デバイス. 表現形式

@No__t_0 プロパティを使用すると、アプリケーションが実行されているデバイスに応じてレイアウトまたは機能を変更できます。 [@No__t_1](xref:Xamarin.Forms.TargetIdiom)列挙には、次の値が含まれます。

- **電話**– IPhone、iPod touch、および Android デバイスが 600 dip ^ よりも狭い
- **タブレット**– IPad、Windows デバイス、および Android デバイスが 600 dip ^ よりも大きい
- **デスクトップ**– windows 10 デスクトップコンピューター上の[UWP アプリ](~/xamarin-forms/platform/windows/installation/index.md)でのみ返されます (連続性のあるシナリオでは、モバイル windows デバイスで `Phone` を返します)。
- **Tv** – Tizen tv デバイス
- **Watch** – Tizen watch デバイス
- **サポート**されていない–未使用

*^ dip は必ずしも物理ピクセル数ではありません*

@No__t_0 プロパティは、次のように、大きな画面を利用するレイアウトを作成する場合に特に便利です。

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

[@No__t_1](xref:Xamarin.Forms.OnIdiom`1)クラスは、XAML と同じ機能を提供します。

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

[@No__t_1](xref:Xamarin.Forms.OnPlatform`1)クラスは、ターゲットの型に一致する `x:TypeArguments` 属性を使用してインスタンス化する必要があるジェネリッククラスです。

また、XAML で `OnIdiom` マークアップ拡張機能を使用して、アプリケーションが実行されているデバイスの表現方法に基づいて UI の外観をカスタマイズすることもできます。 詳細については、「 [Onidiom のマークアップ拡張](~/xamarin-forms/xaml/markup-extensions/consuming.md#onidiom)」を参照してください。

## <a name="deviceflowdirection"></a>System.windows.flowdirection>

[@No__t_1](xref:Xamarin.Forms.VisualElement.FlowDirection)値は、デバイスによって使用されている現在のフロー方向を表す[`FlowDirection`](xref:Xamarin.Forms.FlowDirection)列挙値を取得します。 フロー方向とは、ページ上の UI 要素を視覚でスキャンしていく方向のことです。 列挙値は、次のとおりです。

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

XAML では、`x:Static` マークアップ拡張機能を使用して[`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)値を取得できます。

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

のC#同等のコードは次のとおりです。

```csharp
this.FlowDirection = Device.FlowDirection;
```

フローの方向の詳細については、「[右から左へのローカライズ](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)」を参照してください。

## <a name="devicestyles"></a>デバイス. スタイル

[@No__t_1 プロパティ](~/xamarin-forms/user-interface/styles/index.md)には、いくつかのコントロール (`Label` など) `Style` プロパティに適用できる組み込みのスタイル定義が含まれています。 使用できるスタイルは次のとおりです。

- BodyStyle
- CaptionStyle
- ListItemDetailTextStyle
- ListItemTextStyle
- Subタイトルのスタイル
- タイトルのスタイル

## <a name="devicegetnamedsize"></a>デバイス. GetNamedSize

`GetNamedSize` は、コードでC# [`FontSize`](~/xamarin-forms/user-interface/text/fonts.md)を設定するときに使用できます。

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

## <a name="devicestarttimer"></a>デバイス. StartTimer

@No__t_0 クラスには、`StartTimer` メソッドもあります。このメソッドを使用すると、.NET Standard ライブラリを含む、Xamarin. Forms 共通コードで動作する時間に依存するタスクを簡単にトリガーできます。 @No__t_0 を渡して間隔を設定し、`true` を返して、タイマーの実行を維持するか、現在の呼び出しの後に停止するように `false` します。

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () =>
{
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

タイマー内のコードがユーザーインターフェイスと対話する場合 (`Label` のテキストの設定や警告の表示など)、`BeginInvokeOnMainThread` 式 (下記参照) の内部で実行する必要があります。

> [!NOTE]
> @No__t_0 クラスと `System.Threading.Timer` クラスは、`Device.StartTimer` メソッドを使用する代替手段として .NET Standard ます。

## <a name="interact-with-the-ui-from-background-threads"></a>バックグラウンドスレッドから UI を操作する

IOS、Android、およびユニバーサル Windows プラットフォームを含むほとんどのオペレーティングシステムは、ユーザーインターフェイスを含むコードにシングルスレッドモデルを使用します。 このスレッドは、多くの場合、*メインスレッド*または*UI スレッド*と呼ばれます。 このモデルの結果として、ユーザーインターフェイス要素にアクセスするすべてのコードは、アプリケーションのメインスレッドで実行する必要があります。

アプリケーションでは、バックグラウンドスレッドを使用して、web サービスからデータを取得するなど、長時間実行される可能性がある操作を実行することがあります。 バックグラウンドスレッドで実行されているコードがユーザーインターフェイス要素にアクセスする必要がある場合は、メインスレッドでそのコードを実行する必要があります。

@No__t_0 クラスには、バックグラウンドスレッドからユーザーインターフェイス要素を操作するために使用できる次の `static` メソッドが含まれています。

| メソッド | 引数 | 戻り値 | 目的 |
|---|---|---|---|
| `BeginInvokeOnMainThread` | `Action` | `void` | メインスレッドで `Action` を呼び出します。この処理が完了するまで待機しません。 |
| `InvokeOnMainThreadAsync<T>` | `Func<T>` | `Task<T>` | メイン スレッド上で `Func<T>` を呼び出し、それが完了するまで待機します。 |
| `InvokeOnMainThreadAsync` | `Action` | `Task` | メイン スレッド上で `Action` を呼び出し、それが完了するまで待機します。 |
| `InvokeOnMainThreadAsync<T>`| `Func<Task<T>>` | `Task<T>` | メイン スレッド上で `Func<Task<T>>` を呼び出し、それが完了するまで待機します。 |
| `InvokeOnMainThreadAsync` | `Func<Task>` | `Task` | メイン スレッド上で `Func<Task>` を呼び出し、それが完了するまで待機します。 |
| `GetMainThreadSynchronizationContextAsync` | | `Task<SynchronizationContext>` | メイン スレッドの `SynchronizationContext` を返します。 |

次のコードは、`BeginInvokeOnMainThread` メソッドの使用例を示しています。

```csharp
Device.BeginInvokeOnMainThread (() =>
{
    // interact with UI elements
});
```

## <a name="related-links"></a>関連リンク

- [デバイスのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithdevice)
- [スタイルのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [デバイス](xref:Xamarin.Forms.Device)
