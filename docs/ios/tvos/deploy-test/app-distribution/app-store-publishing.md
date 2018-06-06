---
title: Apple TV のアプリ ストアに発行します。
description: このドキュメントでは、Apple TV のアプリ ストアにアプリを公開する方法について説明します。 これには、構成、プロビジョニング、ビルド、および Xamarin でビルドした tvOS アプリケーションを送信する方法について説明します。
ms.prod: xamarin
ms.assetid: 52448C93-DC19-40FA-BF8C-608AE680FF49
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: ac905caaf0bdefe7f0c5502be0bd63102ca5a813
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789305"
---
# <a name="publishing-to-the-apple-tv-app-store"></a>Apple TV のアプリ ストアに発行します。

Apple TV のすべてのデバイスにアプリケーションの配布の順序で、Apple がアプリを発行する必要があります、 *Apple TV の App Store*、tvOS アプリのワンストップ ショッピング場所は、アプリ ストアを作成します。 さまざまな種類のアプリの開発者は、この単一の配布ポイントの大規模な成功に大文字で入力できます。 Apple TV の App Store は、アプリの開発者に配布および支払いシステムの両方を提供するターンキー ソリューションです。

Apple TV のアプリ ストアへのアプリケーションの送信処理が含まれます。

1. *アプリ ID* の作成および*権利*の選択。
2. *配布プロビジョニング プロファイル*の作成。
3. このプロファイルを使用して、アプリをビルドします。
4. 使用してアプリを送信する*iTunes Connect*です。


この記事では、準備、ビルド、および Apple TV の App Store の配布用のアプリを送信するために必要なすべての手順について説明します。

<a name="Before_you_Submit" />

## <a name="before-you-submit-an-application"></a>アプリケーションを提出する前に

Apple TV のアプリ ストアへのパブリケーション用のアプリを送信した後に品質とコンテンツの Apple のガイドラインを満たしていることを保証する Apple によって確認プロセスを戻ります。 アプリケーションがこのガイドラインを満たしていない場合、Apple はアプリケーションを却下します。その時点で、Apple によって指摘された不適合箇所の改善が必要になります。
したがって、このガイドラインをよく理解し、アプリケーションを適応させることで、Apple の審査を通過できる可能性が最も高くなります。 Apple のガイドラインについては、「 [App Store レビューに関するガイドライン](https://developer.apple.com/appstore/resources/approval/guidelines.html)と[新しい Apple tv アプリ送信を準備する](https://developer.apple.com/tvos/submit/)です。

アプリを提出する際の注意事項をいくつか以下に示します。

1. アプリの説明に一致するアプリに含まれる機能を確認します。
2. アプリがクラッシュしないテスト通常使用します。 これには、Apple TV のデバイスがサポートするすべての使用率が含まれます。


Apple では、Apple TV の App Store の規則のヒントの一覧も保持されます。 これらは、[App Store での配布](https://developer.apple.com/appstore/resources/submission/tips.html)に関するページで確認できます。

<a name="Configuring_your_Application_in_iTunes_Connect" />

## <a name="configuring-your-application-in-itunes-connect"></a>iTunes Connect でのアプリケーションの構成

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) Apple TV のアプリ ストア上、tvOS アプリの管理、特に、ツールを web ベースのスイートです。 Xamarin.tvOS アプリは、正しく設定する必要があり、レビュー用に、Apple に送信でき、販売または Apple TV の App Store の無料のアプリとに、最終的には、解放する前に、iTunes Connect で構成します。

次の手順で行います。

1. 無償または販売用の iOS アプリケーションをリリースするために、iTunes Connect の「**契約/税金/口座情報**」セクションで契約が適切で最新であることを確認します。
2. 新規作成**iTunes Connect レコード**アプリケーションの指定とその**表示名**(よう Apple TV の App Store 内)。
3. **[販売価格]** を選択するか、アプリケーションを無料でリリースするかを指定します。
4. 提供、**アプリ ストア アイコン**(大きいアイコン) とそれをサポートしている Apple TV のデバイスでのアクションでアプリのスクリーン ショット。 参照してください、[アイコンとイメージ処理](~/ios/tvos/app-fundamentals/icons-images.md)詳細についてガイドします。
5. 提供、クリア、簡潔な**説明**アプリの機能など、エンドユーザーにパフォーマンスが向上します。
6. 提供**カテゴリ**、**サブ カテゴリ**、および**キーワード**Apple TV のアプリ ストアでアプリを検索するユーザーを支援します。
7. Apple で必要な**連絡先**と、Web サイトの**サポート** URL を提供します。
8. アプリケーションの設定**評価**、Apple TV の App Store で保護者による制限で使用されます。
9. **Game Center** や**アプリ内購入**など、オプションの App Store テクノロジを構成します。

詳細についてを参照してください、 [、tvOS アプリを iTunes Connect で構成](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md)ドキュメント。

<a name="Preparing_for_App_Store_Distribution" />

## <a name="preparing-for-app-store-distribution"></a>App Store での配布の準備

Apple TV のアプリ ストアにアプリを発行するには、最初の配布で、多くの手順を作成する必要があります。 次のセクションでは、ビルドすることができ、確認し、リリース Apple TV のアプリ ストアに送信できるように、パブリケーションの Xamarin.tvOS アプリを準備するためのすべてのものについて説明します。

<a name="Provisioning_for_Application_Services" />

### <a name="provisioning-for-application-services"></a>アプリケーション サービスのプロビジョニング

Apple では、その一意の ID を作成するときに、tvOS アプリのアクティブ化可能な権利とも呼ばれる特別なのアプリケーション サービスの選択範囲を提供します。 を使用するカスタム権利かどうかどうかを必要となります Apple TV のアプリ ストアに発行する前に、Xamarin.tvOS アプリの一意の ID を作成します。

アプリ ID を作成して、必要に応じて権利を選択するには、Apple の Web ベース iOS プロビジョニング ポータルを使用して、次の手順を実行する必要があります。

1. 選択**プロビジョニング** > **開発**です。
2. **+** ボタンをクリックし、新しいアプリケーションの**名前**と**バンドル ID** を指定します。
3. 画面の下部までスクロールし、いずれも選択**App Services** Xamarin.tvOS アプリでする必要があります。
4. **[続行]** ボタンをクリックし、画面の指示に従って新しいアプリ ID を作成します。

選択して、アプリ ID を定義するときに必要なアプリケーション サービスの構成、に加えても構成する必要がアプリ ID と権利 Xamarin.tvOS プロジェクトの両方を編集することによって、`Info.plist`と`Entitlements.plist`ファイル。

Mac 用 Visual Studio では、次の操作を行います。

1. **ソリューション エクスプローラー**で `Info.plist` ファイルをダブルクリックして、編集用に開きます。
2. **TvOS Application Target**セクションで、アプリケーションの名前を入力し、入力、**バンドル Id**アプリ ID を定義したときに作成されたこと
3. 変更内容を `Info.plist` ファイルに保存します。
4. **ソリューション エクスプローラー**で `Entitlements.plist` ファイルをダブルクリックして、編集用に開きます。
5. 選択し、アプリ ID を定義したときに、上記の実行設定と一致するように Xamarin.tvOS アプリのために必要な権限を構成します。
6. 変更内容を `Entitlements.plist` ファイルに保存します。

詳しい手順については、[App Services のプロビジョニング](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices)に関するドキュメントを参照してください。 このドキュメントは、iOS 用に作成された、同じ手順が Xamarin.tvOS アプリのプロビジョニングに使用されます。

<a name="Setting_the_Apps_Icons_and_Launch_Screens" />

### <a name="setting-the-apps-icons-launch-image-and-top-shelf-image"></a>イメージと上段のイメージを起動して、アプリ アイコンを設定するには、

Apple TV のアプリ ストアに含まれるように、Apple によって受け入れられるを tvOS アプリでは、適切なアイコン、起動およびで実行されている Apple TV のデバイスのすべての上位棚のイメージが必要です。 コンパイルされる必須のイメージ アセットを追加が必要な`Assets.car`ファイルし、iTunes Connect にアップロードされる前に、Xamarin.tvOS アプリのバンドルに含まれます。

詳細については、次を参照してください、[アイコンとイメージ処理](~/ios/tvos/app-fundamentals/icons-images.md)ドキュメント。

<a name="Creating_and_Installing_a_Distribution_Profile" />

### <a name="creating-and-installing-a-distribution-profile"></a>配布プロファイルの作成とインストール

tvOS を使用して*プロビジョニング プロファイル*に特定のアプリケーションのビルドの配置方法を制御します。 これらは、アプリの署名に使用される証明書、*アプリケーション ID*、およびアプリをインストールできる場所に関する情報を含むファイルです。 開発とアドホック配布の場合、プロビジョニング プロファイルには、アプリを展開できる許可デバイスのリストも含まれます。 ただし、Apple TV の App Store の配布の唯一の証明書とアプリ ID の情報が含まれる、パブリック配布用の唯一の方法は Apple TV の App Store からです。

プロビジョニングには、Apple の Web ベース iOS プロビジョニング プロファイルを使用する以下の手順が必要です。

1.  **[プロビジョニング]** > **[配布]** の順に選択します。
2.  クリックして、 **+** ボタンをクリックし、として作成するディストリビューション プロファイルの種類の選択**Apple TV の App Store**です。
3.  配布プロファイルを作成する**アプリ ID** をドロップダウン リストから選択します。
4.  アプリケーションの署名に必要な証明書を選択します。
5.  新しい**配布プロファイル**の**名前**を入力して、プロファイルを生成します。
6.  Xcode で使用可能なプロファイルの一覧を更新します。
7.  プロビジョニング プロファイル用の Visual Studio での分布を選択、 **App Store** _ビルド構成_です。

詳しい手順については、[配布プロファイルの作成](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#creatingprofile)と [Xamarin.iOS プロジェクトでの配布プロファイルの選択](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile)に関するページを参照してください。 IOS に固有では、これらのドキュメントが、tvOS アプリ用のと同じ手法を使用します。


<a name="Setting_the_Build_Configuration_for_your_Application" />

### <a name="setting-the-build-configuration-for-your-application"></a>アプリケーションのビルド構成の設定

既定では、新しい Xamarin.tvOS アプリを作成するときに_ビルド構成_の両方が自動的に作成**デバッグ**と**リリース**展開します。 Apple に送信される、アプリの最終的なビルドを実行する前に、いくつかの変更をベースにする必要がありますを**リリース**構成します。

次の手順で行います。

1. 右クリックし、**プロジェクト名**で、**ソリューション エクスプ ローラー**と選択**オプション**に編集用に開きます。
2. TvOS の特定のバージョンを対象とする場合は選択下**tvOS ビルド** > **iOS SDK バージョン**します。 TvOS のサポートのプレビュー リリースでは、この値に設定のままにしてください。**既定**です。
3. リンク全体のサイズが小さく、アプリの配布可能な未使用のメソッド、プロパティ、クラスを除去など、ほとんどの場合は、の既定値に残しておく必要があります**リンク フレームワーク SDK のみ**です。 場合など、いくつかの状況でいくつかの固有の仕様を使用してサード パーティ製のライブラリ、この値を設定する必要があります**リンクしないように**必要な要素が削除されることを防止します。
4. Xamarin.tvOS アプリを出荷するには、ある LLVM 最適化コンパイラを使用する必要があります。 いることを確認、**ある LLVM のコンパイラの最適化を使用して**下にあるチェック ボックスがオン、**リリース**構成します。
5. Apple では、tvOS アプリが bitcode を使用することも必要です。 もう一度 、**リリース**構成では、追加`--bitcode=asmonly`を**追加 mtouch 引数**ボックス。
6. **IOS 用の最適化の PNG イメージ ファイル**これによって、さらに低下するアプリの成果物のサイズに応じて、チェック ボックスを選択する必要があります。
7. デバッグする必要があります*いない*にするには、ビルドが不必要に大きく、有効にします。


<a name="Building_and_Submitting_the_Distributable" />

## <a name="building-and-submitting-the-distributable"></a>配布可能なアプリケーションのビルドと提出

Xamarin.tvOS アプリを正しく構成されているを確認して、Apple に送信される最終的な分布ビルドを実行する準備ができました。

#### <a name="build-your-archive"></a>アーカイブをビルドする

1. Visual Studio for Mac で**リリースとデバイス**の構成を選択します。

    ![](app-store-publishing-images/buildxs01new.png "リリース構成を選択します。")
2. **[ビルド]** メニューから **[発行のためのアーカイブ]** を選択します。

    [![](app-store-publishing-images/buildxs02new.png "[発行のためのアーカイブ] を選択します")](app-store-publishing-images/buildxs02new.png#lightbox)
3. アーカイブが作成されると、**[アーカイブ]** ビューが表示されます。

    [![](app-store-publishing-images/buildxs03new.png "アーカイブ ビュー")](app-store-publishing-images/buildxs03new.png#lightbox)

### <a name="sign-and-distribute-your-app"></a>アプリに署名して配布する

アーカイブのためにアプリケーションをビルドするたびに、*アーカイブ ビュー*が自動的に開き、アーカイブされているすべてのプロジェクトがソリューション別にグループ化されて表示されます。 既定では、このビューには現在開いているソリューションのみが表示されます。 アーカイブのあるソリューションをすべて表示するには、**[アーカイブをすべて表示]** オプションをクリックします。

(App Store 展開またはエンタープライズ展開で) 顧客に展開したアーカイブは保存しておくことをお勧めします。そうすれば、デバッグ情報が生成された場合、後でそれを記号で表すことができます。

アプリに署名し、配布の準備をするには、次のようにします。

1. 選択、**サインインし、配布しています.**、以下を参照します。

    [![](app-store-publishing-images/buildxs04new.png "、TheSign と配布を選択しています.")](app-store-publishing-images/buildxs04new.png#lightbox)
2. これにより、発行ウィザードが開きます。 **[App Store]** 配布チャネルを選択してパッケージを作成し、アプリケーション ローダーを開きます。

    [![](app-store-publishing-images/distribute01.png "アプリ ストアの配布チャネルを選択します。")](app-store-publishing-images/distribute01.png#lightbox)
3. プロビジョニング プロファイル画面で、署名 id とプロビジョニング プロファイルが、対応するを選択するか、別の id を使って再度署名します。

    [![](app-store-publishing-images/distribute02.png "署名 id と対応するプロビジョニング プロファイルを選択します。")](app-store-publishing-images/distribute02.png#lightbox)
4. パッケージの詳細を確認し、**[発行]** をクリックして `.ipa` パッケージを保存します。

    [![](app-store-publishing-images/distribute03.png "パッケージの詳細を確認してください。")](app-store-publishing-images/distribute03.png#lightbox)
5. `.ipa` が保存されたら、アプリケーション ローダーを使用して、アプリを iTunes Connect にアップロードできます。

    [![](app-store-publishing-images/distribute04.png "アプリケーション ローダーを使用して接続を iTunes にアップロード")](app-store-publishing-images/distribute04.png#lightbox)

配布ビルドが作成され、アーカイブされたら、アプリケーションを iTunes Connect に提出できます。

<a name="Submitting_Your_App_to_Apple" />

## <a name="submitting-your-app-to-apple"></a>Apple へのアプリの提出

配布ビルドが完了したら、審査のために Apple に iOS アプリケーションを提出し、App Store でリリースできます。


Mac 用の Visual Studio で、アーカイブ ワークフローはアプリケーション ローダーを自動的に開く、保存すると、 `.ipa`:

2. *[Deliver Your App]\(アプリの配信\)* を選択して、*[選択]* ボタンをクリックします。

    [![](app-store-publishing-images/publishvs01.png "[Deliver Your App]\(アプリの配信\) を選択します")](app-store-publishing-images/publishvs01.png#lightbox)

3. 前の手順で作成した IPA ファイルまたは zip ファイルを選択し、**[OK]** ボタンをクリックします。
4. アプリケーション ローダーはファイルを検証します。

    [![](app-store-publishing-images/publishvs02.png "アプリケーション ローダー検証画面")](app-store-publishing-images/publishvs02.png#lightbox)
5. *[次へ]* ボタンをクリックすると、アプリケーションは App Store に対して検証されます。

    [![](app-store-publishing-images/publishvs03.png "アプリ ストアに対して検証されるアプリケーション")](app-store-publishing-images/publishvs03.png#lightbox)
6. **[送信]** ボタンをクリックして、審査のために Apple にアプリケーションを送信します。
7. アプリケーション ローダーは、ファイルが正常にアップロードされたときに通知します。

<a name="iTunes_Connect_Status" />

### <a name="itunes-connect-status"></a>iTunes Connect のステータス

ITunes Connect に再度ログインし、使用可能なアプリの一覧からアプリを選択して場合、iTunes Connect でステータス表示されるはずである**確認を待機している**(これは一時的に読み取ることが**受信アップロード**処理中には)。

[![](app-store-publishing-images/image21.png "ITunes で状態の接続確認の待機を表示")](app-store-publishing-images/image21.png#lightbox)

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>トラブルシューティング

Apple TV の App Store に Xamarin.tvOS アプリの提出の問題が発生した場合を参照してください、[トラブルシューティング](~/ios/tvos/troubleshooting.md)ガイドです。 発生する可能性があるいくつかの既知の問題と、Xamarin.tvOS でそれらを解決する方法が含まれています。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、構成、ビルド、および Apple TV の App Store のパブリケーション用のアプリを送信するための手順が表示されます。 最初に、配布プロビジョニング プロファイルの作成とインストールに必要な手順を説明しました。 次に、そのとおし、Visual Studio for Mac を使用して配布ビルドを作成する方法です。 最後に、その方法を紹介を iTunes Connect と Xcode アーカイブ ツールを使用して、Apple TV のアプリ ストアへのアプリケーションを送信します。


## <a name="related-links"></a>関連リンク

- [アイコンとイメージの使用](~/ios/tvos/app-fundamentals/icons-images.md)
- [新しい Apple tv のアプリの提出を準備します。](https://developer.apple.com/tvos/submit/)
- [App Store への提出に関するヒント](https://developer.apple.com/appstore/resources/submission/tips.html)
- [一般的なアプリケーションの却下理由](https://developer.apple.com/app-store/review/rejections/)
- [App Store の審査に関するガイドライン](https://developer.apple.com/appstore/resources/approval/guidelines.html)
