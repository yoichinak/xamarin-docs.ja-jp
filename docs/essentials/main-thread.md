---
title: Xamarin.Essentials:MainThread
description: MainThread クラスを使用すると、アプリケーションでコードをメインの実行スレッドで実行させることができます。
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E1
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 08/20/2019
ms.openlocfilehash: dfef9fc5d1b8e4acaec8d9e1d653b141b04cead6
ms.sourcegitcommit: 83cf2a4d99546751c6394510a463a2b2a8bf75b8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/13/2020
ms.locfileid: "83150057"
---
# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials:MainThread

**MainThread** クラスを使用すると、アプリケーションでコードを実行のメイン スレッドで実行させたり、コードの特定のブロックがメイン スレッドで現在実行中であるかどうかを調べたりすることができます。

## <a name="background"></a>背景

ほとんどのオペレーティング システム — iOS、Android、ユニバーサル Windows プラットフォームなど — では、ユーザー インターフェイスに関連するコードのためにシングル スレッド モデルが使用されます。 このモデルは、ユーザー インターフェイスのイベント (キーストローク、タッチ入力など) をシリアル化するために必要です。 多くの場合、このスレッドは_メイン スレッド_、_ユーザー インターフェイス スレッド_、または _UI スレッド_と呼ばれます。 このモデルの欠点は、ユーザー インターフェイス要素にアクセスするすべてのコードを、アプリケーションのメイン スレッドで実行させなければならないという点です。 

アプリケーションでは、イベント ハンドラーを実行のセカンダリ スレッドで呼び出すイベントを使用することが必要な場合があります。 (Xamarin.Essentials のクラス [`Accelerometer`](accelerometer.md)、[`Compass`](compass.md)、[`Gyroscope`](gyroscope.md)、[`Magnetometer`](magnetometer.md)、[`OrientationSensor`](orientation-sensor.md) はすべて、高速で使用すると、セカンダリ スレッドに関する情報を返す可能性があります。)イベント ハンドラーがユーザー インターフェイス要素にアクセスする必要がある場合は、そのコードをメイン スレッドで実行させる必要があります。 **MainThread** クラスを使用すると、アプリケーションでこのコードをメイン スレッドで実行させることができます。

## <a name="get-started"></a>作業開始

[!include[](~/essentials/includes/get-started.md)]

## <a name="running-code-on-the-main-thread"></a>メイン スレッドでのコードの実行

自分のクラスに Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

メイン スレッドでコードを実行するには、静的メソッド `MainThread.BeginInvokeOnMainThread` を呼び出します。 引数は [`Action`](xref:System.Action) オブジェクトです。これは単に、引数も戻り値も持たないメソッドです。

```csharp
MainThread.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

また、メイン スレッドで実行させる必要があるコード用に、個別のメソッドを定義することもできます。

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

こうすると、`BeginInvokeOnMainThread` メソッド内でこのメソッドを参照することで、これをメイン スレッドで実行させることができます。

```csharp
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms には [`Device.BeginInvokeOnMainThread(Action)`](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread) と呼ばれるメソッドがあります。
> これは `MainThread.BeginInvokeOnMainThread(Action)` と同じ処理を行います。 Xamarin.Forms アプリではどちらのメソッドも使用できますが、呼び出し元のコードが他にも Xamarin.Forms に依存する必要があるかどうかを検討してください。 ない場合は、`MainThread.BeginInvokeOnMainThread(Action)` が適切な選択であると考えられます。

## <a name="determining-if-code-is-running-on-the-main-thread"></a>コードがメイン スレッドで実行されているかどうかの判断

`MainThread` クラスを使用すると、アプリケーションで、コードの特定のブロックがメイン スレッドで実行中であるかどうかを調べることもできます。 `IsMainThread` プロパティを呼び出すコードがメイン スレッドで実行されている場合、プロパティは `true` を返します。 プログラムでこのプロパティを使用して、メイン スレッド用またはセカンダリ スレッド用に別のコードを実行させることができます。

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

`BeginInvokeOnMainThread` を呼び出す前に、コードがセカンダリ スレッドで実行されているかどうかをチェックする必要があるか疑問に思うかもしれません。たとえば、次のような場合です。

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

コードのブロックが既にメイン スレッドで実行されている場合、このチェックによってパフォーマンスが向上するのではないかと思うかもしれません。

_しかし、このチェックは必要ではありません。_ プラットフォームによる `BeginInvokeOnMainThread` の実装自体によって、呼び出しがメイン スレッドで行われたかどうかが確認されます。 不要な `BeginInvokeOnMainThread` を呼び出す場合のパフォーマンスの低下はごくわずかです。

## <a name="additional-methods"></a>他の方法

`MainThread` クラスには、他にも次に示す `static` メソッドが含まれています。これらのメソッドを使用すれば、バックグラウンド スレッドからユーザー インターフェイス要素とやりとりすることができます。

| メソッド | 引数 | 戻り値 | 目的 |
|---|---|---|---|
| `InvokeOnMainThreadAsync<T>` | `Func<T>` | `Task<T>` | メイン スレッド上で `Func<T>` を呼び出し、それが完了するまで待機します。 |
| `InvokeOnMainThreadAsync` | `Action` | `Task` | メイン スレッド上で `Action` を呼び出し、それが完了するまで待機します。 |
| `InvokeOnMainThreadAsync<T>`| `Func<Task<T>>` | `Task<T>` | メイン スレッド上で `Func<Task<T>>` を呼び出し、それが完了するまで待機します。 |
| `InvokeOnMainThreadAsync` | `Func<Task>` | `Task` | メイン スレッド上で `Func<Task>` を呼び出し、それが完了するまで待機します。 |
| `GetMainThreadSynchronizationContextAsync` | | `Task<SynchronizationContext>` | メイン スレッドの `SynchronizationContext` を返します。 |

## <a name="api"></a>API

- [MainThread のソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [MainThread API ドキュメント](xref:Xamarin.Essentials.MainThread)

## <a name="related-video"></a>関連ビデオ

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Main-Thread-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
