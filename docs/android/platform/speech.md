---
title: Android の音声
description: この記事では、非常に強力な Android. Speech 名前空間の使用の基本について説明します。 Android は、その開始以来、音声認識を認識し、テキストとして出力できるようになりました。 これは比較的簡単なプロセスです。 ただし、テキストを音声に変換する場合は、音声エンジンを考慮する必要があるだけでなく、使用可能であり、テキスト読み上げ (TTS) システムからインストールされた言語も関係しています。
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/02/2018
ms.openlocfilehash: 14cce06399b804ba8fd982a40347fb3146b281c8
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757417"
---
# <a name="android-speech"></a>Android の音声

_この記事では、非常に強力な Android. Speech 名前空間の使用の基本について説明します。Android は、その開始以来、音声認識を認識し、テキストとして出力できるようになりました。これは比較的簡単なプロセスです。ただし、テキストを音声に変換する場合は、音声エンジンを考慮する必要があるだけでなく、使用可能であり、テキスト読み上げ (TTS) システムからインストールされた言語も関係しています。_

## <a name="speech-overview"></a>音声の概要

システムを用意することにより、デバイスとの自然な通信の需要に応じて、モバイル開発内で人間の音声と enunciates Text to Speech を "認識" し、何が入力されているかを把握できます。 テキストを音声に変換する機能や、その逆の機能がある場合は、android アプリケーションに組み込むのに非常に便利なツールです。

たとえば、運転中に携帯電話の使用を停止すると、ユーザーは自分のデバイスを自由に操作できます。 Android の磨耗などのさまざまな Android フォームファクターや、Android デバイス (タブレットやメモパッドなど) を使用できるようになるまでに拡大する機能が多数用意されているため、優れた TTS アプリケーションに重点を置いています。

Google は、Android. Speech 名前空間の豊富な Api セットを開発者に提供し、デバイスの "音声認識" (ブラインド向けに設計されたソフトウェアなど) を作成するほとんどのインスタンスに対応します。  名前空間には、テキストを音声`Android.Speech.Tts`に変換する機能、翻訳を実行するために使用するエンジンを制御する機能、および音声をテキストに変換するためのの多数の`RecognizerIntent`機能が含まれています。

音声を認識する施設はありますが、使用されるハードウェアによって制限があります。 使用可能なすべての言語で、デバイスが話されているすべてのものを正常に解釈することはほとんどありません。

## <a name="requirements"></a>必要条件

このガイドには、マイクとスピーカーを備えたデバイス以外に特別な要件はありません。

音声を解釈する Android デバイスのコアは、対応`Intent` `OnActivityResult`するを持つを使用することです。
ただし、音声は認識されないが、テキストに解釈されることを認識することが重要です。 この違いは重要です。

### <a name="the-difference-between-understanding-and-interpreting"></a>理解と解釈の違い

理解の簡単な定義としては、雰囲気とコンテキストによって決定できることと、その意味を示すことがあります。 これを解釈するには、単語を取得して別の形式で出力することを意味します。

日常的な会話で使用される次の簡単な例を考えてみましょう。

<kbd>こんにちは、元気ですか。</kbd>

変 (特定の単語または単語の一部に重点を置く) を使用しない場合、これは簡単な質問です。 ただし、回線に低速なペースが適用されている場合、そのユーザーは、質問者が満足していないこと、または声援が必要であること、または質問がないことを検出します。 強調が "is" に配置されている場合は、通常、回答を求めている人が多くなります。

非常に強力なオーディオ処理を使用しても、文脈を理解するために非常に強力なオーディオ処理はありません。また、このような人工知能 (AI) では、このような状況を把握することはできません。単純な携帯電話では、音声をテキストに変換するのが最適です。

## <a name="setting-up"></a>設定

音声システムを使用する前に、デバイスにマイクがあることを確認することを常にお勧めします。 マイクがインストールされていない状態で、Kindle または Google note パッドでアプリを実行しようとしていることはほとんどありません。

次のコードサンプルでは、マイクが使用可能かどうかを照会し、存在しない場合はアラートを作成する方法を示します。 この時点でマイクが使用できない場合は、アクティビティを終了するか、音声を録音する機能を無効にします。

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

### <a name="creating-the-intent"></a>インテントの作成

Speech システムの目的は、 `RecognizerIntent`と呼ばれる特定の種類のインテントを使用することです。 このインテントは、多数のパラメーターを制御します。これには、記録が検討されるまで、無音を待機する時間、認識して出力する追加の言語、 `Intent`および命令の手段としてのモーダルダイアログに含めるテキストが含まれます。 このスニペットで`readonly int`は`VOICE` 、はで`OnActivityResult`認識するために使用されます。

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

音声から解釈されるテキストは、 `Intent`で配信されます。これは、アクティビティが完了し、経由`GetStringArrayListExtra(RecognizerIntent.ExtraResults)`でアクセスされるときに返されます。 これにより、 `IList<string>`呼び出し元の意図で要求された言語の数 ( `RecognizerIntent.ExtraMaxResults`で指定) に応じて、インデックスを使用および表示できるが返されます。 ただし、他の一覧と同様に、表示されるデータがあることを確認することが重要です。

`StartActivityForResult`の`OnActivityResult`戻り値をリッスンする場合は、メソッドを指定する必要があります。

次の例で`textBox` `TextBox`は、は、どのような内容がディクテーションされたかを出力するために使用されます。 これを使用すると、テキストを何らかの形式のインタープリターに渡すことができ、アプリケーションはテキストと分岐をアプリケーションの別の部分と比較できます。

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

## <a name="text-to-speech"></a>Text to Speech

テキスト読み上げは、音声からテキストへの逆の逆ではなく、2つの主要なコンポーネントに依存しています。デバイスにインストールされている音声合成エンジンとインストールされている言語。

ほとんどの場合、Android デバイスには、既定の Google TTS サービスがインストールされ、少なくとも1つの言語が付属しています。 デバイスが最初を設定しすると、に基づいて時にデバイスは、これが確立される (たとえば、ドイツで設定する電話がインストールされますドイツ語の言語、アメリカ英語アメリカ合衆国のいずれかがある搭載されます)。

### <a name="step-1---instantiating-texttospeech"></a>手順 1-TextToSpeech のインスタンス化

`TextToSpeech`最大3つのパラメーターを受け取ることができます。3番目のパラメーターは`AppContext`省略`IOnInitListener`可能`engine`(,,) である必要があります。 リスナーは、サービスにバインドするために使用されます。エンジンは、任意の数の利用可能な Android テキストエンジンに対応しています。 デバイスは、少なくとも Google 独自のエンジンを持つことになります。

### <a name="step-2---finding-the-languages-available"></a>手順 2-使用可能な言語の検索

クラス`Java.Util.Locale`には、という`GetAvailableLocales()`便利なメソッドが含まれています。 音声エンジンでサポートされている言語の一覧は、インストールされている言語に対してテストできます。

"認識される" 言語の一覧を生成するのは簡単です。 常に既定の言語 (ユーザーがデバイスを最初に設定したときに設定した言語) が使用されます`List<string>` `textToSpeech.IsLanguageAvailable(locale)`。したがって、この例では、最初のパラメーターとして "default" が指定されており、の結果によってリストの残りの部分が入力されます。

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

このコードは、指定されたロケールの言語パッケージが既にデバイス上に存在するかどうかをテストするために、 [Texttospeech. islanguage を呼び出します。](xref:Android.Speech.Tts.TextToSpeech.IsLanguageAvailable*)
このメソッドは、渡されたロケールの言語が使用可能かどうかを示す[LanguageAvailableResult](xref:Android.Speech.Tts.LanguageAvailableResult)を返します。 `LanguageAvailableResult` が`NotSupported`言語であることを示している場合は、その言語の音声パッケージ (ダウンロードでも) は使用できません。 が`LanguageAvailableResult` に`MissingData`設定されている場合は、手順4で後述するように、新しい言語パッケージをダウンロードできます。

### <a name="step-3---setting-the-speed-and-pitch"></a>手順 3-速度とピッチの設定

Android では、ユーザーは`SpeechRate`と`Pitch` (音声の速度と音声の声調速度) を変更することで、音声の音を変更できます。 これは0から1までの間で、"通常の" 音声が1になります。

### <a name="step-4---testing-and-loading-new-languages"></a>手順 4-新しい言語のテストと読み込み

新しい言語のダウンロードは、 `Intent`を使用して実行されます。 このインテントの結果により、 [Onactivityresult](xref:Android.App.Activity.OnActivityResult*)メソッドが呼び出されます。 音声からテキストへの変換の例 ( `PutExtra`の`Intent`パラメーターと[して認識を使用して](xref:Android.Speech.RecognizerIntent)いた) とは異なり、の`Intent`テストと`Action`読み込みはに基づいています。

- [Texttospeech. actioncheckttsdata](xref:Android.Speech.Tts.TextToSpeech.Engine.ActionCheckTtsData) &ndash;は、プラットフォーム`TextToSpeech`エンジンからアクティビティを開始して、デバイスでの言語リソースの適切なインストールと可用性を検証します。

- [Texttospeech. actioninstallttsdata](xref:Android.Speech.Tts.TextToSpeech.Engine.ActionInstallTtsData) &ndash;は、必要な言語をダウンロードするようユーザーに求めるアクティビティを開始します。

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

`TextToSpeech.Engine.ActionCheckTtsData`言語リソースが使用可能かどうかをテストします。 `OnActivityResult`このテストが完了すると、が呼び出されます。 言語リソースをダウンロードする必要がある`OnActivityResult`場合は、 `TextToSpeech.Engine.ActionInstallTtsData`ユーザーが必要な言語をダウンロードできるアクティビティを開始するために、アクションを起動します。 この簡略化さ`OnActivityResult`れた例では`Result` 、言語パッケージをダウンロードする必要があることを確認したため、この実装ではコードがチェックされないことに注意してください。

アクションによって、ダウンロードする言語を選択するための**Google TTS 音声データ**アクティビティがユーザーに表示されます。 `TextToSpeech.Engine.ActionInstallTtsData`

![Google TTS 音声データアクティビティ](speech-images/01-google-tts-voice-data.png)

たとえば、ユーザーがフランス語を選択し、ダウンロードアイコンをクリックしてフランス語の音声データをダウンロードする場合があります。

![フランス語をダウンロードする例](speech-images/02-selecting-french.png)

このデータは、ダウンロードの完了後に自動的にインストールされます。

### <a name="step-5---the-ioninitlistener"></a>手順 5-IOnInitListener

アクティビティがテキストを音声に変換できるようにするには、インターフェイスメソッド`OnInit`を実装する必要があります (これは、 `TextToSpeech`クラスのインスタンス化に対して指定された2番目のパラメーターです)。 これにより、リスナーが初期化され、結果がテストされます。

リスナーは、少なくとも`OperationResult.Success`と`OperationResult.Failure`の両方をテストする必要があります。
次の例では、このことを示しています。

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

## <a name="summary"></a>Summary

このガイドでは、テキストを音声と音声に変換する方法の基本について説明し、独自のアプリ内にテキストを含める方法について説明しました。 すべてのケースに対応しているわけではありませんが、音声の解釈方法、新しい言語のインストール方法、アプリの inclusivity を向上させる方法についての基本的な理解を得られるようになりました。

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms DependencyService](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice//)
- [Text to Speech (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-texttospeech)
- [音声をテキストに変換 (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-speechtotext)
- [Android. Speech 名前空間](xref:Android.Speech)
- [Android. Speech. Tts 名前空間](xref:Android.Speech.Tts)
