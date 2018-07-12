---
title: TvOS 12 の概要
description: このドキュメントでは、現在 c# バインディングを提供する Xamarin のプレビュー リリースの tvOS 12 で新規および更新された機能の概要についての概要を説明します。
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 03841306ba54e511dbf2f2b86a7c17e9f4669bcd
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38847562"
---
# <a name="introduction-to-tvos-12"></a>TvOS 12 の概要

![[プレビュー]](~/media/shared/preview.png)

> [!WARNING]
> Xamarin の tvOS 12 のサポートは現在プレビュー段階で、バグが含まれていることを意味でない機能は完全なし、変わる可能性があります。 実験目的でのみ使用します。

> [!NOTE]
> - レビュー、[概要](~/ios/platform/introduction-to-ios12/get-started.md)iOS 12 と Xamarin で tvOS 12 のアプリの構築を開始する方法についてガイドします。
> - 詳細については、読み取り、[リリース ノート](https://releases.xamarin.com/preview-release-xcode-10-beta/)Xamarin のプレビュー リリース。

このドキュメントは、新規および更新された tvOS の概要を説明する Xamarin のプレビュー リリース現在では c# バインディング 12 の機能を示します。

## <a name="password-autofill"></a>パスワードのオートコンプリート

TvOS 12、ユーザーは 1 回のタップで tvOS アプリにサインインを iOS デバイスを使用できます。 組み合わせによってこれが有効になって`UITextContentType`ユーザー名とパスワードを指定する使用法フィールド、iOS アプリと tvOS アプリでは、ユーザーの後にフォーカスを受け取る項目を選択する優先フォーカス環境との関係を確立するために関連付けられているドメインユーザー名とパスワードを提供します。

## <a name="focus-engine-enhancements"></a>フォーカス エンジンの機能強化

tvOS 12 方法にレンダリングされる、フォーカス エンジンとの対話に関係なく、すべてのアプリを許可します。 Siri のリモート ユーザーの対話を通じて 項目を選択して、示唆している可能性のフォーカスの変更、および自然フォーカスを更新、フォーカス エンジンすべてのアプリで使用できます。 UIKit のカスタム アプリケーションでこれを有効に`IUIFocusItemContainer`、インターフェイス、`UIFocusMovementHint`クラス、`IUIFocusItemScrollableContainer`インターフェイス、およびその他の関連するクラスとメソッド。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – Apple Developer (Apple)](https://developer.apple.com/tvos/)
- [新機能については tvOS 12 (Apple) (ビデオ)](https://developer.apple.com/videos/play/wwdc2018/208/)
- [テレビ (Apple)](https://www.apple.com/tv/)
- Xamarin プレビュー[リリース ノート](https://releases.xamarin.com/preview-release-xcode-10-beta/)
