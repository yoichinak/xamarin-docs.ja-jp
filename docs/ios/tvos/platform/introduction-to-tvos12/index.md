---
title: tvOS 12 の概要
description: このドキュメントでは、Xamarin のプレビューリリースで現在バインドが提供さC#れている tvOS 12 の新機能と更新された機能の概要について説明します。
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: 94fea1786497d04602ea6cf06d875206cf69eb3e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030547"
---
# <a name="introduction-to-tvos-12"></a>tvOS 12 の概要

このドキュメントでは、新規および更新された tvOS 12 の概要について説明します。

Xamarin で tvOS 12 アプリのビルドを開始するには、[ファーストステップガイド](~/ios/platform/introduction-to-ios12/get-started.md)を参照してください。

## <a name="tvuikit"></a>TVUIKit

tvOS 12 には TVUIKit が含まれています。これは、tvOS 開発者がポスタービュー、キャプションボタン、カードビュー、monogram ビューなどの一般的な tvOS コントロールを使用できるようにする Api のセットです。 また、tvOS 12 には、長すぎるテキストをスクロールして完全に表示できないようにするプロパティも導入されています。

## <a name="password-autofill"></a>パスワードのオートコンプリート

TvOS 12 では、ユーザーは iOS デバイスを使用して、1回のタップで tvOS アプリにサインインできます。 これは、ユーザー名とパスワードのフィールドを指定するための `UITextContentType` 使用法の組み合わせ、iOS アプリと tvOS アプリの間の関係を確立するために関連付けられているドメイン、ユーザーの後にフォーカスを受け取る項目を選択するための推奨される環境の組み合わせによって有効になります。ユーザー名とパスワードを提供します。

## <a name="focus-engine-enhancements"></a>フォーカスエンジンの機能強化

tvOS 12 では、レンダリング方法に関係なくすべてのアプリがフォーカスエンジンと対話できます。 Siri リモートとのユーザーのやり取りを通じて、フォーカスエンジンを任意のアプリと共に使用して、項目を選択したり、影響を受ける可能性のある変更や、自然な更新に焦点を当てることができます。 これは、カスタムアプリケーションで、UIKit の `IUIFocusItemContainer` インターフェイス、`UIFocusMovementHint` クラス、`IUIFocusItemScrollableContainer` インターフェイス、およびその他の関連するクラスおよびメソッドを使用して有効にします。

## <a name="vision-framework"></a>ビジョンフレームワーク

ビジョンフレームワークには、さまざまな向きで顔を検出できる改善された顔検出機能が含まれています。 また、要求のリビジョンを使用して、特定のビジョンフレームワークアルゴリズムリビジョンを選択できるようになりました。

## <a name="natural-language-framework"></a>自然言語フレームワーク

自然言語フレームワークを使用すると、アプリケーションはさまざまな種類の言語分析を実行できます。 たとえば、音声の一部を識別し、テキストブロックで表される言語を特定するために使用できます。

## <a name="deprecations"></a>廃止

TvOS 12 では、Apple では OpenGL ES が非推奨とされており、[開発者](https://developer.apple.com/tvos/whats-new/)は金属を採用するようにしています。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS – Apple Developer (Apple)](https://developer.apple.com/tvos/)
- [TvOS 12 (Apple) の新機能 (ビデオ)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [TV (Apple)](https://www.apple.com/tv/)
