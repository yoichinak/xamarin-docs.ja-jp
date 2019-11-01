---
title: Google マップ API キーを取得する
description: アプリに maps 機能を追加するための Google Maps API キーを取得する方法。
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: bf0a099546b2d5610a639cbf9af4c7676d10bef9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020045"
---
# <a name="obtaining-a-google-maps-api-key"></a>Google マップ API キーを取得する

Android で Google Maps 機能を使用するには、Maps API キーを Google に登録する必要があります。 これを行うまでは、アプリケーション内のマップではなく、空のグリッドが表示されます。 Google maps Android API v2 キーキーを取得する必要があります。以前の Google Maps Android API キー v1 は使用できません。

Maps API v2 キーを取得するには、次の手順を実行します。

1. アプリケーションの署名に使用されるキーストアの SHA-1 フィンガープリントを取得します。
2. Google Api コンソールでプロジェクトを作成します。
3. API キーを取得しています。

## <a name="obtaining-your-signing-key-fingerprint"></a>署名キーの指紋を取得する

Google から Maps API キーを要求するには、アプリケーションの署名に使用されるキーストアの SHA-1 フィンガープリントを把握している必要があります。
通常、これは、デバッグキーストアの SHA-1 フィンガープリントと、アプリケーションのリリースに署名するために使用されるキーストアの SHA-1 フィンガープリントを決定する必要があることを意味します。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

既定では、Xamarin. Android アプリケーションのデバッグバージョンに署名するために使用されるキーストアは、次の場所にあります。

**C:\\ユーザー\\[USERNAME]\\AppData\\ローカル\\Xamarin\\Mono for Android\\デバッグ. キーストア**

キーストアに関する情報は、JDK から `keytool` コマンドを実行して取得できます。 このツールは通常、Java bin ディレクトリにあります。

**C:\\Program Files (x86)\\Java\\jdk [バージョン]\\bin\\keytool**

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

既定では、Xamarin. Android アプリケーションのデバッグバージョンに署名するために使用されるキーストアは、次の場所にあります。

**/ユーザー/[USERNAME]/.local/share/Xamarin/Mono for Android/debug. キーストア**

キーストアに関する情報は、JDK から `keytool` コマンドを実行して取得できます。 このツールは通常、Java bin ディレクトリにあります。

**/System/library/java/javavirtualmachines/version .jdk [バージョン] です。 jdk/Contents/Home/bin/keytool**

-----

次のコマンドを使用して keytool を実行します (上記のファイルパスを使用)。

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>キーストアの例

既定のデバッグキー (デバッグ用に自動的に作成されます) の場合は、次のコマンドを使用します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----

### <a name="production-keys"></a>運用キー

Google Play にアプリを展開する場合は、[秘密キーで署名](~/android/deploy-test/signing/index.md)する必要があります。
`keytool` は、秘密キーの詳細と、実稼働 Google Maps API キーの作成に使用される SHA-1 フィンガープリントを使用して実行する必要があります。 デプロイする前に、必ず、正しい Google Maps API キーで**Androidmanifest .xml**ファイルを更新してください。

### <a name="keytool-output"></a>Keytool の出力

コンソールウィンドウに次のような出力が表示されます。

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

このガイドの後半では、SHA-1 フィンガープリント ( **SHA1**の後に記載) を使用します。

## <a name="creating-an-api-project"></a>API プロジェクトの作成

署名キーストアの SHA-1 フィンガープリントを取得した後、Google Api コンソールで新しいプロジェクトを作成する必要があります (または Google Maps Android API v2 サービスを既存のプロジェクトに追加する必要があります)。

1. ブラウザーで[Google 開発者コンソール API & サービスダッシュボード](https://console.developers.google.com/apis/dashboard/)に移動し、 **[プロジェクトの選択]** をクリックします。 プロジェクト名をクリックするか、 **[新しいプロジェクト]** をクリックして新しい名前を作成します。

   [![Google Developer Console [プロジェクトの作成] ボタン](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. 新しいプロジェクトを作成した場合は、表示される **[新しいプロジェクト]** ダイアログボックスにプロジェクト名を入力します。 このダイアログでは、プロジェクト名に基づいて一意のプロジェクト ID が作成されます。 次に、次の例に示すように、 **[作成]** ボタンをクリックします。

   [![の新しいプロジェクトの名前は XamarinMapsDemo です。](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. 1分以上経過すると、プロジェクトが作成され、プロジェクトの **[ダッシュボード]** ページに移動します。 そこから、 **[API とサービスを有効にする]** をクリックします。

   [[ライブラリ] セクションで Google Maps Android API をクリック![](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. **[API ライブラリ]** ページで、 **[Maps SDK for Android]** をクリックします。 次のページで、 **[有効]** をクリックして、このプロジェクトのサービスを有効にします。

   [[ダッシュボード] セクションの [有効にする] ボタンをクリック![](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)

この時点で、API プロジェクトが作成され、Google Maps Android API v2 が追加されました。 ただし、この API をプロジェクトで使用するには、資格情報を作成する必要があります。 次のセクションでは、このキーを使用する権限を持つように、API キーを作成し、Xamarin Android アプリケーションのホワイトリストを作成する方法について説明します。

## <a name="obtaining-the-api-key"></a>API キーの取得

**Google Developer Console** api プロジェクトを作成したら、Android API キーを作成する必要があります。 Android Map API v2 へのアクセス権を付与するには、Xamarin Android アプリケーションに API キーが必要です。

1. 表示される **[MAPS SDK For Android]** ページ (前の手順で **[有効に]** する をクリックした後) で、 **[資格情報]** タブに進み、 **[資格情報の作成]** ボタンをクリックします。

   [![Maps SDK for Android の資格情報メッセージ](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. **[API キー]** をクリックします。

   [[プロジェクト] ダイアログに資格情報を追加![には](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. このボタンをクリックすると、API キーが生成されます。 次に、アプリだけがこのキーを使用して Api を呼び出すことができるように、このキーを制限する必要があります。 **[キーの制限]** をクリックします。

   [[資格情報] ページで [キーの制限] をクリック![](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. **[名前]** フィールドを**API キー 1**から名前に変更します。これにより、キーがどのように使用されるかを覚えやすくなります (この例では**XamarinMapsDemoKey**が使用されています)。 次に、 **[Android アプリ]** ラジオボタンをクリックします。

   [[資格情報] ページで Android アプリを選択![](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. SHA-1 フィンガープリントを追加するには、 **[+ パッケージ名と指紋の追加]** をクリックします。

   [[パッケージ名と指紋の追加] をクリック![](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. アプリのパッケージ名を入力し、SHA-1 証明書のフィンガープリントを入力します (このガイドで既に説明したように、`keytool` によって取得されます)。 次の例では、`XamarinMapsDemo` のパッケージ名を入力し、その後に、デバッグで取得した SHA-1 証明書のフィンガープリントを指定します **。**

   [入力したパッケージ名は![です。](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. APK が Google Maps にアクセスできるようにするには、APK に署名するために使用するすべてのキーストア (デバッグとリリース) に SHA-1 指紋とパッケージ名を含める必要があることに注意してください。 たとえば、デバッグに1台のコンピューターを使用し、リリース APK を生成する別のコンピューターを使用する場合は、最初のコンピューターのデバッグキーストアから SHA-1 証明書のフィンガープリントを指定し、次のリリースキーストアから SHA-1 証明書のフィンガープリントを含める必要があります。2番目のコンピューター。 **[+ パッケージ名と指紋の追加]** をクリックして、次の例に示すように、別の指紋とパッケージ名を追加します。

   [別の指紋を追加![と、別の SHA-1 証明書が作成されます。](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. **[Save]\(保存\)** ボタンをクリックして変更内容を保存します。 次に、API キーの一覧に戻ります。 以前に作成した他の API キーがある場合は、ここにも表示されます。 この例では、(前の手順で作成した) API キーが1つだけ表示されています。

   [![XamarinMapsDemoKey が API キーの一覧に表示されます。](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)

## <a name="connect-the-project-to-a-billable-account"></a>課金対象のアカウントにプロジェクトを接続する

プロジェクトが課金対象アカウントに接続されていない場合 (サービスがまだモバイルアプリに対して無料の場合でも)、11月 2018 11 日以降、API キーは機能しません。

1. [ハンバーガー] メニューボタンをクリックし、**課金**ページを選択します。

   [ハンバーガーメニューの請求セクションを選択![](obtaining-a-google-maps-api-key-images/13-goto-billing-vs-sml.png)](obtaining-a-google-maps-api-key-images/13-goto-billing-vs.png#lightbox)

2. 課金アカウントにプロジェクトをリンクするには、 **[課金アカウントのリンク]** をクリックし、表示されたポップアップで **[課金アカウントの作成]** をクリックします (アカウントがない場合は、新しいアカウントを作成します)。

   [プロジェクトを請求先アカウントにリンク![](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs-sml.png)](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs.png#lightbox)

## <a name="adding-the-key-to-your-project"></a>プロジェクトへのキーの追加

最後に、この API キーを Xamarin Android アプリの**Androidmanifest .xml**ファイルに追加します。 次の例では、`YOUR_API_KEY` は、前の手順で生成された API キーに置き換えられます。

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

- [Google Api コンソール](https://code.google.com/apis/console/)
- [Google Maps API キー](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](https://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
