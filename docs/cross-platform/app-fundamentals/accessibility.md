---
title: Xamarin アプリのユーザー補助機能
description: このドキュメントでは、ユーザー補助アプリを作成するためのさまざまなヒントを提供します。 たとえば、大きなフォント、ハイコントラスト、自己記述型のインターフェイスなどに関する推奨事項が含まれています。
ms.prod: xamarin
ms.assetid: E587F0CF-7C1D-41F8-B5A8-DA3E738EDA81
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: df042521d4e9852d6e23c2bbdf24484f9068250d
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571260"
---
# <a name="accessibility-in-xamarin-apps"></a>Xamarin アプリのユーザー補助機能

_アプリが最も多くのユーザーによって利用可能であることを確認する_

アクセシビリティとは、大規模な種類、ハイコントラスト、ズームイン、画面の読み取り (音声合成)、ビジュアルまたは haptic のフィードバックキュー、代替の入力方法など、オペレーティングシステムの表示と入力支援機能を適切に動作させるアプリユーザーインターフェイスの設計の概念を指します。

IOS、Android、Windows などのデスクトップおよびモバイルプラットフォームには、開発者が[Google TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback)や[Apple の VoiceOver](https://www.apple.com/accessibility/ios/voiceover/)などのアクセス可能なアプリを構築するのに役立つ組み込み api が用意されています。

## <a name="platform-specific-apis"></a>プラットフォーム固有の Api

このドキュメントのガイドラインを実装するには、各プラットフォームに用意されている Api を使用します。

- [**Android のアクセシビリティ**](~/android/app-fundamentals/accessibility.md)
- [**iOS のアクセシビリティ**](~/ios/app-fundamentals/accessibility.md)
- [**OS X アクセシビリティ**](~/mac/app-fundamentals/accessibility.md)
- [**Xamarin.Forms**](~/xamarin-forms/app-fundamentals/accessibility/index.md)

<a name="checklist"></a>

## <a name="accessibility-checklist"></a>アクセシビリティのチェックリスト

これらのヒントに従って、アプリが可能な限り幅広くアクセスできることを確認します。 詳細については、 [Android アクセシビリティテストチェックリスト](https://developer.android.com/training/accessibility/testing.html)と[Apple のアクセシビリティ](https://www.apple.com/accessibility/)に関するページを参照してください。

### <a name="support-large-fonts-and-high-contrast"></a>大きなフォントとハイコントラストをサポートする

コントロールのサイズをハードコーディングしないでください。代わりに、サイズの大きいフォントサイズに合わせてサイズを変更できるレイアウトを優先します。
ハイコントラストモードでカラースキームをテストして、読み取り可能であることを確認します。

### <a name="make-the-user-interface-self-describing"></a>ユーザーインターフェイスを自己記述型にする

ユーザーインターフェイスのすべての要素に、説明的なテキストと、各プラットフォームの画面を読み取っているスクリーンと互換性のあるヒントをタグ付けします。

### <a name="ensure-that-images-and-icons-have-an-alternate-text-description"></a>イメージとアイコンに代替テキストの説明があることを確認する

アプリケーションのユーザーインターフェイスの一部であるイメージとアイコン (たとえば、ボタンや状態のインジケーターなど) には、アクセス可能な説明をタグ付けする必要があります。

### <a name="design-the-visual-tree-with-accessible-navigation-in-mind"></a>アクセス可能なナビゲーションを考慮してビジュアルツリーをデザインする

代替の入力方法を使用してコントロール間を移動する場合、タッチスクリーンを使用する場合と同じ論理フローに従うように、適切なレイアウトコントロールまたは Api を使用します。

スクリーンリーダーから不要な要素を除外します (たとえば、既にアクセス可能なフィールドの装飾的なイメージまたはラベル)。

### <a name="dont-rely-on-audio-or-color-cues-alone"></a>オーディオまたは色の手掛かりのみに依存しない

進行状況、完了状態、またはその他の状態が単にサウンドまたは色の変化として示される状況を回避します。 明確な視覚的な手掛かりを含めるようにユーザーインターフェイスをデザインするか (または、強化のためだけにサウンドと色を使用)、または特定のユーザー補助インジケーターを追加します。

色を選択するときは、色の障碍があるユーザーを区別しにくいパレットを使用しないようにしてください。

### <a name="captioning-for-video-text-for-audio"></a>ビデオのキャプション、音声用のテキスト

ビデオコンテンツのキャプション、およびオーディオコンテンツ用の読み取り可能なスクリプトを提供します。 また、オーディオまたはビデオコンテンツの速度を調整し、[ボリューム] ボタンと [再生] ボタンと [一時停止] ボタンを簡単に見つけて使用できるようにするコントロールを提供するのにも役立ちます。

### <a name="localize"></a>Localize

アプリケーションが複数の言語をサポートしている場合は、ユーザー補助の説明をローカライズする (およびする必要があります) ことができます。

## <a name="related-links"></a>関連リンク

- [Android のアクセシビリティ](~/android/app-fundamentals/accessibility.md)
- [iOS のアクセシビリティ](~/ios/app-fundamentals/accessibility.md)
- [OS X アクセシビリティ](~/mac/app-fundamentals/accessibility.md)
- [Xamarin.Forms のアクセシビリティ](~/xamarin-forms/app-fundamentals/accessibility/index.md)
