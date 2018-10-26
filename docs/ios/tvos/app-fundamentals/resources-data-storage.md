---
title: tvOS リソースと Xamarin でのデータ ストレージ
description: この記事では、リソースと Xamarin でビルドされた tvOS アプリでの永続的なデータ ストレージを操作する方法について説明します。 ICloud のデータ ストレージと、オンデマンドのリソースがについて説明します。
ms.prod: xamarin
ms.assetid: C56B5046-D2C0-4B63-9CE0-ADAA0EFD368A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 8f8ecc115738fb97f71b4ea6b2cdcc5c2714372d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108185"
---
# <a name="tvos-resources-and-data-storage-in-xamarin"></a>tvOS リソースと Xamarin でのデータ ストレージ

_この記事では、リソースと Xamarin.tvOS アプリでの永続的なデータ記憶域の使用について説明します。_

<a name="tvOS-Resource-Limitations" />

## <a name="tvos-resource-limitations"></a>tvOS リソースの制限事項

IOS デバイスとは異なり、新しい Apple TV は tvOS アプリまたはデータの非常に限られた永続的なローカル ストレージを提供します。 (ユーザー設定) などの非常に小さいアイテムの tvOS アプリがまだへのアクセス`NSUserDefaults`で、 [500 KB のデータの上限](https://forums.developer.apple.com/message/50696#50696)します。 ただし、Xamarin.tvOS アプリは、大量の情報を保持する必要がある場合、する必要がありますを格納し、そのデータを取得[iCloud](#iCloud-Data-Storage)します。

さらに、tvOS では、Apple TV にアプリを 200 MB のサイズが制限されます。 必要なアプリは、このサイズを超えるリソースを必要とする場合はパッケージ化しを使用して読み込む[オンデマンド リソース](#On-Demand-Resources)(最大 2 GB の追加) します。 これらの制限を指定するには、ことが重要する正しくときに、アプリのユーザーに最適なエクスペリエンスを提供するその他のアセットをダウンロードします。 詳細については、Apple を参照してください[オンデマンド リソース ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083)します。

<a name="Non-Persistent-Downloads" />

## <a name="non-persistent-downloads"></a>非永続的なダウンロード

TvOS アプリごとに、その他のリソースやアセットをダウンロードする一時的なキャッシュ ディレクトリは提供されます。 アプリがまだ実行されている限り、このディレクトリが保持されます。 ただし、Apple TV では、他のアプリやシステムの使用量の領域を解放する必要がある、このキャッシュを削除することができます。

その結果、アプリは、次回の起動を利用できない以前にダウンロードしたコンテンツを使用できません。 Xamarin.tvOS アプリを常に必要なリソースの存在を確認し、必要に応じてダウンロードします。

> [!IMPORTANT]
> その他の資産および必要に応じてリソースをダウンロードする機能がありますが、Apple は、予期しない結果につながりますに対してすべてのアプリのキャッシュ内の領域を消費してに警告します。




<a name="Managing-Resources" />

## <a name="managing-resources"></a>リソースの管理

これらの制限では、制限されているのため、tvOS アプリに使用できる情報の非永続的ストレージを前述のよう、Xamarin.tvOS アプリの優れたユーザー エクスペリエンスを作成する場合は注意が必要ですが必要です。

<a name="iCloud-Data-Storage" />

### <a name="icloud-data-storage"></a>iCloud データ ストレージ

Apple TV 上の記憶域が限られているためだけでなく、非常に限られた永続的なローカル記憶域、アプリがない以前にダウンロードしたすべての情報を表示する、次回に実行される保証します。

その結果、Xamarin.tvOS アプリは、iCloud データ ストアでユーザー データを格納する必要があります。 Apple では、tvOS アプリの 2 つの iCloud ベースのデータ ストレージ オプションが用意されています。

- **iCloud Key-value ストレージ (KVS)** - 小規模な (1 MB 未満) をアプリが必要な情報 (ユーザー設定) のような iCloud KVS ストレージを使用することができます。 iCloud KVS データは、クラウドと同じアプリを実行しているユーザーのデバイスのすべてに自動的に同期されます。 詳細情報を参照してください、 [Key-value ストレージ](~/ios/data-cloud/introduction-to-icloud.md)のセクション、 [iCloud の概要](~/ios/data-cloud/introduction-to-icloud.md)ドキュメントや Apple の[iCloud のキーと値のデータの設計](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForKey-ValueDataIniCloud.html#//apple_ref/doc/uid/TP40012094-CH7)ドキュメントです。
- **CloudKit** - (1 MB より大きい)、情報の大きいピースの記憶域が CloudKit の Apple のフレームワークを使用します。 ICloud KVS ストレージとは異なり CloudKit のデータは、アプリと同様に 1 人のユーザーにプライベート) のすべてのユーザーの間で共有できます。 フォームの詳細についてを参照してください、 [CloudKit の概要](~/ios/data-cloud/intro-to-cloudkit.md)ドキュメントや Apple の[CloudKit のクイック スタート](https://developer.apple.com/library/prerelease/tvos/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987)します。

> [!IMPORTANT]
> Apple からは、開発者が欧州連合の一般データ保護規則 (GDPR) を適切に処理するための[ツールが提供](https://developer.apple.com/support/allowing-users-to-manage-data/)されています。

<a name="On-Demand-Resources" />

### <a name="on-demand-resources"></a>オンデマンド リソース

オンデマンド リソースは、アプリのコンテンツと、アプリ ストアでホストされ、アプリからの要求としてダウンロードする (アプリのバンドルから独立した) 資産を提供します。 データの追加の 2 GB まで提供できます、オンデマンド リソースを使用します。 小さくなるように、最初のアプリのダウンロードを有効にする (tvOS アプリは、200 MB の最大数に制限されていますが、) 必要に応じて豊富な資産を提供しながらします。

TvOS アプリを要求すると、オンデマンドでリソースをダウンロードし、アプリのキャッシュ ディレクトリには、このコンテンツの記憶域システムが自動的に管理します。 アプリは、このコンテンツをダウンロードし、続行する前に、利用を待つ必要があります。

これらのリソースは、高速化起動サイクルではそのため、アプリの複数の起動で Apple TV でキャッシュを続行できます。 ただし、アプリは、次回の起動を利用できない、以前ダウンロードしたコンテンツを使用できません。 参照してください、[非永続的なダウンロード](#Non-Persistent-Downloads)詳細については、前述の「します。

Xcode を使用すると、特定のリソース タグに関連付けられている関連コンテンツ (すべての資産のゲーム レベル 2) などのバンドルを作成できます。 後で、アプリはこのリソースのタグを指定することで、オンデマンド リソースを要求します。 アプリは、そのコンテンツのことを通知する UI を提示する必要がありますがダウンロードされています。 詳細については、Apple を参照してください[オンデマンド リソース ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083)します。

> [!IMPORTANT]
> アプリがオンデマンドでリソースをダウンロードする回数と個別のダウンロードのサイズの適正なバランスを取るように注意する必要があります。 ユーザーは、新しいコンテンツをダウンロードするがゲームプレイを常に中断された場合、または 1 つのダウンロード時間がかかりすぎる場合に、アプリに不満を感じるになる可能性があります。




<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、tvOS システムによって Xamarin.tvOS アプリ上に配置のサイズ、リソース、データ ストレージの制限をについて説明します。 これには、これらの制限事項と、アプリの優れたユーザー エクスペリエンスを作成する推奨事項を回避するためのオプションが表示されます。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
