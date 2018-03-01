---
title: "デバイス クラス"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 15d5e2b5a7397ddb0d993b1cacf22edb175745ea
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="device-class"></a>デバイス クラス

[ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/)クラスには、レイアウトとプラットフォームごとの機能をカスタマイズする開発者を支援するメソッドとプロパティの数が含まれています。

特定のハードウェアの種類、およびサイズでターゲット コードするメソッドとプロパティだけでなく、`Device`クラスが含まれています、 [BeginInvokeOnMainThread](#Device_BeginInvokeOnMainThread)メソッドから制御 UI と対話するときに使用する必要がありますバック グラウンド スレッドです。

<a name="providing-platform-values" />

## <a name="providing-platform-specific-values"></a>プラットフォーム固有の値を指定します。

Xamarin.Forms 2.3.4、前に、アプリケーションが実行されていたプラットフォームを調べることによって入手でした、 [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/)プロパティと比較して、 [ `TargetPlatform.iOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/)、 [`TargetPlatform.Android` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/)、 [ `TargetPlatform.WinPhone` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/)、および[ `TargetPlatform.Windows` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/)列挙値。 同様に、1 つの[ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/)オーバー ロードを使用して、コントロールにプラットフォーム固有の値を提供する可能性があります。

ただし、Xamarin.Forms 2.3.4 以降これらの Api 廃止され、置き換えられました。 [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/)クラス プラットフォームを識別するパブリック文字列定数を含むようになりました[ `Device.iOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/)、 [ `Device.Android` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/)、 [ `Device.WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/)、 [ `Device.WinRT` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinRT/)、 [ `Device.UWP` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/)、および[ `Device.macOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.macOS/)です。 同様に、 [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/)オーバー ロードが置き換えられて、 [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/)と[ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) Api です。

作成することで、C# の場合は、プラットフォーム固有の値を指定する、`switch`のステートメントで、 [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/)プロパティ、および提供`case`の必要なプラットフォーム ステートメント。

```csharp
double top;
switch (Device.RuntimePlatform)
{
  case Device.iOS:
    top = 20;
    break;
  case Device.Android:
  case Device.WinPhone:
  case Device.UWP:
  default:
    top = 0;
    break;
}
layout.Margin = new Thickness(5, top, 5, 0);
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/)と[ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/)クラスは、XAML で同じ機能を提供します。

```xaml
<StackLayout>
  <StackLayout.Margin>
    <OnPlatform x:TypeArguments="Thickness">
      <On Platform="iOS" Value="0,20,0,0" />
      <On Platform="Android, WinPhone, UWP" Value="0,0,0,0" />
    </OnPlatform>
  </StackLayout.Margin>
  ...
</StackLayout>
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/)クラスはジェネリック クラスであり、使用してインスタンス化する必要があります、`x:TypeArguments`対象の型と一致する属性。 [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/)クラス、 [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/)属性は、1 つを受け入れることができます`string`値、またはコンマで区切られた複数`string`値。

> [!IMPORTANT]
> 正しくないを提供する`Platform`属性の値、`On`クラスは、エラーは発生しません。 代わりに、コードに適用されているプラットフォームに固有の値を使用せず実行されます。

<a name="Device_Idiom" />

## <a name="deviceidiom"></a>Device.Idiom

`Device.Idiom`レイアウトやアプリケーションがで実行されているデバイスによって機能を変更するために使用できます。 [ `TargetIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/)列挙には、次の値が含まれています。

-  **Phone** – iPhone、iPod touch、Windows Phone、Android デバイス 600 dip よりも幅の狭い ^
-  **タブレット**: iPad、Windows 8.1 コンピューターでは、Android デバイス 600 dip よりも太くなって ^
-  **デスクトップ**– で返されるのみ[UWP アプリ](~/xamarin-forms/platform/windows/installation/universal.md)Windows 10 desktop コンピューターに (返します`Phone`一連のシナリオでなど、Windows のモバイル デバイス上)
-  **テレビ**– Tizen TV デバイス
-  **サポートされていない**未使用

*^ dip は必ずしも物理的なピクセルの数*

`Idiom` 次のようより大きな画面を活用するレイアウトを構築するために役立ちます。

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

<a name="Device_Styles" />

## <a name="devicestyles"></a>Device.Styles

[ `Styles`プロパティ](~/xamarin-forms/user-interface/styles/index.md)一部のコントロールに適用できる組み込みのスタイル定義が含まれます (など`Label`)`Style`プロパティです。 使用できるスタイルは次のとおりです。

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

`OpenUri`ネイティブ web ブラウザーで url を開くなど、基になるプラットフォームでの操作をトリガーするメソッドを使用できます (**Safari** iOS 上または**インターネット**Android 上)。

```csharp
Device.OpenUri(new Uri("https://evolve.xamarin.com/"));
```

[WebView サンプル](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithWebview/WorkingWithWebview/WebAppPage.cs)を使用して、例が含まれています`OpenUri`する Url を開き、トリガー電話をかけることもできます。

[マップ サンプル](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/MapAppPage.cs)も使用`Device.OpenUri`マップと、native を使用しての方向を表示する**マップ**iOS および Android 上のアプリです。

<a name="Device_StartTimer" />

## <a name="devicestarttimer"></a>Device.StartTimer

`Device`クラスもあります、 `StartTimer` Xamarin.Forms の一般的なコード (Pcl を含む) で動作する時間に依存するタスクをトリガーする簡単な方法を提供するメソッド。 渡す、`TimeSpan`間隔を設定して返す`true`を実行しているタイマーを保持するまたは`false`を現在の呼び出し後に停止します。

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () => {
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

タイマーの内部コードがユーザー インターフェイスとやり取りするかどうか (のテキストを設定するなど、`Label`通知を表示するか) 内で行う必要がありますが、`BeginInvokeOnMainThread`式 (下記参照)。

<a name="Device_BeginInvokeOnMainThread" />

## <a name="devicebegininvokeonmainthread"></a>Device.BeginInvokeOnMainThread

実行して、タイマーまたは web 要求と同様に、非同期操作の完了ハンドラー コードなどのバック グラウンド スレッドで、ユーザー インターフェイス要素をアクセスしないようにします。 内部ユーザー インターフェイスを更新する必要があるすべてのバック グラウンド コードをラップする必要があります[ `BeginInvokeOnMainThread`](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/)です。 これは、相当の`InvokeOnMainThread`ios、 `RunOnUiThread` android でと`Dispatcher.BeginInvoke`Windows Phone でします。

Xamarin.Forms コードは次のとおりです。

```csharp
Device.BeginInvokeOnMainThread ( () => {
  // interact with UI elements
});
```

メモを使用してそのメソッド`async/await`を使用する必要はありません`BeginInvokeOnMainThread`メイン UI スレッドから実行されている場合。

## <a name="summary"></a>まとめ

Xamarin.Forms`Device`クラスは、プラットフォームごとの機能とレイアウトの詳細に制御を使用できます。-コード (PCL または共有プロジェクト) でも共通します。


## <a name="related-links"></a>関連リンク

- [デバイスのサンプル](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)
- [スタイルのサンプル](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [デバイス](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/)
