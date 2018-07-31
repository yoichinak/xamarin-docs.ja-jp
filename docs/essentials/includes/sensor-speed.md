---
ms.topic: include
ms.openlocfilehash: e4dfd1ac12f3010939d483381a785091d71599ed
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353270"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[センサーの速度](xref:Xamarin.Essentials.SensorSpeed)

- **最も高速な**– (UI スレッド上で返されるとは限りません) をできるだけ高速のセンサー データを取得します。
- **ゲーム**– (UI スレッド上で返されるとは限りません) ゲームに適したを評価します。
- **標準**– 画面の向きの変更に適した既定のレート。
- **UI** – 一般的なユーザー インターフェイスの適切な評価。

UI スレッドで実行し、イベント ハンドラーは、ユーザー インターフェイス要素にアクセスする必要がある場合を使用するかどうか、イベント ハンドラーは保証されません、 [ `MainThread.BeginInvokeOnMainThread` ](~/essentials/main-thread.md)メソッドを UI スレッドでそのコードを実行します。