---
title: Xamarin.iOS での search Api
description: この記事では、iOS 9 で提供される新しいアプリ検索 Api を使用して、情報と、Xamarin.iOS アプリ内の機能を検索するユーザーを許可するについて説明します。
ms.prod: xamarin
ms.assetid: 7323EB3D-A78F-4BF0-9990-3160C7E83CF0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 799d6dd532e530f5ee9c9a974b2d93b6a3be0efb
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50122414"
---
# <a name="search-apis-in-xamarinios"></a>Xamarin.iOS での search Api

_この記事では、iOS 9 で提供されるアプリの検索 Api を使用して、情報と、Xamarin.iOS アプリ内の機能を検索するユーザーを許可するについて説明します。_

検索は、iOS 9 で、提供情報と Xamarin.iOS アプリ内の機能にアクセスする新しい方法で拡張されています。 新しいアプリの検索 Api を使用して、アプリのコンテンツは、Siri アラームと提案など、スポット ライトと Safari の検索結果を検索可能なされます。 これにより、アクティビティと、アプリ内で詳細な情報にすばやくアクセスできます。

さらに、新しい検索 Api を使用すると、以前の検索の実装機能せず、アプリで検索を統合しやすくします。 このため、Apple は iOS 9 アプリのコンテンツをアプリの検索を使用して検索可能な汎用的に数時間を要するそのことを要求します。

[![](images/intro01.png "IOS 9 アプリのコンテンツ アプリの検索を使用して検索可能な汎用の例")](images/intro01.png#lightbox)

アプリの検索では、3 つの個別の Api で構成されます。

1. [**NSUserActivity** ](nsuseractivity.md) -これは、Apple は iOS 8 でリリースされたハンドオフ API の拡張機能。 アプリの操作の履歴を検索できるようにパブリックとプライベートの両方に使用されます)、ユーザーがいます。

2. [**コア スポット ライト**](corespotlight.md) -アプリに検索結果に表示する内容のインデックスを許可します。 データベース API は、アプリ内でプライベート コンテンツのインデックスする最善の方法および項目を追加および削除できる場所のように動作します。

3. [**WebMarkup** ](web-markup.md) - web インターフェイスを使用してそのコンテンツへのアクセスを提供するアプリ (からだけでなく、アプリ内)。 Web コンテンツは、Apple によってクロールして、ユーザーの iOS 9 デバイスのアプリへのディープ リンクを提供できる特別なリンクでマークできます。

## <a name="selecting-an-app-search-approach"></a>アプリの検索方法の選択

これらのメソッドを実装するを決定は、アプリによって提供される対話の種類とコンテンツの表示の種類によって異なります。

次のガイドラインに従います。

- [**NSUserActivity** ](nsuseractivity.md) – このフレームワークを使用して、パブリックおよびプライベートのコンテンツ両方と、アプリ内でのナビゲーションのポイントの検索機能の検索機能を提供します。

- [**コア スポット ライト**](corespotlight.md) – このフレームワークを使用して、デバイスに格納されているプライベート データの検索機能を提供します。

- [**Web マークアップ**](web-markup.md) – からだけでなく、アプリ内では、アプリの web サイトもからコンテンツを表示するアプリの検索機能を提供するこのフレームワークを使用します。

アプローチは distinct とは、アプリ検索の各個別に使用、しかし Apple が設計に連携して動作します。 アプローチの 1 つ以上の特定の項目のインデックスを使用する場合は、同じを使用することを確認**項目 ID**それぞれの方法でそのためその個人結び付ける作業します。

アプローチの 1 つ以上のだけでなくを使うと、コンテンツは、エンドユーザーが見つかるが、検索内の項目の順位付けを向上させることができます。

順位付けのプロセス中にほとんどの場合は開発者に透過的な特定の項目との対話をユーザーの重さが大きくこの順位付け (たとえばユーザー リンクをタップ) するとします。
豊富な情報の項目を提供すること、ユーザーが高くなります、コンテンツと対話するため、ランクの発生を保証できます。

## <a name="what-content-to-index"></a>インデックスにどのようなコンテンツ

Apple では、アプリでの検索インデックスを提供するどのようなコンテンツと操作に関して次の推奨事項を提供します。

 - 任意のコンテンツは、表示、作成またはアプリ内からユーザーが管理します。
 - ナビゲーションのポイントとアプリ内で機能します。
 - 新しいメッセージやコンテンツなど、デバイスを最近ダウンロードされたアプリで表示される項目の種類をなど。

## <a name="app-search-enhancements"></a>アプリの検索の機能強化

IOS 10 でコア スポット ライトは、次のアプリを検索するいくつかの機能強化を提供します。

- **(差分プライバシー) のディープ リンクの支持を引き出します**-検索結果でのディープ リンク アプリのコンテンツを昇格する方法を提供します。
- **アプリ内検索**-使用して、新しい`CSSearchQuery`メール、Messages や Notes アプリの動作方法と同様のアプリ内スポット ライト検索機能を提供するクラス。
- **継続を検索**- ユーザーをスポット ライトや、Safari で検索を開始し、アプリを開くし、その検索を続行します。
- **検証結果の視覚化**-Apple の[アプリ検索 API 検証ツール](https://search.developer.apple.com/appsearch-validation-tool)テストの実行時に web サイトのマークアップとディープ リンクのビジュアル表現が表示されます。
- **アプリの画像の共有をメッセージ**-スポット ライト検索で表示するメッセージ (メッセージ アプリ拡張機能) 経由で共有するために提供される一般的なアプリ内のイメージを使用します。

詳細については、次を参照してください、[アプリ検索の機能強化](~/ios/platform/search/app-search-enhancements.md)ガイド。

### <a name="proactive-suggestions"></a>プロアクティブな候補

iOS 10 では、事前に提示する役に立つ情報に自動的にユーザーに適切なタイミングでシステムを許可することで運転 engagement アプリへの新しい方法を表示します。 同様の iOS 9 には、ディープ検索アプリは、次の場所内で、システムにより、ユーザーに表示することができます機能を公開できる iOS 10 のスポット ライト、ハンドオフおよび Siri の推奨事項を使用してアプリを追加する機能が用意されています。

- アプリケーションのスイッチャー
- ロック画面
- CarPlay
- マップ
- Siri の相互作用
- QuickType 提案 

アプリなどのテクノロジのコレクションを使用して、システムには、この機能を公開[NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/)、コア スポット ライト、MapKit、Media Player、UIKit、web マークアップ。

詳細については、次を参照してください、[プロアクティブな候補](~/ios/platform/search/proactive-suggestions.md)ガイド。

## <a name="summary"></a>まとめ

この記事では、新しい対象が Xamarin.iOS アプリの検索 API の機能を iOS 9 を提供します。 対応して[NSUserActivity](nsuseractivity.md)、[コア スポット ライト](corespotlight.md)と[Web マークアップ](web-markup.md)コンテンツのインデックス作成の方法です。 特定の検索方法を使用して、コンテンツの種類にする必要がありますの簡単な説明と終了インデックスを作成します。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 開発者向け](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [アプリの検索のプログラミング ガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
