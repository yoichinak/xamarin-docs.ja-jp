---
title: Apple TV App Store への発行
description: このドキュメントでは、Apple TV App Store にアプリを発行する方法について説明します。 これには、構成、プロビジョニング、ビルド、および Xamarin でビルドされた tvOS アプリケーションを送信する方法について説明します。
ms.prod: xamarin
ms.assetid: 52448C93-DC19-40FA-BF8C-608AE680FF49
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 73ee7fc3c28fc7a8476010e8bf7567b3e5ef590d
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865083"
---
# <a name="publishing-to-the-apple-tv-app-store"></a>Apple TV App Store への発行

順序ですべての Apple TV のデバイスにアプリケーションの配布、Apple は、要求を発行するアプリ、 *Apple TV App Store*、App Store の tvOS アプリのワンストップ ショッピングの場所を作成します。 さまざまな種類のアプリの開発者は、1 か所に配布の大規模な成功した場合に大文字で入力できます。 Apple TV App Store は、ターンキー ソリューションでは、両方の配布と支払いシステム アプリ開発者に提供します。

Apple TV App Store へのアプリケーションの提出プロセスが含まれます。

1. *アプリ ID* の作成および*権利*の選択。
2. *配布プロビジョニング プロファイル*の作成。
3. アプリを構築するのにには、このプロファイルを使用します。
4. アプリの提出*iTunes Connect*します。


この記事では、プロビジョニング、ビルド、および Apple TV App Store で配布用のアプリの送信に必要なすべての手順について説明します。

<a name="Before_you_Submit" />

## <a name="before-you-submit-an-application"></a>アプリケーションを提出する前に

Apple TV App Store に公開するアプリを登録したら、品質とコンテンツを Apple のガイドラインを満たしていることを保証する Apple によって確認プロセスが。 アプリケーションがこのガイドラインを満たしていない場合、Apple はアプリケーションを却下します。その時点で、Apple によって指摘された不適合箇所の改善が必要になります。
したがって、このガイドラインをよく理解し、アプリケーションを適応させることで、Apple の審査を通過できる可能性が最も高くなります。 Apple のガイドラインについては、「 [App Store レビューに関するガイドライン](https://developer.apple.com/appstore/resources/approval/guidelines.html)と[新しい Apple TV の、アプリの提出を準備する](https://developer.apple.com/tvos/submit/)します。

アプリを提出する際の注意事項をいくつか以下に示します。

1. アプリの説明には、アプリに含まれる機能が一致することを確認します。
2. アプリが通常の使用でクラッシュしないことをテストします。 これには、サポートするすべての Apple TV デバイスでの使用率が含まれます。


Apple では、Apple TV App Store の提出に関するヒントの一覧も管理します。 これらは、[App Store での配布](https://developer.apple.com/appstore/resources/submission/tips.html)に関するページで確認できます。

<a name="Configuring_your_Application_in_iTunes_Connect" />

## <a name="configuring-your-application-in-itunes-connect"></a>iTunes Connect でのアプリケーションの構成

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) Apple TV App Store で tvOS アプリの管理など、web ベースのツールのスイートです。 Xamarin.tvOS アプリは、正しく設定する必要があり、確認のために Apple に提出でき、販売または Apple TV App Store での無料のアプリとしてリリースされる最終的には、前に、iTunes Connect で構成します。

次の手順で行います。

1. 無償または販売用の iOS アプリケーションをリリースするために、iTunes Connect の「**契約/税金/口座情報**」セクションで契約が適切で最新であることを確認します。
2. 新規作成**iTunes Connect レコード**アプリケーションの指定とその**表示名**(Apple TV App Store に表示) とします。
3. **[販売価格]** を選択するか、アプリケーションを無料でリリースするかを指定します。
4. 提供、 **App Store アイコン**(大きいアイコン) とは、Apple TV のデバイス上で実行、アプリケーションのスクリーン ショット。 参照してください、[アイコンとイメージを使用する](~/ios/tvos/app-fundamentals/icons-images.md)詳細はガイド。
5. 提供明確で簡潔な**説明**アプリのなどの機能と、エンドユーザーにパフォーマンスが向上します。
6. 提供**カテゴリ**、**細かく分類さ**、および**キーワード**Apple TV App Store でアプリを検索するユーザーを支援します。
7. Apple で必要な**連絡先**と、Web サイトの**サポート** URL を提供します。
8. アプリケーションの設定**評価**、Apple TV App Store で保護者による制限で使用されます。
9. **Game Center** や**アプリ内購入**など、オプションの App Store テクノロジを構成します。

詳細についてを参照してください、 [tvOS アプリの iTunes Connect で構成](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md)ドキュメント。

<a name="Preparing_for_App_Store_Distribution" />

## <a name="preparing-for-app-store-distribution"></a>App Store での配布の準備

Apple TV App Store にアプリを発行するには、まず、配布で、多くの手順をビルドする必要があります。 次のセクションを構築でき、審査およびリリースの Apple TV App Store に送信するように、パブリケーションの Xamarin.tvOS アプリを準備するために必要なすべてのものについて説明します。

<a name="Provisioning_for_Application_Services" />

### <a name="provisioning-for-application-services"></a>アプリケーション サービスのプロビジョニング

Apple では、特別なアプリケーション サービスの権利は、その一意の ID を作成するときに、tvOS アプリをアクティブにできるとも呼ばれます。 選択範囲を提供します。 かどうかをカスタムの権利を使用しているかどうか、Apple TV App Store に発行する前に、Xamarin.tvOS アプリの一意の ID を作成する必要があります。

アプリ ID を作成して、必要に応じて権利を選択するには、Apple の Web ベース iOS プロビジョニング ポータルを使用して、次の手順を実行する必要があります。

1. 選択**プロビジョニング** > **開発**します。
2. **+** ボタンをクリックし、新しいアプリケーションの**名前**と**バンドル ID** を指定します。
3. 画面の一番下までスクロールし、いずれかを選択**App Services** Xamarin.tvOS アプリでする必要があります。
4. **[続行]** ボタンをクリックし、画面の指示に従って新しいアプリ ID を作成します。

選択して、アプリ ID を定義するときに必要なアプリケーション サービスの構成、に加えても構成する必要がアプリ ID と権利 Xamarin.tvOS プロジェクトの両方を編集することによって、`Info.plist`と`Entitlements.plist`ファイル。

Visual studio for Mac の次の操作を行います。

1. **ソリューション エクスプローラー**で `Info.plist` ファイルをダブルクリックして、編集用に開きます。
2. **TvOS アプリケーションのターゲット**セクションで、アプリケーションの名前を入力し、入力、**バンドル識別子**アプリ ID を定義したときに作成されたこと
3. 変更内容を `Info.plist` ファイルに保存します。
4. **ソリューション エクスプローラー**で `Entitlements.plist` ファイルをダブルクリックして、編集用に開きます。
5. 選択し、アプリ ID を定義するときに行った前述のセットアップと一致するように、Xamarin.tvOS アプリが必要な権利を構成します。
6. 変更内容を `Entitlements.plist` ファイルに保存します。

詳しい手順については、[App Services のプロビジョニング](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices)に関するドキュメントを参照してください。 このドキュメントは、iOS 用に記述された、Xamarin.tvOS アプリをプロビジョニングする同じ手順が使用されます。

<a name="Setting_the_Apps_Icons_and_Launch_Screens" />

### <a name="setting-the-apps-icons-launch-image-and-top-shelf-image"></a>アプリのアイコン、起動イメージとトップ シェルフのイメージの設定

Apple TV App Store に含めるように Apple によって受け入れられる tvOS アプリの場合は、適切なアイコン、起動とトップ シェルフのイメージで実行されている Apple TV のデバイスのすべてのことが必要です。 コンパイルされる必要なのイメージ アセットを追加します必要があります、`Assets.car`ファイルを開き、Xamarin.tvOS アプリのバンドルに含まれる前に、iTunes Connect にアップロードされます。

詳細な手順についてを参照してください、[アイコンとイメージを使用する](~/ios/tvos/app-fundamentals/icons-images.md)ドキュメント。

<a name="Creating_and_Installing_a_Distribution_Profile" />

### <a name="creating-and-installing-a-distribution-profile"></a>配布プロファイルの作成とインストール

tvOS を使用して*プロビジョニング プロファイル*を特定のアプリケーションのビルドを展開する方法を制御します。 これらは、アプリの署名に使用される証明書、*アプリケーション ID*、およびアプリをインストールできる場所に関する情報を含むファイルです。 開発とアドホック配布の場合、プロビジョニング プロファイルには、アプリを展開できる許可デバイスのリストも含まれます。 ただし、Apple TV App Store 配布のみ証明書とアプリ ID の情報が含まれる場合は、一般に公開する唯一のメカニズムは Apple TV App Store から。

プロビジョニングには、Apple の Web ベース iOS プロビジョニング プロファイルを使用する以下の手順が必要です。

1.  **[プロビジョニング]**  >  **[配布]** の順に選択します。
2.  をクリックして、 **+** ボタンをクリックし、として作成する配布プロファイルの種類を選択**Apple TV App Store**します。
3.  配布プロファイルを作成する**アプリ ID** をドロップダウン リストから選択します。
4.  アプリケーションの署名に必要な証明書を選択します。
5.  新しい**配布プロファイル**の**名前**を入力して、プロファイルを生成します。
6.  Xcode で使用可能なプロファイルの一覧を更新します。
7.  プロビジョニング プロファイルを Visual Studio での分布を選択、 **App Store** _ビルド構成_します。

詳しい手順については、[配布プロファイルの作成](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#creatingprofile)と [Xamarin.iOS プロジェクトでの配布プロファイルの選択](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile)に関するページを参照してください。 IOS に固有ではこれらのドキュメントの両方が、tvOS アプリの同じ方法を使用します。


<a name="Setting_the_Build_Configuration_for_your_Application" />

### <a name="setting-the-build-configuration-for-your-application"></a>アプリケーションのビルド構成の設定

既定で、新しい Xamarin.tvOS アプリを作成するときに_ビルド構成_の両方が自動的に作成**デバッグ**と**リリース**展開します。 Apple に提出するアプリの最終ビルドを実行する前に、ベースにする必要がありますをいくつかの変更は**リリース**構成します。

次の手順で行います。

1. 右クリックし、**プロジェクト名**で、**ソリューション エクスプ ローラー**と選択**オプション**にそれらを開いて編集します。
2. TvOS の特定のバージョンを対象とする場合は、下を選択します。 **tvOS ビルド** > **iOS SDK バージョン**します。 TvOS のサポートのプレビュー リリースでは、この値に設定を残してください**既定**します。
3. リンク全体のサイズが小さく、アプリの再頒布可能を未使用のメソッド、プロパティ、クラスの除去など、ほとんどの場合は、の既定値に残しておく必要が**リンク フレームワーク SDK のみ**します。 場合など、いくつかの状況で一部の特定を使用してサード パーティ ライブラリでは、この値を設定する必要があります**リンクしない**必要な要素が削除されることを防止します。
4. Xamarin.tvOS アプリを出荷するには、LLVM 最適化コンパイラを使用する必要があります。 いることを確認、 **LLVM 最適化コンパイラを使用して、** チェック ボックスをオン、**リリース**構成します。
5. Apple では、tvOS アプリがビットコードを使用することも必要です。 もう一度、**リリース**構成では、追加`--bitcode=asmonly`を**追加 mtouch 引数**ボックス。
6. **IOS 用に最適化 PNG イメージ ファイル**これにより、さらに小さくする、アプリの成果物のサイズと、チェック ボックスを選択する必要があります。
7. デバッグする必要があります*いない*にするには、ビルドが不必要に大きくを有効にします。


<a name="Building_and_Submitting_the_Distributable" />

## <a name="building-and-submitting-the-distributable"></a>配布可能なアプリケーションのビルドと提出

Xamarin.tvOS アプリを正しく構成されている、審査およびリリースを Apple に提出する最終的な配布ビルドを行う準備ができました。

#### <a name="build-your-archive"></a>アーカイブをビルドする

1. Visual Studio for Mac で**リリースとデバイス**の構成を選択します。

    ![](app-store-publishing-images/buildxs01new.png "リリース構成を選択します。")
2. **[ビルド]** メニューから **[発行のためのアーカイブ]** を選択します。

    [![](app-store-publishing-images/buildxs02new.png "[発行のためのアーカイブ] を選択します")](app-store-publishing-images/buildxs02new.png#lightbox)
3. アーカイブが作成されると、 **[アーカイブ]** ビューが表示されます。

    [![](app-store-publishing-images/buildxs03new.png "[アーカイブ] ビュー")](app-store-publishing-images/buildxs03new.png#lightbox)

### <a name="sign-and-distribute-your-app"></a>アプリに署名して配布する

アーカイブのためにアプリケーションをビルドするたびに、*アーカイブ ビュー*が自動的に開き、アーカイブされているすべてのプロジェクトがソリューション別にグループ化されて表示されます。 既定では、このビューには現在開いているソリューションのみが表示されます。 アーカイブのあるソリューションをすべて表示するには、 **[アーカイブをすべて表示]** オプションをクリックします。

(App Store 展開またはエンタープライズ展開で) 顧客に展開したアーカイブは保存しておくことをお勧めします。そうすれば、デバッグ情報が生成された場合、後でそれを記号で表すことができます。

アプリに署名し、配布の準備をするには、次のようにします。

1. 選択、**署名し、配布しています.** 、以下を参照します。

    [![](app-store-publishing-images/buildxs04new.png "、TheSign と配布を選択しています.")](app-store-publishing-images/buildxs04new.png#lightbox)
2. これにより、発行ウィザードが開きます。 **[App Store]** 配布チャネルを選択してパッケージを作成し、アプリケーション ローダーを開きます。

    [![](app-store-publishing-images/distribute01.png "App Store の配布チャネルを選択します。")](app-store-publishing-images/distribute01.png#lightbox)
3. プロビジョニング プロファイル 画面で、署名 id と対応するプロビジョニング プロファイルを選択するか、別の id で再署名します。

    [![](app-store-publishing-images/distribute02.png "署名 id と対応するプロビジョニング プロファイルを選択します。")](app-store-publishing-images/distribute02.png#lightbox)
4. パッケージの詳細を確認し、 **[発行]** をクリックして `.ipa` パッケージを保存します。

    [![](app-store-publishing-images/distribute03.png "パッケージの詳細を確認します")](app-store-publishing-images/distribute03.png#lightbox)
5. `.ipa` が保存されたら、アプリケーション ローダーを使用して、アプリを iTunes Connect にアップロードできます。

    [![](app-store-publishing-images/distribute04.png "ITunes Connect アプリケーション ローダーを使用してにアップロード")](app-store-publishing-images/distribute04.png#lightbox)

配布ビルドが作成され、アーカイブされたら、アプリケーションを iTunes Connect に提出できます。

<a name="Submitting_Your_App_to_Apple" />

## <a name="submitting-your-app-to-apple"></a>Apple へのアプリの提出

配布ビルドが完了したら、審査のために Apple に iOS アプリケーションを提出し、App Store でリリースできます。


Visual Studio for Mac でのアーカイブ ワークフローは Application Loader を自動的に開く、保存した後、 `.ipa`:

1. *[Deliver Your App]\(アプリの配信\)* を選択して、 *[選択]* ボタンをクリックします。

    [![](app-store-publishing-images/publishvs01.png "[Deliver Your App]\(アプリの配信\) を選択します")](app-store-publishing-images/publishvs01.png#lightbox)

2. 前の手順で作成した IPA ファイルまたは zip ファイルを選択し、 **[OK]** ボタンをクリックします。
3. アプリケーション ローダーはファイルを検証します。

    [![](app-store-publishing-images/publishvs02.png "アプリケーション ローダーの検証画面")](app-store-publishing-images/publishvs02.png#lightbox)
4. *[次へ]* ボタンをクリックすると、アプリケーションは App Store に対して検証されます。

    [![](app-store-publishing-images/publishvs03.png "App Store に対して検証されるアプリケーション")](app-store-publishing-images/publishvs03.png#lightbox)
5. **[送信]** ボタンをクリックして、審査のために Apple にアプリケーションを送信します。
6. アプリケーション ローダーは、ファイルが正常にアップロードされたときに通知します。

<a name="iTunes_Connect_Status" />

### <a name="itunes-connect-status"></a>iTunes Connect のステータス

ITunes Connect に再度ログインし、使用可能なアプリの一覧からアプリを選択する場合は、iTunes Connect で状態がであるようになりました  **Waiting for Review** (一時的に読み取ることがあります**受信アップロード**処理中には)。

[![](app-store-publishing-images/image21.png "ITunes で状態の接続確認のための待機中の表示")](app-store-publishing-images/image21.png#lightbox)

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>トラブルシューティング

Xamarin.tvOS、Apple TV App Store にアプリの提出に関する問題が発生した場合を参照してください、[トラブルシューティング](~/ios/tvos/troubleshooting.md)ガイド。 発生する可能性のあるいくつかの既知の問題と、Xamarin.tvOS でそれらを解決する方法が含まれています。

<a name="Summary" />

## <a name="summary"></a>Summary

この記事では、構成、構築、および Apple TV App Store アプリを送信するためのステップ バイ ステップ ガイドを示しました。 最初に、配布プロビジョニング プロファイルの作成とインストールに必要な手順を説明しました。 次に、Visual Studio for Mac を使用して配布ビルドを作成する方法を説明しました。 最後に、その方法を示しました iTunes Connect と Xcode のアーカイブ ツールを使用して、Apple TV App Store にアプリケーションを提出します。


## <a name="related-links"></a>関連リンク

- [アイコンとイメージの使用](~/ios/tvos/app-fundamentals/icons-images.md)
- [新しい Apple TV のアプリの提出を準備します。](https://developer.apple.com/tvos/submit/)
- [App Store への提出に関するヒント](https://developer.apple.com/appstore/resources/submission/tips.html)
- [一般的なアプリケーションの却下理由](https://developer.apple.com/app-store/review/rejections/)
- [App Store の審査に関するガイドライン](https://developer.apple.com/appstore/resources/approval/guidelines.html)
