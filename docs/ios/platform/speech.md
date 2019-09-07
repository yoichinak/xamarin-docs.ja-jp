---
title: Xamarin の音声認識 (iOS)
description: この記事では、新しい Speech API について説明します。また、これを Xamarin. iOS アプリに実装して、音声認識を継続的に (ライブまたは録音されたオーディオストリームから) テキストに変換する方法を示します。
ms.prod: xamarin
ms.assetid: 64FED50A-6A28-4833-BEAE-63CEC9A09010
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 66bea7d2a9660018c7cec9b7bafeadafd5029ed9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769421"
---
# <a name="speech-recognition-in-xamarinios"></a>Xamarin の音声認識 (iOS)

_この記事では、新しい Speech API について説明します。また、これを Xamarin. iOS アプリに実装して、音声認識を継続的に (ライブまたは録音されたオーディオストリームから) テキストに変換する方法を示します。_

IOS 10 の新機能である Apple は、音声認識 API をリリースしました。これにより、iOS アプリでは、音声認識の音声認識と、(ライブまたは録音されたオーディオストリームからの) 音声の議事録をテキストに変換できるようになります。

音声認識 API には、Apple による次のような機能と利点があります。

- 非常に正確
- アートの状態
- 使いやすい
- Fast
- 複数の言語をサポートする
- ユーザーのプライバシーを尊重する

## <a name="how-speech-recognition-works"></a>音声認識のしくみ

音声認識は、iOS アプリで、(API がサポートしている任意の話された言語で) ライブオーディオまたは録音済みオーディオを取得し、音声認識エンジンに渡すことによって実装されます。

[![](speech-images/speech01.png "音声認識のしくみ")](speech-images/speech01.png#lightbox)

### <a name="keyboard-dictation"></a>キーボードディクテーション

ほとんどのユーザーは、iOS デバイスで音声認識を考えている場合、組み込みの Siri 音声アシスタントを使用しています。これは、iOS 5 のキーボードディクテーションと iPhone 4S を使用してリリースされています。

キーボードディクテーションは、textkit ( `UITextField`や`UITextArea`など) をサポートする任意のインターフェイス要素によってサポートされ、iOS 仮想キーボードの [ディクテーション] ボタン (space キーの左側) をクリックするとアクティブ化されます。

Apple は、次のキーボードディクテーション統計 (2011 以降に収集) をリリースしました。

- キーボードディクテーションは、iOS 5 でリリースされたため、広く使用されています。
- 約65000のアプリで1日に使用されます。
- サードパーティのアプリでは、すべての iOS ディクテーションの3分の1が実行されます。

キーボードディクテーションは、アプリの UI デザインで TextKit インターフェイス要素を使用するのではなく、開発者の作業を必要としないため、非常に簡単に使用できます。 キーボードのディクテーションでは、アプリを使用する前に、アプリから特別な特権を要求する必要がないという利点もあります。

新しい音声認識 Api を使用するアプリでは、ユーザーがアクセスするための特殊なアクセス許可が必要です。音声認識では、Apple のサーバー上のデータの伝送と一時的な保存が必要になるためです。 詳細については、[セキュリティとプライバシーの強化](~/ios/app-fundamentals/security-privacy.md)に関するドキュメントを参照してください。

キーボードディクテーションは簡単に実装できますが、いくつかの制限事項と欠点があります。

- テキスト入力フィールドとキーボードの表示を使用する必要があります。
- ライブオーディオ入力のみで動作し、アプリはオーディオ記録プロセスを制御できません。
- ユーザーの音声を解釈するために使用される言語を制御することはできません。
- ユーザーがディクテーションボタンを使用できるかどうかをアプリが認識する方法はありません。
- アプリでオーディオ記録プロセスをカスタマイズすることはできません。
- これには、タイミングや信頼度などの情報を持たない非常に浅い結果セットが用意されています。

### <a name="speech-recognition-api"></a>音声認識 API

IOS 10 の新機能である Apple は、音声認識 API をリリースしました。これにより、iOS アプリで音声認識を実装するためのより強力な方法が提供されます。 この API は、Apple が Siri とキーボードディクテーションの両方の機能を強化するために使用するものと同じものであり、アートの精度の状態をすばやく説明することができます。

音声認識 API によって提供される結果は、アプリが個人のユーザーデータを収集したりアクセスしたりすることなく、個々のユーザーに対して透過的にカスタマイズされます。

音声認識 API は、呼び出し元のアプリに、ユーザーが話しているときとほぼリアルタイムで結果を返します。これにより、テキストだけではなく翻訳の結果に関する詳細情報が提供されます。 不足している機能には次が含まれます。

- ユーザーが言った内容を複数の意味で解釈します。
- 個々の翻訳の信頼レベル。
- タイミング情報。

前述のように、audio for translation はライブフィードまたは事前記録されたソースから、iOS 10 でサポートされている50のすべての言語と言語で提供できます。

音声認識 API は、iOS 10 を実行するすべての iOS デバイスで使用できますが、ほとんどの場合、Apple のサーバーで大量の翻訳が行われるため、ライブインターネット接続が必要になります。 ただし、一部の新しい iOS デバイスでは、特定の言語のデバイス上での常時変換がサポートされています。

Apple には、現在のところ、特定の言語を翻訳できるかどうかを判断するための可用性 API が含まれています。 アプリは、インターネット接続自体を直接テストするのではなく、この API を使用する必要があります。

前述の「キーボードのディクテーション」セクションで説明したように、音声認識では、インターネット経由で Apple のサーバー上のデータを転送および一時的に保存する必要が_あり_ます。そのため、アプリは、ファイル内の`SFSpeechRecognizer.RequestAuthorization`キー。メソッドを呼び出します。 `NSSpeechRecognitionUsageDescription` `Info.plist` 

音声認識に使用されるオーディオのソースに基づいて、アプリの`Info.plist`ファイルに対するその他の変更が必要になる場合があります。 詳細については、[セキュリティとプライバシーの強化](~/ios/app-fundamentals/security-privacy.md)に関するドキュメントを参照してください。

## <a name="adopting-speech-recognition-in-an-app"></a>アプリで音声認識を導入する

IOS アプリで音声認識を採用するには、次の4つの主要な手順を実行する必要があります。

- キーを使用して、アプリの`Info.plist`ファイルに使用方法の説明を入力します。 `NSSpeechRecognitionUsageDescription` たとえば、カメラアプリには次のような説明が含まれている場合があり_ます。 "チーズという単語を言うだけで写真を撮ることができます。"_
- `SFSpeechRecognizer.RequestAuthorization`メソッドを呼び出して、ダイアログボックスでユーザーに音声認識アクセスを`NSSpeechRecognitionUsageDescription`要求し、受け入れまたは拒否を許可する必要がある理由の説明を提示することによって、承認を要求します。
- 音声認識要求を作成します。
  - ディスク上の録音済みオーディオの場合は、 `SFSpeechURLRecognitionRequest`クラスを使用します。
  - ライブオーディオ (またはメモリからのオーディオ) の場合`SFSPeechAudioBufferRecognitionRequest`は、クラスを使用します。
- 音声認識要求を音声認識エンジン (`SFSpeechRecognizer`) に渡して、認識を開始します。 アプリは、返さ`SFSpeechRecognitionTask`れたを使用して、認識結果を監視および追跡できます。

これらの手順については、以下で詳しく説明します。

### <a name="providing-a-usage-description"></a>使用法の説明を指定する

ファイルに必要な`NSSpeechRecognitionUsageDescription`キーを指定するには、次の手順を実行します。 `Info.plist`

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. `Info.plist`ファイルをダブルクリックして開き、編集します。
2. **ソース**ビューに切り替えます。 

    [![](speech-images/speech02.png "ソースビュー")](speech-images/speech02.png#lightbox)
3. **[新しいエントリの追加]** を`NSSpeechRecognitionUsageDescription`クリックし、プロパティ`String`として **「** 」と入力し、**使用法の説明**を**値**として入力します。 例えば: 

    [![](speech-images/speech03.png "NSSpeechRecognitionUsageDescription の追加")](speech-images/speech03.png#lightbox)
4. アプリがライブオーディオの議事録処理を行う場合は、マイクの使用方法の説明も必要になります。 **[新しいエントリの追加]** を`NSMicrophoneUsageDescription`クリックし、プロパティ`String`として **「** 」と入力し、**使用法の説明**を**値**として入力します。 例えば: 

    [![](speech-images/speech04.png "Nsマイクロの操作の説明を追加しています")](speech-images/speech04.png#lightbox)
5. 変更内容をファイルに保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. `Info.plist`ファイルをダブルクリックして開き、編集します。
2. **[新しいエントリの追加]** を`NSSpeechRecognitionUsageDescription`クリックし、プロパティ`String`として **「** 」と入力し、**使用法の説明**を**値**として入力します。 例えば: 

    [![](speech-images/speech03w.png "NSSpeechRecognitionUsageDescription の追加")](speech-images/speech03w.png#lightbox)
3. アプリがライブオーディオの議事録処理を行う場合は、マイクの使用方法の説明も必要になります。 **[新しいエントリの追加]** を`NSMicrophoneUsageDescription`クリックし、プロパティ`String`として **「** 」と入力し、**使用法の説明**を**値**として入力します。 例えば: 

    [![](speech-images/speech04w.png "Nsマイクロの操作の説明を追加しています")](speech-images/speech04w.png#lightbox)
4. 変更内容をファイルに保存します。

-----

> [!IMPORTANT]
> 上記`Info.plist`のいずれかのキーを指定し`NSSpeechRecognitionUsageDescription`ない`NSMicrophoneUsageDescription`と (または)、音声認識またはマイクを使用してライブオーディオにアクセスしようとすると、アプリが警告なしに失敗する可能性があります。

### <a name="requesting-authorization"></a>承認の要求

アプリが音声認識にアクセスできるようにするために必要なユーザー承認を要求するには、メインビューコントローラークラスを編集し、次のコードを追加します。

```csharp
using System;
using UIKit;
using Speech;

namespace MonkeyTalk
{
    public partial class ViewController : UIViewController
    {
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Request user authorization
            SFSpeechRecognizer.RequestAuthorization ((SFSpeechRecognizerAuthorizationStatus status) => {
                // Take action based on status
                switch (status) {
                case SFSpeechRecognizerAuthorizationStatus.Authorized:
                    // User has approved speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Denied:
                    // User has declined speech recognition
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.NotDetermined:
                    // Waiting on approval
                    ...
                    break;
                case SFSpeechRecognizerAuthorizationStatus.Restricted:
                    // The device is not permitted
                    ...
                    break;
                }
            });
        }
    }
}
```

クラスのメソッドは`RequestAuthorization` 、 `Info.plist`ファイルの`NSSpeechRecognitionUsageDescription`キーで開発者が提供した理由を使用して、音声認識にアクセスするためのアクセス許可をユーザーに要求します。 `SFSpeechRecognizer`

結果は、ユーザーのアクセス`RequestAuthorization`許可に基づいてアクションを実行するために使用できる、メソッドのコールバックルーチンに返されます。 `SFSpeechRecognizerAuthorizationStatus` 

> [!IMPORTANT]
> Apple は、このアクセス許可を要求する前に、音声認識を必要とするアプリのアクションをユーザーが開始するまで待機することを提案します。

### <a name="recognizing-pre-recorded-speech"></a>事前記録された音声を認識する

アプリで録音済みの WAV または MP3 ファイルから音声認識を希望する場合は、次のコードを使用できます。

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
...

public void RecognizeFile (NSUrl url)
{
    // Access new recognizer
    var recognizer = new SFSpeechRecognizer ();

    // Is the default language supported?
    if (recognizer == null) {
        // No, return to caller
        return;
    }

    // Is recognition available?
    if (!recognizer.Available) {
        // No, return to caller
        return;
    }

    // Create recognition task and start recognition
    var request = new SFSpeechUrlRecognitionRequest (url);
    recognizer.GetRecognitionTask (request, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said, \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}
```

まず、このコードを詳しく見て、音声認識エンジン (`SFSpeechRecognizer`) を作成しようとします。 既定の言語で音声認識がサポートされ`null`ていない場合は、が返され、関数が終了します。

音声認識エンジンが既定の言語で使用できる場合、アプリは、 `Available`プロパティを使用して現在認識できるかどうかを確認します。 たとえば、デバイスにアクティブなインターネット接続がない場合は、認識を利用できないことがあります。

は`SFSpeechUrlRecognitionRequest` 、iOS デバイス上`NSUrl`の事前記録されたファイルの場所から作成され、音声認識エンジンに渡され、コールバックルーチンで処理されます。

コールバックが呼び出されたときに`NSError` 、 `null`によって処理される必要があるエラーが発生している場合。 音声認識は段階的に行われるため、コールバックルーチンは複数回呼び出される可能性`SFSpeechRecognitionResult.Final`があります。そのため、プロパティをテストして、変換が完了したかどう`BestTranscription`かを確認し、変換の最適なバージョンを書き出すことができます ()。

### <a name="recognizing-live-speech"></a>ライブ音声認識

アプリでライブ音声認識が必要な場合、このプロセスは、事前に記録された音声を認識することとよく似ています。 例えば:

```csharp
using System;
using UIKit;
using Speech;
using Foundation;
using AVFoundation;
...

#region Private Variables
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
#endregion
...

public void StartRecording ()
{
    // Setup audio session
    var node = AudioEngine.InputNode;
    var recordingFormat = node.GetBusOutputFormat (0);
    node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
        // Append buffer to recognition request
        LiveSpeechRequest.Append (buffer);
    });

    // Start recording
    AudioEngine.Prepare ();
    NSError error;
    AudioEngine.StartAndReturnError (out error);

    // Did recording start?
    if (error != null) {
        // Handle error and return
        ...
        return;
    }

    // Start recognition
    RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
        // Was there an error?
        if (err != null) {
            // Handle error
            ...
        } else {
            // Is this the final translation?
            if (result.Final) {
                Console.WriteLine ("You said \"{0}\".", result.BestTranscription.FormattedString);
            }
        }
    });
}

public void StopRecording ()
{
    AudioEngine.Stop ();
    LiveSpeechRequest.EndAudio ();
}

public void CancelRecording ()
{
    AudioEngine.Stop ();
    RecognitionTask.Cancel ();
}
```

このコードを詳しく見ると、認識プロセスを処理するためのプライベート変数がいくつか作成されます。

```csharp
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
```

これは、認識要求を処理するためにに渡される`SFSpeechAudioBufferRecognitionRequest`オーディオを、AV Foundation を使用して記録します。

```csharp
var node = AudioEngine.InputNode;
var recordingFormat = node.GetBusOutputFormat (0);
node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
    // Append buffer to recognition request
    LiveSpeechRequest.Append (buffer);
});
```

アプリは記録を開始しようとしますが、記録を開始できない場合はエラーが処理されます。

```csharp
AudioEngine.Prepare ();
NSError error;
AudioEngine.StartAndReturnError (out error);

// Did recording start?
if (error != null) {
    // Handle error and return
    ...
    return;
}
```

認識タスクが開始され、ハンドルが認識タスク (`SFSpeechRecognitionTask`) に保持されます。

```csharp
RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
    ...
});
```

コールバックは、事前に記録された音声に対して、上記と同様の方法で使用されます。

記録がユーザーによって stoped された場合、オーディオエンジンと音声認識の両方の要求が通知されます。

```csharp
AudioEngine.Stop ();
LiveSpeechRequest.EndAudio ();
```

ユーザーが認識をキャンセルすると、オーディオエンジンと認識タスクに通知されます。

```csharp
AudioEngine.Stop ();
RecognitionTask.Cancel ();
```

メモリとデバイスのプロセッサ`RecognitionTask.Cancel`の両方を解放するためにユーザーが変換をキャンセルした場合は、を呼び出すことが重要です。

> [!IMPORTANT]
> キー `NSSpeechRecognitionUsageDescription`または`NSMicrophoneUsageDescription`キーを指定しないと、音声認識またはマイク (ライブオーディオ) にアクセスしようとしたときにアプリ`var node = AudioEngine.InputNode;`が警告を表示せずに失敗する可能性があります ()。 `Info.plist` 詳細については、上記の「**使用方法の説明を指定する**」を参照してください。

## <a name="speech-recognition-limits"></a>音声認識の制限

IOS アプリで音声認識を使用する場合、Apple では次の制限があります。

- 音声認識はすべてのアプリで無料ですが、使用量は無制限ではありません。
  - 個々の iOS デバイスには、1日に実行できる認識の数が制限されています。
  - アプリは、1日あたりの要求に応じてグローバルに調整されます。
- 音声認識のネットワーク接続と使用率の制限のエラーを処理できるように、アプリを準備する必要があります。
- 音声認識を使用すると、ユーザーの iOS デバイスのバッテリドレインとネットワークトラフィックの両方に対して高いコストがかかることがあります。そのため、Apple では、音声の最大値として約1分間の厳密なオーディオ期間制限が設けられています。

アプリのレート調整制限に定期的に達している場合、Apple は開発者に連絡するように求めます。

## <a name="privacy-and-usability-considerations"></a>プライバシーとユーザビリティに関する考慮事項

IOS アプリで音声認識を使用する場合、Apple は透過的であり、ユーザーのプライバシーを尊重するために次の提案を行います。

- ユーザーの音声を録音する場合は、アプリのユーザーインターフェイスで記録が行われていることを明確に確認してください。 たとえば、アプリは "録音" サウンドを再生し、記録インジケーターを表示する場合があります。
- パスワード、正常性データ、財務情報などの重要なユーザー情報には音声認識を使用しないでください。
- 機能_する前_に認識結果を表示します。 これにより、アプリが何を行っているかについてのフィードバックだけでなく、ユーザーが作成時に認識エラーを処理できるようになります。

## <a name="summary"></a>Summary

この記事では、新しい Speech API について説明し、それを Xamarin. iOS アプリに実装する方法を示しました。これにより、音声認識を継続的に (ライブまたは録音されたオーディオストリームから) テキストに変換することができます。 

## <a name="related-links"></a>関連リンク

- [SpeakToMe (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-speaktome)
