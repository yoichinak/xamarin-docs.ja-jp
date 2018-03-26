---
title: iTunes Connect でのアプリの構成
description: この記事では、iTunes Connect で Xamarin.iOS アプリケーションを App Store で配布するためにリリースできるように、設定および維持するために必要な手順について説明します。
ms.topic: article
ms.prod: xamarin
ms.assetid: 74587317-4b15-4904-9582-dcd914827cbc
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 3d5c84aee12c374317a797aa41446630a441f6df
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="configuring-an-app-in-itunes-connect"></a>iTunes Connect でのアプリの構成

_この記事では、iTunes Connect で Xamarin.iOS アプリケーションを App Store で配布するためにリリースできるように、設定および維持するために必要な手順について説明します。_

iTunes Connect は、特に App Store で iOS アプリケーションを管理するための Web ベースのツール群です。 Xamarin.iOS アプリケーションをレビューのために Apple に提出し、最終的に App Store で販売または無償アプリとしてリリースする前に、アプリケーションを正しく設定して、iTunes Connect で構成する必要があります。

iTunes Connect は、次のことに使用できます。

- アプリケーションの名前 (App Store に表示される名前) を設定する。
- アプリケーションがサポートしている iOS デバイスで動作中のアプリケーションのスクリーンショットやビデオを提供する。
- 機能やエンドユーザーへのメリットを含む、アプリケーションの明確で簡潔な説明を提供する。
- ユーザーが App Store でアプリを見つけやすくするため、カテゴリとサブカテゴリを提供する。
- ユーザーがアプリをより見つけやすくするために、キーワードを提供する。
- 連絡先と Web サイトのサポート URL を提供する (Apple で必要)。
- App Store で保護者による規制を通知するために使用されるアプリケーションの評価を設定する。
- 販売価格を選択するか、アプリケーションを無料でリリースするかを指定する。
- Game Center やアプリ内購入など、オプションの App Store テクノロジを構成する。

さらに、アプリには、Apple が App Store でフィーチャーする場合に使用できる魅力的で高解像度のアートワークも必要です。 詳細については、Apple の [iTunes Connect デベロッパ ガイド](https://developer.apple.com/support/itunes-connect/)を参照してください。

## <a name="managing-agreements-tax-and-banking"></a>契約、税金、銀行の管理

iTunes Connect の **[Agreements, Tax, and Banking]\(契約、税金と銀行の情報\)** セクションは、iTunes 開発者の支払いと源泉徴収に関する必須の財務情報を提供し、Apple と結んだすべての契約のステータスを追跡するために使用されます。 App Store で iOS アプリケーションをリリース (無償または販売) するには、その前に適切な契約を締結し、既存の契約の変更に合意する必要があります。

[![](itunesconnect-images/agreement01.png "契約、税金、銀行の管理")](itunesconnect-images/agreement01.png#lightbox)

ここでは、次の操作を実行できます。

- **チーム エージェント**を指定し、iTunes Connect アカウントのその他のユーザー ロール (**Admin** や **Finance** など) を定義します。
- App Store でのアプリケーションの配布を組織に許可する**契約**を締結し保持します。
- 組織に関連付けられている契約、銀行情報、および税金情報の照合に使用するための**法人**名 (App Store の Seller Name) を指定します。
- App Store 経由でアプリケーションを販売する場合は、**銀行**と**税金**の情報を提供します。

繰り返しますが、iOS アプリケーションをレビューとリリースのために iTunes Connect を提出する前に、この情報を正しく設定し、最新のものにする_必要があります_。 詳細については、Apple の[契約、税金、銀行の管理](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/ManagingContractsandBanking.html#//apple_ref/doc/uid/TP40011225-CH21-SW1)に関するドキュメントを参照してください。

<a name="creating" />

## <a name="creating-an-itunes-connect-record"></a>iTunes Connect レコードの作成

Xamarin.iOS アプリケーションを App Store から配布するために iTunes Connect にアップロードする前に、iTunes Connect でアプリケーションのレコードを作成する必要があります。 このレコードには、App Store に表示されるアプリケーションに関するすべての情報 (必要なだけの言語で) と、配布プロセスを通じてアプリ管理に必要なすべての情報が含まれます。 また、iAd App Network や Game Center などの App Store テクノロジを構成するためにも使用されます。

iOS アプリケーションを iTunes Connect に追加するには、**チーム エージェント**になるか、**Admin** または**Technical** の役割を持つユーザーである必要があります。

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) で次の操作を行います。

1. **[My Apps]\(マイ App\)** をクリックします。

    [![](itunesconnect-images/add01.png "[My Apps] をクリックします")](itunesconnect-images/add01.png#lightbox)
2. 左上隅の **+** をクリックして、**[New iOS App]\(\新規 iOS アプリ)** を選択します。

    [![](itunesconnect-images/add02.png "新規 iOS アプリの追加")](itunesconnect-images/add02.png#lightbox)
3. iTunes Connect で **[New iOS App]\(\新規 iOS アプリ)** ダイアログが表示されます。

    [![](itunesconnect-images/add03.png "[New iOS App] ダイアログ")](itunesconnect-images/add03.png#lightbox)
4. App Store に表示するアプリケーションの**名前**と**バージョン番号**を入力します。
5. **プライマリ言語**を選択します。
6. **SKU** 番号を入力します。これはアプリケーションを追跡するために使用される、一意の定数の識別子です。 これはエンドユーザーには表示されず、アプリが作成されると、変更_できません_。
7. アプリケーションをプロビジョニングするときに、Developer Center で作成したアプリケーションの**バンドル ID** を選択します。 Visual Studio for Mac または Visual Studio で配布バンドルに署名する際に、これと同じバンドル ID を使用する必要があります。 詳細については、[配布プロファイルの作成](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile)と [Xamarin.iOS プロジェクトでの配布プロファイルの選択](~/ios/get-started/installation/device-provisioning/index.md)に関するドキュメントを参照してください。
8. **[Create]\(\作成)** ボタンをクリックして、アプリケーションの新しい iTunes Connect レコードをビルドします。

新しいアプリケーションが iTunes Connect で作成され、説明、価格、カテゴリ、評価などの必要な情報を入力できます。

[![](itunesconnect-images/add04.png "新しいアプリケーションは iTunes Connect に作成されます")](itunesconnect-images/add04.png#lightbox)

<a name="managing" />

## <a name="managing-app-videos-and-screenshots"></a>アプリのビデオとスクリーンショットの管理

App Store で iOS アプリケーションのマーケティングを成功させるために最も重要な要素の 1 つは、すばらしい一連のスクリーンショットと、オプションのビデオ プレビューです。 ユーザー操作を強調し、独自機能を紹介する、実行中のアプリケーションの実際のビューを使用します。 アプリケーションのプレビュー ビデオを使用して、ユーザーにアプリケーションの使用についてのアイデアを提供します。

Apple では、スクリーンショットを作成する際に次のことを推奨しています。

- アプリケーションでサポートしている iOS デバイスでの最高のプレゼンテーションのため、スクリーンショットを最適化し、コンテンツが判読できることを確認します。
- iOS デバイス イメージのスクリーンショットにはフレームを付けないでください。
- 全画面表示を使用してアプリケーションの実際のビューを表示し、スクリーンショットの周囲にグラフィックや枠線がないようにします。
- スクリーンショットからステータス バーを必ず削除してください、iTunes Connect ではこの領域が除外されたディメンションのスクリーンショットが必要です。
- 可能であれば、(iOS シミュレーターではなく) 本物の高解像度の Retina iOS ハードウェアでスクリーンショットを作成してください。
- アプリのプレビュー ビデオが利用できない場合は、App Store の検索結果として最初のスクリーンショットが iPhone および iPad に表示されるため、最も良いスクリーンショットを最初に配置してください。
- 5 つのスクリーンショットすべてを使用して、強調表示の瞬間に、説得力のあるアプリケーションのストーリーを伝えられるようにします。

Apple では、アプリケーションがサポートするすべての画面サイズと解像度でスクリーンショットとビデオを提供することを求めています。 さらに、サポートしている画面の向きに基づいて、縦長と横長のバージョンを提供する必要があります。

現在必須となっている画面サイズと解像度は次のとおりです。

[!include[](~/ios/includes/table-itunesconnect.md)]

### <a name="editing-screenshots-in-itunes-connect"></a>iTunes Connect でのスクリーンショットの編集

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) で次の操作を行います。

1. **[My Apps]\(マイ App\)** をクリックします。
2. 編集するアプリケーションの**アイコン**をクリックします。
3. **[Versions]\(バージョン\)** タブを選択します。
4. **[Screenshots]\(スクリーンショット\)** セクションまでスクロールします。
5. **[Image Size]\(画像サイズ\)** を選択し、必要なイメージにドラッグします (画面サイズあたり 5 まで)。

    [![](itunesconnect-images/screenshot01.png "[Image Size] を選択し、必要なイメージにドラッグします")](itunesconnect-images/screenshot01.png#lightbox)
6. すべての必要な画面サイズに繰り返します。
7. 画面の上部にある **[Save]\(保存\)** ボタンをクリックして、変更を保存します。

> [!NOTE]
> 注: スクリーンショットまたはアプリのプレビュー ビデオがアプリケーションの現在の機能と一致しない場合、Apple によって提出が拒否されます。

<a name="metadata" />

## <a name="managing-name-description-whats-new-keywords-and-urls"></a>名前、説明、新機能、キーワード、URL の管理

iTunes Connect アプリケーション レコードのこのセクションは、アプリケーションのローカライズされた情報、アプリケーションの機能、新バージョンの変更点、検索に使用するキーワード、iAd サポート、およびサポート URL を指定します。

### <a name="app-name"></a>アプリ名

アプリケーションの機能を反映したわかりやすいアプリケーション名を選択します。 できる限り短く簡潔なものにします。 アプリケーションの名前は、ユーザーがアプリケーションを検索して見つけるのに重要な役割を果たすため、単純で覚えやすい名前にします。 iOS デバイス (iPad、iPhone、iPod touch) で表示したときにどのように名前が表示されるかに、特に注意してください。

Apple では、アプリケーションの名前を選択するときに、次のガイドラインを示しています。

- 短く、単純で、覚えやすいものにする。
- 第三者の著作権や商標を侵害しないこと。
- アプリケーションの機能と一致したものにする。
- 該当する場合は、外国の市場向けにローカライズされた名前を提供する。

### <a name="description"></a>説明

アプリケーションとその機能の明確かつ簡潔でわかりやすい説明を記述します。 最初の数行が最も重要で、良い第一印象を与え、ユーザーを引き付けるチャンスです。 アプリケーションの特徴や他の同様のアプリケーションと異なる点について説明します。

Apple では、アプリケーションの説明の記述について、次のことを提案しています。

- 簡潔な第 1 段落または第 2 段落と、主な機能の短い箇条書きリストを含める。
- 該当する場合は、外国の市場向けにローカライズされた説明を提供する。
- ユーザー レビュー、称賛、またはユーザーの声は、(ある場合でも) 最後にのみ含める。
- より読みやすくするため、改行と箇条書きを使用する。
- 各デバイスの種類で App Store にアプリの説明がどのように表示されるかに注意して、説明の中で最も重要な文章が見やすく表示されることを確認する。

### <a name="whats-new"></a>新機能

アプリケーションの新しいバージョンをアップロードするときに、**[What’s New in this Version]\(このバージョンの新機能\)** フィールドが使用できます。慎重に熟考したうえで記入します。

Apple では、新機能の情報を入力する際に、次のガイドラインを示しています。

- 更新をユーザーに促すメッセージを追加する。
- 重要な順に項目をリストし、変更およびバグの修正を正確に示す。
- 技術的な専門用語ではなく、わかりやすく正確な言葉で変更を示する。

### <a name="keywords"></a>キーワード

アプリケーションの機能に関連するよく考えられた戦略的なキーワードは、ユーザーが App Store で検索するときにアプリケーションを簡単に見つけることに役立ちます。 また、アプリケーションで iAd 広告を表示する場合、iAd App Network はキーワードを使用してアプリに表示する広告を選択します。

Apple では、キーワードを選択する際に、次のことを推奨しています。

- 競合するアプリケーション名、会社名、製品名、または商標名を使用しない。
- 一般的な語句や用語を使用しない。
- 不適切または不快感を与える用語や、有名人の名前などの関連性のない用語は使用しない。
- 該当する場合は、外国の市場向けにキーワードをローカライズする。

### <a name="urls"></a>URL

Apple では、ユーザーのアプリケーションに関する問題や質問をサポートするため、開発者に自身の Web サイトへのリンクの提供を求めています。 また、アプリケーションのプライバシー ポリシー (これも自身の Web サイトでホストする必要があります) へのリンクも求められます。

必要に応じて、自身の Web サイトでホストしているマーケティング情報へのリンクを提供して、App Store よりも詳しいアプリケーションの情報を提供することができます。

### <a name="editing-name-description-whats-new-keywords-and-urls-in-itunes-connect"></a>iTunes Connect での名前、説明、新機能、キーワード、URL の編集

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) で次の操作を行います。

1. **[My Apps]\(マイ App\)** をクリックします。
2. 編集するアプリケーションの**アイコン**をクリックします。
3. **[Versions]\(バージョン\)** タブを選択します。
4. **[Name]\(名前\)** セクションまでスクロールします。
5. 必要なすべての情報を入力します。

    [![](itunesconnect-images/name01.png "iTunes Connect での名前、説明、新機能、キーワード、URL の編集")](itunesconnect-images/name01.png#lightbox)
6. 画面の上部にある **[Save]\(保存\)** ボタンをクリックして、変更を保存します。

> [!IMPORTANT]
> 注: 名前、説明、新機能、キーワード、または URL がアプリケーションの現在の機能と一致しない場合、Apple によって提出が拒否されます。

<a name="general" />

## <a name="maintaining-general-app-information"></a>一般的なアプリの情報の維持

iTunes Connect アプリケーション レコードのこのセクションでは、(Apple によって割り当てられた) アプリケーションの一意の ID、アプリケーションが属するカテゴリ、評価、著作権、およびアプリケーションをリリースする会社の情報を提供します。

### <a name="app-icon"></a>アプリ アイコン

> [!IMPORTANT]
>  アプリ アイコンは、iTunes Connect を通して提出されなくなりました。 プロジェクトの **Assets.xcassets** ファイルに設定された **AppIcon** の画像を通して提出する必要があります。 詳細については、[App Store アイコン](~/ios/app-fundamentals/images-icons/app-store-icon.md)のガイドをご覧ください。

アプリのアイコンは、ユーザーにとってアプリケーションの顔です。そのため、覚えやすく、小さなサイズで適切に表示されるものにする必要があります。 覚えやすいアイコンとは、クリーンで単純で、すぐに認識できるものです。

Apple では、アプリケーションのアイコンをデザインするときに、次のガイドラインを示しています。

- アプリケーションに適したアイコンにする。
- アプリケーションのデザインと一貫性のある単純なアイコンを作成する。
- アイコンに語句の使用は避ける。
- グローバルに考える: 1 つのアプリ アイコンがストアのすべての販売区域で使用される。

App Store で表示されるアプリ アイコンには、1024 × 1024 ピクセルの画像が必要です。

詳細については、Apple の「[iOS Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/index.html#//apple_ref/doc/uid/TP40006556)」 (iOS ヒューマン インターフェイス ガイドライン) と、[一般的なアプリ情報](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Appendices/Properties.html#//apple_ref/doc/uid/TP40011225-CH26-SW7)のドキュメントの大きなアプリ アイコンのセクションの説明を参照してください。

### <a name="app-id"></a>アプリ ID

これは、iTunes Connect レコードの作成時に、Apple によってアプリケーションに割り当てられる一意の ID 番号です。 Apple が提供する複数の Web ベースのインターフェイス (自身の Web サイト内の App Store の情報を含む) を呼び出すときにこの番号を使用することができます。

### <a name="version-number"></a>バージョン番号

これは、App Store でユーザーに表示されるアプリケーションの現在のアクティブなバージョンです。

### <a name="category-and-sub-category"></a>カテゴリとサブカテゴリ

アプリケーションの検索性の重要な点の 1 つは、App Store で表示するカテゴリです。 カテゴリによりユーザーはアプリのコレクションを参照し、関心のあるものを検索できます。 iTunes Connect では、アプリケーションを定義するときに、最大 2 つの異なるカテゴリを割り当てることができます。 アプリケーションの主な機能を説明するのに最適なカテゴリを慎重に選択します。

### <a name="ratings"></a>評価

App Store では、すべてのアプリケーションに評価を設定する必要があります。 この評価は、アプリケーションの種類と内容に基づいて、保護者による制限を通知し、子供のアクセスを制限するために使用されます。 アプリケーションを定義するときに、iTunes Connect によってコンテンツの説明のリストが提供され、そのコンテンツがアプリケーションにどのくらいの頻度で表示されるかを明らかにします。 これらの選択が、App Store で表示される評価に変換されます。

子供向けのアプリケーションを作成する場合、App Store には 11 歳以下の子供向けの特別なカテゴリがあります。 アプリケーションが特に子供を対象としたものでなくても、コンテンツの適切な評価を提供することで、顧客の選択に役立ちます。

> [!IMPORTANT]
> 注: わいせつ、ポルノ、有害または中傷的と認められたアプリケーションは、Apple によって提出が拒否されます。

### <a name="copyright-and-company-information"></a>著作権および企業情報

Apple では、アプリケーションの著作権情報を提供することができ、住所や連絡先情報など、アプリケーションをリリースする会社に関する情報が必要です (これは韓国の App Store にアプリケーションをリリースするために必要です)。 この情報は、必要に応じて App Store に表示されます。

### <a name="editing-general-app-information-in-itunes-connect"></a>iTunes Connect での一般的なアプリ情報の編集

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) で次の操作を行います。

1. **[My Apps]\(マイ App\)** をクリックします。
2. 編集するアプリケーションの**アイコン**をクリックします。
3. **[Versions]\(バージョン\)** タブを選択します。
4. **[General App Information]\(App 一般情報\)** セクションまでスクロールします。
5. 必要なすべての情報を入力します。

    [![](itunesconnect-images/general01.png "iTunes Connect での一般的なアプリ情報の編集")](itunesconnect-images/general01.png#lightbox)
6. **[Edit]\(編集\)** ボタンをクリックして、**[Rating]\(レーティング\)** で評価情報を設定します。

    [![](itunesconnect-images/general02.png "評価の編集")](itunesconnect-images/general02.png#lightbox)
6. 画面の上部にある **[Save]\(保存\)** ボタンをクリックして、変更を保存します。

> [!NOTE]
> 注: カテゴリや評価がアプリケーションの現在の機能と一致しない場合は、Apple によって提出が拒否されます。

<a name="game-center" />

## <a name="maintaining-game-center-information"></a>Game Center 情報の保持

Apple の Game Center をサポートする iOS ゲーム アプリケーションの場合、ユーザーが使用可能なリーダー ボードやゲームの成績、およびアプリケーションがネットワーク接続でマルチプレーヤーに対応しているかどうかといった情報を提供できます。

### <a name="editing-game-center-information-in-itunes-connect"></a>iTunes Connect での Game Center 情報の編集

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) で次の操作を行います。

1. **[My Apps]\(マイ App\)** をクリックします。
2. 編集するアプリケーションの**アイコン**をクリックします。
3. **[Versions]\(バージョン\)** タブを選択します。
4. **[Game Center]** セクションにスクロールします。
5. **[Game Center]** セクションのスイッチを、**オン**の位置にします。
5. 必要なすべての情報を入力します。

    [![](itunesconnect-images/gamecenter01.png "iTunes Connect での Game Center 情報の編集")](itunesconnect-images/gamecenter01.png#lightbox)
6. 画面の上部にある **[Save]\(保存\)** ボタンをクリックして、変更を保存します。

**[Game Center]** タブを使用して Game Center をアクティブにし、このアプリケーションに使用可能な **Leaderboards** または **Achievements** を保持します。

[![](itunesconnect-images/gamecenter02.png "Game Center を有効にします")](itunesconnect-images/gamecenter02.png#lightbox)

[![](itunesconnect-images/gamecenter03.png "このアプリケーションに使用可能な Leaderboards または Achievements を保持します")](itunesconnect-images/gamecenter03.png#lightbox)

## <a name="maintaining-app-review-information"></a>App Review 情報の保持

このセクションを使用して、アプリケーションをレビューする Apple 担当者に、(技術者が質問がある場合の) 連絡先情報や、必要なデモ アカウント、およびテスト担当者がアプリのレビューに役立つメモなどの情報を提供します。

### <a name="editing-app-review-information-in-itunes-connect"></a>iTunes Connect での App Review 情報の編集

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) で次の操作を行います。

1. **[My Apps]\(マイ App\)** をクリックします。
2. 編集するアプリケーションの**アイコン**をクリックします。
3. **[Versions]\(バージョン\)** タブを選択します。
4. **[App Review Information]\(App Review に関する情報\)** セクションまでスクロールします。
5. 必要なすべての情報を入力します。

    [![](itunesconnect-images/review01.png "iTunes Connect での App Review 情報の編集")](itunesconnect-images/review01.png#lightbox)
6. アプリケーションのレビューが問題なく終了したら、App Store にどのようにリリースするかを選択します。

    [![](itunesconnect-images/review02.png "iTunes Connect でのリリース情報の編集")](itunesconnect-images/review02.png#lightbox)
6. 画面の上部にある **[Save]\(保存\)** ボタンをクリックして、変更を保存します。


## <a name="maintaining-pricing-information"></a>価格情報の保持

販売用にアプリケーションをリリースすることを計画している場合は、Apple の使用可能な Price Tier を選択して販売価格を設定し、指定した価格を有効にする日付を設定する必要があります。 たとえば、このドキュメントの執筆時点では、**Tier 1** 価格は次のようになっています。

[![](itunesconnect-images/price01.png "価格情報の保持")](itunesconnect-images/price01.png#lightbox)

### <a name="educational-discount"></a>教育用の割引

教育機関が一度に複数のコピーを購入するときに割引価格でアプリケーションを提供する場合は、このチェック ボックスをオンにします。 割引の詳細については、最新版の **Paid Application Agreement** を参照してください。教育機関の顧客にこのアプリケーションを提供する前にこの契約書に署名する必要があります。

### <a name="custom-business-to-business-application"></a>カスタム B2B アプリケーション

**カスタム B2B アプリケーション**として設定されているアプリケーションは、iTunes Connect で指定した **Volume Purchase Program** の顧客のみを対象とし、該当する地域でのみ販売されます (たとえば、米国のVolume Purchase Program の顧客は、米国のApp Store Volume Purchase Program for Business を使用する必要があります)。

カスタム B2B アプリケーションは、教育機関や App Store の一般の顧客は利用できません。 *App Store Volume Purchase Program for Business* の詳細については、Apple の[よく寄せられる質問](http://vpp.itunes.apple.com/faq)に関するページを参照してください。 顧客が **Volume Purchase Program** にサインアップする方法の詳細については、Apple の [Deployment Programs](http://enroll.vpp.itunes.apple.com) に関するページを参照してください。

### <a name="editing-pricing-information-in-itunes-connect"></a>iTunes Connect での価格情報の編集

[iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa) で次の操作を行います。

1. **[My Apps]\(マイ App\)** をクリックします。
2. 編集するアプリケーションの**アイコン**をクリックします。
3. **[Pricing]\(価格\)** タブを選択します。

    [![](itunesconnect-images/price02.png "iTunes Connect での価格情報の編集")](itunesconnect-images/price02.png#lightbox)
4. **[Availability Date]\(公開日\)** を選択します。
5. **[Price Tier]\(価格\)** ドロップダウン リストから希望価格を選択します。
5. 必要に応じて**教育機関への割引**を有効にします。
6. 必要に応じて、アプリケーションを**カスタム B2B アプリケーション**として定義します。
6. **[Save]\(保存\)** ボタンをクリックして変更内容を保存します。

<a name="iap" />

## <a name="maintaining-in-app-purchase-information"></a>アプリ内購入情報の保持

アプリケーションから仮想のアプリ内製品 (新しいゲーム レベルやアプリケーションの機能など) の販売を計画している場合は、このセクションを使用してこれらの購入アイテムを作成および管理します。

[![](itunesconnect-images/inapp01.png "アプリ内購入情報の保持")](itunesconnect-images/inapp01.png#lightbox)

Xamarin.iOS アプリケーションでのアプリ内購入の使用に関する詳細については、「[In-App Purchasing](~/ios/platform/in-app-purchasing/index.md)」 (アプリ内購入) ドキュメントを参照してください。

## <a name="viewing-application-reviews"></a>アプリケーションのレビューの表示

App Store にアプリがリリースされると、アプリケーションを購入したユーザーまたは無料でダウンロードしたユーザーはアプリのレビューを記入し、星評価をすることができます。 このセクションを使用して、これらのレビューを表示します。 例:

[![](itunesconnect-images/reviews01.png "アプリケーションのレビューの表示")](itunesconnect-images/reviews01.png#lightbox)

## <a name="summary"></a>まとめ

この記事では、iTunes Connect を使用して App Store にリリースするために Xamarin.iOS アプリケーションを準備する方法、および App Store に表示されるアプリケーションに関するすべての情報を維持する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [イメージの処理](~/ios/app-fundamentals/images-icons/index.md)
- [iOS App Development Workflow Guide: Distributing Applications (iOS アプリの開発ワークフロー ガイド: アプリケーションの配布)](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/35-Distributing_Applications/distributing_applications.html)
- [App Store への提出に関するヒント](https://developer.apple.com/appstore/resources/submission/tips.html)
- [App Store の審査に関するガイドライン](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [iTunes Connect 開発者ガイド](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/Chapters/About.html#//apple_ref/doc/uid/TP40011225-CH1-SW1)
