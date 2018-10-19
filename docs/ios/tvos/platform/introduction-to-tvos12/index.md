---
title: TvOS 12 の概要
description: このドキュメントでは、現在 c# バインディングを提供する Xamarin のプレビュー リリースの tvOS 12 で新規および更新された機能の概要についての概要を説明します。
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/05/2018
ms.openlocfilehash: f7fb8cc379a070b848c5154c9c1d4fbfc8186266
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2018
ms.locfileid: "39615204"
---
# <a name="introduction-to-tvos-12"></a>TvOS 12 の概要

このドキュメントでは、新規および更新された tvOS 12 の大まかな概要を説明します。

Xamarin で tvOS 12 のアプリの構築を開始するを参照してください、[ファースト ステップ ガイド](~/ios/platform/introduction-to-ios12/get-started.md)します。

## <a name="tvuikit"></a>TVUIKit

tvOS 12 には、TVUIKit、tvOS 開発者ポスター ビュー、キャプション ボタン、カード ビュー、およびモノグラム ビューなどの一般的な tvOS コントロールを使用することができるようにする Api のセットが含まれています。 tvOS 12 には、長すぎるため、完全に表示されるテキストをスクロールするラベルのプロパティも導入されています。

## <a name="password-autofill"></a>パスワードのオートコンプリート

TvOS 12、ユーザーは 1 回のタップで tvOS アプリにサインインを iOS デバイスを使用できます。 組み合わせによってこれが有効になって`UITextContentType`ユーザー名とパスワードを指定する使用法フィールド、iOS アプリと tvOS アプリでは、ユーザーの後にフォーカスを受け取る項目を選択する優先フォーカス環境との関係を確立するために関連付けられているドメインユーザー名とパスワードを提供します。

## <a name="focus-engine-enhancements"></a>フォーカス エンジンの機能強化

tvOS 12 方法にレンダリングされる、フォーカス エンジンとの対話に関係なく、すべてのアプリを許可します。 Siri のリモート ユーザーの対話を通じて 項目を選択して、示唆している可能性のフォーカスの変更、および自然フォーカスを更新、フォーカス エンジンすべてのアプリで使用できます。 UIKit のカスタム アプリケーションでこれを有効に`IUIFocusItemContainer`、インターフェイス、`UIFocusMovementHint`クラス、`IUIFocusItemScrollableContainer`インターフェイス、およびその他の関連するクラスとメソッド。

## <a name="vision-framework"></a>ビジョン フレームワーク

ビジョン フレームワークには、さまざまな方向に顔を検出できる強化された顔検出機能が含まれています。 また、要求のリビジョンはビジョン framework アルゴリズムの特定のリビジョンを選択するようになりました使用できます。

## <a name="natural-language-framework"></a>自然言語フレームワーク

自然言語、フレームワークは、アプリケーションをさまざまな種類の言語分析を実行できます。 たとえば、品詞を特定し、テキストのブロックで表される言語を決定する、使用できます。

## <a name="deprecations"></a>廃止された機能

TvOS 12 で Apple の OpenGL ES、使用は推奨[開発者](https://developer.apple.com/tvos/whats-new/)メタルを採用します。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – Apple Developer (Apple)](https://developer.apple.com/tvos/)
- [新機能については tvOS 12 (Apple) (ビデオ)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [テレビ (Apple)](https://www.apple.com/tv/)