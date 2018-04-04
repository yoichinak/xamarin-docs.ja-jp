---
title: Android でアプリ リンク
description: このガイドでは、Android 6.0 がアプリにリンクすると、モバイル アプリ web サイトの Url に応答できるようにする方法をサポートする方法について説明します。 どのようなアプリ リンクが、Android 6.0 アプリケーションは、アプリ リンクを実装する方法、およびドメイン用のモバイル アプリへのアクセス許可を付与する web サイトを構成する方法を説明します。
ms.prod: xamarin
ms.assetid: 48174E39-19FD-43BC-B54C-9AF11D4B1F91
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 2ef6b8044387d759e26d05c1468caaad7efb9bdc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="app-linking-in-android"></a>Android でアプリ リンク

_このガイドでは、Android 6.0 がアプリにリンクすると、モバイル アプリ web サイトの Url に応答できるようにする方法をサポートする方法について説明します。どのようなアプリ リンクが、Android 6.0 アプリケーションは、アプリ リンクを実装する方法、およびドメイン用のモバイル アプリへのアクセス許可を付与する web サイトを構成する方法を説明します。_

## <a name="app-linking-overview"></a>アプリのリンクの概要

モバイル アプリケーションが不要になったサイロに live&ndash;多くの場合は、web サイトと共に、ビジネスの重要なコンポーネントです。 モバイル アプリケーションを起動して、モバイル アプリに関連するコンテンツを表示するには、web サイト上のリンクと、その web サイトとモバイル アプリケーションをシームレスに接続すると、企業のことをお勧めします。 *アプリ リンク*(とも呼びます*ディープ リンク*) は、URI に応答し、その URI に対応するモバイル アプリケーションを起動する、モバイル デバイスを許可する方法の 1 つです。

Android 処理を通じてアプリ リンク、*インテント システム*&ndash;モバイル ブラウザーでアプリを登録済みのアプリケーションに委任する目的がディスパッチ モバイル ブラウザーでリンクをクリックすると、ユーザー、します。 たとえば、料理の web サイト上のリンクをクリックするとはその web サイトに関連付けられているモバイル アプリを開き特定のレシピをユーザーに表示します。 その目的を処理する複数のアプリケーションが登録されているし、Android を発生させると呼ばれるものがある場合、*あいまいさを排除ダイアログ*で処理する目的、アプリケーションを選択するには、どのようなアプリケーションからユーザーに要請します。例:

![あいまいさを排除 ダイアログ ボックスの例のスクリーン ショット](app-linking-images/01-disambiguation-dialog.png)

Android 6.0 は、自動リンクの処理を使用してこのが向上します。 Android 用に自動的にアプリケーションを登録する既定のハンドラーとして URI に可能であれば&ndash;アプリは自動的に起動して、関連するアクティビティに直接移動します。 Android 6.0、URI のクリックを処理することを決定は、次の条件によって異なります。

1. **既存のアプリは、URI に関連付けは既に**&ndash;ユーザーが既に関連付けられている既存のアプリ、URI を使用します。 その場合は、Android は、そのアプリケーションを使用し続けます。
2. **URI に関連付けられた既存のアプリはありませんが、サポートするアプリがインストールされている**&ndash;このシナリオでは、ユーザーが指定されていない、既存のアプリでは、Android がインストールされているサポートしているアプリケーションを使用して要求を処理するようにします。
3. **URI に関連付けられた既存のアプリはありませんが、多くの関連アプリケーションがインストールされている** &ndash; URI をサポートする複数のアプリケーションがあるため、あいまいさを排除ダイアログが表示されます、どのアプリがユーザーを選択する必要がありますURI を処理します。

ユーザーには、URI をサポートするアプリがインストールされておらず、1 つが、後でインストールされている場合、Android がそのアプリケーション設定既定のハンドラーとして URI の URI に関連付けられている web サイトとの関連付けを確認した後します。

このガイドは、作成し、Android 6.0 でアプリ リンクをサポートするためのデジタル資産へのリンク ファイルを発行する方法と、Android 6.0 アプリケーションを構成する方法について説明します。

## <a name="requirements"></a>要件

このガイドは、Xamarin.Android 6.1 と Android 6.0 (API level 23) を対象とするアプリケーションが必要です。 またはそれ以降。

使用して以前のバージョンの Android アプリ リンクは、 [Rivets NuGet パッケージ](https://www.nuget.org/packages/Rivets/)Xamarin コンポーネント ストアからです。 リベット パッケージ互換性がありません; Android 6.0 でアプリ リンクAndroid 6.0 アプリをリンクすることはできません。

## <a name="configuring-app-linking-in-android-60"></a>Android 6.0 でのアプリのリンクの構成

Android 6.0 でのアプリのリンクを設定するには、2 つの主要な手順が含まれます。

1. **フィルターを追加する 1 つまたは複数の目的の web サイトの URI の**&ndash;インテントのフィルターは、モバイル ブラウザーで URL をクリックを処理する方法で Android をガイドします。
2. **発行、*デジタル資産へのリンクの JSON* 、web サイト上のファイル** &ndash; web サイトにアップロードされ、Android によって、モバイル アプリと web サイトのドメイン間のリレーションシップを確認するために使用されるファイルです。 これを行わない場合 Android ことはできません、アプリのインストール既定ハンドルとしての URI のです。ユーザー手動で行ってください。

<a name="configure-intent-filter" />

### <a name="configuring-the-intent-filter"></a>目的のフィルターの構成

Android アプリケーションのアクティビティへの web サイトの URI (または可能な一連の Uri) をマップするインテント フィルターを構成する必要があります。 Xamarin.Android、装飾を含むアクティビティでこのリレーションシップを確立、 [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)です。 目的のフィルターは、次の情報を宣言する必要があります。

* **`Intent.ActionView`** &ndash; これを情報を表示する要求に応答するインテント フィルターが登録されます。
* **`Categories`** &ndash;  目的のフィルターは、両方を登録する必要があります**[Intent.CategoryBrowsable](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryBrowsable/)**と**[Intent.CategoryDefault](https://developer.xamarin.com/api/field/Android.Content.Intent.CategoryDefault/)**できる正常web の URI を処理します。
* **`DataScheme`** &ndash; 目的のフィルターを宣言する必要があります`http`や`https`です。 これらは、2 つの有効なパターンです。
* **`DataHost`** &ndash; これは、Uri は元のドメインです。
* **`DataPathPrefix`** &ndash; これは、web サイト上のリソースへの省略可能なパスです。
* **`AutoVerify`** &ndash; `autoVerify`属性は、web サイトとアプリケーション間のリレーションシップを確認する Android を示します。 これについては説明より下です。

次の例を使用する方法を示しています、 [IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)からのリンクを処理する`https://www.recipe-app.com/recipes`から`http://www.recipe-app.com/recipes`:

```csharp
[IntentFilter(new [] { Intent.ActionView },
              Categories = new[] { Intent.CategoryBrowsable, Intent.CategoryDefault },
              DataScheme = "http",
              DataHost = "recipe-app.com",
              DataPathPrefix = "/recipe"),
              AutoVerify=true]
public class RecipeActivity : Activity
{
    // Code for the activity omitted
}
```

Android では、URI の既定のハンドラーとしてアプリケーションを登録する前に、web サイト上のデジタル資産ファイルに対してインテント フィルターによって識別されるすべてのホストを確認します。 インテントのすべてのフィルターは、Android は、既定のハンドラーとしてアプリを確立する前に検証を通過する必要があります。

### <a name="creating-the-digital-assets-link-file"></a>デジタル アセットにリンク ファイルを作成します。

アプリ リンク android 6.0 では、Android が URI の既定のハンドラーとしてアプリケーションを設定する前に、web サイトとアプリケーション間の関連付けを確認する必要があります。 この検証は、アプリケーションが最初にインストールされたときに発生します。 *デジタル資産へのリンク*ファイルは、関連する webdomain(s) によってホストされている JSON ファイルです。

> [!NOTE]
> `android:autoVerify`インテント フィルターで属性を設定する必要があります&ndash;それ以外の場合 Android では、検証を実行しません。

場所にあるドメインの web サイトの管理者により、ファイルが配置された **https://domain/.well-known/assetlinks.json**です。

デジタル アセット ファイルには、Android 用の関連付けの検証に、メタデータの必要なが含まれています。 **Assetlinks.json**ファイルに次のキー/値ペア。

* `namespace` &ndash; Android アプリケーションの名前空間。
* `package_name` &ndash; Android アプリケーション (アプリケーション マニフェストで宣言) のパッケージ名。
* `sha256_cert_fingerprints` &ndash; 署名済みアプリケーションの SHA256 指紋です。 番組ガイドを参照してください[キーストアの MD5 または SHA1 署名を検索する](~/android/deploy-test/signing/keystore-signature.md)アプリケーションの SHA1 フィンガー プリントを取得する方法の詳細。

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

異なるバージョンをサポートするために 2 つ以上の SHA256 フィンガー プリントを登録することが、アプリケーションのビルドまたはことできます。 この次**assetlinks.json**ファイルは、複数のアプリケーションを登録する次の例。

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

アプリへのリンクを実装した後に、期待どおりに動作することを確認するさまざまな部分をテストしてください。

デジタル資産ファイルが正しくフォーマットされており、この例のように、Google のデジタル資産へのリンク、API を使用してホストされていることを確認できます。

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

これには、目的のフィルターが正しく構成されていると、アプリは、URI の既定のハンドラーとして設定されていることを確認する実行可能な 2 つのテストがあります。

1.  デジタル アセット ファイルは、上記のように正しくホストされます。 最初のテストには、Android モバイル アプリケーションにリダイレクトする目的がディスパッチされます。 Android アプリケーションを起動し、URL の登録アクティビティを表示する必要があります。 コマンド プロンプトで入力します。

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2.  特定のデバイスにインストールされているアプリケーションのポリシーを処理する既存のリンクを表示します。 次のコマンドは、次の情報を持つデバイス上の各ユーザー リンク ポリシーの一覧をダンプします。 コマンド プロンプトに次のコマンドを入力します。

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    * **`Package`** &ndash; アプリケーションのパッケージ名。
    * **`Domain`** &ndash; Web リンクを持つ、アプリケーションによって処理されます (スペースで区切って) ドメイン
    * **`Status`** &ndash; これは、アプリの現在のリンク処理状態です。 値**常に**アプリケーションを持つことを意味`android:autoVerify=true`宣言され、システムの検証に合格します。 優先順位の Android のシステムのレコードを表す 16 進数が続くことはできます。

    例えば:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>まとめ

このガイドでは、Android 6.0 でどのアプリ リンクの動作について説明します。 Android 6.0 アプリケーションをサポートし、アプリへのリンクに応答を構成する方法を説明します。 Android アプリケーションでアプリ リンクをテストする方法についても説明します。


## <a name="related-links"></a>関連リンク

- [キーストアの MD5 または SHA1 署名の検索](~/android/deploy-test/signing/keystore-signature.md)
- [アクティビティと目的](https://university.xamarin.com/classes#4)
- [AppLinks](http://applinks.org/)
- [Google のデジタル資産へのリンク](https://developers.google.com/digital-asset-links/)
- [ステートメントの一覧のジェネレーターおよびテスト担当者](https://developers.google.com/digital-asset-links/tools/generator)
