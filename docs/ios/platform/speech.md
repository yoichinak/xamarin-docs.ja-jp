---
title: "音声認識"
description: "この記事では、新しい音声 API を表示し、継続的な音声認識をサポートし、(生または録画のオーディオ ストリーム) からの音声をテキストに議事録の作成、Xamarin.iOS アプリに実装する方法を示しています。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 64FED50A-6A28-4833-BEAE-63CEC9A09010
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 33e27043c3738c5213b17786e5a88fb30a7fc017
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="speech-recognition"></a>音声認識

_この記事では、新しい音声 API を表示し、継続的な音声認識をサポートし、(生または録画のオーディオ ストリーム) からの音声をテキストに議事録の作成、Xamarin.iOS アプリに実装する方法を示しています。_

新しい iOS 10、Apple がリリースをテキストに iOS アプリを継続的な音声認識をサポートし、(生または録画のオーディオ ストリーム) からの音声議事録の作成を許可する音声認識 API です。

Apple、に従って、音声認識の API は、以下の機能と利点があります。

- 正確な
- 最新
- 使いやすい
- Fast
- 複数の言語をサポートします。
- ユーザーのプライバシーを尊重

## <a name="how-speech-recognition-works"></a>音声認識のしくみ

音声認識は、iOS アプリでライブまたは事前に録音オーディオ (API をサポートする音声の言語のいずれか) を取得して、活字のテキスト形式のトランスクリプションが返されます音声認識エンジンに渡すことによって実装されます。

[![](speech-images/speech01.png "音声認識のしくみ")](speech-images/speech01.png#lightbox)

### <a name="keyboard-dictation"></a>キーボード ディクテーション モード

ほとんどのユーザーは、iOS デバイスで音声認識と考えるとときに、iOS 5 と iPhone 4 秒でキーボード ディクテーションと共にリリースされた組み込み Siri 音声アシスタントと考えます。

キーボード ディクテーション TextKit をサポートする任意のインターフェイス要素ではサポートされて (など`UITextField`または`UITextArea`) し、ユーザーが iOS 仮想キーボードで (スペース バーの左側) に直接ディクテーション ボタンをクリックしてアクティブにします。

Apple は (2011年以降に収集された) 次のキーボード ディクテーション統計情報をリリースしました。

- Ios 5 のリリース以降、キーボード ディクテーションを広く使用されています。
- 約 65,000 1 日にアプリを使用します。
- すべての iOS の 3 つ目はディクテーション モードは、サード パーティ製アプリで行われます。

キーボード ディクテーション モードは、非常に使いやすい TextKit インターフェイス要素を使用して、アプリの UI の設計で以外の開発者の部分での作業は必要ありません。 キーボード ディクテーション モードでは、使用するには、アプリからの特別な権限の要求が必要としないという利点もあります。

音声認識の新しい Api を使用するアプリには、音声認識に転送し、Apple のサーバー上のデータの一時的なストレージが必要があるため、ユーザーが許可する特別なアクセス許可が必要です。 参照してください、[セキュリティとプライバシーの機能強化](~/ios/app-fundamentals/security-privacy.md)詳細についてはドキュメントです。

キーボード ディクテーションで簡単に実装は、いくつかの制限事項と欠点を含んでいるは。

- テキスト入力フィールドを使用して、キーボードの表示が必要です。
- ライブ オーディオの入力のみと連携して、アプリがオーディオ録音プロセスが細かく制御を持ちません。
- ユーザーの音声を解釈するために使用される言語の制御は得られません。
- アプリ ディクテーション ボタンがユーザーにも使用可能なかどうかを把握するための方法はありません。
- アプリは、オーディオ録音処理をカスタマイズすることはできません。
- タイミングと信頼度などの情報が欠落している簡易非常に結果セットを提供します。

### <a name="speech-recognition-api"></a>音声認識 API

新しい iOS 10、Apple が解放されることを音声認識 API を音声認識を実装する iOS アプリのより強力な方法を提供します。 この API は、Apple が Siri とキーボード ディクテーションの両方の電源を使用するものと同じとは、最先端の精度で高速議事録を提供できます。

音声認識 API によって提供される結果は、アプリを収集したり、任意のプライベート ユーザー データにアクセスすることがなく、個々 のユーザーに透過的にカスタマイズされます。

音声認識 API で呼び出し元アプリケーションに結果をほぼリアルタイムように、ユーザーが話すと、テキストだけでなく変換の結果に関する詳細情報を提供します。 次の設定があります。

- どのようなユーザーの複数の意味と言われます。
- 各翻訳の信頼レベル。
- タイミングの情報です。

前述のように、いずれかでライブ フィードとは、記録済みのソースからや、50 以上の言語と iOS 10 でサポートされている言語のいずれかで翻訳のオーディオを提供できます。

音声認識 API では、iOS 10 を実行しているすべての iOS デバイスで使用できるし、Apple のサーバーで行われる翻訳の一括のため、ほとんどの場合、インターネット接続が必要です。 ただし、いくつか新しい iOS デバイスで 常にサポート、言語固有のデバイス上の変換。

Apple には、特定の言語が、現在の翻訳のために使用できるかを判断可用性 API が含まれています。 アプリは、インターネット接続自体を直接テストする代わりにこの API を使用する必要があります。

前述のキーボード ディクテーション セクションで、音声認識が必要です、転送と Apple のサーバー上のデータの一時的なストレージと同様に、アプリ、インターネット経由で_必要があります_を実行するユーザーのアクセス許可を要求認識を含めることによって、`NSSpeechRecognitionUsageDescription`キーをその`Info.plist`ファイルと呼び出し元、`SFSpeechRecognizer.RequestAuthorization`メソッド。 

音声認識で使用されているオーディオのソースに基づき、その他のアプリの変更`Info.plist`ファイルが必要な可能性があります。 参照してください、[セキュリティとプライバシーの機能強化](~/ios/app-fundamentals/security-privacy.md)詳細についてはドキュメントです。

## <a name="adopting-speech-recognition-in-an-app"></a>アプリ内での音声認識を採用します。

IOS アプリでの音声認識を採用する開発者が行う必要のある主要な手順は 4 つです。

- アプリの使用状況の説明を提供`Info.plist`ツールを使用して、`NSSpeechRecognitionUsageDescription`キー。 たとえば、カメラ アプリが次の説明を含めることが_"これを取得できるように、写真という単語を言うことによりだけ 'チーズ'."_
- 呼び出してに承認を要求、`SFSpeechRecognizer.RequestAuthorization`説明を表示する方法 (で提供される、`NSSpeechRecognitionUsageDescription`上記のキー)、アプリが音声認識を希望する理由の認識とへのアクセス ダイアログ ボックスで、ユーザー同意または拒否できるようにします。
- 音声認識の要求を作成します。
    * ディスク上の事前に録音されたオーディオは、使用、`SFSpeechURLRecognitionRequest`クラスです。
    * ライブ オーディオ (またはメモリからオーディオ) を使用して、`SFSPeechAudioBufferRecognitionRequest`クラスです。
- 音声認識エンジンに音声認識の要求を渡す (`SFSpeechRecognizer`) を認識を開始します。 アプリが返されたを保持できます必要に応じて`SFSpeechRecognitionTask`を監視し、認識の結果を追跡します。

次の手順は、以下で詳しく取り上げます。

### <a name="providing-a-usage-description"></a>使用状況の説明を提供します。

必須の提供する`NSSpeechRecognitionUsageDescription`キー、`Info.plist`ファイルで、次の操作します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. ダブルクリックして、`Info.plist`ファイルを開いて編集するファイル。
2. 切り替えて、**ソース**ビュー。 

    [![](speech-images/speech02.png "ソース ビュー")](speech-images/speech02.png#lightbox)
3. をクリックして**新しいエントリの追加**、入力`NSSpeechRecognitionUsageDescription`の**プロパティ**、`String`の**型**と**用途説明**として、**値**です。 例: 

    [![](speech-images/speech03.png "NSSpeechRecognitionUsageDescription を追加します。")](speech-images/speech03.png#lightbox)
4. アプリがライブ オーディオ議事録を処理する場合が、マイクの使用状況の説明も必要です。 をクリックして**新しいエントリの追加**、入力`NSMicrophoneUsageDescription`の**プロパティ**、`String`の**型**と**用途説明**として、**値**です。 例: 

    [![](speech-images/speech04.png "NSMicrophoneUsageDescription を追加します。")](speech-images/speech04.png#lightbox)
4. 変更内容をファイルに保存します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. ダブルクリックして、`Info.plist`ファイルを開いて編集するファイル。
3. をクリックして**新しいエントリの追加**、入力`NSSpeechRecognitionUsageDescription`の**プロパティ**、`String`の**型**と**用途説明**として、**値**です。 例: 

    [![](speech-images/speech03w.png "NSSpeechRecognitionUsageDescription を追加します。")](speech-images/speech03w.png#lightbox)
4. アプリがライブ オーディオ議事録を処理する場合が、マイクの使用状況の説明も必要です。 をクリックして**新しいエントリの追加**、入力`NSMicrophoneUsageDescription`の**プロパティ**、`String`の**型**と**用途説明**として、**値**です。 例: 

    [![](speech-images/speech04w.png "NSMicrophoneUsageDescription を追加します。")](speech-images/speech04w.png#lightbox)
4. 変更内容をファイルに保存します。

-----

> [!IMPORTANT]
> **注:**に上記のいずれかを提供できない場合`Info.plist`キー (`NSSpeechRecognitionUsageDescription`または`NSMicrophoneUsageDescription`) 音声認識またはライブ オーディオのマイクにアクセスしようとしているときに警告なしに失敗しているアプリで発生することができます。




### <a name="requesting-authorization"></a>承認を要求します。

アプリケーションに音声認識のアクセスを許可する必要なユーザー認証を要求するにはのメイン ビュー コント ローラー クラスを編集し、次のコードを追加します。

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

`RequestAuthorization`のメソッド、`SFSpeechRecognizer`クラスはアクセス音声認識を使用する理由で、開発者が提供するユーザーからアクセス許可を要求するが、`NSSpeechRecognitionUsageDescription`のキー、`Info.plist`ファイル。

A`SFSpeechRecognizerAuthorizationStatus`に結果が返されます、`RequestAuthorization`アクションを実行するために使用するメソッドのコールバック ルーチンがユーザーのアクセス許可に基づいています。 

> [!IMPORTANT]
> **注:**ユーザーがこのアクセス許可を要求する前に音声認識を必要とするアプリでアクションを開始するまで待機している Apple を提案します。

### <a name="recognizing-pre-recorded-speech"></a>記録済みの音声を認識します。

アプリは、事前に録音 WAV ファイルまたは MP3 ファイルからの音声を認識する必要がある場合は、次のコードを使用してできます。

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

このコードの詳細を見ると、最初に、作成しようと音声認識エンジン (`SFSpeechRecognizer`)。 音声認識用の既定の言語がサポートされていない場合`null`が返されると、関数が終了します。

音声認識エンジンが既定の言語の使用可能な場合は、アプリを確認しますは現在認識で使用できるかどうか、`Available`プロパティです。 たとえば、認識できない可能性があります、デバイスがアクティブなインターネット接続を持っていない場合。

A`SFSpeechUrlRecognitionRequest`から作成された、 `NSUrl` iOS デバイスに事前に録音ファイルの場所がコールバック ルーチンで処理する音声認識エンジンに渡されました。

コールバックが呼び出されたとき場合、`NSError`いない`null`処理する必要があるエラーが発生しました。 音声認識は段階的に行われるため、コールバック ルーチンを呼び出すことは 1 回ためより詳細`SFSpeechRecognitionResult.Final`プロパティがテストされ、変換が完了し、最適なバージョンの翻訳が出力されたかどうかを参照してください (`BestTranscription`)。

### <a name="recognizing-live-speech"></a>ライブ音声を認識します。

アプリは、ライブ音声を認識する必要がある場合、プロセスは記録済みの音声を認識することとよく似ています。 例:

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

このコードの詳細を見ると、認識プロセスを処理するためのいくつかのプライベート変数を作成します。

```csharp
private AVAudioEngine AudioEngine = new AVAudioEngine ();
private SFSpeechRecognizer SpeechRecognizer = new SFSpeechRecognizer ();
private SFSpeechAudioBufferRecognitionRequest LiveSpeechRequest = new SFSpeechAudioBufferRecognitionRequest ();
private SFSpeechRecognitionTask RecognitionTask;
```

オーディオを録音に渡される AV Foundation を使用して、`SFSpeechAudioBufferRecognitionRequest`認識要求を処理します。

```csharp
var node = AudioEngine.InputNode;
var recordingFormat = node.GetBusOutputFormat (0);
node.InstallTapOnBus (0, 1024, recordingFormat, (AVAudioPcmBuffer buffer, AVAudioTime when) => {
    // Append buffer to recognition request
    LiveSpeechRequest.Append (buffer);
});
```

アプリが録画を開始しようとして、録画を開始できない場合、エラーが処理されます。

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

認識のタスクが開始され、認識タスクへのハンドルを保持 (`SFSpeechRecognitionTask`)。

```csharp
RecognitionTask = SpeechRecognizer.GetRecognitionTask (LiveSpeechRequest, (SFSpeechRecognitionResult result, NSError err) => {
    ...
});
```

コールバックは、上記記録済みの音声認識で使用する 1 つと同様の方法で使用されます。

記録は、ユーザーによって %n%n%2、オーディオ エンジンと、音声認識を要求の両方に通知されます。

```csharp
AudioEngine.Stop ();
LiveSpeechRequest.EndAudio ();
```

ユーザーが認識をキャンセルし、オーディオ エンジンと認識するタスクに通知されます。

```csharp
AudioEngine.Stop ();
RecognitionTask.Cancel ();
```

呼び出すことが重要`RecognitionTask.Cancel`メモリと、デバイスのプロセッサの両方を解放する翻訳が取り消された場合。

> [!IMPORTANT]
> **注:**に提供できない場合、`NSSpeechRecognitionUsageDescription`または`NSMicrophoneUsageDescription``Info.plist`キー音声認識またはライブ オーディオのマイクにアクセスしようとしているときに警告なしに失敗しているアプリで発生することができます (`var node = AudioEngine.InputNode;`)。 参照してください、**使用状況の説明を提供する**の詳細については、上記「します。

## <a name="speech-recognition-limits"></a>音声認識の制限

Apple は iOS アプリで音声認識を操作するときに、次の制限があります。

- 音声認識が自由に、すべてのアプリには、その使用法が無制限ではありません。
    - 個々 の iOS デバイスでは、1 日に実行できる認定の数に制限があります。
    - アプリは、1 日あたりの要求ごとにグローバルに抑制されます。
- 音声認識のネットワーク接続および使用率の制限エラーを処理するアプリを準備する必要があります。
- Apple 制限厳密なオーディオ期間音声最大約 1 分間の音声認識はこのため、ユーザーの iOS デバイスでバッテリ ドレインと高いネットワーク トラフィックの両方で高コストを設定することができます。

アプリは、レート調整の限界をヒットたままにし、通常は、Apple は開発者に問い合わせてことを確認します。

## <a name="privacy-and-usability-considerations"></a>プライバシーと操作性に関する考慮事項

Apple では、透過的なされ、iOS アプリに音声認識を含むときは、ユーザーのプライバシーを考慮し、その次提案があります。

- ユーザーの音声を記録するときにを明確に示す、アプリのユーザー インターフェイスでの記録が行われていることを確認します。 たとえば、アプリは、「録画」のサウンドを再生し、記録インジケーターを表示する可能性があります。
- パスワード、正常性のデータまたは財務情報などの機密性の高いユーザー情報については、音声認識を使用しないでください。
- 認識の結果を表示する_する前に_に機能します。 これだけでなく、アプリを行ってが認識エラーを処理できるようにユーザーを許可する新機能に関するフィードバックを提供します。

## <a name="summary"></a>まとめ

この記事が、新しい音声 API を表示し、Xamarin.iOS アプリを継続的な音声認識をサポートし、(生または録画のオーディオ ストリーム) からの音声をテキストに議事録の作成で実装する方法を示しました。 



## <a name="related-links"></a>関連リンク

- [SpeakToMe (サンプル)](https://developer.xamarin.com/samples/monotouch/ios10/SpeakToMe/)
