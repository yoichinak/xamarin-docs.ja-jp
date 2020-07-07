---
title: Android Speech
description: この記事では、非常に強力な Android.Speech 名前空間を使用する場合の基本について説明します。 Android には、当初から、音声を認識してテキストとして出力する機能がありました。 これは比較的簡単なプロセスです。 ただし、テキスト読み上げの場合、このプロセスはより複雑になります。音声エンジンだけでなく、テキスト読み上げ (TTS) システムから使用できる、またインストールできる言語も考慮する必要があるためです。
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/02/2018
ms.openlocfilehash: e8c7d1a4fb3537644ed3b7737158a5e50abcdae5
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019764"
---
# <a name="android-speech"></a>Android Speech

_この記事では、非常に強力な Android.Speech 名前空間を使用する場合の基本について説明します。Android には、当初から、音声を認識してテキストとして出力する機能がありました。これは比較的簡単なプロセスです。ただし、テキスト読み上げの場合、このプロセスはより複雑になります。音声エンジンだけでなく、テキスト読み上げ (TTS) システムから使用できる、またインストールできる言語も考慮する必要があるためです。_

## <a name="speech-overview"></a>音声の概要

人間の音声を "理解" し、入力されている内容を音声で伝えるシステム (音声テキスト変換とテキスト読み上げ) は、デバイスとの自然なコミュニケーションの需要の高まりにつれて、モバイル開発の分野で成長を続けています。 テキストを音声に (またはその逆方向に) 変換する機能を Android アプリケーションに組み込むと、非常に便利なツールになる例が多数あります。

たとえば、運転中は携帯電話の使用が制限されるため、ユーザーはデバイスを操作するハンズ フリーの方法を求めています。 Android Wear などのさまざまな Android フォーム ファクターや、Android デバイス (タブレットやノート パッドなど) を使用できるデバイスは増え続けているため、優れた TTS アプリケーションがますます重視されています。

Google は、デバイスを "音声認識" にするほとんどの例 (視覚障碍者向けに設計されたソフトウェアなど) をカバーできる Android.Speech 名前空間の豊富な API セットを開発者に提供しています。  この名前空間には、`Android.Speech.Tts` を介してテキストを音声に変換し、翻訳の実行に使用されるエンジンを制御する機能と、音声をテキストに変換できる多数の `RecognizerIntent` が含まれます。

音声を理解するための機能はありますが、使用するハードウェアによって制限があります。 使用できるすべての言語で話されたすべてがデバイスで適切に解釈される可能性はほとんどありません。

## <a name="requirements"></a>必要条件

このガイドには、マイクとスピーカーを備えたデバイス以外に特別な要件はありません。

音声を解釈する Android デバイスの中核には、`Intent` とそれに対応する `OnActivityResult` の使用があります。
ただし、音声は理解されるのではなく、テキストに解釈されるという点を認識することが重要です。 この違いは重要です。

### <a name="the-difference-between-understanding-and-interpreting"></a>理解と解釈の違い

理解の簡単な定義は、話されている内容の本当の意味をトーンと文脈で判断できるということです。 解釈とは、単に単語を取得して別の形式で出力することを意味します。

日常会話で使用される次のような簡単な例を考えてみましょう。

<kbd>Hello, how are you? (こんにちは、元気ですか。)</kbd>

抑揚 (特定の単語または単語の一部の強調) がなければ、簡単な質問です。 ただし、この一節がゆっくり話された場合、聞いている人は、質問者があまり幸せではなく、おそらく元気づける必要があるか、または質問者の具合が悪いと気付きます。 "are" が強調されている場合は、質問している人は、通常、答えにより興味を持っています。

抑揚を利用する非常に強力なオーディオ処理と、文脈を理解できるある程度の人工知能 (AI) がないソフトウェアでは、言われたことを理解することすらできません。単純な電話で実行できることは、音声のテキストへの変換くらいです。

## <a name="setting-up"></a>設定

音声システムを使用する前に、デバイスにマイクがあることを確認することを常にお勧めします。 マイクが搭載されていない Kindle または Google ノート パッドでアプリを実行しようとしても、ほとんど意味はありません。

次のコード サンプルでは、マイクを使用できるかどうかのクエリを実行し、使用できない場合はアラートを作成する方法を示しています。 この時点でマイクを使用できない場合は、アクティビティを終了するか、音声を録音する機能を無効にします。

```csharp
string rec = Android.Content.PM.PackageManager.FeatureMicrophone;
if (rec != "android.hardware.microphone")
{
    var alert = new AlertDialog.Builder(recButton.Context);
    alert.SetTitle("You don't seem to have a microphone to record with");
    alert.SetPositiveButton("OK", (sender, e) =>
    {
        return;
    });
    alert.Show();
}
```

### <a name="creating-the-intent"></a>意図の作成

音声システムの意図には、`RecognizerIntent` という特定の種類の意図が使用されます。 この意図によって、無音が続き、録音が終了したと見なされるまでの待機時間、認識および出力する追加言語、および指示の手段として `Intent` のモーダル ダイアログに含めるテキストなど、多数のパラメーターを制御します。 このスニペットで、`VOICE` は `OnActivityResult` での認識に使用される `readonly int` です。

```csharp
var voiceIntent = new Intent(RecognizerIntent.ActionRecognizeSpeech);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguageModel, RecognizerIntent.LanguageModelFreeForm);
voiceIntent.PutExtra(RecognizerIntent.ExtraPrompt, Application.Context.GetString(Resource.String.messageSpeakNow));
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputPossiblyCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputMinimumLengthMillis, 15000);
voiceIntent.PutExtra(RecognizerIntent.ExtraMaxResults, 1);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguage, Java.Util.Locale.Default);
StartActivityForResult(voiceIntent, VOICE);
```

### <a name="conversion-of-the-speech"></a>音声の変換

音声から解釈されたテキストは、`Intent` 内で配信されます。これは、アクティビティが完了し、`GetStringArrayListExtra(RecognizerIntent.ExtraResults)` を介してアクセスされたときに返されます。 これにより、`IList<string>` が返され、呼び出し元の意図で要求された (かつ `RecognizerIntent.ExtraMaxResults` で指定された) 言語の数に応じて、そのインデックスを使用および表示できます。 ただし、他の一覧と同様に、表示するデータが存在することを確認することが重要です。

`StartActivityForResult` の戻り値をリッスンする場合、`OnActivityResult` メソッドを指定する必要があります。

次の例では、`textBox` は、ディクテーションされた内容を出力するために使用される `TextBox` です。 同様に、テキストを何らかの形式のインタープリターに渡すために使用できます。また、アプリケーションでは、そこからテキストとブランチをアプリケーションの別の部分と比較できます。

```csharp
protected override void OnActivityResult(int requestCode, Result resultVal, Intent data)
{
    if (requestCode == VOICE)
    {
        if (resultVal == Result.Ok)
        {
            var matches = data.GetStringArrayListExtra(RecognizerIntent.ExtraResults);
            if (matches.Count != 0)
            {
                string textInput = textBox.Text + matches[0];
                textBox.Text = textInput;
                switch (matches[0].Substring(0, 5).ToLower())
                {
                    case "north":
                        MovePlayer(0);
                        break;
                    case "south":
                        MovePlayer(1);
                        break;
                }
            }
            else
            {
                textBox.Text = "No speech was recognised";
            }
        }
        base.OnActivityResult(requestCode, resultVal, data);
    }
}
```

## <a name="text-to-speech"></a>テキストから音声へ

テキスト読み上げは、音声テキスト変換の逆ではなく、2 つの主要なコンポーネントに依存しています。デバイスにインストールされているテキスト読み上げエンジンと、インストールされている言語です。

ほとんどの場合、Android デバイスには、少なくとも 1 つの言語の Google TTS サービスが既定でインストールされています。 これは、デバイスが最初に設定されるときに確立され、その時点のデバイスの場所に基づいて決定されます (たとえば、ドイツで設定された電話にはドイツ語がインストールされ、アメリカ合衆国の場合は米国英語がインストールされます)。

### <a name="step-1---instantiating-texttospeech"></a>手順 1 - TextToSpeech のインスタンス化

`TextToSpeech` は最大 3 つのパラメーターを受け取ることができます。最初の 2 つは必須であり、3 つ目は省略可能です (`AppContext`、`IOnInitListener`、`engine`)。 リスナーを使うと、使用できる Android テキスト読み上げエンジンの数にかかわらず、サービスにバインドし、エンジンの障害をテストできます。 少なくとも、デバイスには Google 独自のエンジンが搭載されます。

### <a name="step-2---finding-the-languages-available"></a>手順 2 - 使用できる言語の検索

`Java.Util.Locale` クラスには、`GetAvailableLocales()` という便利なメソッドがあります。 音声エンジンでサポートされているこの言語の一覧は、インストールされている言語に対してテストできます。

"理解される" 言語の一覧の生成は簡単です。 既定の言語 (ユーザーがデバイスを最初に設定したときに設定した言語) は常に存在するため、この例では、`List<string>` には最初のパラメーターとして "Default" があり、一覧の残りは `textToSpeech.IsLanguageAvailable(locale)` の結果に応じて入力されます。

```csharp
var langAvailable = new List<string>{ "Default" };
var localesAvailable = Java.Util.Locale.GetAvailableLocales().ToList();
foreach (var locale in localesAvailable)
{
    var res = textToSpeech.IsLanguageAvailable(locale);
    switch (res)
    {
        case LanguageAvailableResult.Available:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryVarAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
    }
}
langAvailable = langAvailable.OrderBy(t => t).Distinct().ToList();
```

このコードを実行すると、[TextToSpeech.IsLanguageAvailable](xref:Android.Speech.Tts.TextToSpeech.IsLanguageAvailable*) が呼び出され、特定のロケールの言語パッケージがデバイスに既に存在するかどうかがテストされます。
このメソッドからは、渡されたロケールの言語を使用できるかどうかを示す [LanguageAvailableResult](xref:Android.Speech.Tts.LanguageAvailableResult) が返されます。 `LanguageAvailableResult` に言語が `NotSupported` であると示されている場合、その言語の音声パッケージは (ダウンロードであっても) 入手できません。 `LanguageAvailableResult` が `MissingData` に設定されている場合、手順 4 で後述するように、新しい言語パッケージをダウンロードできます。

### <a name="step-3---setting-the-speed-and-pitch"></a>手順 3 - 速度とピッチの設定

Android では、ユーザーは `SpeechRate` と `Pitch` (音声の速度と音声のトーン) を変更することにより、音声の聞こえ方を変更できます。 これは 0 から 1 の範囲であり、"通常の" 音声はいずれについても 1 です。

### <a name="step-4---testing-and-loading-new-languages"></a>手順 4 - 新しい言語のテストと読み込み

新しい言語をダウンロードするには、`Intent` を使用します。 この意図の結果、[OnActivityResult](xref:Android.App.Activity.OnActivityResult*) メソッドが呼び出されます。 音声テキスト変換の例 ([RecognizerIntent](xref:Android.Speech.RecognizerIntent) を `Intent` の `PutExtra` パラメーターとして使用したもの) とは異なり、`Intent` のテストと読み込みは `Action` ベースです。

- [TextToSpeech.Engine.ActionCheckTtsData](xref:Android.Speech.Tts.TextToSpeech.Engine.ActionCheckTtsData) &ndash; プラットフォームの `TextToSpeech` エンジンから、デバイスへの言語リソースの適切なインストールと可用性を確認するアクティビティを開始します。

- [TextToSpeech.Engine.ActionInstallTtsData](xref:Android.Speech.Tts.TextToSpeech.Engine.ActionInstallTtsData) &ndash; 必要な言語のダウンロードするようにユーザーに求めるアクティビティを開始します。

次のコード例は、これらのアクションを使用して言語リソースをテストし、新しい言語をダウンロードする方法を示しています。

```csharp
var checkTTSIntent = new Intent();
checkTTSIntent.SetAction(TextToSpeech.Engine.ActionCheckTtsData);
StartActivityForResult(checkTTSIntent, NeedLang);
//
protected override void OnActivityResult(int req, Result res, Intent data)
{
    if (req == NeedLang)
    {
        var installTTS = new Intent();
        installTTS.SetAction(TextToSpeech.Engine.ActionInstallTtsData);
        StartActivity(installTTS);
    }
}
```

`TextToSpeech.Engine.ActionCheckTtsData` を使うと、言語リソースを使用できるかどうかをテストできます。 このテストが完了すると、`OnActivityResult` が呼び出されます。 言語リソースをダウンロードする必要がある場合、`OnActivityResult` から `TextToSpeech.Engine.ActionInstallTtsData` アクションを起動して、ユーザーが必要な言語をダウンロードできるようにするアクティビティを開始します。 この簡略化された例では、言語パッケージをダウンロードする必要があるという決定が既に行われているため、この `OnActivityResult` 実装では `Result` コードを確認しません。

`TextToSpeech.Engine.ActionInstallTtsData` アクションにより、ユーザーがダウンロードする言語を選択できる **Google TTS 音声データ** アクティビティが表示されます。

![Google TTS 音声データ アクティビティ](speech-images/01-google-tts-voice-data.png)

たとえば、ユーザーはフランス語を選択し、ダウンロード アイコンをクリックしてフランス語の音声データをダウンロードするとします。

![フランス語をダウンロードする例](speech-images/02-selecting-french.png)

このデータのインストールは、ダウンロードの完了後に自動的に実行されます。

### <a name="step-5---the-ioninitlistener"></a>手順 5 - IOnInitListener

アクティビティでテキストを音声に変換できるようにするには、インターフェイス メソッド `OnInit` を実装する必要があります (これは、`TextToSpeech` クラスのインスタンス化に指定された 2 番目のパラメーターです)。 これにより、リスナーが初期化され、結果がテストされます。

リスナーでは、少なくとも `OperationResult.Success` と `OperationResult.Failure` の両方をテストする必要があります。
次の例はそれを示しています。

```csharp
void TextToSpeech.IOnInitListener.OnInit(OperationResult status)
{
    // if we get an error, default to the default language
    if (status == OperationResult.Error)
        textToSpeech.SetLanguage(Java.Util.Locale.Default);
    // if the listener is ok, set the lang
    if (status == OperationResult.Success)
        textToSpeech.SetLanguage(lang);
}
```

## <a name="summary"></a>まとめ

このガイドでは、テキストを音声に変換し、音声をテキストに変換する場合の基本と、それらを自分のアプリに追加する場合に考えられる方法について説明しました。 あらゆるケースをカバーしているわけではありませんが、音声の解釈方法、新しい言語のインストール方法、アプリの包括性を高める方法についての基本的な知識が身についたはずです。

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の DependencyService](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice//)
- [テキスト読み上げ (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-texttospeech)
- [音声テキスト変換 (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-speechtotext)
- [Android.Speech 名前空間](xref:Android.Speech)
- [Android.Speech.Tts 名前空間](xref:Android.Speech.Tts)
