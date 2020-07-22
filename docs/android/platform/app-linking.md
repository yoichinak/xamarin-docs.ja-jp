---
title: Android のアプリリンク
description: このガイドでは、Android 6.0 でのアプリリンクのサポートについて説明します。アプリリンクは、モバイル アプリが Web サイト上の URL に応答できるようにするための手法です。 ここでは、アプリリンクの概要、Android 6.0 アプリケーションでアプリリンクを実装する方法、モバイル アプリにドメインに対するアクセス許可を付与するように Web サイトを構成する方法について説明します。
ms.prod: xamarin
ms.assetid: 48174E39-19FD-43BC-B54C-9AF11D4B1F91
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: bdcefd6a1b0192dc337afd5b5a5535a20eeaef9e
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571390"
---
# <a name="app-linking-in-android"></a>Android のアプリリンク

_このガイドでは、Android 6.0 でのアプリリンクのサポートについて説明します。アプリリンクは、モバイル アプリが Web サイト上の URL に応答できるようにするための手法です。ここでは、アプリリンクの概要、Android 6.0 アプリケーションでアプリリンクを実装する方法、モバイル アプリにドメインに対するアクセス許可を付与するように Web サイトを構成する方法について説明します。_

## <a name="app-linking-overview"></a>アプリリンクの概要

モバイル アプリケーションは、サイロ&ndash;に存在するものではなくなりました。多くの場合、Web サイトと共に、ビジネスの重要なコンポーネントとなっています。 ビジネスでは、モバイル アプリケーションを起動してモバイル アプリに関連コンテンツを表示する、Web サイト上のリンクによって、Web プレゼンスとモバイル アプリケーションをシームレスに接続することが望まれます。 "*アプリリンク*" ("*ディープリンク*" とも呼ばれます) は、モバイル デバイスが URI に応答し、その URI に対応するモバイル アプリケーションを起動できるようにする 1 つの手法です。

Android では、"*インテント システム*"&ndash; によってアプリリンクを処理します。ユーザーがモバイル ブラウザーでリンクをクリックすると、モバイルブラウザーによって、Android が登録済みのアプリケーションに委任するインテントがディスパッチされます。 たとえば、料理の Web サイトでリンクをクリックすると、その Web サイトに関連付けられているモバイル アプリが開き、特定のレシピがユーザーに表示されます。 そのインテントを処理するために複数のアプリケーションが登録されている場合は、インテントを処理するアプリケーションを選択するようユーザーに求める、"*あいまいさ回避ダイアログ*" と呼ばれるものが表示されます。次に例を示します。

![あいまいさ回避ダイアログの例を示すスクリーンショット](app-linking-images/01-disambiguation-dialog.png)

Android 6.0 では、自動リンク処理を使用してこれを改善しています。 Android では、アプリケーションを URI &ndash; の既定のハンドラーとして自動的に登録できます。そのアプリが自動的に起動し、関連するアクティビティに直接移動します。 Android 6.0 で URI のクリックを処理する方法は、次の条件によって決まります。

1. **既存のアプリが URI に既に関連付けられている** &ndash; ユーザーが既存のアプリを URI に既に関連付けている場合があります。 その場合、そのアプリケーションが引き続き使用されます。
2. **URI に既存のアプリは関連付けられていないが、サポート アプリがインストールされている** &ndash; このシナリオでは、ユーザーが既存のアプリを指定していないため、インストールされているサポート アプリケーションを使用して要求が処理されます。
3. **URI に既存のアプリは関連付けられていないが、複数のサポート アプリがインストールされている** &ndash; URI をサポートするアプリケーションが複数あるため、あいまいさ回避ダイアログが表示され、URI を処理するアプリをユーザーが選択する必要があります。

ユーザーが URI をサポートするアプリをインストールしておらず、その後アプリがインストールされた場合は、URI に関連付けられている Web サイトとの関連付けが検証された後に、そのアプリケーションが URI の既定のハンドラーとして設定されます。

このガイドでは、Android 6.0 アプリケーションを構成する方法と、Android 6.0 でアプリリンクをサポートするためのデジタル アセット リンク ファイルを作成および公開する方法について説明します。

## <a name="requirements"></a>必要条件

このガイドでは、Xamarin.Android 6.1 と、Android 6.0 (API レベル 23) 以上を対象とするアプリケーションが必要です。

Xamarin コンポーネント ストアの [Rivets NuGet パッケージ](https://www.nuget.org/packages/Rivets/)を使用すると、以前のバージョンの Android でアプリリンクを使用できます。 Rivets パッケージは、Android 6.0 のアプリリンクと互換性がありません。Android 6.0 のアプリリンクはサポートされていません。

## <a name="configuring-app-linking-in-android-60"></a>Android 6.0 でのアプリリンクの構成

Android 6.0 でアプリリンクを設定するには、2 つの主な手順が必要です。

1. **Web サイトの URI の 1 つ以上のインテント フィルターを追加する** &ndash; インテント フィルターは、モバイル ブラウザーでの URL のクリックを処理する方法を Android に指示します。
2. **"*デジタル アセット リンク JSON*" ファイルを Web サイトで公開する** &ndash; これは、Web サイトにアップロードされ、モバイル アプリと Web サイトのドメインの関係を検証するために Android で使用されるファイルです。 これがないと、Android は URI の既定のハンドルとしてアプリをインストールできません。その場合、ユーザーが手動で行う必要があります。

<a name="configure-intent-filter"></a>

### <a name="configuring-the-intent-filter"></a>インテント フィルターの構成

URI (または使用可能な一連の URI) を Web サイトから Android アプリケーションのアクティビティにマップするインテント フィルターを構成する必要があります。 Xamarin.Android では、[IntentFilterAttribute](xref:Android.App.IntentFilterAttribute) でアクティビティを装飾することによってこの関係が確立されます。 インテント フィルターでは、次の情報を宣言する必要があります。

- **`Intent.ActionView`** &ndash; 情報を表示する要求に応答するインテント フィルターを登録します。
- **`Categories`** &ndash; インテント フィルターでは、Web URI を適切に処理できるように、 **[Intent.CategoryBrowsable](xref:Android.Content.Intent.CategoryBrowsable)** と **[Intent.CategoryDefault](xref:Android.Content.Intent.CategoryDefault)** の両方を登録する必要があります。
- **`DataScheme`** &ndash; インテント フィルターでは、`http` または `https` (あるいはその両方) を宣言する必要があります。 有効なスキームはこの 2 つだけです。
- **`DataHost`** &ndash; URI の取得元となるドメインです。
- **`DataPathPrefix`** &ndash; Web サイト上のリソースの省略可能なパスです。
- **`AutoVerify`** &ndash; `autoVerify` 属性は、アプリケーションと Web サイトの関係を検証するよう Android に指示します。 これについては、後ほど詳しく説明します。

次の例は、[IntentFilterAttribute](xref:Android.App.IntentFilterAttribute) を使用して、`https://www.recipe-app.com/recipes` および `http://www.recipe-app.com/recipes` からのリンクを処理する方法を示しています。

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

Android では、アプリケーションを URI の既定のハンドラーとして登録する前に、Web サイト上のデジタル アセット ファイルに対して、インテント フィルターで識別されるすべてのホストを検証します。 Android がアプリを既定のハンドラーとして設定するには、すべてのインテント フィルターが検証を通過している必要があります。

### <a name="creating-the-digital-assets-link-file"></a>デジタル アセット リンク ファイルの作成

Android 6.0 のアプリリンクでは、Android はアプリケーションを URI の既定のハンドラーとして設定する前に、アプリケーションと Web サイトの関連付けを検証する必要があります。 この検証は、アプリケーションが最初にインストールされたときに実行されます。 "*デジタル アセット リンク*" ファイルは、関連する Web ドメインでホストされる JSON ファイルです。

> [!NOTE]
> インテント フィルター&ndash;で `android:autoVerify` 属性を設定する必要があります。これを行わないと、検証は実行されません。

このファイルは、ドメインの Web マスターによって **https://domain/.well-known/assetlinks.json** に配置されます。

デジタル アセット ファイルには、Android が関連付けを検証するために必要なメタデータが含まれています。 **assetlinks.json** ファイルには、次のキーと値のペアが含まれています。

- `namespace` &ndash; Android アプリケーションの名前空間。
- `package_name` &ndash; Android アプリケーションのパッケージ名 (アプリケーション マニフェストで宣言)。
- `sha256_cert_fingerprints` &ndash; 署名済みアプリケーションの SHA256 フィンガープリント。 アプリケーションの SHA1 フィンガープリントを取得する方法の詳細については、[キーストアの MD5 または SHA1 署名の検索](~/android/deploy-test/signing/keystore-signature.md)に関するガイドをご覧ください。

次のスニペットは、アプリケーションが 1 つだけ記載された **assetlinks.json** の例を示しています。

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

アプリケーションのさまざまなバージョンやビルドをサポートするために、複数の SHA256 フィンガープリントを登録できます。 次の **assetlinks.json** ファイルは、複数のアプリケーションの登録例を示しています。

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

[Google Digital Asset Links Web サイト](https://developers.google.com/digital-asset-links/tools/generator)には、デジタル アセット ファイルの作成とテストを支援するオンライン ツールがあります。

### <a name="testing-app-links"></a>アプリリンクのテスト

アプリリンクを実装したら、さまざまな部分をテストして、想定どおりに機能することを確認する必要があります。

次の例に示すように、Google の Digital Asset Links API を使用して、デジタル アセット ファイルが適切に書式設定され、ホストされていることを確認できます。

```html
https://digitalassetlinks.googleapis.com/v1/statements:list?source.web.site=
  https://<WEB SITE ADDRESS>:&relation=delegate_permission/common.handle_all_urls
```

インテント フィルターが適切に構成されており、アプリが URI の既定のハンドラーとして設定されていることを確認するために実行できる 2 つのテストがあります。

1. 前述のように、デジタル アセット ファイルは適切にホストされています。 最初のテストでは、Android がモバイル アプリケーションにリダイレクトするインテントをディスパッチします。 Android アプリケーションが起動し、URL に登録されているアクティビティが表示されます。 コマンド プロンプトで次のように入力します。

    ```shell
    $ adb shell am start -a android.intent.action.VIEW \
        -c android.intent.category.BROWSABLE \
        -d "http://<domain1>/recipe/scalloped-potato"
    ```

2. 特定のデバイスにインストールされているアプリケーションの既存のリンク処理ポリシーを表示します。 次のコマンドでは、デバイス上の各ユーザーのリンク ポリシーのリストを次の情報と共にダンプします。 コマンド プロンプトに次のコマンドを入力します。

    ```shell
    $ adb shell dumpsys package domain-preferred-apps
    ```

    - **`Package`** &ndash; アプリケーションのパッケージ名。
    - **`Domain`** &ndash; Web リンクがアプリケーションによって処理されるドメイン (スペースで区切られています)。
    - **`Status`** &ndash; アプリの現在のリンク処理の状態。 値 **always** は、アプリケーションで `android:autoVerify=true` が宣言されており、システムの検証を通過したことを意味します。 その後に、Android システムのプリファレンスのレコードを表す 16 進数が続きます。

    次に例を示します。

    ```shell
    $ adb shell dumpsys package domain-preferred-apps

    App linkages for user 0:
    Package: com.android.vending
    Domains: play.google.com market.android.com
    Status: always : 200000002
    ```

## <a name="summary"></a>まとめ

このガイドでは、Android 6.0 でのアプリリンクのしくみについて説明しました。 次に、アプリリンクをサポートし、アプリリンクに応答するように Android 6.0 アプリケーションを構成する方法について説明しました。 また、Android アプリケーションでアプリリンクをテストする方法についても説明しました。

## <a name="related-links"></a>関連リンク

- [キーストアの MD5 または SHA1 署名の検索](~/android/deploy-test/signing/keystore-signature.md)
- [AppLinks (アプリリンク)](https://developers.facebook.com/docs/applinks)
- [Google Digital Assets Links](https://developers.google.com/digital-asset-links/)
- [Statement List Generator and Tester](https://developers.google.com/digital-asset-links/tools/generator)
