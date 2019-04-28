---
title: Jelly Bean の機能
description: 'このドキュメントでは、Android 4.1 で導入された開発者向け新機能の概要を説明します。 これらの機能が含まれます: Android ビーム マルチ メディア、ピア ツー ピア ネットワーク探索、アニメーション、新しいアクセス許可の更新プログラム、大きなファイルを共有する更新プログラムの通知を強化します。'
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 24fc14b0342591c56f5bf91862b0d94759a42834
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61078425"
---
# <a name="jelly-bean-features"></a>Jelly Bean の機能

_このドキュメントでは、Android 4.1 で導入された開発者向け新機能の概要を説明します。これらの機能が含まれます: Android ビーム マルチ メディア、ピア ツー ピア ネットワーク探索、アニメーション、新しいアクセス許可の更新プログラム、大きなファイルを共有する更新プログラムの通知を強化します。_



## <a name="overview"></a>概要

Android 4.1 (API レベル 16、)、Jelly Bean"とも呼ばれます"は、2012 年 7 月 9 日にリリースしました。 この記事では、開発者が Xamarin.Android を使用して高レベルな概要については、Android 4.1 の新機能の一部に提供されます。 導入されたこれらの新機能の一部は、アクティビティ、カメラの場合は、新しいサウンドとアプリケーション スタックのナビゲーションのサポートの強化を起動するためのアニメーションの機能強化です。 切り取ってインテントに貼り付けることはようになりました。

Android アプリケーションの安定性が不安定なコンテンツ プロバイダーへの依存関係を分離する機能を向上します。 サービスは、それらを開始したアクティビティによってのみアクセスできるように分離あります。

サポートは、Bonjour、UPnP、またはマルチキャストの DNS ベースのサービスのネットワーク サービスの検出を使用して追加がされました。 高度な通知に書式設定されたテキスト、アクション ボタンおよびサイズの大きなイメージをなる可能性が生まれます。

最後に Android 4.1 では、いくつかの新しいアクセス許可が追加されました。



## <a name="requirements"></a>必要条件

必要があります Xamarin.Android を Jelly Bean を使用して Xamarin.Android アプリケーションを開発する 4.2.6 または高い、Android 4.1 (API レベル 16) は、次のスクリーン ショットに示すように、Android SDK Manager を使用してにインストールします。

[![Android SDK Manager での Android 4.1 の選択](jelly-bean-images/image1.png)](jelly-bean-images/image1.png#lightbox)



## <a name="whats-new"></a>新機能



### <a name="animations"></a>Animations

使用してズーム アニメーションまたはカスタム アニメーションのいずれかを使用してアクティビティを起動する可能性があります、`ActivityOptions`クラス。 これらのアニメーションをサポートするためには、次の新しいメソッドが用意されています。

-   `MakeScaleUpAnimation` – これにより、開始位置とサイズ、画面上のアクティビティ ウィンドウを拡大/縮小するアニメーションが作成されます。
-   `MakeThumbnailScaleUpAnimation` – これにより、画面上の指定した位置からサムネイル イメージからスケール アップするアニメーションが作成されます。
-   `MakeCustomAnimation` – これは、アプリケーション内のリソースからアニメーションを作成します。 アクティビティが開かれたときの 1 つのアニメーションと別のアクティビティが停止したときがあります。


新しい`TimeAnimator`クラス インターフェイスを提供する`TimeAnimator.ITimeListener`アニメーションのフレームが変更されるたびにアプリケーションに通知することができます。 たとえば、次の実装の`TimeAnimator.ITimeListener`:

```csharp
class MyTimeListener : Java.Lang.Object,  TimeAnimator.ITimeListener
{
    public void OnTimeUpdate(TimeAnimator animation, long totalTime, long deltaTime)
    {
        Log.Debug("Activity1", "totalTime={0}, deltaTime={1}", totalTime, deltaTime);
    }
}
```

次に、クラスのインスタンスを使用する`TimeAnimator`が作成され、リスナーが設定されます。

```csharp
var animator = new TimeAnimator();
animator.SetTimeListener(new MyTimeListener());
animator.Start();
```

として、`TimeAnimator`インスタンスが実行されているを呼び出す`ITimeAnimator.ITimeListener`方法がログインし、アニメーターが長い間実行し、どのくらいの期間にように、前回以来、メソッドが呼び出されたでした。



### <a name="application-stack-navigation"></a>アプリケーション スタックのナビゲーション

Android 4.1 の Android 3.0 で導入されたアプリケーション スタックのナビゲーションが向上します。 指定することによって、`ParentName`のプロパティ、 `ActivityAttribute`、Android は、ユーザーが押したときに適切な親アクティビティを開くことができます、[上方向ボタン](https://developer.android.com/design/patterns/navigation.html#up-vs-back)Android には、操作バーの上、で指定されたアクティビティがインスタンス化`ParentName`プロパティ。 これにより、特定のタスクを構成するアクティビティの階層を保持するアプリケーション。

ほとんどのアプリケーション設定、`ParentName`アクティビティには、アプリケーション スタックを移動するための正しい動作を提供する Android 用の十分な情報Android は親アクティビティごとに一連のインテントを作成して、必要に応じてバック スタックを合成します。 ただし、これは人為的なアプリケーション スタックであるため、各代理の活動は自然なアクティビティが、保存された状態がありません。 アクティビティはオーバーライドの代理の親アクティビティに保存された状態を提供する、`OnPrepareNavigationUpTaskStack`メソッド。 このメソッドは受信、 `TaskStackBuilder` Android が戻るスタックの作成に使用することを目的のコレクションを持つインスタンスのオブジェクトします。 アクティビティは、代理の活動が作成されると、適切な状態情報を受信するため、これらのインテントを変更できます。

複雑なシナリオは、ナビゲーションの動作を処理し、戻るスタックを構築するために使用するアクティビティ クラスに新しいメソッドがあります。

-   `OnNavigateUp` – このメソッドをオーバーライドしてカスタム アクションを実行することはときに、<span class="ui">を</span>ボタンが押されました。
-   `NavigateUpTo` – このメソッドを呼び出すと、現在のアクティビティから特定の目的で指定されたアクティビティに移動するアプリケーションが発生します。
-   `ParentActivityIntent` – これを使用して、現在のアクティビティの親アクティビティを起動するインテントを取得できます。
-   `ShouldUpRecreateTask` – このメソッドは、クエリ、親アクティビティまで移動する代理戻るスタックを作成する必要があるかどうかに使用されます。 返します`true`場合は、合成のスタックを作成する必要があります。 
-   `FinishAffinity` – 現在のアクティビティとすべてのアクティビティ下にある現在のタスクを同じタスクのアフィニティを持つ、このメソッドの呼び出しが完了します。
-   `OnCreateNavigateUpTaskStack` – このメソッドは、合成のスタックを作成する方法を完全に制御する必要がある場合にオーバーライドされます。




### <a name="camera"></a>カメラ

新しいインターフェイス、 `Camera.IAutoFocusMoveCallback`、自動フォーカスが開始または停止を移動するときに検出するために使用できます。 この新しいインターフェイスの例は、次のスニペットで確認できます。

```csharp
public class AutoFocusCallbackActivity : Activity, Camera.IAutoFocusCallback
{
    public void OnAutoFocus(bool success, Camera camera)
    {
        // camera is an instance of the camera service object.

        if (success)
        {
            // Auto focus was successful - do something here.
        }
        else
        {
            // Auto focus didn't happen for some reason - react to that here.
        }
    }
}
```

新しいクラス`MediaActionSound`音のさまざまなメディアの操作を生成するための API のセットを提供します。 これらは列挙型で定義されているカメラで発生することがいくつかの操作`Android.Media.MediaActionSoundType`:

-   `MediaActionSoundType.FocusComplete` – 重点を置くことが完了したときに再生されるサウンドこのにします。
-   `MediaActionSoundType.ShutterClick` – このサウンドは、静止画像写真の作成時に再生されます。
-   `MediaActionSoundType.StartVideoRecording` – このサウンドが使用されるビデオ記録の開始を示します。
-   `MediaActionSoundType.StopVideoRecording` – ビデオ記録の終了を示すこのサウンドが再生されます。


使用する方法の例、`MediaActionSound`クラスは、次のスニペットで確認できます。

```csharp
var mediaActionPlayer = new MediaActionSound();

// Preload the sound for a shutter click.
mediaActionPlayer.Load(MediaActionSoundType.ShutterClick);
var button = FindViewById<Button>(Resource.Id.MyButton);

// Play the sound on a button click.
button.Click += (sender, args) => mediaActionPlayer.Play(MediaActionSoundType.ShutterClick);

// This releases the preloaded resources. Don’t make any calls on
// mediaActionPlayer after this.
mediaActionPlayer.Release();
```



### <a name="connectivity"></a>接続



#### <a name="android-beam"></a>Android ビーム

Android ビームは、互いに通信する 2 つの Android デバイスを許可する NFC ベース テクノロジです。 Android 4.1 では、大きなファイルの転送のサポートの向上を提供します。 新しいメソッドを使用して`NfcAdapter.SetBeamPushUris()`Android は高速転送速度を達成するために代替トランスポート機構 (Bluetooth) などの間で切り替わります。



#### <a name="network-services-discovery"></a>ネットワーク サービスの検出

Android 4.1 には、マルチキャストの DNS ベースのサービスの検出の新しい API が含まれています。
これにより、アプリケーションを検出し、プリンター、カメラ、メディア デバイスなどの他のデバイスに Wi-fi 経由で接続できます。 これらの新しい API は、`Android.Net.Nsd`パッケージ。

その他のサービスで使用できるサービスを作成する、`NsdServiceInfo`クラスは、サービスのプロパティを定義するオブジェクトの作成に使用します。 このオブジェクトは提供され、`NsdManager.RegisterService()`の実装と共に`NsdManager.ResolveListener`します。 実装`NsdManager.ResolveListener`は正常に登録されるを通知して、サービスの登録を解除するために使用します。

ネットワークの実装上のサービスを検出する`Nsd.DiscoveryListener`に渡される`NsdManager.discoverServices()`します。



#### <a name="network-usage"></a>ネットワーク使用率

新しいメソッド、`ConnectivityManager.IsActiveNetworkMetered`デバイスを従量制ネットワークに接続されているかどうかを確認できます。 このメソッドは、データ操作の負荷の高い料金が存在する可能性をユーザーに正確に通知してデータの使用状況を管理するために使用できます。



#### <a name="wifi-direct-service-discovery"></a>WiFi ダイレクト サービスの検出

`WifiP2pManager`クラスがサポートするために Android 4.0 で導入された*zeroconf*します。 Zeroconf (0 はネットワークを構成する) は、人間のネットワーク オペレーター、または特別な構成サーバーの操作でのデバイス (コンピューター、プリンター、スマート フォン) を自動的には、ネットワークに接続を許可する手法のセットです。

Jelly Bean で`WifiP2pManager`近くのいずれかを使用してデバイスを検出できる*Bonjour*または*Upnp*します。 Bonjour は、zeroconf の Apple の実装です。 Upnp は zeroconf をサポートするネットワーク プロトコルのセットです。 次のメソッドに追加、 `WiFiP2pManager` Wi-fi サービス検出をサポートします。

-   `AddLocalService()` – このメソッドが使用されるピア検出 Wi-fi 経由でサービスとしてアプリケーションを発表します。
-   `AddServiceRequest(` ) – この方法は、フレームワークにサービス探索要求を送信します。 Wi-fi のサービスの検出を初期化するために使用されます。
-   `SetDnsSdResponseListeners()` – このメソッドは、Bonjour から検出要求に対する応答を受け取ると呼び出されるコールバックを登録に使用されます。
-   `SetUpnpServiceResponseListener()` – このメソッドは、Upnp の検出要求に対する応答を受け取ると呼び出されるコールバックを登録に使用されます。




### <a name="content-providers"></a>コンテンツ プロバイダー

`ContentResolver`クラスには、新しいメソッドが受信した`AcquireUnstableContentProvider`します。 このメソッドは、「不安定」のコンテンツ プロバイダーを取得するアプリケーションを使用します。 通常、クラッシュ、アプリケーションがコンテンツ プロバイダー、およびそのコンテンツ プロバイダーを獲得する場合、する際に、アプリケーション。 このメソッドの呼び出しで、コンテンツ プロバイダーがクラッシュした場合、アプリケーションがクラッシュしません。 代わりに、`Android.OS.DeadObjectionException`コンテンツ プロバイダーがなくなったアプリケーションを通知するために、コンテンツ プロバイダーの呼び出しからスローされます。 「不安定」のコンテンツ プロバイダーは、他のアプリケーションからコンテンツ プロバイダーとやり取りするときに便利です – 別のアプリケーションからのバグのあるコードが別のアプリケーションに影響する可能性が低くなります。



### <a name="copy-and-paste-with-intents"></a>コピーと貼り付けのインテント

`Intent`クラスを指定できます、`ClipData`経由でそれに関連付けられているオブジェクト、`Intent.ClipData`プロパティ。 このメソッドは、クリップボードから余分なデータの目的で送信できます。 インスタンス`ClipData`1 つまたは複数含めることができます`ClipData.Item`します。 `ClipData.Item`次の種類のアイテムには。

-   **テキスト**– これは、任意の文字列のいずれかの HTML または組み込みの Android スタイルによって形式がサポートされている任意の文字列にまたがます。
-  **インテント**: 任意`Intent`オブジェクト。
-   **Uri** – HTTP のブックマークをやコンテンツ プロバイダーへの URI など、任意の URI を指定できます。




### <a name="isolated-services"></a>分離サービス

独立したサービスは、サービスを独自の特別なプロセスで実行され、独自のアクセス許可がありません。 サービスとのみ通信は、ときにサービスを開始して、サービス API を使用してバインドします。 プロパティを設定して、サービスを分離としてを宣言することは`IsolatedProcess="true"`で、`ServiceAttribute`サービス クラスのものです。


### <a name="media"></a>メディア

新しい`Android.Media.MediaCodec`クラスには、低レベルの media コーデックの API が用意されています。 アプリケーションでは、どのような低レベルのコーデックは、デバイスで使用可能なシステムをクエリできます。

新しい`Android.Media.Audiofx.AudioEffect`処理前のキャプチャされたオーディオの追加のオーディオをサポートするサブクラスが追加されています。

-   `Android.Media.Audiofx.AcousticEchoCanceler` -このクラスは、キャプチャしたオーディオ信号からリモート パーティからの信号を削除するオーディオを事前処理するために使用されます。 たとえば、音声通信のアプリケーションからエコーを削除しています。
-   `Android.Media.Audiofx.AutomaticGainControl` -このクラスは、出力シグナルが定数のように入力信号を下げるかコレクション ブースティングによってキャプチャされた信号を正規化に使用されます。
-   `Android.Media.Audiofx.NoiseSuppressor` – このクラスは、キャプチャされた信号からバック グラウンド ノイズを除去します。


すべてのデバイスでは、これらの効果をサポートします。 メソッド`AudioEffect.IsAvailable`アプリケーションを実行しているデバイスの問題のオーディオ効果がサポートされているかどうかに、アプリケーションで呼び出す必要があります。

`MediaPlayer`クラスようになりましたで再生をすきまがなく、`SetNextMediaPlayer()`メソッド。 この新しいメソッドでは、現在、media player の再生を終了するときに開始する次の MediaPlayer を指定します。

次の新しいクラスは、メディアを再生するかを選択するための標準的なメカニズムと UI を提供します。

-   `MediaRouter` -このクラスは、外部のスピーカーにデバイスまたはその他のデバイスからメディア チャネルのルーティングを制御するアプリケーションを使用できます。
-   `MediaRouterActionProvider` `MediaRouteButton` – これらのクラスを選択して、メディアの再生の一貫性のある UI を提供するのに役立ちます。




### <a name="notifications"></a>通知

Android 4.1 では、柔軟性とコントロール通知の表示、アプリケーションができます。 アプリケーションでは、ユーザーにより大きく、性能の通知を表示できます。 新しいメソッド、`NotificationBuilder.SetStyle()`通知に設定するための新しい 3 つの新しいスタイルのいずれか。

-   `Notification.BigPictureStyle` – これは、それらのイメージが通知を生成するヘルパー クラスです。 次の図は、大きなイメージを使用して、通知の例を示します。


 [![BigPictureStyle 通知の例のスクリーン ショット](jelly-bean-images/image2.png)](jelly-bean-images/image2.png#lightbox)

-   `Notification.BigTextStyle` – これは、テキスト、電子メールなどの複数の行がある通知を生成するヘルパー クラスです。 この新しい通知スタイルの例は、次のスクリーン ショットで確認できます。


 [![BigTextStyle 通知の例のスクリーン ショット](jelly-bean-images/image3.png)](jelly-bean-images/image3.png#lightbox)

-   `Notification.InboxStyle` – これは、このスクリーン ショットで示すように、電子メール メッセージからのスニペットなどの文字列のリストを含めることが通知を生成するヘルパー クラスです。


 [![Notification.InboxStyle 通知の例のスクリーン ショット](jelly-bean-images/image4.png)](jelly-bean-images/image4.png#lightbox)

通知が通常以上のスタイルを使用する場合、通知メッセージの下部にある最大 2 つのアクション ボタンを追加することになります。
この例は、アクション ボタンが、通知の下部に表示する、次のスクリーン ショットで確認できます。

 [![通知メッセージの下に表示されるアクション ボタンのスクリーン ショットの例](jelly-bean-images/image5.png)](jelly-bean-images/image5.png#lightbox)

`Notification`クラスが通知の 5 つの優先度レベルのいずれかを指定できるようにする新しい定数を受信します。 使用して通知を設定するには、`Priority`プロパティ。



### <a name="permissions"></a>アクセス許可

次の新しいアクセス許可が追加されました。

-   `READ_EXTERNAL_STORAGE` -アプリケーションには、外部ストレージに読み取り専用アクセスが必要です。 現在のすべてのアプリケーションが既定では、読み取りアクセス権があるが、今後のリリースの Android アプリケーションは、読み取りアクセスを明示的に要求が必要になります。
-   `READ_USER_DICTIONARY` -ユーザーの word の辞書への読み取りアクセスを許可します。
-   `READ_CALL_LOG` -呼び出しのログの読み取り、着信および発信呼び出しに関する情報を取得するアプリケーションを許可します。
-   `WRITE_CALL_LOG` -携帯電話の呼び出しのログに書き込むアプリケーションを許可します。
-   `WRITE_USER_DICTIONARY` -ユーザーの word の辞書に書き込むアプリケーションを許可します。


重要な変更に注意してください`READ_EXTERNAL_STORAGE`– Android でこのアクセス許可を自動的に付与されています。 Android の将来のバージョンは、アクセス許可を付与する前にこのアクセス許可を要求するアプリケーションを必要があります。



## <a name="summary"></a>まとめ

この記事では、いくつか、新しい API の Android 4.1 (API レベル 16) で利用できるので導入されました。 アニメーションと、アクティビティの起動をアニメーション化の変更の一部を強調表示し、新しい API の Bonjour、UPnP などのプロトコルを使用して他のデバイスのネットワーク探索を導入します。 API への他の変更はされた独立したサービスまたは「不安定」のコンテンツ プロバイダーを使用する機能、インテントを使用してデータをコピー アンド ペーストを機能なども強調表示されます。

この記事では、しに通知を更新プログラムを導入するが発生し、Android 4.1 で導入された新しいアクセス許可の説明


## <a name="related-links"></a>関連リンク

- [時間のアニメーション例 (サンプル)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TimeAnimatorExample/)
- [Android 4.1 Api](https://developer.android.com/about/versions/android-4.1.html)
- [タスク ウィンドウとバック スタック](https://developer.android.com/guide/components/tasks-and-back-stack.html)
- [[戻る] と アップによるナビゲーション](https://developer.android.com/design/patterns/navigation.html)
