---
title: "iOS でのメインスレッド制御の更新" の説明: "プラットフォーム固有では、特定のプラットフォームでのみ使用可能な機能を使用できます。カスタムレンダラーや特殊効果を実装する必要はありません。 この記事では、コントロールのレイアウトとレンダリングの更新をメインスレッドで実行できるようにする iOS プラットフォーム固有のを使用する方法について説明します。 "
ms. 製品: xamarin ms. assetid: 945E711D-9BD2-4BF9-9FB3-CBE0D5B25A49: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 10/24/2018 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="main-thread-control-updates-on-ios"></a>IOS でのメインスレッド制御の更新

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有では、バックグラウンドスレッドで実行するのではなく、メインスレッドでコントロールレイアウトとレンダリング更新を実行できます。 これはほとんど必要ありませんが、場合によってはクラッシュを防ぐことができます。 バインド可能なプロパティをに設定することにより、XAML で使用され `Application.HandleControlUpdatesOnMainThread` `true` ます。

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.HandleControlUpdatesOnMainThread="true">
    ...
</Application>
```

または、fluent API を使用して C# から使用することもできます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetHandleControlUpdatesOnMainThread(true);
```

メソッドは、 `Application.On<iOS>` このプラットフォーム固有のが iOS 上でのみ実行されることを指定します。 `Application.SetHandleControlUpdatesOnMainThread`名前空間のメソッドは、 [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) バックグラウンドスレッドで実行されるのではなく、コントロールのレイアウトとレンダリングの更新をメインスレッドで実行するかどうかを制御するために使用されます。 また、メソッドを使用して、 `Application.GetHandleControlUpdatesOnMainThread` コントロールのレイアウトとレンダリングの更新がメインスレッドで実行されているかどうかを返すことができます。

## <a name="related-links"></a>関連リンク

- [PlatformSpecifics (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム固有設定の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
