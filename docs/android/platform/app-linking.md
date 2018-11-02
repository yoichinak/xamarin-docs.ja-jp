---
title: Android でのアプリ リンク
description: このガイドでは、Android 6.0 がアプリのリンク、web サイトの Url に応答するモバイル アプリに許可する手法がサポートする方法について説明します。 これは、どのようなアプリ リンクは、Android 6.0 アプリケーションでのアプリ リンクを実装する方法、およびドメイン用のモバイル アプリへのアクセス許可を付与する web サイトを構成する方法について説明します。
ms.prod: xamarin
ms.assetid: 48174E39-19FD-43BC-B54C-9AF11D4B1F91
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: dd4ba236df8e5993c7f7ed86393eb66ce01db595
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111266"
---
# <a name="app-linking-in-android"></a>Android でのアプリ リンク

_このガイドでは、Android 6.0 がアプリのリンク、web サイトの Url に応答するモバイル アプリに許可する手法がサポートする方法について説明します。これは、どのようなアプリ リンクは、Android 6.0 アプリケーションでのアプリ リンクを実装する方法、およびドメイン用のモバイル アプリへのアクセス許可を付与する web サイトを構成する方法について説明します。_

## <a name="app-linking-overview"></a>アプリのリンクの概要

モバイル アプリケーションが不要になったサイロで live&ndash;多くの場合は、web サイトと共に、ビジネスの重要なコンポーネント。 企業でモバイル アプリケーションを起動し、モバイル アプリで関連するコンテンツを表示するには、web サイト上のリンク、web サイトおよびモバイル アプリケーションをシームレスに接続することをお勧めします。 *アプリ リンク*(とも呼ば*ディープ リンク*) は、URI に応答し、その URI に対応するモバイル アプリケーションを起動する、モバイル デバイスを許可する 1 つの手法です。

Android でアプリ リンクを処理する、*インテント システム*&ndash;ユーザーは、モバイル ブラウザーにあるリンクをクリックすると、モバイル ブラウザーは Android が登録されているアプリケーションに委任するインテントをディスパッチします。 たとえば、料理の web サイト上のリンクをクリックするとその web サイトに関連付けられているモバイル アプリを開くになりユーザーに特定のレシピを表示します。 1 つ以上のアプリケーションは、その意図を処理するために登録されているし、Android を発生させると呼ばれるものがある場合、*曖昧性除去ダイアログ*インテントを処理するアプリケーションを選択するには、どのようなアプリケーション ユーザーに要請します。例:

![あいまいさ排除のダイアログ ボックスのスクリーン ショットの例](app-linking-images/01-disambiguation-dialog.png)

Android 6.0 は、自動リンクの処理を使用してこのが向上します。 URI の既定のハンドラーとしてアプリケーションを自動的に登録する Android の可能性が&ndash;アプリは自動的に起動し、関連するアクティビティに直接移動します。 URI のクリックを処理するために Android 6.0 の決定は、次の条件によって異なります。

1. **既存のアプリは、URI に関連付けられて既に**&ndash;ユーザーが既に関連付けられている既存のアプリを指定する uri。 その場合は、Android は、そのアプリケーションを使用し続けます。
2. **既存のアプリは、URI に関連付けられていませんが、サポートのアプリがインストールされている**&ndash;このシナリオで、ユーザーが指定されていない既存のアプリ、Android がインストールされているサポートしているアプリケーションを使用して要求を処理するためです。
3. **既存のアプリは、URI に関連付けられていませんが、多くの関連アプリがインストールされている** &ndash; URI をサポートする複数のアプリケーションがあるため、曖昧性除去 ダイアログが表示され、どのアプリは、ユーザーが選択する必要がありますURI を処理します。

場合は、ユーザーは、URI をサポートするようにインストールされているアプリを持たない 1 つがインストールされている、その後、し、Android はそのアプリケーション設定既定のハンドラーとして、URI の URI に関連付けられている web サイトとの関連付けを確認した後。

このガイドは、Android 6.0 アプリケーションを構成する方法と作成して、Android 6.0 でのアプリ リンクをサポートするために、デジタル資産へのリンク ファイルを発行する方法について説明します。

## <a name="requirements"></a>必要条件

このガイドは、Xamarin.Android 6.1 と Android 6.0 (API レベル 23) を対象とするアプリケーションが必要です。 以上。

使用して以前のバージョンの Android で可能なアプリ リンクは、 [Rivets NuGet パッケージ](https://www.nuget.org/packages/Rivets/)Xamarin コンポーネント ストアから。 リベット パッケージに互換性がないと、Android 6.0; でのアプリ リンクAndroid 6.0 アプリをリンクすることはできません。

## <a name="configuring-app-linking-in-android-60"></a>Android 6.0 でのアプリ リンクの構成

Android 6.0 でのアプリ リンクの設定には、2 つの主要な手順が含まれます。

1. **Web サイトの URI の 1 つまたは複数インテント フィルターを追加する**&ndash;インテント フィルターは、モバイル ブラウザーで URL をクリックを処理する方法で Android をガイドします。
2. **発行、 *デジタル資産へのリンクの JSON* 、web サイト上のファイル** &ndash;は web サイトにアップロードされ、Android によってモバイル アプリと web サイトのドメイン間のリレーションシップを確認するために使用するファイルです。 これを行わない Android アプリをインストールできません既定ハンドルとしての URI の;ユーザーは手動で行ってください。

<a name="configure-intent-filter" />

### <a name="configuring-the-intent-filter"></a>インテント フィルターを構成します。

Web サイトから URI (または可能な Uri のセット) を Android アプリケーションのアクティビティにマップされるインテント フィルターを構成する必要があります。 アクティビティを装飾して Xamarin.Android では、この関係が確立されている、 [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)します。 インテント フィルターには、次の情報を宣言する必要があります。

* **`Intent.ActionView`** &ndash; これは情報を表示する要求に応答するインテント フィルターを登録します。
* **`Categories`** &ndash;  インテント フィルターは、両方を登録する必要があります **[Intent.CategoryBrowsable](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryBrowsable/)** と **[Intent.CategoryDefault](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryDefault/)** ができなければ正しくweb の URI を処理します。
* **`DataScheme`** &ndash; インテント フィルターを宣言する必要があります`http`や`https`します。 これらは、2 つだけ有効なスキームです。
* **`DataHost`** &ndash; これは、ドメインから発信されます。 Uri です。
* **`DataPathPrefix`** &ndash; これは、web サイト上のリソースへのオプションのパスです。
* **`AutoVerify`** &ndash; `autoVerify`属性が Android、web サイトとアプリケーション間のリレーションシップを確認するように指示します。 これは、以下の詳しく説明されます。

次の例は、使用する方法を示します、 [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)からのリンクを処理するために`https://www.recipe-app.com/recipes`との間`http://www.recipe-app.com/recipes`:

```csharp
[IntentFilter(new [] { Intent.ActionView },
              Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
              DataScheme = "http",
              DataHost = "recipe-app.com",
              DataPathPrefix = "/recipe",
              AutoVerify=true)]
public class RecipeActivity : Activity
{
    // Code for the activity omitted
}
```

Android では、URI の既定のハンドラーとして、アプリケーションを登録する前に、デジタル資産ファイル、web サイトに対してインテント フィルターで識別されるすべてのホストを確認します。 すべてのインテント フィルターは、Android は、既定のハンドラーとしてアプリを確立できる前に検証を渡す必要があります。

### <a name="creating-the-digital-assets-link-file"></a>デジタル資産のリンク ファイルを作成します。

アプリ リンク android 6.0 では、Android が URI の既定のハンドラーとして、アプリケーションを設定する前に、web サイトとアプリケーション間の関連付けを確認することが必要です。 この検証は、アプリケーションが最初にインストールされている場合に発生します。 *デジタル資産へのリンク*ファイルは、関連する webdomain(s) によってホストされている JSON ファイルです。

> [!NOTE]
> `android:autoVerify`インテント フィルターで属性を設定する必要があります&ndash;そうしないと Android では、検証を実行しません。

ファイルは、場所にあるドメインの web サイトの管理者によって配置 **https://domain/.well-known/assetlinks.json**します。

デジタル資産ファイルには、関連付けを確認する Android のメタ データのために必要なが含まれています。 **Assetlinks.json**ファイルが次のキー/値ペア。

* `namespace` &ndash; Android アプリケーションの名前空間。
* `package_name` &ndash; (アプリケーション マニフェストで宣言)、Android アプリケーションのパッケージの名前。
* `sha256_cert_fingerprints` &ndash; 署名済みアプリケーションの SHA256 の指紋。 このガイドを参照してください[キーストアの MD5 または SHA1 署名の検索](~/android/deploy-test/signing/keystore-signature.md)アプリケーションの SHA1 フィンガー プリントを取得する方法の詳細について。

次のスニペットの例に示します**assetlinks.json** 1 つのアプリケーションを一覧表示します。

```json
[
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"com.example",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   }
]
```

さまざまなバージョンをサポートするために 1 つ以上の SHA256 フィンガー プリントを登録することができますか、アプリケーションのビルドします。 この次**assetlinks.json**ファイルは、複数のアプリケーションの登録の例。

```json
[
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"example.com.puppies.app",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   },
   {
      "relation":[
         "delegate_permission/common.handle_all_urls"
      ],
      "target":{
         "namespace":"android_app",
         "package_name":"example.com.monkeys.app",
         "sha256_cert_fingerprints":[
            "14:6D:E9:83:C5:73:06:50:D8:EE:B9:95:2F:34:FC:64:16:A0:83:42:E6:1D:BE:A8:8A:04:96:B2:3F:CF:44:E5"
         ]
      }
   }
]
```

[Google デジタル資産へのリンクの web サイト](https://developers.google.com/digital-asset-links/tools/generator)の作成とデジタル資産ファイルのテストに役立つ可能性のあるオンライン ツールを持っています。

### <a name="testing-app-links"></a>アプリへのリンクのテスト

アプリへのリンクを実装したら、期待どおりに機能するようにさまざまな部分をテストしてください。

デジタル資産のファイルが正しく書式設定し、この例で示すように、Google のデジタル資産のリンクの API を使用してホストされていることを確認できます。

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

これには、インテント フィルターが正しく構成されていると、アプリは、URI の既定のハンドラーとして設定されていることを確認して実行できる 2 つのテストがあります。

1.  デジタル資産ファイルは、前述のように正しくホストされます。 最初のテストでは、Android のモバイル アプリケーションにリダイレクトする必要がありますインテントをディスパッチします。 Android アプリケーションを起動し、URL の登録のアクティビティを表示する必要があります。 コマンド プロンプトで。

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2.  特定のデバイスにインストールされているアプリケーション用のポリシーを処理する既存のリンクを表示します。 次のコマンドは、次の情報とデバイスのユーザーごとにポリシーをリンクの一覧をダンプします。 コマンド プロンプトに次のコマンドを入力します。

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    * **`Package`** &ndash; アプリケーションのパッケージの名前。
    * **`Domain`** &ndash; Web リンクを持つアプリケーションによって処理されるドメイン (スペースで区切られた)
    * **`Status`** &ndash; これは、アプリの現在のリンク処理状態です。 値**常に**、アプリケーションがつまり`android:autoVerify=true`宣言され、システムの検証に合格します。 Android システムのレコードの優先順位を表す 16 進数が続いています。

    例えば:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>まとめ

このガイドでは、Android 6.0 でのアプリ リンクする方法のしくみについて説明します。 Android 6.0 をサポートし、アプリへのリンクに対応するアプリケーションを構成する方法を説明します。 Android アプリケーションでのアプリ リンクをテストする方法についても説明します。


## <a name="related-links"></a>関連リンク

- [キーストアの MD5 または SHA1 署名の検索](~/android/deploy-test/signing/keystore-signature.md)
- [アクティビティおよびインテント](https://university.xamarin.com/classes#4)
- [AppLinks](http://applinks.org/)
- [Google のデジタル資産へのリンク](https://developers.google.com/digital-asset-links/)
- [ステートメントの一覧のジェネレーターとテスト担当者](https://developers.google.com/digital-asset-links/tools/generator)
