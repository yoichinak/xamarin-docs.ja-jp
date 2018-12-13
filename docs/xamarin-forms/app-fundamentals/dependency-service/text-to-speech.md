---
title: 音声合成の実装
description: この記事では、Xamarin.Forms の DependencyService クラスを使用して、各プラットフォームのネイティブな Text to Speech API の呼び出しを行う方法について説明します。
ms.prod: xamarin
ms.assetid: 1D6164F9-4ECE-43A6-B583-1F5D5EFC1DDF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/18/2017
ms.openlocfilehash: 6d1948214b97a1b536b07b6420c32e4d27124518
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2018
ms.locfileid: "38997544"
---
# <a name="implementing-text-to-speech"></a>音声合成の実装

この記事では、ネイティブな Text to Speech API にアクセスするために [`DependencyService`](xref:Xamarin.Forms.DependencyService) を使用するクロスプラットフォーム アプリを作成する方法を示します。

- **[インターフェイスの作成](#Creating_the_Interface)** &ndash; 共有コード内にインターフェイスを作成する方法を理解します。
- **[iOS での実装](#iOS_Implementation)** &ndash; iOS 用のネイティブ コード内にインターフェイスを実装する方法を学習します。
- **[Android での実装](#Android_Implementation)** &ndash; Android 用のネイティブ コードでインターフェイスを実装する方法を学習します。
- **[UWP での実装](#WindowsImplementation)** &ndash; ユニバーサル Windows プラットフォーム (UWP) 用のネイティブ コードでインターフェイスを実装する方法を学習します。
- **[共有コードでの実装](#Implementing_in_Shared_Code)** &ndash; `DependencyService` を使用して共有コードからネイティブ実装を呼び出す方法を学習します。

`DependencyService` を使用するアプリケーションは次のような構造になります。

![](text-to-speech-images/tts-diagram.png "DependencyService アプリケーションの構造")

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>インターフェイスの作成

最初に、実装する機能を表すインターフェイスを共有コードで作成します。 この例では、インターフェイスには単一のメソッド `Speak` が含まれます。

```csharp
public interface ITextToSpeech
{
    void Speak (string text);
}
```

共有コード内でこのインターフェイスに対してコーディングすると、Xamarin.Forms アプリから各プラットフォーム上の Speech API にアクセスできます。

> [!NOTE]
> インターフェイスを実装するクラスで `DependencyService` を使用するには、パラメーターのないコンストラクターが必要です。

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>iOS での実装

各プラットフォームに固有のアプリケーション プロジェクト内に、インターフェイスを実装する必要があります。 `DependencyService` で新しいインスタンスを作成できるよう、クラスにパラメーターなしのコンストラクターがあることに注意してください。

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

`[assembly]` 属性では、クラスが `ITextToSpeech` インターフェイスの実装として登録されます。つまり、共有コードで `DependencyService.Get<ITextToSpeech>()` を使用してそのインスタンスを作成できます。

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Android での実装

Android のコードは、iOS バージョンよりも複雑です。Android 固有の `Java.Lang.Object` から継承すると共に、`IOnInitListener` インターフェイスを実装するために、クラスを実装する必要があります。 また、`MainActivity.Instance` プロパティによって公開される、現在の Android コンテキストへのアクセスも必要になります。

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

`[assembly]` 属性は、クラスを `ITextToSpeech` インターフェイスの実装として登録します。つまり、共有コードで `DependencyService.Get<ITextToSpeech>()` を使用してそのインスタンスを作成できます。

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>ユニバーサル Windows プラットフォームでの実装

ユニバーサル Windows プラットフォームでは、`Windows.Media.SpeechSynthesis` 名前空間に Speech API があります。 注意点は、忘れずにマニフェスト内の**マイク**機能を作動させることだけです。そうしないと、Speech API へのアクセスがブロックされます。

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

`[assembly]` 属性は、クラスを `ITextToSpeech` インターフェイスの実装として登録します。つまり、共有コードで `DependencyService.Get<ITextToSpeech>()` を使用してそのインスタンスを作成できます。

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>共有コードでの実装

これで、音声合成インターフェイスにアクセスする共有コードを記述してテストできます。 このシンプルなページには、音声機能をトリガーするボタンがあります。 `DependencyService` を使用して、`ITextToSpeech` インターフェイスのインスタンスを取得します &ndash; 実行時には、このインスタンスは、ネイティブ SDK にフル アクセスできるプラットフォーム固有の実装になります。

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

iOS、Android、または UWP 上でこのアプリケーションを実行してボタンを押すと、各プラットフォーム上のネイティブな音声 SDK を使用して、アプリケーションによる読み上げが行われます。

 ![iOS と Android の音声合成ボタン](text-to-speech-images/running.png "音声合成のサンプル")


## <a name="related-links"></a>関連リンク

- [DependencyService の使用 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample/)

