---
title: MacOS Sierra の概要
description: この記事では、Xamarin.Mac 開発者向けのすべての新規および変更した Api と macOS Sierra で使用できる機能を紹介します。
ms.prod: xamarin
ms.assetid: 71A8A737-F310-4320-BD23-743AA1E9033C
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 5a944fd8f7dcfdcbb3f025c92b4afac35673416f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50111013"
---
# <a name="introduction-to-macos-sierra"></a>MacOS Sierra の概要

新しい macOS Sierra で、開発者は、以前は不可能だった方法で、アプリと web サイトと対話するエンドユーザーに許可する新しい Api の利用できます。 たとえば、Apple できるようになりましたことにより、お客様の支払いに安全に Apple Pay、および金属製フレームワーク ブーストの機能強化を使用して、アプリのグラフィックスとコンピューティングのオプションの web サイト潜在的です。 

MacOS Sierra の詳細については、Apple を参照してください[macOS + アプリ](https://developer.apple.com/macos/)ドキュメント。

<a name="Whats-New-in-macOS-Sierra" />

## <a name="whats-new-in-macos-sierra"></a>MacOS Sierra で新します。

Apple には、多くの機能強化を含め、既存の機能と共に、macOS Sierra でいくつかの新しい Api やサービスが追加します。

<a name="Apple-File-System" />

### <a name="apple-file-system"></a>Apple のファイル システム

MacOS Sierra では、Apple は iOS、macOS、tvOS、watchOS 用の最新のファイル システムとして、新しい Apple ファイル システムをリリースが。 Apple のファイル システムは、Flash と SSD の記憶域用に最適化され、次の機能を提供します。 強力な暗号化、コピー オン ライト メタデータ、共有、ファイルとディレクトリ、スナップショット、高速のディレクトリのサイズ変更および分割不可能な安全な保存のプリミティブの複製の領域。

詳細については、Apple を参照してください[Apple ファイル システム ガイド](https://developer.apple.com/library/prerelease/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40016999)します。

<a name="Apple-Pay-Enhancements" />

### <a name="apple-pay-enhancements"></a>Apple Pay 機能強化

Apple は、ユーザーが web サイトからの支払いはセキュリティで保護を行うことを許可する macOS Sierra で Apple Pay にいくつかの機能強化を行ったが。

MacOS Sierra では、いくつかの新しい Api が追加されました macOS Sierra、iOS、および動的な支払いネットワークと新しいサンド ボックス テスト環境をサポートする watchOS で使用できます。

macOS Sierra では、開発者は iOS および macOS Safari ベースの web サイトに直接 Apple Pay を組み込むことを可能にする新しい ApplePay Javascript フレームワークが含まれています。 Apple Pay をサポートする web サイト、ユーザーは、iPhone、または Apple Watch のいずれかを使用して支払いを承認できます。

詳細については、Apple を参照してください[ApplePay JS Framework](https://developer.apple.com/reference/applepayjs)参照。

<a name="Building-Modern-macOS-Apps" />

### <a name="building-modern-macos-apps"></a>最新の macOS アプリの構築

Apple の Safari web ブラウザー、ワード プロセッサのページ番号スプレッド シートの使用がフローティング パネルなどの従来の UI 要素を削除する、統一された、状況依存のユーザー インターフェイスを提供する多くの新しいテクノロジと複数を開くなどの最新の macOS アプリwindows とします。

[![Mac のタブ付きウィンドウの例](images/content08.png)](images/content08.png#lightbox)

当社[ビルドの最新の macOS アプリ](~/mac/platform/introduction-to-macos-sierra/modern-cocoa-apps.md)ガイドは、いくつかのヒント、機能と手法開発者 Xamarin.Mac で最新の macOS アプリのビルドに使用することについて説明します。

<a name="CloudKit-Data-Sharing" />

### <a name="cloudkit-data-sharing"></a>CloudKit のデータの共有

Macos Sierra を迅速かつ簡単に共有レコードまたはレコード セットをプライベート iCloud データベースからユーザーを許可する CloudKit フレームワークが拡張されました。

CloudKit の送信およびレコードの共有の招待を受け付けての完全な UI を提供します、ユーザーは完全な読み取り/書き込みのレコードにアクセス権を持つユーザーを制御します。

詳細については、Apple を参照してください[CloudKit のフレームワーク参照](https://developer.apple.com/reference/clockkit)と[CloudKit JS のフレームワーク参照](https://developer.apple.com/reference/cloudkitjs)します。

> [!IMPORTANT]
> Apple からは、開発者が欧州連合の一般データ保護規則 (GDPR) を適切に処理するための[ツールが提供](https://developer.apple.com/support/allowing-users-to-manage-data/)されています。

<a name="Safari-App-Extensions-Support" />

### <a name="safari-app-extensions-support"></a>Safari アプリ拡張機能のサポート

Safari のアプリの拡張機能では、アプリが macOS Sierra と緊密に統合されているときに、Safari web ブラウザーの動作を拡張できるようにします。 MacOS Safari のアプリの拡張機能は動作 iOS Safari のアプリの拡張機能に似ていますが後、は簡単なポートに別のシステムからです。

詳細については、Apple を参照してください[Safari アプリ拡張機能のプログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternetWeb/Conceptual/SafariAppExtension_PG/index.html#//apple_ref/doc/uid/TP40017319)します。

<a name="Security-and-Privacy-Enhancements" />

### <a name="security-and-privacy-enhancements"></a>セキュリティとプライバシーの機能強化

Apple には、セキュリティとプライバシー macOS Sierra のに役立つ、アプリのセキュリティを向上させ、エンドユーザーのプライバシーの次のように、アプリの両方に対していくつかの機能強化が行われます。

- 新しい`NSAllowsArbitraryLoadsInWebContent`アプリのキーを追加できます`Info.plist`ファイルし、web ページをアプリの残りの部分で、Apple Transport Security (ATS) の保護がまだ有効になっている、正しくロードできるようになります。
- 一般的なデータのセキュリティ アーキテクチャ (CDSA) API は非推奨し、非対称キーを生成する SecKey API で置き換える必要があります。
- すべての SSL/TLS 接続の RC4 対称暗号は既定で無効になりました。 さらに、トランスポートのセキュリティで保護された API が SSLv3 がサポートされなくされ、アプリでは、SHA 1 および 3 des 暗号化を使用して、できるだけ早く停止するをお勧めします。
- IOS 10 デバイスと macOS で新しいクリップボード Sierra をコピーして貼り付けるデバイス間でユーザーが許可されているため、API は、特定のデバイスに制限して、特定の時点に自動的にオフにするタイムスタンプを記録するクリップボードを許可する拡張されました。 さらに、名前付きペーストはもう残っていませんし、共有ペースト ボード コンテナーに置き換える必要があります。
- アプリ (ユーザーのカレンダーの場合) などの保護されたデータにアクセスする場合、_する必要があります_で正しい目的の文字列値のキーを使用して、その意図を宣言、`Info.plist`ファイル (`NSCalendarUsageDescription`予定表の場合)。
- Mac App Store を使用していない配信される開発者署名済みアプリでは、CloudKit、iCloud キーチェーン、iCloud ドライブ、リモートのプッシュ通知、MapKit と VPN の権利の今すぐ利用できます。
- macOS Sierra では、ランタイム前にランタイムのパスが認識されていない外部コードやデータと共に、zip アーカイブまたは符号なしのディスク イメージのコード署名者のアプリの配信をサポートしていません。

さらに、アプリが macOS Sierra で実行されている (またはそれ以降) で 1 つまたは複数のプライバシーに関する特定キーを入力して特定の機能やユーザー情報にアクセスしようとするを静的に宣言する必要があります、`Info.plist`ユーザー、アプリが取得しようとした理由を説明するファイルアクセスします。

MacOS Sierra では、iOS 10 とこれらの変更を共有するため、iOS 10 を参照してください[セキュリティおよびプライバシーの強化機能](~/ios/app-fundamentals/security-privacy.md)詳細情報をガイドします。

<a name="Smart-Card-Driver-Extension-Support" />

### <a name="smart-card-driver-extension-support"></a>スマート カード ドライバーの拡張機能のサポート

MacOS Sierra では、アプリを作成できます`NSExtension`ベースのスマート カードのドライバー コンテンツを特定の種類のスマート カードから読み取り専用アクセスを許可します。 この情報は、(非推奨の一般的なデータのセキュリティ アーキテクチャ メソッドを置き換える) システム キーチェーン内で表示されます。

詳細については、Pleas 参照 Apple の[CryptoTokenKit フレームワーク参照](https://developer.apple.com/reference/cryptotokenkit)します。

<a name="Unified-Logging" />

### <a name="unified-logging"></a>統合されたログ記録

統合されたログ記録では、システムのすべてのレベル間で効率的なメッセージングを 1 つの API を使用したアプリを提供します。 統合ログでは、アプリは、プライバシー管理と動作状況の追跡を簡単にデバッグを含む複数のレベルのログ記録の詳細に制御が。 

ログ記録は、アクティビティの追跡およびログ記録を併用すると、メッセージの自動相関関係を提供します。

macOS Sierra では、新しいコンソール アプリを (アプリケーション/ユーティリティ) で接続されているデバイスを含む複数のソースからログ データを表示することが含まれています。 また、トークン化および保存された検索をサポートし、複数のプロセスで関連するメッセージの間の接続を表示します。

さらに、ログ メッセージは表示することや、コマンド ライン ツールを使用して保持されます。

詳細については、Apple を参照してください[ログ参照](https://developer.apple.com/reference/os/1891852-logging)します。

<a name="Wide-Color" />

### <a name="wide-color"></a>色

macOS Sierra では、拡張範囲のピクセル形式とコア グラフィックス、Core のイメージ、金属製および AVFoundation などのフレームワークを含めて、システム全体で全体の色域スペースのサポートを拡張します。 ワイド カラー ディスプレイを使用したデバイスのサポートはさらに、全体のグラフィックス スタック全体でこの動作を提供することで緩和されました。

さらに、`AppKit`が変更されている、新しい機能を拡張**sRGB**大幅なパフォーマンス失わずワイド色域にカラーの混合をやすくするための色空間。

Apple は、さまざまな色を使用する場合、次のベスト プラクティスを提供します。

- `NSColor` これは、sRGB 色空間となることはありませんクランプする値、`0.0`に`1.0`範囲。 アプリは、以前のクランプ動作に依存する場合は、macOS Sierra を変更する必要があります。
- コア グラフィックスや金属などの低レベルの API を使用して、イメージの処理を提供する、アプリは、16 ビット浮動小数点値をサポートする拡張範囲の色領域とピクセル形式を使用する必要があります。 必要に応じて、アプリが色コンポーネントの値を手動でクランプする必要があります。
- コア グラフィックス、Core イメージおよび金属パフォーマンス シェーダーは、2 つのカラー スペース間で変換するための新しいメソッドを提供します。

詳細については、次を参照してください、[色の概要](~/ios/platform/wide-color.md)ガイド。

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>その他のフレームワークの変更

だけでなく、主要なフレームワークの変更と上記に一覧表示されます Apple は、macOS Sierra の多くの他の軽微なフレームワークの変更を加えたが。

詳細については、次を参照してください、 [Framework 変更](~/mac/platform/introduction-to-macos-sierra/additional-framework-changes.md)ガイド。

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>非推奨の API

MacOS Sierra では、次の Api が廃止されました。

- HFS 標準ファイル システムがサポートされていません。

Apple を参照してください。 [OS X v10.12 API の差分を](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/APIDiffsMacOS10_12/index.html)廃止された機能と変更の完全な一覧についてはドキュメントです。

## <a name="related-links"></a>関連リンク

- [Mac サンプル](https://developer.xamarin.com/samples/mac/)
- [新機能については OS X 10.12 です。](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
