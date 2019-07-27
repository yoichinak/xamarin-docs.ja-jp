---
title: Android でのアプリリンク
description: このガイドでは、モバイルアプリが web サイトの Url に応答できるようにするための手法である Android 6.0 でのアプリリンクのサポートについて説明します。 アプリリンクとは何か、Android 6.0 アプリケーションでアプリリンクを実装する方法、およびドメインのモバイルアプリにアクセス許可を付与するように web サイトを構成する方法について説明します。
ms.prod: xamarin
ms.assetid: 48174E39-19FD-43BC-B54C-9AF11D4B1F91
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 2256e52e1b2a468ecbed97d5c7ed2d0a05f6cc4e
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510744"
---
# <a name="app-linking-in-android"></a>Android でのアプリリンク

_このガイドでは、モバイルアプリが web サイトの Url に応答できるようにするための手法である Android 6.0 でのアプリリンクのサポートについて説明します。アプリリンクとは何か、Android 6.0 アプリケーションでアプリリンクを実装する方法、およびドメインのモバイルアプリにアクセス許可を付与するように web サイトを構成する方法について説明します。_

## <a name="app-linking-overview"></a>アプリリンクの概要

モバイルアプリケーションは、多くの場合、 &ndash;ビジネスの重要なコンポーネントであり、web サイトと共にサイロに存在しなくなりました。 ビジネスでは、web プレゼンスとモバイルアプリケーションをシームレスに接続し、モバイルアプリケーションを起動し、関連するコンテンツをモバイルアプリに表示する web サイトのリンクを使用することをお勧めします。 *アプリリンク*(*ディープリンク*とも呼ばれます) は、モバイルデバイスが uri に応答し、その uri に対応するモバイルアプリケーションを起動できるようにする手法の1つです。

Android では、ユーザーがモバイルブラウザーでリンクをクリックしたときに、*インテントシステム* &ndash;を使用してアプリリンクを処理します。モバイルブラウザーは、登録されたアプリケーションに対して android が委任するという意図をディスパッチします。 たとえば、料理の web サイトのリンクをクリックすると、その web サイトに関連付けられているモバイルアプリが開き、特定のレシピがユーザーに表示されます。 その目的を処理するために複数のアプリケーションが登録されている場合、Android ではあいまいさを解消する*ダイアログ*と呼ばれるものが生成されます。これは、意図を処理する必要があるアプリケーションを選択するアプリケーションをユーザーに要求します。次に例を示します。

![不明瞭なダイアログの例のスクリーンショット](app-linking-images/01-disambiguation-dialog.png)

Android 6.0 では、自動リンク処理が使用されています。 Android では、アプリケーションを URI &ndash;の既定のハンドラーとして自動的に登録することができます。これにより、アプリは自動的に起動され、関連するアクティビティに直接移動します。 Android 6.0 で URI クリックを処理する方法は、次の条件によって異なります。

1. **既存のアプリは既に URI に関連付けられています**&ndash;ユーザーは既に既存のアプリを URI に関連付けている可能性があります。 その場合、Android は引き続きそのアプリケーションを使用します。
2. **URI に関連付けられている既存のアプリはありませんが、サポートアプリがインストール**されています&ndash;このシナリオでは、ユーザーは既存のアプリを指定していないため、Android は、インストールされているサポートアプリケーションを使用して要求を処理します。
3. **URI に関連付けられている既存のアプリはありませんが、多くのサポートアプリがインストール**されています&ndash; Uri をサポートするアプリケーションが複数存在するため、あいまいさを解消するダイアログが表示され、ユーザーは uri を処理するアプリを選択する必要があります。

URI をサポートするアプリがインストールされておらず、その後にインストールされたアプリがある場合、Android は uri に関連付けられている web サイトとの関連付けを確認した後に、そのアプリケーションを URI の既定のハンドラーとして設定します。

このガイドでは、android 6.0 アプリケーションを構成する方法と、Android 6.0 でのアプリリンクをサポートするデジタル資産リンクファイルを作成して発行する方法について説明します。

## <a name="requirements"></a>必要条件

このガイドでは、Xamarin 6.1 と、Android 6.0 (API レベル 23) 以降を対象とするアプリケーションが必要です。

以前のバージョンの Android では、Xamarin コンポーネントストアからの[リベット NuGet パッケージ](https://www.nuget.org/packages/Rivets/)を使用してアプリリンクを行うことができます。 リベットパッケージは、Android 6.0 のアプリリンクと互換性がありません。Android 6.0 のアプリリンクはサポートされていません。

## <a name="configuring-app-linking-in-android-60"></a>Android 6.0 でのアプリリンクの構成

Android 6.0 でのアプリリンクの設定には、次の2つの主要な手順が含まれます。

1. **Web サイト URI の1つまたは複数のインテントフィルターを追加する**&ndash;インテントフィルターガイド「Android」では、モバイルブラウザーで URL クリックを処理する方法を説明しています。
2. **発行、 *デジタル資産へのリンクの JSON* 、web サイト上のファイル** &ndash;は web サイトにアップロードされ、Android によってモバイル アプリと web サイトのドメイン間のリレーションシップを確認するために使用するファイルです。 これを行わないと、Android は URI の既定のハンドルとしてアプリをインストールできません。ユーザーは手動で行う必要があります。

<a name="configure-intent-filter" />

### <a name="configuring-the-intent-filter"></a>インテントフィルターの構成

Web サイトの URI (または一連の Uri) を Android アプリケーションのアクティビティにマップするインテントフィルターを構成する必要があります。 Xamarin Android では、この関係は、 [Intentfilterattribute](xref:Android.App.IntentFilterAttribute)を使用してアクティビティを装飾することによって確立されます。 インテントフィルターでは、次の情報を宣言する必要があります。

* **`Intent.ActionView`** &ndash;これにより、情報を表示する要求に応答するインテントフィルターが登録されます
* **`Categories`** &ndash;  インテント フィルターは、両方を登録する必要があります **[Intent.CategoryBrowsable](xref:Android.Content.Intent.CategoryBrowsable)** と **[Intent.CategoryDefault](xref:Android.Content.Intent.CategoryDefault)** ができなければ正しくweb の URI を処理します。
* **`DataScheme`** インテントフィルターでは、 `http` /または`https`を宣言する必要があります。 &ndash; これらは、2つの有効なスキームだけです。
* **`DataHost`** &ndash;これは、uri の生成元となるドメインです。
* **`DataPathPrefix`** &ndash;これは、web サイト上のリソースへの省略可能なパスです。
* **`AutoVerify`** 属性は、アプリケーションと web サイト間の関係を確認するように Android に指示し`autoVerify`ます。 &ndash; 詳細については、以下で説明します。

次の例では、 [intentfilterattribute](xref:Android.App.IntentFilterAttribute)を使用して、と`https://www.recipe-app.com/recipes` `http://www.recipe-app.com/recipes`の間のリンクを処理する方法を示します。

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

Android では、URI の既定のハンドラーとしてアプリケーションを登録する前に、web サイトのデジタル資産ファイルに対してインテントフィルターによって識別されるすべてのホストが検証されます。 Android でアプリを既定のハンドラーとして設定するには、すべてのインテントフィルターが検証に合格する必要があります。

### <a name="creating-the-digital-assets-link-file"></a>デジタル資産リンクファイルを作成しています

Android 6.0 のアプリリンクを使用するには、アプリケーションを URI の既定のハンドラーとして設定する前に、Android でアプリケーションと web サイトの関連付けを確認する必要があります。 この確認は、アプリケーションが最初にインストールされたときに行われます。 *デジタル資産リンク*ファイルは、関連する webdomain によってホストされる JSON ファイルです。

> [!NOTE]
> 属性`android:autoVerify`はインテントフィルター &ndash;によって設定する必要があります。そうしないと、Android は検証を実行しません。

ファイルは、その場所 **https://domain/.well-known/assetlinks.json** にあるドメインの web マスターによって配置されます。

デジタル資産ファイルには、Android が関連付けを確認するために必要なメタデータが含まれています。 **Assetlinks**ファイルには、次のキーと値のペアがあります。

* `namespace`&ndash; Android アプリケーションの名前空間。
* `package_name`&ndash; (アプリケーションマニフェストで宣言されている) Android アプリケーションのパッケージ名。
* `sha256_cert_fingerprints`&ndash;署名されたアプリケーションの SHA256 指紋。 アプリケーションの SHA1 フィンガープリントを取得する方法の詳細については、「[キーストアの MD5 または Sha1 署名の検索](~/android/deploy-test/signing/keystore-signature.md)」ガイドを参照してください。

次のスニペットは、1つのアプリケーションが一覧表示された**assetlinks**の例です。

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

複数の SHA256 フィンガープリントを登録して、アプリケーションのさまざまなバージョンまたはビルドをサポートすることができます。 次の**assetlinks**ファイルは、複数のアプリケーションを登録する例です。

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

[Google Digital Asset Links web サイト](https://developers.google.com/digital-asset-links/tools/generator)には、デジタル資産ファイルの作成とテストに役立つオンラインツールがあります。

### <a name="testing-app-links"></a>アプリのテスト-リンク

アプリリンクを実装した後、さまざまな部分をテストして、想定どおりに動作することを確認する必要があります。

次の例に示すように、デジタルアセットファイルが正しく書式設定され、Google のデジタル資産リンク API を使用してホストされていることを確認できます。

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

インテントフィルターが適切に構成されていること、およびアプリが URI の既定のハンドラーとして設定されていることを確認するために、次の2つのテストを実行できます。

1.  デジタル資産ファイルは、前述のように適切にホストされています。 最初のテストでは、Android がモバイルアプリケーションにリダイレクトするインテントをディスパッチします。 Android アプリケーションが起動し、URL に登録されているアクティビティが表示されます。 コマンドプロンプトで、次のように入力します。

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2.  特定のデバイスにインストールされているアプリケーションの既存のリンク処理ポリシーを表示します。 次のコマンドを実行すると、デバイス上の各ユーザーのリンクポリシーの一覧が、次の情報と共にダンプされます。 コマンド プロンプトに次のコマンドを入力します。

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    * **`Package`** &ndash;アプリケーションのパッケージ名。
    * **`Domain`** &ndash;アプリケーションによって処理される web リンクを持つドメイン (スペース区切り)
    * **`Status`** &ndash;これは、アプリの現在のリンク処理状態です。 値は**常に**、アプリケーション`android:autoVerify=true`がを宣言し、システムの検証に合格したことを意味します。 その後に、設定の Android システムのレコードを表す16進数が続きます。

    例えば:

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>Summary

このガイドでは、Android 6.0 でのアプリリンクのしくみについて説明しました。 次に、アプリリンクをサポートし、応答するように Android 6.0 アプリケーションを構成する方法について説明します。 また、Android アプリケーションでアプリリンクをテストする方法についても説明しました。


## <a name="related-links"></a>関連リンク

- [キーストアの MD5 または SHA1 署名の検索](~/android/deploy-test/signing/keystore-signature.md)
- [アクティビティとインテント](https://university.xamarin.com/classes#4)
- [AppLinks](http://applinks.org/)
- [Google デジタル資産のリンク](https://developers.google.com/digital-asset-links/)
- [ステートメントリストジェネレーターとテスト担当者](https://developers.google.com/digital-asset-links/tools/generator)
