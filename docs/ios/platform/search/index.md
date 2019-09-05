---
title: Xamarin での Api の検索
description: この記事では、iOS 9 に用意されている新しいアプリ検索 Api を使用して、ユーザーが Xamarin. iOS アプリ内の情報と機能を検索できるようにする方法について説明します。
ms.prod: xamarin
ms.assetid: 7323EB3D-A78F-4BF0-9990-3160C7E83CF0
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/20/2017
ms.openlocfilehash: eb88f7c1de12eee59ea4c2a271079e6b96c29b09
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70286601"
---
# <a name="search-apis-in-xamarinios"></a>Xamarin での Api の検索

_この記事では、iOS 9 に用意されているアプリ検索 Api を使用して、ユーザーが Xamarin. iOS アプリ内の情報と機能を検索できるようにする方法について説明します。_

IOS 9 では Search が拡張されており、Xamarin iOS アプリ内の情報や機能にアクセスするための新しい方法を提供しています。 新しいアプリ検索 Api を使用すると、アプリのコンテンツはスポットライトと Safari の検索結果、ハンドオフおよび Siri のリマインダーと提案によって検索可能になります。 これにより、ユーザーはアプリ内のアクティビティや情報にすばやくアクセスできます。

また、新しい検索 Api を使用すると、以前に検索を実装することなく、アプリ内の検索を簡単に統合できます。 このため、Apple は、アプリ検索を使用して、iOS 9 アプリのコンテンツを汎用的に検索できるように、通常は数時間かかることを要求しています。

[![](images/intro01.png "アプリ検索を使用して、汎用的に検索可能な iOS 9 アプリコンテンツの例")](images/intro01.png#lightbox)

アプリ検索は、3つの個別の Api で構成されています。

1. [**Nsuseractivity**](nsuseractivity.md) -Apple が iOS 8 でリリースしたハンドオフ API の拡張機能です。 これは、ユーザーがアプリの相互作用の履歴をパブリックとプライベートの両方で検索できるようにするために使用されます。

2. [**コアスポットライト**](corespotlight.md)-アプリが検索結果に表示されるコンテンツのインデックスを作成できるようにします。 これは、項目を追加および削除できるデータベース API と同様に動作し、アプリ内でプライベートコンテンツにインデックスを作成するための最適な方法です。

3. [**Webmarkup**](web-markup.md) -web インターフェイス経由でコンテンツへのアクセスを提供するアプリ (アプリ内からだけでなく)。 Web コンテンツは、Apple によってクロールされる特別なリンクを使用してマークすることができ、ユーザーの iOS 9 デバイス上のアプリへのディープリンクを提供します。

## <a name="selecting-an-app-search-approach"></a>アプリの検索方法の選択

実装する方法を決定する方法は、アプリによって提供される相互作用の種類と、アプリが提示するコンテンツの種類によって異なります。

次のガイドラインに従ってください。

- [**Nsuseractivity**](nsuseractivity.md) –このフレームワークを使用して、パブリックコンテンツとプライベートコンテンツの両方に対して検索機能を提供し、アプリ内のナビゲーションポイントを検索できます。

- [**コアスポットライト**](corespotlight.md)–このフレームワークを使用して、デバイスに格納されているプライベートデータの検索を提供します。

- [**Web マークアップ**](web-markup.md)–このフレームワークを使用すると、アプリ内だけでなく、アプリの web サイトからもコンテンツを提供するアプリを検索できます。

アプリの検索方法はそれぞれ異なり、個別に使用することができますが、Apple では連携するように設計されています。 複数の方法を使用して特定の項目のインデックスを作成する場合は、個々のリンクが連携するように、各アプローチで同じ**項目 ID**を使用するようにしてください。

複数の方法を使用すると、エンドユーザーがコンテンツを見つけられるだけでなく、検索内から項目のランキングを向上させることができます。

ほとんどの場合、の順位付けプロセスは開発者にとって透過的ですが、特定の項目とのユーザー操作はこの順位に対して頻繁に格付けされます (たとえば、ユーザーがリンクをタップ)。
豊富な情報を提供することにより、ユーザーがコンテンツと対話し、順位付けを enticed ことができるようになります。

## <a name="what-content-to-index"></a>インデックスを作成するコンテンツ

Apple では、アプリで検索インデックスを提供するコンテンツとアクションについて、次の提案を提供しています。

- アプリ内でユーザーによって表示、作成、または curated されたすべてのコンテンツ。
- アプリ内のナビゲーションポイントと機能。
- 最近、デバイスにダウンロードされた新しいメッセージ、コンテンツ、またはアプリによって表示されるその他の種類の項目などです。

## <a name="app-search-enhancements"></a>アプリ検索の機能強化

IOS 10 のコアスポットライトは、次のようなアプリ検索に対していくつかの機能強化を提供します。

- **引き出しディープリンクの人気度 (差分プライバシー)** -検索結果でディープリンクアプリのコンテンツを昇格する方法を提供します。
- **アプリ内検索**-新しい`CSSearchQuery`クラスを使用して、メール、メッセージ、ノートアプリの動作と同様に、アプリ内スポットライト検索機能を提供します。
- **[検索の継続]** -ユーザーがスポットライトまたは Safari で検索を開始し、アプリを開いて検索を続行できるようにします。
- **検証結果の視覚化**-Apple の[App Search API 検証ツール](https://search.developer.apple.com/appsearch-validation-tool)では、テストを事前に形成するときに、web サイトのマークアップとディープリンクが視覚的に表示されるようになりました。
- **メッセージアプリイメージの共有**-メッセージ (メッセージアプリ拡張機能を使用) での共有用に提供された、人気のあるアプリ内イメージがスポットライト検索に表示されます。

詳細については、[アプリ検索の拡張機能](~/ios/platform/search/app-search-enhancements.md)に関するガイドを参照してください。

### <a name="proactive-suggestions"></a>プロアクティブな候補

iOS 10 は、システムが適切なタイミングで有益な情報をユーザーに事前に自動的に提示できるようにすることで、アプリに対するエンゲージメントを促進する新しい方法を提供します。 Ios 9 では、スポットライト、ハンドオフ、Siri の提案を使用してアプリにディープ検索を追加できるようになったのと同じように、iOS 10 では、アプリは次の場所からシステムによってユーザーに提示される機能を公開することができます。

- アプリスイッチャー
- ロック画面
- CarPlay
- マップ
- Siri の相互作用
- QuickType 候補 

アプリは、 [Nsuseractivity](xref:Foundation.NSUserActivity)、web マークアップ、コアスポットライト、mapkit、Media Player、uikit などのテクノロジのコレクションを使用して、この機能をシステムに公開します。

詳細については、「[プロアクティブな提案](~/ios/platform/search/proactive-suggestions.md)ガイド」を参照してください。

## <a name="summary"></a>Summary

この記事では、iOS 9 が Xamarin iOS アプリ用に提供する新しい Search API の機能について説明しました。 ここでは、コンテンツのインデックスを作成するための[Nsuseractivity](nsuseractivity.md)、[コアスポットライト](corespotlight.md)、 [Web マークアップ](web-markup.md)メソッドについて説明しています。 ここでは、特定の検索方法を使用するタイミングと、インデックスを作成する必要があるコンテンツの種類について簡単に説明しました。



## <a name="related-links"></a>関連リンク

- [iOS 9 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [iOS 9 (開発者向け)](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [アプリ検索のプログラミングガイド](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
