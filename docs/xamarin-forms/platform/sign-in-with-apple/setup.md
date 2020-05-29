---
title: "セットアップ手順-Apple でのサインイン Xamarin.Forms " の説明: "apple セットアップでのサインインは、モバイルアプリケーションが対象とするさまざまなプラットフォームによって異なります。"
ms. 製品: xamarin ms. assetid: 8f712802氏 395b47 69b5davidortinau c927ad1a8391 ミリ秒。テクノロジ: xamarin-forms author: ms. 09/10/2019 author:: [ Xamarin.Forms ,] のように指定します。 Xamarin.Essentials
---

# <a name="setup-sign-in-with-apple-for-xamarinforms"></a>Apple でのサインインのセットアップXamarin.Forms

このガイドでは、Apple で高度なサインインを行うためにクロスプラットフォームアプリケーションをセットアップするために必要な一連の手順について説明します。 Apple のセットアップは Apple Developer Portal で簡単に実行できますが、Android と Apple の間にセキュリティで保護された関係を作成するには追加の手順が必要です。 

## <a name="apple-developer-setup"></a>Apple developer セットアップ

アプリケーションで Apple とのサインインを使用するには、Apple の開発者ポータルの [[証明書] の [識別子 & プロファイル](https://developer.apple.com/account/resources/)] セクションにあるいくつかのセットアップ手順に対処する必要があります。

### <a name="apple-sign-in-domain"></a>Apple サインインドメイン

ドメイン名を登録し、Apple で確認します。[詳細につい](https://developer.apple.com/account/resources/services/list)ては、「*証明書、識別子 & プロファイル*」セクションを参照してください。

![その他のセクション](sign-in-images/readme-signin-domain-configure.png)

ドメインを追加し、[**登録**] をクリックします。

![ドメインの登録フォーム](sign-in-images/readme-signin-domain-more.png)

> [!NOTE]
> ドメインが SPF に準拠していないことに関するエラーが表示された場合は、次のように spf DNS TXT レコードをドメインに追加し、それが反映されるまで待機する必要があります。 SPF TXT は次のようになります。`v=spf1 a a:myapp.com -all`

次に、[**ダウンロード**] をクリックしてファイルを取得 `apple-developer-domain-association.txt` し、ドメインの web サイトのフォルダーにアップロードすることで、ドメインの所有権を確認する必要があり `.well-known` ます。

`.well-known/apple-developer-domain-association.txt`ファイルがアップロードされ、到達可能になったら、[**確認**] をクリックして、Apple がドメインの所有権を確認できるようにします。

> [!NOTE]
> Apple は、との所有権を確認し `https://` ます。 SSL がセットアップされていて、セキュリティで保護された URL でファイルにアクセスできることを確認します。

続行する前に、このプロセスを正常に完了してください。

## <a name="setup-your-app-id"></a>アプリ ID を設定する

[[識別子](https://developer.apple.com/account/resources/identifiers/list)] セクションで、新しい識別子を作成し、[**アプリ id**] を選択します。 既にアプリ ID を持っている場合は、代わりにそれを編集することを選択します。

![新しいアプリ ID を作成する](sign-in-images/readme-appid-create.png)

**Apple でのサインイン**を有効にします。 通常は、[**プライマリアプリ ID として有効に**する] オプションを使用します。

![Apple でのサインインを有効にする](sign-in-images/readme-appid-signin.png)

アプリ ID の変更を保存します。

## <a name="create-a-service-id"></a>サービス ID を作成する

[[識別子](https://developer.apple.com/account/resources/identifiers/list/serviceId)] セクションで、新しい識別子を作成し、[**サービス id**] を選択します。

![新しいサービス ID を作成します](sign-in-images/readme-serviceid-create.png)

サービス ID に説明と識別子を指定します。  この識別子は、になり `ServerId` ます。  必ず**Apple でのサインイン**を有効にしてください。

続行する前に、有効にした [ _Apple でサインイン_] オプションの横にある [**構成**] をクリックします。

[構成] パネルで、正しい**プライマリアプリ ID**が選択されていることを確認します。

次に、以前に構成した**Web ドメイン**を選択します。

最後に、1つまたは複数の**リターン url**を追加します。  後で使用する場合は、使用しているとおりに `redirect_uri` ここに登録する必要があります。  `http://`入力時には、URL にまたはが含まれていることを確認してください `https://` 。

> [!NOTE]
> テスト目的では、またはを使用することはできませんが、など `127.0.0.1` `localhost` の他のドメインを使用することもでき `local.test` ます。  これを選択した場合は、コンピューターのファイルを編集して、 `hosts` この架空のドメインをローカル IP アドレスに解決することができます。

![Apple サインインを構成する](sign-in-images/readme-serviceid-configure.png)

完了したら変更を保存します。

## <a name="create-a-key-for-your-services-id"></a>サービス ID のキーを作成する

[[キー](https://developer.apple.com/account/resources/authkeys/list) ] セクションで、新しい**キー**を作成します。

キーに名前を付け、 **Apple でのサインイン**を有効にします。

![新しいキーを作成する](sign-in-images/readme-key-create.png)

[ _Apple でサインイン_] の横にある [**構成**] をクリックします。

正しい**プライマリアプリ ID**が選択されていることを確認し、[**保存**] をクリックします。

[**続行**] をクリックし、[**登録**] をクリックして新しいキーを作成します。

次に、生成したキーをダウンロードする機会が1つだけあります。  **[Download]** をクリックします。

![キーのダウンロード](sign-in-images/readme-key-download.png)

また、この手順で**キー ID**をメモしておきます。 これは、後でに使用され `KeyId` ます。

キーファイルがダウンロードされ `.p8` ます。  このファイルをメモ帳で開くことも、VSCode を使用してテキストの内容を表示することもできます。  次のようになります。

```
-----BEGIN PRIVATE KEY-----
MIGTAgEAMBMGBasGSM49AgGFCCqGSM49AwEHBHkwdwIBAQQg3MX8n6VnQ2WzgEy0
Skoz9uOvatLMKTUIPyPCAejzzUCgCgYIKoZIzj0DAQehRANCAARZ0DoM6QPqpJxP
JKSlWz0AohFhYre10EXPkjrih4jTm+b0AeG2BGuoIWd18i8FimGDgK6IzHHPsEqj
DHF5Svq0
-----END PRIVATE KEY-----
```

このキー `P8FileContents` に名前を設定し、安全な場所に保管します。 このサービスをモバイルアプリケーションに統合するときに使用します。

## <a name="summary"></a>まとめ

この記事では、アプリケーションで使用するために Apple でサインインを設定するために必要な手順について説明しました Xamarin.Forms 。

## <a name="related-links"></a>関連リンク

- [Apple でのサインインに関するガイドライン](https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple/overview/)
  