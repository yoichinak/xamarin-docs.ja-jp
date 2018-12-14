---
title: DependencyService の概要
description: この記事では、ネイティブ プラットフォームの機能にアクセスする Xamarin.Forms の DependencyService クラスのしくみについて説明します。
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/15/2018
ms.openlocfilehash: 3c8cc31c21f354b60001cefb919b51bf4d42da9f
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675017"
---
# <a name="introduction-to-dependencyservice"></a>DependencyService の概要

## <a name="overview"></a>概要

[`DependencyService`](xref:Xamarin.Forms.DependencyService) を使用すると、アプリで共有コードからプラットフォーム固有の機能を呼び出すことができます。 この機能により、ネイティブ アプリでできるすべてのことを、Xamarin.Forms アプリで行うことができます。

`DependencyService` はサービス ロケーターです。 実際には、インターフェイスが定義されており、`DependencyService` ではさまざまなプラットフォームのプロジェクトからそのインターフェイスの適切な実装が検索されます。

> [!NOTE]
> 既定では、パラメーターなしのコンストラクターがあるプラットフォームの実装だけが、[`DependencyService`](xref:Xamarin.Forms.DependencyService) によって解決されます。 ただし、依存関係挿入コンテナーまたはファクトリの方法を使用してプラットフォームの実装を解決する依存関係解決方法を、Xamarin.Forms に挿入することができます。 この方法を使用して、パラメーターがあるコンストラクターを持つプラットフォームの実装を解決できます。 詳しくは、「[Xamarin.Forms での依存関係の解決](~/xamarin-forms/internals/dependency-resolution.md)」をご覧ください。

## <a name="how-dependencyservice-works"></a>DependencyService のしくみ

Xamarin.Forms アプリで `DependencyService` を使用するには、4 つのコンポーネントが必要です。

- **インターフェイス** &ndash; 必要な機能は、共有コード内のインターフェイスによって定義されます。
- **プラットフォームごとの実装** &ndash; インターフェイスを実装するクラスを、各プラットフォーム プロジェクトに追加する必要があります。
- **登録** &ndash; メタデータ属性を使用して、各実装クラスを `DependencyService` に登録する必要があります。 登録することにより、`DependencyService` で実装クラスを検出し、実行時にインターフェイスの代わりにそれを提供できるようになります。
- **DependencyService の呼び出し** &ndash; 共有コードでは、`DependencyService` インターフェイスを明示的に呼び出して、インターフェイスの実装を要求する必要があります。

ソリューション内の各プラットフォーム プロジェクトに対して実装を提供する必要があることに注意してください。 実装のないプラットフォーム プロジェクトは、実行時に失敗します。

次の図はアプリケーションの構造を説明したものです。

![](introduction-images/overview-diagram.png "DependencyService アプリケーションの構造")

### <a name="interface"></a>Interface

設計するインターフェイスでは、プラットフォーム固有の機能とやり取りする方法を定義します。 コンポーネントまたは NuGet パッケージとして共有するコンポーネントを開発する場合は注意してください。 API の設計によってパッケージが作成されたり壊れたりする可能性があります。 次の例はテキストを読み上げるシンプルなインターフェイスですが、読み上げる単語を柔軟に指定できるようになっている一方で、実装はプラットフォームごとにカスタマイズできます。

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>プラットフォームごとの実装

適切なインターフェイスを設計した後は、対象となるプラットフォームごとのプロジェクトで、そのインターフェイスを実装する必要があります。 たとえば、次のクラスでは、iOS での `ITextToSpeech` インターフェイスが実装されています。

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

インターフェイスの各実装を、メタデータ属性で `DependencyService` に登録する必要があります。 次のコードでは、iOS 用の実装が登録されます。

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
  ...
}
```

まとめると、プラットフォーム固有の実装は次のようになります。

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

注: 登録は、クラス レベルではなく名前空間レベルで実行されることに注意してください。

#### <a name="universal-windows-platform-net-native-compilation"></a>ユニバーサル Windows プラットフォームの .NET ネイティブ コンパイル

.NET ネイティブ コンパイル オプションを使用する UWP プロジェクトでは、Xamarin.Forms を初期化するときに[若干異なる構成](~/xamarin-forms/platform/windows/installation/index.md#target-invocation-exception)に従う必要があります。 .NET ネイティブ コンパイルでは、依存関係サービスに対する若干異なる登録も必要です。

次に示すように、**App.xaml.cs** ファイルで `Register<T>` メソッドを使用して、UWP プロジェクトで定義されている各依存関係サービスを手動で登録します。

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

注: `Register<T>` を使用する手動登録は、.NET ネイティブ コンパイルを使用したリリース ビルドでのみ有効です。 この行を省略した場合、デバッグ ビルドでも動作しますが、リリース ビルドでの依存関係サービスの読み込みが失敗します。

### <a name="call-to-dependencyservice"></a>DependencyService を呼び出す

共通インターフェイスとプラットフォームごとの実装でプロジェクトを設定した後、実行時には `DependencyService` を使用して適切な実装を取得します。

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` では、インターフェイス `T` の適切な実装が検索されます。

### <a name="solution-structure"></a>ソリューションの構造

次に示すのは iOS および Android に対する[サンプルの UsingDependencyService ソリューション](https://developer.xamarin.com/samples/UsingDependencyService/)であり、上で説明したコードの変更が強調表示されています。

 [![iOS と Android のソリューション](introduction-images/solution-sml.png "DependencyService サンプル ソリューションの構造")](introduction-images/solution.png#lightbox "DependencyService サンプル ソリューションの構造")

> [!NOTE]
> すべてのプラットフォーム プロジェクトで実装を提供する**必要があります**。 インターフェイスの実装が登録されていない場合、`DependencyService` では実行時に `Get<T>()` メソッドを解決できません。

## <a name="related-links"></a>関連リンク

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
