---
title: App Store に Xamarin.iOS アプリを公開する
description: このドキュメントでは、App Store で配布する Xamarin.iOS アプリケーションの構成、ビルド、発行の方法を示します。
ms.prod: xamarin
ms.assetid: DFBCC0BA-D233-4DC4-8545-AFBD3768C3B9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/23/2017
ms.openlocfilehash: 489d9fa569b083f5cb655dc503ab4fa551810b6d
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209475"
---
# <a name="publishing-xamarinios-apps-to-the-app-store"></a>App Store に Xamarin.iOS アプリを公開する

> [!IMPORTANT]
> Apple は、2018 年 7 月以降に App Store に提出されるすべてのアプリおよび更新プログラムが iOS 11 SDK でビルドされ、[iPhone X ディスプレイをサポートする](~/ios/platform/introduction-to-ios11/updating-your-app/visual-design.md)必要があることを[通知しました](https://developer.apple.com/news/?id=05072018a)。

アプリケーションをすべての iOS デバイスに配布するには、Apple ではアプリを *App Store* から発行する必要があります。これにより、App Store で iOS アプリケーションのワンストップ ショッピングが可能になります。 ストアには 500,000 を超えるアプリケーションがあり、多くの種類のアプリケーション開発者は、1 か所に配布することで大きな利益を得ています。 App Store はすぐに使用可能なソリューションであり、配布と支払いシステムの両方をアプリ開発者に提供します。

App Store へのアプリケーションの提出プロセスは次のとおりです。

1. **アプリ ID** の作成および**権利**の選択。
2. **配布プロビジョニング プロファイル**の作成。
3. このプロファイルを使用したアプリケーションのビルド。
4. **iTunes Connect** を使用したアプリケーションの提出。


この記事には、App Store に配布するアプリケーションのプロビジョニング、ビルド、および提出に必要なすべての手順が含まれます。

## <a name="before-you-submit-an-application"></a>アプリケーションを提出する前に

App Store に配布するアプリを提出した後、Apple の品質およびコンテンツに関するガイドラインを満たしているかどうかを確認するための審査プロセスを経ることになります。 アプリケーションがこのガイドラインを満たしていない場合、Apple はアプリケーションを却下します。その時点で、Apple によって指摘された不適合箇所の改善が必要になります。
したがって、このガイドラインをよく理解し、アプリケーションを適応させることで、Apple の審査を通過できる可能性が最も高くなります。 Apple のガイドラインは、「[App Store 審査ガイドライン](https://developer.apple.com/appstore/resources/approval/guidelines.html)」で確認できます。

アプリを提出する際の注意事項をいくつか以下に示します。

1. アプリケーションの説明がアプリに含まれる機能と一致していることを確認してください。
2. アプリケーションが通常の使用でクラッシュしないことをテストしてください。 これには、サポートするすべての iOS デバイスでの使用が含まれます。


Apple では、App Store に提出する際のヒントのリストも保持されています。 これらは、[App Store での配布](https://developer.apple.com/appstore/resources/submission/tips.html)に関するページで確認できます。

## <a name="configuring-your-application-in-itunes-connect"></a>iTunes Connect でのアプリケーションの構成

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) は、特に App Store で iOS アプリケーションを管理するための Web ベースのツール群です。 Xamarin.iOS アプリケーションを審査のために Apple に提出し、最終的に App Store で販売または無償アプリとしてリリースする前に、アプリケーションを正しく設定して、iTunes Connect で構成する必要があります。

次の手順で行います。

1. 無償または販売用の iOS アプリケーションをリリースするために、iTunes Connect の「**契約/税金/口座情報**」セクションで契約が適切で最新であることを確認します。
2. アプリケーションの新しい **iTunes Connect レコード**を作成し、その**表示名** (App Store で表示される) を指定します。
3. **[販売価格]** を選択するか、アプリケーションを無料でリリースするかを指定します。
5. 機能やエンドユーザーへのメリットを含む、アプリケーションの明確で簡潔な**説明**を提供します。
6. ユーザーが App Store でアプリを見つけやすくするため、**カテゴリ**、**サブカテゴリ**、および**キーワード**を提供します。
7. Apple で必要な**連絡先**と、Web サイトの**サポート** URL を提供します。
8. App Store で保護者による規制で使用されるアプリケーションの**評価**を設定します。
9. **Game Center** や**アプリ内購入**など、オプションの App Store テクノロジを構成します。

詳細については、「[Configuring an App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)」 (iTunes Connect でのアプリの構成) ドキュメントを参照してください。

## <a name="preparing-for-app-store-distribution"></a>App Store での配布の準備

App Store にアプリケーションを公開するには、まず、配布するアプリケーションをビルドする必要があります。これには、多くの手順が必要です。 次のセクションでは、Xamarin.iOS アプリケーションをビルドして、審査およびリリースのために App Store に提出できるようにするために、アプリケーションの公開準備に必要なすべてのものを示します。

### <a name="provisioning-for-application-services"></a>アプリケーション サービスのプロビジョニング

Apple では、iOS アプリケーションの一意の ID を作成する際にアクティブ化できる、権利とも呼ばれる特別なアプリケーション サービスの選択肢を提供します。 カスタムの権利を使用するかどうかに関係なく、App Store で公開する前に、Xamarin.iOS アプリケーションの一意の ID を作成する必要があります。

アプリ ID を作成して、必要に応じて権利を選択するには、Apple の Web ベース iOS プロビジョニング ポータルを使用して、次の手順を実行する必要があります。

1. **[Certificates, Identifiers & Profiles]\(証明書、ID およびプロファイル\)** セクションで、**[識別子]** > **[アプリ ID]** の順に選択します。
2. **+** ボタンをクリックし、新しいアプリケーションの**名前**と**バンドル ID** を指定します。
3. 画面の下部までスクロールし、Xamarin.iOS アプリケーションで必要になる **App Services** を選択します。
4. **[続行]** ボタンをクリックし、画面の指示に従って新しいアプリ ID を作成します。

アプリ ID を定義する際に必要なアプリケーション サービスを選択して構成するだけでなく、アプリ ID と権利を Xamarin.iOS プロジェクトで構成する必要もあります。その場合は、`Info.plist` と `Entitlements.plist` の両方のファイルを編集します。

次の手順で行います。

1. **ソリューション エクスプローラー**で `Info.plist` ファイルをダブルクリックして、編集用に開きます。
2. **[iOS アプリケーションのターゲット]** セクションで、アプリケーションの名前を入力し、アプリ ID を定義したときに作成した**バンドル ID** を入力します。
3. 変更内容を `Info.plist` ファイルに保存します。
4. **ソリューション エクスプローラー**で `Entitlements.plist` ファイルをダブルクリックして、編集用に開きます。
5. Xamarin.iOS アプリケーションに必要な権利を選択して構成し、アプリ ID を定義したときに行った前述のセットアップと一致するようにします。
6. 変更内容を `Entitlements.plist` ファイルに保存します。

詳しい手順については、[App Services のプロビジョニング](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices)に関するドキュメントを参照してください。

### <a name="setting-the-store-icons"></a>ストア アイコンの設定

アセット カタログによってアプリケーション ストア アイコンが配信されるようになりました。 App Store アイコンを追加するには、まずプロジェクトの **Assets.xcassets** ファイルで **AppIcon** イメージ セットを見つけます。

アセット カタログに必要なアイコンは、名前が **App Store** で、サイズは **1024 x 1024** です。 Apple では、アセット カタログ内の App Store アイコンは、透明だったり、アルファ チャネルを含んだりすることはできないと規定されています。

ストア アイコンの設定の詳細については、[App Store アイコン](~/ios/app-fundamentals/images-icons/app-store-icon.md) ガイドをご覧ください。

### <a name="setting-the-apps-icons-and-launch-screens"></a>アプリ アイコンと起動画面の設定

iOS アプリケーションが Apple に受け入れられ、App Store に含まれるようにするには、アプリケーションを実行するすべての iOS デバイスの適切なアイコンと起動画面が必要になります。 アプリ アイコンは、**Assets.xcassets** ファイルの **AppIcon** イメージ セットを介して、アセット カタログのプロジェクトに追加します。 起動画面は、ストーリーボードを介して追加します。

アプリ アイコンと起動画面を作成する手順の詳細については、[アプリケーション アイコン](~/ios/app-fundamentals/images-icons/app-icons.md)と[起動画面](~/ios/app-fundamentals/images-icons/launch-screens.md)のガイドをご覧ください。

### <a name="creating-and-installing-a-distribution-profile"></a>配布プロファイルの作成とインストール

iOS では*プロビジョニング プロファイル*を使用して、特定のアプリケーション ビルドの展開方法を制御します。 これらは、アプリの署名に使用される証明書、*アプリケーション ID*、およびアプリをインストールできる場所に関する情報を含むファイルです。 開発とアドホック配布の場合、プロビジョニング プロファイルには、アプリを展開できる許可デバイスのリストも含まれます。 ただし、App Store での配布の場合は、証明書とアプリ ID の情報のみが含まれます。これは、App Store を介してのみ一般に配布されるためです。

プロビジョニングには、Apple の Web ベース iOS プロビジョニング プロファイルを使用する以下の手順が必要です。

1.  **[プロビジョニング]** > **[配布]** の順に選択します。
2.  **+** ボタンをクリックし、**App Store** として作成する配布プロファイルの種類を選択します。
3.  配布プロファイルを作成する**アプリ ID** をドロップダウン リストから選択します。
4.  アプリケーションに署名するために有効な運用 (配布) 証明書を選択します。
5.  新しい**配布プロファイル**の**名前**を入力して、プロファイルを生成します。
6.  Mac で Xcode を開き、**[基本設定] を参照し、Apple ID を選択して、[詳細の表示...]** をクリックします。Mac で、Xcode で使用可能なすべてのプロファイルをダウンロードします。
7.  IDE に戻り、**[iOS バンドル署名]** オプションで、適切な**ビルド構成**の配布プロビジョニング プロファイルを選択します (これは、**App Store** または_リリース_のいずれかになります)。

詳しい手順については、[配布プロファイルの作成](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile)と [Xamarin.iOS プロジェクトでの配布プロファイルの選択](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile)に関するページを参照してください。

## <a name="setting-the-build-configuration-for-your-application"></a>アプリケーションのビルド構成の設定

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

次の手順で行います。

1. **Solution Pad** で **[プロジェクト名]** を右クリックし、**[オプション]** を選択して編集用に開きます。
2. **[iOS ビルド]** を選択し、**[構成]** ドロップダウン リストから **[Release | iPhone]\(リリース | iPhone\)** を選択します。

    ![](publishing-to-the-app-store-images/configurevs01.png "[構成] ドロップダウン リストから [AppStore] を選択します")

3. 対象となる特定の iOS バージョンがある場合は、**[SDK バージョン]** からそれを選択します。それ以外の場合は、この値を既定の設定のままにしておきます。
4. リンクを設定し、未使用のメソッド、プロパティ、クラスなどを削除することで、配布可能なアプリケーションの全体のサイズが小さくなります。ほとんどの場合、既定値の **[SDK アセンブリのみをリンクする]** のままにしておきます。 状況によっては (一部の特定のサードパーティ製のライブラリを使用する場合など)、必要な要素が削除されないように、この値を強制的に **[リンクしない]** に設定する必要がある場合があります。 詳細については、「[iOS Build Mechanics](~/ios/deploy-test/ios-build-mechanics.md)」 (iOS ビルドのしくみ) ガイドを参照してください。
5. **[iOS 用に PNG イメージ ファイルを最適化する]** チェック ボックスをオンにする必要があります。そうすれば、アプリケーションの配布可能サイズをさらに小さくすることができます。
6. デバッグはビルド サイズが不必要に大きくなるため、有効に_しない_でください。
8. iOS 11 の場合は、**ARM64** をサポートするデバイスのアーキテクチャの 1 つを選択する必要があります。 64 ビット iOS デバイスのビルドの詳細については、「[32/64 bit Platform Considerations](~/cross-platform/macios/32-and-64/index.md)」 (32/64 ビット プラットフォームの考慮事項) ドキュメントの「**Enabling 64 Bit Builds of Xamarin.iOS Apps**」 (Xamarin.iOS アプリの 64 ビット ビルドの有効化) セクションを参照してください。
9. 必要に応じて、より小さな高速コードを作成する **LLVM** コンパイラを使用することはできますが、コンパイルに時間がかかります。
10. アプリケーションのニーズに基づいて、**国際化**対応のために使用および設定する**ガベージ コレクション**の種類を調整することもできます。
11. ビルド構成の変更内容を保存します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

既定では、Visual Studio で新しい Xamarin.iOS アプリケーションを作成するときに、**アドホック**と **App Store** の両方の展開について、_ビルド構成_が自動的に作成されます。 Apple に提出するアプリケーションの最終ビルドを行う前に、基本構成をいくつか変更する必要があります。

次の手順で行います。

1. **ソリューション エクスプローラー**で **[プロジェクト名]** を右クリックし、**[プロパティ]** を選択して編集用に開きます。
2. **[構成]** ドロップダウンから、**[iOS ビルド]** と **[AppStore]** (または、AppStore を利用できない場合は **[リリース]**) を選択します。

    ![](publishing-to-the-app-store-images/configurevs01.png "[構成] ドロップダウン リストから [AppStore] を選択します")

3. 対象となる特定の iOS バージョンがある場合は、**[SDK バージョン]** からそれを選択します。それ以外の場合は、この値を既定の設定のままにしておきます。
4. リンクを設定し、未使用のメソッド、プロパティ、クラスなどを削除することで、配布可能なアプリケーションの全体のサイズが小さくなります。ほとんどの場合、既定値の **[SDK アセンブリのみをリンクする]** のままにしておきます。 状況によっては (一部の特定のサードパーティ製のライブラリを使用する場合など)、必要な要素が削除されないように、この値を強制的に **[リンクしない]** に設定する必要がある場合があります。 詳細については、「[iOS Build Mechanics](~/ios/deploy-test/ios-build-mechanics.md)」 (iOS ビルドのしくみ) ガイドを参照してください。
5. **[iOS 用に PNG イメージ ファイルを最適化する]** チェック ボックスをオンにする必要があります。そうすれば、アプリケーションの配布可能サイズをさらに小さくすることができます。
6. デバッグはビルド サイズが不必要に大きくなるため、有効に_しない_でください。
7. **[詳細設定]** タブをクリックします。

    ![](publishing-to-the-app-store-images/configurevs02.png "[詳細設定] タブ")

8. Xamarin.iOS アプリケーションの対象が iOS 8 および 64 ビット iOS デバイスである場合は、**ARM64** をサポートするデバイス アーキテクチャのいずれかを選択する必要があります。 64 ビット iOS デバイスのビルドの詳細については、「[32/64 bit Platform Considerations](~/cross-platform/macios/32-and-64/index.md)」 (32/64 ビット プラットフォームの考慮事項) ドキュメントの「**Enabling 64 Bit Builds of Xamarin.iOS Apps**」 (Xamarin.iOS アプリの 64 ビット ビルドの有効化) セクションを参照してください。
9. 必要に応じて、より小さな高速コードを作成する **LLVM** コンパイラを使用することはできますが、コンパイルに時間がかかります。
10. アプリケーションのニーズに基づいて、**国際化**対応のために使用および設定する**ガベージ コレクション**の種類を調整することもできます。
11. ビルド構成の変更内容を保存します。

-----

## <a name="building-and-submitting-the-distributable"></a>配布可能なアプリケーションのビルドと提出

正しく構成された Xamarin.iOS アプリケーションを使用して、審査およびリリースのために Apple に提出する最終的な配布ビルドを行う準備ができました。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

### <a name="build-your-archive"></a>アーカイブをビルドする

1. Visual Studio for Mac で**リリースとデバイス**の構成を選択します。

    ![](publishing-to-the-app-store-images/buildxs01new.png "リリースの選択 | デバイスの構成")
1. **[ビルド]** メニューから **[発行のためのアーカイブ]** を選択します。

    ![](publishing-to-the-app-store-images/buildxs02new.png "[発行のためのアーカイブ] を選択します")

1. アーカイブが作成されると、**[アーカイブ]** ビューが表示されます。

    ![](publishing-to-the-app-store-images/buildxs03new.png "[アーカイブ] ビューが表示されます")


> [!NOTE]
> 古い _App Store_ と "_アドホック_" の構成がすべての Visual Studio for Mac テンプレート プロジェクトから削除されましたが、古いプロジェクトにまだこれらの構成が含まれている場合があります。 その場合は、引き続き、上記の手順 1 で **App Store とデバイス**の構成を使用できます。

### <a name="sign-and-distribute-your-app"></a>アプリに署名して配布する

 アーカイブのためにアプリケーションをビルドするたびに、**アーカイブ ビュー**が自動的に開き、アーカイブされているすべてのプロジェクトがソリューション別にグループ化されて表示されます。 既定では、このビューには現在開いているソリューションのみが表示されます。 アーカイブのあるソリューションをすべて表示するには、**[アーカイブをすべて表示]** オプションをクリックします。

 (App Store 展開またはエンタープライズ展開で) 顧客に展開したアーカイブは保存しておくことをお勧めします。そうすれば、デバッグ情報が生成された場合、後でそれを記号で表すことができます。

 アプリに署名し、配布の準備をするには、次のようにします。


1. 下の画像のように、**[署名と配布...]** を選択します。

    ![](publishing-to-the-app-store-images/buildxs04new.png "[署名と配布] を選択します")

1. これにより、発行ウィザードが開きます。 **[App Store]** 配布チャネルを選択してパッケージを作成し、アプリケーション ローダーを開きます。

    ![](publishing-to-the-app-store-images/distribute01.png "アプリケーション ローダーを開きます")

1. [プロビジョニング プロファイル] 画面で、署名 ID と対応するプロビジョニング プロファイルを選択するか、別の ID で再署名します。

    ![](publishing-to-the-app-store-images/distribute02.png "署名 ID と対応するプロビジョニング プロファイルを選択します")

1. パッケージの詳細を確認し、**[発行]** をクリックして `.ipa` パッケージを保存します。

    ![](publishing-to-the-app-store-images/distribute03.png "パッケージの詳細を確認します")

1. `.ipa` が保存されたら、アプリケーション ローダーを使用して、アプリを iTunes Connect にアップロードできます。

    ![](publishing-to-the-app-store-images/distribute04.png "[Publication Succeeded]\(発行に成功しました\) 画面")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio の Xamarin プラグインでは現在、App Store に iOS アプリケーションを公開するためのアーカイブ ワークフローはサポートされていません。 そのため、以下の説明のとおり、**[アドホック/エンタープライズ パッケージ (IPA) をビルドする]** コマンドで作成された IPA をアップロードします。


## <a name="build-an-ipa"></a>IPA をビルドする

 このセクションでは、アドホックまたはエンタープライズ配布を使用する場合のワークフローと同様の、IPA のビルドについて説明します。 ただし、署名は、前の手順で作成された App Store プロビジョニング プロファイルを使用して行われます。

 次の手順で行います。

1. **ソリューション エクスプローラー**で、Xamarin.iOS プロジェクト名を右クリックし、**[プロパティ]** を選択して編集用に開きます。

    ![](publishing-to-the-app-store-images/imagevs01.png "[プロパティ] を選択します")
1. **[iOS バンドル署名]** を選択し、プロビジョニング プロファイルを App Store プロビジョニング プロファイルに変更します。

    ![](publishing-to-the-app-store-images/ipa01.png "[iOS バンドル署名] を選択し、プロビジョニング プロファイルを App Store プロビジョニング プロファイルに変更します")
1. **[iOS IPA オプション]、[構成]、[アドホック]** の順に選択し (**[アドホック]** が表示されない場合は、代わりに **[リリース]** を選択します)、IPA ファイルをビルドするためのチェック ボックスをオンにします。

    ![](publishing-to-the-app-store-images/imagevs02.png "[構成] ドロップダウン リストから [アドホック] を選択します")

1. 必要に応じて、IPA に **[パッケージ名]** を指定できます。指定しない場合、Xamarin.iOS プロジェクトと同じ名前が付けられます。
1. プロジェクト プロパティに変更を保存します。
1. Visual Studio for Mac の **[ビルド構成]** ドロップダウン リストから **[アドホック]** を選択します。

    ![](publishing-to-the-app-store-images/imagevs05.png "[ビルド構成] ドロップダウン リストから [アドホック] を選択します")
1. プロジェクトをビルドし、IPA パッケージを作成します。
1. `Bin` > _iOS デバイス_ > `Ad Hoc` フォルダーに IPA がビルドされます。

    ![](publishing-to-the-app-store-images/imagevs06.png "エクスプローラーに表示される IPA")

-----


## <a name="customizing-the-ipa-location"></a>IPA の場所のカスタマイズ

新しい **MSBuild** プロパティ `IpaPackageDir` が追加され、`.ipa` ファイルの出力場所を簡単にカスタマイズできるようになりました。 `IpaPackageDir` がカスタムの場所に設定されている場合、タイムスタンプが付いた既定のサブディレクトリではなく、その場所に `.ipa` ファイルが置かれます。 このような変更は、継続的インテグレーション (CI) ビルドに利用される自動化ビルドのように、正常に動作するために特定のディレクトリ パスに依存する自動化ビルドを作成する際に便利な場合があります。

新しいプロパティの利用はいくつかの方法で利用される可能性があります。

たとえば、(Xamarin.iOS 9.6 以前の) 古い既定のディレクトリに `.ipa` ファイルを出力する場合は、次のいずれかの手法を使用して `IpaPackageDir` プロパティを `$(OutputPath)` に設定できます。 いずれの手法も、IDE ビルドや、`xbuild`、`msbuild`、または `mdtool` を使用するコマンド ライン ビルドを含む、すべての Unified API Xamarin.iOS ビルドと互換性があります。

  - 最初のオプションでは、**MSBuild** ファイルの `<PropertyGroup>` 要素内で `IpaPackageDir` プロパティを設定します。 たとえば、次の `<PropertyGroup>` を iOS アプリ プロジェクトの `.csproj` ファイルの一番下に追加できます (終了タグ `</Project>` の直前)。

      ```xml
        <PropertyGroup>
            <IpaPackageDir>$(OutputPath)</IpaPackageDir>
        </PropertyGroup>
      ```
  - もっと良い手法は、`.ipa` ファイルのビルドに使用される構成に対応する既存の `<PropertyGroup>` の一番下に `<IpaPackageDir>` 要素を追加することです。 この方法が優れているのは、iOS IPA オプション プロジェクト プロパティ ページで予定されている設定との将来的な互換性がプロジェクトに与えられるためです。 現在、`Release|iPhone` 構成を使用して `.ipa` ファイルをビルドしている場合、更新されたプロパティ グループは次のようになります。

      ```xml
      <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone' )
        <Optimize>true</Optimize>
        <OutputPath>bin\iPhone\Release</OutputPath>
        <ErrorReport>prompt</ErrorReport>
        <WarningLevel>4</WarningLevel>
        <ConsolePause>false</ConsolePause>
        <CodesignKey>iPhone Developer</CodesignKey>
        <MtouchUseSGen>true</MtouchUseSGen>
        <MtouchUseRefCounting>true</MtouchUseRefCounting>
        <MtouchFloat32>true</MtouchFloat32>
        <CodesignEntitlements>Entitlements.plist</CodesignEntitlements>
        <MtouchLink>SdkOnly</MtouchLink>
        <MtouchArch>;ARMv7, ARM64</MtouchArch>
        <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
        <MtouchTlsProvider>Default</MtouchTlsProvider>
        <PlatformTarget>x86&</PlatformTarget>
        <BuildIpa>true</BuildIpa>
        <IpaPackageDir>$(OutputPath</IpaPackageDir>
      </PropertyGroup>
      ```
コマンド ライン ビルドの `msbuild` または `xbuild` のための代替手法は、`/p:` コマンド ライン引数を追加し、`IpaPackageDir` プロパティを設定することです。 この場合、`msbuild` はコマンド ラインに渡される `$()` 式を展開しません。そのため、`$(OutputPath)` 構文は使用できません。 代わりに、完全パス名を指定する必要があります。 Mono の `xbuild` コマンドは `$()` 式を展開しますが、それでも完全パス名の使用が推奨されます。`xbuild` は最終的に非推奨とされ、今後のリリースでは[クロスプラットフォーム バージョンの `msbuild`](http://www.mono-project.com/docs/about-mono/releases/4.4.0/#msbuild-preview-for-os-x) が使用されるようになるためです。 この手法を Windows で行うと次のようになります。

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

Mac の場合は次のようになります。

```bash
xbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

配布ビルドが作成され、アーカイブされたら、アプリケーションを iTunes Connect に提出できます。

### <a name="automatically-copy-app-bundles-back-to-windows"></a>.app バンドルを自動的に Windows にコピーする

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]

## <a name="submitting-your-app-to-apple"></a>Apple へのアプリの提出

> [!NOTE]
> Apple では最近 iOS アプリケーションの検証プロセスが変更されたため、IPA に `iTunesMetadata.plist` が含まれるアプリは却下される可能性があります。 `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"` というエラーが発生した場合は、[ここ](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1)に示されている回避策で問題が解決されるはずです。

配布ビルドが完了したら、審査のために Apple に iOS アプリケーションを提出し、App Store でリリースできます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

 次の手順で行います。

1. **Xcode** を起動します。
2. **[ウィンドウ]** メニューから、**[オーガナイザー]** を選択します。
3. **[アーカイブ]** タブをクリックし、ビルド済みのアーカイブを選択します。

    ![](publishing-to-the-app-store-images/publishxs01.png "送信するアーカイブを選択します")
4. **[検証...]** ボタンをクリックします。
5. 検証対象のアカウントを選択し、**[選択]** ボタンをクリックします。

    ![](publishing-to-the-app-store-images/publishxs02.png "検証対象のアカウントを選択します")
6. **[検証]** ボタンをクリックします。

    ![](publishing-to-the-app-store-images/publishxs03.png "ファイルの概要ダイアログ")
7. バンドルに問題があった場合は、それが表示されます。
8. 問題を修正し、Visual Studio for Mac でアーカイブをリビルドします。
9. **[提出...]** ボタンを選択します。
10. 検証対象のアカウントを選択し、**[選択]** ボタンをクリックします。

    ![](publishing-to-the-app-store-images/publishxs04.png "検証対象のアカウントを選択します")
11. **[提出]** ボタンを選択します。

    ![](publishing-to-the-app-store-images/publishxs05.png "ファイルの概要ダイアログ")
12. Xcode は、iTunes Connect へのファイルのアップロードが完了したときに通知します。


Visual Studio for Mac のアーカイブ ワークフローでは、アプリケーション ローダーは自動的に開きます (_.ipa_ が保存されている場合)。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

審査のための Apple へのアプリケーションの提出は、アプリケーション ローダー アプリを使用して行われます。 これらの手順は、Mac ビルド ホストで行う必要があります。 アプリケーション ローダーのコピーは[ここ](https://itunesconnect.apple.com/apploader/ApplicationLoader_3.0.dmg)からダウンロードできます。

-----

1. *[Deliver Your App]\(アプリの配信\)* を選択して、*[選択]* ボタンをクリックします。

    [![](publishing-to-the-app-store-images/publishvs01.png "[Deliver Your App]\(アプリの配信\) を選択します")](publishing-to-the-app-store-images/publishvs01.png#lightbox)

2. 前の手順で作成した IPA ファイルまたは zip ファイルを選択し、**[OK]** ボタンをクリックします。

3. アプリケーション ローダーはファイルを検証します。

    [![](publishing-to-the-app-store-images/publishvs02.png "検証画面")](publishing-to-the-app-store-images/publishvs02.png#lightbox)
4. *[次へ]* ボタンをクリックすると、アプリケーションは App Store に対して検証されます。

    [![](publishing-to-the-app-store-images/publishvs03.png "App Store に対する検証")](publishing-to-the-app-store-images/publishvs03.png#lightbox)
5. **[送信]** ボタンをクリックして、審査のために Apple にアプリケーションを送信します。
6. アプリケーション ローダーは、ファイルが正常にアップロードされたときに通知します。

## <a name="itunes-connect-status"></a>iTunes Connect のステータス

ITunes Connect に再度ログインし、使用可能なアプリのリストからアプリケーションを選択した場合、iTunes Connect のステータスは **[審査待ち]** になります (処理中に、一時的に **[Upload Received]\(アップロード受信済み\)** になる場合があります)。

[![](publishing-to-the-app-store-images/image21.png "iTunes Connect の状態は [Waiting for Review] と表示されます")](publishing-to-the-app-store-images/image21.png#lightbox)

## <a name="summary"></a>まとめ

この記事では、App Store に発行するアプリケーションを構成、ビルド、および提出するためのステップ バイ ステップ ガイドを示しました。 最初に、配布プロビジョニング プロファイルの作成とインストールに必要な手順を説明しました。 次に、Visual Studio と Visual Studio for Mac を使用して配布ビルドを作成する方法を説明しました。 最後に、iTunes Connect とツールを使用して App Store にアプリケーションを提出する方法を示しました。


## <a name="related-links"></a>関連リンク

- [イメージの処理](~/ios/app-fundamentals/images-icons/index.md)
- [iOS App Development Workflow Guide: Distributing Applications (iOS アプリの開発ワークフロー ガイド: アプリケーションの配布)](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/35-Distributing_Applications/distributing_applications.html)
- [App Store への提出に関するヒント](https://developer.apple.com/appstore/resources/submission/tips.html)
- [一般的なアプリケーションの却下理由](https://developer.apple.com/app-store/review/rejections/)
- [App Store の審査に関するガイドライン](https://developer.apple.com/appstore/resources/approval/guidelines.html)
