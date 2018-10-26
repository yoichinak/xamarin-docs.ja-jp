---
title: Xamarin.Forms のアクセシビリティ
description: アプリケーションのニーズとエクスペリエンスの範囲で、ユーザー インターフェイスのアプローチの方で使用できるようにユーザー補助対応のアプリケーションを構築します。
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2018
ms.openlocfilehash: ac0ffbdce6b0c55e8ad9d774d80e3d9b8bf84089
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116447"
---
# <a name="xamarinforms-accessibility"></a>Xamarin.Forms のアクセシビリティ

_アプリケーションのニーズとエクスペリエンスの範囲で、ユーザー インターフェイスのアプローチの方で使用できるようにユーザー補助対応のアプリケーションを構築します。_

Xamarin.Forms アプリケーションにアクセスできるようにには、レイアウトと多くのユーザー インターフェイス要素のデザインについて考えることを意味します。 考慮すべき問題のガイドラインについては、次を参照してください。、[アクセシビリティ チェックリスト](~/cross-platform/app-fundamentals/accessibility.md)します。 大きなフォント サイズ、および適切な色とコントラストの設定などの多くのアクセシビリティに関する問題は、Xamarin.Forms の Api を既に対処できます。

[Android のアクセシビリティ](~/android/app-fundamentals/accessibility.md)と[iOS アクセシビリティ](~/ios/app-fundamentals/accessibility.md)ガイドには、Xamarin によって公開されるネイティブ Api の詳細が含まれて、 [MSDN の UWP アクセシビリティ ガイド](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information)について説明します、プラットフォームでネイティブなアプローチです。 これらの Api は、各プラットフォームで完全にアクセス可能なアプリケーションを実装するために使用されます。

Xamarin.Forms が現在持っていない*組み込み*のすべてのユーザー補助の基になるプラットフォームで利用可能な Api をサポートします。 ただしはサポート オートメーションのプロパティの設定画面リーダーおよびナビゲーションの支援ツールをサポートするためにユーザー インターフェイス要素にアクセスできるアプリケーションの構築の最も重要な部分の 1 つです。 詳細については、次を参照してください。[のオートメーション プロパティ](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md)します。

Xamarin.Forms アプリケーションでは、指定されたコントロールのタブ オーダーはこともできます。 詳細については、次を参照してください。[キーボード ナビゲーション](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md)します。

その他のユーザー補助 Api (など[ios PostNotification](~/ios/app-fundamentals/accessibility.md)) するほうが適している可能性があります、 [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md)または[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)実装します。 これらは、このガイドでは説明しません。

## <a name="testing-accessibility"></a>ユーザー補助のテスト

Xamarin.Forms アプリケーションには、プラットフォームに応じて、ユーザー補助機能をテストすることを意味する複数のプラットフォームでは、通常ターゲットします。 各プラットフォームでのアクセシビリティをテストする方法については、これらのリンクに従います。

- [**iOS テスト**](~/ios/app-fundamentals/accessibility.md)
- [**Android のテスト**](~/android/app-fundamentals/accessibility.md)
- [**Windows AccScope (MSDN)**](https://msdn.microsoft.com/library/windows/desktop/dn433239)

## <a name="related-links"></a>関連リンク

- [クロス プラットフォームのユーザー補助機能](~/cross-platform/app-fundamentals/accessibility.md)
- [オートメーションのプロパティ](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md)
- [キーボードのアクセシビリティ](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md)
