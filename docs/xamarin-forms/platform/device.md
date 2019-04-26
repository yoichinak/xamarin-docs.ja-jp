---
title: Xamarin.Forms のデバイス クラス
description: この記事では、機能とレイアウトを細かく制御する、プラットフォームごとに、Xamarin.Forms のデバイス クラスを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/01/2018
ms.openlocfilehash: 4ba4bd7528b635d099868f093268d2d83e44dae0
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61359883"
---
# <a name="xamarinforms-device-class"></a>Xamarin.Forms のデバイス クラス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)

[ `Device` ](xref:Xamarin.Forms.Device)クラスには、さまざまなプロパティとレイアウトと、プラットフォームごとに機能をカスタマイズする開発者を支援するメソッドが含まれています。

ターゲットのコードでは、特定のハードウェアの種類とサイズ、するメソッドとプロパティだけでなく、`Device`クラスが含まれています、 [BeginInvokeOnMainThread](#Device_BeginInvokeOnMainThread)からコントロールを UI と対話する際に使用する必要があるメソッドバック グラウンド スレッドです。

<a name="providing-platform-values" />

## <a name="providing-platform-specific-values"></a>プラットフォーム固有の値を提供します。

Xamarin.Forms 2.3.4、前に、アプリケーションが実行されていたプラットフォームを調べることによって入手でした、 [ `Device.OS` ](xref:Xamarin.Forms.Device.OS)プロパティと比較することに、 [ `TargetPlatform.iOS` ](xref:Xamarin.Forms.TargetPlatform.iOS)、 [`TargetPlatform.Android` ](xref:Xamarin.Forms.TargetPlatform.Android)、 [ `TargetPlatform.WinPhone` ](xref:Xamarin.Forms.TargetPlatform.WinPhone)、および[ `TargetPlatform.Windows` ](xref:Xamarin.Forms.TargetPlatform.Windows)列挙値。 同様に、1 つの[ `Device.OnPlatform` ](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action))コントロールにプラットフォーム固有の値を指定するオーバー ロードを使用する可能性があります。

ただし、Xamarin.Forms 2.3.4 以降これらの Api 非推奨とされ、置き換えられました。 [ `Device` ](xref:Xamarin.Forms.Device)クラスでは、プラットフォームを識別するパブリック文字列定数に含まれて[ `Device.iOS` ](xref:Xamarin.Forms.Device.iOS)、 [ `Device.Android` ](xref:Xamarin.Forms.Device.Android)、 `Device.WinPhone`(非推奨)、 `Device.WinRT` (非推奨)、 [ `Device.UWP` ](xref:Xamarin.Forms.Device.UWP)、および[ `Device.macOS`](xref:Xamarin.Forms.Device.macOS)します。 同様に、 [ `Device.OnPlatform` ](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action))オーバー ロードに置き換えられましたが、 [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1)と[ `On` ](xref:Xamarin.Forms.On) Api。

C# で作成してプラットフォーム固有の値を指定できます、`switch`のステートメントで、 [ `Device.RuntimePlatform` ](xref:Xamarin.Forms.Device.RuntimePlatform)プロパティ、および提供`case`ステートメントの必要なプラットフォーム。

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

[ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1)と[ `On` ](xref:Xamarin.Forms.On)クラスは、XAML で同じ機能を提供します。

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

[ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1)クラスは、ジェネリック クラスをインスタンス化する必要があります、`x:TypeArguments`ターゲット型と一致する属性。 [ `On` ](xref:Xamarin.Forms.On)クラス、 [ `Platform` ](xref:Xamarin.Forms.On.Platform)属性は、1 つを受け入れることができる`string`値、またはコンマで区切られた複数`string`値。

> [!IMPORTANT]
> 提供が不適切な`Platform`属性の値、`On`クラスは、エラーは発生しません。 代わりに、コードは、適用されているプラットフォームに固有の値を指定せずに実行されます。

または、`OnPlatform`プラットフォームごとに UI の外観をカスタマイズするには、XAML マークアップ拡張機能を使用できます。 詳細については、次を参照してください。 [OnPlatform マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform)します。

<a name="Device_Idiom" />

## <a name="deviceidiom"></a>Device.Idiom

`Device.Idiom`レイアウトを変更するプロパティを使用できますか、デバイス、アプリケーションによって機能がで実行されています。 [ `TargetIdiom` ](xref:Xamarin.Forms.TargetIdiom)列挙には、次の値が含まれています。

-  **Phone** – iPhone、iPod touch、および Android デバイスの 600 dip より狭い幅 ^
-  **タブレット**: iPad、Windows デバイス、および Android デバイスの 600 dip よりも広い ^
-  **デスクトップ**– で返されるのみ[UWP アプリ](~/xamarin-forms/platform/windows/installation/index.md)Windows 10 のデスクトップ コンピューター (返します`Phone`Continuum シナリオにおけるを含む、Windows のモバイル デバイスで)
-  **テレビ**– Tizen TV のデバイス
-  **ウォッチ**– Tizen watch デバイス
-  **サポートされていない**– 使用されていません。

*^ dip とは限りません物理ピクセル数*

`Idiom`プロパティは、次のように、大きな画面を活用するレイアウトを構築するために特に便利です。

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

[ `OnIdiom` ](xref:Xamarin.Forms.OnIdiom`1)クラスには、XAML で同じ機能が用意されています。

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

[ `OnIdiom` ](xref:Xamarin.Forms.OnPlatform`1)クラスは、ジェネリック クラスをインスタンス化する必要があります、`x:TypeArguments`ターゲット型と一致する属性。

または、`OnIdiom`で、アプリケーションが実行されているデバイスの表現形式に基づく UI の外観をカスタマイズするには、XAML マークアップ拡張機能を使用できます。 詳細については、次を参照してください。 [OnIdiom マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md#onidiom)します。

## <a name="deviceflowdirection"></a>Device.FlowDirection

[ `Device.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)値の取得、 [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection)デバイスで使用されている現在のフローの方向を表す列挙値。 フロー方向とは、ページ上の UI 要素を視覚でスキャンしていく方向のことです。 列挙値は、次のとおりです。

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToRight`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

XAML、 [ `Device.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection)を使用して値を取得することができます、`x:Static`マークアップ拡張機能。

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

C# での同等のコードに示します。

```csharp
this.FlowDirection = Device.FlowDirection;
```

フローの方向に関する詳細については、次を参照してください。[右から左のローカリゼーション](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)します。

<a name="Device_Styles" />

## <a name="devicestyles"></a>Device.Styles

[ `Styles`プロパティ](~/xamarin-forms/user-interface/styles/index.md)一部のコントロールに適用できる組み込みのスタイル定義が含まれます (など`Label`)`Style`プロパティ。 使用可能なスタイルは次のとおりです。

* BodyStyle
* CaptionStyle
* ListItemDetailTextStyle
* ListItemTextStyle
* SubtitleStyle
* TitleStyle

<a name="Device_GetNamedSize" />

## <a name="devicegetnamedsize"></a>Device.GetNamedSize

`GetNamedSize` 設定時に使用できる[ `FontSize` ](~/xamarin-forms/user-interface/text/fonts.md) c# コードで。

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

<a name="Device_OpenUri" />

## <a name="deviceopenuri"></a>Device.OpenUri

`OpenUri`基になるプラットフォームでネイティブの web ブラウザーで URL を開くなどの操作をトリガーするメソッドを使用できます (**Safari** iOS 上または**インターネット**Android 上)。

```csharp
Device.OpenUri(new Uri("https://evolve.xamarin.com/"));
```

[WebView サンプル](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithWebview/WorkingWithWebview/WebAppPage.cs)使用例が含まれています。`OpenUri`を Url を開き、また電話呼び出しをトリガーします。

[マップのサンプル](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/MapAppPage.cs)では`Device.OpenUri`地図と道順ネイティブを使用して表示する**マップ**iOS と Android アプリ。

<a name="Device_StartTimer" />

## <a name="devicestarttimer"></a>Device.StartTimer

`Device`クラスがあります、 `StartTimer` .NET Standard ライブラリを含む Xamarin.Forms の一般的なコードで動作する時間に依存するタスクをトリガーする簡単な方法を提供するメソッド。 渡す、`TimeSpan`間隔を設定して返す`true`実行されているタイマーを保持するまたは`false`を現在の呼び出しの後に停止します。

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () => {
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

タイマーの内部コードがユーザー インターフェイスとやり取りするかどうか (のテキストを設定するなど、`Label`または通知を表示する) 内で行う必要がありますが、`BeginInvokeOnMainThread`式 (下記参照)。

<a name="Device_BeginInvokeOnMainThread" />

## <a name="devicebegininvokeonmainthread"></a>Device.BeginInvokeOnMainThread

ユーザー インターフェイス要素は、タイマーまたは web 要求などの非同期操作の完了ハンドラーで実行されるコードなどのバック グラウンド スレッドによってアクセスすることことはありません。 ユーザー インターフェイスを更新する必要がある任意のバック グラウンド コード内でラップする必要があります[ `BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action))します。 これは、相当の`InvokeOnMainThread`ios では、 `RunOnUiThread` android と`Dispatcher.RunAsync`ユニバーサル Windows プラットフォームで。

Xamarin.Forms コードです。

```csharp
Device.BeginInvokeOnMainThread ( () => {
  // interact with UI elements
});
```

メモを使用してそのメソッド`async/await`を使用する必要はありません`BeginInvokeOnMainThread`メイン UI スレッドから実行されている場合。

## <a name="summary"></a>まとめ

Xamarin.Forms`Device`クラスは、プラットフォームごとの機能とレイアウトの詳細に制御を使用できます。-コード (.NET Standard ライブラリ プロジェクトまたは共有プロジェクト) でも共通します。


## <a name="related-links"></a>関連リンク

- [デバイスのサンプル](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)
- [スタイルのサンプル](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [デバイス](xref:Xamarin.Forms.Device)
