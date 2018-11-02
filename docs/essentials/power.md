---
title: 'Xamarin.Essentials: 電源の省電力の状態'
description: Power クラスを使用すると、プログラムで省電力の状態を取得して、デバイスが低電力モードで動作しているかどうかを判断できます。
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: jamesmontemagno
ms.author: jamont
ms.date: 06/27/2018
ms.openlocfilehash: 5a89dba16a93b007c5d7312221d8d33e00c7404a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110005"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials: 電源の省電力の状態

![プレリリースの NuGet](~/media/shared/pre-release.png)

**Power** クラスでは、デバイスの省電力の状態に関する情報を取得できます。これはデバイスが低電力モードで実行されているかどうかを示します。 デバイスの省電力状態がオンになっている場合、アプリケーションはバックグラウンド処理を避ける必要があります。

## <a name="background"></a>背景

バッテリで動作するデバイスは、低電力の省電力モードに切り替えることができます。 デバイスが自動的にこのモードに切り替わる場合があります。たとえば、バッテリ残量が 20% を下回ったときなどです。 オペレーティング システムは、バッテリを消耗させる傾向があるアクティビティを減らすことで、省電力モードに対応します。 アプリケーションでは、バックグラウンド処理やその他の電力消費の大きいアクティビティを回避することで、省電力モードがオンになった場合をサポートできます。

Android デバイスの場合、**Power** クラスによって意味のある情報が返されるのは、バージョン 5.0 (Lollipop) 以降の Android のみです。

## <a name="using-the-power-class"></a>Power クラスの使用

自分のクラスの Xamarin.Essentials に参照を追加します。

```csharp
using Xamarin.Essentials;
```

静的プロパティ `Power.EnergySaverStatus` を使用して、デバイスの現在の省電力状態を取得します。

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

このプロパティは `EnergySaverStatus` 列挙型のメンバーを返します。それは `On`、`Off`、または `Unknown` です。 プロパティが `On` を返す場合、アプリケーションでは、バックグラウンド処理やその他の電力消費の大きいアクティビティを回避する必要があります。

アプリケーションでは、イベント ハンドラーもインストールする必要があります。 **Power** クラスでは、省電力の状態が変更されたときにトリガーされるイベントが公開されています。

```csharp
public class EnergySaverTest
{
    public EnergySaverTest()
    {
        // Subscribe to changes of energy-saver status
        Power.EnergySaverStatusChanged += OnEnergySaverStatusChanged;
    }

    private void OnEnergySaverStatusChanged(EnergySaverStatusChangedEventArgs e)
    {
        // Process change
        var status = e.EnergySaverStatus;
    }
}
```

省電力の状態が `On` に変わった場合、アプリケーションはバックグラウンド処理の実行を停止させる必要があります。 状態が `Unknown` または `Off` に変わった場合、アプリケーションはバックグラウンド処理を再開することができます。

## <a name="api"></a>API

- [Power のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Power API ドキュメント](xref:Xamarin.Essentials.Power)
