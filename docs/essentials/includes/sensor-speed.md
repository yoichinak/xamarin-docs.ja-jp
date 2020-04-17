---
ms.topic: include
ms.openlocfilehash: b82ebeaeb28195e3ec0f5a0200c0448b6b76196a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "61259477"
---
## <a name="sensor-speed"></a>[センサーの速度](xref:Xamarin.Essentials.SensorSpeed)

- **Fastest** – センサー データを最高速度で取得します (UI スレッドに返ることが保証されません)。
- **Game** – ゲームに適した速度です (UI スレッドに返ることが保証されません)。
- **Default** – 画面の向きの変更に適した既定の速度です。
- **UI** – 一般的なユーザー インターフェイスに適した速度です。

イベント ハンドラーが UI スレッドでの実行を保証されておらず、そのイベント ハンドラーでユーザー インターフェイス要素にアクセスする必要がある場合は、[`MainThread.BeginInvokeOnMainThread`](~/essentials/main-thread.md) メソッドを使用してそのコードを UI スレッドで実行します。
