---
title: "リソースとデータ ストレージ"
description: "この記事では、リソースと Xamarin.tvOS アプリ内の永続的なデータ ストレージの操作について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: C56B5046-D2C0-4B63-9CE0-ADAA0EFD368A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: e72c013516de5bcf97e2e9f58a7a4f5cd87c730b
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2018
---
# <a name="resources-and-data-storage"></a>リソースとデータ ストレージ

_この記事では、リソースと Xamarin.tvOS アプリ内の永続的なデータ ストレージの操作について説明します。_

<a name="tvOS-Resource-Limitations" />

## <a name="tvos-resource-limitations"></a>tvOS リソースの制限事項

IOS デバイスとは異なりは、新しい Apple TV は、tvOS アプリやデータのごく限られた範囲の永続的でローカル ストレージを提供します。 (ユーザー設定) などの非常に小さいアイテムに対して、tvOS アプリがまだへのアクセス`NSUserDefaults`で、 [500 KB のデータの上限](https://forums.developer.apple.com/message/50696#50696)です。 ただし、Xamarin.tvOS アプリは、大量の情報を保持する必要がある場合、必要がありますを格納し、データを取得するから[iCloud](#iCloud-Data-Storage)です。

さらに、tvOS は 200 MB を Apple TV アプリのサイズを制限します。 アプリは、このサイズよりもリソースを必要とする場合に、必要があります。 パッケージ化され、を使用して読み込む[オンデマンド リソース](#On-Demand-Resources)(最大 2 GB の追加) します。 これらの制限を指定するには、それを正しくときに、アプリのユーザーに最適なエクスペリエンスを提供するその他の資産のダウンロードが重要です。 詳細については、Apple を参照してください[のオンデマンド リソース ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083)です。

<a name="Non-Persistent-Downloads" />

## <a name="non-persistent-downloads"></a>非持続のダウンロード

各 tvOS アプリには、その他のリソースに、資産にダウンロードされている一時的なキャッシュ ディレクトリは提供されます。 このディレクトリは、アプリがまだ実行されている限り保持されます。 ただし、Apple TV は、他のアプリやシステムの使用量の領域を解放する必要がある、このキャッシュを削除することができます。

その結果、アプリは、以前にダウンロードしたコンテンツを利用可能な次回起動時に依存できません。 Xamarin.tvOS アプリを常に必要なリソースの有無を確認し、必要に応じてそれらをダウンロードします。

> [!IMPORTANT]
> 他の資産および必要に応じてリソースをダウンロードすることがありますが、予期しない結果になることができます、に対してすべてのアプリのキャッシュ内の領域を消費して Apple に警告します。




<a name="Managing-Resources" />

## <a name="managing-resources"></a>リソースの管理

、限定されるため、tvOS アプリに使用できる情報の非永続的なストレージを前述のようこれらの制限では、慎重に計画 Xamarin.tvOS アプリの優れたユーザー エクスペリエンスを作成する必要があります。

<a name="iCloud-Data-Storage" />

### <a name="icloud-data-storage"></a>iCloud データ ストレージ

Apple TV の記憶域が限られているためだけでなく、非常に限定された永続的なローカル記憶域、アプリがありません以前にダウンロードしたすべての情報が利用できる、次回実行されるという保証します。

その結果、Xamarin.tvOS アプリでは、データ ストアを iCloud に任意のユーザー データを格納する必要があります。 Apple は、tvOS アプリ用の 2 つの iCloud ベースのデータ記憶域オプションを提供します。

- **iCloud キーと値ストレージ (KVS)** - は小規模な情報 (1 MB 未満) アプリ必要があること (ユーザーの設定など) のような iCloud KVS 記憶域を使用することができます。 iCloud KVS データが、クラウドと、同じアプリを実行しているユーザーのデバイスのすべてに自動的に同期します。 詳細情報を参照してください、[キーと値の記憶域](~/ios/data-cloud/introduction-to-icloud.md)のセクション、 [iCloud 概要](~/ios/data-cloud/introduction-to-icloud.md)ドキュメントや Apple の[iCloud にキーと値のデータの設計](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForKey-ValueDataIniCloud.html#//apple_ref/doc/uid/TP40012094-CH7)ドキュメントです。
- **CloudKit** : (1 MB より大きい)、情報の大きな部分の記憶域は、Apple の CloudKit フレームワークを使用します。 ICloud KVS 記憶域とは異なり CloudKit データはアプリ (だけでなく、単一のユーザーにプライベート) のすべてのユーザー間で共有できます。 フォームの詳細についてを参照してください、 [CloudKit 概要](~/ios/data-cloud/intro-to-cloudkit.md)ドキュメントや Apple の[CloudKit クイック スタート](https://developer.apple.com/library/prerelease/tvos/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987)です。

<a name="On-Demand-Resources" />

### <a name="on-demand-resources"></a>オンデマンド リソース

オンデマンドでのリソースは、アプリのコンテンツやアプリ ストアでホストされ、アプリで必要に応じてダウンロードする (アプリのバンドルから個別)、資産を提供します。 データの追加の 2 GB まで処理のオンデマンド リソースの使用します。 小さくなるように、最初のアプリのダウンロードを有効にする (tvOS アプリは、200 MB の最大数に制限は、) 必要に応じて豊富な資産を提供しながらです。

TvOS アプリでは、オンデマンドでリソースを要求するときに、システムは、ダウンロードして、アプリのキャッシュ ディレクトリにこのコンテンツの記憶域を自動的に管理します。 アプリは、このコンテンツをダウンロードし、続行する前に使用できるを待つ必要があります。

これらのリソースを複数起動し、アプリのための高速化起動サイクル全体で Apple TV にキャッシュされる続行できます。 ただし、アプリは、次回起動時にも使用できない、以前にダウンロードしたコンテンツに依存できません。 参照してください、 [Non-persistent ダウンロード](#Non-Persistent-Downloads)の詳細については、上記「します。

付与リソース タグに関連付けられている関連コンテンツ (ゲーム レベル 2 のすべての資産) などのバンドルを作成するのにには、Xcode を使用します。 後で、アプリでは、このリソースのタグを指定することで、オンデマンド リソースを要求します。 アプリは、そのコンテンツのというユーザーに UI を提示する必要がありますのダウンロード中です。 詳細については、Apple を参照してください[のオンデマンド リソース ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/FileManagement/Conceptual/On_Demand_Resources_Guide/index.html#//apple_ref/doc/uid/TP40015083)です。

> [!IMPORTANT]
> アプリには、オンデマンドのリソースをダウンロードする回数を超えると、個別のダウンロードのサイズの適正なバランスをとろうと注意する必要があります。 新しいコンテンツをダウンロードするゲームを常に中断された場合、または単一のダウンロード時間がかかりすぎる場合、ユーザーはアプリに不満をしまいます。




<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、tvOS システムで Xamarin.tvOS アプリに加えられたサイズ、リソース、データ ストレージの制限について説明しました。 これには、これらの制限事項と、アプリの優れたユーザー エクスペリエンスを作成する推奨事項を回避するオプションが表示されます。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
