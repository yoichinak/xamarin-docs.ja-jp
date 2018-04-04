---
title: ユーザー補助
description: ユーザー補助対応アプリケーションを構築すると、アプリケーションがさまざまなニーズを経験を持つユーザー インターフェイスのアプローチのユーザーによって使用可能なことです。
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: e4fb151b9664df7236d2c22ed54db09bf7bc65b8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="accessibility"></a>ユーザー補助

_ユーザー補助対応アプリケーションを構築すると、アプリケーションがさまざまなニーズを経験を持つユーザー インターフェイスのアプローチのユーザーによって使用可能なことです。_

Xamarin.Forms アプリケーションにアクセスできるように、レイアウトと多くのユーザー インターフェイス要素の設計について考えることを意味します。 考慮すべき問題についてのガイドラインについては、次を参照してください。、[アクセシビリティ チェックリスト](~/cross-platform/app-fundamentals/accessibility.md)です。 大きなフォント サイズ、および適切な色とコントラストの設定などの多くのアクセシビリティに関する問題は、Xamarin.Forms Api によって既にアドレス指定できます。

[Android のユーザー補助](~/android/app-fundamentals/accessibility.md)と[iOS アクセシビリティ](~/ios/app-fundamentals/accessibility.md)ガイドには、Xamarin、によって公開されるネイティブ Api の詳細が含まれますと[MSDN UWP アクセシビリティ ガイド](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information)について説明します、そのプラットフォームのネイティブ アプローチです。 これらの Api は、各プラットフォームで使用可能なアプリケーションを完全に実装に使用されます。

Xamarin.Forms は現在ありません*組み込み*すべてのユーザー補助の基になるプラットフォームで利用可能な Api をサポートします。 ただしはサポート ユーザー補助の設定値画面リーダーおよびナビゲーションのサポート ツールをサポートするためにユーザー インターフェイス要素にアクセスできるアプリケーションの構築の最も重要な部分の 1 つです。 詳細については、次を参照してください。[ユーザー インターフェイス要素のユーザー補助機能値の設定](~/xamarin-forms/app-fundamentals/accessibility/setting-accessibility-values.md)です。

その他のユーザー補助 Api (など[iOS で PostNotification](~/ios/app-fundamentals/accessibility.md)) に適している可能性があります、 [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md)または[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)実装します。 これらはこのガイドでは説明しません。

## <a name="testing-accessibility"></a>ユーザー補助のテスト

Xamarin.Forms アプリケーション通常対象に複数のプラットフォームでは、プラットフォームに応じて、ユーザー補助機能をテストすることを意味します。 各プラットフォームでは、ユーザー補助機能をテストする方法については、これらのリンクに従います。

- [**iOS テスト**](~/ios/app-fundamentals/accessibility.md)
- [**Android のテスト**](~/android/app-fundamentals/accessibility.md)
- [**Windows AccScope (MSDN)**](https://msdn.microsoft.com/library/windows/desktop/dn433239)


## <a name="related-links"></a>関連リンク

- [クロスプラット フォームのユーザー補助機能](~/cross-platform/app-fundamentals/accessibility.md)
- [ユーザー インターフェイス要素のアクセシビリティ値の設定](~/xamarin-forms/app-fundamentals/accessibility/setting-accessibility-values.md)
