---
title: Xamarin.Essentials プラットフォーム
description: プラットフォームのクラスは、メインの実行スレッドでコードを実行するアプリケーションを使用できます。
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E0
author: charlespetzold
ms.author: chape
ms.date: 06/03/2018
ms.openlocfilehash: 82e248ee702e104dff98b342aec72179273fc34f
ms.sourcegitcommit: d9ecac62bcf743aff52408fbbd09c5509921a000
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/23/2018
ms.locfileid: "36329949"
---
# <a name="xamarinessentials-platform"></a>Xamarin.Essentials プラットフォーム

![プレリリース NuGet](~/media/shared/pre-release.png)

**プラットフォーム**クラスにより、アプリケーションの実行のメイン スレッドでコードを実行して場合、特定のコード ブロックを決定するのには、メイン スレッドで現在実行中です。

## <a name="background"></a>背景

ほとんどのオペレーティング システム-iOS、Android、およびユニバーサル Windows プラットフォームを含む: ユーザー インターフェイスに関連するコードにシングル スレッド処理モデルを使用します。 このモデルは、正しくキーストロークを含む、ユーザー インターフェイスのイベントをシリアル化され、タッチ入力する必要があります。 このスレッドが多くの場合と呼ばれる、_メイン スレッド_または_ユーザー インターフェイス スレッド_または_UI スレッド_です。 このモデルの短所には、アプリケーションのメイン スレッドでのユーザー インターフェイス要素にアクセスするすべてのコードを実行する必要があります。 

アプリケーションも、実行のセカンダリ スレッドでイベント ハンドラーを呼び出してイベントを使用する必要があります。 (Xamarin.Essentials クラス[ `Accelerometer` ](accelerometer.md)、 [ `Compass` ](compass.md)、 [ `Gyroscope` ](gyroscope.md)、 [ `Magnetometer` ](magnetometer.md)、および[ `OrientationSensor` ](orientation-sensor.md)より高速で使用する場合、セカンダリ スレッドの情報を返す可能性があります)。イベント ハンドラーは、ユーザー インターフェイス要素にアクセスする必要がある場合、は、そのコードをメイン スレッドで実行があります。 **プラットフォーム**クラスにより、アプリケーションは、メイン スレッドでこのコードを実行します。

## <a name="running-code-on-the-main-thread"></a>メイン スレッドでコードを実行します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

コードを実行するメイン スレッドで、呼び出す静的`Platform.BeginInvokeOnMainThread`メソッドです。 引数が、 [ `Action` ](xref:System.Action)引数と戻り値のないメソッドだけであるオブジェクト。

```csharp
Platform.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

派生したクラス内の Xamarin.Forms アプリケーションでこのメソッドを呼び出す必要がある場合`Element`(から派生したクラスが含まれます`View`または`Page`) では、`Platform`クラス名を使用して完全修飾する必要があります、名前空間の名前。

```csharp
Xamarin.Essentials.Platform.BeginInvokeOnMainThread(() =>
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
Xamarin.Essentials.Platform.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms は、呼び出されるメソッドを持つ[ `Device.BeginInvokeOnMainThread(Action)` ](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread)と同じ動作を行うこと`Xamarin.Essentials.Platform.BeginInvokeOnMainThread(Action)`です。 Xamarin.Forms アプリでは、いずれかの方法を使用できますが、呼び出し元のコードが、Xamarin.Forms で依存関係の他の必要性を持つかどうかを検討してください。 ない場合は、`Xamarin.Essentials.Platform.BeginInvokeOnMainThread(Action)`より良いオプションは、可能性があります。

## <a name="determining-if-code-is-running-on-the-main-thread"></a>コードがメイン スレッドで実行されているかどうかを決定します。

`Platform`クラスでは、アプリケーションで特定のコード ブロックが、メイン スレッドで実行されているかどうかを決定することもできます。 `IsMainThread`プロパティから返される`true`プロパティを呼び出すコードがメイン スレッドで実行されている場合。 プログラムは、このプロパティを使用して、メイン スレッド、またはセカンダリ スレッドのさまざまなコードを実行することができます。

```csharp
if (Xamarin.Essentials.Platform.IsMainThread)
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
if (Xamarin.Essentials.Platform.IsMainThread)
{
    MyMainThreadCode();
}
else
{
    Xamarin.Essentials.Platform.BeginInvokeOnMainThread(MyMainThreadCode);
}
```

コードのブロックが既にメイン スレッドで実行されている場合に、このチェックがパフォーマンスを向上させることが疑われる場合があります。

_ただし、このチェックは必要ではありません。_ プラットフォームの実装の`BeginInvokeOnMainThread`自体を確認して、メイン スレッドで呼び出しが行われたかどうか。 呼び出す場合はごくわずかなパフォーマンスの低下がある`BeginInvokeOnMainThread`と本当に必要はありません。

## <a name="api"></a>API

- [プラットフォームのソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Platform)
- [プラットフォーム API のドキュメント](xref:Xamarin.Essentials.Platform)
