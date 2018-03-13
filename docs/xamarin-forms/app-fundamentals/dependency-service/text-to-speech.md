---
title: "音声合成の実装"
description: "DependencyService を使用して、各プラットフォームのネイティブの音声合成 API を呼び出せる"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: 6ac9ca7bae517602c33729134eb0bd48359afbc7
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="implementing-text-to-speech"></a>音声合成の実装

この記事の内容は説明を使用するクロスプラット フォーム アプリを作成すると[ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/)音声合成のネイティブの Api にアクセスします。

- **[インターフェイスを作成する](#Creating_the_Interface)** &ndash;共有コードで、インターフェイスを作成する方法を理解します。
- **[iOS 実装](#iOS_Implementation)** &ndash; iOS 用のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[Android 実装](#Android_Implementation)** &ndash; for Android のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[Windows 実装](#WindowsImplementation)** &ndash; Windows Phone とユニバーサル Windows プラットフォーム (UWP) のネイティブ コードにインターフェイスを実装する方法について説明します。
- **[共有コードで実装する](#Implementing_in_Shared_Code)** &ndash;を使用する方法を学習`DependencyService`に共有コードからネイティブの実装を呼び出します。

アプリケーションを使用して、`DependencyService`次のような構造になります。

![](text-to-speech-images/tts-diagram.png "DependencyService アプリケーション構造")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>インターフェイスの作成

最初に、共有コードを実装する予定の機能を表すインターフェイスを作成します。 この例で、インターフェイスには、1 つのメソッドが含まれます`Speak`:

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

共有コードでは、このインターフェイスに対するコーディングすると、各プラットフォームでの音声 Api にアクセスする Xamarin.Forms アプリが許可されます。

> [!NOTE]
> インターフェイスを実装するクラスを使用するパラメーターなしのコンス トラクターを持つ必要があります、`DependencyService`です。

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS の実装

各プラットフォームに固有のアプリケーション プロジェクトでこのインターフェイスを実装する必要があります。 クラスが、パラメーターなしのコンス トラクターを持つことに注意してくださいできるように、`DependencyService`新しいインスタンスを作成できます。

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

Android のコードは、iOS のバージョンよりも複雑: 実装するクラスを Android 固有から継承する必要があります`Java.Lang.Object`を実装して、`IOnInitListener`インターフェイスもします。 現在のコンテキストによって公開されている Android へのアクセスも必要です、`MainActivity.Instance`プロパティです。

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

## <a name="windows-phone-and-universal-windows-platform-implementation"></a>Windows Phone とユニバーサル Windows プラットフォームの実装

Windows Phone とユニバーサル Windows プラットフォームで音声認識 API がある、`Windows.Media.SpeechSynthesis`名前空間。 唯一の注意点は、目盛りに注意してください、**マイク**マニフェストで機能は、それ以外の場合 Api がブロックされている音声にアクセスします。

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

今すぐおを記述し、音声合成のインターフェイスにアクセスする共有コードのテストします。 この単純なページには、音声認識機能をトリガーするボタンが含まれています。 使用して、`DependencyService`のインスタンスを取得する、`ITextToSpeech`インターフェイス&ndash;実行時にこのインスタンスになりますが、ネイティブの SDK へのフル アクセス プラットフォームに固有の実装です。

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

IOS、Android、または Windows プラットフォームおよびキーを押して、ボタンが応対する各プラットフォームでネイティブ speech SDK を使用するアプリケーションで発生でこのアプリケーションを実行します。

 ![iOS と Android の音声合成ボタン](text-to-speech-images/running.png "テキスト読み上げサンプル")


## <a name="related-links"></a>関連リンク

- [DependencyService (サンプル) を使用します。](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)
- [Text to Speech ブック](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/text-to-speech/text-to-speech.workbook)
