---
title: Apple TV App Store への発行
description: このドキュメントでは、Apple TV App Store にアプリを発行する方法について説明します。 Xamarin でビルドされた tvOS アプリケーションを構成、プロビジョニング、ビルド、および送信する方法について説明します。
ms.prod: xamarin
ms.assetid: 52448C93-DC19-40FA-BF8C-608AE680FF49
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: aaffaf3d66bb51d467c40bc3e5707a149154c0a7
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528735"
---
# <a name="publishing-to-the-apple-tv-app-store"></a>Apple TV App Store への発行

Apple では、すべての Apple TV デバイスにアプリケーションを配布するために、 *APPLE Tv App Store*を通じてアプリを公開する必要があります。これにより、アプリは tvOS アプリ用にワンストップショッピングを行うことができます。 多くの種類のアプリの開発者は、この単一の配布ポイントの大部分の成功に大文字と小文字を使用できます。 Apple TV App Store はターンキーソリューションであり、アプリケーション開発者が配布システムと支払いシステムの両方を提供しています。

Apple TV App Store にアプリケーションを送信するプロセスには、次のものが含まれます。

1. *アプリ ID* の作成および*権利*の選択。
2. *配布プロビジョニング プロファイル*の作成。
3. このプロファイルを使用してアプリをビルドします。
4. *ITunes Connect*を使用してアプリを送信します。


この記事では、Apple TV App Store 配布用アプリのプロビジョニング、ビルド、および送信に必要なすべての手順について説明します。

<a name="Before_you_Submit" />

## <a name="before-you-submit-an-application"></a>アプリケーションを提出する前に

アプリを Apple TV App Store に送信した後は、apple によるレビュープロセスを経て、品質とコンテンツに関する Apple のガイドラインを満たしていることを確認します。 アプリケーションがこのガイドラインを満たしていない場合、Apple はアプリケーションを却下します。その時点で、Apple によって指摘された不適合箇所の改善が必要になります。
したがって、このガイドラインをよく理解し、アプリケーションを適応させることで、Apple の審査を通過できる可能性が最も高くなります。 Apple のガイドラインは、 [App Store のレビューに関するガイドライン](https://developer.apple.com/appstore/resources/approval/guidelines.html)に従って、[新しい Apple TV のアプリの送信を準備](https://developer.apple.com/tvos/submit/)することができます。

アプリを提出する際の注意事項をいくつか以下に示します。

1. アプリの説明がアプリに含まれる機能と一致していることを確認します。
2. アプリが通常の使用でクラッシュしないことをテストします。 これには、サポートするすべての Apple TV デバイスでの使用が含まれます。


Apple では、Apple TV App Store の提出に関するヒントの一覧も保持しています。 これらは、[App Store での配布](https://developer.apple.com/appstore/resources/submission/tips.html)に関するページで確認できます。

<a name="Configuring_your_Application_in_iTunes_Connect" />

## <a name="configuring-your-application-in-itunes-connect"></a>iTunes Connect でのアプリケーションの構成

[ITunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa)は、特に、Apple TV App Store で tvOS アプリを管理するための web ベースのツールのスイートです。 TvOS アプリは、確認のために Apple に提出する前に iTunes Connect で適切に設定して構成する必要があります。最終的には、Apple TV App Store で販売または無料のアプリとしてリリースされます。

次の手順で行います。

1. 無償または販売用の iOS アプリケーションをリリースするために、iTunes Connect の「**契約/税金/口座情報**」セクションで契約が適切で最新であることを確認します。
2. アプリケーションの新しい**ITunes Connect レコード**を作成し、その**表示名**を指定します (Apple TV App Store に表示されます)。
3. **[販売価格]** を選択するか、アプリケーションを無料でリリースするかを指定します。
4. サポートしている Apple TV デバイスで、動作中のアプリケーションの**アプリストアアイコン**(大きいアイコン) とスクリーンショットを提供します。 詳細については、「[アイコンとイメージの操作](~/ios/tvos/app-fundamentals/icons-images.md)」ガイドを参照してください。
5. アプリの機能やエンドユーザーへのメリットを含む、明確で簡潔な**説明**を提供します。
6. ユーザーが Apple TV App Store でアプリを検索する際に役立つ**カテゴリ**、**サブカテゴリ**、**キーワード**を指定します。
7. Apple で必要な**連絡先**と、Web サイトの**サポート** URL を提供します。
8. Apple TV App Store で保護者による制限によって使用される、アプリケーションの**評価**を設定します。
9. **Game Center** や**アプリ内購入**など、オプションの App Store テクノロジを構成します。

詳細については、 [ITunes Connect での TvOS アプリの構成に](~/ios/tvos/deploy-test/app-distribution/itunes-connect.md)関するドキュメントを参照してください。

<a name="Preparing_for_App_Store_Distribution" />

## <a name="preparing-for-app-store-distribution"></a>App Store での配布の準備

Apple TV App Store にアプリを発行するには、まず、配布用にビルドする必要があります。これには多くの手順が含まれます。 以下のセクションでは、tvOS アプリをビルドして Apple TV App Store に提出してレビューおよびリリースできるようにするために必要なすべてのものについて説明します。

<a name="Provisioning_for_Application_Services" />

### <a name="provisioning-for-application-services"></a>アプリケーション サービスのプロビジョニング

Apple では、tvOS アプリの一意の ID を作成するときにライセンス認証を行うことができる特別なアプリケーションサービス (権利とも呼ばれます) を選択できます。 カスタム権利を使用しているかどうかにかかわらず、Apple TV App Store で公開する前に、tvOS アプリの一意の ID を作成する必要があります。

アプリ ID を作成して、必要に応じて権利を選択するには、Apple の Web ベース iOS プロビジョニング ポータルを使用して、次の手順を実行する必要があります。

1. [**プロビジョニング** > の**開発**] を選択します。
2. **+** ボタンをクリックし、新しいアプリケーションの**名前**と**バンドル ID** を指定します。
3. 画面の一番下までスクロールし、tvOS アプリで必要となる任意の**App Services**を選択します。
4. **[続行]** ボタンをクリックし、画面の指示に従って新しいアプリ ID を作成します。

アプリ id を定義するときに必要なアプリケーションサービスを選択して構成するだけでなく、ファイルと`Info.plist` `Entitlements.plist`ファイルの両方を編集して、tvOS プロジェクトのアプリ id と権利を構成する必要もあります。

Visual Studio for Mac で次の操作を行います。

1. **ソリューション エクスプローラー**で `Info.plist` ファイルをダブルクリックして、編集用に開きます。
2. **[TvOS Application Target]** セクションで、アプリケーションの名前を入力し、アプリ ID を定義したときに作成した**バンドル id**を入力します。
3. 変更内容を `Info.plist` ファイルに保存します。
4. **ソリューション エクスプローラー**で `Entitlements.plist` ファイルをダブルクリックして、編集用に開きます。
5. アプリ ID を定義したときに上記で実行したセットアップと一致するように、tvOS アプリに必要な資格を選択して構成します。
6. 変更内容を `Entitlements.plist` ファイルに保存します。

詳しい手順については、[App Services のプロビジョニング](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices)に関するドキュメントを参照してください。 このドキュメントは iOS 用に記述されていますが、tvOS アプリのプロビジョニングにも同じ手順を使用します。

<a name="Setting_the_Apps_Icons_and_Launch_Screens" />

### <a name="setting-the-apps-icons-launch-image-and-top-shelf-image"></a>アプリアイコン、起動イメージ、およびトップシェルフイメージの設定

Apple TV App Store に含まれるように Apple によって tvOS アプリが受け入れられるようにするには、実行されるすべての Apple TV デバイスに対して、適切なアイコン、起動、および上部の棚イメージが必要です。 ITunes Connect にアップロードする前に、 `Assets.car`ファイルにコンパイルされ、tvOS アプリのバンドルに含まれる、必要なイメージアセットを追加する必要があります。

詳細な手順については、[アイコンとイメージの操作](~/ios/tvos/app-fundamentals/icons-images.md)に関するドキュメントを参照してください。

<a name="Creating_and_Installing_a_Distribution_Profile" />

### <a name="creating-and-installing-a-distribution-profile"></a>配布プロファイルの作成とインストール

tvOS では、*プロビジョニングプロファイル*を使用して、特定のアプリケーションのビルドを展開する方法を制御します。 これらは、アプリの署名に使用される証明書、*アプリケーション ID*、およびアプリをインストールできる場所に関する情報を含むファイルです。 開発とアドホック配布の場合、プロビジョニング プロファイルには、アプリを展開できる許可デバイスのリストも含まれます。 ただし、Apple TV App Store で配布する場合は、証明書とアプリ ID の情報のみが含まれます。これは、公開配布の唯一のメカニズムが Apple TV App Store を介して行われるためです。

プロビジョニングには、Apple の Web ベース iOS プロビジョニング プロファイルを使用する以下の手順が必要です。

1. **[プロビジョニング]**  >  **[配布]** の順に選択します。
2. ボタンをクリックし、 **Apple TV App Store**として作成する配布プロファイルの種類を選択します。 **+**
3. 配布プロファイルを作成する**アプリ ID** をドロップダウン リストから選択します。
4. アプリケーションに署名するために必要な証明書を選択します。
5. 新しい**配布プロファイル**の**名前**を入力して、プロファイルを生成します。
6. Xcode で利用可能なプロファイルの一覧を更新します。
7. **アプリストア**の_ビルド構成_について、Visual Studio で配布プロビジョニングプロファイルを選択します。

詳しい手順については、[配布プロファイルの作成](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#creatingprofile)と [Xamarin.iOS プロジェクトでの配布プロファイルの選択](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile)に関するページを参照してください。 ここでも、これらのドキュメントはどちらも iOS に固有のものですが、tvOS アプリでも同じ手法が使用されます。


<a name="Setting_the_Build_Configuration_for_your_Application" />

### <a name="setting-the-build-configuration-for-your-application"></a>アプリケーションのビルド構成の設定

既定では、新しい tvOS アプリを作成すると、**デバッグ**配置と**リリース**配置の両方に対して_ビルド構成_が自動的に作成されます。 Apple に提出するアプリの最終ビルドを実行する前に、基本**リリース**構成に加える必要がある変更がいくつかあります。

次の手順で行います。

1. [**ソリューションエクスプローラー**と選択 **] オプション**で**プロジェクト名**を右クリックして、編集用に開きます。
2. TvOS の特定のバージョンを対象としている場合は、[ **tvOS** > ] **[iOS SDK バージョン]** の下で選択します。 TvOS サポートのプレビューリリースでは、この値を**既定**値のままにしてください。
3. リンクを使用すると、未使用のメソッド、プロパティ、クラスなどを除去することによって、アプリの再頒布可能な全体のサイズを削減できます。ほとんどの場合、既定値の **[Link FRAMEWORK SDK のみ]** のままにしておく必要があります。 一部の特定のサードパーティライブラリを使用する場合など、状況によっては、必要な要素が削除されるのを防ぐために、この値を **[リンクしない]** に設定する必要があります。
4. TvOS アプリを出荷するには、LLVM 最適化コンパイラを使用する必要があります。 **リリース**構成の下にある [ **llvm 最適化コンパイラを使用**する] チェックボックスがオンになっていることを確認します。
5. Apple では、tvOS アプリで bitcode を使用する必要もあります。 **リリース**構成の下で、[ `--bitcode=asmonly`追加の**mtouch 引数**] ボックスにを追加します。
6. アプリの成果物のサイズをさらに小さくするために、[ **iOS 用 PNG イメージファイルを最適化**する] チェックボックスをオンにする必要があります。
7. デバッグを有効にし*ない*でください。ビルドが不必要に大きくなります。


<a name="Building_and_Submitting_the_Distributable" />

## <a name="building-and-submitting-the-distributable"></a>配布可能なアプリケーションのビルドと提出

TvOS アプリが適切に構成されたら、レビューとリリースのために Apple に提出する最終的なディストリビューションビルドを行う準備が整いました。

#### <a name="build-your-archive"></a>アーカイブをビルドする

1. Visual Studio for Mac で**リリースとデバイス**の構成を選択します。

    ![](app-store-publishing-images/buildxs01new.png "リリース構成を選択します")
2. **[ビルド]** メニューから **[発行のためのアーカイブ]** を選択します。

    [![](app-store-publishing-images/buildxs02new.png "[発行のためのアーカイブ] を選択します")](app-store-publishing-images/buildxs02new.png#lightbox)
3. アーカイブが作成されると、 **[アーカイブ]** ビューが表示されます。

    [![](app-store-publishing-images/buildxs03new.png "アーカイブビュー")](app-store-publishing-images/buildxs03new.png#lightbox)

### <a name="sign-and-distribute-your-app"></a>アプリに署名して配布する

アーカイブのためにアプリケーションをビルドするたびに、*アーカイブ ビュー*が自動的に開き、アーカイブされているすべてのプロジェクトがソリューション別にグループ化されて表示されます。 既定では、このビューには現在開いているソリューションのみが表示されます。 アーカイブのあるソリューションをすべて表示するには、 **[アーカイブをすべて表示]** オプションをクリックします。

(App Store 展開またはエンタープライズ展開で) 顧客に展開したアーカイブは保存しておくことをお勧めします。そうすれば、デバッグ情報が生成された場合、後でそれを記号で表すことができます。

アプリに署名し、配布の準備をするには、次のようにします。

1. 次に示すように、 **[署名と配布...]** を選択します。

    [![](app-store-publishing-images/buildxs04new.png "で、[theSign と配布...] を選択します。")](app-store-publishing-images/buildxs04new.png#lightbox)
2. これにより、発行ウィザードが開きます。 **[App Store]** 配布チャネルを選択してパッケージを作成し、アプリケーション ローダーを開きます。

    [![](app-store-publishing-images/distribute01.png "App Store の配布チャネルを選択します")](app-store-publishing-images/distribute01.png#lightbox)
3. [プロビジョニングプロファイル] 画面で、署名 id と対応するプロビジョニングプロファイルを選択するか、別の id で再署名します。

    [![](app-store-publishing-images/distribute02.png "署名 id と対応するプロビジョニングプロファイルを選択します")](app-store-publishing-images/distribute02.png#lightbox)
4. パッケージの詳細を確認し、 **[発行]** をクリックして `.ipa` パッケージを保存します。

    [![](app-store-publishing-images/distribute03.png "パッケージの詳細を確認する")](app-store-publishing-images/distribute03.png#lightbox)
5. `.ipa` が保存されたら、アプリケーション ローダーを使用して、アプリを iTunes Connect にアップロードできます。

    [![](app-store-publishing-images/distribute04.png "アプリケーションローダーを介して iTunes Connect にアップロードされました")](app-store-publishing-images/distribute04.png#lightbox)

配布ビルドが作成され、アーカイブされたら、アプリケーションを iTunes Connect に提出できます。

<a name="Submitting_Your_App_to_Apple" />

## <a name="submitting-your-app-to-apple"></a>Apple へのアプリの提出

配布ビルドが完了したら、審査のために Apple に iOS アプリケーションを提出し、App Store でリリースできます。


を保存`.ipa`すると、Visual Studio for Mac のアーカイブワークフローによってアプリケーションローダーが自動的に開きます。

1. *[Deliver Your App]\(アプリの配信\)* を選択して、 *[選択]* ボタンをクリックします。

    [![](app-store-publishing-images/publishvs01.png "[Deliver Your App]\(アプリの配信\) を選択します")](app-store-publishing-images/publishvs01.png#lightbox)

2. 前の手順で作成した IPA ファイルまたは zip ファイルを選択し、 **[OK]** ボタンをクリックします。
3. アプリケーション ローダーはファイルを検証します。

    [![](app-store-publishing-images/publishvs02.png "アプリケーションローダーの検証画面")](app-store-publishing-images/publishvs02.png#lightbox)
4. *[次へ]* ボタンをクリックすると、アプリケーションは App Store に対して検証されます。

    [![](app-store-publishing-images/publishvs03.png "アプリストアに対して検証されているアプリケーション")](app-store-publishing-images/publishvs03.png#lightbox)
5. **[送信]** ボタンをクリックして、審査のために Apple にアプリケーションを送信します。
6. アプリケーション ローダーは、ファイルが正常にアップロードされたときに通知します。

<a name="iTunes_Connect_Status" />

### <a name="itunes-connect-status"></a>iTunes Connect のステータス

ITunes Connect にログインして、使用可能なアプリの一覧からアプリを選択した場合、iTunes Connect の状態は、**確認を待機**していることを示します (処理中に**受信したアップロード**を一時的に読み取ることができます)。

[![](app-store-publishing-images/image21.png "確認待ちの iTunes Connect の状態")](app-store-publishing-images/image21.png#lightbox)

<a name="Troubleshooting" />

## <a name="troubleshooting"></a>トラブルシューティング

TvOS アプリを Apple TV App Store に送信する際に問題が発生した場合は、[トラブルシューティング](~/ios/tvos/troubleshooting.md)ガイドを参照してください。 これには、発生する可能性のある既知の問題と、それらを tvOS で解決する方法が含まれています。

<a name="Summary" />

## <a name="summary"></a>Summary

この記事では、Apple TV App Store 発行用アプリの構成、ビルド、および提出に関するステップバイステップガイドを紹介しています。 最初に、配布プロビジョニング プロファイルの作成とインストールに必要な手順を説明しました。 次に、Visual Studio for Mac を使用して配布ビルドを作成する方法を説明しました。 最後に、iTunes Connect と Xcode Archive ツールを使用して、Apple TV App Store にアプリケーションを送信する方法について説明しました。


## <a name="related-links"></a>関連リンク

- [アイコンとイメージの使用](~/ios/tvos/app-fundamentals/icons-images.md)
- [新しい Apple TV のアプリの送信を準備する](https://developer.apple.com/tvos/submit/)
- [App Store への提出に関するヒント](https://developer.apple.com/appstore/resources/submission/tips.html)
- [一般的なアプリケーションの却下理由](https://developer.apple.com/app-store/review/rejections/)
- [App Store の審査に関するガイドライン](https://developer.apple.com/appstore/resources/approval/guidelines.html)
