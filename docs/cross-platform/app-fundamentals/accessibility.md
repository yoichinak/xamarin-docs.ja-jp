---
title: Xamarin アプリのユーザー補助機能
description: このドキュメントでは、アクセシビリティ対応アプリの作成のさまざまなヒントを提供します。 たとえば、大きなフォント、ハイ コントラスト、自己記述型のインターフェイス、およびに関する推奨事項が含まれています。
ms.prod: xamarin
ms.assetid: E587F0CF-7C1D-41F8-B5A8-DA3E738EDA81
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 0ec264e0f3d381fdac46c79dd479da2bc768954f
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668336"
---
# <a name="accessibility-in-xamarin-apps"></a>Xamarin アプリのユーザー補助機能

_アプリが多くのユーザーから使用可能なことを確認します_

アクセシビリティとは、画面閲覧 (音声変換)、visual またはハプティクス フィードバックの手掛かりにズーム イン ハイ コントラストなどの大きな型など、オペレーティング システムの表示と入力支援機能作業もアプリのユーザー インターフェイスの設計の概念と代替の入力方法。

など、iOS、Android、および Windows デスクトップおよびモバイル プラットフォームの提供など、アクセス可能なアプリのビルドを支援するための Api で構築された[Google TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback)と[Apple の VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)します。

## <a name="platform-specific-apis"></a>プラットフォーム固有の Api

このドキュメントでのガイドラインを実装するには、各プラットフォームで提供される Api を使用します。

- [**Android のアクセシビリティ**](~/android/app-fundamentals/accessibility.md)
- [**iOS ユーザー補助機能**](~/ios/app-fundamentals/accessibility.md)
- [**OS X のアクセシビリティ**](~/mac/app-fundamentals/accessibility.md)
- [**Xamarin.Forms**](~/xamarin-forms/app-fundamentals/accessibility/index.md)

<a name="checklist" />

## <a name="accessibility-checklist"></a>ユーザー補助のチェックリスト

アプリが可能な最も幅の広い対象ユーザーにアクセス可能であることを確認する次のヒントに従います。 チェック アウト、 [Android のアクセシビリティのテスト チェックリスト](https://developer.android.com/training/accessibility/testing.html)と[Apple のユーザー補助に関するページ](http://www.apple.com/accessibility/)の追加情報。

### <a name="support-large-fonts-and-high-contrast"></a>大きなフォントおよびハイ コントラストをサポートします。

ハードコーディング コントロールのサイズを回避し、代わりに、大きなフォント サイズに合わせてサイズを変更できるレイアウトを使用します。
読み取り可能であることを確認するハイ コントラスト モードでの配色をテストします。

### <a name="make-the-user-interface-self-describing"></a>ユーザー インターフェイス自己記述型

説明のテキストと、画面の各プラットフォームでの Api の読み取りと互換性があるヒントでユーザー インターフェイスのすべての要素のタグを付けます。

### <a name="ensure-that-images-and-icons-have-an-alternate-text-description"></a>イメージとアイコンが代替テキストの説明があることを確認します。

ユーザー補助の説明では、イメージとアプリケーションのユーザー インターフェイス (ボタンや状態などのインジケーター) などの一部であるアイコンをタグ付けする必要があります。

### <a name="design-the-visual-tree-with-accessible-navigation-in-mind"></a>デザインを考慮してアクセスできるナビゲーションを使用したビジュアル ツリー

適切なレイアウト コントロールを使用して、または Api 代替入力方式を使用してコントロール間を移動できるように、タッチ スクリーンを使用する場合と同じ論理フローに依存しています。

(装飾用の画像または既にアクセス可能である、たとえばフィールドのラベル) のスクリーン リーダーから不要な要素を除外します。

### <a name="dont-rely-on-audio-or-color-cues-alone"></a>オーディオまたは色のキューだけに依存しないように

進行状況、終了した場合、またはその他の状態の唯一の指標がサウンドまたは色の変更状況を回避します。 いずれか (サウンドと強化のみの色) を持つ視覚チェック ボックスをオフまたは特定のユーザー補助のインジケーターを追加するユーザー インターフェイスをデザインします。

色を選択する際に、色覚にユーザーを区別するが難しいパレットを回避するためにお試しください。

### <a name="captioning-for-video-text-for-audio"></a>オーディオ、ビデオ、テキストのキャプション

ビデオのコンテンツ、およびオーディオのコンテンツの読み取り可能なスクリプトのキャプションを提供します。 オーディオまたはビデオのコンテンツの速度を調整するコントロールを提供し、そのボリュームを確実に便利ですし、再生/一時停止ボタンは簡単に検索を使用できます。

### <a name="localize"></a>Localize

ユーザー補助の説明できます (および必要があります) は、ローカライズ、アプリケーションが複数の言語をサポートします。



## <a name="related-links"></a>関連リンク

- [Android のアクセシビリティ](~/android/app-fundamentals/accessibility.md)
- [iOS ユーザー補助機能](~/ios/app-fundamentals/accessibility.md)
- [OS X のアクセシビリティ](~/mac/app-fundamentals/accessibility.md)
- [Xamarin.Forms のアクセシビリティ](~/xamarin-forms/app-fundamentals/accessibility/index.md)
