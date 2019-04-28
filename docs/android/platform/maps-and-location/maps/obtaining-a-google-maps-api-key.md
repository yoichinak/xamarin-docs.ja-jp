---
title: Google マップ API キーを取得する
description: 追加するための Google マップ API キーを取得する方法は、機能をアプリにマップします。
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/25/2018
ms.openlocfilehash: bfeb9d8fa2a0b5a9b18ab8266500586e2e3b6c68
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61155365"
---
# <a name="obtaining-a-google-maps-api-key"></a>Google マップ API キーを取得する

Google Maps 機能を使用して、Android では、Google マップ API キーを登録する必要があります。 これを行うまでは、アプリケーションで空白のグリッド、マップではなくだけ表示されます。 Google Maps Android API v2 キーを取得する必要があります - 古い Google Maps Android API キー v1 からのキーは機能しません。

V2 マップ API キーを取得するには、次の手順が含まれます。

1.  アプリケーションの署名に使用されるキーストアの sha-1 フィンガー プリントを取得します。
2.  Google APIs console で、プロジェクトを作成します。
3.  API キーを取得します。


## <a name="obtaining-your-signing-key-fingerprint"></a>署名、キーの拇印を取得します。

Google マップ API キーを要求するには、アプリケーションに署名するために使用されるキーストアの sha-1 フィンガー プリントを把握する必要があります。
通常、これは、デバッグ キーストアの sha-1 で指紋を決定する必要があり、リリースのアプリケーションへの署名に使用されるキーストアの sha-1 がフィンガー プリントを意味します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

既定では、キーストアのデバッグ バージョンのアプリケーションは、Xamarin.Android の署名に使用するには、次の場所に見つかりませんでした。

**C:\\ユーザー\\[USERNAME]\\AppData\\ローカル\\Xamarin\\Mono for Android\\debug.keystore**

キーストアに関する情報は、JDK から `keytool` コマンドを実行して取得できます。 このツールは、Java の bin ディレクトリでよく見られます。

**C:\\Program Files (x86)\\Java\\jdk [バージョン]\\bin\\keytool.exe**

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

既定では、キーストアのデバッグ バージョンのアプリケーションは、Xamarin.Android の署名に使用するには、次の場所に見つかりませんでした。

**Android/debug.keystore の/Users/[USERNAME]/.local/share/Xamarin/Mono**

キーストアに関する情報は、JDK から `keytool` コマンドを実行して取得できます。 このツールは、Java の bin ディレクトリでよく見られます。

**/System/Library/Java/JavaVirtualMachines/[VERSION].jdk/Contents/Home/bin/keytool**

-----


(上記のファイル パスを使用して) 次のコマンドを使用して keytool を実行します。

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Debug.keystore 例

(デバッグ用に自動的に作成) を既定のデバッグ キーのこのコマンドを使用します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----


### <a name="production-keys"></a>実稼働キー

Google Play にアプリをデプロイするときにする必要があります[秘密キーで署名](~/android/deploy-test/signing/index.md)します。
`keytool`秘密キーの詳細と運用環境の Google マップ API キーを作成するために使用、結果として得られる sha-1 指紋併用して実行する必要があります。 忘れずに更新、 **AndroidManifest.xml**展開する前に正しい Google マップ API キーを持つファイル。

### <a name="keytool-output"></a>Keytool の出力

コンソール ウィンドウに次の出力のようなものが表示されます。

```shell
Alias name: androiddebugkey
Creation date: Jan 01, 2016
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 4aa9b300
Valid from: Mon Jan 01 08:04:04 UTC 2013 until: Mon Jan 01 18:04:04 PST 2033
Certificate fingerprints:
    MD5:  AE:9F:95:D0:A6:86:89:BC:A8:70:BA:34:FF:6A:AC:F9
    SHA1: BB:0D:AC:74:D3:21:E1:43:07:71:9B:62:90:AF:A1:66:6E:44:5D:75
    Signature algorithm name: SHA1withRSA
    Version: 3
```

Sha-1 で指紋を使用する (後に表示されている**SHA1**) このガイドで後述します。

## <a name="creating-an-api-project"></a>API プロジェクトを作成します。

署名キーストアの sha-1 フィンガー プリントを取得した後には、Google APIs console で新しいプロジェクトを作成 (または Google Maps Android API v2 サービスを既存のプロジェクトに追加する) 必要があります。

1. ブラウザーに移動します。、 [Google 開発者コンソール API とサービスのダッシュ ボード](https://console.developers.google.com/apis/dashboard/)クリック**プロジェクトを選択**。 プロジェクト名をクリックするかをクリックして、新しく作成**新しいプロジェクト**:

   [![Google デベロッパー コンソール プロジェクトの作成 ボタン](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. 新しいプロジェクトを作成する場合は、プロジェクト名を入力、**新しいプロジェクト**ダイアログが表示されます。 このダイアログ ボックスは、プロジェクト名に基づいている一意のプロジェクト ID を製造します。 次に、をクリックして、**作成**この例で示すようにボタンをクリックします。

   [![XamarinMapsDemo は新しいプロジェクトを名前します。](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. プロジェクトの作成後、しばらくすると表示されます、**ダッシュ ボード**プロジェクトのページ。 そこから、次のようにクリックします**API とサービスを有効にする**:。

   [![ライブラリ セクションでの Google Maps Android API をクリックします。](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. **API ライブラリ**] ページで [ **Maps SDK for Android**します。 次のページで次のようにクリックします。**を有効にする**このプロジェクトのサービスを有効にします。

   [![ダッシュ ボード セクションで有効にする ボタンをクリックして](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)

この時点で、API プロジェクトが作成されているし、Google Maps Android API v2 は追加されています。 ただし、その資格情報を作成するまで、プロジェクトでこの API を使用することはできません。 次のセクションでは、このキーを使用することが承認されているように、API キーと、Xamarin.Android アプリケーションのホワイト リストを作成する方法について説明します。

## <a name="obtaining-the-api-key"></a>API キーを取得します。

後に、 **Google Developer Console** API プロジェクトを作成する必要は Android API キーを作成します。 Xamarin.Android アプリケーションは、Android の Map API v2 へのアクセスが付与される前に、API キーにすることが必要です。

1. **Maps SDK for Android**表示されるページ (クリックした後**を有効にする**前の手順で) に移動、**資格情報** タブでをクリックし、**を作成します。資格情報**ボタンをクリックします。

   [![Android の資格情報のメッセージ用 maps SDK](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. クリックして**API キー**:

   [![資格情報、[プロジェクト] ダイアログ ボックスに追加します。](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. このボタンをクリックすると、API キーが生成されます。 次にアプリのみがこのキーを使用して Api を呼び出すことができますので、このキーを制限するために必要です。 クリックして**RESTRICT キー**:

   [![資格情報 ページで [キーの制限] をクリックして](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. 変更、**名前**フィールドから**API キー 1**キーの使用を覚えておくのに役立つ名前を (**XamarinMapsDemoKey**はこの例で使用)。 次に、クリックして、 **Android アプリ**ラジオ ボタンをクリックします。

   [![資格情報 ページで Android アプリを選択します。](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. Sha-1 フィンガー プリントを追加するには、次のようにクリックします **+ パッケージ名と指紋の追加**:。

   [![名前を追加するパッケージとフィンガー プリント](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. アプリのパッケージ名を入力し、sha-1 証明書フィンガー プリントを入力します (経由で取得した`keytool`このガイドの前半で説明)。 次の例では、パッケージの名前の`XamarinMapsDemo`が入力され、その後から取得した sha-1 証明書フィンガー プリント**debug.keystore**:

   [![パッケージ名の入力が com.xamarin.docs.android.map](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. 注意してください、APK を Google Maps にアクセスするためには、する必要があります sha-1 指紋が含まれますすべてキーストアの (デバッグとリリース)、APK の署名に使用する名前をパッケージ化します。 たとえば、デバッグおよびリリース APK を生成するための別のコンピュータを 1 台のコンピューターを使用する場合を含める必要がありますデバッグ キーストアを最初のコンピューターから、sha-1 証明書フィンガー プリントのリリース キーストアから sha-1 証明書フィンガー プリント2 番目のコンピューター。 クリックして **+ パッケージ名と指紋の追加**この例で示すように、別の指紋とパッケージ名を追加します。

   [![別の指紋を追加するには、別の sha-1 証明書が作成されます。](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. **[Save]\(保存\)** ボタンをクリックして変更内容を保存します。 次に、API キーの一覧に戻ります。 前に作成したその他の API キーがあればをここに表示することもされます。 この例では、(前の手順で作成される) 1 つの API キーが表示されます。

   [![XamarinMapsDemoKey が API キーの一覧に表示されます。](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)

## <a name="connect-the-project-to-a-billable-account"></a>課金対象のアカウントに、プロジェクトを接続します。

2018 年 6 月、11、(たとえサービスは、モバイル アプリ向け無料) に、プロジェクトが課金対象のアカウントに接続されていない場合も、API キーは機能ではありません。

1. ハンバーガー メニュー ボタンをクリックし、選択、**課金**ページ。

   [![ハンバーガーメニューの [課金] セクションを選択します。](obtaining-a-google-maps-api-key-images/13-goto-billing-vs-sml.png)](obtaining-a-google-maps-api-key-images/13-goto-billing-vs.png#lightbox)

2. 請求先アカウントをクリックしてプロジェクトをリンク**請求先アカウントをリンク**続けて**請求先アカウントの作成**で表示されているポップアップ (アカウントを持っていない場合は説明、新しく作成する)。

   [![請求先アカウントにリンク プロジェクト](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs-sml.png)](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs.png#lightbox)

## <a name="adding-the-key-to-your-project"></a>プロジェクトにキーを追加します。

最後に、この API キーを追加、 **AndroidManifest.XML** Xamarin.Android アプリのファイル。 次の例では、`YOUR_API_KEY`が前の手順で生成された API キーで置き換えられます。

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionName="4.10" package="com.xamarin.docs.android.mapsandlocationdemo"
    android:versionCode="10">
...
  <application android:label="@string/app_name">
    <!-- Put your Google Maps V2 API Key here. -->
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
    <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
  </application>
</manifest>
```

## <a name="related-links"></a>関連リンク

- [Google APIs Console](https://code.google.com/apis/console/)
- [Google マップ API キー](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
