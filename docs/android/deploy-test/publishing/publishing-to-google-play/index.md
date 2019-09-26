---
title: GooglePlay に公開する
ms.prod: xamarin
ms.assetid: FB1CC234-3554-8566-48BD-2B9B3A28CC7F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 656b74bce10d30ddd463486c5103d65c6ba5eb97
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "71250020"
---
# <a name="publishing-to-google-play"></a>GooglePlay に公開する

アプリケーション配布用のアプリ マーケットは数多くありますが、Google Play はほぼ間違いなく Android アプリ用のストアとして世界最大であり最も多くの訪問者があります。 Google Play は、Android アプリケーションの配布、広告、販売、および売上分析のための単一のプラットフォームを提供します。

このセクションでは、パブリッシャーになるための登録、Google Play でのアプリケーションのプロモーションと広告に役立つアセットの収集、Google Play でのアプリケーションのレーティングのガイドライン、フィルターを使った特定のデバイスへのアプリケーションの展開の制限など、Google Play に固有のトピックについて説明します。

## <a name="requirements"></a>必要条件

Google Play でアプリケーションを配布するには、開発者アカウントを作成する必要があります。 これは一度だけ行えばよく、25 米ドルの手数料が 1 回だけかかります。

すべてのアプリケーションは、2033 年 10 月 22 日より後に期限切れになる暗号化キーで署名する必要があります。

Google Play で公開される APK の最大サイズは 100 MB です。 アプリケーションがこのサイズを超える場合は、"*APK 拡張ファイル*" で追加アセットを配布できます。 Android 拡張ファイルでは、APK は 2 つの追加ファイルを持つことができ、それぞれ最大サイズは 2 GB です。 Google Play はこれらのファイルをコストなしでホストおよび配布します。 拡張ファイルについては別のセクションで説明します。

Google Play は世界中どこでも利用できるわけではありません。 場所によっては、アプリケーションの配布がサポートされていないことがあります。

## <a name="becoming-a-publisher"></a>パブリッシャーになる

Google Play でアプリケーションを公開するには、パブリッシャーのアカウントが必要です。 パブリッシャー アカウントのサインアップは次の手順で行います。

1. [Google Play Developer Console](https://play.google.com/apps/publish) にアクセスします。
1. 開発者の身元に関する基本的な情報を入力します。
1. ロケールの開発者配布契約を読んで同意します。
1. 登録料 25 米ドルを払います。
1. 電子メールによる検証を確認します。
1. アカウントが作成された後は、Google Play を使ってアプリケーションを公開できます。

Google Play では、世界中のすべての国はサポートされません。 サポートされている国の最新の一覧は、次のリンクをご覧ください。

1. [Supported Locations for Developer &amp; Merchant Registration](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=150324) (開発者と販売者の登録をサポートされている場所) &ndash; 開発者が販売者として登録して有料アプリケーションを販売できる国の一覧です。

1. [Supported Locations for distribution to Google Play users](https://support.google.com/googleplay/android-developer/bin/answer.py?hl=en&amp;answer=138294) (Google Play ユーザーへの配布をサポートされている場所) &ndash; アプリケーションを配布できる国の一覧です。

### <a name="preparing-promotional-assets"></a>プロモーション アセットを準備する

Google Play でアプリケーションのプロモーションと宣伝を効果的に行うことができるように、Google では開発者がスクリーンショット、グラフィック、ビデオなどのプロモーション アセットを送信することが許可されています。 Google Play はそれらのアセットを使用して、アプリケーションの宣伝とプロモーションを行います。

#### <a name="launcher-icons"></a>ランチャー アイコン

"*ランチャー アイコン*" は、アプリケーションを表すグラフィックです。 各ランチャー アイコンは、透明化のためのアルファ チャネルを持つ 32 ビット PNG でなければなりません。 アプリケーションでは、以下の一覧で示すすべての一般的な画面密度用のアイコンを用意する必要があります。

- **ldpi** (120 dpi) &ndash; 36 x 36 ピクセル
- **mdpi** (160 dpi) &ndash; 48 x 48 ピクセル
- **hdpi** (240 dpi) &ndash; 72 x 72 ピクセル
- **xhdpi** (320 dpi) &ndash; 96 x 96 ピクセル

ランチャー アイコンはユーザーが Google Play でアプリケーションについて最初に目にするものなので、視覚的に訴える、意味のあるものにする必要があります。

ランチャー アイコンのヒント:

1. **単純ですっきりさせる** &ndash; ランチャー アイコンは単純ですっきりしたものにする必要があります。 これは、アイコンからアプリケーションの名前を除外することを意味します。 アイコンは単純なほど記憶に残りやすく、小さいサイズでも簡単に見分けることができます。

1. **アイコンを薄い色にしない** &ndash; 過度に薄いアイコンでは、すべての背景で適切に目立たせることができません。

1. **アルファ チャネルを使う** &ndash; アイコンではアルファ チャネルを使い、フル フレーム画像にしないようにする必要があります。

#### <a name="high-resolution-application-icons"></a>高解像度アプリケーション アイコン

Google Play のアプリケーションでは、アプリケーション アイコンの高品質バージョンが必要です。 これは Google Play によってのみ使われ、アプリケーション ランチャー アイコンの代わりにはなりません。 高解像度アイコンの仕様は次のとおりです。

1. アルファ チャネルを持つ 32 ビット PNG
1. 512 x 512 ピクセル
1. 最大サイズ 1024 KB

[Android Asset Studio](https://romannurik.github.io/AndroidAssetStudio/) は、適切なランチャー アイコンと高解像度のアプリケーション アイコンを作るのに便利なツールです。

#### <a name="screen-shots"></a>スクリーンショット

Google play では、2 個以上 8 個以下のアプリケーションのスクリーンショットが必要です。 これらは、Google Play のアプリケーションの詳細ページに表示されます。

スクリーンショットの仕様は次のとおりです。

1. 24 ビットの PNG または JPG、アルファ チャネルなし
1. 320w x 480h または 480w x 800h または 480w x 854h。 横長の画像はトリミングされます。

#### <a name="promotional-graphic"></a>プロモーション グラフィック

これは、Google Play で使われるオプションの画像です。

1. 180w x 120h、24 ビットの PNG または JPG、アルファ チャネルなし
1. アートに枠線なし

#### <a name="feature-graphic"></a>機能のグラフィック

Google Play のおすすめセクションで使われます。 このグラフィックは、アプリケーション アイコンを含まず単独で表示される場合があります。

1. 1024w x 500h の PNG または JPG、アルファ チャネルなし、透明度なし。
1. すべての重要なコンテンツは 924 x 500 のフレーム内に収める必要があります。 このフレームの外側のピクセルは、スタイルのためにトリミングされる可能性があります。
1. このグラフィックは縮小される可能性があります。大きなテキストを使い、単純なグラフィックスにします。

#### <a name="video-link"></a>ビデオのリンク

これは、アプリケーションを紹介する YouTube ビデオへの URL です。 ビデオは、30 秒から 2 分の長さで、アプリケーションの最適な部分を紹介する必要があります。

### <a name="publishing-to-google-play"></a>GooglePlay に公開する

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Xamarin Android 7.0 では、Visual Studio から Google Play にアプリを公開するためのワークフローが統合されました。 Xamarin Android 7.0 より前のバージョンを使っている場合は、Google Play Developer Console を使って APK を手動でアップロードする必要があります。 また、統合されたワークフローを使うには、その前に少なくとも 1 つの APK をアップロードしておく必要があります。 最初の APK をまだアップロードしていない場合は、手動でアップロードする必要があります。 詳細については、「[APK を手動でアップロードする](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md)」をご覧ください。

「[新しい証明書の作成](~/android/deploy-test/signing/index.md#newcert)」では、Android アプリに署名するための新しい証明書の作成方法について説明しました。 次の手順では、Google Play に署名済みアプリを公開します。

1. Google Play 開発者アカウントにサインインし、Google Play 開発者アカウントにリンクされた新しいプロジェクトを作成します。
2. アプリを認証する "**OAuth クライアント**" を作成します。
3. 結果のクライアント ID とクライアント シークレットを Visual Studio に入力します。
4. アカウントを Visual Studio に登録します。
5. 証明書でアプリに署名します。
6. 署名されたアプリを Google Play に公開します。

[公開のためのアーカイブ](~/android/deploy-test/release-prep/index.md#archive)では、 **[配布チャネル]** ダイアログに 2 種類の配布方法が表示されていました。 **[Ad Hoc]\(アドホック\)** と **[Google Play]** です。 **[署名 ID]** ダイアログが代わりに表示される場合は、 **[戻る]** をクリックして **[配布チャネル]** ダイアログに戻ります。 **[Google Play]** を選択して **[Next]\(次へ\)** をクリックします。

[![[配布チャネル] ダイアログ](images/vs/01-distribution-channel-sml.png)](images/vs/01-distribution-channel.png#lightbox)

**[署名 ID]** ダイアログで、「[新しい証明書の作成](~/android/deploy-test/signing/index.md#newcert)」で作成した ID を選び、 **[続行]** をクリックします。

[![[署名 ID] ダイアログ](images/vs/02-select-identity-sml.png)](images/vs/02-select-identity.png#lightbox)

**[Google Play アカウント]** ダイアログで、 **[+]** ボタンをクリックして新しい Google Play アカウントを追加します。

[![[Google Play アカウント] ダイアログ](images/vs/03-google-play-accounts-sml.png)](images/vs/03-google-play-accounts.png#lightbox)

**[Google API Access の登録]** ダイアログでは、Google Play 開発者アカウントに API アクセスを提供する "_クライアント ID_" と "_クライアント シークレット_" を指定する必要があります。

[![[Google API Access の登録] ダイアログ](images/vs/04-register-google-api-access-sml.png)](images/vs/04-register-google-api-access.png#lightbox)

次のセクションでは、新しい Google API プロジェクトを作成し、必要な "_クライアント ID_" と "_クライアント シークレット_" を生成する方法を説明します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac は、Google Play にアプリを公開するための統合ワークフローを備えています。

「[新しい証明書の作成](~/android/deploy-test/signing/index.md#newcert)」では、Android アプリに署名するための新しい証明書の作成について説明しました。 次の手順では、Google Play に Xamarin.Android アプリを公開する方法を説明します。

1. Google Play 開発者アカウントにサインインし、Google Play 開発者アカウントにリンクされた新しいプロジェクトを作成します。
2. アプリを認証する "_OAuth クライアント_" を作成します。
3. 結果の_クライアント ID_ と_クライアント シークレット_を Visual Studio for Mac に入力します。
4. アカウントを Visual Studio for Mac に登録します。
5. 証明書でアプリケーションに署名します。
6. 署名されたアプリケーションを Google Play に公開します。

「[発行のためのアーカイブ](~/android/deploy-test/release-prep/index.md#archive)」では、 **[署名と配布...]** ダイアログに 2 種類の配布方法が表示されました。 **[Google Play]** を選択して **[Next]\(次へ\)** をクリックします。

[![[Select Android Distribution]\(Android の配布の選択\) ダイアログ](images/xs/01-select-google-play-sml.png)](images/xs/01-select-google-play.png#lightbox)

**[Google Play API アカウント]** ダイアログでは、Google Play 開発者アカウントに API アクセスを提供する "_クライアント ID_" と "_クライアント シークレット_" を指定する必要があります。

[![[Google Play API アカウント] ダイアログ](images/xs/02-google-play-api-account-sml.png)](images/xs/02-google-play-api-account.png#lightbox)

次のセクションでは、新しい Google API プロジェクトを作成し、必要な "_クライアント ID_" と "_クライアント シークレット_" を生成する方法を説明します。

-----

#### <a name="create-a-google-api-project"></a>Google API プロジェクトを作成する

最初に、[Google Play 開発者アカウント](https://play.google.com/apps/publish)にサインインします。
Google Play 開発者アカウントがまだない場合は、「[Get Started with Publishing](https://developer.android.com/distribute/googleplay/start.html)」(公開の概要) をご覧ください。
また、Google Play 開発者 API の「[Getting Started](https://developers.google.com/android-publisher/getting_started)」(概要) でも、Google Play 開発者 API の使い方が説明されています。 Google Play Developer Console にサインインした後、 **[Settings]\(設定\)** をクリックします。

[![[Settings]\(設定\) アイコン](images/01-google-play-developer-console-sml.png)](images/01-google-play-developer-console.png#lightbox)

**[SETTINGS]\(設定\)** ページで、 **[API access]\(API アクセス\)** を選び、 **[Create new project]\(新しいプロジェクトの作成\)** ボタンをクリックします。

[![[Create new project]\(新しいプロジェクトの作成\) ボタン](images/02-create-new-project-sml.png)](images/02-create-new-project.png#lightbox)

しばらくすると、新しい API プロジェクトが自動的に生成され、Google Play Developer Console アカウントにリンクされます。

次の手順では、アプリ用の OAuth クライアントを作成します (まだ作成されていない場合)。 ユーザーがアプリを使ってプライベート データへのアクセスを要求すると、アプリの認証に OAuth クライアント ID が使用されます。
**[Create OAuth Client]\(OAuth クライアントの作成\)** をクリックして、新しい OAuth クライアントを作成します。

[![[Create OAuth Client]\(OAuth クライアントの作成\) ボタン](images/03-create-oauth-client-sml.png)](images/03-create-oauth-client.png#lightbox)

数秒後に、新しいクライアント ID が生成されます。 **[View in Google Developers Console]\(Google Developers Console で表示\)** をクリックして、Google Developers Console で新しいクライアント ID を確認します。

[![表示されたクライアント ID](images/04-generated-client-id-sml.png)](images/04-generated-client-id.png#lightbox)

クライアント ID およびその名前と作成日が表示されます。 **[Edit OAuth Client]\(OAuth クライアントの編集\)** アイコンをクリックし、アプリのクライアント シークレットを表示します。

[![アプリの資格情報の表示](images/05-google-developer-console-sml.png)](images/05-google-developer-console.png#lightbox)

OAuth クライアントの既定の名前は *Google Play Android Developer* です。 これは、Xamarin.Android アプリの名前または任意の適切な名前に変更できます。 この例では、OAuth クライアント名をアプリの名前 **MyApp** に変更します。

[![表示されたクライアント ID とシークレット](images/06-client-id-and-secret-sml.png)](images/06-client-id-and-secret.png#lightbox)

**[Save]\(保存\)** をクリックして変更を保存します。 **[Credentials]\(資格情報\)** ページに戻るので、 **[Download JSON]\(JSON のダウンロード\)** アイコンをクリックして資格情報をダウンロードします。

[![[Download JSON]\(JSON のダウンロード\) アイコン](images/07-download-json-sml.png)](images/07-download-json.png#lightbox)

この JSON ファイルに含まれるクライアント ID とクライアント シークレットをコピーし、次のステップで **[署名と配布]** ダイアログに貼り付けることができます。

#### <a name="register-google-api-access"></a>Google API アクセスを登録する

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

クライアント ID とクライアント シークレットを使って、Visual Studio for Mac の **[Google Play API アカウント]** ダイアログを設定します。 アカウントの説明を入力できます。そうすれば、複数の Google Play アカウントを登録し、後で異なる Google Play アカウントに APK をアップロードできます。 クライアント ID とクライアント シークレットをこのダイアログにコピーし、 **[登録]** をクリックします。

[![[Google API Access の登録] ダイアログ](images/vs/05-enter-client-id-and-secret-sml.png)](images/vs/05-enter-client-id-and-secret.png#lightbox)

Web ブラウザーが開き、Google Play Android 開発者アカウントへのサインインを求められます (まだサインインしていない場合)。 サインインした後は、Web ブラウザーに次のプロンプトが表示されます。
**[許可]** をクリックしてアプリを承認します。

[![アプリ承認ダイアログ](images/vs/06-authorize-app-sml.png)](images/vs/06-authorize-app.png#lightbox)

#### <a name="publish"></a>公開

**[許可]** をクリックすると、 _[Received verification code.Closing...]\(確認コードを受け取りました。閉じています...\)_ と表示され、Visual Studio の Google Play アカウントのリストにアプリが追加されます。 **[Google Play アカウント]** ダイアログで **[続行]** をクリックします。

[![Google Play アカウントに追加されたアカウント](images/vs/07-account-added-sml.png)](images/vs/07-account-added.png#lightbox)

次に、 **[Google Play トラック]** ダイアログが表示されます。 Google Play では、4 種類のアプリのアップロードのトラックが提供されています。

- **[アルファ]** &ndash; 少数のテスト担当者一覧にごく初期のバージョンのアプリをアップロードするために使います。
- **[ベータ]** &ndash; 多数のテスト担当者一覧に初期のバージョンのアプリをアップロードするために使います。
- **[ロールアウト]** &ndash; ある割合のユーザーが、アプリの更新バージョンを受け取れます。たとえば、10% のユーザーから始めて、バグを解決しながら 100% のユーザーまで割合をゆっくり増やすことができます。
- **[実稼働]** &ndash; アプリが Google Play ストアから完全に配布できる状態のときは、このトラックを選びます。

アプリのアップロードに使う Google Play トラックを選び、 **[アップロード]** をクリックします。 **[ロールアウト]** を選ぶ場合は、割合の値を入力します。

[![[アルファ]、[ベータ]、[ロールアウト]、[実稼働] の選択](images/vs/08-google-play-track-sml.png)](images/vs/08-google-play-track.png#lightbox)

Google Play のテストおよび段階的なロールアウトの詳細については、「[Set up alpha/beta tests](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en)」(アルファ/ベータ テストの設定) をご覧ください。

次に、署名証明書のパスワードを入力するダイアログが表示されます。
パスワードを入力して、 **[OK]** をクリックします。

[![[署名パスワード] ダイアログ](images/vs/09-certificate-password-sml.png)](images/vs/09-certificate-password.png#lightbox)

**[アーカイブ マネージャー]** にアップロードの進行状況が表示されます。

[![APK のアップロードの進行状況](images/vs/10-uploading-apk-sml.png)](images/vs/10-uploading-apk.png#lightbox)

アップロードが完了すると、完了ステータスが Visual Studio の左下隅に示されます。

[![公開プロジェクト完了メッセージ](images/vs/11-published-sml.png)](images/vs/11-published.png#lightbox)

### <a name="troubleshooting"></a>トラブルシューティング

**[Google Play に公開する]** が機能するには、Google Play ストアに APK が 1 つ既に送信されている必要があることに注意してください。 APK がまだアップロードされていない場合、公開ウィザードの **[エラー]** ウィンドウに次のエラーが表示されます。

[![このアプリの最初の APK を手動でアップロードする必要があります](images/vs/12-upload-error-sml.png)](images/vs/12-upload-error.png#lightbox)

このエラーが発生したら、Google Play Developer Console を使って手動で APK をアップロードし (アドホック ビルドなど)、それ以降の APK の更新には **[配布チャネル]** ダイアログを使います。  詳細については、「[APK を手動でアップロードする](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md)」をご覧ください。 APK のバージョン コードはアップロードのたびに変更する必要があります。変更しないと、次のエラーが発生する可能性があります。

[![バージョン コード (1) を持つ APK が既にアップロードされています](images/vs/13-version-code-error-sml.png)](images/vs/13-version-code-error.png#lightbox)

このエラーを解決するには、異なるバージョン番号でアプリを再ビルドし、 **[配布チャネル]** ダイアログを使って Google Play に再送信します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

クライアント ID とクライアント シークレットを使って、Visual Studio for Mac の **[Google Play API アカウント]** ダイアログを設定します。 アカウントの説明を入力できます。そうすれば、複数の Google Play アカウントを登録し、後で異なる Google Play アカウントに APK をアップロードできます。 クライアント ID とクライアント シークレットをこのダイアログにコピーし、 **[登録]** をクリックします。

[![アクセス承認ダイアログ](images/xs/10-register-sml.png)](images/xs/10-register.png#lightbox)

クライアント ID とクライアント シークレットが受け入れられた場合は、"**登録に成功しました**" というメッセージが表示されます。 **[次へ]** をクリックします。

[![登録成功メッセージ](images/xs/11-registration-successful-sml.png)](images/xs/11-registration-successful.png#lightbox)

**[Google Play アカウント]** ダイアログで、Google アカウントと、アプリケーションのアップロードのトラックを選びます。

[![Google アカウント選択ダイアログ](images/xs/12-choose-google-account-sml.png)](images/xs/12-choose-google-account.png#lightbox)

Google Play では、4 種類のアプリのアップロードのトラックが提供されています。

- **[アルファ]** &ndash; 少数のテスト担当者一覧にごく初期のバージョンのアプリをアップロードするために使います。

- **[ベータ]** &ndash; 多数のテスト担当者一覧に初期のバージョンのアプリをアップロードするために使います。

- **[ロールアウト]** &ndash; ある割合のユーザーが、アプリの更新バージョンを受け取れます。たとえば、10% のユーザーから始めて、バグを解決しながら 100% のユーザーまで割合をゆっくり増やすことができます。

- **[実稼働]** &ndash; アプリが Google Play ストアから完全に配布できる状態のときは、このトラックを選びます。

Google Play のテストおよび段階的なロールアウトの詳細については、「[Set up alpha/beta tests](https://support.google.com/googleplay/android-developer/answer/3131213?hl=en)」(アルファ/ベータ テストの設定) をご覧ください。

次に、アプリの署名に使用される署名 ID を選択します。
既存の署名 ID を使用する場合は **[既存のキーを使用]** を選択します。それ以外の場合は、「[新しい証明書の作成](~/android/deploy-test/signing/index.md#newcert)」ガイドで新しいキーの作成方法をご覧ください。 アプリケーションに署名する証明書を選んだ後、 **[次へ]** をクリックします。

[![[Android の署名の識別情報] ダイアログ](images/xs/13-android-signing-identity-sml.png)](images/xs/13-android-signing-identity.png#lightbox)

この時点で、アプリを Google Play にアップロードできます。 **[Google Play に公開する]** ダイアログに、アプリに関する情報の要約が表示されます。 **[公開]** をクリックして、Google Play にアプリを公開します。

[![[Google Play に公開する] ダイアログ](images/xs/14-publish-to-google-play-sml.png)](images/xs/14-publish-to-google-play.png#lightbox)

**[Google Play に公開する]** が機能するには、Google Play ストアに APK が 1 つ既に送信されている必要があることに注意してください。 APK がアップロードされていない場合、次のエラーが発生する可能性があります。

> _Google Play では、このアプリの最初の APK を手動でアップロードする必要があります。これにはアドホック APK を使用できます。_

or

> _No application was found for the given package name. [404]_ (指定されたパッケージ名のアプリケーションが見つかりませんでした。[404])

このエラーを解決するには、Google Play Developer Console を使って手動で APK をアップロードし (アドホック ビルドなど)、それ以降の APK の更新には **[Google Play に公開する]** ダイアログを使います。 APK を手動でアップロードする方法については、「[APK を手動でアップロードする](~/android/deploy-test/publishing/publishing-to-google-play/manually-uploading-the-apk.md)」をご覧ください。

-----
