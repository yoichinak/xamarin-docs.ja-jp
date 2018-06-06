---
title: DependencyService の概要
description: DependencyService とアクセス ネイティブ プラットフォームの機能の動作を理解します。
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 6a0544a58245efed696b92c543fbaf5e7e1b8365
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732828"
---
# <a name="introduction-to-dependencyservice"></a>DependencyService の概要

## <a name="overview"></a>概要

[`DependencyService`](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) 共有コードからプラットフォーム固有の機能への呼び出しをアプリを使用できます。 この機能により、Xamarin.Forms アプリをすべてのネイティブ アプリを実行できます。

`DependencyService` 依存関係競合回避モジュールがします。 実際には、インターフェイスが定義されていると`DependencyService`さまざまなプラットフォームのプロジェクトからそのインターフェイスの適切な実装を検索します。

## <a name="how-dependencyservice-works"></a>DependencyService のしくみ

Xamarin.Forms アプリが次の 4 つのコンポーネントを使用する必要がある`DependencyService`:

- **インターフェイス**&ndash;必要な機能は、共有コードのインターフェイスによって定義されます。
- **プラットフォームごとの実装**&ndash;インターフェイスを実装するクラスは、各プラットフォームのプロジェクトに追加する必要があります。
- **登録**&ndash;に各実装するクラスを登録する必要があります`DependencyService`メタデータ属性を使用しています。 登録有効`DependencyService`を実装するクラスを検索し、実行時に、インターフェイスの代わりに指定します。
- **DependencyService への呼び出し**&ndash;共有コードの要件を明示的に呼び出す`DependencyService`にインターフェイスの実装を確認してください。

内の各プラットフォーム プロジェクト、ソリューションの実装を指定する必要がありますに注意してください。 実装を含まないプラットフォームのプロジェクトは、実行時に失敗します。

アプリケーションの構造は次の図でについて説明します。

![](introduction-images/overview-diagram.png "DependencyService アプリケーション構造")

### <a name="interface"></a>Interface

インターフェイスを設計するには、プラットフォーム固有の機能と対話する方法を定義します。 コンポーネントまたは Nuget パッケージとして共有されるコンポーネントを開発している場合は注意します。 API の設計では、作成したり、パッケージを中断することができます。 次の例は、テキストを読み上げる、単語を指定するときに柔軟性が各プラットフォーム用にカスタマイズする実装状態のままで話しの単純なインターフェイスを指定します。

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>プラットフォームごとの実装

適切なインターフェイスを設計すると後、を対象とする各プラットフォームのプロジェクトでそのインターフェイスを実装する必要があります。 たとえば、次のクラスを実装、 `ITextToSpeech` iOS 上のインターフェイス。

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

登録する必要がある各インターフェイスの実装の`DependencyService`メタデータ属性を持つ。 次のコードは、iOS の実装を登録します。

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
  ...
}
```

まとめ、プラットフォーム固有の実装は、これのようになります。

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

.NET ネイティブのコンパイル オプションを使用する UWP プロジェクトが従う必要があります、[わずかに異なる構成](~/xamarin-forms/platform/windows/installation/index.md#target-invocation-exception)Xamarin.Forms を初期化するときにします。 .NET ネイティブのコンパイルには、依存サービスのための若干異なる登録も必要です。

**App.xaml.cs**ファイルを使用して、UWP プロジェクトで定義されている各依存関係サービスを手動で登録、`Register<T>`メソッドを次のようにします。

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

注: 手動で登録を使用して`Register<T>`リリースでのみ効果的なビルド .NET ネイティブ コンパイルを使用します。 依存関係サービスを読み込むには、この行を省略すると、デバッグ ビルドは動作しますが、リリース ビルドは失敗します。

### <a name="call-to-dependencyservice"></a>DependencyService への呼び出し

共通のインターフェイスと各プラットフォームの実装を持つプロジェクトがセットアップになったら、使用して`DependencyService`を実行時に、的確な実装を取得します。

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` インターフェイスの適切な実装を見つける`T`です。

### <a name="solution-structure"></a>ソリューションの構造

[サンプル UsingDependencyService ソリューション](https://developer.xamarin.com/samples/UsingDependencyService/)は iOS および Android 用の下に示すように、上記のコード変更で強調表示されます。

 [![iOS と Android ソリューション](introduction-images/solution-sml.png "DependencyService サンプル ソリューションの構造")](introduction-images/solution.png#lightbox "DependencyService サンプル ソリューションの構造")

> [!NOTE]
> **必要があります**すべてのプラットフォーム プロジェクトでの実装を提供します。 インターフェイスの実装が登録されていない場合、`DependencyService`を解決することはできません、`Get<T>()`メソッド実行時にします。


## <a name="related-links"></a>関連リンク

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
