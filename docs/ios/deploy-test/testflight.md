---
title: TestFlight を使用して Xamarin.iOS アプリを配布する
description: Apple が所有するようになった TestFlight は、Xamarin.iOS アプリのベータ テストを行う主要な方法です。 この記事では、アプリのアップロードから iTunes Connect での使用まで、TestFlight プロセスのすべての手順について説明します。
ms.prod: xamarin
ms.assetid: BA880768-2BC8-41E4-B57E-A56F8EED4690
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: 8267c49302a755dcc433345b6a53aa9f2e2c71e6
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "71250174"
---
# <a name="using-testflight-to-distribute-xamarinios-apps"></a>TestFlight を使用して Xamarin.iOS アプリを配布する

_Apple が所有するようになった TestFlight は、Xamarin.iOS アプリのベータ テストを行う主要な方法です。この記事では、アプリのアップロードから iTunes Connect での使用まで、TestFlight プロセスのすべての手順について説明します。_

ベータ テストはソフトウェア開発サイクルの不可欠な部分であり、このプロセスを効率化する多くのクロスプラットフォーム アプリケーションが提供されています ([HockeyApp](http://hockeyapp.net/features/)、[Applause](http://www.applause.com/mobile-app-testing)、そしてもちろん Google Play の Android アプリ用 Native App Beta Testing など)。 このドキュメントでは、Apple の TestFlight について説明します。

TestFlight は Apple の iOS アプリ用ベータ テスト サービスであり、[iTunes Connect](https://itunesconnect.apple.com/) を介してのみアクセスできます。 現在は、iOS 8.0 以降のアプリに利用できます。 TestFlight は内部ユーザーと外部ユーザー両方のベータ テストに対応しており、外部ユーザー向けのベータ アプリ レビューにより、App Store に発行するときの最終レビューのプロセスが簡単になります。

以前は、テスト担当者に配布するには、Visual Studio for Mac でバイナリを生成し、TestFlightApp Web サイトにアップロードする必要がありました。 新しいプロセスは多くの点が改善されており、高品質で十分にテストされたアプリを App Store に公開できます。 次に例を示します。

- 外部テストに必要なベータ アプリ レビューは、最終 App Store レビュー成功の可能性を高くします。どちらも、Apple のガイドラインに準拠している必要があります。
- アップロードする前に、アプリを iTunes Connect に登録する必要があります。 これにより、プロビジョニング プロファイル、名前、証明書に不一致が存在しないことが保証されます。
- TestFlight アプリは実際の iOS アプリになったので、より高速に動作します。
- ベータ テスト完了後にアプリをレビューに移動するプロセスは、迅速かつ効率的であり、1 つのボタンをクリックするだけです。

## <a name="requirements"></a>必要条件

TestFlight でテストできるのは iOS 8.0 以降のアプリだけです。

すべてのテスト担当者は、少なくとも iOS 8 デバイスでアプリをテストする必要があります。 ただし、ベスト プラクティスでは iOS のすべてのバージョンでアプリをテストする必要があるようになっています。

## <a name="provisioning"></a>プロビジョニング

TestFlight でビルドをテストするには、新しいベータ資格で "*App Store 配布プロファイル*" を作成する必要があります。 この資格により TestFlight でのベータ テストが可能になり、すべての**新しい** App Store 配布プロファイルにこの資格が自動的に組み込まれます。 新しいプロファイルの生成手順については、「[Creating a Distribution Profile](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile)」(配布プロファイルの作成) ガイドをご覧ください。

[Xcode でビルドを検証する](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)ときに、配布プロファイルにベータ資格が含まれることを確認できます (下図参照)。

[![](testflight-images/validate-build.png "Apple へのアプリの提出")](testflight-images/validate-build.png#lightbox)

## <a name="testflight-workflow"></a>TestFlight のワークフロー

次のワークフローでは、アプリのベータ テスト用に TestFlight を使い始めるために必要な手順について説明します。

1. 新しいアプリの場合は、[iTunes Connect レコード](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)を作成します。
2. アプリケーションを iTunes Connect に[アーカイブして発行](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)します。
3. ベータ テストを管理します。
    - メタデータを追加します。
    - 内部ユーザーを追加します。
      - 最大 25 ユーザー。
    - 外部ユーザーを追加します。
      - 最大 1000 ユーザー。
      - ベータ テスト レビューが必要です。そのためには、Apple のガイドラインへの準拠が必要です。
4. ユーザーからのフィードバックを受け取り、それに対応して、ステップ 2 に戻ります。

## <a name="create-an-itunes-connect-record"></a>iTunes Connect レコードを作成する

1. Apple 開発者の資格情報を使って、[iTunes Connect ポータル](https://itunesconnect.apple.com/)にログインします。
2. **[My Apps]\(マイ アプリ\)** を選びます。

    [![](testflight-images/my-apps.png "[My Apps] を選びます")](testflight-images/my-apps.png#lightbox)

3. **[My Apps]\(マイ アプリ\)** 画面の左上隅にある **[+]** ボタンをクリックして、新しいアプリを追加します。 Mac および iOS の開発者アカウントがある場合は、ここで新しいアプリの種類を選ぶように求められます。

**[New iOS App]\(新しい iOS アプリ\)** 送信ウィンドウには、アプリの Info.plist とまったく同じ情報が表示される必要があります。

新しい iTunes Connect レコードの作成について詳しくは、「[Creating an iTunes Connect Record](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)」(iTunes Connect レコードの作成) ガイドをご覧ください。

### <a name="completing-the-new-ios-app-submission-form"></a>新しい iOS アプリ送信フォームの設定

このフォームはアプリの Info.plist ファイルとまったく同じ情報にする必要があります (下図参照)。

[![](testflight-images/infoplist.png "アプリの Info.plist")](testflight-images/infoplist.png#lightbox)
[![](testflight-images/newiosapp.png "iTunes Connect のフォーム")](testflight-images/newiosapp.png#lightbox)

- **[Name]\(名前\)** — アプリ バンドルの設定に使われるわかりやすい名前。 `Info.plist` の**アプリケーション名**エントリと完全に一致する必要があります。
- **[Primary Language]\(第一言語\)** — アプリ内で使われるベース言語。 通常はユーザーが使っている言語です。
- **[Bundle ID]\(バンドル ID\)** — 開発者アカウントに作成されているすべてのアプリ ID が一覧表示されるドロップダウン メニュー。
  - **[Bundle ID Suffix]\(バンドル ID サフィックス\)** — ワイルド カード バンドル ID (上の例のように * で終わる ID) を選んだ場合、バンドル ID サフィックスの入力を求めるボックスが追加表示されます。 上の例では、**バンドル ID** が `mobi.chkn.*`、サフィックスが **PageView** です。 これらを合わせて、`Info.plist` の**バンドル ID** が作成されます。
- **[Version]\(バージョン\)** — アップロードされるアプリのバージョン番号。 これは開発者が選びます。
- **[SKU]\(SKU\)** — SKU は、ユーザーには示されないアプリの一意 ID です。 製品 ID と同じようなものと考えることができます。 上の例では、日付とその日付のバージョン番号にしてあります。

## <a name="upload-your-app"></a>アプリをアップロードする

iTunes Connect レコードが作成されたら、新しいビルドをアップロードできます。 ビルドには新しいベータ資格が必要なことに注意してください。

最初に、IDE で[最終的な配布可能アプリ](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)をビルドした後、アプリケーション ローダーまたは Xcode のアーカイブ機能を使って [Apple にアプリを送信](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)します。

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

### <a name="create-an-archive"></a>アーカイブを作成する

 Visual Studio for Mac でバイナリをビルドするには、"_アーカイブ_" 機能を使う必要があります。 プロジェクトを右クリックし、 **[発行のためのアーカイブ]** を選びます (下図参照)。

 [![](testflight-images/new-archive.png "[発行のためのアーカイブ] を選択します")](testflight-images/new-archive.png#lightbox)

 詳しくは、「[Building the Distributable](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)」(配布可能アプリのビルド) ガイドをご覧ください。

### <a name="sign-and-distribute-your-app"></a>アプリに署名して配布する

 アーカイブを作成すると**アーカイブ ビュー**が自動的に開き、アーカイブされているすべてのプロジェクトがソリューション別にグループ化されて表示されます。 アプリに署名して配布の準備をするには、 **[署名と配布...]** を選びます (下図参照)。

[![](testflight-images/archive-view.png "アーカイブを作成するとアーカイブ ビューが自動的に開きます")](testflight-images/archive-view.png#lightbox)

 これにより、発行ウィザードが開きます。 **[App Store]** 配布チャネルを選んでパッケージを作成し、アプリケーション ローダーを開きます。 [プロビジョニング プロファイル] 画面で、署名 ID とプロビジョニング プロファイルを選ぶか、別の ID で再署名します。 パッケージの詳細を確認し、 **[発行]** をクリックして `.ipa` を保存します。

[![](testflight-images/group.png "署名 ID とプロビジョニング プロファイルを選ぶか、別の ID で再署名します")](testflight-images/group.png#lightbox)

 手順について詳しくは、「[Submitting your App to Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)」(Apple へのアプリの送信) セクションをご覧ください。

### <a name="submitting-your-build"></a>ビルドを送信する
 iTunes Connect にビルドをアップロードするためのアプリケーション ローダー プログラムが開きます。 **[Deliver Your App]\(アプリの配信\)** オプションを選び、前に作成した `.ipa` ファイルをアップロードします。 アプリケーション ローダーがビルドを検証して iTunes Connect にアップロードします。

 手順について詳しくは、「[Submitting your App to Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)」(Apple へのアプリの送信) セクションをご覧ください。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

### <a name="building-your-final-distributable"></a>最終的な配布可能アプリをビルドする
 Visual Studio 用 Xamarin プラグインは App Store に発行するための Xamarin.iOS アプリのアーカイブをサポートしていないので、Visual Studio から iOS アプリケーションを発行するには 2 つのオプションがあります。 これらの数値は、次のとおりです。

1. [Build Adhoc IPA]\(アドホック IPA のビルド\) コマンドを使って作成した IPA をアップロードします。
1. zip 形式の `.app` バンドルをアップロードします。

 「[Building the Distributable](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)」(配布可能アプリのビルド) ガイドでは、両方のオプションが説明されています。

### <a name="submitting-your-build"></a>ビルドを送信する
 Apple にアプリを送信するには、ビルド ホストに移動し、アプリケーション ローダー プログラムを使う必要があります。このプログラムは、Xcode の一部としてインストールされます。 アプリケーション ローダーへのアクセスの詳細については、Apple の「[Access Application Loader (アプリケーション ローダーにアクセスする)](http://help.apple.com/itc/apploader/#/apdATD1E927-D1E1A1303-D1E927A1126)」ガイドをご覧ください。

開いたら、 **[Deliver Your App]\(アプリの配信\)** オプションを選び、前に作成した zip または `.ipa` ファイルをアップロードします。 アプリケーション ローダーがビルドを検証して iTunes Connect にアップロードします。

 手順について詳しくは、「[Submitting your App to Apple](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)」(Apple へのアプリの送信) セクションをご覧ください。

-----

以上の手順など、App Store への送信プロセスについて詳しくは、「[Publishing to the App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)」(App Store への発行) ガイドをご覧ください。

iTunes Connect の **[My Apps]\(マイ アプリ\)** セクションに戻ると、アプリケーションが正常にアップロードされているはずです。 以上で、ベータ テストを行う準備ができました。

## <a name="manage-beta-testing"></a>ベータ テストを管理する

### <a name="add-metadata"></a>メタデータを追加する

TestFlight を使い始めるには、アプリの **[Prerelease]\(プレリリース\)** タブに移動します。 3 つのタブ [Builds]\(ビルド\)、[Internal Testers]\(内部テスト担当者\)、[External Testers]\(外部テスト担当者\) に一覧が表示されます (下図参照)。

[![](testflight-images/app-uploaded.png "[Builds] タブ、[Internal Testers] タブ、[External Testers] タブ")](testflight-images/app-uploaded.png#lightbox)

アプリにメタデータを追加するには、ビルド番号をクリックし、TestFlight をクリックします。

[![](testflight-images/metadata.png "メタデータを追加します")](testflight-images/metadata.png#lightbox)

**[Test Information]\(テスト情報\)** では、アプリに関する重要な情報をテスト担当者に提供できます。以下はその例です。

- テストの内容。
- アプリの説明。
- マーケティング URL — 追加しているアプリに関する情報を提供します。
- プライバシー ポリシー URL — 会社のプライバシー ポリシーに関する情報を提供する URL。
- フィードバック メール アドレス。

このメタデータは、内部テスト担当者に対しては**必須ではありません**が、外部テスト担当者に対しては**必須である**ことに注意してください。

<a name="beta-testing" />

### <a name="enable-beta-testing"></a>ベータ テストを有効にする

アプリのテストを開始する準備ができたら、バージョンの **[TestFlight Beta Testing]\(TestFlight ベータ テスト\)** スイッチをオンにします。

[![](testflight-images/turn-on-testing.png "[TestFlight Beta Testing] スイッチをオンにします")](testflight-images/turn-on-testing.png#lightbox)

各ビルドは、[TestFlight Beta Testing]\(TestFlight ベータ テスト\) をオンにしてから **60 日**間アクティブになります。 各ビルドの残り日数は、 **[Test Information]\(テスト情報\)** ページで確認できます。

[![](testflight-images/daysleft.png "テスト情報ページ")](testflight-images/daysleft.png#lightbox)

いつでもテストを無効にできます。

### <a name="internal-testers"></a>内部テスト担当者

内部テスト担当者は、iTunes Connect で次のロールのいずれかを割り当てられている、開発チームのメンバーです。

- **管理者** – 管理者は、iTunes Connect での新しいユーザーの追加と管理を担当します。
- **法務担当者** – チーム エージェントは、法務担当者ロールを割り当てられる唯一の管理者ユーザーです。 法的契約書に署名することができます。
- **技術担当者** – 技術ユーザーは、アプリに関するほとんどのプロパティを変更できます。 たとえば、アプリ情報の編集、バイナリのアップロード、レビュー用のアプリの送信を行うことができます。

各ビルドは、最大 25 人のメンバーで共有できます。

テスト担当者を追加するには、iTunes Connect メイン画面の **[Users and Roles]\(ユーザーとロール\)** を使います。

[![](testflight-images/users-and-roles.png "iTunes Connect メイン画面の [Users and Roles]\(ユーザーとロール\)")](testflight-images/users-and-roles.png#lightbox)

既存の iTunes Connect ユーザーが一覧に表示されます。 ユーザーを選ぶには、名前をクリックし、 **[Internal Tester]\(内部テスト担当者\)** スイッチをオンにして、 **[Save]\(保存\)** をクリックします。

[![](testflight-images/internal-tester.png "[Internal Tester] \(内部テスト担当者\) スイッチをオンにします")](testflight-images/internal-tester.png#lightbox)

一覧にないユーザーを追加するには、 *[Users]\(ユーザー\)* の横の **[+]** を選び、名、姓、メール アドレスを指定してアカウントを作成します。 アカウントをアクティブ化するには、ユーザーがメールを確認する必要があります。

[![](testflight-images/add-new-user.png "ユーザーの追加")](testflight-images/add-new-user.png#lightbox)

**[My Apps]\(マイ アプリ\) > [Prerelease]\(プレリリース\) > [Internal Testers]\(内部テスト担当者\)** に戻ると、TestFlight 内部ベータ テストに追加されたユーザーが表示されます。

[![](testflight-images/select-users.png "TestFlight 内部ベータ テストに追加されたユーザーの一覧")](testflight-images/select-users.png#lightbox)

名前を選んで **[Invite]\(招待\)** ボタンをクリックすることで、これらのテスト担当者を招待できます。 テスト担当者は、アプリのテストに招待するメールを受け取ります。

[Internal Testers]\(内部テスト担当者\) ページの状態列で、招待の状態を確認できます。

[![](testflight-images/status-added.png "招待の状態")](testflight-images/status-added.png#lightbox)

### <a name="external-testers"></a>外部テスト担当者

アプリのベータ テストに外部テスト担当者を招待する前に、ベータ アプリ レビューを行う必要があり、したがって、[App Store レビュー ガイドライン](https://developer.apple.com/app-store/review/guidelines/)に準拠する必要があります。

レビュー用にアプリを送信するには、ビルドの隣にある **[Submit For Beta App Review]\(ベータ アプリ レビュー用に送信\)** テキストをクリックします (下図参照)。

[![](testflight-images/beta-app-review.png "[Submit For Beta App Review]\(ベータ アプリ レビュー用に送信\)")](testflight-images/beta-app-review.png#lightbox)

アプリがレビューに合格するには、TestFlight のベータ情報ページで必要なすべてのメタデータを入力する必要があります。

招待の準備を開始し、[External Testers]\(外部テスト担当者\) タブでメール アドレス、名、姓を入力して、最大 2000 人の外部テスト担当者を追加できます (下図参照)。 入力するメール アドレスは、Apple ID でなくてもかまいません。これは、招待を受け取るためだけのメール アドレスです。

[![](testflight-images/add-external.png "テスターを招待します")](testflight-images/add-external.png#lightbox)

外部テスト担当者の数が多い場合は、 **[Import File]\(ファイルのインポート\)** リンクを使い、次のような形式の行を含む `CSV` ファイルをインポートできます。

``` 
first name, last name, email address
```

また、外部テスト担当者を異なるグループに追加して、テスト担当者を整理することもできます。

外部テスト担当者の詳細を入力した後、 **[Add]\(追加\)** をクリックして、招待に対するユーザーの同意があることを確認します。

[![](testflight-images/confirm-consent.png "招待に対するユーザーの同意があることを確認します")](testflight-images/confirm-consent.png#lightbox)

ベータ アプリ レビューが正常に行われた後でのみ、外部テスト担当者に招待を送信できます。 この時点で、ビルド ページの **[External]\(外部\)** の下のテキストが、 **[Send Invites]\(招待の送信\)** に変わります。 これをクリックして、既に追加しているすべてのテスト担当者に招待を送信します。

アプリが却下された場合は、 **[Resolution Center]\(解決センター\)** で示されている問題を修正し、更新されたバイナリ全体をレビューのために再送信する必要があります。

## <a name="as-a-beta-tester"></a>ベータ テスト担当者として

テスト担当者を招待すると、テスト担当者は以下のスクリーンショットのようなメールを受け取ります。

[![](testflight-images/tester-email.png "電子メールの招待状の例")](testflight-images/tester-email.png#lightbox)

**[Open in TestFlight]\(TestFlight で開く\)** ボタンをクリックすると、アプリが TestFlight アプリケーションで開きます。または、まだダウンロードされていない場合は、App Store に移動してダウンロードできます。

アプリが TestFlight で開くと、テスト内容の詳細が表示され、テスト担当者は自分の iOS 8.0 (またはそれ以降) デバイスにアプリケーションをインストールするように求められます。

[![](testflight-images/install-app.png "TestFlight に、テスト内容の詳細が表示されます")](testflight-images/install-app.png#lightbox)

デバイスのホーム画面では、テスト ビルドはアプリケーション名の前のオレンジ色のドットで示されます。

テスト担当者は TestFlight アプリでフィードバックを送信でき、開発者はメタデータで指定されているメール アドレスでこの情報を受け取ることができます。

### <a name="beta-testing-complete"></a>ベータ テストの完了

ベータ テストが完了すると、Apple による App Store レビュー用にアプリを送信できます。 このプロセスは、iTunes Connect で **[Submit for Review]\(レビュー用に送信\)** ボタンをクリックするだけの簡単なものです (下図参照)。

[![](testflight-images/submit-for-review.png "[Submit for Review] \(レビュー用に送信\) ボタンをクリックします")](testflight-images/submit-for-review.png#lightbox)

## <a name="summary"></a>まとめ

この記事では、iTunes Connect で Apple の TestFlight ベータ テストを使う方法を説明しました。 iTunes Connect に新しいビルドをアップロードする方法と、アプリを使うように内部および外部のベータ テスト担当者を招待する方法を説明しました。

## <a name="related-links"></a>関連リンク

- [iTunes Connect レコードの作成](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md#creating)
- [App Store への発行](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [App Store で配布するためのアプリのプロビジョニング](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#provisioning)
- [Apple TestFlight ベータの使用](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/BetaTestingTheApp.html#//apple_ref/doc/uid/TP40011225-CH35-SW2)
