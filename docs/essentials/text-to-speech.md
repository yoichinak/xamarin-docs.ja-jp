---
title: 'Xamarin.Essentials: 音声合成'
description: アプリケーションをデバイスからと、エンジンがサポートできる使用可能な言語のクエリにも、バックのテキストを読み上げる音声合成エンジンで、組み込み利用 Xamarin.Essentials で TextToSpeech クラスです。
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: ba822870edafce44140caa66b01f4da242fb7779
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353614"
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials: 音声合成

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

**TextToSpeech**クラスは、デバイスからと、エンジンがサポートできる使用可能な言語のクエリにも、バックのテキストを読み上げる音声合成エンジンで、ビルドを利用するアプリケーションを使用できます。

## <a name="using-text-to-speech"></a>音声合成を使用

クラスで Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

音声合成機能が呼び出すことによって、`SpeakAsync`メソッドでは、テキストと省略可能なパラメーター、発話が完了した後に返します。 

```csharp
public async Task SpeakNowDefaultSettings()
{
    await TextToSpeech.SpeakAsync("Hello World");

    // This method will block until utterance finishes.
}

public void SpeakNowDefaultSettings2()
{
    TextToSpeech.SpeakAsync("Hello World").ContinueWith((t) =>
    {
        // Logic that will run after utterance finishes.

    }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

このメソッドは、省略可能な`CancellationToken`を起動した場合、発話を停止します。

```csharp
CancellationTokenSource cts;
public async Task SpeakNowDefaultSettings()
{
    cts = new CancellationTokenSource();
    await TextToSpeech.SpeakAsync("Hello World", cancelToken: cts.Token);

    // This method will block until utterance finishes.
}

public void CancelSpeech()
{
    if (cts?.IsCancellationRequested ?? false)
        return;

    cts.Cancel();
}
```

音声合成では、同じスレッドからの音声要求を自動的にキューされます。

```csharp
bool isBusy = false;
public void SpeakMultiple()
{
    isBusy = true;
    Task.Run(async () =>
    {
        await TextToSpeech.SpeakAsync("Hello World 1");
        await TextToSpeech.SpeakAsync("Hello World 2");
        await TextToSpeech.SpeakAsync("Hello World 3");
        isBusy = false;
    });

    // or you can query multiple without a Task:
    Task.WhenAll(
        TextToSpeech.SpeakAsync("Hello World 1"),
        TextToSpeech.SpeakAsync("Hello World 2"),
        TextToSpeech.SpeakAsync("Hello World 3"))
        .ContinueWith((t) => { isBusy = false; }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

### <a name="speech-settings"></a>音声認識の設定

オーディオがどのように話されているかをより細かく制御して、バックアップ`SpeakSettings`ボリューム、ピッチ、およびロケールを設定することができます。

```csharp
public async Task SpeakNow()
{
    var settings = new SpeakSettings()
        {
            Volume = .75,
            Pitch = 1.0
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

次に、これらのパラメーターのサポートされている値を示します。

| パラメーター | 最小要件 | 最大 |
| --- | :---: | :---: |
| ピッチ | 0 | 2.0 |
| ボリューム | 0 | 1 |

### <a name="speech-locales"></a>音声のロケール

各プラットフォームでは、複数の言語とアクセントのバックのテキストを読み上げるロケールを提供します。 各プラットフォームにさまざまなコードとは、これを指定する方法は Essentials は、クロス プラットフォームを提供します。`Locale`クラスと、使用してクエリを実行する方法`GetLocalesAsync`します。

```csharp
public async Task SpeakNow()
{
    var locales = await TextToSpeech.GetLocalesAsync();

    // Grab the first locale
    var locale = locales.FirstOrDefault();

    var settings = new SpeakSettings()
        {
            Volume = .75,
            Pitch = 1.0,
            Locale = locale
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

## <a name="limitations"></a>制限事項

- 複数のスレッドで呼び出された場合は、発話のキューは保証されません。
- バック グラウンド オーディオ再生は公式にサポートされていません。

## <a name="api"></a>API

- [TextToSpeech ソース コード](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/TextToSpeech)
- [TextToSpeech API ドキュメント](xref:Xamarin.Essentials.TextToSpeech)
