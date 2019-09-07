---
title: Jelly Bean の機能
description: このドキュメントでは、Android 4.1 で導入された開発者向けの新機能の概要について説明します。 これらの機能には、強化された通知、大きなファイルを共有する Android ビームの更新、マルチメディアの更新、ピアツーピアのネットワーク探索、アニメーション、新しいアクセス許可が含まれます。
ms.prod: xamarin
ms.assetid: 23F57634-2EF9-5C15-C710-B3E19A5AF7E1
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 7fa116f716c3c30e8d41dd19cbc09477a7709e49
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761538"
---
# <a name="jelly-bean-features"></a>Jelly Bean の機能

_このドキュメントでは、Android 4.1 で導入された開発者向けの新機能の概要について説明します。これらの機能には、強化された通知、大きなファイルを共有する Android ビームの更新、マルチメディアの更新、ピアツーピアのネットワーク探索、アニメーション、新しいアクセス許可が含まれます。_

## <a name="overview"></a>概要

Android 4.1 (API レベル 16) は、"Jelly Bean" とも呼ばれ、2012年7月9日にリリースされました。 この記事では、Xamarin を使用する開発者向けの Android 4.1 の新機能の概要について説明します。 これらの新機能のいくつかは、アクティビティを起動するためのアニメーション、カメラの新しいサウンド、およびアプリケーションスタックナビゲーションのサポートが向上しています。 インテントを使用して切り取りと貼り付けを行うことができるようになりました。

Android アプリケーションの安定性は、不安定なコンテンツプロバイダーとの依存関係を分離する機能によって改善されています。 サービスは、サービスを開始したアクティビティによってのみアクセスできるように分離することもできます。

Bonjour、UPnP、またはマルチキャスト DNS ベースのサービスを使用したネットワークサービス検出のサポートが追加されました。 テキスト、アクションボタン、および大きな画像を書式設定して、より豊富な通知を受け取ることができるようになりました。

最後に、Android 4.1 でいくつかの新しいアクセス許可が追加されました。

## <a name="requirements"></a>必要条件

Jelly Bean を使用して Xamarin Android アプリケーションを開発するには、次のスクリーンショットに示すように、Android SDK Manager を使用して Xamarin Android 4.2.6 以降と Android 4.1 (API レベル 16) をインストールする必要があります。

[![Android SDK Manager での Android 4.1 の選択](jelly-bean-images/image1.png)](jelly-bean-images/image1.png#lightbox)

## <a name="whats-new"></a>新機能

### <a name="animations"></a>アニメーション

アクティビティは、ズームアニメーションまたはカスタムアニメーションのいずれかを使用`ActivityOptions`して、クラスを使用して起動できます。 これらのアニメーションをサポートするために、次の新しいメソッドが用意されています。

- `MakeScaleUpAnimation`–画面の開始位置とサイズからアクティビティウィンドウをスケールアップするアニメーションを作成します。
- `MakeThumbnailScaleUpAnimation`–サムネイル画像から画面上の指定した位置にスケールアップするアニメーションを作成します。
- `MakeCustomAnimation`–アプリケーションのリソースからアニメーションを作成します。 アクティビティが開いたときと、アクティビティが停止したときの1つのアニメーションがあります。

新しい`TimeAnimator`クラスには、アニメーション`TimeAnimator.ITimeListener`でフレームが変更されるたびにアプリケーションに通知できるインターフェイスが用意されています。 たとえば、の次の`TimeAnimator.ITimeListener`実装について考えてみます。

```csharp
class MyTimeListener : Java.Lang.Object,  TimeAnimator.ITimeListener
{
    public void OnTimeUpdate(TimeAnimator animation, long totalTime, long deltaTime)
    {
        Log.Debug("Activity1", "totalTime={0}, deltaTime={1}", totalTime, deltaTime);
    }
}
```

クラスを使用するために、のインスタンスが`TimeAnimator`作成され、次のようにリスナーが設定されます。

```csharp
var animator = new TimeAnimator();
animator.SetTimeListener(new MyTimeListener());
animator.Start();
```

インスタンスが実行されている間、 `ITimeAnimator.ITimeListener`はを呼び出します。これにより、アニメーターが実行された時間と、メソッドが最後に呼び出されてからの経過時間がログに記録されます。 `TimeAnimator`

### <a name="application-stack-navigation"></a>アプリケーションスタックのナビゲーション

Android 4.1 では、Android 3.0 で導入されたアプリケーションスタックのナビゲーションが向上しています。 Android では`ParentName` 、 `ActivityAttribute`のプロパティを指定することによって、操作バーの [[上へ] ボタン](https://developer.android.com/design/patterns/navigation.html#up-vs-back)を押すと、適切な親アクティビティを開くことが`ParentName`できます。 android は、プロパティによって指定されたアクティビティをインスタンス化します。 これにより、アプリケーションは、特定のタスクを行うアクティビティの階層を維持できます。

ほとんどのアプリケーションでは`ParentName` 、アプリケーションスタック内を移動するための適切な動作を Android に提供するために、アクティビティでを設定するだけで十分です。Android では、親アクティビティごとに一連のインテントを作成することによって、必要なバックスタックを合成します。 ただし、これは人工のアプリケーションスタックであるため、各合成アクティビティには、自然なアクティビティに含まれる保存された状態がありません。 合成された状態を合成親アクティビティに提供するために、 `OnPrepareNavigationUpTaskStack`アクティビティはメソッドをオーバーライドする場合があります。 このメソッドは、 `TaskStackBuilder` Android がバックスタックを作成するために使用するインテントオブジェクトのコレクションを持つインスタンスを受け取ります。 アクティビティは、合成アクティビティが作成されると適切な状態情報を受け取るように、これらのインテントを変更することがあります。

より複雑なシナリオでは、アクティビティクラスに新しいメソッドがあり、これを使用して、上のナビゲーションの動作を処理し、バックスタックを構築できます。

- `OnNavigateUp`–このメソッドをオーバーライドすることによって、[**上**へ] ボタンが押されたときにカスタム動作を実行することができます。
- `NavigateUpTo`–このメソッドを呼び出すと、アプリケーションは、現在のアクティビティから、指定されたインテントによって指定されたアクティビティに移動します。
- `ParentActivityIntent`–現在のアクティビティの親アクティビティを起動するインテントを取得するために使用されます。
- `ShouldUpRecreateTask`–このメソッドは、親アクティビティに移動するために、合成バックスタックを作成する必要があるかどうかを照会するために使用されます。 合成`true`スタックを作成する必要がある場合はを返します。 
- `FinishAffinity`–このメソッドを呼び出すと、現在のアクティビティとその下にあるすべてのアクティビティが、同じタスクアフィニティを持つ現在のタスクで完了します。
- `OnCreateNavigateUpTaskStack`–このメソッドは、合成スタックの作成方法を完全に制御する必要がある場合にオーバーライドされます。

### <a name="camera"></a>カメラ

新しいインターフェイス`Camera.IAutoFocusMoveCallback`があります。これは、自動フォーカスが移動を開始または停止したことを検出するために使用できます。 この新しいインターフェイスの例を次のスニペットに示します。

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

新しいクラス`MediaActionSound`は、さまざまなメディア操作に対してサウンドを生成するための API のセットを提供します。 カメラでは、次のようないくつかの操作を実行できます。 `Android.Media.MediaActionSoundType`これらは列挙型によって定義されます。

- `MediaActionSoundType.FocusComplete`–このサウンドは、フォーカスが完了したときに再生されます。
- `MediaActionSoundType.ShutterClick`–このサウンドは、静止画像画像が取得されたときに再生されます。
- `MediaActionSoundType.StartVideoRecording`–このサウンドは、ビデオ記録の開始を示すために使用されます。
- `MediaActionSoundType.StopVideoRecording`–このサウンドは、ビデオ記録の終了を示すために再生されます。

クラスの`MediaActionSound`使用例を次のスニペットに示します。

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

Android ビームは、2つの Android デバイスが相互に通信できるようにする NFC ベースのテクノロジです。 Android 4.1 では、大きなファイルを転送するためのサポートが強化されています。 新しい方法`NfcAdapter.SetBeamPushUris()`を使用すると、Android は代替トランスポート機構 (Bluetooth など) を切り替えて高速転送速度を実現します。

#### <a name="network-services-discovery"></a>Network Services 検出

Android 4.1 には、マルチキャスト DNS ベースのサービス検出用の新しい API が含まれています。
これにより、アプリケーションは Wi-fi を検出し、プリンター、カメラ、メディアデバイスなどの他のデバイスに接続することができます。 これらの新しい API は、パッケージ`Android.Net.Nsd`に含まれています。

他のサービス`NsdServiceInfo`によって使用される可能性のあるサービスを作成するには、クラスを使用して、サービスのプロパティを定義するオブジェクトを作成します。 このオブジェクトは、の`NsdManager.RegisterService()` `NsdManager.ResolveListener`実装と共に、に提供されます。 の`NsdManager.ResolveListener`実装は、正常に登録されたことを通知し、サービスの登録を解除するために使用されます。

ネットワーク上のサービスと、に`Nsd.DiscoveryListener` `NsdManager.discoverServices()`渡されたの実装を探索する。

#### <a name="network-usage"></a>ネットワーク使用率

新しい方法では`ConnectivityManager.IsActiveNetworkMetered` 、デバイスが従量制課金ネットワークに接続されているかどうかを確認できます。 この方法を使用すると、データ操作に対して費用がかかる可能性があることをユーザーに正確に通知することで、データの使用状況を管理できます。

#### <a name="wifi-direct-service-discovery"></a>WiFi ダイレクトサービス検出

クラス`WifiP2pManager`は、*ゼロ conf*をサポートするために Android 4.0 で導入されました。 Zero conf (ゼロ構成ネットワーク) は、人間のネットワークオペレーターや特別な構成サーバーの介入によって、デバイス (コンピューター、プリンター、電話) がネットワークに自動的に接続できるようにする一連の手法です。

Jelly Bean で、 `WifiP2pManager`は*Bonjour*または*Upnp*を使用して近くのデバイスを検出できます。 Bonjour は、Apple によるゼロ conf の実装です。 Upnp は、ゼロ conf もサポートするネットワークプロトコルのセットです。 Wi-fi サービス検出をサポートする`WiFiP2pManager`ために、に追加された次のメソッド。

- `AddLocalService()`–このメソッドは、ピアによる検出のために、Wi-fi 経由でアプリケーションをサービスとしてアナウンスするために使用されます。
- `AddServiceRequest(`) –このメソッドは、サービス検出要求をフレームワークに送信します。 Wi-fi サービス検出を初期化するために使用されます。
- `SetDnsSdResponseListeners()`–このメソッドは、Bonjour からの探索要求への応答を受信するときに呼び出されるコールバックを登録するために使用されます。
- `SetUpnpServiceResponseListener()`–このメソッドは、Upnp に対する検出要求への応答を受信するときに呼び出されるコールバックを登録するために使用されます。

### <a name="content-providers"></a>コンテンツ プロバイダー

クラス`ContentResolver`は、新しいメソッドを`AcquireUnstableContentProvider`受け取りました。 このメソッドを使用すると、アプリケーションは "不安定な" コンテンツプロバイダーを取得できます。 通常、アプリケーションがコンテンツプロバイダーを取得し、そのコンテンツプロバイダーがクラッシュした場合、アプリケーションが発生します。 このメソッドを呼び出すと、コンテンツプロバイダーがクラッシュした場合にアプリケーションがクラッシュすることはありません。 代わりに、 `Android.OS.DeadObjectionException`コンテンツプロバイダーの呼び出しから、コンテンツプロバイダーが失われたことをアプリケーションに通知するために、がスローされます。 "不安定な" コンテンツプロバイダーは、他のアプリケーションからコンテンツプロバイダーと対話する場合に役立ちます。他のアプリケーションのバグコードが別のアプリケーションに影響を与える可能性は低くなります。

### <a name="copy-and-paste-with-intents"></a>インテントをコピーして貼り付ける

クラスには、 `ClipData` `Intent.ClipData`プロパティを使用してオブジェクトを関連付けることができるようになりました。 `Intent` このメソッドを使用すると、クリップボードからの追加データをインテントと共に送信できます。 のインスタンスに`ClipData` `ClipData.Item`は、1つ以上のを含めることができます。 `ClipData.Item`は、次の種類の項目です。

- **Text** –これは、HTML または組み込みの Android スタイル範囲でサポートされている形式を持つ任意の文字列です。
- **インテント**–任意`Intent`のオブジェクト。
- **Uri** – HTTP ブックマークやコンテンツプロバイダーの uri など、任意の uri を指定できます。

### <a name="isolated-services"></a>分離サービス

分離サービスは、独自の特別なプロセスで実行され、独自のアクセス許可を持たないサービスです。 サービスとの通信は、サービスを開始し、サービス API を介してサービスにバインドする場合のみです。 ものサービスクラスのプロパティ`IsolatedProcess="true"` `ServiceAttribute`を設定することによって、サービスを分離されたものとして宣言できます。

### <a name="media"></a>メディア

新しい`Android.Media.MediaCodec`クラスは、低レベルのメディアコーデックに API を提供します。 アプリケーションはシステムを照会して、デバイスで使用可能な低レベルのコーデックを調べることができます。

キャプチャし`Android.Media.Audiofx.AudioEffect`たオーディオでの追加のオーディオの事前処理をサポートするために、新しいサブクラスが追加されました。

- `Android.Media.Audiofx.AcousticEchoCanceler`–このクラスは、キャプチャされたオーディオ信号からリモートパーティからシグナルを削除するために、オーディオを事前処理するために使用されます。 たとえば、音声通信アプリケーションからエコーを削除します。
- `Android.Media.Audiofx.AutomaticGainControl`–このクラスは、出力シグナルが一定になるように入力信号をブーストまたは減少させることによって、キャプチャされたシグナルを正規化するために使用されます。
- `Android.Media.Audiofx.NoiseSuppressor`–このクラスは、キャプチャされたシグナルからバックグラウンドノイズを除去します。

すべてのデバイスでこれらの影響がサポートされるわけではありません。 アプリケーションから`AudioEffect.IsAvailable`メソッドを呼び出して、問題のオーディオ効果がアプリケーションを実行しているデバイスでサポートされているかどうかを確認する必要があります。

クラス`MediaPlayer`は、 `SetNextMediaPlayer()`メソッドを使用したすきまの再生をサポートするようになりました。 この新しいメソッドは、現在のメディアプレーヤーの再生が終了したときに、次の MediaPlayer を開始するように指定します。

次の新しいクラスは、メディアを再生する場所を選択するための標準的なメカニズムと UI を提供します。

- `MediaRouter`–このクラスを使用すると、アプリケーションは、デバイスから外部スピーカーまたは他のデバイスへのメディアチャネルのルーティングを制御できます。
- `MediaRouterActionProvider`および`MediaRouteButton` –これらのクラスは、メディアを選択して再生するための一貫した UI を提供するのに役立ちます。

### <a name="notifications"></a>通知

Android 4.1 を使用すると、アプリケーションの柔軟性が向上し、通知を表示することができます。 アプリケーションでユーザーに対してより大きな通知を表示できるようになりました。 新しいメソッドでは`NotificationBuilder.SetStyle()` 、通知に新しい3つの新しいスタイルのいずれかを設定できます。

- `Notification.BigPictureStyle`–これは、イメージを含む通知を生成するヘルパークラスです。 次の図は、大きな画像を含む通知の例を示しています。

 [![Big画像スタイルの通知のスクリーンショットの例](jelly-bean-images/image2.png)](jelly-bean-images/image2.png#lightbox)

- `Notification.BigTextStyle`–これは、電子メールなどの複数行のテキストを含む通知を生成するヘルパークラスです。 この新しい通知スタイルの例を次のスクリーンショットに示します。

 [![BigTextStyle 通知のスクリーンショットの例](jelly-bean-images/image3.png)](jelly-bean-images/image3.png#lightbox)

- `Notification.InboxStyle`–これは、次のスクリーンショットに示すように、電子メールメッセージのスニペットなどの文字列のリストを含む通知を生成するヘルパークラスです。

 [![通知のスクリーンショットの例-受信トレイのスタイル通知](jelly-bean-images/image4.png)](jelly-bean-images/image4.png#lightbox)

通知で通常またはそれ以上のスタイルを使用している場合は、通知メッセージの下部に最大2つのアクションボタンを追加できます。
この例は、次のスクリーンショットで確認できます。このスクリーンショットでは、通知の下部にアクションボタンが表示されています。

 [![通知メッセージの下に表示されるアクションボタンのスクリーンショットの例](jelly-bean-images/image5.png)](jelly-bean-images/image5.png#lightbox)

クラス`Notification`は、開発者が通知に5つの優先度レベルのいずれかを指定できるようにする新しい定数を受け取りました。 これらは、 `Priority`プロパティを使用して通知に設定できます。

### <a name="permissions"></a>アクセス許可

次の新しいアクセス許可が追加されました。

- `READ_EXTERNAL_STORAGE`-アプリケーションは、外部ストレージへの読み取り専用アクセスを必要とします。 現在、すべてのアプリケーションには既定で読み取りアクセス権がありますが、Android の今後のリリースでは、アプリケーションが読み取りアクセスを明示的に要求する必要があります。
- `READ_USER_DICTIONARY`-ユーザーの word 辞書への読み取りアクセスを許可します。
- `READ_CALL_LOG`-アプリケーションが呼び出しログを読み取ることによって、受信および送信の呼び出しに関する情報を取得できるようにします。
- `WRITE_CALL_LOG`-アプリケーションが電話の通話ログに書き込むことを許可します。
- `WRITE_USER_DICTIONARY`-アプリケーションがユーザーの word 辞書に書き込むことを許可します。

注意`READ_EXTERNAL_STORAGE`すべき重要な変更: 現在、このアクセス許可は Android によって自動的に付与されます。 Android の将来のバージョンでは、アクセス許可を付与する前に、アプリケーションからこのアクセス許可を要求する必要があります。

## <a name="summary"></a>Summary

この記事では、Android 4.1 (API レベル 16) で利用できる新しい API のいくつかを紹介しました。 アニメーションのいくつかの変更を強調表示し、アクティビティの起動をアニメーション化し、Bonjour や UPnP などのプロトコルを使用して他のデバイスのネットワーク探索用の新しい API を導入しました。 その他の API への変更も強調表示されています。たとえば、インテントを使用してデータを切り取って貼り付ける機能や、分離サービスや "不安定な" コンテンツプロバイダーを使用する機能などです。

この記事では、通知の更新プログラムを紹介し、Android 4.1 で導入された新しいアクセス許可の一部について説明しました。

## <a name="related-links"></a>関連リンク

- [時間アニメーションの例 (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-timeanimatorexample)
- [Android 4.1 Api](https://developer.android.com/about/versions/android-4.1.html)
- [タスクとバックスタック](https://developer.android.com/guide/components/tasks-and-back-stack.html)
- [ナビゲーションと戻る](https://developer.android.com/design/patterns/navigation.html)
