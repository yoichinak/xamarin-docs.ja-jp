---
title: リモート Firebase 通知クラウド メッセージング
description: このチュートリアルでは、アプリケーションで Xamarin.Android Firebase Cloud Messaging を使用して、リモートの通知 (プッシュ通知とも呼ばれる) を実装する方法の詳細な手順についてを提供します。 Firebase クラウド メッセージング (FCM) との通信に必要な FCM へのアクセス用の Android のマニフェストを構成する方法の例を示しますおよびダウン ストリームを使用してメッセージング、Firebase について説明するさまざまなクラスを実装する方法を示していますコンソールです。
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/12/2018
ms.openlocfilehash: e2f25504b971a0332dc51dc9b017c9c83222ec57
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2018
ms.locfileid: "31044939"
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>リモート Firebase 通知クラウド メッセージング

_このチュートリアルでは、アプリケーションで Xamarin.Android Firebase Cloud Messaging を使用して、リモートの通知 (プッシュ通知とも呼ばれる) を実装する方法の詳細な手順についてを提供します。Firebase クラウド メッセージング (FCM) との通信に必要な FCM へのアクセス用の Android のマニフェストを構成する方法の例を示しますおよびダウン ストリームを使用してメッセージング、Firebase について説明するさまざまなクラスを実装する方法を示していますコンソールです。_

## <a name="fcm-notifications-overview"></a>FCM 通知の概要

このチュートリアルで基本的なアプリと呼ばれる**FCMClient** FCM メッセージングの基礎を説明するために作成されます。 **FCMClient** Google Play サービスの存在を確認、FCM から登録トークンを受信、Firebase コンソールから送信するリモートの通知を表示およびトピックのメッセージをサブスクライブします。

[![アプリのサンプルのスクリーン ショット](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png#lightbox)

次のトピックを取り上げたはとおりです。

1.  バック グラウンド通知

2.  トピックのメッセージ

3.  フォア グラウンド通知

このチュートリアルでは増分機能を追加する**FCMClient**し、デバイスまたは FCM との対話方法を理解するためのエミュレーターで実行します。 ライブ アプリ トランザクション FCM サーバーとミラーリング監視サーバーにログ記録を使用して、Firebase コンソール通知 GUI に入力された FCM メッセージからの通知を生成する方法が観察します。 

## <a name="requirements"></a>要件

このチュートリアルを続行するには、Google の FCM サーバーを使用するために必要な資格情報を取得する必要があります。このプロセスは」で説明されて[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)です。 具体的には、ダウンロードする必要があります、 **google services.json**このチュートリアルで示すサンプル コードで使用するファイル。 かどうかはまだ作成していないプロジェクト Firebase コンソールで (まだダウンロードしていない場合や、 **google services.json**ファイル) を参照してください[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)です。 

例のアプリを実行するには、テストの Android デバイスまたは Firebase と互換性のあるエミュレーターを必要があります。 Firebase Cloud Messaging Android 2.3 (ジンジャーブレッド) 以降を実行しているクライアントをサポートしているし、これらのデバイスが必要もあります、インストールされている Google Play ストア アプリ (Google Play サービスの 9.2.1 または以降が必要)。 デバイスにインストールされている、Google Play ストア アプリがない場合は、次を参照してください。、 [Google Play](https://support.google.com/googleplay) web サイトをダウンロードしてインストールします。 また、2.3 以降 (Android SDK エミュレーターを使用している場合は、Google Play ストアをインストールする必要はありません) のテスト デバイスではなく、Android を実行している Android SDK エミュレーターを使用することができます。 

## <a name="start-an-app-project"></a>アプリ プロジェクトを開始します。

呼ばれる新しい空の Xamarin.Android プロジェクトの作成を開始する、 **FCMClient**です。 Xamarin.Android プロジェクトの作成に慣れていない場合は、次を参照してください。 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md)です。 新しいアプリを作成した後、次の手順は、パッケージ名を設定し、FCM との通信に使用されるいくつかの NuGet パッケージをインストールするのには。 

### <a name="set-the-package-name"></a>パッケージ名を設定します。

[Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)、FCM 対応アプリのパッケージ名を指定します。 このパッケージの名前としても、*アプリケーション ID* API キーに関連付けられています。 このパッケージ名を使用するアプリを構成します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  プロパティを開き、 **FCMClient**プロジェクト。 

2.  **Android マニフェスト** ページで、パッケージ名を設定します。 

次の例では、パッケージ名が に設定されている`com.xamarin.fcmexample`: 

[![パッケージ名を設定します。](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png#lightbox)

更新するときに、 **Android マニフェスト**、ことを確認のチェックも、`Internet`アクセス許可を有効にします。 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  プロパティを開き、 **FCMClient**プロジェクト。 

2.  **Android アプリケーション** ページで、パッケージ名を設定します。 

次の例では、パッケージ名が に設定されている`com.xamarin.fcmexample`: 

[![パッケージ名を設定します。](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png#lightbox)

更新するときに、 **Android マニフェスト**、ことを確認のチェックも、`INTERNET`アクセス許可が有効になっている (**アクセス許可を必要な**)。 

-----

クライアント アプリがこのパッケージ名がない場合、FCM から登録トークンを受信することはできませんに注意してください*まったく*Firebase コンソールに入力されているパッケージ名と一致します。 

### <a name="add-the-xamarin-google-play-services-base-package"></a>Xamarin の Google Play サービスの基本パッケージを追加します。

Firebase Cloud Messaging Google Play サービス、依存するため、 [Xamarin Google Play サービスのベース](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/)Xamarin.Android プロジェクトに NuGet パッケージを追加する必要があります。 バージョン 29.0.0.2 必要がありますまたはそれ以降。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Visual Studio を右クリックして**参照 > NuGet パッケージの管理.**. 

2.  クリックして、**参照**タブし、検索**Xamarin.GooglePlayServices.Base**です。 

3.  このパッケージをインストール、 **FCMClient**プロジェクト。 

    [![Google Play サービス ベースのインストール](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Mac 用 Visual Studio で、右クリック**パッケージ > パッケージを追加しています.**. 

2.  検索**Xamarin.GooglePlayServices.Base**です。 

3.  このパッケージをインストール、 **FCMClient**プロジェクト。 

    [![Google Play サービス ベースのインストール](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png#lightbox)

-----

NuGet のインストール中にエラーが発生した場合は閉じます、 **FCMClient**プロジェクトで、もう一度開き、NuGet のインストールを再試行します。 

インストールするときに**Xamarin.GooglePlayServices.Base**、すべての必要な依存関係もインストールされます。 編集**MainActivity.cs**し、以下の追加`using`ステートメント。 

```csharp
using Android.Gms.Common;
```

このステートメントを使用する、`GoogleApiAvailability`クラス**Xamarin.GooglePlayServices.Base**に利用可能な**FCMClient**コード。 
`GoogleApiAvailability` Google Play サービスの存在をチェックに使用されます。 

### <a name="add-the-xamarin-firebase-messaging-package"></a>パッケージをメッセージング Xamarin Firebase を追加します。

FCM からメッセージを受信する、 [Xamarin Firebase - メッセージング](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/)NuGet パッケージは、アプリのプロジェクトに追加する必要があります。 このパッケージは、Android アプリケーションは FCM サーバーからのメッセージを受信できません。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Visual Studio を右クリックして**参照 > NuGet パッケージの管理.**. 

2.  確認**プレリリースを含める**を検索および**Xamarin.Firebase.Messaging**です。 

3.  このパッケージをインストール、 **FCMClient**プロジェクト。 

    [![Xamarin Firebase メッセージングをインストールします。](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  Mac 用 Visual Studio で、右クリック**パッケージ > パッケージを追加しています.**. 

2.  確認**プレリリースのパッケージを表示**を検索および**Xamarin.Firebase.Messaging**です。 

3.  このパッケージをインストール、 **FCMClient**プロジェクト。 

    [![Xamarin Firebase メッセージングをインストールします。](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png#lightbox)

-----
 
インストールするときに**Xamarin.Firebase.Messaging**、すべての必要な依存関係もインストールされます。

次に、編集**MainActivity.cs**し、以下の追加`using`ステートメント。

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

最初の 2 つのステートメント内の型の作成、 **Xamarin.Firebase.Messaging** NuGet パッケージを使用可能な**FCMClient**コード。 **Android.Util** FMS でのトランザクションを確認するために使用するログ記録機能を追加します。 


### <a name="add-the-google-services-json-file"></a>Google サービスの JSON ファイルを追加します。

次の手順が追加するには、 **google services.json**プロジェクトのルート ディレクトリにファイル。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  コピー **google services.json**プロジェクト フォルダーにします。

2.  追加**google services.json**アプリケーション プロジェクトに (をクリックして**すべてのファイル**で、**ソリューション エクスプ ローラー**を右クリックして**google services.json**選択してから、**プロジェクトに含める**)。 

3.  選択**google services.json**で、**ソリューション エクスプ ローラー**ウィンドウです。

4.  **プロパティ**ペインで、設定、**ビルド アクション**に**GoogleServicesJson** (場合、 **GoogleServicesJson**ビルド アクションは表示されませんが、保存し、ソリューションを閉じて再び開く)。

    [![ビルド アクションを GoogleServicesJson に設定します。](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  コピー **google services.json**プロジェクト フォルダーにします。

2.  追加**google services.json**アプリケーション プロジェクトにします。 

3.  右クリック**google services.json**です。

4.  設定、**ビルド アクション**に**GoogleServicesJson**: 

    [![ビルド アクションを GoogleServicesJson に設定します。](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png#lightbox)
 
-----
 

ときに**google services.json**がプロジェクトに追加 (および**GoogleServicesJson**ビルド アクションが設定されている)、ビルド プロセスがクライアント ID と API キーを抽出し、マージにこれらの資格情報を追加/生成された**AndroidManifest.xml**に存在する**obj/Debug/android/AndroidManifest.xml**です。 このマージ プロセスは、すべてのアクセス許可と FCM サーバーへの接続に必要なその他の FCM 要素に自動的に追加します。 


## <a name="check-for-google-play-services"></a>Google Play サービスのチェック

アプリの UI の初期レイアウトを最初に作成されます。 編集**Resources/layout/Main.axml**しその内容を次の XML に置き換えます。 

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp">
    <TextView
        android:text=" "
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/msgText"
        android:textAppearance="?android:attr/textAppearanceMedium"
        android:padding="10dp" />
</LinearLayout>
```

これは、 `TextView` Google Play サービスがインストールされているかどうかを示すメッセージを表示するために使用されます。 変更を保存**Main.axml**です。 編集**MainActivity.cs**に次のインスタンスの変数宣言を追加し、`MainActivity`クラス。 

```csharp
TextView msgText;
```

Google は、Google Play サービスの機能にアクセスする前に Android アプリを確認 Google プレイ サービス APK が存在することをお勧め (詳細については、次を参照してください。 [、Google Play サービスを確認](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play))。 次の例で、`OnCreate`メソッドは、アプリが、FCM サービスを使用しようとしています。 前に Google Play サービスが利用可能なであることを確認します。 次のメソッドを追加、`MainActivity`クラス。 

```csharp
public bool IsPlayServicesAvailable ()
{
    int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable (this);
    if (resultCode != ConnectionResult.Success)
    {
        if (GoogleApiAvailability.Instance.IsUserResolvableError (resultCode))
            msgText.Text = GoogleApiAvailability.Instance.GetErrorString (resultCode);
        else
        {
            msgText.Text = "This device is not supported";
            Finish ();
        }
        return false;
    }
    else
    {
        msgText.Text = "Google Play Services is available.";
        return true;
    }
}
```

このコードは、Google プレイ サービス APK がインストールされているかどうかにデバイスを確認します。 メッセージが表示されますがインストールされていない場合、 `TextBox` APK、Google Play ストアからダウンロードする (または、デバイスのシステム設定で有効にする) のユーザーに指示します。 

`OnCreate` メソッドを次のコードで置き換えます。 

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);
    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);
    IsPlayServicesAvailable ();
}
```

`IsPlayServicesAvailable` 最後に呼び出されます`OnCreate`Google Play サービスは、それぞれの実行時間、アプリが起動を確認できるようにします。 アプリがある場合、`OnResume`メソッドを呼び出して`IsPlayServicesAvailable`から`OnResume`もします。 完全に再構築し、アプリを実行します。 すべてが正しく構成されて、次のスクリーン ショットのような画面が表示されます。 

[![アプリは、Google Play サービスが利用可能なであることを示します](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png#lightbox)

この結果が得られない場合は、Google プレイ サービス APK がデバイスにインストールされていることを確認してください (詳細については、次を参照してください。[設定を Google Play サービス](https://developers.google.com/android/guides/setup))。 追加されていることを確認してください、 **Xamarin.Google.Play.Services.Base**をパッケージ化、 **FCMClient**プロジェクトの前に説明したようです。


## <a name="add-the-instance-id-receiver"></a>インスタンス ID のレシーバーを追加します。

次の手順が拡張するサービスを追加するには`FirebaseInstanceIdService`ハンドルの作成、回転、および Firebase 登録トークンの更新します。 (登録トークンの詳細についてを参照してください[FCM で登録](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)。)。`FirebaseInstanceIdService`サービスがデバイスにメッセージを送信できる FCM 必要です。 ときに、`FirebaseInstanceIdService`サービスは、アプリに追加して、クライアント アプリは自動的に FCM メッセージを受信し、アプリが backgrounded されるたびに通知として表示します。 

### <a name="declare-the-receiver-in-the-android-manifest"></a>Android マニフェストに、受信側を宣言します。

編集**AndroidManifest.xml**し、次の挿入`<receiver>`要素を`<application>`セクション。 

```xml
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver"
    android:exported="false" />
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver"
    android:exported="true"
    android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
    </intent-filter>
</receiver>
```

この XML は次のとおり

-   宣言、`FirebaseInstanceIdReceiver`実装を提供する、[一意識別子](https://developers.google.com/instance-id/)アプリ インスタンスごとにします。 この受信者は認証し、操作を認証します。 

-   内部宣言`FirebaseInstanceIdInternalReceiver`を安全にサービスを開始するために使用する実装。

`FirebaseInstanceIdReceiver`は、`WakefulBroadcastReceiver`を受け取る`FirebaseInstanceId`と`FirebaseMessaging`イベントから派生したクラスに配信および`FirebaseInstanceIdService`です。 

### <a name="implement-the-firebase-instance-id-service"></a>Firebase インスタンスの ID サービスを実装します。

FCM でアプリケーションの登録作業は、カスタム処理`FirebaseInstanceIdService`提供するサービスです。 
`FirebaseInstanceIdService` 次の手順を実行します。 

1.  使用して、[インスタンス ID API](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID) FCM およびアプリケーション サーバーにアクセスするクライアント アプリを承認するセキュリティ トークンを生成します。 代わりに、アプリから返される登録トークン FCM です。 

2.  アプリケーション サーバーで必要な場合は、アプリ サーバーの登録トークンを転送します。

という名前の新しいファイルを追加**MyFirebaseIIDService.cs**し、テンプレート コードを次に置き換えます。 

```csharp
using System;
using Android.App;
using Firebase.Iid;
using Android.Util;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
    public class MyFirebaseIIDService : FirebaseInstanceIdService
    {
        const string TAG = "MyFirebaseIIDService";
        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "Refreshed token: " + refreshedToken);
            SendRegistrationToServer(refreshedToken);
        }
        void SendRegistrationToServer(string token)
        {
            // Add custom implementation, as needed.
        }
    }
}
```

このサービスを実装する`OnTokenRefresh`登録トークンが最初に作成または変更されたときに呼び出されるメソッド。 ときに`OnTokenRefresh`実行されるから最新のトークンを取得、`FirebaseInstanceId.Instance.Token`プロパティ (FCM によっては非同期的に更新されます)。 この例では、出力ウィンドウに表示できるように、更新トークンが記録されます。 

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh` 呼び出される頻度の低い: 次の状況でトークンの更新に使用されます。 

-   ときに、アプリをインストールまたはアンインストールします。

-   ときに、ユーザーは、アプリ データを削除します。 

-   アプリに消去されるインスタンスの id。

-   トークンのセキュリティが侵害された場合。

Google のに従って[インスタンス ID](https://developers.google.com/instance-id/guides/android-implementation)ドキュメントについては、FCM インスタンスの ID サービスが要求に、アプリがそのトークン定期的に (通常は、6 か月ごと) を更新します。 

`OnTokenRefresh` 呼び出しも`SendRegistrationToAppServer`にユーザーの登録を関連付けるにはサーバー側のアカウント (存在する場合) を使用してトークンに保持される、アプリケーションで。 

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

この実装は、アプリ サーバーの設計に依存しているために、この例で空のメソッド本体が提供されます。 アプリケーション サーバーは、FCM 登録情報を必要とする場合は変更`SendRegistrationToAppServer`にユーザーの FCM インスタンス ID トークンをアプリで管理されている任意のサーバー側アカウントに関連付けます。 (詳しくは、トークンがクライアント アプリケーションに対して非透過的であるを注意してください)。 

トークンがアプリ サーバーに送信される`SendRegistrationToAppServer`をトークンがサーバーに送信されたかどうかを示すブール値を維持する必要があります。 このブール値が false の場合`SendRegistrationToAppServer`トークンをアプリ サーバーに送信&ndash;それ以外の場合、トークンは、前の呼び出しで、アプリ サーバーに送信されました既にです。 場合によっては (このなど`FCMClient`例)、アプリ サーバーにトークンが必要ありませんです。 したがって、このメソッドはこの例で必要はありません。 

## <a name="implement-client-app-code"></a>クライアント アプリ コードの実装

受信側サービスがあると、これで、これらのサービスを活用するためにクライアント アプリ コードを記述できます。 次のセクションで、ボタンが登録トークンをログオン UI に追加 (とも呼ばれる、*インスタンス ID トークン*) にさらにコードを追加および`MainActivity`を表示する`Intent`からアプリを起動したときに情報、通知: 

[![アプリの画面に追加ログ トークン ボタン](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

### <a name="log-tokens"></a>ログのトークン

この手順で追加したコードは、デモンストレーション用のみ&ndash;実稼働クライアント アプリには登録トークンをログオンする必要はありません。 編集**Resources/layout/Main.axml**し、以下の追加`Button`宣言の直後に、`TextView`要素。 

```xml
<Button
  android:id="@+id/logTokenButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:text="Log Token" />
```

編集**MainActivity.cs**に次のメンバーの宣言を追加し、`MainActivity`クラス。

```csharp
const string TAG = "MainActivity";
```

末尾に次のコードを追加、`OnCreate`メソッド。 

```csharp
var logTokenButton = FindViewById<Button>(Resource.Id.logTokenButton);
logTokenButton.Click += delegate {
    Log.Debug(TAG, "InstanceID token: " + FirebaseInstanceId.Instance.Token);
};
```

このコードは、出力ウィンドウに現在のトークンをログに記録時に、**ログ トークン** ボタンをタップします。 

### <a name="handle-notification-intents"></a>通知の目的を処理します。

ユーザーがから発行された通知をタップした**FCMClient**、その通知に付属するすべてのデータ メッセージを使用可能で`Intent`extras です。 編集**MainActivity.cs**の先頭に次のコードを追加し、`OnCreate`メソッド (呼び出しの前に`IsPlayServicesAvailable`)。 

```csharp
if (Intent.Extras != null)
{
    foreach (var key in Intent.Extras.KeySet())
    {
        var value = Intent.Extras.GetString(key);
        Log.Debug(TAG, "Key: {0} Value: {1}", key, value);
    }
}
```

アプリの起動ツール`Intent`ため、このコードに付属するデータをログイン、ユーザーがその通知メッセージをタップしたときに発生、`Intent`出力ウィンドウをします。 場合は、さまざまな`Intent`発生する必要があります、`click_action`に通知メッセージのフィールドを設定する必要があります`Intent`(ランチャー`Intent`がない場合に使用`click_action`が指定されている)。 


## <a name="background-notifications"></a>バック グラウンド通知

ビルドおよび実行する、 **FCMClient**アプリ。 **ログ トークン**ボタンが表示されます。

[![トークン [ログ] ボタンが表示されます。](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

タップして、**ログ トークン**ボタンをクリックします。 次のようなメッセージは、IDE は、出力ウィンドウに表示する必要があります。 

[![[出力] ウィンドウに表示されるインスタンス ID トークン](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png#lightbox)

長い文字列が付いた**トークン**Firebase コンソールに貼り付け、インスタンス ID のトークン&ndash;を選択し、この文字列をクリップボードにコピーします。 インスタンス ID トークンが表示されない場合は、先頭に次の行を追加、`OnCreate`ことを確認するメソッド**google services.json**正しく解析されました。

```csharp
Log.Debug(TAG, "google app id: " + GetString(Resource.String.google_app_id));
```

`google_app_id`値は、出力ウィンドウにログ記録に一致する必要があります、`mobilesdk_app_id`で記録された値**google services.json**です。 

### <a name="send-a-message"></a>メッセージを送信します。

サインイン、 [Firebase コンソール](https://console.firebase.google.com)、プロジェクトを選択してをクリックし、**通知**、 をクリック**最初のメッセージを送信**: 

[![送信の最初のメッセージ ボタン](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png#lightbox)

**作成メッセージ** ページで、メッセージのテキストを入力し、選択**1 つのデバイス**です。 IDE は、出力ウィンドウからインスタンス ID トークンをコピーして貼り付けます、 **FCM 登録トークン**Firebase コンソール フィールド。 

[![メッセージ ダイアログ ボックスを作成します。](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png#lightbox)

Android デバイス (またはエミュレーター)、Android をタップして、アプリのバック グラウンド**概要**ボタンをクリックし、ホーム画面を変更することです。 デバイスが準備完了 をクリックして**メッセージの送信**Firebase コンソール。 

[![送信メッセージ ボタン](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png#lightbox)

ときに、**レビュー メッセージ**ダイアログが表示されたら、をクリックして**送信**です。
通知アイコンは、デバイス (またはエミュレーター) の通知領域に表示する必要があります。 

[![通知アイコンが表示されます。](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png#lightbox)

メッセージを表示する通知アイコンを開きます。 通知メッセージにどのような入力が正確にする必要があります、**メッセージ テキスト**Firebase コンソール フィールド。 

[![デバイスに通知メッセージが表示されます。](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png#lightbox)

戻るには、通知アイコンをタップして、 **FCMClient**アプリ。 `Intent`に送信される extras **FCMClient** IDE の出力ウィンドウに表示されます。 

[![キー、メッセージ ID、および折りたたみキーからインテント extras が一覧表示します。](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png#lightbox)

この例では、**から**Firebase プロジェクトの数、アプリにキーが設定されている (この例では`41590732`)、および**collapse_key**パッケージの名前に設定されている (**com.xamarin.fcmexample**)。 メッセージを受け取らない場合は、削除してください。、 **FCMClient**デバイス (またはエミュレーター) 上のアプリ、上記の手順を繰り返します。 


> [!NOTE]
> キューブを強制終了アプリとして設定すると、FCM は停止通知を配信します。 Android では、バック グラウンド サービス ブロードキャストが不必要にまたは誤ってが停止状態のアプリケーションのコンポーネントを起動することを防止します。 (この動作の詳細については、次を参照してください[停止状態のアプリケーション上のコントロールを起動して](https://developer.android.com/about/versions/android-3.1.html#launchcontrols)。)。このため、必要があるたびにアプリを手動でアンインストールを実行し、デバッグ セッションから停止&ndash;これにより、受信するメッセージが継続されるように、新しいトークンを生成する FCM です。

### <a name="add-a-custom-default-notification-icon"></a>カスタムの既定の通知アイコンを追加します。

前の例では、通知アイコンはアプリケーションのアイコンに設定されます。 次の XML では、カスタムの既定のアイコンを通知を構成します。 Android では、通知アイコンは、明示的に設定されていないすべての通知メッセージの場合は、この既定のカスタム アイコンが表示されます。 

カスタムの既定の通知アイコンを追加するには、アイコンを追加、**リソース/描画**ディレクトリ、編集**AndroidManifest.xml**、次の挿入と`<meta-data>`に要素を`<application>`セクション。 

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

この例では、通知アイコンに配置されている**ic/リソース/ドロウアブル\_stat\_ic\_notification.png**カスタムの既定の通知アイコンとして使用されます。 カスタムの既定のアイコンがで構成されていない場合**AndroidManifest.xml**通知ペイロードにアイコンが設定されていないと、Android を使用してアプリケーションのアイコン、通知アイコン (上記の通知アイコン スクリーン ショットに表示)。 

## <a name="handle-topic-messages"></a>トピックのメッセージを処理します。

これまでに記述されたコードでは、登録トークンを処理し、リモートの通知機能をアプリに追加します。 次の例では、リッスンしているコード*トピック メッセージ*し、ユーザーに、リモートの通知を転送します。 トピックのメッセージは、特定のトピックを定期受信する 1 つまたは複数のデバイスに送信される FCM メッセージです。 トピックのメッセージの詳細については、次を参照してください。[トピック メッセージング](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)です。 

### <a name="subscribe-to-a-topic"></a>トピックを購読するには

編集**Resources/layout/Main.axml**し、以下の追加`Button`直後に、前の宣言`Button`要素。

```xml
<Button
  android:id="@+id/subscribeButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:layout_marginTop="20dp"
  android:text="Subscribe to Notifications" />
```

この XML を追加、**通知を受け取る**レイアウト ボタンをクリックします。
編集**MainActivity.cs**の末尾に次のコードを追加し、`OnCreate`メソッド。 

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

このコードを検索、**通知を受け取る**レイアウトでボタンをクリックし、そのクリック ハンドラーを呼び出すコードに代入`FirebaseMessaging.Instance.SubscribeToTopic`、サブスクライブしているトピックを渡して、_ニュース_です。 ユーザーがタップしたときに、**購読** ボタンをサブスクライブしているアプリ、_ニュース_トピックです。 次のセクションで、_ニュース_Firebase コンソール通知 GUI から、トピックのメッセージが送信されます。 

### <a name="send-a-topic-message"></a>トピックのメッセージを送信します。

アプリをアンインストール、再度実行して、再構築します。 クリックして、**通知にサブスクライブする**ボタンをクリックします。

[![ボタンの通知をサブスクライブします。](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png#lightbox)

アプリが正常にサブスクライブしているはず**トピックの同期が成功した**IDE では、出力ウィンドウ。 

[![[出力] ウィンドウは、トピックの同期が成功したメッセージを示しています。](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png#lightbox)

トピックのメッセージを送信するのにには、次の手順を使用します。

1.  Firebase コンソールで **新しいメッセージ**です。 

2.  **作成メッセージ** ページで、メッセージのテキストを入力し、選択**トピック**です。 

3.  **トピック**プルダウン メニューでは、組み込みのトピックを選択して**ニュース**: 

    [![ニュースのトピックを選択します。](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png#lightbox)

4.  Android デバイス (またはエミュレーター)、Android をタップして、アプリのバック グラウンド**概要**ボタンをクリックし、ホーム画面を変更することです。 

5.  デバイスが準備完了 をクリックして**メッセージの送信**Firebase コンソールでします。 

6.  チェック、IDE の出力を表示するウィンドウ **/トピック/ニュース**ログの出力。 

    [![/Topic/news からメッセージが表示されます。](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png#lightbox)

このメッセージは、出力ウィンドウに表示されますが、ときに通知アイコンも Android デバイスで通知領域に表示されます。 トピックのメッセージを表示する通知アイコンを開きます。 

[![通知としてトピックのメッセージが表示されます。](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png#lightbox)

メッセージを受け取らない場合は、削除してください。、 **FCMClient**デバイス (またはエミュレーター) 上のアプリ、上記の手順を繰り返します。 

## <a name="foreground-notifications"></a>フォア グラウンド通知

実装する必要があります foregrounded アプリで通知を受信する`FirebaseMessagingService`です。 このサービスは、データ ペイロードを受信するため、アップ ストリームのメッセージを送信する必要もできます。 次の例は、サービス拡張を実装する方法を示します`FirebaseMessagingService`&ndash;結果として得られるアプリケーションがフォア グラウンドで実行中に、リモートの通知を処理できます。 

### <a name="implement-firebasemessagingservice"></a>FirebaseMessagingService を実装します。

`FirebaseMessagingService`サービスが含まれています、`OnMessageReceived`メソッド受信メッセージを処理するために作成します。 なお`OnMessageReceived`通知メッセージが呼び出される*のみ*アプリがフォア グラウンドで実行されている場合。 アプリは、バック グラウンドで実行中は、自動的に生成された通知が表示されます (このチュートリアルで既に説明した)。 

という名前の新しいファイルを追加**MyFirebaseMessagingService.cs**し、テンプレート コードを次に置き換えます。 

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Media;
using Android.Util;
using Firebase.Messaging;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
    public class MyFirebaseMessagingService : FirebaseMessagingService
    {
        const string TAG = "MyFirebaseMsgService";
        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);
            Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
        }
    }
}
```

なお、`MESSAGING_EVENT`インテント フィルターは、新しい FCM メッセージに送信できるように宣言する必要があります`MyFirebaseMessagingService`: 

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

クライアント アプリは FCM からメッセージを受信`OnMessageReceived`から渡されたメッセージの内容を抽出`RemoteMessage`オブジェクトを呼び出してその`GetNotification`メソッドです。 次に、IDE の出力 ウィンドウで表示できるように、メッセージの内容を記録します。 

```csharp
Log.Debug(TAG, "From: " + message.From);
Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
```

> [!NOTE]
> ブレークポイントを設定した場合`FirebaseMessagingService`、デバッグ セッションはまたは FCM がメッセージを配信する方法のためのこれらのブレークポイントをヒットしない可能性があります。
 

### <a name="send-another-message"></a>別のメッセージを送信します。

アプリをアンインストール、再構築から実行し直して、および別のメッセージを送信する次の手順します。

1.  Firebase コンソールで **新しいメッセージ**です。 

2.  **作成メッセージ** ページで、メッセージのテキストを入力し、選択**1 つのデバイス**です。 

3.  IDE は、出力ウィンドウからトークン文字列をコピーして貼り付けます、 **FCM 登録トークン**Firebase コンソールを以前と同様のフィールドです。 

4.  アプリがフォア グラウンドで実行されていることを確認し、クリックして**メッセージの送信**Firebase コンソール。 

    [![コンソールから別のメッセージを送信します。](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png#lightbox)

5.  ときに、**レビュー メッセージ**ダイアログが表示されたら、をクリックして**送信**です。

6.  IDE は、出力ウィンドウには、受信メッセージが記録されます。

    [![メッセージ本文は、出力ウィンドウに出力](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png#lightbox)


### <a name="add-a-local-notifications-sender"></a>ローカルの通知の送信側を追加します。

残りの例では、受信 FCM メッセージは、アプリがフォア グラウンドで実行されているときに起動されるローカルの通知に変換されます。 編集**MyFirebaseMessageService.cs**し、以下の追加`using`ステートメント。 

```csharp
using FCMClient;
using System.Collections.Generic;
```

次のメソッドを追加`MyFirebaseMessagingService`: 

```csharp
void SendNotification(string messageBody, IDictionary<string, string> data)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    foreach (string key in data.Keys)
    {
        intent.PutExtra(key, data[key]);
    }
    var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

    var notificationBuilder = new Notification.Builder(this)
        .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
        .SetContentTitle("FCM Message")
        .SetContentText(messageBody)
        .SetAutoCancel(true)
        .SetContentIntent(pendingIntent);

    var notificationManager = NotificationManager.FromContext(this);
    notificationManager.Notify(0, notificationBuilder.Build());
}
```

バック グラウンド通知からこの通知を区別するためには、このコードは、applicatiion アイコンとは異なるアイコンが通知をマークします。 追加[ic\_stat\_ic\_notification.png](remote-notifications-with-fcm-images/ic-stat-ic-notification.png)に**リソース/描画**し、含めることで、 **FCMClient**プロジェクト。 

`SendNotification`メソッドを使用`Notification.Builder`、通知を作成して`NotificationManager`を使用して、通知を起動します。 通知を保持する`PendingIntent`はアプリを開きに渡された文字列の内容を表示するユーザーに許可する`messageBody`です。 Android アプリを対象とする場合のバージョンに応じてが使用する`NotificationCompat.Builder`代わりにします。 詳細については`Notification.Builder`を参照してください[ローカル通知](~/android/app-fundamentals/notifications/local-notifications.md)です。 

呼び出す、`SendNotification`メソッドの最後に、`OnMessageReceived`メソッド。 

```csharp
SendNotification(message.GetNotification().Body, message.Data);
```

これらの変更の結果として`SendNotification`アプリが、フォア グラウンドでは、通知領域に、通知は表示通知を受け取るたびに実行されます。 場合は、アプリを backgrounded`SendNotification`実行されず、代わりに、(このチュートリアルで示したよう)、バック グラウンド通知を起動します。 

### <a name="send-the-last-message"></a>最後のメッセージを送信します。

アプリをアンインストールし、再構築をから実行し直して、最後のメッセージを送信する、次の手順を使用します。

1.  Firebase コンソールで **新しいメッセージ**です。 

2.  **作成メッセージ** ページで、メッセージのテキストを入力し、選択**1 つのデバイス**です。 

3.  IDE は、出力ウィンドウからトークン文字列をコピーして貼り付けます、 **FCM 登録トークン**Firebase コンソールを以前と同様のフィールドです。 

4.  アプリがフォア グラウンドで実行されていることを確認し、クリックして**メッセージの送信**Firebase コンソール。 

    [![フォア グラウンド メッセージを送信します。](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png#lightbox)

このとき、出力ウィンドウに記録されたメッセージは、新しい通知にパッケージも&ndash;アプリがフォア グラウンドで実行されているときに、通知のトレイに通知アイコンが表示されます。 

[![フォア グラウンド メッセージの通知アイコン](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png#lightbox)

開くと、通知、Firebase コンソール通知 GUI から送信された最後のメッセージが表示されます。 

[![フォア グラウンドのアイコンが示すようにフォア グラウンド通知](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png#lightbox)


## <a name="disconnecting-from-fcm"></a>FCM からの切断

トピック サブスクリプションの解除を呼び出す、 [UnsubscribeFromTopic](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging.html#unsubscribeFromTopic%28java.lang.String%29)メソッドを[FirebaseMessaging](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging)クラスです。 たとえばの登録を解除するため、_ニュース_にトピックが以前にサブスクライブしている、 **Unsubscribe**ボタンは、レイアウトを次のハンドラー コードに追加できませんでした。

```csharp
var unSubscribeButton = FindViewById<Button>(Resource.Id.unsubscribeButton);
unSubscribeButton.Click += delegate {
    FirebaseMessaging.Instance.UnsubscribeFromTopic("news");
    Log.Debug(TAG, "Unsubscribed from remote notifications");
};
```

FCM を完全からデバイスの登録を解除するには、呼び出すことにより、インスタンス ID を削除、 [DeleteInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId.html#deleteInstanceId%28%29)メソッドを[FirebaseInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId)クラスです。 例えば:

```csharp
FirebaseInstanceId.Instance.DeleteInstanceId();
```

このメソッドの呼び出しによって削除され、インスタンス ID と関連付けられているデータ。 その結果、デバイスに FCM データの定期的な送信は停止します。

 
## <a name="troubleshooting"></a>トラブルシューティング

次の記述問題と Xamarin.Android を Firebase Cloud Messaging を使用するときに発生する可能性が回避策が必要です。

### <a name="firebaseapp-is-not-initialized"></a>FirebaseApp が初期化されていません

場合によっては、このエラー メッセージを参照してください可能性があります。

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

これは、ソリューションをクリーンアップして、プロジェクトを再構築を回避することが既知の問題 (**ビルド > ソリューションのクリーン**、**ビルド > ソリューションのリビルド**)。 詳細については、これを参照して[フォーラムのディスカッション](https://forums.xamarin.com/discussion/96263/default-firebaseapp-is-not-initialized-in-this-process)です。


## <a name="summary"></a>まとめ

このチュートリアルでは、Xamarin.Android アプリケーションでリモートの通知を Firebase クラウド メッセージングを実装するための手順が詳しく説明します。 FCM 通信のために必要なパッケージをインストールする方法が説明されているし、FCM サーバーにアクセスする Android マニフェストを構成する方法を説明します。 これには、Google Play サービスの存在をチェックする方法を示したサンプル コードが用意されています。 登録トークンでは、FCM とネゴシエートするインスタンス ID リスナー サービスを実装する方法が示されているし、アプリが backgrounded 中に、このコードがバック グラウンド通知を作成する方法が説明されています。 トピックのメッセージをサブスクライブする方法を説明し、受信し、アプリがフォア グラウンドで実行中に、リモートの通知を表示するために使用するメッセージ リスナー サービスの実装例を提供します。 


## <a name="related-links"></a>関連リンク

- [FCMNotifications (サンプル)](https://developer.xamarin.com/samples/monodroid/Firebase/FCMNotifications)
- [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
