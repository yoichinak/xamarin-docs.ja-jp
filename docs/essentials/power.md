---
title: 'Xamarin.Essentials: 電源エネルギー省の状態'
description: 電源クラスは、デバイスが省電力モードで動作しているかを判断するエネルギーを節約の状態を取得するプログラムを使用できます。
ms.assetid: C176D177-8B77-4A9C-9F3B-27852A8DCD5F
author: charlespetzold
ms.author: chape
ms.date: 06/27/2018
ms.openlocfilehash: 6d8ccb5be69eb1ea7ea63d3f5c373d9284089679
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831521"
---
# <a name="xamarinessentials-power-energy-saver-status"></a>Xamarin.Essentials: 電源エネルギー省の状態

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**Power**クラスを示す、デバイスが省電力モードで実行されているかどうか、デバイスの電力を節約状態に関する情報を提供します。 デバイスの電力を節約の状態がある場合、アプリケーションはバック グラウンド処理を避ける必要があります。

## <a name="background"></a>背景

バッテリで実行するデバイスは、スクリーン セーバーのエネルギー省電力モードに配置できます。 場合がありますデバイスはモードに切り替えるこの自動的には、たとえば、バッテリ容量が 20% を下回ると。 オペレーティング システムは、バッテリが消費されます傾向があるアクティビティを減らすことで、エネルギー節約機能モードに応答します。 エネルギー省のモードがオンの場合は、バック グラウンド処理または変量の他のアクティビティを回避することでアプリケーションに役立ちます。

Android デバイスで、 **Power**クラスは、以降の Android バージョン 5.0 (Lollipop) にのみ意味のある情報を返します。

## <a name="using-the-power-class"></a>電源クラスを使用します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

静的なを使用してデバイスの現在のエネルギーを節約状態を取得`Power.EnergySaverStatus`プロパティ。

```csharp
// Get energy saver status
var status = Power.EnergySaverStatus;
```

このプロパティのメンバーを返します、`EnergySaverStatus`列挙型であるか、 `On`、 `Off`、または`Unknown`します。 プロパティを返す場合`On`アプリケーションは、バック グラウンド処理または大量の電力を消費する可能性がある他のアクティビティを避ける必要があります。

アプリケーションは、イベント ハンドラーをインストールする必要があります。 **Power**クラスは、エネルギーを節約の状態が変更されたときにトリガーされるイベントを公開します。

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

エネルギー省の状態に変わる場合`On`アプリケーションがバック グラウンド処理の実行を停止する必要があります。 状態に変わる場合`Unknown`または`Off`アプリケーションがバック グラウンド処理を再開できます。

## <a name="api"></a>API

- [電源のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Power)
- [Power API ドキュメント](xref:Xamarin.Essentials.Power)
