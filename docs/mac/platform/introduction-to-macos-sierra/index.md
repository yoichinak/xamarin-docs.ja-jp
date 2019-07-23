---
title: macOS Sierra の概要
description: この記事では、Xamarin の開発者向け macOS Sierra で提供されている、新しい Api と変更された Api と機能について説明します。
ms.prod: xamarin
ms.assetid: 71A8A737-F310-4320-BD23-743AA1E9033C
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 43497797fe1e740787e531997a62a0ee11deceec
ms.sourcegitcommit: 8fe8d163cb9927917f6a83204b4c387fc50181c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2019
ms.locfileid: "68388481"
---
# <a name="introduction-to-macos-sierra"></a>macOS Sierra の概要

新しい macOS Sierra では、開発者は新しい Api を活用して、エンドユーザーが以前に使用できない方法でアプリや web サイトと対話できるようにすることができます。 たとえば、Apple では、Apple Pay によって安全に支払いを行うことができるようになりました。また、メタルフレームワークの機能強化により、アプリのグラフィックスとコンピューティングの可能性を向上させることができます。 

MacOS Sierra の詳細については、Apple の[macOS とアプリ](https://developer.apple.com/macos/)に関するドキュメントを参照してください。

<a name="Whats-New-in-macOS-Sierra" />

## <a name="whats-new-in-macos-sierra"></a>macOS Sierra の新機能

Apple では、macOS Sierra に新しい Api とサービスがいくつか追加され、既存の機能に対する多くの機能強化が加えられました。

<a name="Apple-File-System" />

### <a name="apple-file-system"></a>Apple ファイルシステム

MacOS Sierra では、Apple は新しい Apple ファイルシステムを iOS、macOS、tvOS、watchOS の最新のファイルシステムとしてリリースしました。 Apple ファイルシステムは、フラッシュおよび SSD ストレージ用に最適化されており、強力な暗号化、書き込み可能なメタデータ、領域の共有、ファイルとディレクトリの複製、スナップショット、ディレクトリの高速サイズ設定、およびアトミックの安全な保存の機能を備えています。

詳細については、Apple の[Apple ファイルシステムガイド](https://developer.apple.com/library/prerelease/content/documentation/FileManagement/Conceptual/APFS_Guide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40016999)を参照してください。

<a name="Apple-Pay-Enhancements" />

### <a name="apple-pay-enhancements"></a>Apple Pay の機能強化

Apple では、ユーザーが web サイトからのセキュリティで保護された支払いを行うことができる macOS Sierra の Apple Pay に対していくつかの機能強化が行われています。

MacOS Sierra では、動的な支払いネットワークと新しいサンドボックステスト環境をサポートするために、macOS Sierra、iOS、および watchOS と連携する新しい Api がいくつか追加されています。

macOS Sierra には、開発者が iOS および macOS の Safari ベースの web サイトに直接 Apple Pay 組み込むことができる新しい ApplePay Javascript フレームワークが含まれています。 Apple Pay をサポートする web サイトでは、ユーザーは iPhone または Apple Watch のいずれかを使用して支払いを承認できます。

詳細については、「Apple の[APPLEPAY JS Framework](https://developer.apple.com/reference/applepayjs)リファレンス」を参照してください。

<a name="Building-Modern-macOS-Apps" />

### <a name="building-modern-macos-apps"></a>最新の macOS アプリの構築

Apple の Safari web ブラウザー、ページワードプロセッサ、数値スプレッドシートなどの最新の macOS アプリでは、多くの新しいテクノロジを使用して、浮動パネルや複数のオープンを含む従来の UI 要素とは別に、一元化された状況依存のユーザーインターフェイスを提供しています。ウィンドウ.

[![タブ付きの Mac ウィンドウの例](images/content08.png)](images/content08.png#lightbox)

[最新の Macos アプリの構築](~/mac/platform/introduction-to-macos-sierra/modern-cocoa-apps.md)ガイドでは、開発者が Xamarin. Mac で最新の macos アプリを構築するために使用できるいくつかのヒント、機能、および手法について説明しています。

<a name="CloudKit-Data-Sharing" />

### <a name="cloudkit-data-sharing"></a>CloudKit データ共有

CloudKit フレームワークは macOS Sierra で拡張され、ユーザーは自分のプライベート iCloud データベースからレコードまたはレコードセットをすばやく簡単に共有できるようになりました。

CloudKit には、共有レコードの招待を送受信するための完全な UI が用意されています。ユーザーは、レコードへのアクセス権を持つユーザーに対して、完全な読み取り/書き込み制御を行うことができます。

詳細については、Apple の[Cloudkit Framework リファレンス](https://developer.apple.com/reference/clockkit)および[Cloudkit JS フレームワークリファレンス](https://developer.apple.com/reference/cloudkitjs)を参照してください。

> [!IMPORTANT]
> Apple からは、開発者が欧州連合の一般データ保護規則 (GDPR) を適切に処理するための[ツールが提供](https://developer.apple.com/support/allowing-users-to-manage-data/)されています。

<a name="Safari-App-Extensions-Support" />

### <a name="safari-app-extensions-support"></a>Safari アプリ拡張機能のサポート

Safari アプリ拡張機能を使用すると、アプリは macOS Sierra と密接に統合されながら、Safari web ブラウザーの動作を拡張することができます。 MacOS Safari アプリ拡張機能は iOS Safari アプリ拡張機能に似ているため、あるシステムから別のシステムに簡単に移植できます。

詳細については、「Apple の[Safari アプリ拡張機能のプログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternetWeb/Conceptual/SafariAppExtension_PG/index.html#//apple_ref/doc/uid/TP40017319)」を参照してください。

<a name="Security-and-Privacy-Enhancements" />

### <a name="security-and-privacy-enhancements"></a>セキュリティとプライバシーの強化

Apple では、アプリのセキュリティを向上させ、次のようなエンドユーザーのプライバシーを確保するために、アプリのセキュリティを強化するために、macOS Sierra のセキュリティとプライバシーの両方に対していくつかの機能強化が行われています。

- 新しい`NSAllowsArbitraryLoadsInWebContent`キーはアプリの`Info.plist`ファイルに追加することができ、Apple Transport Security (ATS) の保護はアプリの残りの部分でも有効になっていますが、web ページを正しく読み込むことができます。
- Common Data Security Architecture (CDSA) API は非推奨とされており、非対称キーを生成するには SecKey API で置き換える必要があります。
- すべての SSL/TLS 接続では、RC4 対称暗号が既定で無効になっています。 さらに、セキュリティで保護されたトランスポート API は SSLv3 をサポートしなくなりました。アプリは、できるだけ早く SHA-1 と3DES 暗号化の使用を停止することをお勧めします。
- IOS 10 と macOS Sierra の新しいクリップボードを使用すると、デバイス間でコピーと貼り付けを行うことができます。そのため、特定のデバイスに対してクリップボードを制限し、特定の時点で自動的に消去されるようにタイムスタンプを設定できるように、API が拡張されています。 また、名前付き pasteboards は保持されなくなったため、共有のペーストボードコンテナーに置き換える必要があります。
- アプリが保護されたデータにアクセスする場合 (ユーザーの予定表など) は、その目的を正しい文字列値のキー `Info.plist`でファイル (`NSCalendarUsageDescription`暦の場合) で宣言_する必要があり_ます。
- Mac App Store 経由で配信されていない開発者向け署名済みアプリでは、CloudKit、iCloud キーチェーン、iCloud ドライブ、リモートプッシュ通知、MapKit、および VPN 権利を利用できるようになりました。
- ランタイムパスはランタイムの前にわからないため、macOS Sierra は、zip アーカイブまたは署名されていないディスクイメージで、コード署名者アプリと共に外部コードまたはデータの配信をサポートしなくなりました。

さらに、macOS Sierra (またはそれ以降) で実行されているアプリでは、特定の機能やユーザー情報にアクセスする目的を静的`Info.plist`に宣言する必要があります。そのためには、ファイルにプライバシー固有のキーを1つ以上入力しますaccess.

MacOS Sierra は iOS 10 でこれらの変更を共有するため、詳細については、iOS 10 の[セキュリティとプライバシーの強化](~/ios/app-fundamentals/security-privacy.md)に関するガイドを参照してください。

<a name="Smart-Card-Driver-Extension-Support" />

### <a name="smart-card-driver-extension-support"></a>スマートカードドライバーの拡張機能のサポート

MacOS Sierra では、アプリは、 `NSExtension`特定の種類のスマートカードからのコンテンツへの読み取り専用アクセスを許可する、ベースのスマートカードドライバーを作成できます。 この情報は、システムキーチェーン内に表示されます (非推奨の Common Data Security Architecture メソッドを置き換えます)。

詳細については、「Apple の[Cryptotokenkit フレームワークリファレンス](https://developer.apple.com/reference/cryptotokenkit)」を参照してください。

<a name="Unified-Logging" />

### <a name="unified-logging"></a>統合ログ

統合ログを使用すると、すべてのレベルのシステム間で効率的なメッセージングを行うための単一の API をアプリに提供できます。 統合ログを使用すると、アプリは、プライバシー管理や、デバッグを容易にするためのアクティビティ追跡を含む、複数レベルのログ記録をきめ細かく制御できます。 

ログ記録は、アクティビティの追跡とログが一緒に使用されるときに、メッセージの自動関連付けを行います。

macOS Sierra には、接続されているデバイスを含む複数のソースからのログデータを表示できる新しいコンソールアプリ (アプリケーション/ユーティリティ) が含まれています。 また、トークン化および保存された検索をサポートし、複数のプロセス間で関連するメッセージ間の接続を表示します。

また、コマンドラインツールを使用してログメッセージを表示および管理することもできます。

詳細については、Apple の[ログ記録のリファレンス](https://developer.apple.com/documentation/os/logging)を参照してください。

<a name="Wide-Color" />

### <a name="wide-color"></a>広色域

macOS Sierra は、コアグラフィックス、コアイメージ、メタル、AVFoundation などのフレームワークを含む、システム全体の拡張範囲のピクセル形式と広い範囲の色空間のサポートを拡張します。 グラフィックススタック全体でこの動作を提供することにより、さまざまな色で表示されるデバイスのサポートがさらに緩和さます。

また、 `AppKit`は新しい extended **sRGB** colorspace で動作するように変更されています。これにより、パフォーマンスが大幅に低下することなく、広い色域で色を簡単に混在させることができます。

Apple では、広範囲にわたる色を使用するときに、次のベストプラクティスを提供しています。

- `NSColor`では、sRGB 色空間が使用されるようになり、 `0.0`値`1.0`が to 範囲にクランプされなくなりました。 アプリが以前のクランプ動作に依存している場合は、macOS Sierra に対して変更する必要があります。
- コアグラフィックスや金属などの低レベルの API を使用してイメージ処理を行う場合、アプリでは16ビット浮動小数点値をサポートする拡張範囲の色空間とピクセル形式を使用する必要があります。 必要に応じて、アプリで色コンポーネントの値を手動で固定する必要があります。
- コアグラフィックス、コアイメージ、および金属パフォーマンスシェーダーはすべて、2つの色空間間で変換を行うための新しいメソッドを提供します。

詳細については、「 [Wide カラー](~/ios/platform/wide-color.md)ガイドの概要」を参照してください。

<a name="Additional-Framework-Changes" />

## <a name="additional-framework-changes"></a>追加のフレームワークの変更

Apple では、上に示したフレームワークの主な変更点と追加機能に加えて、macOS Sierra に多くの重要なフレームワーク変更が加えられています。

詳細については、追加の[フレームワーク変更](~/mac/platform/introduction-to-macos-sierra/additional-framework-changes.md)ガイドを参照してください。

<a name="Deprecated-APIs" />

## <a name="deprecated-apis"></a>非推奨の API

次の Api は macOS Sierra で非推奨とされました。

- HFS 標準ファイルシステムはサポートされなくなりました。

廃止と変更の完全な一覧については、Apple の[macOS v 10.12 API の相違](https://developer.apple.com/library/archive/releasenotes/General/APIDiffsMacOS10_12/index.html)点に関するドキュメントを参照してください。

## <a name="related-links"></a>関連リンク

- [Mac サンプル](https://developer.xamarin.com/samples/mac/)
- [MacOS 10.12 の新機能](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
