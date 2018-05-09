---
title: Xamarin.Essentials 音声合成
description: TextToSpeech クラスを使用するデバイスと、エンジンでサポートできるクエリの使用可能な言語もバックのテキストを読み上げるために音声合成エンジンで組み込み、アプリケーションを使用します。
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: e5ee5a324c6e753d389f7e80df106dbf4af1a8ca
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-text-to-speech"></a>Xamarin.Essentials 音声合成

![プレリリース NuGet](~/media/shared/pre-release.png)

**TextToSpeech**デバイスと、エンジンでサポートできるクエリの使用可能な言語もバックのテキストを読み上げるために音声合成エンジンで組み込みクラスを使用するアプリケーションを使用します。

## <a name="using-text-to-speech"></a>音声合成を使用します。

クラスの Xamarin.Essentials への参照を追加します。

```csharp
using Xamarin.Essentials;
```

音声合成機能が呼び出すことによって、`SpeakAsync`テキストと省略可能なパラメーターを返します、発話が完了した後を持つメソッドです。 

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

このメソッドは、いずれかの開始発話を停止する、省略可能な CancellationToken で受け取ります。 
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

音声合成では、同じスレッドからの音声の要求を自動的にキューがします。 

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

オーディオが話される方法をより細かく制御でバックアップ`SpeakSettings`ボリューム、ピッチ、およびロケールを設定することができます。

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

これらのパラメーターのサポートされている値を次に示します。

| パラメーター | 最小要件 | 最大 |
| --- | :---: | :---: |
| 声の高さ | 0 | 2.0 |
| ボリューム | 0 | 1 |

### <a name="speech-locales"></a>音声認識のロケール

各プラットフォームでは、複数の言語とアクセントのバックのテキストを読み上げるためにロケールを提供します。 各プラットフォームには、さまざまなコードとは、これを指定する方法 Essentials は、クロスプラット フォーム`Locale`クラスとそれらのクエリを実行する方法`GetLocalesAsync`です。

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

- 発話キューは、複数のスレッドで呼び出された場合に保証されません。
- バック グラウンド オーディオ再生が正式にサポートされていません。

## <a name="api"></a>API

- [TextToSpeech ソース コード](https://github.com/xamarin/Essentials/tree/master/Essentials/TextToSpeech)
- [TextToSpeech API ドキュメント](xref:Xamarin.Essentials.TextToSpeech)
