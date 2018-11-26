---
ms.topic: include
ms.openlocfilehash: e4dfd1ac12f3010939d483381a785091d71599ed
ms.sourcegitcommit: 676c5a6795ab4896ccd1b288424bf2040b1208aa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2018
ms.locfileid: "52294817"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[センサーの速度](xref:Xamarin.Essentials.SensorSpeed)

- **Fastest** – センサー データを最高速度で取得します (UI スレッドに返ることが保証されません)。
- **Game** – ゲームに適した速度です (UI スレッドに返ることが保証されません)。
- **Normal** – 画面の向きの変更に適した既定の速度です。
- **UI** – 一般的なユーザー インターフェイスに適した速度です。

イベント ハンドラーが UI スレッドでの実行を保証されておらず、そのイベント ハンドラーでユーザー インターフェイス要素にアクセスする必要がある場合は、[`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) メソッドを使用してそのコードを UI スレッドで実行します。