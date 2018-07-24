---
title: 'Xamarin.Essentials: MainThread'
description: MainThread クラスは、メインの実行スレッドでコードを実行するアプリケーションを使用できます。
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E1
author: charlespetzold
ms.author: chape
ms.date: 06/26/2018
ms.openlocfilehash: e07d36d3e9a5492e6e170b62dbacb36be44dbfa9
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831426"
---
# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials: MainThread

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**MainThread**クラスにより、アプリケーションの実行のメイン スレッドでコードを実行して場合、特定のコード ブロックを決定するのには、メイン スレッドで現在実行中です。

## <a name="background"></a>背景

ほとんどのオペレーティング システム: iOS、Android、およびユニバーサル Windows プラットフォームを含む、ユーザー インターフェイスに関連するコードにシングル スレッド モデルを使用します。 このモデルは、タッチ入力、および正しくキーストロークを含む、ユーザー インターフェイスのイベントをシリアル化する必要があります。 このスレッドが多くの場合と呼ばれる、_メイン スレッド_または_ユーザー インターフェイス スレッド_または_UI スレッド_します。 このモデルの欠点には、アプリケーションのメイン スレッドでのユーザー インターフェイス要素にアクセスするすべてのコードを実行する必要があります。 

アプリケーションは、実行のセカンダリ スレッドでイベント ハンドラーを呼び出してイベントを使用する必要があります。 (Xamarin.Essentials クラス[ `Accelerometer` ](accelerometer.md)、 [ `Compass` ](compass.md)、 [ `Gyroscope` ](gyroscope.md)、 [ `Magnetometer`](magnetometer.md)と[ `OrientationSensor` ](orientation-sensor.md)高速で使用する場合、セカンダリ スレッド情報を返す可能性があります)。をイベント ハンドラーがユーザー インターフェイス要素にアクセスする必要がある場合は、そのコードをメイン スレッドで実行があります。 **MainThread**クラスは、メイン スレッドでこのコードを実行するアプリケーションを使用できます。

## <a name="running-code-on-the-main-thread"></a>メイン スレッドでコードを実行します。

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

静的なを呼び出すコードをメイン スレッドで実行する`MainThread.BeginInvokeOnMainThread`メソッド。 引数が、 [ `Action` ](xref:System.Action)引数と戻り値のないメソッドだけであるオブジェクト。

```csharp
MainThread.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

メイン スレッドで実行する必要があるコードの別のメソッドを定義することです。

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

参照することで、メイン スレッドでこのメソッドを実行することができますし、`BeginInvokeOnMainThread`メソッド。

```csharp
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms が呼び出されるメソッド[ `Device.BeginInvokeOnMainThread(Action)` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread)と同じ処理を行うこと`MainThread.BeginInvokeOnMainThread(Action)`します。 いずれかの方法を使用するには、Xamarin.Forms アプリで、呼び出し元のコードが Xamarin.Forms の依存関係の他の必要性を持っているかどうかを検討してください。 ない場合は、`MainThread.BeginInvokeOnMainThread(Action)`より適切なオプションは、可能性があります。

## <a name="determining-if-code-is-running-on-the-main-thread"></a>コードがメイン スレッドで実行されているかどうかを決定します。

`MainThread`クラスは、特定のコード ブロックがメイン スレッドで実行されているかどうかを判断するアプリケーションも使用できます。 `IsMainThread`プロパティが返す`true`プロパティを呼び出すコードがメイン スレッドで実行されている場合。 プログラムは、このプロパティを使用して、メイン スレッド、またはセカンダリ スレッドのさまざまなコードを実行することができます。

```csharp
if (MainThread.IsMainThread)
{
    // Code to run if this is the main thread
}
else
{
    // Code to run if this is a secondary thread
}
```

かどうかは、呼び出す前にセカンダリ スレッドでコードが実行されているかどうかをチェックする必要があります疑問に思うかもしれません`BeginInvokeOnMainThread`、たとえば、次のようにします。

```csharp
if (MainThread.IsMainThread)
{
    MyMainThreadCode();
}
else
{
    MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
}
```

コードのブロックが既にメイン スレッドで実行されている場合に、このチェックがパフォーマンスを向上可能性がありますが疑われる場合があります。

_ただし、このチェックは必要はありません。_ プラットフォームの実装の`BeginInvokeOnMainThread`メイン スレッドで呼び出しが行われたかどうか自体を確認します。 呼び出す場合はごくわずかなパフォーマンスの低下`BeginInvokeOnMainThread`と本当に必要はありません。

## <a name="api"></a>API

- [MainThread ソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [MainThread API ドキュメント](xref:Xamarin.Essentials.MainThread)
