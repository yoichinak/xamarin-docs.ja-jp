---
title: "ゼリー Bean の機能"
description: "このドキュメントでは、Android 4.1 で導入された開発者向けの新機能の高レベルの概要を説明します。 これらの機能が含まれます: Android ビーム マルチ メディア、ピア ツー ピア ネットワーク探索、アニメーション、新しいアクセス許可の更新プログラム、大きなファイルを共有する更新プログラムの通知を強化します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 136484644779ac40e661f50ff19cf15884c864c2
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="jelly-bean-features"></a>ゼリー Bean の機能

_このドキュメントでは、Android 4.1 で導入された開発者向けの新機能の高レベルの概要を説明します。これらの機能が含まれます: Android ビーム マルチ メディア、ピア ツー ピア ネットワーク探索、アニメーション、新しいアクセス許可の更新プログラム、大きなファイルを共有する更新プログラムの通知を強化します。_



## <a name="overview"></a>概要

Android 4.1 (API レベル 16、)、ゼリー Bean"とも呼ばれる"が、2012 年 7 月 9 日にリリースしました。 この記事では、Android 4.1 の新機能のいくつかに高レベルな概要を Xamarin.Android を使用する開発者に提供します。 導入されたこれらの新機能の一部は、アクティビティ、カメラ、新しいサウンドとアプリケーション スタックのナビゲーションのサポートの改善を起動するためのアニメーションの機能強化です。 切り取りし、意図的での貼り付けをすることは今すぐです。

Android アプリケーションの安定性が不安定なコンテンツ プロバイダーへの依存関係を分離する機能を向上します。 サービスもあります分離は、それらを開始するアクティビティによってのみアクセスできるようにします。

サポートが追加されましたネットワーク探索を使用してサービスの Bonjour、UPnP、またはマルチキャスト DNS ベースのサービスです。 可能であれば今すぐテキスト、アクション ボタンと大きいイメージの形式が豊富に通知します。

最後に Android 4.1 では、いくつかの新しいアクセス許可が追加されました。



## <a name="requirements"></a>必要条件

Xamarin.Android アプリケーションの開発に使用するゼリー Bean 必要 Xamarin.Android 4.2.6 または高い値と Android 4.1 (API レベル 16) は、次のスクリーン ショットに示すように、Android SDK Manager を使用してにインストールします。

[![Android SDK Manager で Android 4.1 の選択](jelly-bean-images/image1.png)](jelly-bean-images/image1.png#lightbox)



## <a name="whats-new"></a>新機能



### <a name="animations"></a>Animations

使用してズーム アニメーションまたはカスタム アニメーションのいずれかを使用してアクティビティを起動する可能性があります、`ActivityOptions`クラスです。 これらのアニメーションをサポートするためには、次の新しいメソッドが用意されています。

-   `MakeScaleUpAnimation` – これは、開始位置と、画面上のサイズから、[アクティビティ] ウィンドウを拡大縮小するアニメーションで作成されます。
-   `MakeThumbnailScaleUpAnimation` – これは、画面上の指定位置からサムネイル イメージからに応じてスケール アップするアニメーションを作成します。
-   `MakeCustomAnimation` – これは、アプリケーションのリソースからアニメーションを作成します。 アクティビティが起動したときに 1 つのアニメーションと別のアクティビティを停止する場合があります。


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

クラスのインスタンスを使用して今すぐ`TimeAnimator`が作成され、リスナーを設定。

```csharp
var animator = new TimeAnimator();
animator.SetTimeListener(new MyTimeListener());
animator.Start();
```

として、`TimeAnimator`インスタンスが実行されているを呼び出す`ITimeAnimator.ITimeListener`方法がログインし、長いアニメーターされました実行時間それ以来、最後に、メソッドとして呼び出されています。



### <a name="application-stack-navigation"></a>アプリケーション スタックのナビゲーション

Android 4.1 は、Android 3.0 で導入されたアプリケーション スタックのナビゲーションを向上します。 指定して、`ParentName`のプロパティ、 `ActivityAttribute`、Android は、ユーザーが押すと、適切な親アクティビティを開くことができます、 [ ボタン](http://developer.android.com/design/patterns/navigation.html#up-vs-back)Android 操作バーで、で指定されたアクティビティがインスタンス化され`ParentName`プロパティです。 これにより、特定のタスクを構成するアクティビティの階層を保持するためにアプリケーションです。

ほとんどのアプリケーションの設定、`ParentName`アクティビティではアプリケーション スタックを移動するため、正しい動作を提供する Android 用の十分な情報Android では、インテントの親アクティビティごとの系列を作成することで、必要なバック スタックを合成するされます。 ただし、これは、架空のアプリケーション スタックであるため、各代理の活動は、自然なアクティビティが、保存された状態がありません。 アクティビティはオーバーライド代理親アクティビティに保存された状態を提供する、`OnPrepareNavigationUpTaskStack`メソッドです。 このメソッドは受信、 `TaskStackBuilder` Android の使用は、バック スタックを作成することを目的のコレクションを持つインスタンスのオブジェクトします。 アクティビティは代理の活動が作成されると、適切な状態情報を受信するため、これらの意図的に変更できます。

複雑なシナリオにはナビゲーションの動作の処理し、バック スタックを構築するために使用するアクティビティ クラスに新しいメソッドがあります。

-   `OnNavigateUp` – このメソッドをオーバーライドすることが、カスタム アクションを実行するときに、<span class="ui">を</span>ボタンが押されました。
-   `NavigateUpTo` – このメソッドを呼び出すと、特定の目的で指定されたアクティビティに、現在のアクティビティから移動するアプリケーションが発生します。
-   `ParentActivityIntent` – これは、現在のアクティビティの親アクティビティを起動する目的の取得に使用します。
-   `ShouldUpRecreateTask` – この方法は、使用して照会かどうかは合成バック スタックを作成して、親アクティビティに移動する必要があります。 返します`true`場合は、合成のスタックを作成する必要があります。 
-   `FinishAffinity` – このメソッドを呼び出すと、現在の活動と活動をすべて下にある現在のタスクを同じタスクのアフィニティを持つが完了します。
-   `OnCreateNavigateUpTaskStack` – この方法は、合成のスタックを作成する方法を完全に制御する必要がある場合はオーバーライドされます。




### <a name="camera"></a>カメラ

新しいインターフェイスがある`Camera.IAutoFocusMoveCallback`、自動フォーカスが開始または移動を停止したときを検出するために使用できます。 この新しいインターフェイスの例は、次のスニペットで参照できます。

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

新しいクラス`MediaActionSound`さまざまなメディアのアクションのサウンドを生成するための API のセットを提供します。 これらは、列挙型によって定義されます、カメラで発生することがいくつかの操作`Android.Media.MediaActionSoundType`:

-   `MediaActionSoundType.FocusComplete` – この再生されるサウンドをフォーカス設定が完了するとします。
-   `MediaActionSoundType.ShutterClick` – 静止画像写真を撮るときに、このサウンドが再生されます。
-   `MediaActionSoundType.StartVideoRecording` – このサウンドが使用するビデオ記録の開始を示します。
-   `MediaActionSoundType.StopVideoRecording` – ビデオ記録の終了を示すこのサウンドが再生されます。


使用する方法の例、`MediaActionSound`クラスは、次のスニペットで参照できます。

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

Android のビームは、互いに通信するために 2 つの Android デバイスでできる NFC ベース テクノロジです。 Android 4.1 では、大きなファイルの転送を適切にサポートを提供します。 新しいメソッドを使用して`NfcAdapter.SetBeamPushUris()`Android 切り替えることは代替のトランスポート機構 (Bluetooth) など、高速転送速度を実現するためにします。



#### <a name="network-services-discovery"></a>ネットワーク サービスの検出

Android 4.1 には、DNS ベースのマルチキャスト サービス検出用の新しい API が含まれています。
これにより、アプリケーションを検出し、プリンター、カメラ、およびメディア デバイスなどの他のデバイスに Wi-fi 経由で接続できます。 これらの新しい API は、`Android.Net.Nsd`パッケージです。

他のサービスが消費されるサービスを作成する、`NsdServiceInfo`クラスは、サービスのプロパティを定義するオブジェクトを作成するために使用します。 このオブジェクトは提供され、`NsdManager.RegisterService()`の実装と共に`NsdManager.ResolveListener`です。 実装`NsdManager.ResolveListener`登録の成功を通知して、サービスの登録を解除に使用されます。

実装と、ネットワーク上のサービスを検出する`Nsd.DiscoveryListener`に渡される`NsdManager.discoverServices()`です。



#### <a name="network-usage"></a>ネットワーク使用率

新しいメソッド`ConnectivityManager.IsActiveNetworkMetered`デバイスを従量制ネットワークに接続されているかどうかを確認できます。 このメソッドは、正確にユーザーに通知するデータの操作の負荷の高い料金が存在する可能性がありますでデータの使用状況を管理するために使用できます。



#### <a name="wifi-direct-service-discovery"></a>WiFi ダイレクト サービスの検出

`WifiP2pManager`クラスがサポートするために Android 4.0 で導入された*zeroconf*です。 Zeroconf (0 はネットワークを構成する) は、人間のネットワーク オペレーター、または特別な構成サーバーの操作でデバイス (コンピューター、プリンター、携帯電話) に自動的には、ネットワークに接続できるようにする一連の技法がします。

ゼリー Bean の`WifiP2pManager`最寄りのいずれかを使用してデバイスを検出できる*Bonjour*または*Upnp*です。 Bonjour は、zeroconf の Apple の実装です。 Upnp は zeroconf をサポートするネットワーク プロトコルの設定されています。 次のメソッドに追加された、 `WiFiP2pManager` Wi-fi サービス検出をサポートします。

-   `AddLocalService()` このメソッドは、使用のピアで検出、Wi-fi 経由でサービスとしてアプリケーションを発表します。
-   `AddServiceRequest(` ) – この方法は、フレームワークにサービス探索要求を送信します。 Wi-fi サービスの検出を初期化するために使用されます。
-   `SetDnsSdResponseListeners()` – この方法は Bonjour から検出要求に対する応答を受け取ると呼び出されるコールバックを登録するために使用します。
-   `SetUpnpServiceResponseListener()` – この方法は検出要求 Upnp への応答を受け取ると呼び出されるコールバックを登録するために使用します。




### <a name="content-providers"></a>コンテンツ プロバイダー

`ContentResolver`クラスの新しいメソッドが受信した`AcquireUnstableContentProvider`です。 このメソッドは、「不安定」のコンテンツ プロバイダーを取得するアプリケーションを使用します。 通常、クラッシュしたアプリケーションが、コンテンツ プロバイダーとそのコンテンツ プロバイダーを獲得する場合、アプリケーションもロールバックされます。 このメソッドの呼び出しでは、コンテンツ プロバイダーがクラッシュした場合場合にいない、アプリケーションがクラッシュします。 代わりに、`Android.OS.DeadObjectionException`コンテンツ プロバイダーがなくなったアプリケーションを通知するために、コンテンツ プロバイダーの呼び出しからスローされます。 「不安定」のコンテンツ プロバイダーは、他のアプリケーションからのコンテンツ プロバイダーと対話するときに役立ちます: 別のアプリケーションからのバグのあるコードが別のアプリケーションに影響することはあまり考えられません。



### <a name="copy-and-paste-with-intents"></a>コピーと貼り付けの目的で

`Intent`クラスが持つができるようになりました、`ClipData`オブジェクトを使用してそれに関連付けられている、`Intent.ClipData`プロパティです。 このメソッドは、クリップボードから余分なデータの目的で転送することができます。 インスタンス`ClipData`1 つ以上含めることができます`ClipData.Item`です。 `ClipData.Item`次の種類のアイテムであります。

-   **テキスト**– これは、任意の文字列のいずれかの HTML または組み込み Android スタイルでフォーマットがサポートされている任意の文字列にまたがるです。
-  **インテント**: 任意`Intent`オブジェクト。
-   **Uri** – HTTP ブックマークやコンテンツ プロバイダーへの URI などの任意の URI を指定できます。




### <a name="isolated-services"></a>分離サービス

分離のサービスは、ですが、独自の特殊なプロセスで実行して、独自のアクセス許可を持たないサービスです。 場合にのみ、サービスとの通信は、サービスを開始し、サービス API を使用してバインドします。 プロパティを設定して、サービスを分離としてを宣言することは`IsolatedProcess="true"`で、`ServiceAttribute`サービス クラスを adorns です。


### <a name="media"></a>メディア

新しい`Android.Media.MediaCodec`クラスは低レベルの media コーデックの API を提供します。 アプリケーションでは、デバイスにどのような低レベルのコーデックは、システムを照会できます。

新しい`Android.Media.Audiofx.AudioEffect`サブクラスが処理前のキャプチャされたオーディオの他のオーディオをサポートするために追加されました。

-   `Android.Media.Audiofx.AcousticEchoCanceler` このクラスはキャプチャされたオーディオ信号からリモート パーティからの信号を削除するオーディオを事前処理するために使用されます。 たとえば、音声通信アプリケーションからエコーを削除しています。
-   `Android.Media.Audiofx.AutomaticGainControl` このクラスは、ブースティングまたは出力信号が定数になるように入力信号を下げることによってキャプチャされた信号を正規化する使用されます。
-   `Android.Media.Audiofx.NoiseSuppressor` このクラスは、キャプチャされた信号からバック グラウンド ノイズを除去します。


すべてのデバイスでは、これらの効果をサポートします。 メソッド`AudioEffect.IsAvailable`オーディオ効果の問題が、アプリケーションを実行しているデバイスでサポートされているかどうかのアプリケーションによって呼び出す必要があります。

`MediaPlayer`クラスようになりましたで再生するすきまがなく、`SetNextMediaPlayer()`メソッドです。 この新しいメソッドは、現在 media player の再生を終了するときに開始する次の media Player を指定します。

次の新しいクラスは、メディアの再生位置を選択するための標準メカニズムおよび UI を提供します。

-   `MediaRouter` このクラスは、外部のスピーカーにデバイスまたはその他のデバイスからのメディア チャネルのルーティングを制御するアプリケーションを使用できます。
-   `MediaRouterActionProvider` および`MediaRouteButton`– これらのクラスにより、一貫性のある UI を選択し、メディアを再生します。




### <a name="notifications"></a>通知

アプリケーションは、android 4.1 が柔軟性が向上し通知を表示するコントロールにできます。 アプリケーションは、ユーザーにより良くより大きな通知を表示することができますようになりました。 新しいメソッド`NotificationBuilder.SetStyle()`では、新しい 3 つの新しいスタイルのいずれかの通知を設定します。

-   `Notification.BigPictureStyle` – これは、それらのイメージを持っている通知を生成するヘルパー クラスです。 次の図は、大規模なイメージでの通知の例を示します。


 [![BigPictureStyle 通知のサンプルのスクリーン ショット](jelly-bean-images/image2.png)](jelly-bean-images/image2.png#lightbox)

-   `Notification.BigTextStyle` – これは、テキスト、電子メールなどの複数の行がある通知を生成するヘルパー クラスです。 この新しい通知形式の例は、次のスクリーン ショットに表示できます。


 [![BigTextStyle 通知のサンプルのスクリーン ショット](jelly-bean-images/image3.png)](jelly-bean-images/image3.png#lightbox)

-   `Notification.InboxStyle` – これは、このスクリーン ショットに示すように、電子メール メッセージからスニペットなど、文字列のリストを含む通知を生成するヘルパー クラスです。


 [![Notification.InboxStyle 通知のサンプルのスクリーン ショット](jelly-bean-images/image4.png)](jelly-bean-images/image4.png#lightbox)

これは通常以上のスタイルを使用しているとき、通知、通知メッセージの下部に最大 2 つのアクション ボタンを追加することです。
この例は、アクション ボタンが、通知の下部に表示されている、次のスクリーン ショットに表示されることができます。

 [![通知メッセージの下に表示されるアクション ボタンの例のスクリーン ショット](jelly-bean-images/image5.png)](jelly-bean-images/image5.png#lightbox)

`Notification`クラスが、開発者に通知を 5 つの優先度レベルのいずれかを指定できるようにする新しい定数を受信します。 これらを使用して通知に対して設定できる、`Priority`プロパティです。



### <a name="permissions"></a>アクセス許可

次の新しいアクセス許可が追加されました。

-   `READ_EXTERNAL_STORAGE` -アプリケーションでは、外部ストレージに読み取り専用アクセスが必要です。 現在のすべてのアプリケーションが既定では、読み取りアクセス権があるが、Android の将来のリリースのアプリケーションは、読み取りアクセスを明示的に要求が必要になります。
-   `READ_USER_DICTIONARY` -ユーザーの word ディクショナリへの読み取りアクセスを許可します。
-   `READ_CALL_LOG` -アプリケーションの呼び出しのログの読み取りで受信および送信呼び出しに関する情報を取得できます。
-   `WRITE_CALL_LOG` -電話の呼び出しのログに書き込むアプリケーションをできます。
-   `WRITE_USER_DICTIONARY` -アプリケーションにユーザーの word ディクショナリへの書き込みを許可します。


注意する重要な変更`READ_EXTERNAL_STORAGE`– Android によってこのアクセス許可を自動的に付与されています。 Android の将来のバージョンは、権限を許可する前にこのアクセス許可を要求するアプリケーションを必要があります。



## <a name="summary"></a>まとめ

この記事では、いくつかの新しい API の Android 4.1 (API レベル 16) で使用可能な導入されました。 アニメーション、および、アクティビティの起動をアニメーション化の変更の一部を強調表示されているし、新しい API の Bonjour、UPnP などのプロトコルを使用して他のデバイスのネットワーク探索が導入されました。 API への他の変更が強調表示されている同様に、切り取りと貼り付けインテントを使用してデータを分離されたサービスまたは「不安定」のコンテンツ プロバイダーを使用できる機能などです。

この記事に通知を更新プログラムを導入したし、Android 4.1 で導入された新しいアクセス許可の説明


## <a name="related-links"></a>関連リンク

- [Time アニメーションの例 (サンプル)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TimeAnimatorExample/)
- [Android 4.1 Api](http://developer.android.com/about/versions/android-4.1.html)
- [タスク ウィンドウとバック スタック](http://developer.android.com/guide/components/tasks-and-back-stack.html)
- [[戻る] と アップによるナビゲーション](http://developer.android.com/design/patterns/navigation.html)
