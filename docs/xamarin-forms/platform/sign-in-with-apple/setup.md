---
title: セットアップ手順-Apple for Xamarin を使用してサインインする
description: Apple セットアップでのサインインは、モバイルアプリケーションが対象とするさまざまなプラットフォームによって異なります。
ms.prod: xamarin
ms.assetid: 8F712802-395B-469B-B5BE-C927AD1A8391
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 09/10/2019
ms.openlocfilehash: c46da6b4ed877cc85f98e6ef0ab2b9a28d811723
ms.sourcegitcommit: cf56d2bae34dc0f8e94c2d3d28d5f460d59807bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/13/2019
ms.locfileid: "70986124"
---
# <a name="setup-sign-in-with-apple-for-xamarinforms"></a>Xamarin 用の Apple でのサインインの設定

![この API は現在プレビューの段階です](~/media/shared/preview.png)

このガイドでは、Apple で高度なサインインを行うためにクロスプラットフォームアプリケーションをセットアップするために必要な一連の手順について説明します。 Apple のセットアップは Apple Developer Portal で簡単に実行できますが、Android と Apple の間にセキュリティで保護された関係を作成するには追加の手順が必要です。 

## <a name="apple-developer-setup"></a>Apple developer セットアップ

アプリケーションで Apple とのサインインを使用するには、Apple の開発者ポータルの [[証明書] の [識別子 & プロファイル](https://developer.apple.com/account/resources/)] セクションにあるいくつかのセットアップ手順に対処する必要があります。

### <a name="apple-sign-in-domain"></a>Apple サインインドメイン

ドメイン名を登録し、Apple で確認します。[詳細につい](https://developer.apple.com/account/resources/services/list)ては、「*証明書、識別子 & プロファイル*」セクションを参照してください。

![その他のセクション](sign-in-images/readme-signin-domain-configure.png)

ドメインを追加し、 **[登録]** をクリックします。

![ドメインの登録フォーム](sign-in-images/readme-signin-domain-more.png)

> [!NOTE]
> ドメインが SPF に準拠していないことに関するエラーが表示された場合は、次に進む前に、SPF DNS TXT レコードをドメインに追加し、それが反映されるまで待機する必要があります。SPF TXT は次のようになります。`v=spf1 a a:myapp.com -all`

次に、 **[ダウンロード]** をクリックして`apple-developer-domain-association.txt`ファイルを取得し、ドメインの web サイトの`.well-known`フォルダーにアップロードすることで、ドメインの所有権を確認する必要があります。

ファイルがアップロードされ、到達可能になったら、[確認] をクリックして、Apple がドメインの所有権を確認できるようにします。 `.well-known/apple-developer-domain-association.txt`

> [!NOTE]
> Apple は、との`https://`所有権を確認します。 SSL がセットアップされていて、セキュリティで保護された URL でファイルにアクセスできることを確認します。

続行する前に、このプロセスを正常に完了してください。

## <a name="setup-your-app-id"></a>アプリ ID を設定する

[[識別子](https://developer.apple.com/account/resources/identifiers/list)] セクションで、新しい識別子を作成し、 **[アプリ id]** を選択します。 既にアプリ ID を持っている場合は、代わりにそれを編集することを選択します。

![新しいアプリ ID を作成する](sign-in-images/readme-appid-create.png)

**Apple でのサインイン**を有効にします。 通常は、[**プライマリアプリ ID として有効に**する] オプションを使用します。

![Apple でのサインインを有効にする](sign-in-images/readme-appid-signin.png)

アプリ ID の変更を保存します。

## <a name="create-a-service-id"></a>サービス ID を作成する

[[識別子](https://developer.apple.com/account/resources/identifiers/list/serviceId)] セクションで、新しい識別子を作成し、 **[サービス id]** を選択します。

![新しいサービス ID を作成します](sign-in-images/readme-serviceid-create.png)

サービス ID に説明と識別子を指定します。  この識別子は、に`ServerId`なります。  必ず**Apple でのサインイン**を有効にしてください。

続行する前に、有効にした [ _Apple でサインイン_] オプションの横にある **[構成]** をクリックします。

[構成] パネルで、正しい**プライマリアプリ ID**が選択されていることを確認します。

次に、以前に構成した**Web ドメイン**を選択します。

最後に、1つまたは複数の**リターン url**を追加します。  `redirect_uri`後で使用する場合は、使用しているとおりにここに登録する必要があります。  入力時には、 `http://` URL `https://`にまたはが含まれていることを確認してください。

> [!NOTE]
> テスト目的では、または`127.0.0.1` `localhost`を使用することはできませんが、 `local.test`などの他のドメインを使用することもできます。  これを選択した場合は、コンピューターのファイルを編集`hosts`して、この架空のドメインをローカル IP アドレスに解決することができます。

![Apple サインインを構成する](sign-in-images/readme-serviceid-configure.png)

完了したら変更を保存します。

## <a name="create-a-key-for-your-services-id"></a>サービス ID のキーを作成する

[[キー](https://developer.apple.com/account/resources/authkeys/list) ] セクションで、新しい**キー**を作成します。

キーに名前を付け、 **Apple でのサインイン**を有効にします。

![新しいキーを作成する](sign-in-images/readme-key-create.png)

[ _Apple でサインイン_] の横にある **[構成]** をクリックします。

正しい**プライマリアプリ ID**が選択されていることを確認し、 **[保存]** をクリックします。

**[続行]** をクリックし、 **[登録]** をクリックして新しいキーを作成します。

次に、生成したキーをダウンロードする機会が1つだけあります。  **[ダウンロード]** をクリックします。

![キーのダウンロード](sign-in-images/readme-key-download.png)

また、この手順で**キー ID**をメモしておきます。 これは、 `KeyId`後でに使用されます。

`.p8`キーファイルがダウンロードされます。  このファイルをメモ帳で開くことも、VSCode を使用してテキストの内容を表示することもできます。  次のようになります。

```
-----BEGIN PRIVATE KEY-----
MIGTAgEAMBMGBasGSM49AgGFCCqGSM49AwEHBHkwdwIBAQQg3MX8n6VnQ2WzgEy0
Skoz9uOvatLMKTUIPyPCAejzzUCgCgYIKoZIzj0DAQehRANCAARZ0DoM6QPqpJxP
JKSlWz0AohFhYre10EXPkjrih4jTm+b0AeG2BGuoIWd18i8FimGDgK6IzHHPsEqj
DHF5Svq0
-----END PRIVATE KEY-----
```

このキー `P8FileContents`に名前を設定し、安全な場所に保管します。 このサービスをモバイルアプリケーションに統合するときに使用します。

## <a name="summary"></a>Summary

この記事では、Xamarin アプリケーションで使用するために Apple でサインインを設定するために必要な手順について説明しました。

## <a name="related-links"></a>関連リンク

- [Apple でのサインインに関するガイドライン](https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple/overview/)
  