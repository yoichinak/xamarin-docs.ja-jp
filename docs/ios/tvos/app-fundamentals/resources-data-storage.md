---
title: Xamarin でのリソースとデータストレージの tvOS
description: この記事では、Xamarin でビルドされた tvOS アプリでリソースと永続的なデータストレージを操作する方法について説明します。 ICloud のデータストレージとオンデマンドリソースについて説明します。
ms.prod: xamarin
ms.assetid: C56B5046-D2C0-4B63-9CE0-ADAA0EFD368A
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/16/2017
ms.openlocfilehash: e9c27233e0905435e14505195b7b0e4d3283790b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283827"
---
# <a name="tvos-resources-and-data-storage-in-xamarin"></a>Xamarin でのリソースとデータストレージの tvOS

_この記事では、tvOS アプリでのリソースと永続データストレージの使用について説明します。_

<a name="tvOS-Resource-Limitations" />

## <a name="tvos-resource-limitations"></a>tvOS リソースの制限事項

IOS デバイスとは異なり、新しい Apple TV は、tvOS アプリまたはデータに対して非常に限定された永続的なローカルストレージを提供します。 非常に小さな項目 (ユーザー設定など) の場合、tvOS アプリでは、 `NSUserDefaults` [500 KB のデータの制限](https://forums.developer.apple.com/message/50696#50696)でにアクセスできます。 ただし、tvOS アプリでより多くの情報を保持する必要がある場合は、 [iCloud](#iCloud-Data-Storage)からそのデータを格納して取得する必要があります。

さらに、tvOS では Apple TV アプリのサイズが 200 MB に制限されています。 アプリがこのサイズを超えるリソースを必要とする場合は、[オンデマンドリソース](#On-Demand-Resources)(最大で 2 gb まで) を使用してパッケージ化して読み込む必要があります。 これらの制限事項を考慮して、アプリのユーザーに最適なエクスペリエンスを提供するために、追加のアセットのダウンロードを正しく行うことが重要です。 詳細については、Apple の[オンデマンドリソースに関するガイド](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083)を参照してください。

<a name="Non-Persistent-Downloads" />

## <a name="non-persistent-downloads"></a>非永続的ダウンロード

各 tvOS アプリには、追加のリソースとアセットのダウンロード先となる一時的なキャッシュディレクトリが用意されています。 このディレクトリは、アプリがまだ実行されている限り保持されます。 ただし、Apple TV は他のアプリやシステムの使用のために空き領域を確保する必要があるため、このキャッシュを削除することができます。

その結果、アプリは、次回の起動時に使用可能なコンテンツに依存することはできません。 TvOS アプリでは、必要なリソースがあるかどうかを常に確認し、必要に応じてダウンロードする必要があります。

> [!IMPORTANT]
> 必要に応じて他のアセットやリソースをダウンロードすることができますが、アプリのキャッシュ内のすべての領域を消費することに対して警告が表示されます。これは予測できない結果につながる可能性があるためです。




<a name="Managing-Resources" />

## <a name="managing-resources"></a>リソースの管理

前述のように、tvOS アプリで利用できる情報の限定された非永続的ストレージであるため、これらの制限には、tvOS アプリの優れたユーザーエクスペリエンスを作成するための慎重な計画が必要です。

<a name="iCloud-Data-Storage" />

### <a name="icloud-data-storage"></a>iCloud データストレージ

Apple TV の記憶域は制限されているため、永続的なローカルストレージが非常に限られているだけでなく、以前にダウンロードした情報が次回実行されたときに利用可能になるという保証はありません。

その結果、tvOS アプリは、iCloud データストアにすべてのユーザーデータを保存する必要があります。 Apple では、tvOS アプリ用の iCloud ベースのデータストレージオプションが2つ用意されています。

- **Icloud キー-値ストレージ (KVS)** -アプリが必要とする可能性のある (1 mb 未満の) 情報 (ユーザー設定など) については、Icloud KVS ストレージを使用できます。 iCloud KVS データは、同じアプリを実行しているすべてのユーザーのデバイスで、クラウドとすべてのユーザーのデバイスに自動的に同期されます。 詳細については、icloud ドキュメントの[概要に](~/ios/data-cloud/introduction-to-icloud.md)関する記事の「[キー値のストレージ](~/ios/data-cloud/introduction-to-icloud.md)」セクションを参照するか、Icloud のドキュメント[でキー値データに対する](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForKey-ValueDataIniCloud.html#//apple_ref/doc/uid/TP40012094-CH7)Apple の設計を参照してください。
- **Cloudkit** -より大きな情報 (1 mb を超える) を格納するには、Apple の Cloudkit フレームワークを使用します。 ICloud KVS ストレージとは異なり、CloudKit データはアプリのすべてのユーザー間で共有できます (また、1人のユーザーにプライベートにすることもできます)。 詳細については、 [CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)のドキュメントまたは Apple の[Cloudkit クイックスタート](https://developer.apple.com/library/prerelease/tvos/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987)の概要を参照してください。

> [!IMPORTANT]
> Apple からは、開発者が欧州連合の一般データ保護規則 (GDPR) を適切に処理するための[ツールが提供](https://developer.apple.com/support/allowing-users-to-manage-data/)されています。

<a name="On-Demand-Resources" />

### <a name="on-demand-resources"></a>オンデマンドリソース

オンデマンドリソースはアプリのコンテンツとアセット (アプリバンドルとは別のもの) を提供します。アプリストアでホストされ、アプリによって必要に応じてダウンロードされます。 オンデマンドリソースを使用して、さらに 2 GB のデータを提供できます。 これにより、最初のアプリのダウンロードを小さくすることができます (tvOS アプリは最大 200 MB に制限されています) が、必要に応じて豊富なアセットを提供します。

TvOS アプリがオンデマンドリソースを要求すると、システムはこのコンテンツのダウンロードと保存をアプリのキャッシュディレクトリに自動的に管理します。 アプリは、このコンテンツがダウンロードされ、利用可能になるのを待ってから続行する必要があります。

これらのリソースは、アプリの複数の起動を通じて Apple TV にキャッシュされ続け、起動サイクルが短縮されます。 ただし、アプリは、次回の起動時に使用可能なコンテンツに依存することはできません。 詳細については、上記の「[非永続的ダウンロード](#Non-Persistent-Downloads)」セクションを参照してください。

Xcode を使用して、リソースの提供タグに関連付けられている関連コンテンツ (ゲームレベル2のすべての資産など) のバンドルを作成します。 その後、アプリは、このリソースタグを指定することによってオンデマンドリソースを要求します。 アプリでは、コンテンツがダウンロードされていることを示す UI をユーザーに表示する必要があります。 詳細については、Apple の[オンデマンドリソースに関するガイド](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083)を参照してください。

> [!IMPORTANT]
> 必要に応じて、アプリがオンデマンドリソースをダウンロードする回数と個々のダウンロードのサイズとのバランスを取る必要があります。 ゲームプレイが継続的に中断されて新しいコンテンツをダウンロードする場合や、1回のダウンロードに時間がかかりすぎる場合は、ユーザーがアプリに不満を感じてしまう可能性があります。




<a name="Summary" />

## <a name="summary"></a>Summary

この記事では、tvOS システムによって tvOS アプリに適用されるサイズ、リソース、およびデータストレージの制限事項について説明しました。 アプリの優れたユーザーエクスペリエンスを実現するために、これらの制限と提案を回避するオプションが示されています。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
