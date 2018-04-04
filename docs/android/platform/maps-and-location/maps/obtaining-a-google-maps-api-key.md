---
title: API キーをマップして、Google の取得
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: c37fce491b2e6f5e0211fcc6aa7906643a1bac2a
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="obtaining-a-google-maps-api-key"></a>API キーをマップして、Google の取得

Google Maps 機能を使用して、Android で、Google とのマップの API キーを登録する必要があります。 これを行うまでは、アプリケーションで、マップの代わりに空白のグリッドだけ表示されます。 Google Maps Android API v2 キーを取得する必要があります - 古い Google Maps Android API キー v1 からのキーは機能しません。

マップの API v2 キーを取得するには、次の手順が含まれます。

1.  アプリケーションの署名に使用されるキー ストアの sha-1 フィンガー プリントを取得します。
2.  Google Api コンソールで、プロジェクトを作成します。
3.  API キーを取得します。


## <a name="obtaining-your-signing-key-fingerprint"></a>署名キーのフィンガー プリントを取得します。

Google からのマップの API キーを要求するには、アプリケーションの署名に使用されるキー ストアの sha-1 フィンガー プリントを理解する必要があります。
通常、つまりデバッグ キーストアの sha-1 指紋を決定する必要がおよび、sha-1 のリリースのアプリケーションの署名に使用されるキー ストア用の指紋しです。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

既定では、使われるキーストアをアプリケーションであることができます Xamarin.Android のデバッグ バージョンの署名には、次の場所が見つかりました。

**C:\\ユーザー\\[USERNAME]\\AppData\\ローカル\\Xamarin\\Android 用の Mono\\debug.keystore**

キーストアに関する情報は、JDK から `keytool` コマンドを実行して取得できます。 このツールは、Java の bin ディレクトリでよく見られます。

**C:\\%program Files (x86)\\Java\\jdk [バージョン]\\bin\\keytool.exe**

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

既定では、使われるキーストアをアプリケーションであることができます Xamarin.Android のデバッグ バージョンの署名には、次の場所が見つかりました。

**/Users/[USERNAME]/.local/share/Xamarin/Mono for Android/debug.keystore**

キーストアに関する情報は、JDK から `keytool` コマンドを実行して取得できます。 このツールは、Java の bin ディレクトリでよく見られます。

**/System/Library/Java/JavaVirtualMachines/[VERSION].jdk/Contents/Home/bin/keytool**

-----


Keytool (上記のファイルのパスを使用)、次のコマンドを実行します。

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Debug.keystore 例

(デバッグ用に自動的に作成) を既定のデバッグ キーに対してこのコマンドを使用します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----


### <a name="production-keys"></a>実稼働キー

アプリを Google Play を展開する場合があります[秘密キーで署名](~/android/deploy-test/signing/index.md)です。
`keytool`する秘密キーの詳細と、結果として得られる sha-1 指紋を実稼働 Google Maps API キーを作成するために使用して実行する必要があります。 忘れずに更新、 **AndroidManifest.xml**展開する前に正しい Google Maps API キーを持つファイルです。

### <a name="keytool-output"></a>Keytool 出力

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

Sha-1 指紋を使用する (後に表示されている**SHA1**) このガイドで後述します。


## <a name="creating-an-api-project"></a>API プロジェクトを作成します。

キーストアを署名の sha-1 フィンガー プリントを取得した後は、Google APIs console で新しいプロジェクトを作成する (または Google Maps Android API v2 のサービスを既存のプロジェクトに追加) 必要があります。

1. ブラウザーに移動、 [Google Developers Console](https://console.developers.google.com/): をクリックして**プロジェクトの作成**:

   [![Google 開発者コンソール プロジェクトの作成 ボタン](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. **新しいプロジェクト**、表示されるダイアログ ボックスに、プロジェクト名を入力してください。
   この例で示すように、ダイアログ ボックスは、プロジェクト名に基づいている一意のプロジェクト ID を製造します。

   [![XamarinMapsDemo は新しいプロジェクトを名前します。](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. **[作成]** ボタンをクリックします。 1 分間、またはそのため、後に、プロジェクトが作成されに表示される、 **API マネージャー**ページ。 **ライブラリ**セクションで、 **Google Maps Android API**:

   [![[ライブラリ] セクションの Google Maps Android API のクリックして](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. 上部にある、 **Google Maps Android API**  ページで、をクリックして**を有効にする**このプロジェクトのサービスを有効にします。

   [![ダッシュ ボード セクションで有効にする ボタンをクリックして](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)


API プロジェクトを作成したこの時点でとして、Google マップ Android API v2 が追加されました。 ただし、その資格情報を作成するまで、プロジェクトのこの API を使用することはできません。 次にこのキーを使用することが承認されているように、API キーおよび Xamarin.Android アプリケーションのホワイト リストを作成する方法を紹介します。


## <a name="obtaining-the-api-key"></a>API キーを取得します。

後に、 **Google Developer Console** API プロジェクトを作成する必要は Android API キーを作成します。 Xamarin.Android アプリケーションは、Android のマップ API v2 へのアクセスを許可する前に、API キーにすることが必要です。

1. **Google Maps Android API**表示されるページ (クリックした後**を有効にする**前の手順で)、をクリックして、**資格情報を参照してください**ボタン。

   [![この API が有効になっているメッセージ](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. **資格情報** ページで、をクリックして、**資格情報が必要ですか?**ボタン。

   [![資格情報を [プロジェクト] ダイアログ ボックスに追加します。](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. このボタンをクリックすると、API キーが生成されます。 次に必要があるアプリのみがこのキーを持つ Api を呼び出すことができますので、このキーを制限します。 をクリックして**Restrict キー**:

   [![資格情報 ページをクリックするとキーの制限](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. 変更、**名前**からフィールド**API キー 1**キーの用途がわかりやすい名前に (**XamarinMapsDemoKey**は、この例で使用)。 次をクリックして、**に Android アプリを**ラジオ ボタンをクリックします。

   [![資格情報 ページで Android アプリを選択します。](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. Sha-1 指紋を追加する をクリックして**+ パッケージの名前と指紋の追加**:

   [![名前を追加するパッケージと指紋](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. アプリのパッケージ名を入力し、sha-1 の証明書フィンガー プリントを入力 (経由で取得した`keytool`このガイドの前半で説明したよう)。 次の例では、パッケージの名前の`XamarinMapsDemo`入力は、sha-1 証明書フィンガー プリントから取得した後に、 **debug.keystore**:

   [![入力したパッケージ名は com.xamarin.docs.android.map](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. ドキュメントが開かれて、APK Google Maps にアクセスするためには、する必要があります sha-1 指紋が含まれますすべてキーストアの (デバッグとリリース)、APK の署名に使用する名前をパッケージ化します。 たとえば、デバッグ構成とリリース APK を生成するための別のコンピューターを 1 台のコンピューターを使用する場合を含める必要がありますデバッグ キーストアを最初のコンピューターから sha-1 証明書フィンガー プリントおよびのリリース キーストアから sha-1 の証明書フィンガー プリント2 番目のコンピューター。 をクリックして**+ パッケージの名前と指紋の追加**をこの例で示すように、別の指紋とパッケージ名を追加します。

   [![Sha-1 の別の証明書を作成する別の指紋を追加します。](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. **[Save]\(保存\)** ボタンをクリックして変更内容を保存します。 次に、API キーの一覧に戻ります。 先ほど作成した他の API キーを使っている場合はもここに表示します。 この例では、(前の手順で作成される) 1 つだけの API キーが表示されます。

   [![XamarinMapsDemoKey リストに表示される、API キー](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)



## <a name="adding-the-key-to-your-project"></a>キーをプロジェクトに追加します。

最後に、この API キーを追加、 **AndroidManifest.XML** Xamarin.Android アプリのファイルです。 次の例では、`YOUR_API_KEY`前の手順で生成される API キーと置き換えられます。

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionName="4.10" package="com.xamarin.docs.android.mapsandlocationdemo"
    android:versionCode="10">
...

  <application android:label="@string/app_name">
    <!-- Put your Google Maps V2 API Key here. -->
    <meta-data android:name="com.google.android.geo.API_KEY" android:value="YOUR_API_KEY" />
    <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
  </application>
</manifest>
```


## <a name="related-links"></a>関連リンク

- [Google Api コンソール](https://code.google.com/apis/console/)
- [Google は、API キーをマップします。](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
