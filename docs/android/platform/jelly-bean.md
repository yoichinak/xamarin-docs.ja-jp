---
title: Jelly Bean の機能
description: このドキュメントでは、Android 4.1 で導入された開発者向けの新機能の概要について説明します。 これらの機能には、強化された通知、大きなファイルを共有するための Android ビームの更新、マルチメディアの更新、ピアツーピア ネットワーク探索、アニメーション、新しいアクセス許可が含まれます。
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 55969993a4cb3755f5a26e681ae21bf993307a0a
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91456887"
---
# <a name="jelly-bean-features"></a>Jelly Bean の機能

_このドキュメントでは、Android 4.1 で導入された開発者向けの新機能の概要について説明します。これらの機能には、強化された通知、大きなファイルを共有するための Android ビームの更新、マルチメディアの更新、ピアツーピア ネットワーク探索、アニメーション、新しいアクセス許可が含まれます。_

## <a name="overview"></a>概要

Android 4.1 (API レベル 16) (別名 "Jelly Bean") は、2012 年 7 月 9 日にリリースされました。 この記事では、Xamarin.Android を使用する開発者向け Android 4.1 の新機能の概要について説明します。 導入されたこれらの新機能には、アクティビティを開始するためのアニメーションの強化、カメラ用の新しいサウンド、およびアプリケーション スタック ナビゲーションのサポートの向上などがあります。 インテントを使用して切り取りと貼り付けを実行できるようになりました。

不安定なコンテンツ プロバイダーへの依存関係を分離できるようになったことで、Android アプリケーションの安定性が向上しました。 開始元のアクティビティのみがアクセスできるようにサービスも分離できます。

Bonjour、UPnP、またはマルチキャスト DNS に基づくサービスを使用したネットワーク サービス探索のサポートが追加されました。 書式設定されたテキスト、アクション ボタン、および大きな画像を含む、より高度な通知を利用できるようになりました。

最後に、いくつかの新しいアクセス許可が Android 4.1 に追加されました。

## <a name="requirements"></a>必要条件

Jelly Bean を使用して Xamarin.Android アプリケーションを開発するには、Xamarin.Android 4.2.6 以降と、Android SDK Manager を使用して Android 4.1 (API レベル 16) をインストールする必要があります (次のスクリーンショットを参照)。

[![Android SDK Manager で Android 4.1 を選択](jelly-bean-images/image1.png)](jelly-bean-images/image1.png#lightbox)

## <a name="whats-new"></a>新着記事

### <a name="animations"></a>Animations

`ActivityOptions` クラスを使用することにより、ズーム アニメーションまたはカスタム アニメーションを使用してアクティビティを開始できます。 これらのアニメーションをサポートするために、次の新しいメソッドが用意されています。

- `MakeScaleUpAnimation` - 画面上の開始位置とサイズからアクティビティ ウィンドウをスケールアップするアニメーションを作成します。
- `MakeThumbnailScaleUpAnimation` - 画面上の指定した位置からサムネイル画像をスケールアップするアニメーションを作成します。
- `MakeCustomAnimation` - アプリケーションのリソースからアニメーションを作成します。 アクティビティが開いたときとアクティビティが停止したときのアニメーションが 1 つずつあります。

新しい `TimeAnimator` クラスには、アニメーションでフレームが変化するたびにアプリケーションに通知できるインターフェイス `TimeAnimator.ITimeListener` が用意されています。 たとえば、次の `TimeAnimator.ITimeListener` の実装について考えてみます。

```csharp
class MyTimeListener : Java.Lang.Object,  TimeAnimator.ITimeListener
{
    public void OnTimeUpdate(TimeAnimator animation, long totalTime, long deltaTime)
    {
        Log.Debug("Activity1", "totalTime={0}, deltaTime={1}", totalTime, deltaTime);
    }
}
```

そして次に、クラスを使用するために、`TimeAnimator` のインスタンスが作成され、リスナーが設定されます。

```csharp
var animator = new TimeAnimator();
animator.SetTimeListener(new MyTimeListener());
animator.Start();
```

`TimeAnimator` インスタンスは実行されているときに `ITimeAnimator.ITimeListener` を呼び出します。これにより、アニメーターが実行されている時間と、前回メソッドが呼び出されてからの経過時間がログに記録されます。

### <a name="application-stack-navigation"></a>アプリケーション スタックのナビゲーション

Android 4.1 では、Android 3.0 で導入されたアプリケーション スタックのナビゲーションが向上しています。 `ActivityAttribute` の `ParentName` プロパティを指定することにより、ユーザーが操作バーの[上方向ボタン](https://developer.android.com/design/patterns/navigation.html#up-vs-back)を押したときに、Android は適切な親アクティビティを開くことができます。Android は、`ParentName` プロパティによって指定されたアクティビティをインスタンス化します。 これにより、アプリケーションは、特定のタスクを形成するアクティビティの階層を維持できます。

ほとんどのアプリケーションでは、アクティビティに `ParentName` を設定することで、Android がアプリケーション スタックをナビゲートするための正しい動作を提供するのに十分な情報となります。Android は、親アクティビティごとに一連のインテントを作成して、必要なバック スタックを合成します。 ただし、これは人工的なアプリケーション スタックであるため、各合成アクティビティには、自然なアクティビティにはある保存された状態がありません。 保存された状態を合成の親アクティビティに提供するために、アクティビティは `OnPrepareNavigationUpTaskStack` メソッドをオーバーライドする場合があります。 このメソッドは、Android がバック スタックを作成するために使用する Intent オブジェクトのコレクションを持つ `TaskStackBuilder` インスタンスを受け取ります。 アクティビティは、合成アクティビティが作成されるときに適切な状態情報を受け取れるように、これらのインテントを変更することがあります。

より複雑なシナリオでは、Activity クラスに新しいメソッドがあり、それを使用して、上方向へのナビゲーションの動作を処理し、バック スタックを構築できます。

- `OnNavigateUp` - このメソッドをオーバーライドすることにより、**上方向**ボタンが押されたときにカスタム アクションを実行することができます。
- `NavigateUpTo` - このメソッドを呼び出すと、アプリケーションは現在のアクティビティから、特定のインテントによって指定されたアクティビティにナビゲートします。
- `ParentActivityIntent` - これは、現在のアクティビティの親アクティビティを開始するインテントを取得するために使用されます。
- `ShouldUpRecreateTask` - このメソッドは、親アクティビティにナビゲートするために、合成バック スタックを作成する必要があるかどうかのクエリを実行するために使用されます。 合成スタックを作成する必要がある場合は、`true` が返されます。 
- `FinishAffinity` - このメソッドを呼び出すと、同じタスク アフィニティを持つ、現在のタスクにある現在のアクティビティとその下のすべてのアクティビティが終了します。
- `OnCreateNavigateUpTaskStack` - このメソッドは、合成スタックの作成方法を完全に制御する必要がある場合にオーバーライドされます。

### <a name="camera"></a>Camera

新しいインターフェイス `Camera.IAutoFocusMoveCallback` があります。これを使用すると、いつ自動フォーカスが動きを開始または停止したかを検出できます。 この新しいインターフェイスの例を次のスニペットに示します。

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

新しいクラス `MediaActionSound` には、さまざまなメディア操作に対してサウンドを生成するための API のセットが用意されています。 カメラで行われる可能性があるいくつかの操作があります。これらは列挙型 `Android.Media.MediaActionSoundType` によって定義されています。

- `MediaActionSoundType.FocusComplete` - このサウンドは、ピント合わせが完了したときに再生されます。
- `MediaActionSoundType.ShutterClick` - このサウンドは、静止画像の写真が取られるときに再生されます。
- `MediaActionSoundType.StartVideoRecording` - このサウンドは、録画の開始を示すために使用されます。
- `MediaActionSoundType.StopVideoRecording` - このサウンドは、録画の終了を示すために使用されます。

`MediaActionSound` クラスの使用方法の例を次のスニペットに示します。

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

Android ビームは、2 つの Android デバイスが相互に通信できるようにする NFC ベースのテクノロジです。 Android 4.1 では、大きなファイルの転送に対するサポートが強化されています。 新しいメソッド `NfcAdapter.SetBeamPushUris()` を使用すると、Android は別の転送メカニズム (Bluetooth など) との間で切り替えを行って、高速の転送速度を実現します。

#### <a name="network-services-discovery"></a>ネットワーク サービスの探索

Android 4.1 には、マルチキャスト DNS ベースのサービス探索用の新しい API が含まれています。
これにより、アプリケーションはプリンター、カメラ、メディア デバイスなどの他のデバイスを検出して Wi-Fi 経由で接続できます。 これらの新しい API は、`Android.Net.Nsd` パッケージに含まれています。

他のサービスによって使用される可能性のあるサービスを作成するには、`NsdServiceInfo` クラスを使用して、サービスのプロパティを定義するオブジェクトを作成します。 このオブジェクトは、`NsdManager.ResolveListener` の実装と共に `NsdManager.RegisterService()` に提供されます。 `NsdManager.ResolveListener` の実装は、正常に登録されたことを通知するため、およびサービスの登録を解除するために使用されます。

ネットワーク上のサービスを探索するために、`Nsd.DiscoveryListener` の実装が `NsdManager.discoverServices()` に渡されます。

#### <a name="network-usage"></a>ネットワーク使用率

新しいメソッド `ConnectivityManager.IsActiveNetworkMetered` を使用すると、デバイスは従量制課金ネットワークに接続しているかどうかを確認できます。 このメソッドを使用すると、データ操作に対して高額な課金の可能性があることをユーザーに正確に通知することで、データの使用状況を管理できます。

#### <a name="wifi-direct-service-discovery"></a>WiFi Direct サービス探索

`WifiP2pManager` クラスは、*zeroconf* をサポートするために Android 4.0 で導入されました。 Zeroconf (ゼロ構成ネットワーク) とは、人間のネットワーク オペレーターや特別な構成サーバーの介入なしに、デバイス (コンピューター、プリンター、電話) が自動的にネットワークに接続することを可能にする一連の手法です。

Jelly Bean では、`WifiP2pManager` は *Bonjour* または *Upnp* を使用して近くのデバイスを探索できます。 Bonjour は、Apple による zeroconf の実装です。 Upnp はネットワーキング プロトコルのセットで、zeroconf もサポートしています。 Wi-Fi サービス探索をサポートするために、次のメソッドが `WiFiP2pManager` に追加されました。

- `AddLocalService()` - このメソッドは、ピアによる探索のために、アプリケーションを Wi-Fi 経由のサービスと宣言するために使用されます。
- `AddServiceRequest(` ) - このメソッドは、サービス探索要求をフレームワークに送信するためのものです。 これは、Wi-Fi サービス探索を初期化するために使用されます。
- `SetDnsSdResponseListeners()` - このメソッドは、Bonjour からの探索要求に対する応答を受信したときに呼び出されるコールバックを登録するために使用されます。
- `SetUpnpServiceResponseListener()` - このメソッドは、Upnp からの探索要求に対する応答を受信したときに呼び出されるコールバックを登録するために使用されます。

### <a name="content-providers"></a>コンテンツ プロバイダー

`ContentResolver` クラスは、新しいメソッド `AcquireUnstableContentProvider` を受け取りました。 このメソッドを使用すると、アプリケーションは "不安定な" コンテンツ プロバイダーを取得できます。 通常、アプリケーションがコンテンツ プロバイダーを取得し、そのコンテンツ プロバイダーがクラッシュすると、アプリケーションもクラッシュします。 このメソッド呼び出しを使用すると、コンテンツ プロバイダーがクラッシュしてもアプリケーションはクラッシュしません。 代わりに、コンテンツ プロバイダーに対する呼び出しから `Android.OS.DeadObjectionException` がスローされ、コンテンツ プロバイダーが見つからないことをアプリケーションに通知します。 "不安定な" コンテンツ プロバイダーは、他のアプリケーションからのコンテンツ プロバイダーと対話するときに役立ちます。あるアプリケーションのバグを含むコードが別のアプリケーションに影響を与える可能性は低くなります。

### <a name="copy-and-paste-with-intents"></a>インテントを使用してコピーと貼り付けを行う

`Intent` クラスは、`Intent.ClipData` プロパティを使用して `ClipData` オブジェクトを関連付けられるようになりました。 このメソッドを使用すると、クリップボードからの追加データをインテントを使用して送信できます。 `ClipData` のインスタンスには、1 つ以上の `ClipData.Item` を含めることができます。 `ClipData.Item` は、次の種類の項目です。

- **Text** - これは、任意のテキスト文字列 (HTML、または組み込みの Android スタイルの範囲でサポートされている形式を持つ任意の文字列) です。
- **Intent** 任意の `Intent` オブジェクト。
- **Uri** - これは、HTTP ブックマークや、コンテンツ プロバイダーの URI など、任意の URI です。

### <a name="isolated-services"></a>分離されたサービス

分離されたサービスとは、独自の特別なプロセスで実行され、独自のアクセス許可を持たないサービスです。 そのサービスとの通信は、サービスを開始するときと、サービス API を介してサービスにバインドするときだけです。 サービス クラスを装飾する `ServiceAttribute` にプロパティ `IsolatedProcess="true"` を設定することによって、サービスを分離されているものとして宣言できます。

### <a name="media"></a>Media

新しい `Android.Media.MediaCodec` クラスは、下位のメディア コーデックに API を提供します。 アプリケーションはシステムを照会して、デバイスで使用可能な下位のコーデックを調べることができます。

キャプチャしたオーディオに対する追加のオーディオ前処理をサポートするために、新しい `Android.Media.Audiofx.AudioEffect` サブクラスが追加されました。

- `Android.Media.Audiofx.AcousticEchoCanceler` - このクラスは、オーディオの前処理で、キャプチャしたオーディオ信号からリモート パーティの信号を除去するために使用されます。 たとえば、音声通信アプリケーションからエコーを除去します。
- `Android.Media.Audiofx.AutomaticGainControl` - このクラスは、出力信号が一定になるように入力信号を強めたり弱めたりすることによって、キャプチャされた信号を正規化するために使用されます。
- `Android.Media.Audiofx.NoiseSuppressor` - このクラスは、キャプチャされた信号から背景のノイズを除去します。

すべてのデバイスでこれらの効果がサポートされるわけではありません。 アプリケーションからメソッド `AudioEffect.IsAvailable` を呼び出して、該当するオーディオ エフェクトが、アプリケーションを実行しているデバイスでサポートされているかどうかを確認する必要があります。

`MediaPlayer` クラスで、`SetNextMediaPlayer()` メソッドを使用したギャップレス再生がサポートされるようになりました。 この新しいメソッドは、現在のメディア プレーヤーの再生が終了したときに開始する、次の MediaPlayer を指定します。

次の新しいクラスは、メディアが再生される場所を選択するための標準的なメカニズムと UI を提供します。

- `MediaRouter` - このクラスを使用すると、アプリケーションは、デバイスから外部スピーカーまたは他のデバイスへのメディア チャネルのルーティングを制御できます。
- `MediaRouterActionProvider` および `MediaRouteButton` - これらのクラスは、メディアを選択して再生するための一貫した UI を提供するのに役立ちます。

### <a name="notifications"></a>通知

Android 4.1 を使用すると、アプリケーションの柔軟性が向上し、通知の表示を制御することができます。 アプリケーションは、より大きく優れた通知をユーザーに表示できるようになりました。 新しいメソッド `NotificationBuilder.SetStyle()` を使用すると、3 つの新しいスタイルのいずれかを通知に設定できます。

- `Notification.BigPictureStyle` - これは、画像を含む通知を生成するヘルパー クラスです。 次の図は、大きな画像を含む通知の例を示しています。

 [![BigPictureStyle 通知のスクリーンショットの例](jelly-bean-images/image2.png)](jelly-bean-images/image2.png#lightbox)

- `Notification.BigTextStyle` - これは、電子メールなど、複数行のテキストを含む通知を生成するヘルパー クラスです。 この新しい通知スタイルの例を次のスクリーンショットに示します。

 [![BigTextStyle 通知のスクリーンショットの例](jelly-bean-images/image3.png)](jelly-bean-images/image3.png#lightbox)

- `Notification.InboxStyle` - これは、次のスクリーンショットに示すように、電子メール メッセージからのスニペットなど、文字列のリストを含む通知を生成するヘルパー クラスです。

 [![Notification.InboxStyle 通知のスクリーンショットの例](jelly-bean-images/image4.png)](jelly-bean-images/image4.png#lightbox)

通知で標準またはより大きいスタイルが使用されている場合は、通知メッセージの下部に最大 2 個のアクション ボタンを追加できます。
この例は、次のスクリーンショットで確認できます。ここでは、通知の下部にアクション ボタンが表示されています。

 [![通知メッセージの下に表示されているアクション ボタンのスクリーンショットの例](jelly-bean-images/image5.png)](jelly-bean-images/image5.png#lightbox)

`Notification` クラスは、開発者が通知に対して 5 つの優先度レベルのいずれかを指定できるようにする新しい定数を受け取りました。 これらは、`Priority` プロパティを使用して通知に設定できます。

### <a name="permissions"></a>アクセス許可

次に示す新しいアクセス許可が追加されました。

- `READ_EXTERNAL_STORAGE` - アプリケーションは、外部ストレージへの読み取り専用アクセスを必要とします。 現在は、すべてのアプリケーションに既定で読み取りアクセス権がありますが、Android の今後のリリースでは、アプリケーションは読み取りアクセスを明示的に要求する必要があります。
- `READ_USER_DICTIONARY` - ユーザーの単語辞書への読み取りアクセスを許可します。
- `READ_CALL_LOG` - 通話記録を読み取ることにより、アプリケーションが電話の着信と発信に関する情報を取得することを許可します。
- `WRITE_CALL_LOG` - アプリケーションが電話の通話記録に書き込むことを許可します。
- `WRITE_USER_DICTIONARY` - アプリケーションがユーザーの単語辞書に書き込むことを許可します。

注意すべき重要な変更点: `READ_EXTERNAL_STORAGE` - 現時点では、このアクセス許可は Android によって自動的に付与されます。 Android の将来のバージョンでは、アプリケーションによってこのアクセス許可を要求する必要があります。その後、このアクセス許可が付与されます。

## <a name="summary"></a>まとめ

この記事では、Android 4.1 (API レベル 16) で利用できる新しい API のいくつかを紹介しました。 アニメーションに関するいくつかの変更を取り上げ、アクティビティの開始をアニメーション化し、Bonjour や UPnP などのプロトコルを使用して他のデバイスのネットワーク探索用の新しい API を紹介しました。 インテントを使用してデータを切り取って貼り付ける機能や、分離されたサービスや "不安定な" コンテンツ プロバイダーを使用する機能など、API に対するその他の変更も取り上げました。

さらに、この記事では、通知に対する更新を紹介し、Android 4.1 で導入された新しいアクセス許可の一部についても説明しました。

## <a name="related-links"></a>関連リンク

- [Time Animation の例 (サンプル)](/samples/xamarin/monodroid-samples/platformfeatures-timeanimatorexample)
- [Android 4.1 API](https://developer.android.com/about/versions/android-4.1.html)
- [タスクとバック スタック](https://developer.android.com/guide/components/tasks-and-back-stack.html)
- [[戻る] と [上へ] を使用したナビゲーション](https://developer.android.com/design/patterns/navigation.html)