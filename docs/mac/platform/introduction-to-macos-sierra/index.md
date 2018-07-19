---
title: MacOS Sierra の概要
description: この記事では、Xamarin.Mac 開発者向けのすべての新しいまたは変更された Api と macOS Sierra で使用できる機能を紹介します。
ms.prod: xamarin
ms.assetid: 71A8A737-F310-4320-BD23-743AA1E9033C
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3b8211e4c38fd37040fab5b35be4709d4b926c91
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2018
ms.locfileid: "31044981"
---
# <a name="introduction-to-macos-sierra"></a>MacOS Sierra の概要

新しい macOS Sierra などで、開発者は、以前は不可能だった方法で、アプリと web サイトと対話するエンドユーザーに許可する新しい Api の利用できます。 たとえば、Apple できるようになりましたお客様の支払いに安全に Apple Pay と金属製のフレームワーク ブーストする拡張機能を使用して、アプリのグラフィックスとコンピューティングのオプションを提供する web サイト潜在的なです。 

MacOS Sierra の詳細については、Apple を参照してください[macOS + アプリ](https://developer.apple.com/macos/)ドキュメント。

<a name="Whats-New-in-macOS-Sierra" />

## <a name="whats-new-in-macos-sierra"></a>新機能 macOS Sierra

Apple が macOS Sierra と共になど、既存の機能に多くの機能強化では、いくつかの新しい Api とサービスを追加が。

<a name="Apple-File-System" />

### <a name="apple-file-system"></a>Apple のファイル システム

MacOS Sierra、Apple は iOS、macOS、tvOS および watchOS の最新のファイル システムとして新しい Apple ファイル システムをリリースがします。 Apple ファイル システムのフラッシュ デバイスと SSD 記憶域用に最適化され、次の機能を提供します。 強力な暗号化、書き込み時コピーのメタデータ、領域の共有、ファイルおよびディレクトリ、スナップショット、高速のディレクトリのサイズ変更および分割不可能な安全な保存プリミティブのクローン作成します。

詳細については、Apple を参照してください[Apple ファイル システム ガイド](https://developer.apple.com/library/prerelease/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40016999)です。

<a name="Apple-Pay-Enhancements" />

### <a name="apple-pay-enhancements"></a>Apple 給与の機能強化

Apple の拡張機能を Apple のお支払いに向けた macOS Sierra の web サイトからセキュリティで保護された支払にユーザーに許可します。

MacOS Sierra などのいくつかの新しい Api が追加されました macOS Sierra、iOS、および動的支払ネットワークと新しいサンド ボックスのテスト環境をサポートするために watchOS を操作します。

macOS Sierra には、iOS や macOS Safari ベースの web サイトに直接 Apple Pay を統合する開発者を可能にする新しい ApplePay Javascript フレームワークが含まれています。 Apple Pay をサポートする web サイトの場合、ユーザーは、iPhone、または Apple Watch のいずれかを使用して支払を承認できます。

詳細については、Apple を参照してください[ApplePay JS Framework](https://developer.apple.com/reference/applepayjs)参照します。

<a name="Building-Modern-macOS-Apps" />

### <a name="building-modern-macos-apps"></a>最新 macOS アプリの構築

Apple の Safari web ブラウザー、ページのワード プロセッサ番号スプレッド シートを使用してフローティング パネルなどの従来の UI 要素を削除する、統一された、状況依存のユーザー インターフェイスを提供する多数の新しいテクノロジおよび複数開くなど最新 macOS アプリwindows です。

[![Mac のタブ付きウィンドウの例](images/content08.png)](images/content08.png#lightbox)

当社[構築最新 macOS アプリ](~/mac/platform/introduction-to-macos-sierra/modern-cocoa-apps.md)ガイドには、いくつかのヒント、機能と手法、開発者が Xamarin.Mac で最新 macOS アプリのビルドに使用できますがについて説明します。

<a name="CloudKit-Data-Sharing" />

### <a name="cloudkit-data-sharing"></a>CloudKit データの共有

CloudKit フレームワークは、macOS を迅速かつ簡単に共有をプライベート iCloud データベースからレコード セットのレコードまたはユーザーを許可する Sierra で展開されています。

CloudKit 完全な UI を送信して、レコードの共有の招待を受信しており、ユーザーが完全な読み取り/書き込み、レコードへのアクセスを持つユーザーを制御します。

詳細については、Apple を参照してください[CloudKit フレームワーク参照](https://developer.apple.com/reference/clockkit)と[CloudKit JS フレームワーク参照](https://developer.apple.com/reference/cloudkitjs)です。

> [!IMPORTANT]
> Apple[ツールを提供](https://developer.apple.com/support/allowing-users-to-manage-data/)開発者が、欧州連合の一般的なデータ保護規制 (GDPR) を適切に処理します。

<a name="Safari-App-Extensions-Support" />

### <a name="safari-app-extensions-support"></a>Safari アプリ拡張機能のサポート

Safari アプリ拡張機能は、macOS Sierra と緊密に統合されているときに、Safari web ブラウザーの動作を拡張するアプリを許可します。 MacOS Safari アプリ拡張機能は動作 iOS Safari アプリ拡張機能に似ていますが、以降は簡単なポートに別のシステムからです。

詳細については、Apple を参照してください[Safari アプリ拡張機能のプログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternetWeb/Conceptual/SafariAppExtension_PG/index.html#//apple_ref/doc/uid/TP40017319)です。

<a name="Security-and-Privacy-Enhancements" />

### <a name="security-and-privacy-enhancements"></a>セキュリティとプライバシーの機能強化

Apple には、両方のセキュリティとプライバシー macOS アプリケーションは、アプリのセキュリティを強化し、次を含む、エンドユーザーのプライバシーを確保に役立つ Sierra に対していくつかの機能強化が行われます。

- 新しい`NSAllowsArbitraryLoadsInWebContent`キーは、アプリに追加できます`Info.plist`ファイルし、web ページをアプリの残りの部分の Apple のトランスポート セキュリティ (ATS) 保護が有効にまだ中に正しく読み込むことができます。
- 一般的なデータのセキュリティ アーキテクチャ (CDSA) API は廃止されており、非対称キーを生成する SecKey API で置き換える必要があります。
- すべての SSL/TLS 接続の RC4 対称暗号は既定では無効になります。 さらをできるだけ早く SHA 1 および 3 des 暗号化を使用して、アプリを停止することをお勧め、セキュリティで保護されたトランスポート API が不要になった SSLv3 をサポートします。
- IOS 10 や macOS で新しいクリップボード Sierra をコピーして貼り付けるデバイス間でユーザーが許可されているため、API は、クリップボード、特定のデバイスに制限して、特定の時点で自動的にクリアするタイムスタンプを記録に使用できるように拡張されました。 さらに、名前付きペーストはもう残っていませんし、共有ペースト ボード コンテナーで置き換える必要があります。
- アプリ (ユーザーのカレンダーの場合) などの保護されたデータにアクセスする場合、_必要があります_で正しい目的の文字列値のキーでその目的を宣言します。 その`Info.plist`ファイル (`NSCalendarUsageDescription`予定表の場合)。
- Mac App Store を使用して配信される開発者署名済みのアプリでは、CloudKit、iCloud キーチェーン、iCloud ドライブ、リモートのプッシュ通知、MapKit と VPN の権利のようになりました利用できます。
- macOS Sierra では、ランタイムのパスはランタイムの前に、認識されていない外部コードまたはその zip アーカイブまたは符号なしのディスク イメージでコード署名者のアプリとデータを配信をサポートしていません。

MacOS Sierra で実行されているアプリさらに、(またはそれ以降) の 1 つまたは複数のプライバシーに関する特定キーを入力して特定の機能、またはユーザー情報にアクセスしようとするを宣言する必要がありますに静的にその`Info.plist`アプリが取得しようとした理由をユーザーに説明するファイルアクセスします。

MacOS Sierra は、iOS 10 で、これらの変更を共有するため、iOS 10 を参照してください[セキュリティとプライバシーの機能強化](~/ios/app-fundamentals/security-privacy.md)についてガイドします。

<a name="Smart-Card-Driver-Extension-Support" />

### <a name="smart-card-driver-extension-support"></a>スマート カード ドライバーの拡張機能のサポート

MacOS Sierra、アプリを作成できます`NSExtension`スマート カードの特定の種類から、コンテンツへの読み取り専用アクセスを許可するスマート カードのドライバーに基づいています。 この情報は、(非推奨の一般的なデータのセキュリティ アーキテクチャ メソッドに置き換える) システム キーチェーン内に表示されます。

詳細については、Pleas を参照してください Apple の[CryptoTokenKit フレームワーク参照](https://developer.apple.com/reference/cryptotokenkit)です。

<a name="Unified-Logging" />

### <a name="unified-logging"></a>統一されたログ記録

統一されたログ記録は、システムのすべてのレベル間で効率的なメッセージングを単一の API を使用してアプリを提供します。 統合ログ、アプリを持つプライバシー管理と簡単にデバッグ追跡アクティビティを含む複数のレベルのログ記録をきめ細かく制御できます。 

ログ記録は、アクティビティ追跡およびログ記録が一緒に使用するときに、自動メッセージの相関関係を提供します。

macOS Sierra には、新しいコンソール アプリでのアプリケーションおよびユーティリティ) が接続されているデバイスを含む複数のソースからログ データを表示できるが含まれています。 また、トークン化されたおよび保存された検索をサポートし、複数のプロセスで関連するメッセージの間の接続を表示します。

さらに、ログ メッセージは表示でき、コマンド ライン ツールを使用してメンテナンスします。

詳細については、Apple を参照してください[ログ参照](https://developer.apple.com/reference/os/1891852-logging)です。

<a name="Wide-Color" />

### <a name="wide-color"></a>広色

macOS Sierra 拡張範囲のピクセル形式とコア グラフィックス、Core のイメージ、金属および AVFoundation などのフレームワークを含む、システム全体で全体の色域スペースのサポートを拡張します。 ワイド カラー ディスプレイを使用したデバイスのサポートはさらに、グラフィック スタック全体におけるこの動作を提供することにより容易になります。

さらに、`AppKit`が変更された新しい機能を拡張**sRGB**大幅なパフォーマンスを失うことがなくワイド色域内の色を混在させるをやすくするための色空間です。

幅の色を扱うときに、Apple は次のベスト プラクティスを提供します。

- `NSColor` これで、使用、sRGB 領域の色およびしなくなります固定値を`0.0`に`1.0`範囲です。 アプリは、以前のクランプ動作に依存する場合は、macOS Sierra に変更する必要があります。
- コア グラフィックや金属などの低レベルの API を使用して、イメージ処理を提供する、アプリは 16 ビット浮動小数点値をサポートする拡張範囲色領域とのピクセル形式を使用する必要があります。 必要に応じて、アプリがコンポーネントの色の値を手動で固定する必要があります。
- コア グラフィックス、Core のイメージとメタル パフォーマンス シェーダーは、2 つのカラー スペース間で変換するための新しいメソッドを提供します。

詳細については、次を参照してください、[広色の概要](~/ios/platform/wide-color.md)ガイドです。

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>その他のフレームワークの変更

に加えて、主要なフレームワークの変更と追加機能の上に示した Apple 追加マイナー フレームワークの多くの変更に向けた macOS Sierra です。

詳細については、次を参照してください、 [Framework の変更の追加](~/mac/platform/introduction-to-macos-sierra/additional-framework-changes.md)ガイドです。

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>非推奨 API

次の Api が macOS Sierra で廃止されました。

- HFS 標準のファイル システムがサポートされていません。

Apple を参照してください[OS X v10.12 API の Diff](https://developer.apple.com/library/prerelease/content/releasenotes/Miscellaneous/APIDiffsMacOS10_12/index.html)機能廃止と変更の完全な一覧についてはドキュメントです。

## <a name="related-links"></a>関連リンク

- [Mac サンプル](https://developer.xamarin.com/samples/mac/)
- [OS X 10.12 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
