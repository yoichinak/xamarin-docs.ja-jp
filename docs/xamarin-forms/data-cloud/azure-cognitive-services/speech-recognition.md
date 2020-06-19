---
title: Speech サービス API を使用した音声認識
description: この記事では、Azure Speech Service API を使用して、音声をアプリケーションのテキストに表示する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 01/14/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 171ecc02fda304135e5f535c3e798067595d7047
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139322"
---
# <a name="speech-recognition-using-azure-speech-service"></a>Azure Speech Service を使用した音声認識

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-cognitivespeechservice)

Azure Speech Service は、次の機能を提供するクラウドベースの API です。

- **音声からテキスト**への speech オーディオファイルまたはストリームをテキストに変換します。
- **テキストから音声へ**の変換は、入力テキストを人間に似た合成音声に変換します。
- **音声翻訳**を使用すると、音声からテキストへの変換と音声合成音声の両方について、リアルタイムで多言語翻訳を行うことができます。
- **音声アシスタント**は、アプリケーション用の人間に似たメッセージ交換インターフェイスを作成できます。

この記事では Xamarin.Forms 、Azure Speech サービスを使用して、サンプルアプリケーションで音声からテキストを実装する方法について説明します。 次のスクリーンショットは、iOS と Android のサンプルアプリケーションを示しています。

[![IOS と Android のサンプルアプリケーションのスクリーンショット](speech-recognition-images/speech-recognition-cropped.png)](speech-recognition-images/speech-recognition.png#lightbox "IOS と Android のサンプルアプリケーションのスクリーンショット")

## <a name="create-an-azure-speech-service-resource"></a>Azure Speech Service リソースを作成する

Azure Speech Service は Azure Cognitive Services の一部であり、画像認識、音声認識、翻訳、Bing search などのタスクにクラウドベースの Api を提供します。 詳細については、「 [Azure Cognitive Services とは](https://docs.microsoft.com/azure/cognitive-services/welcome)」を参照してください。

サンプルプロジェクトでは、Azure portal に Azure Cognitive Services リソースを作成する必要があります。 Cognitive Services リソースは、Speech Service などの1つのサービスに対して、またはマルチサービスリソースとして作成できます。 Speech サービスリソースを作成する手順は次のとおりです。

1. [Azure portal](https://portal.azure.com)にログインします。
1. マルチサービスまたは単一サービスのリソースを作成します。
1. リソースの API キーとリージョン情報を取得します。
1. サンプルの**Constants.cs**ファイルを更新します。

リソースを作成する手順については、「 [Cognitive Services リソースの作成](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)」を参照してください。

> [!NOTE]
> [Azure サブスクリプション](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)をお持ちでない場合は、開始する前に[無料アカウント](https://aka.ms/azfree-docs-mobileapps)を作成してください。 アカウントを作成したら、サービスを試用するために、free レベルで1つのサービスリソースを作成できます。

## <a name="configure-your-app-with-the-speech-service"></a>Speech サービスを使用してアプリを構成する

Cognitive Services リソースを作成した後、Azure リソースのリージョンと API キーを使用して**Constants.cs**ファイルを更新できます。

```csharp
public static class Constants
{
    public static string CognitiveServicesApiKey = "YOUR_KEY_GOES_HERE";
    public static string CognitiveServicesRegion = "westus";
}
```

## <a name="install-nuget-speech-service-package"></a>NuGet Speech Service パッケージのインストール

このサンプルアプリケーションでは、 **cognitiveservices account** NuGet パッケージを使用して Azure Speech サービスに接続します。 この NuGet パッケージは、共有プロジェクトと各プラットフォームプロジェクトにインストールします。

## <a name="create-an-imicrophoneservice-interface"></a>IMicrophoneService インターフェイスを作成する

各プラットフォームには、マイクにアクセスするためのアクセス許可が必要です。 サンプルプロジェクトは `IMicrophoneService` 共有プロジェクトにインターフェイスを提供し、を使用して Xamarin.Forms `DependencyService` インターフェイスのプラットフォーム実装を取得します。

```csharp
public interface IMicrophoneService
{
    Task<bool> GetPermissionAsync();
    void OnRequestPermissionResult(bool isGranted);
}
```

## <a name="create-the-page-layout"></a>ページレイアウトを作成する

サンプルプロジェクトでは、 **mainpage.xaml**ファイルに基本的なページレイアウトを定義しています。 キーレイアウト要素は、 `Button` 議事録の処理を開始する、 `Label` 書き起こしテキストを格納する、および `ActivityIndicator` 議事録の処理が進行中であることを示すためのです。

```xaml
<ContentPage ...>
    <StackLayout>
        <Frame ...>
            <ScrollView x:Name="scroll"
                        ...>
                <Label x:Name="transcribedText"
                       ... />
            </ScrollView>
        </Frame>

        <ActivityIndicator x:Name="transcribingIndicator"
                           IsRunning="False" />
        <Button x:Name="transcribeButton"
                ...
                Clicked="TranscribeClicked"/>
    </StackLayout>
</ContentPage>
```

## <a name="implement-the-speech-service"></a>Speech サービスを実装する

**MainPage.xaml.cs**分離コードファイルには、Azure Speech サービスから音声を送信し、書き起こしテキストを受信するためのすべてのロジックが含まれています。

`MainPage`コンストラクターは、からインターフェイスのインスタンスを取得し `IMicrophoneService` `DependencyService` ます。

```csharp
public partial class MainPage : ContentPage
{
    SpeechRecognizer recognizer;
    IMicrophoneService micService;
    bool isTranscribing = false;

    public MainPage()
    {
        InitializeComponent();

        micService = DependencyService.Resolve<IMicrophoneService>();
    }

    // ...
}
```

`TranscribeClicked`インスタンスがタップされると、メソッドが呼び出され `transcribeButton` ます。

```csharp
async void TranscribeClicked(object sender, EventArgs e)
{
    bool isMicEnabled = await micService.GetPermissionAsync();

    // EARLY OUT: make sure mic is accessible
    if (!isMicEnabled)
    {
        UpdateTranscription("Please grant access to the microphone!");
        return;
    }

    // initialize speech recognizer 
    if (recognizer == null)
    {
        var config = SpeechConfig.FromSubscription(Constants.CognitiveServicesApiKey, Constants.CognitiveServicesRegion);
        recognizer = new SpeechRecognizer(config);
        recognizer.Recognized += (obj, args) =>
        {
            UpdateTranscription(args.Result.Text);
        };
    }

    // if already transcribing, stop speech recognizer
    if (isTranscribing)
    {
        try
        {
            await recognizer.StopContinuousRecognitionAsync();
        }
        catch(Exception ex)
        {
            UpdateTranscription(ex.Message);
        }
        isTranscribing = false;
    }

    // if not transcribing, start speech recognizer
    else
    {
        Device.BeginInvokeOnMainThread(() =>
        {
            InsertDateTimeRecord();
        });
        try
        {
            await recognizer.StartContinuousRecognitionAsync();
        }
        catch(Exception ex)
        {
            UpdateTranscription(ex.Message);
        }
        isTranscribing = true;
    }
    UpdateDisplayState();
}
```

`TranscribeClicked` メソッドは以下を実行します。

1. アプリケーションがマイクにアクセスできるかどうかを確認し、存在しない場合は早期に終了します。
1. クラスがまだ存在しない場合は、そのインスタンスを作成し `SpeechRecognizer` ます。
1. 進行中の場合は、連続した議事録を停止します。
1. タイムスタンプを挿入し、進行中でない場合は継続的な議事録を開始します。
1. アプリケーションに、新しいアプリケーションの状態に基づいて外観を更新するように通知します。

クラスメソッドの残りの部分 `MainPage` は、アプリケーションの状態を表示するためのヘルパーです。

```csharp
void UpdateTranscription(string newText)
{
    Device.BeginInvokeOnMainThread(() =>
    {
        if (!string.IsNullOrWhiteSpace(newText))
        {
            transcribedText.Text += $"{newText}\n";
        }
    });
}

void InsertDateTimeRecord()
{
    var msg = $"=================\n{DateTime.Now.ToString()}\n=================";
    UpdateTranscription(msg);
}

void UpdateDisplayState()
{
    Device.BeginInvokeOnMainThread(() =>
    {
        if (isTranscribing)
        {
            transcribeButton.Text = "Stop";
            transcribeButton.BackgroundColor = Color.Red;
            transcribingIndicator.IsRunning = true;
        }
        else
        {
            transcribeButton.Text = "Transcribe";
            transcribeButton.BackgroundColor = Color.Green;
            transcribingIndicator.IsRunning = false;
        }
    });
}
```

メソッドは、 `UpdateTranscription` 指定されたを `newText` `string` `Label` という名前の要素に書き込み `transcribedText` ます。 この更新は、UI スレッドで強制的に実行されるため、例外を発生させずに任意のコンテキストから呼び出すことができます。 は、 `InsertDateTimeRecord` 現在の日付と時刻をインスタンスに書き込み、 `transcribedText` 新しい議事録の開始をマークします。 最後に、メソッドは、との各要素を更新して、 `UpdateDisplayState` `Button` `ActivityIndicator` 議事録が進行中かどうかを反映します。

## <a name="create-platform-microphone-services"></a>プラットフォームマイクサービスを作成する

音声データを収集するには、アプリケーションがマイクにアクセスできる必要があります。 `IMicrophoneService`アプリケーションが機能するためには、インターフェイスを実装し、各プラットフォームのに登録する必要があり `DependencyService` ます。

### <a name="android"></a>Android

サンプルプロジェクトでは、 `IMicrophoneService` という名前の Android の実装を定義し `AndroidMicrophoneService` ます。

```csharp
[assembly: Dependency(typeof(AndroidMicrophoneService))]
namespace CognitiveSpeechService.Droid.Services
{
    public class AndroidMicrophoneService : IMicrophoneService
    {
        public const int RecordAudioPermissionCode = 1;
        private TaskCompletionSource<bool> tcsPermissions;
        string[] permissions = new string[] { Manifest.Permission.RecordAudio };

        public Task<bool> GetPermissionAsync()
        {
            tcsPermissions = new TaskCompletionSource<bool>();

            if ((int)Build.VERSION.SdkInt < 23)
            {
                tcsPermissions.TrySetResult(true);
            }
            else
            {
                var currentActivity = MainActivity.Instance;
                if (ActivityCompat.CheckSelfPermission(currentActivity, Manifest.Permission.RecordAudio) != (int)Permission.Granted)
                {
                    RequestMicPermissions();
                }
                else
                {
                    tcsPermissions.TrySetResult(true);
                }

            }

            return tcsPermissions.Task;
        }

        public void OnRequestPermissionResult(bool isGranted)
        {
            tcsPermissions.TrySetResult(isGranted);
        }

        void RequestMicPermissions()
        {
            if (ActivityCompat.ShouldShowRequestPermissionRationale(MainActivity.Instance, Manifest.Permission.RecordAudio))
            {
                Snackbar.Make(MainActivity.Instance.FindViewById(Android.Resource.Id.Content),
                        "Microphone permissions are required for speech transcription!",
                        Snackbar.LengthIndefinite)
                        .SetAction("Ok", v =>
                        {
                            ((Activity)MainActivity.Instance).RequestPermissions(permissions, RecordAudioPermissionCode);
                        })
                        .Show();
            }
            else
            {
                ActivityCompat.RequestPermissions((Activity)MainActivity.Instance, permissions, RecordAudioPermissionCode);
            }
        }
    }
}
```

には、 `AndroidMicrophoneService` 次の機能があります。

1. `Dependency`属性は、クラスをに登録し `DependencyService` ます。
1. メソッドは、 `GetPermissionAsync` Android SDK のバージョンに基づいてアクセス許可が必要かどうかをチェックし、 `RequestMicPermissions` アクセス許可がまだ付与されていない場合はを呼び出します。
1. このメソッドは、クラスを使用して、 `RequestMicPermissions` `Snackbar` 論理的な根拠が必要な場合にユーザーにアクセス許可を要求します。それ以外の場合は、オーディオ記録アクセス許可を直接要求します。
1. この `OnRequestPermissionResult` メソッドは、ユーザーが `bool` アクセス許可要求に応答した後に、結果と共に呼び出されます。

クラスは、 `MainActivity` `AndroidMicrophoneService` アクセス許可要求の完了時にインスタンスを更新するようにカスタマイズされています。

```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
{
    IMicrophoneService micService;
    internal static MainActivity Instance { get; private set; }
    
    protected override void OnCreate(Bundle savedInstanceState)
    {
        Instance = this;
        // ...
        micService = DependencyService.Resolve<IMicrophoneService>();
    }
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
    {
        // ...
        switch(requestCode)
        {
            case AndroidMicrophoneService.RecordAudioPermissionCode:
                if (grantResults[0] == Permission.Granted)
                {
                    micService.OnRequestPermissionResult(true);
                }
                else
                {
                    micService.OnRequestPermissionResult(false);
                }
                break;
        }
    }
}
```

`MainActivity`クラスは、という静的参照を定義し `Instance` ます。これは、 `AndroidMicrophoneService` アクセス許可を要求するときにオブジェクトによって要求されます。 `OnRequestPermissionsResult` `AndroidMicrophoneService` アクセス許可の要求がユーザーによって承認または拒否されたときに、メソッドをオーバーライドしてオブジェクトを更新します。

最後に、Android アプリケーションには、 **AndroidManifest.xml**ファイルにオーディオを記録するためのアクセス許可が含まれている必要があります。

```xml
<manifest ...>
    ...
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
</manifest>
```

### <a name="ios"></a>iOS

サンプルプロジェクトでは、 `IMicrophoneService` という iOS の実装を定義してい `iOSMicrophoneService` ます。

```csharp
[assembly: Dependency(typeof(iOSMicrophoneService))]
namespace CognitiveSpeechService.iOS.Services
{
    public class iOSMicrophoneService : IMicrophoneService
    {
        TaskCompletionSource<bool> tcsPermissions;

        public Task<bool> GetPermissionAsync()
        {
            tcsPermissions = new TaskCompletionSource<bool>();
            RequestMicPermission();
            return tcsPermissions.Task;
        }

        public void OnRequestPermissionResult(bool isGranted)
        {
            tcsPermissions.TrySetResult(isGranted);
        }

        void RequestMicPermission()
        {
            var session = AVAudioSession.SharedInstance();
            session.RequestRecordPermission((granted) =>
            {
                tcsPermissions.TrySetResult(granted);
            });
        }
    }
}
```

には、 `iOSMicrophoneService` 次の機能があります。

1. `Dependency`属性は、クラスをに登録し `DependencyService` ます。
1. `GetPermissionAsync`メソッドはを呼び出して、 `RequestMicPermissions` デバイスユーザーからアクセス許可を要求します。
1. メソッドは、 `RequestMicPermissions` 共有インスタンスを使用して `AVAudioSession` 記録アクセス許可を要求します。
1. メソッドは、 `OnRequestPermissionResult` 指定された値を使用してインスタンスを更新し `TaskCompletionSource` `bool` ます。

最後に、iOS アプリ**情報 plist**には、アプリがマイクへのアクセスを要求している理由をユーザーに通知するメッセージが含まれている必要があります。 情報の plist ファイルを編集して、要素内に次のタグを追加し `<dict>` ます。

```xml
<plist>
    <dict>
        ...
        <key>NSMicrophoneUsageDescription</key>
        <string>Voice transcription requires microphone access</string>
    </dict>
</plist>
```

### <a name="uwp"></a>UWP

サンプルプロジェクトでは、 `IMicrophoneService` という UWP の実装を定義してい `UWPMicrophoneService` ます。

```csharp
[assembly: Dependency(typeof(UWPMicrophoneService))]
namespace CognitiveSpeechService.UWP.Services
{
    public class UWPMicrophoneService : IMicrophoneService
    {
        public async Task<bool> GetPermissionAsync()
        {
            bool isMicAvailable = true;
            try
            {
                var mediaCapture = new MediaCapture();
                var settings = new MediaCaptureInitializationSettings();
                settings.StreamingCaptureMode = StreamingCaptureMode.Audio;
                await mediaCapture.InitializeAsync(settings);
            }
            catch(Exception ex)
            {
                isMicAvailable = false;
            }

            if(!isMicAvailable)
            {
                await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-microphone"));
            }

            return isMicAvailable;
        }

        public void OnRequestPermissionResult(bool isGranted)
        {
            // intentionally does nothing
        }
    }
}
```

には、 `UWPMicrophoneService` 次の機能があります。

1. `Dependency`属性は、クラスをに登録し `DependencyService` ます。
1. メソッドは、 `GetPermissionAsync` インスタンスの初期化を試み `MediaCapture` ます。 失敗した場合は、マイクを有効にするためのユーザー要求を起動します。
1. この `OnRequestPermissionResult` メソッドはインターフェイスを満たすために存在しますが、UWP 実装には必要ありません。

最後に、 **package.appxmanifest**は、アプリケーションでマイクを使用するように指定する必要があります。 Package.appxmanifest ファイルをダブルクリックし、Visual Studio 2019 の [**機能**] タブで [**マイク**] オプションを選択します。

[![Visual Studio 2019 のマニフェストのスクリーンショット](speech-recognition-images/package-manifest-cropped.png)](speech-recognition-images/package-manifest.png#lightbox "Visual Studio 2019 のマニフェストのスクリーンショット")

## <a name="test-the-application"></a>アプリケーションをテストする

アプリを実行し、[**議事録**] ボタンをクリックします。 アプリはマイクへのアクセスを要求し、議事録の処理を開始する必要があります。 が `ActivityIndicator` アニメーション化され、議事録がアクティブであることが示されます。 話すと、アプリは Azure Speech Services リソースにオーディオデータをストリーミングし、書き起こしテキストで応答します。 書き起こしテキストは、受信時に要素に表示され `Label` ます。

> [!NOTE]
> Android エミュレーターは、Speech サービスライブラリの読み込みと初期化に失敗します。 Android プラットフォームでは、物理デバイスでのテストをお勧めします。

## <a name="related-links"></a>関連リンク

- [Azure Speech Service のサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-cognitivespeechservice)
- [Azure Speech Service の概要](https://docs.microsoft.com/azure/cognitive-services/speech-service/overview)
- [Cognitive Services リソースの作成](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)
- [クイック スタート: マイクから音声を認識する](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstarts/speech-to-text-from-microphone)