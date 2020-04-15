---
title: Google マップ API キーを取得する
description: アプリにマップ機能を追加するための Google Maps API キーを取得する方法。
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: 371876d087c7027d4cfe2d2d9ada8b0dbedb5dd5
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "75488973"
---
# <a name="obtaining-a-google-maps-api-key"></a>Google マップ API キーを取得する

Android で Google Maps 機能を使用するには、Maps API キーを Google に登録する必要があります。 これを行うまで、アプリケーションには、マップではなく空白のグリッドだけが表示されます。 Google Maps Android API v2 キーを取得する必要があります。古い Google Maps Android API v1 キーは機能しません。

Maps API v2 キーを取得するには、次の手順を実行します。

1. アプリケーションの署名に使用されるキーストアの SHA-1 フィンガープリントを取得します。
2. Google API Console でプロジェクトを作成します。
3. API キーを取得します。

## <a name="obtaining-your-signing-key-fingerprint"></a>署名キーのフィンガープリントを取得する

Google に Maps API キーを要求するには、アプリケーションの署名に使用されるキーストアの SHA-1 フィンガープリントを知っておく必要があります。
通常は、デバッグ キーストアの SHA-1 フィンガープリントと、アプリケーションの署名に使用されるリリース キーストアの SHA-1 フィンガープリントを確認する必要があります。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

既定では、Xamarin.Android アプリケーションのデバッグ バージョンの署名に使用されるキーストアは次の場所にあります。

**C:\\Users\\[ユーザー名]\\AppData\\Local\\Xamarin\\Mono for Android\\debug.keystore**

キーストアに関する情報は、JDK から `keytool` コマンドを実行して取得できます。 通常、このツールは Java bin ディレクトリにあります。

**C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_[VERSION]\\bin\\keytool.exe**

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

既定では、Xamarin.Android アプリケーションのデバッグ バージョンの署名に使用されるキーストアは次の場所にあります。

**/Users/[ユーザー名]/.local/share/Xamarin/Mono for Android/debug.keystore**

キーストアに関する情報は、JDK から `keytool` コマンドを実行して取得できます。 通常、このツールは Java bin ディレクトリにあります。

**/System/Library/Java/JavaVirtualMachines/[バージョン].jdk/Contents/Home/bin/keytool**

-----

次のコマンドを使用して keytool を実行します (上記のファイル パスを使用)。

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Debug.keystore の例

(デバッグ用に自動的に作成される) 既定のデバッグ キーの場合、次のコマンドを使用します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----

### <a name="production-keys"></a>実稼働キー

アプリを Google Play にデプロイする場合、[秘密キーで署名](~/android/deploy-test/signing/index.md)する必要があり ます。
秘密キーの詳細と、実稼働 Google Maps API キーの作成に使用される取得済みの SHA-1 フィンガープリントを使用して、`keytool` を実行する必要があります。 デプロイする前に、**AndroidManifest.xml** ファイルを正しい Google Maps API キーで必ず更新してください。

### <a name="keytool-output"></a>keytool の出力

コンソール ウィンドウに次のような出力が表示されます。

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

(**SHA1** の後に表示されている) SHA-1 フィンガープリントは、このガイドで後ほど使用します。

## <a name="creating-an-api-project"></a>API プロジェクトを作成する

署名キーストアの SHA-1 フィンガープリントを取得したら、Google API Console で新しいプロジェクトを作成する (または Google Maps Android API v2 サービスを既存のプロジェクトに追加する) 必要があります。

1. ブラウザーで、[Google Developers Console の [API とサービス] ダッシュボード](https://console.developers.google.com/apis/dashboard/)に移動し、 **[プロジェクトの選択]** をクリックします。 プロジェクト名をクリックするか、 **[新しいプロジェクト]** をクリックして新しいプロジェクトを作成します。

   [![Google Developers Console のプロジェクトの作成ボタン](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. 新しいプロジェクトを作成する場合は、表示される **[新しいプロジェクト]** ダイアログにプロジェクト名を入力します。 このダイアログでは、プロジェクト名に基づいた一意のプロジェクト ID が作成されます。 次の例に示すように、 **[作成]** ボタンをクリックします。

   [![新しいプロジェクトに XamarinMapsDemo という名前を付ける](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. 1 分程度でプロジェクトが作成され、プロジェクトの **[ダッシュボード]** ページが表示されます。 そこから、 **[API とサービスの有効化]** をクリックします。

   [![[ライブラリ] セクションで Google Maps Android API をクリックする](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. **[API ライブラリ]** ページで、 **[Maps SDK for Android]** をクリックします。 次のページで、 **[有効にする]** をクリックして、このプロジェクトでサービスを有効にします。

   [![[ダッシュボード] セクションで [有効にする] をクリックする](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)

この時点で、API プロジェクトが作成され、Google Maps Android API v2 が追加されました。 ただし、プロジェクトの資格情報を作成するまで、プロジェクトでこの API を使用することはできません。 次のセクションでは、API キーを作成し、Xamarin.Android アプリケーションをホワイトリストに登録して、このキーを使用する権限を付与する方法について説明します。

## <a name="obtaining-the-api-key"></a>API キーを取得する

**Google Developers Console** で API プロジェクトが作成されたら、Android API キーを作成する必要があります。 Xamarin.Android アプリケーションに Android Map API v2 へのアクセスを許可するには、API キーが必要です。

1. (前の手順で **[有効にする]** をクリックした後に) 表示された **[Maps SDK for Android]** ページで、 **[認証情報]** タブに移動し、 **[認証情報を作成]** ボタンをクリックします。

   [![Maps SDK for Android の資格情報に関するメッセージ](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. **[API キー]** をクリックします。

   [![プロジェクトに資格情報を追加するためのダイアログ](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. このボタンをクリックすると、API キーが生成されます。 次に、自分のアプリだけがこのキーを使用して API を呼び出すことができるように、このキーを制限する必要があります。 **[キーを制限]** をクリックします。

   [![[認証情報] ページで [キーを制限] をクリックする](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. **[名前]** フィールドを **API キー 1** から、キーの使用目的を思い出しやすい名前に変更します (この例では **XamarinMapsDemoKey** を使用)。 次に、 **[Android アプリ]** ラジオ ボタンをクリックします。

   [![[認証情報] ページで [Android アプリ] を選択する](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. SHA-1 フィンガープリントを追加するには、 **[+ パッケージ名とフィンガープリントを追加]** をクリックします。

   [![[+ パッケージ名とフィンガープリントを追加] をクリックする](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. アプリのパッケージ名を入力し、SHA-1 証明書フィンガープリント (このガイドで前述したように、`keytool` を使用して取得) を入力します。 次の例では、`XamarinMapsDemo` のパッケージ名が入力され、**debug.keystore** から取得した SHA-1 証明書フィンガープリントが入力されています。

   [![入力されたパッケージ名は com.xamarin.docs.android.map](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. APK が Google Maps にアクセスするには、APK の署名に使用するすべてのキーストア (デバッグおよびリリース) の SHA-1 フィンガープリントとパッケージ名を含める必要があります。 たとえば、1 台のコンピューターをデバッグに使用し、別のコンピューターをリリース APK の生成に使用する場合、最初のコンピューターのデバッグ キーストアの SHA-1 証明書フィンガープリントと、2 番目のコンピューターのリリース キーストアの SHA-1 証明書フィンガープリントを含める必要があります。 次の例に示すように、 **[+ パッケージ名とフィンガープリントを追加]** をクリックして、別のフィンガープリントとパッケージ名を追加します。

   [![別のフィンガープリントを追加すると、別の SHA-1 証明書が作成される](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. **[Save]\(保存\)** ボタンをクリックして変更内容を保存します。 次に、API キーの一覧に戻ります。 以前に作成した他の API キーがある場合は、それらもここに表示されます。 この例では、前の手順で作成された API キーだけが表示されています。

   [![API キーの一覧に表示された XamarinMapsDemoKey](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)

## <a name="connect-the-project-to-a-billable-account"></a>プロジェクトを課金対象アカウントに接続する

2018 年 6 月 11 日以降、プロジェクトが課金対象アカウントに接続されていない場合、API キーは機能しません (そのサービスがモバイル アプリで引き続き無料である場合も同様です)。

1. ハンバーガー メニュー ボタンをクリックし、 **[お支払い]** ページを選択します。

   [![ハンバーガー メニューの [お支払い] セクションを選択する](obtaining-a-google-maps-api-key-images/13-goto-billing-vs-sml.png)](obtaining-a-google-maps-api-key-images/13-goto-billing-vs.png#lightbox)

2. **[請求先アカウントをリンク]** をクリックし、表示されるポップアップで **[請求先アカウントの作成]** をクリックして、プロジェクトを請求先アカウントにリンクします (アカウントがない場合は、指示に従って新しいアカウントを作成します)。

   [![プロジェクトを請求先アカウントにリンクする](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs-sml.png)](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs.png#lightbox)

## <a name="adding-the-key-to-your-project"></a>プロジェクトにキーを追加する

最後に、この API キーを Xamarin.Android アプリの **AndroidManifest.XML** ファイルに追加します。 次の例では、`YOUR_API_KEY` が、前の手順で生成された API キーに置き換えられます。

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

- [Google API Console](https://code.google.com/apis/console/)
- [Google Maps API キー](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](https://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
