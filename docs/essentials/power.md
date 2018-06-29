---
title: 'Xamarin.Essentials: 電源エネルギー省ステータス'
description: 電源クラスでは、プログラムがデバイスが低電力モードで動作しているかどうかを決定する省電力状態を取得できるようにします。
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: charlespetzold
ms.author: chape
ms.date: 06/27/2018
ms.openlocfilehash: 6d8ccb5be69eb1ea7ea63d3f5c373d9284089679
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080532"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials: 電源エネルギー省ステータス

![プレリリース NuGet](~/media/shared/pre-release.png)

**Power**クラスは、デバイスが低電力モードで実行されているかどうかを示すデバイスの電力を節約の状態に関する情報を提供します。 デバイスの省電力状態にある場合、アプリケーションはバック グラウンド処理を避ける必要があります。

## <a name="background"></a>背景

バッテリで動作するデバイスは、低電力の消費電力を節約モードに配置できます。 場合もありますデバイスはモードに切り替えるこの自動的に、たとえば、バッテリが 20% を下回ると。 オペレーティング システムは、バッテリを消費する可能性があるアクティビティを減らすことによって、省電力モードに応答します。 アプリケーションは、省電力モードがオンの場合は、バック グラウンド処理または変量他のアクティビティを回避することで役立ちます。

Android デバイスで、 **Power**クラスは、以降の Android バージョン 5.0 (ロリポップ) にのみ意味のある情報を返します。

## <a name="using-the-power-class"></a>電源クラスを使用します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

静的なを使用してデバイスの現在の省電力状態の取得`Power.EnergySaverStatus`プロパティ。

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

このプロパティのメンバーを返します、`EnergySaverStatus`列挙体は、次のいずれか`On`、 `Off`、または`Unknown`です。 プロパティを返す場合`On`アプリケーションは、バック グラウンド処理または電源の多くを消費する可能性がある他のアクティビティを避ける必要があります。

アプリケーションでは、イベント ハンドラーもインストールする必要があります。 **Power**クラスは、省電力状態が変更されたときにトリガーされるイベントを公開します。

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Power.EnergySaverStatusChanaged += OnEnergySaverStatusChanaged;
    }

    private void OnEnergySaverStatusChanaged(EnergySaverStatusChanagedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

省電力状態に変わる場合`On`アプリケーションがバック グラウンド処理の実行を停止する必要があります。 状態に変わる場合`Unknown`または`Off`アプリケーションはバック グラウンド処理を再開できます。

## <a name="api"></a>API

- [ソース コードの電源](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [電源 API ドキュメント](xref:Xamarin.Essentials.Power)
