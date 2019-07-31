---
title: IOS でのメインスレッド制御の更新
description: プラットフォーム仕様はカスタム レンダラーや特殊効果を実装することなく、特定のプラットフォームでのみ利用できる機能の使用を可能にします。 この記事では、メインスレッドでコントロールのレイアウトとレンダリングの更新を実行できるようにする iOS プラットフォーム固有のを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 945E711D-9BD2-4BF9-9FB3-CBE0D5B25A49
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 9603cccc1f08be057bc66012cdde75e1b7391f1a
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655275"
---
# <a name="main-thread-control-updates-on-ios"></a>IOS でのメインスレッド制御の更新

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

この iOS プラットフォーム固有では、バックグラウンドスレッドで実行するのではなく、メインスレッドでコントロールレイアウトとレンダリング更新を実行できます。 ほとんど必要する必要がありますが、場合によってはクラッシュができない可能性があります。 設定してそので消費された XAML、`Application.HandleControlUpdatesOnMainThread`バインド可能なプロパティを`true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.HandleControlUpdatesOnMainThread="true">
    ...
</Application>
```

代わりに、fluent API を使用して C# から使用できます。

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetHandleControlUpdatesOnMainThread(true);
```

`Application.On<iOS>`メソッドは、このプラットフォーム仕様が iOS上 でのみ動作することを指定します。 `Application.SetHandleControlUpdatesOnMainThread`メソッドで、 [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)名前空間を使用してコントロールのレイアウトとレンダリングの更新プログラムがバック グラウンド スレッドで実行されているのではなく、メイン スレッドで実行されるかどうか。 さらに、`Application.GetHandleControlUpdatesOnMainThread`をコントロールのレイアウトとレンダリングの更新プログラムをメイン スレッドで実行されているかどうかを返すメソッドを使用できます。

## <a name="related-links"></a>関連リンク

- [プラットフォーム仕様 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [プラットフォーム仕様の作成](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific の API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
