---
ms.topic: include
ms.openlocfilehash: 5c11c94956f8d56c66c50a9a480177c5b77c2643
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947435"
---
## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[センサーの速度](xref:Xamarin.Essentials.SensorSpeed)

- **最も高速な**– (UI スレッド上で返されるとは限りません) をできるだけ高速のセンサー データを取得します。
- **ゲーム**– (UI スレッド上で返されるとは限りません) ゲームに適したを評価します。
- **標準**– 画面の向きの変更に適した既定のレート。
- **Ui** – 一般的なユーザー インターフェイスの適切な評価。

UI スレッドで実行し、イベント ハンドラーは、ユーザー インターフェイス要素にアクセスする必要がある場合を使用するかどうか、イベント ハンドラーは保証されません、 [ `MainThread.BeginInvokeOnMainThread` ](~/essentials/main-thread.md)メソッドを UI スレッドでそのコードを実行します。