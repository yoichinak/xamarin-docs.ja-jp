---
title: 音声合成の実装
description: この記事では、各プラットフォームのネイティブの音声合成 API を呼び出して Xamarin.Forms DependencyService クラスを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: 6d1948214b97a1b536b07b6420c32e4d27124518
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2018
ms.locfileid: "38997544"
---
# <a name="implementing-text-to-speech"></a>音声合成の実装

この記事で使用するクロス プラットフォーム アプリを作成するようにガイドするには[ `DependencyService` ](xref:Xamarin.Forms.DependencyService)ネイティブの音声合成 Api にアクセスします。

- **[インターフェイスを作成する](#Creating_the_Interface)** &ndash;共有コードで、インターフェイスを作成する方法を理解します。
- **[iOS 実装](#iOS_Implementation)** &ndash; iOS のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[Android の実装](#Android_Implementation)** &ndash; Android のネイティブ コードでインターフェイスを実装する方法について説明します。
- **[UWP 実装](#WindowsImplementation)** &ndash;ユニバーサル Windows プラットフォーム (UWP) のネイティブ コードでインターフェイスを実装する方法について説明します。
- **[共有コードで実装する](#Implementing_in_Shared_Code)** &ndash;を使用する方法について説明します`DependencyService`共有コードからネイティブの実装を呼び出す。

アプリケーションを使用して、`DependencyService`次の構造になります。

![](text-to-speech-images/tts-diagram.png "DependencyService アプリケーション構造")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>インターフェイスの作成

最初に、共有コードを実装する予定の機能を表すインターフェイスを作成します。 この例では、インターフェイスには 1 つのメソッド、 `Speak`:

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

共有コードでは、このインターフェイスに対するコーディングで、各プラットフォームで音声認識 Api にアクセスする Xamarin.Forms アプリを許可します。

> [!NOTE]
> インターフェイスを実装するクラスには、パラメーターなしのコンス トラクターを使用する必要があります、`DependencyService`します。

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS の実装

各プラットフォームに固有のアプリケーション プロジェクトでインターフェイスを実装する必要があります。 クラスは、パラメーターなしのコンス トラクターを`DependencyService`新しいインスタンスを作成できます。

```csharp
[assembly: Dependency(typeof(TextToSpeechImplementation))]
namespace DependencyServiceSample.iOS
{

    public class TextToSpeechImplementation : ITextToSpeech
    {
        public TextToSpeechImplementation() { }

        public void Speak(string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer();
            var speechUtterance = new AVSpeechUtterance(text)
            {
                Rate = AVSpeechUtterance.MaximumSpeechRate / 4,
                Voice = AVSpeechSynthesisVoice.FromLanguage("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance(speechUtterance);
        }
    }
}
```

`[assembly]`属性の実装としてクラスを登録する、`ITextToSpeech`インターフェイス、つまり`DependencyService.Get<ITextToSpeech>()`そのインスタンスを作成する共有コードで使用できます。

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android の実装

Android のコードは、iOS のバージョンよりも複雑: Android 固有の継承を実装するクラスに要する`Java.Lang.Object`を実装して、`IOnInitListener`インターフェイスも。 現在のコンテキストで公開される Android へのアクセスも必要です、`MainActivity.Instance`プロパティ。

```csharp
[assembly: Dependency(typeof(TextToSpeechImplementation))]
namespace DependencyServiceSample.Droid
{
    public class TextToSpeechImplementation : Java.Lang.Object, ITextToSpeech, TextToSpeech.IOnInitListener
    {
        TextToSpeech speaker;
        string toSpeak;

        public void Speak(string text)
        {
            toSpeak = text;
            if (speaker == null)
            {
                speaker = new TextToSpeech(MainActivity.Instance, this);
            }
            else
            {
                speaker.Speak(toSpeak, QueueMode.Flush, null, null);
            }
        }

        public void OnInit(OperationResult status)
        {
            if (status.Equals(OperationResult.Success))
            {
                speaker.Speak(toSpeak, QueueMode.Flush, null, null);
            }
        }
    }
}
```

`[assembly]`属性の実装としてクラスを登録する、`ITextToSpeech`インターフェイス、つまり`DependencyService.Get<ITextToSpeech>()`そのインスタンスを作成する共有コードで使用できます。

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>ユニバーサル Windows プラットフォームの実装

ユニバーサル Windows プラットフォームがで speech API、`Windows.Media.SpeechSynthesis`名前空間。 唯一の注意事項をオンにしておく、**マイク**Api がブロックされている音声へのアクセスそれ以外の場合をマニフェストで機能します。

```csharp
[assembly:Dependency(typeof(TextToSpeechImplementation))]
public class TextToSpeechImplementation : ITextToSpeech
{
    public async void Speak(string text)
    {
        var mediaElement = new MediaElement();
        var synth = new Windows.Media.SpeechSynthesis.SpeechSynthesizer();
        var stream = await synth.SynthesizeTextToStreamAsync(text);

        mediaElement.SetSource(stream, stream.ContentType);
        mediaElement.Play();
    }
}
```

`[assembly]`属性の実装としてクラスを登録する、`ITextToSpeech`インターフェイス、つまり`DependencyService.Get<ITextToSpeech>()`そのインスタンスを作成する共有コードで使用できます。

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>共有コードで実装します。

ここで記述し、音声合成のインターフェイスにアクセスする共有コードをテストすることができます。 この単純なページには、音声認識機能をトリガーするボタンが含まれています。 使用して、`DependencyService`のインスタンスを取得する、`ITextToSpeech`インターフェイス&ndash;実行時にこのインスタンスは、ネイティブ SDK へのフル アクセスのあるプラットフォーム固有の実装になります。

```csharp
public MainPage ()
{
    var speak = new Button {
        Text = "Hello, Forms !",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    speak.Clicked += (sender, e) => {
        DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
    };
    Content = speak;
}
```

IOS、Android、または UWP でこのアプリケーションを実行し をクリックすると、各プラットフォームでネイティブ speech SDK を使用して自動的に言うと、アプリケーションが作成されます。

 ![iOS と Android の音声合成ボタン](text-to-speech-images/running.png "テキスト読み上げサンプル")


## <a name="related-links"></a>関連リンク

- [DependencyService (サンプル) を使用します。](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)

