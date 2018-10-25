---
title: DependencyService の概要
description: この記事では、ネイティブ プラットフォーム機能の利用の Xamarin.Forms DependencyService クラスのしくみについて説明します。
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/15/2018
ms.openlocfilehash: 28c6daa361b7de09a0d9332b21f1b6f75e035850
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/24/2018
ms.locfileid: "38995415"
---
# <a name="introduction-to-dependencyservice"></a>DependencyService の概要

## <a name="overview"></a>概要

[`DependencyService`](xref:Xamarin.Forms.DependencyService) 共有コードからプラットフォーム固有の機能を呼び出すアプリを許可します。 この機能により、ネイティブ アプリで実行できるも何もする Xamarin.Forms アプリです。

`DependencyService` サービス ロケーターです。 実際には、インターフェイスが定義されていると`DependencyService`さまざまなプラットフォームのプロジェクトからそのインターフェイスの適切な実装を検索します。

> [!NOTE]
> 既定で、 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService)パラメーターなしのコンス トラクターがあるプラットフォームの実装を唯一の解決になります。 ただし、依存関係の解決方法は、Xamarin.Forms プラットフォームの実装を解決するのには、依存関係注入コンテナーまたはファクトリ メソッドを使用するに挿入することができます。 この方法は、パラメーターを持つコンス トラクターがあるプラットフォームの実装を解決するのには使用できます。 詳細については、次を参照してください。 [Xamarin.Forms での依存関係の解決](~/xamarin-forms/internals/dependency-resolution.md)します。

## <a name="how-dependencyservice-works"></a>DependencyService のしくみ

Xamarin.Forms アプリが 4 つのコンポーネントを使用する必要がある`DependencyService`:

- **インターフェイス**&ndash;必要な機能は、共有コードで、インターフェイスによって定義されます。
- **プラットフォームごとの実装**&ndash;インターフェイスを実装するクラスは、各プラットフォーム プロジェクトに追加する必要があります。
- **登録**&ndash;を実装する各クラスを登録する必要があります`DependencyService`メタデータ属性を使用しています。 登録有効`DependencyService`を実装するクラスを見つけて、実行時に、インターフェイスの代わりにそれを指定します。
- **DependencyService を呼び出す**&ndash;を明示的に呼び出すコードの要件に共有`DependencyService`インターフェイスの実装を要求します。

ソリューション内の各プラットフォーム プロジェクトの実装を指定する必要がありますに注意してください。 実装を含まないプラットフォーム プロジェクトは、実行時に失敗します。

アプリケーションの構造は次の図で説明します。

![](introduction-images/overview-diagram.png "DependencyService アプリケーション構造")

### <a name="interface"></a>Interface

インターフェイスを設計するには、プラットフォーム固有の機能と対話する方法を定義します。 コンポーネントまたは Nuget パッケージとして共有されるコンポーネントを開発している場合は注意します。 API の設計では、作成したり、パッケージを中断することができます。 次の例では、読み上げられる、単語を指定するときに柔軟性が各プラットフォーム用にカスタマイズを実装するテキストの読み上げのシンプルなインターフェイスを指定します。

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>プラットフォームごとの実装

適切なインターフェイスを設計すると後、は、対象とする各プラットフォーム用のプロジェクトでそのインターフェイスを実装する必要があります。 たとえば、次のクラス実装、 `ITextToSpeech` iOS 上のインターフェイス。

```csharp
namespace UsingDependencyService.iOS
{
    public class TextToSpeech_iOS : ITextToSpeech
    {
        public void Speak (string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer ();

            var speechUtterance = new AVSpeechUtterance (text) {
                Rate = AVSpeechUtterance.MaximumSpeechRate/4,
                Voice = AVSpeechSynthesisVoice.FromLanguage ("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance (speechUtterance);
        }
    }
}
```

### <a name="registration"></a>登録

登録する必要があるインターフェイスの各実装`DependencyService`メタデータ属性を持つ。 次のコードは、iOS の実装を登録します。

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
  ...
}
```

プラットフォーム固有の実装はすべてをまとめて、ようになります。

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
    public class TextToSpeech_iOS : ITextToSpeech
    {
        public void Speak (string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer ();

            var speechUtterance = new AVSpeechUtterance (text) {
                Rate = AVSpeechUtterance.MaximumSpeechRate/4,
                Voice = AVSpeechSynthesisVoice.FromLanguage ("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance (speechUtterance);
        }
    }
}
```

注: 登録をクラス レベルではなく、名前空間レベルで実行されます。

#### <a name="universal-windows-platform-net-native-compilation"></a>ユニバーサル Windows プラットフォームの .NET ネイティブ コンパイル

.NET ネイティブのコンパイル オプションを使用する UWP プロジェクトが従う必要があります、[若干異なる構成](~/xamarin-forms/platform/windows/installation/index.md#target-invocation-exception)Xamarin.Forms を初期化するときにします。 .NET ネイティブのコンパイルには、依存サービスのための若干異なる登録も必要です。

**App.xaml.cs**ファイルを使用して UWP プロジェクトで定義された各依存関係サービスを手動で登録、`Register<T>`メソッドを次に示すよう。

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

注: 手動で登録を使用して`Register<T>`リリースでのみ有効なビルド .NET ネイティブ コンパイルを使用します。 依存関係サービスを読み込むには、この行を省略すると、デバッグ ビルドは動作しますが、リリース ビルドは失敗します。

### <a name="call-to-dependencyservice"></a>DependencyService への呼び出し

共通のインターフェイスと実装の各プラットフォーム プロジェクトを設定すると、使用して`DependencyService`実行時に適切な実装を取得します。

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` 適切なインターフェイスの実装を見つける`T`します。

### <a name="solution-structure"></a>ソリューション構造

[サンプル UsingDependencyService ソリューション](https://developer.xamarin.com/samples/UsingDependencyService/)は iOS および Android 用の次に示す、上記で説明したコードの変更が強調表示されます。

 [![iOS と Android ソリューション](introduction-images/solution-sml.png "DependencyService サンプルのソリューション構造")](introduction-images/solution.png#lightbox "DependencyService サンプル ソリューションの構造")

> [!NOTE]
> **する必要があります**プラットフォーム プロジェクトごとに実装を提供します。 インターフェイスの実装が登録されていない場合、`DependencyService`を解決することはできません、`Get<T>()`メソッド実行時にします。

## <a name="related-links"></a>関連リンク

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
