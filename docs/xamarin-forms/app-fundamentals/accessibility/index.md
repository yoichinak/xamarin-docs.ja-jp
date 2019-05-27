---
title: Xamarin.Forms のアクセシビリティ
description: アクセシビリティを備えたアプリケーションを構築することで、一定範囲のニーズとエクスペリエンスでユーザー インターフェイスを使用するユーザーがアプリケーションを使用できるようになります。
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2018
ms.openlocfilehash: 20ea72e588e2e3b1d575bd593446bf6724d73d8c
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971064"
---
# <a name="xamarinforms-accessibility"></a>Xamarin.Forms のアクセシビリティ

_アクセシビリティを備えたアプリケーションを構築することで、一定範囲のニーズとエクスペリエンスでユーザー インターフェイスを使用するユーザーがアプリケーションを使用できるようになります。_

Xamarin.Forms アプリケーションにアクセシビリティを備えるということは、多くのユーザー インターフェイス要素のレイアウトとデザインについて考えることを意味します。 考慮すべき問題に関するガイドラインについては、[アクセシビリティのチェックリスト](~/cross-platform/app-fundamentals/accessibility.md)をご覧ください。 大きなフォント サイズや、適切な色とコントラストの設定など、アクセシビリティに関する問題の多くは、Xamarin.Forms の API で既に対処されている場合があります。

[Android のアクセシビリティ](~/android/app-fundamentals/accessibility.md)と [iOS のアクセシビリティ](~/ios/app-fundamentals/accessibility.md)に関するガイドには、Xamarin によって公開されているネイティブ API の詳細が含まれています。また、[MSDN 上の UWP アクセシビリティ ガイド](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information)では、そのプラットフォームでのネイティブなアプローチについて説明しています。 これらの API は、各プラットフォーム上にアクセシビリティを備えたアプリケーションを完全に実装するために使われます。

現在、Xamarin.Forms には、基になる各プラットフォーム上で使用できるアクセシビリティ API のすべてに対する "*組み込み*" のサポートが含まれているわけではありません。 ただし、ユーザー インターフェイス要素上でのオートメーション プロパティの設定がサポートされ、スクリーン リーダーとナビゲーション支援ツールがサポートされます。これは、アクセシビリティを備えたアプリケーション構築における最も重要な部分の 1 つです。 詳細については、[オートメーション プロパティ](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md)に関するページをご覧ください。

Xamarin.Forms アプリケーションでは、指定したコントロールのタブ オーダーを指定して、ユーザビリティとアクセシビリティを改善することもできます。 詳細については、[キーボード アクセシビリティ](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md)に関するページをご覧ください。

その他のアクセシビリティ API ([iOS での PostNotification](~/ios/app-fundamentals/accessibility.md) など) については、[`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md) または [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)の実装の方が適している可能性があります。 これらについては、このガイドでは説明しません。

## <a name="testing-accessibility"></a>アクセシビリティのテスト

通常、Xamarin.Forms アプリケーションは複数のプラットフォームを対象とします。これは、プラットフォームに応じてアクセシビリティ機能をテストすることを意味しています。 次のリンクに従って、各プラットフォーム上でアクセシビリティをテストする方法について学習します。

- [**iOS のテスト**](~/ios/app-fundamentals/accessibility.md)
- [**Android のテスト**](~/android/app-fundamentals/accessibility.md)
- [**Windows AccScope (MSDN)** ](https://msdn.microsoft.com/library/windows/desktop/dn433239)

## <a name="related-links"></a>関連リンク

- [クロスプラットフォームのアクセシビリティ](~/cross-platform/app-fundamentals/accessibility.md)
- [オートメーションのプロパティ](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md)
- [キーボード アクセシビリティ](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md)
