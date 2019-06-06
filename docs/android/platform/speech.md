---
title: Android の音声認識
description: この記事では、非常に強力な Android.Speech 名前空間の基本的な使い方について説明します。 以来、Android を音声を認識し、テキストとして出力することができました。 比較的単純なプロセスです。 テキスト読み上げ、ただし、そのプロセスより複雑なだけでなくに音声認識エンジンが、アカウントにあるが、使用可能なと音声合成 (TTS) システムからインストールされている言語もします。
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/02/2018
ms.openlocfilehash: 3d4c29a7d206b826046fd1f79e0513e85ea57898
ms.sourcegitcommit: d3f48bfe72bfe03aca247d47bc64bfbfad1d8071
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/06/2019
ms.locfileid: "66740666"
---
# <a name="android-speech"></a>Android の音声認識

_この記事では、非常に強力な Android.Speech 名前空間の基本的な使い方について説明します。以来、Android を音声を認識し、テキストとして出力することができました。比較的単純なプロセスです。テキスト読み上げ、ただし、そのプロセスより複雑なだけでなくに音声認識エンジンが、アカウントにあるが、使用可能なと音声合成 (TTS) システムからインストールされている言語もします。_

## <a name="speech-overview"></a>音声認識の概要

人間の音声認識を「認識」し、enunciates 入力中、システム — 音声をテキスト、およびテキストを音声に変換: 増え領域内のモバイル開発では、デバイスとの自然なコミュニケーションの需要が増加します。 多くのインスタンスがあるテキストを音声に変換またはその逆の場合は、android アプリケーションに組み込むに非常に便利なツールは、機能を持つ場所。

たとえば、運転中に使用する携帯電話をクランプでは、ユーザーの自分のデバイスのオペレーティング手無料方法が必要です。 Android のさまざまなフォーム ファクターの大量-Android Wear のものが拡大し続けて含める Android デバイス (タブレットやノート パッド) を使用することが上に作成大きいフォーカス優れた TTS アプリケーション。

Google では、開発者は、デバイス (for the blind に設計されたソフトウェア) など「音声認識」のほとんどのインスタンスをカバーする Android.Speech 名前空間の Api の豊富なセットが用意されています。  名前空間には、テキストを音声に変換を許可する機能が含まれています。 `Android.Speech.Tts`、制御の数と同様に、変換を実行するために使用するエンジン`RecognizerIntent`%s がテキストに変換する音声認識を許可します。

機能は、音声理解しておくが、中には、使用されるハードウェアに基づく制限があります。 デバイスが正常に解釈があることすべてに使用可能なすべての言語で話される可能性が高いことはできません。

## <a name="requirements"></a>必要条件

このガイドは、マイクとスピーカーを持つデバイス以外で特別な要件はありません。

基本的な音声を解釈する Android デバイスは、使用、`Intent`対応する`OnActivityResult`します。
ただしでは、重要な認識、音声認識が認識されません-テキストが、解釈されました。 違いは重要です。

### <a name="the-difference-between-understanding-and-interpreting"></a>理解と解釈の違い

定義を簡単に理解するができるなら、トーンやコンテキストすの本当の意味を判別するのには だけを解釈するには、単語を受け取り、それらを別の形式で出力することです。

日常的なメッセージ交換で使用されている次の単純な例を検討してください。

<kbd>こんにちは、元気ですか。</kbd>

傾き (重点が置かれて特定の単語または単語の一部)、単純な質問です。 ただし、低速のペースが行に適用される場合は、リッスンしている人は、質問者が適切ではないことや、質問者が不満でないし、おそらく声援必要があります検出されます。 強調が配置されている場合に"are"、求める人は通常、応答に興味。

処理を非常に強力なオーディオなし、転換と人工知能 (AI) のある程度のコンテキストを理解するを使用して、ソフトウェア言われたについて理解を始める-最高の単純な電話を行うことができますが、音声をテキストに変換します。

## <a name="setting-up"></a>設定

音声認識システムを使用する前に、デバイスのマイクことを確認するは賢明は常にします。 インストールされているマイクがなくても、Kindle や Google のメモ帳でアプリを実行しようとしています。 ほとんどのポイントがありません。

次のコード例に示します、マイクが使用可能な場合とそうでない場合にクエリを実行するアラートを作成します。 マイクが使用できない場合この時点では、アクティビティを終了または無効にする、音声を記録する機能。

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

### <a name="creating-the-intent"></a>インテントを作成します。

音声認識システムの目的のインテントと呼ばれる特定の種類を使用して、`RecognizerIntent`します。 この目的の制御パラメーター、サイレント状態記録が経由で、追加の言語を認識し、出力と見なされるまで待機する時間を含むおよびに含める任意のテキストの数が多い、`Intent`のモーダル ダイアログ ボックスの命令のことを意味します。 このスニペットで`VOICE`は、`readonly int`で認識するため`OnActivityResult`します。

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

音声から解釈されるテキスト内で配信される、 `Intent`、アクティビティが完了したし、は経由でアクセス時に返される`GetStringArrayListExtra(RecognizerIntent.ExtraResults)`します。 返されます、`IList<string>`をインデックスを使用でき、発信者の意図で要求された言語の数に応じて、表示の (で指定されていると、 `RecognizerIntent.ExtraMaxResults`)。 任意のリストは表示されるデータがあることを確認することを確認します。

戻り値のリッスンしている場合、 `StartActivityForResult`、`OnActivityResult`メソッドを指定する必要があります。

次の例で`textBox`は、`TextBox`の内容が指示されている出力に使用します。 インタープリターのされ、そこから何らかの形にテキストを渡す均等に使わでした、アプリケーションは、テキストと、アプリケーションの別の部分に分岐を比較できます。

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

## <a name="text-to-speech"></a>テキスト読み上げ

テキストから音声をテキストの音声の逆では非常には、; 2 つの主要コンポーネントに依存しています音声合成エンジンは、デバイスにインストールされていると、言語がインストールされています。

既定値は、Android デバイスには、Google TTS サービスがインストールされていると、少なくとも 1 つの言語。 デバイスが最初を設定しすると、に基づいて時にデバイスは、これが確立される (たとえば、ドイツで設定する電話がインストールされますドイツ語の言語、アメリカ英語アメリカ合衆国のいずれかがある搭載されます)。

### <a name="step-1---instantiating-texttospeech"></a>手順 1 - TextToSpeech をインスタンス化します。

`TextToSpeech` 最大 3 つのパラメーターを受け取り、最初の 2 つは、3 つ目では必要で省略可能 (`AppContext`、 `IOnInitListener`、 `engine`)。 リスナーは、サービス、およびエラーのテストにバインドされる任意の数の使用可能な Android テキストに音声認識エンジンのエンジンに使用されます。 少なくとも、デバイスは Google のエンジンがあります。

### <a name="step-2---finding-the-languages-available"></a>手順 2 - 使用できる言語の検索

`Java.Util.Locale`クラスにはと呼ばれる便利なメソッドが含まれています`GetAvailableLocales()`します。 この音声認識エンジンによってサポートされている言語の一覧は、インストールされている言語に対してテストできます。

「認識」言語の一覧を生成する作業は簡単になります。 (ユーザー設定を最初に自分のデバイスをセットアップしたときの言語)、既定の言語が必ずこの例では、`List<string>`最初のパラメーターとして"Default"を持つの結果に応じて、一覧の残りの部分は塗りつぶされます、`textToSpeech.IsLanguageAvailable(locale)`します。

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

このコードは呼び出して[TextToSpeech.IsLanguageAvailable](https://developer.xamarin.com/api/member/Android.Speech.Tts.TextToSpeech.IsLanguageAvailable/p/Java.Util.Locale/)特定のロケールの言語パッケージが既にデバイス上に存在する場合をテストします。
このメソッドが戻る、 [LanguageAvailableResult](https://developer.xamarin.com/api/type/Android.Speech.Tts.LanguageAvailableResult/)、渡されたロケールの言語が使用できるかどうかを示します。 場合`LanguageAvailableResult`言語があることを示します`NotSupported`、(ダウンロード) の場合でも使用可能な音声パッケージはありませんし、その言語の。 場合`LanguageAvailableResult`に設定されている`MissingData`、手順 4. で、次に示すように、新しい言語パッケージをダウンロードする可能性があります。

### <a name="step-3---setting-the-speed-and-pitch"></a>手順 3 - 速度とピッチの設定

Android には、変更することによって、音声のサウンドを変更するユーザーができるように、`SpeechRate`と`Pitch`(の速度と、音声の声のトーン レート)。 これには 0 から 1、"normal"の音声認識が両方の 1 の中で。

### <a name="step-4---testing-and-loading-new-languages"></a>手順 4 - テストと新しい言語の読み込み

使用して実行する新しい言語のダウンロード、`Intent`します。 この目的の結果により、 [OnActivityResult](https://developer.xamarin.com/api/member/Android.App.Activity.OnActivityResult/)メソッドを呼び出します。 音声からテキストの例とは異なり (使用する、 [RecognizerIntent](https://developer.xamarin.com/api/type/Android.Speech.RecognizerIntent/)として、`PutExtra`パラメーターを`Intent`)、テストおよびロード`Intent`は`Action`-ベースします。

-   [TextToSpeech.Engine.ActionCheckTtsData](https://developer.xamarin.com/api/field/Android.Speech.Tts.TextToSpeech+Engine.ActionCheckTtsData/) &ndash;プラットフォームからアクティビティが開始される`TextToSpeech`エンジンを適切にインストールし、デバイス上の言語リソースの可用性を確認します。

-   [TextToSpeech.Engine.ActionInstallTtsData](https://developer.xamarin.com/api/field/Android.Speech.Tts.TextToSpeech+Engine.ActionInstallTtsData/) &ndash;必要な言語をダウンロードするユーザーに求めるアクティビティを開始します。

次のコード例では、これらのアクションを使用して、言語リソースをテストし、新しい言語をダウンロードする方法を示します。

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

`TextToSpeech.Engine.ActionCheckTtsData` 言語リソースの可用性をテストします。 `OnActivityResult` このテストが完了したときに呼び出されます。 言語リソースをダウンロードする必要がある場合`OnActivityResult`起動、`TextToSpeech.Engine.ActionInstallTtsData`ユーザーが必要な言語をダウンロードできるアクティビティを開始するアクション。 注この`OnActivityResult`実装をチェックしません、`Result`コードのため、この簡略化された例では、決定が行われている言語パッケージをダウンロードする必要があること。

`TextToSpeech.Engine.ActionInstallTtsData`操作エラーの原因、 **Google 音声合成の音声データ**をダウンロードする言語を選択するために、ユーザーに表示するアクティビティ。

![Google の音声合成の音声データ アクティビティ](speech-images/01-google-tts-voice-data.png)

例として、ユーザーがフランス語を選択し、フランス語の音声データをダウンロードするダウンロード アイコンをクリックして可能性があります。

![フランス語の言語のダウンロードの例](speech-images/02-selecting-french.png)

ダウンロードが完了した後、このデータのインストールは自動的に行われます。


### <a name="step-5---the-ioninitlistener"></a>手順 5 -、IOnInitListener

テキストを音声、インターフェイス メソッドに変換できるアクティビティの`OnInit`を実装しなければなりません (これは、2 番目のパラメーターのインスタンス化に指定された、`TextToSpeech`クラス)。 これは、リスナーを初期化し、結果をテストします。

両方のリスナーをテストする必要があります`OperationResult.Success`と`OperationResult.Failure`には、少なくともします。
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

このガイドでは、テキストから音声、音声に変換するテキストを独自のアプリ内でそれらを含める方法の使用可能な方法の基本にきました。 すべてのケースれてはいませんが、中に音声を解釈する方法、新しい言語をインストールする方法、およびアプリの inclusivity を向上させる方法の基本を理解を今すぐが必要です。



## <a name="related-links"></a>関連リンク

- [Xamarin.Forms DependencyService](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [テキストを音声 (サンプル)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TextToSpeech)
- [音声からテキスト (サンプル)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SpeechToText)
- [Android.Speech の名前空間](https://developer.xamarin.com/api/namespace/Android.Speech/)
- [Android.Speech.Tts 名前空間](https://developer.xamarin.com/api/namespace/Android.Speech.Tts/)
