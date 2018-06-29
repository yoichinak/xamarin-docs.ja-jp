---
title: 'Xamarin.Essentials: MainThread'
description: MainThread クラスは、メインの実行スレッドでコードを実行するアプリケーションを使用できます。
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E1
author: charlespetzold
ms.author: chape
ms.date: 06/26/2018
ms.openlocfilehash: e07d36d3e9a5492e6e170b62dbacb36be44dbfa9
ms.sourcegitcommit: 72450a6a29599fa133ff4f16fb0b1f443d89f9dc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37080484"
---
# <a name="xamarinessentials-mainthread"></a>Xamarin.Essentials: MainThread

![プレリリース NuGet](~/media/shared/pre-release.png)

**MainThread**クラスにより、アプリケーションの実行のメイン スレッドでコードを実行して場合、特定のコード ブロックを決定するのには、メイン スレッドで現在実行中です。

## <a name="background"></a>背景

ほとんどのオペレーティング システム-iOS、Android、およびユニバーサル Windows プラットフォームを含む: ユーザー インターフェイスに関連するコードにシングル スレッド処理モデルを使用します。 このモデルは、正しくキーストロークを含む、ユーザー インターフェイスのイベントをシリアル化され、タッチ入力する必要があります。 このスレッドが多くの場合と呼ばれる、_メイン スレッド_または_ユーザー インターフェイス スレッド_または_UI スレッド_です。 このモデルの短所には、アプリケーションのメイン スレッドでのユーザー インターフェイス要素にアクセスするすべてのコードを実行する必要があります。 

アプリケーションも、実行のセカンダリ スレッドでイベント ハンドラーを呼び出してイベントを使用する必要があります。 (Xamarin.Essentials クラス[ `Accelerometer` ](accelerometer.md)、 [ `Compass` ](compass.md)、 [ `Gyroscope` ](gyroscope.md)、 [ `Magnetometer` ](magnetometer.md)、および[ `OrientationSensor` ](orientation-sensor.md)より高速で使用する場合、セカンダリ スレッドの情報を返す可能性があります)。イベント ハンドラーは、ユーザー インターフェイス要素にアクセスする必要がある場合、は、そのコードをメイン スレッドで実行があります。 **MainThread**クラスにより、アプリケーションは、メイン スレッドでこのコードを実行します。

## <a name="running-code-on-the-main-thread"></a>メイン スレッドでコードを実行します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

コードを実行するメイン スレッドで、呼び出す静的`MainThread.BeginInvokeOnMainThread`メソッドです。 引数が、 [ `Action` ](xref:System.Action)引数と戻り値のないメソッドだけであるオブジェクト。

```csharp
MainThread.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

メイン スレッドで実行する必要がありますのあるコードの別のメソッドを定義することも。

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

参照することによって、メイン スレッドでこのメソッドを実行することができますし、`BeginInvokeOnMainThread`メソッド。

```csharp
MainThread.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms は、呼び出されるメソッドを持つ[ `Device.BeginInvokeOnMainThread(Action)` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread)と同じ動作を行うこと`MainThread.BeginInvokeOnMainThread(Action)`です。 Xamarin.Forms アプリでは、いずれかの方法を使用できますが、呼び出し元のコードが、Xamarin.Forms で依存関係の他の必要性を持つかどうかを検討してください。 ない場合は、`MainThread.BeginInvokeOnMainThread(Action)`より良いオプションは、可能性があります。

## <a name="determining-if-code-is-running-on-the-main-thread"></a>コードがメイン スレッドで実行されているかどうかを決定します。

`MainThread`クラスでは、アプリケーションで特定のコード ブロックが、メイン スレッドで実行されているかどうかを決定することもできます。 `IsMainThread`プロパティから返される`true`プロパティを呼び出すコードがメイン スレッドで実行されている場合。 プログラムは、このプロパティを使用して、メイン スレッド、またはセカンダリ スレッドのさまざまなコードを実行することができます。

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

かどうかは、コードを呼び出す前にセカンダリ スレッドで実行されるかどうかを確認する必要がありますをでしょうか`BeginInvokeOnMainThread`、たとえば、次のようにします。

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

コードのブロックが既にメイン スレッドで実行されている場合に、このチェックがパフォーマンスを向上させることが疑われる場合があります。

_ただし、このチェックは必要ではありません。_ プラットフォームの実装の`BeginInvokeOnMainThread`自体を確認して、メイン スレッドで呼び出しが行われたかどうか。 呼び出す場合はごくわずかなパフォーマンスの低下がある`BeginInvokeOnMainThread`と本当に必要はありません。

## <a name="api"></a>API

- [MainThread ソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/MainThread)
- [MainThread API ドキュメント](xref:Xamarin.Essentials.MainThread)
