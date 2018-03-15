---
title: "Android の音声"
description: "この記事では、非常に強力な Android.Speech 名前空間の基本的な使い方について説明します。 以来、Android を音声を認識し、テキストとして出力ができました。 これは、比較的単純なプロセスです。 テキスト読み上げ、ただし、そのプロセスはより複雑にだけでなく、音声認識エンジンに考慮すると、使用可能で、音声合成 (TTS) システムからインストールされている言語もします。"
ms.topic: article
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: e8e56afbdf0b68ecc49a89b08b2e67a9715f2aef
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="android-speech"></a>Android の音声

_この記事では、非常に強力な Android.Speech 名前空間の基本的な使い方について説明します。以来、Android を音声を認識し、テキストとして出力ができました。これは、比較的単純なプロセスです。テキスト読み上げ、ただし、そのプロセスはより複雑にだけでなく、音声認識エンジンに考慮すると、使用可能で、音声合成 (TTS) システムからインストールされている言語もします。_

## <a name="speech-overview"></a>音声認識の概要

人間の音声を「認識」と入力されている内容は enunciates システム — テキスト、テキストを音声に音声 — モバイル開発内かつての領域は、デバイスとの通信を自然の需要が増加します。 多くのインスタンスがある場所テキスト読み上げに変換またはその逆の場合は、android アプリケーションに組み込むに非常に便利なツール機能が含まれています。

たとえば、運転中に、携帯電話の使用にダウン クランプ、ユーザーする自分のデバイスの動作の手空き方法になります。 さまざまな Android フォーム ファクター多種多様-Android 着用など — およびこれまで拡大追加は、これらの Android デバイス (タブレットやノート パッド) を使用することが作成より大きなフォーカス優れた音声合成によるアプリケーションのです。

Google には、デバイス (など、ブラインド用に設計されたソフトウェア)「音声認識」のほとんどのインスタンスをカバーする豊富な一連の Api Android.Speech 名前空間内で、開発者が用意されています。  名前空間には音声に変換するテキストを許可する機能が含まれています`Android.Speech.Tts`、エンジンの数と同様に、変換を実行するために使用を制御`RecognizerIntent`%s がテキストに変換する音声認識を許可します。

機能は音声認識されるがありますは、使用されるハードウェアに基づく制限があります。 これは、デバイスが正常に解釈に利用可能なすべての言語で話されるすべてのものがほとんどありません。

## <a name="requirements"></a>必要条件

このガイドでは、マイクとスピーカーを持つデバイス以外の特別な要件はありません。

音声認識を解釈する Android デバイスのコアの使用は、`Intent`に対応する`OnActivityResult`です。
重要ですが、ただし、認識、音声認識が認識されません-テキストに、解釈されました。 違いは重要です。

### <a name="the-difference-between-understanding-and-interpreting"></a>理解と解釈の違い

定義を簡単に理解するのでは、トーンとコンテキストであれの本当の意味を決定できることです。 だけを解釈するには、単語を実行し、それらを別の形式で出力することです。

日常的なメッセージ交換で使用されている次の単純な例について考えます。 

<kbd>こんにちは、元気ですか。</kbd>

(重点が置かれて特定の単語や単語の一部) 語尾変化がない単純な質問です。 ただし、行に低速のペースを適用する場合は、リッスンしている人はいる発言権は不満声援おそらく必要も発言権が適切ではないことが検出されます。 重点が置かれている場合に"are"、通常の応答に重点をユーザーのかを確認します。

処理を行う、非常に強力なオーディオなしの使用、語形変化し、人工知能 (AI) のある程度のコンテキストを理解する、ソフトウェアが提案した内容を理解する始める — 最善単純な電話ことができますが、音声をテキストに変換します。

## <a name="setting-up"></a>設定

音声システムを使用する前に、デバイスには、マイクをことを確認することが賢明は常にします。 インストールされているマイクがなくても kindle 用または Google のメモ帳でアプリを実行しようとしています。 ほとんどのポイントがありません。

次のコード例では、マイクが使用可能な場合とできない場合、クエリを示しています、警告を作成します。 マイクが使用できない場合この時点では、アクティビティを終了または無効にする、音声を記録する機能。

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

### <a name="creating-the-intent"></a>目的の作成

音声システムの目的は、特定の種類と呼ばれる目的を使用して、`RecognizerIntent`です。 この目的の制御を無音経由で、追加の言語を認識し、出力と見なされます、記録されるまで待機する時間などのパラメーターとに含める任意のテキストの数が多い、`Intent`のモーダル ダイアログ ボックスの命令のことを意味します。 このスニペットで`VOICE`は、`readonly int`で認識するために使用される`OnActivityResult`です。

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

内での音声から解釈されるテキストを配信は、 `Intent`、アクティビティが完了し、経由でアクセス時に返される`GetStringArrayListExtra(RecognizerIntent.ExtraResults)`です。 これは、戻り値は、 `IList<string>`、するインデックスを使用して、呼び出し元の目的で要求された言語の数に応じて表示の (とで指定した、 `RecognizerIntent.ExtraMaxResults`)。 任意のリストは価値があるデータを表示することを確認します。

戻り値を待機している、 `StartActivityForResult`、`OnActivityResult`メソッドが指定されることができます。

次の例で`textBox`は、`TextBox`新機能が指示されてを出力するために使用します。 インタープリターのされ、そこから何らかの形式をテキストを渡すために使用可能でした均等には、アプリケーションは、テキストと、アプリケーションの別の部分に分岐を比較できます。

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
                  switch(matches[0].Substring(0,5).ToLower())
                  {
                     case "north":
                      MovePlayer(0);
                     break;
                   case "south":
                     MovePlayer(1);
                     break;
             }
             else
                  textBox.Text = "No speech was recognised";
        }
   }
    base.OnActivityResult(requestCode, resultVal, data);
}
```

## <a name="text-to-speech"></a>Text to Speech

テキスト読み上げをテキストの音声の逆順では非常になっていないと; 2 つの主要コンポーネントに依存しています音声合成エンジンは、デバイスにインストールされていると、言語がインストールされています。

既定の Android デバイスに付属して大きく、Google TTS サービスがインストールされていると、少なくとも 1 つの言語です。 デバイスが最初を設定しすると、に基づいて時にデバイスは、これが確立される (たとえば、ドイツで設定する電話がインストールされますドイツ語の言語、アメリカ英語アメリカのいずれかがある搭載されます)。

### <a name="step-1---instantiating-texttospeech"></a>手順 1 - TextToSpeech をインスタンス化します。

`TextToSpeech` 最大 3 つのパラメーターを受け取り、最初の 2 つを 3 番目と共に使用される省略可能な (`AppContext`、 `IOnInitListener`、 `engine`)。 リスナーを使用して、任意の数の使用可能な Android 音声合成エンジンをされているエンジンのサービス、およびエラーのテストにバインドします。 少なくとも、デバイスは、Google のエンジンがあります。

### <a name="step-2---finding-the-languages-available"></a>手順 2 - 使用できる言語を検索します。

`Java.Util.Locale`クラスと呼ばれる便利なメソッドが含まれています。`GetAvailableLocales()`です。 音声認識エンジンによってサポートされる言語のこの一覧は、インストールされている言語に対してテストできます。

「認識」言語の一覧を生成する作業は簡単です。 常にある既定の言語 (ユーザーに設定された言語を最初に自分のデバイスをセットアップしたとき)、この例では、 `List<string>` "Default"は、最初のパラメーターと、一覧の残りの部分は、の結果に応じて、自動的に入力されます、`textToSpeech.IsLanguageAvailable(locale)`です。

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

### <a name="step-3---setting-the-speed-and-pitch"></a>手順 3 - 速度と声の高さの設定

Android を変更することにより、音声のサウンドを変更するユーザーの許可、`SpeechRate`と`Pitch`(速度と、音声の色調の比率)。 これには、0 から 1 に両方の場合は 1 をされている"normal"音声認識でします。

### <a name="step-4---testing-and-loading-new-languages"></a>手順 4 - テストと、新しい言語の読み込み

これは、実行を使用して、`Intent`で解釈される結果を含む`OnActivityResult`です。 使用する音声からテキストの例とは異なり、`RecognizerIntent`として、`PutExtra`パラメーターを`Intent`、インテントを使用して、インストール、`Action`です。

次のコードを使用して Google からの新しい言語をインストールすることができます。 結果、`Activity`かどうか確認し、言語が必要な場合、場合は、ダウンロードが発生する指示を受けた後、言語をインストールします。

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

### <a name="step-5---the-ioninitlistener"></a>手順 5 -、IOnInitListener

テキスト読み上げ、インターフェイス メソッドに変換できるアクティビティの`OnInit`を実装するのには (これは、2 番目のパラメーターをインスタンス化に指定された、`TextToSpeech`クラス)。 これは、リスナーを初期化し、結果をテストします。

両方のリスナーをテストする必要があります`OperationResult.Success`と`OperationResult.Failure`最低限です。
次の例では、だけを示しています。

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

このガイドでは、テキスト読み上げ、音声認識を変換するテキストと、独自のアプリ内でそれらを含める方法の可能なメソッドを基本にきました。 すべて特定のケースをカバーしていませんが、中に音声を解釈する方法、新しい言語をインストールする方法、およびアプリの inclusivity を強化する方法の基本的な知識を今すぐが必要です。



## <a name="related-links"></a>関連リンク

- [Xamarin.Forms DependencyService](https://developer.xamarin.com/samples/UsingDependencyService/)
- [テキストを音声 (サンプル)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TextToSpeech)
- [音声入力 (サンプル)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SpeechToText)
- [Android.Speech 名前空間](https://developer.xamarin.com/api/namespace/Android.Speech/)
- [Android.Speech.Tts 名前空間](https://developer.xamarin.com/api/namespace/Android.Speech.Tts/)
