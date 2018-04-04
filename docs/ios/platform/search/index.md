---
title: 新しい検索 Api
description: この記事では、Api を使用して、新しいアプリ検索 iOS 9 によって提供される情報と、Xamarin.iOS アプリ内の機能を検索できるようにするについて説明します。
ms.prod: xamarin
ms.assetid: 7323EB3D-A78F-4BF0-9990-3160C7E83CF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 5dcd3d9665befaa82fd0f5677a4a662f633ed45b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="new-search-apis"></a>新しい検索 Api

_この記事では、Api を使用して、新しいアプリ検索 iOS 9 によって提供される情報と、Xamarin.iOS アプリ内の機能を検索できるようにするについて説明します。_

検索は、iOS 9 情報や、Xamarin.iOS アプリ内の機能にアクセスする新しい方法を提供するで展開されています。 新しいアプリの検索 Api を使用して、アプリのコンテンツが行われますハンドオフと Siri アラームと推奨事項など、メディア、および Safari の検索結果を検索可能です。 これにより、アクティビティと、アプリ内で詳細な情報にすばやくアクセスできます。

さらに、新しい Search Api を使用すると、以前の検索の実装機能せず、アプリで検索を統合しやすくします。 このため、Apple は、通常して、iOS 9 アプリのコンテンツを検索可能ユニバーサル アプリの検索を使用してほんの数時間を要することを要求します。

[![](images/intro01.png "IOS 9 アプリ コンテンツ アプリの検索を使用して検索可能な汎用の例")](images/intro01.png#lightbox)

アプリの検索は、3 つの独立した Api で構成されます。

1. [**NSUserActivity** ](nsuseractivity.md) -これは、Apple は iOS 8 でリリースされたハンドオフ API の拡張機能です。 公開および非公開でアプリの相互作用の履歴を検索できるようにするために使用されます)、ユーザーがします。

2. [**スポット ライトのコア**](corespotlight.md) -を使用するアプリに検索結果に表示するには、そのコンテンツのインデックスを作成します。 データベース API で項目を追加、削除であり、アプリ内でプライベート コンテンツのインデックスに最適な方法と同様に動作します。

3. [**WebMarkup** ](web-markup.md) - web インターフェイスを使用して、コンテンツへのアクセスを提供するアプリ (からだけでなく、アプリ内で)。 Web コンテンツは、Apple によってクロールされ、ユーザーの iOS 9 デバイスにアプリのディープ リンクを提供できる特別なリンクでマークできます。

## <a name="selecting-an-app-search-approach"></a>アプリの検索方法の選択

これらのメソッドの実装の決定は、アプリによって提供される操作の種類とコンテンツの表示の種類によって異なります。

次のガイドラインに従います。

- [**NSUserActivity** ](nsuseractivity.md) : パブリックおよびプライベートのコンテンツとの両方も、アプリ内でナビゲーション ポイントの検索機能の検索機能を提供するこのフレームワークを使用します。

- [**スポット ライトのコア**](corespotlight.md) : デバイスに格納されている秘密データの検索機能を提供するこのフレームワークを使用します。

- [**マークアップを web** ](web-markup.md) – からだけでなく、アプリの web サイトもからでは、アプリ内でコンテンツを表示するアプリの検索機能を提供するこのフレームワークを使用します。

アプリの検索方法が異なるでき、それぞれを個別に使用、しかし Apple が設計に連携して動作します。 特定の項目のインデックスを作成する方法の 1 つ以上を使用している場合は、同じを使用することを確認してください。**項目 ID**それぞれの方法でためその個人とをつなぐ作業します。

複数の方法を使用するだけでなくにより、コンテンツは、エンドユーザーによって検出されますは検索から項目の順位付けを向上するのにも役立ちます。

ランク付けプロセス中にほとんどの場合は開発者に透過的な特定のアイテムでのユーザー操作、評価頻度の高いこのランク (たとえば、ユーザーのリンクをタップ) にします。
豊富な役に立つアイテムを提供すること、ユーザーが高くなります、コンテンツと対話するため、ランクの発生を確認することができます。

## <a name="what-content-to-index"></a>インデックスにどのようなコンテンツ

Apple では、どのようなコンテンツと操作に関して、アプリ内のインデックスの検索を提供する次の提案を提供します。

 - 任意のコンテンツでは、作成、またはから、アプリ内でのユーザーによって curated 表示。
 - ナビゲーション ポイントと、アプリ内で機能します。
 - 新しいメッセージ、コンテンツまたは他の種類のデバイスにダウンロードした最新のアプリで表示できるアイテムをなどです。

## <a name="app-search-enhancements"></a>アプリの検索の機能強化

IOS 10 の主要なメディアは、次のアプリを検索するいくつかの拡張機能を提供します。

- **(差分プライバシー) の Crowdsourced ディープ リンク人気**-検索結果にコンテンツをディープ リンク アプリを昇格する方法を提供します。
- **アプリ内検索**-新しい`CSSearchQuery`メール、Messages や Notes アプリの作業と同様に、アプリ内・ Spotlight 検索機能を提供するクラス。
- **継続タスクを検索**- により、ユーザーはアプリを開き、メディアまたは Safari、検索を開始して、その検索を続行します。
- **検証結果の視覚化**-Apple の[アプリの検索 API 検証ツール](https://search.developer.apple.com/appsearch-validation-tool)preforming テストと web サイトのマークアップとディープ リンクの視覚的表現が表示されます。
- **アプリ画像の共有をメッセージ**-Spotlight 検索で表示するメッセージ (メッセージ アプリ拡張機能) 経由で共有するために提供される一般的なアプリでイメージを使用します。

詳細については、次を参照してください、[アプリ検索の機能強化](~/ios/platform/search/app-search-enhancements.md)ガイドです。

### <a name="proactive-suggestions"></a>プロアクティブなご提案

iOS 10 では、事前対応的に提示する役に立つ情報自動的にユーザーに適切な時点ではシステムによって推進エンゲージメント アプリへの新しい方法を表示します。 同様に iOS 9 には、ios 10 アプリは、次の場所内で、システムにより、ユーザーに表示できる機能を公開できますスポット ライト、ハンドオフおよび Siri の推奨事項を使用してアプリに詳細な検索を追加する機能が用意されています。

- アプリのスイッチャー
- ロック画面
- CarPlay
- マップ
- Siri の相互作用
- QuickType 提案 

アプリがなど、テクノロジのコレクションを使用して、システムには、この機能を公開[NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/)コアのスポット ライト、MapKit、Media Player および UIKit web マークアップ。

詳細については、次を参照してください、[プロアクティブ提案](~/ios/platform/search/proactive-suggestions.md)ガイドです。

## <a name="summary"></a>まとめ

この記事は、新しいカバーされて Xamarin.iOS アプリの検索 API の機能を iOS 9 を提供します。 対応して[NSUserActivity](nsuseractivity.md)、[コアのスポット ライト](corespotlight.md)と[Web マークアップ](web-markup.md)コンテンツのインデックス作成の方法です。 特定の検索方法を使用すべき状況およびコンテンツの種類にする必要がありますの簡単に説明を完了してインデックスを作成します。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://developer.xamarin.com/samples/ios/iOS9/)
- [開発者向けの iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [アプリの検索プログラミング ガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
