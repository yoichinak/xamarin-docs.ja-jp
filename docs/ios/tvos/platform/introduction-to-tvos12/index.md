---
title: TvOS 12 の概要
description: このドキュメントでは、現在 (C#) バインドを提供する Xamarin のプレビュー リリースの tvOS 12 で新規および更新された機能の概要の高度なを提供します。
ms.prod: xamarin
ms.assetid: 037F7FFF-2155-4017-B99A-839CE7EC5C9C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 03841306ba54e511dbf2f2b86a7c17e9f4669bcd
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37067318"
---
# <a name="introduction-to-tvos-12"></a>TvOS 12 の概要

![[プレビュー]](~/media/shared/preview.png)

> [!WARNING]
> Xamarin の tvOS の 12 のサポートは現在プレビュー期間中、つまり、バグが含まれているでない機能完了し、変更することがあります。 実験のみを使用します。

> [!NOTE]
> - 確認、[作業の開始](~/ios/platform/introduction-to-ios12/get-started.md)iOS 12 および Xamarin を使用した tvOS 12 アプリの構築を開始する方法についてガイドします。
> - 詳細については、読み取り、[リリース ノート](https://releases.xamarin.com/preview-release-xcode-10-beta/)Xamarin のプレビュー リリースです。

このドキュメントは、新規および更新された tvOS の大まかな概要にどの Xamarin のプレビューのリリース現在 c# バインディングを提供します 12 の機能を提供します。

## <a name="password-autofill"></a>パスワードのオートコンプリートを使用

TvOS 12、ユーザーは、シングル タップによる tvOS アプリにサインインする自分の iOS デバイスを使用できます。 これは、オプションが有効の組み合わせによって`UITextContentType`フィールドの使用量をユーザー名とパスワードを指定して、iOS アプリと、tvOS アプリとユーザーの後にフォーカスを受け取る項目を選択する優先フォーカス環境間のリレーションシップを確立するために関連付けられているドメインユーザー名とパスワードを提供します。

## <a name="focus-engine-enhancements"></a>フォーカス エンジンの機能強化

tvOS 12 では、どのにレンダリングされる、フォーカス エンジンとの対話に関係なく、すべてのアプリを使用できます。 Siri リモートとの対話をユーザーのを通じてでは、項目を選択して、考えられるフォーカスの変更をほのめかすおよび必然的にフォーカスを更新する、すべてのアプリで、フォーカス エンジンを使用します。 UIKit のカスタム アプリケーションでこれが有効になって`IUIFocusItemContainer`、インターフェイス、`UIFocusMovementHint`クラス、`IUIFocusItemScrollableContainer`インターフェイス、およびその他の関連するクラスとメソッド。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS – Apple 開発者 (Apple)](https://developer.apple.com/tvos/)
- [TvOS 12 (Apple) (ビデオ) の新機能](https://developer.apple.com/videos/play/wwdc2018/208/)
- [テレビ (Apple)](https://www.apple.com/tv/)
- Xamarin プレビュー[リリース ノート](https://releases.xamarin.com/preview-release-xcode-10-beta/)
