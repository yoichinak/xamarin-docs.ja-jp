---
title: Xamarin.iOS での音声認識
description: この記事では、新しい Speech API を表示し、Xamarin.iOS アプリを継続的な音声認識をサポートし、(生または録画のオーディオ ストリーム) からの音声をテキストに議事録の作成での実装方法を示しています。
ms.prod: xamarin
ms.assetid: 64FED50A-6A28-4833-BEAE-63CEC9A09010
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: c1f488213f9b3be945fd98e09f630c243d0b0d62
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122765"
---
# <a name="speech-recognition-in-xamarinios"></a>Xamarin.iOS での音声認識

_この記事では、新しい Speech API を表示し、Xamarin.iOS アプリを継続的な音声認識をサポートし、(生または録画のオーディオ ストリーム) からの音声をテキストに議事録の作成での実装方法を示しています。_

新しい Apple ios 10、iOS アプリに許可する継続的な音声認識をサポートし、議事録の作成 (生または録画のオーディオ ストリーム) からの音声をテキストに音声認識 API をリリースが。

に従って、Apple、音声認識 API は、次の機能と利点。

- 正確な
- 最先端
- 使いやすい
- Fast
- 複数の言語をサポートしています
- ユーザー プライバシーの保護に努めています

## <a name="how-speech-recognition-works"></a>音声認識のしくみ

音声認識は、(API をサポートする音声の言語のいずれか) でのオーディオのライブまたは録音済みのいずれかを取得し、話された単語のプレーン テキストの議事録を返す音声認識エンジンに渡すことによって、iOS アプリケーションに実装されます。

[![](speech-images/speech01.png "音声認識のしくみ")](speech-images/speech01.png#lightbox)

### <a name="keyboard-dictation"></a>キーボード ディクテーション

ほとんどのユーザーは、iOS デバイスで音声認識と考えるとときに、iOS 5 で iPhone 4 秒にキーボード ディクテーションと共にリリースされた組み込み Siri 音声アシスタントと考えます。

TextKit をサポートする任意のインターフェイス要素でキーボード ディクテーションがサポートされています (など`UITextField`または`UITextArea`) し、ユーザーが iOS 仮想キーボード (直接、スペース バーの左側) にディクテーション ボタンをクリックしてアクティブにします。

Apple は、次のキーボード ディクテーションの統計情報 (2011 年から収集) をリリースしました。

- IOS 5 のリリース後、キーボード ディクテーションを広く使用されています。
- 65,000 を約 1 日あたりアプリケーションで使用します。
- すべての iOS の 3 つ目の詳細については、ディクテーションはサード パーティのアプリで実行されます。

キーボード ディクテーション モードは、非常に使いやすく TextKit インターフェイス要素を使用して、アプリの UI の設計に以外の開発者の側での作業は必要ありません。 キーボード ディクテーションを使用するアプリからの特別な権限要求を必要としないという利点があります。

新しい音声認識 Api を使用するアプリには、音声認識は、送信および Apple のサーバー上のデータの一時的なストレージが必要なため、ユーザーが許可する特別なアクセス許可が必要です。 参照してください、[セキュリティおよびプライバシーの強化機能](~/ios/app-fundamentals/security-privacy.md)詳細についてはドキュメントです。

キーボード ディクテーションは簡単に実装できますは、いくつかの制限事項やデメリットが付属しては。

- テキスト入力フィールドを使用して、キーボードの表示が必要です。
- ライブのオーディオ入力のみで動作するされ、アプリには、オーディオ録音プロセスが細かく制御はありません。
- ユーザーの音声を解釈するために使用する言語の制御ではありません。
- ディクテーション ボタンがユーザーにも使用可能なかどうかに、アプリの方法はありません。
- アプリでは、オーディオ録音プロセスをカスタマイズできません。
- 信頼性とタイミングなどの情報が不足している非常に簡易結果セットを提供します。

### <a name="speech-recognition-api"></a>音声認識 API

新しい ios 10 で、Apple の音声認識を実装するために iOS アプリのより強力な方法を提供する音声認識 API がリリースされました。 この API は、Apple は Siri とキーボード ディクテーションの両方の電源を使用しているものと同じとは、最新の精度で高速の議事録を提供できます。

音声認識 API によって提供される結果は、アプリを収集したり、プライベート ユーザー データにアクセスすることがなく個別のユーザーに透過的にカスタマイズされます。

Speech Recognition API では、呼び出し元のアプリに結果をほぼリアルタイムように、ユーザーが言うと、変換の結果に関する詳細については単なるテキストよりも提供を提供します。 次の設定があります。

- どのようなユーザーの複数の解釈と呼ばれます。
- 個々 の翻訳の信頼レベル。
- タイミング情報。

前述のように、翻訳のオーディオは、ライブのフィードまたは記録済みのソースからでを 50 以上の言語および iOS 10 でサポートされている言語のいずれかに指定できます。

Speech Recognition API では、iOS 10 を実行しているすべての iOS デバイスで使用できるし、翻訳の大部分は、Apple のサーバー上の場所をかかるため、ほとんどの場合のライブ インターネット接続を必要があります。 ただし、いくつかで常にデバイスをサポートする新しい iOS、言語固有のデバイスに変換します。

Apple には、特定の言語が、現在の変換に使用できるかを判断するための可用性 API が含まれています。 アプリでは、直接自体のインターネット接続のテストではなく、この API を使用する必要があります。

キーボード ディクテーションのセクションで上記のよう音声認識が必要です、転送と Apple のサーバー上のデータの一時的なストレージと同様、アプリ、インターネット経由で_する必要があります_を実行するユーザーのアクセス許可を要求認識を含めることによって、`NSSpeechRecognitionUsageDescription`キーでその`Info.plist`ファイルと呼び出し、`SFSpeechRecognizer.RequestAuthorization`メソッド。 

音声認識に使用されているオーディオのソースに基づき、他のアプリの変更`Info.plist`ファイルが必要な可能性があります。 参照してください、[セキュリティおよびプライバシーの強化機能](~/ios/app-fundamentals/security-privacy.md)詳細についてはドキュメントです。

## <a name="adopting-speech-recognition-in-an-app"></a>アプリでの音声認識を採用します。

IOS アプリでの音声認識を採用する開発者が行う必要のある 4 つの主要な手順があります。

- アプリの利用状況の説明を提供`Info.plist`ファイルを使用して、`NSSpeechRecognitionUsageDescription`キー。 たとえば、カメラ アプリは、次の説明を含めることが _「これにより、'チーズ' という単語をするだけで写真を撮影することです」。_
- 呼び出すことによって、承認を要求、`SFSpeechRecognizer.RequestAuthorization`説明を表示する方法 (で提供される、`NSSpeechRecognitionUsageDescription`上記のキー)、アプリが音声を希望する理由の認識 ダイアログ ボックスで、ユーザーへのアクセスし、承諾または辞退するようにします。
- 音声認識の要求を作成します。
    * ディスクの記録済みのオーディオ、使用、`SFSpeechURLRecognitionRequest`クラス。
    * ライブ オーディオ (または、メモリからオーディオ) を使用して、`SFSPeechAudioBufferRecognitionRequest`クラス。
- 音声認識に音声認識の要求を渡す (`SFSpeechRecognizer`) 認識を開始します。 アプリが、返されたを保持できます必要に応じて`SFSpeechRecognitionTask`を監視し、認識結果を追跡します。

次の手順は、以下で詳しく取り上げます。

### <a name="providing-a-usage-description"></a>利用状況の説明を提供します。

必要なため`NSSpeechRecognitionUsageDescription`キー、`Info.plist`ファイルで、次の操作を行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. ダブルクリックして、`Info.plist`ファイルを開き、編集します。
2. 切り替えて、**ソース**ビュー。 

    [![](speech-images/speech02.png "ソース ビュー")](speech-images/speech02.png#lightbox)
3. をクリックして**新しいエントリの追加**、入力`NSSpeechRecognitionUsageDescription`の**プロパティ**、`String`の**型**と**利用状況の説明**として、**値**します。 例えば: 

    [![](speech-images/speech03.png "NSSpeechRecognitionUsageDescription を追加します。")](speech-images/speech03.png#lightbox)
4. アプリがライブ オーディオ トラン スクリプトを処理する場合が、マイクの利用状況の説明も必要です。 をクリックして**新しいエントリの追加**、入力`NSMicrophoneUsageDescription`の**プロパティ**、`String`の**型**と**利用状況の説明**として、**値**します。 例えば: 

    [![](speech-images/speech04.png "NSMicrophoneUsageDescription を追加します。")](speech-images/speech04.png#lightbox)
4. 変更内容をファイルに保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. ダブルクリックして、`Info.plist`ファイルを開き、編集します。
3. をクリックして**新しいエントリの追加**、入力`NSSpeechRecognitionUsageDescription`の**プロパティ**、`String`の**型**と**利用状況の説明**として、**値**します。 例えば: 

    [![](speech-images/speech03w.png "NSSpeechRecognitionUsageDescription を追加します。")](speech-images/speech03w.png#lightbox)
4. アプリがライブ オーディオ トラン スクリプトを処理する場合が、マイクの利用状況の説明も必要です。 をクリックして**新しいエントリの追加**、入力`NSMicrophoneUsageDescription`の**プロパティ**、`String`の**型**と**利用状況の説明**として、**値**します。 例えば: 

    [![](speech-images/speech04w.png "NSMicrophoneUsageDescription を追加します。")](speech-images/speech04w.png#lightbox)
4. 変更内容をファイルに保存します。

-----

> [!IMPORTANT]
> 上記のいずれかの提供に失敗する`Info.plist`キー (`NSSpeechRecognitionUsageDescription`または`NSMicrophoneUsageDescription`) 音声認識またはライブ オーディオ、マイクにアクセスしようとしたときに警告なしに失敗しているアプリで発生することができます。




### <a name="requesting-authorization"></a>承認を要求します。

音声認識にアクセスするアプリを許可する必要なユーザー認証を要求するには、メイン ビュー コント ローラー クラスを編集し、次のコードを追加します。

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

`RequestAuthorization`のメソッド、`SFSpeechRecognizer`クラスは、ユーザーからアクセス許可をアクセス音声認識を使用する理由で、開発者が提供する要求は、`NSSpeechRecognitionUsageDescription`のキー、`Info.plist`ファイル。

A`SFSpeechRecognizerAuthorizationStatus`に結果が返されます、`RequestAuthorization`ユーザーのアクセス許可に基づいてアクションを実行するために使用できるメソッドのコールバック ルーチン。 

> [!IMPORTANT]
> Apple では、ユーザーがこのアクセス許可を要求する前に、音声認識を必要とするアプリでアクションを開始するまで待機しているお勧めします。

### <a name="recognizing-pre-recorded-speech"></a>録音済み音声を認識します。

アプリは、記録済みの WAV または MP3 ファイルからの音声を認識する必要がある場合は、次のコードを使用できます。

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

このコードの詳細を見ると、最初に、作成しようと音声認識エンジン (`SFSpeechRecognizer`)。 音声認識では、既定の言語がサポートされていない場合`null`が返されると、関数が終了します。

アプリが認識を使用するために現在使用できることを確認するチェック音声認識エンジンが既定の言語の使用可能な場合、`Available`プロパティ。 たとえば、認識できない可能性があります、デバイスはアクティブなインターネット接続を持っていない場合。

A`SFSpeechUrlRecognitionRequest`から作成された、 `NSUrl` iOS デバイスに記録済みのファイルの場所がコールバック ルーチンで処理する音声認識エンジンに渡されます。

コールバックが呼び出された場合場合、`NSError`いない`null`処理する必要がありますエラーが発生しました。 音声認識は段階的に行われるため、コールバック ルーチンを呼び出すことは 1 回のためより詳細`SFSpeechRecognitionResult.Final`プロパティがテストされ、変換が完了し、翻訳の最適なバージョンが書き出さかどうかを参照してください (`BestTranscription`)。

### <a name="recognizing-live-speech"></a>ライブ音声を認識します。

アプリは、ライブの音声を認識する必要がある場合、プロセスは録音済み音声の認識によく似ています。 例えば:

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

このコードの詳細を見ると、認識プロセスを処理するいくつかのプライベート変数を作成します。

```csharp
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
```

渡されるオーディオの録音を AV Foundation を使用して、`SFSpeechAudioBufferRecognitionRequest`認識要求を処理するために。

```csharp
var node = AudioEngine.InputNode;
var recordingFormat = node.GetBusOutputFormat (0);
node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
    // Append buffer to recognition request
    LiveSpeechRequest.Append (buffer);
});
```

アプリがの記録を開始しようとして、録画を開始できない場合、エラーの処理します。

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

認識タスクが開始され、ハンドルが認識タスクに保持されます (`SFSpeechRecognitionTask`)。

```csharp
RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
    ...
});
```

コールバックは、上記で使用した記録済みの音声認識に同様の方法で使用されます。

記録はとまる、ユーザーが場合、は、オーディオ エンジンと音声認識の要求が通知されます。

```csharp
AudioEngine.Stop ();
LiveSpeechRequest.EndAudio ();
```

ユーザーが認識をキャンセルし、オーディオ エンジンと認識タスクに通知されます。

```csharp
AudioEngine.Stop ();
RecognitionTask.Cancel ();
```

呼び出しすることが重要`RecognitionTask.Cancel`メモリと、デバイスのプロセッサの両方を解放する翻訳が取り消された場合。

> [!IMPORTANT]
> 提供できない場合、`NSSpeechRecognitionUsageDescription`または`NSMicrophoneUsageDescription``Info.plist`キー音声認識またはライブ オーディオ、マイクにアクセスしようとしたときに警告なしに失敗しているアプリで発生することができます (`var node = AudioEngine.InputNode;`)。 参照してください、**利用状況の説明を提供する**詳細については、前述の「します。

## <a name="speech-recognition-limits"></a>音声認識の制限

Apple では iOS アプリに音声認識を使用する場合、次の制限が課せられます。

- 音声認識は無料で、すべてのアプリが、その使用法が無制限ではありません。
    - 個々 の iOS デバイスでは、1 日に実行できる認識の数に制限があります。
    - アプリは、1 日あたりの要求ごとにグローバルに調整されます。
- 音声認識のネットワーク接続と使用量レート制限エラーを処理するために、アプリを準備する必要があります。
- 音声認識にバッテリの消耗と大量のネットワーク トラフィックの両方でコストが高いユーザーの iOS デバイスでは、このため、Apple 音声最大約 1 分間の厳密なオーディオ期間制限しています。

アプリ レート調整の限界に達する日常的には、Apple は、開発者に連絡することを確認します。

## <a name="privacy-and-usability-considerations"></a>プライバシーと使いやすさに関する考慮事項

Apple では、透過、iOS アプリに音声認識を含むときは、ユーザーのプライバシーを尊重し、次の提案があります。

- ユーザーの音声を記録するときにを明確に示すアプリのユーザー インターフェイスでの記録が行われていることを確認します。 たとえば、アプリを「記録」のサウンドを再生し、記録インジケーターを表示します。
- パスワード、正常性データまたは財務情報などのユーザーの機密情報の音声認識を使用しないでください。
- 認識結果を表示する_する前に_操作します。 これだけでなく、アプリを行っているが実行時に認識エラーを処理するためにユーザーを許可するものに関するフィードバックを提供します。

## <a name="summary"></a>まとめ

この記事が新しい Speech API を表示し、Xamarin.iOS アプリを継続的な音声認識をサポートし、(生または録画のオーディオ ストリーム) からの音声をテキストに議事録の作成で実装する方法を示しました。 



## <a name="related-links"></a>関連リンク

- [SpeakToMe (サンプル)](https://developer.xamarin.com/samples/monotouch/ios10/SpeakToMe/)
