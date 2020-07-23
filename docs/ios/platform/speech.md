---
title: Xamarin の音声認識 (iOS)
description: この記事では、新しい Speech API について説明します。また、これを Xamarin. iOS アプリに実装して、音声認識を継続的に (ライブまたは録音されたオーディオストリームから) テキストに変換する方法を示します。
ms.prod: xamarin
ms.assetid: 64FED50A-6A28-4833-BEAE-63CEC9A09010
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: c4b818bcf3c4a5280c0280a2e28e2f59c65c8c81
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930222"
---
# <a name="speech-recognition-in-xamarinios"></a>Xamarin の音声認識 (iOS)

_この記事では、新しい Speech API について説明します。また、これを Xamarin. iOS アプリに実装して、音声認識を継続的に (ライブまたは録音されたオーディオストリームから) テキストに変換する方法を示します。_

IOS 10 の新機能である Apple は、音声認識 API をリリースしました。これにより、iOS アプリでは、音声認識の音声認識と、(ライブまたは録音されたオーディオストリームからの) 音声の議事録をテキストに変換できるようになります。

音声認識 API には、Apple による次のような機能と利点があります。

- 非常に正確
- アートの状態
- 使いやすい
- 速い
- 複数の言語をサポートする
- ユーザーのプライバシーを尊重する

## <a name="how-speech-recognition-works"></a>音声認識のしくみ

音声認識は、iOS アプリで、(API がサポートしている任意の話された言語で) ライブオーディオまたは録音済みオーディオを取得し、音声認識エンジンに渡すことによって実装されます。

[![音声認識のしくみ](speech-images/speech01.png)](speech-images/speech01.png#lightbox)

### <a name="keyboard-dictation"></a>キーボードディクテーション

ほとんどのユーザーは、iOS デバイスで音声認識を考えている場合、組み込みの Siri 音声アシスタントを使用しています。これは、iOS 5 のキーボードディクテーションと iPhone 4S を使用してリリースされています。

キーボードディクテーションは、TextKit (やなど) をサポートする任意のインターフェイス要素によってサポートされ、 `UITextField` `UITextArea` iOS 仮想キーボードの [ディクテーション] ボタン (space キーの左側) をクリックするとアクティブ化されます。

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

音声認識 API は、呼び出し元のアプリに、ユーザーが話しているときとほぼリアルタイムで結果を返します。これにより、テキストだけではなく翻訳の結果に関する詳細情報が提供されます。 次に例を示します。

- ユーザーが言った内容を複数の意味で解釈します。
- 個々の翻訳の信頼レベル。
- タイミング情報。

前述のように、audio for translation はライブフィードまたは事前記録されたソースから、iOS 10 でサポートされている50のすべての言語と言語で提供できます。

音声認識 API は、iOS 10 を実行するすべての iOS デバイスで使用できますが、ほとんどの場合、Apple のサーバーで大量の翻訳が行われるため、ライブインターネット接続が必要になります。 ただし、一部の新しい iOS デバイスでは、特定の言語のデバイス上での常時変換がサポートされています。

Apple には、現在のところ、特定の言語を翻訳できるかどうかを判断するための可用性 API が含まれています。 アプリは、インターネット接続自体を直接テストするのではなく、この API を使用する必要があります。

前述の「キーボードディクテーション」セクションで説明したように、音声認識では、インターネット経由で Apple のサーバー上のデータを転送および一時的に保存する必要があります。そのため、アプリは、ファイルにキーを含め、メソッドを呼び出すことによって、認識を実行するユーザーのアクセス許可を要求_する必要があり_ `NSSpeechRecognitionUsageDescription` `Info.plist` `SFSpeechRecognizer.RequestAuthorization` ます。 

音声認識に使用されるオーディオのソースに基づいて、アプリのファイルに対するその他の変更 `Info.plist` が必要になる場合があります。 詳細については、[セキュリティとプライバシーの強化](~/ios/app-fundamentals/security-privacy.md)に関するドキュメントを参照してください。

## <a name="adopting-speech-recognition-in-an-app"></a>アプリで音声認識を導入する

IOS アプリで音声認識を採用するには、次の4つの主要な手順を実行する必要があります。

- キーを使用して、アプリのファイルに使用方法の説明を入力し `Info.plist` `NSSpeechRecognitionUsageDescription` ます。 たとえば、カメラアプリには次のような説明が含まれている場合があり_ます。 "チーズという単語を言うだけで写真を撮ることができます。"_
- メソッドを呼び出して、 `SFSpeechRecognizer.RequestAuthorization` `NSSpeechRecognitionUsageDescription` ダイアログボックスでユーザーに音声認識アクセスを要求し、受け入れまたは拒否を許可する必要がある理由の説明を提示することによって、承認を要求します。
- 音声認識要求を作成します。
  - ディスク上の録音済みオーディオの場合は、クラスを使用し `SFSpeechURLRecognitionRequest` ます。
  - ライブオーディオ (またはメモリからのオーディオ) の場合は、クラスを使用し `SFSPeechAudioBufferRecognitionRequest` ます。
- 音声認識要求を音声認識エンジン () に渡して、 `SFSpeechRecognizer` 認識を開始します。 アプリは、返されたを使用して、 `SFSpeechRecognitionTask` 認識結果を監視および追跡できます。

これらの手順については、以下で詳しく説明します。

### <a name="providing-a-usage-description"></a>使用法の説明を指定する

ファイルに必要なキーを指定するには、 `NSSpeechRecognitionUsageDescription` `Info.plist` 次の手順を実行します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. ファイルをダブルクリックし `Info.plist` て開き、編集します。
2. **ソース**ビューに切り替えます。 

    [![ソースビュー](speech-images/speech02.png)](speech-images/speech02.png#lightbox)
3. [**新しいエントリの追加**] をクリックし、プロパティとして「」と入力し、 `NSSpeechRecognitionUsageDescription` **Property** `String` **使用法の説明**を**値**と**Type**して入力します。 次に例を示します。 

    [![NSSpeechRecognitionUsageDescription の追加](speech-images/speech03.png)](speech-images/speech03.png#lightbox)
4. アプリがライブオーディオの議事録処理を行う場合は、マイクの使用方法の説明も必要になります。 [**新しいエントリの追加**] をクリックし、プロパティとして「」と入力し、 `NSMicrophoneUsageDescription` **Property** `String` **使用法の説明**を**値**と**Type**して入力します。 次に例を示します。 

    [![Nsマイクロの操作の説明を追加しています](speech-images/speech04.png)](speech-images/speech04.png#lightbox)
5. 変更をファイルに保存します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. ファイルをダブルクリックし `Info.plist` て開き、編集します。
2. [**新しいエントリの追加**] をクリックし、プロパティとして「」と入力し、 `NSSpeechRecognitionUsageDescription` **Property** `String` **使用法の説明**を**値**と**Type**して入力します。 次に例を示します。 

    [![NSSpeechRecognitionUsageDescription の追加](speech-images/speech03w.png)](speech-images/speech03w.png#lightbox)
3. アプリがライブオーディオの議事録処理を行う場合は、マイクの使用方法の説明も必要になります。 [**新しいエントリの追加**] をクリックし、プロパティとして「」と入力し、 `NSMicrophoneUsageDescription` **Property** `String` **使用法の説明**を**値**と**Type**して入力します。 次に例を示します。 

    [![Nsマイクロの操作の説明を追加しています](speech-images/speech04w.png)](speech-images/speech04w.png#lightbox)
4. 変更をファイルに保存します。

-----

> [!IMPORTANT]
> 上記のいずれかのキーを指定し `Info.plist` ないと ( `NSSpeechRecognitionUsageDescription` または `NSMicrophoneUsageDescription` )、音声認識またはマイクを使用してライブオーディオにアクセスしようとすると、アプリが警告なしに失敗する可能性があります。

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

`RequestAuthorization`クラスのメソッドは、 `SFSpeechRecognizer` `NSSpeechRecognitionUsageDescription` ファイルのキーで開発者が提供した理由を使用して、音声認識にアクセスするためのアクセス許可をユーザーに要求します `Info.plist` 。

`SFSpeechRecognizerAuthorizationStatus`結果は、 `RequestAuthorization` ユーザーのアクセス許可に基づいてアクションを実行するために使用できる、メソッドのコールバックルーチンに返されます。 

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

まず、このコードを詳しく見て、音声認識エンジン () を作成しようと `SFSpeechRecognizer` します。 既定の言語で音声認識がサポートされていない場合 `null` は、が返され、関数が終了します。

音声認識エンジンが既定の言語で使用できる場合、アプリは、プロパティを使用して現在認識できるかどうかを確認し `Available` ます。 たとえば、デバイスにアクティブなインターネット接続がない場合は、認識を利用できないことがあります。

は、 `SFSpeechUrlRecognitionRequest` iOS デバイス上の事前記録されたファイルの場所から作成され、 `NSUrl` 音声認識エンジンに渡され、コールバックルーチンで処理されます。

コールバックが呼び出されたときに、によっ `NSError` `null` て処理される必要があるエラーが発生している場合。 音声認識は段階的に行われるため、コールバックルーチンは複数回呼び出される可能性があります。そのため、プロパティをテストして、 `SFSpeechRecognitionResult.Final` 変換が完了したかどうかを確認し、変換の最適なバージョンを書き出すことができ `BestTranscription` ます ()。

### <a name="recognizing-live-speech"></a>ライブ音声認識

アプリでライブ音声認識が必要な場合、このプロセスは、事前に記録された音声を認識することとよく似ています。 次に例を示します。

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

これは、認識要求を処理するためにに渡されるオーディオを、AV Foundation を使用して記録し `SFSpeechAudioBufferRecognitionRequest` ます。

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

認識タスクが開始され、ハンドルが認識タスク () に保持され `SFSpeechRecognitionTask` ます。

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

`RecognitionTask.Cancel`メモリとデバイスのプロセッサの両方を解放するためにユーザーが変換をキャンセルした場合は、を呼び出すことが重要です。

> [!IMPORTANT]
> `NSSpeechRecognitionUsageDescription`キーまたはキーを指定しないと、 `NSMicrophoneUsageDescription` `Info.plist` 音声認識またはマイク (ライブオーディオ) にアクセスしようとしたときにアプリが警告を表示せずに失敗する可能性が `var node = AudioEngine.InputNode;` あります ()。 詳細については、上記の「**使用方法の説明を指定する**」を参照してください。

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

## <a name="summary"></a>まとめ

この記事では、新しい Speech API について説明し、それを Xamarin. iOS アプリに実装する方法を示しました。これにより、音声認識を継続的に (ライブまたは録音されたオーディオストリームから) テキストに変換することができます。 

## <a name="related-links"></a>関連リンク

- [SpeakToMe (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-speaktome)
