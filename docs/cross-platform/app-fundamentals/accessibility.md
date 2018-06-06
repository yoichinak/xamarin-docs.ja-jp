---
title: Xamarin アプリのユーザー補助機能
description: このドキュメントでは、アクセス可能なアプリの作成のさまざまなヒントを提供します。 たとえば、大きなフォント、ハイ コントラスト、自己記述型のインターフェイスに関する推奨事項が含まれています。
ms.prod: xamarin
ms.assetid: E587F0CF-7C1D-41F8-B5A8-DA3E738EDA81
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 97cd3655ac47a017d9590e1890b93d74f10a9c34
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780305"
---
# <a name="accessibility-in-xamarin-apps"></a>Xamarin アプリのユーザー補助機能

_アプリが可能な最大限のユーザーが使用可能なことを確認してください。_

アクセシビリティとは、大きな型、ハイ コントラストは、ズーム インなど、オペレーティング システムの表示および入力アシスタンス機能作業もアプリのユーザー インターフェイスを設計、画面読み取り (音声合成)、visual または haptic フィードバック キューの概念と代替の入力方法です。

など、iOS、Android、および Windows デスクトップおよびモバイル プラットフォームを提供など、アクセス可能なアプリをビルドする開発者を支援する Api に組み込まれている[Google 応答](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback)と[Apple の VoiceOver](http://www.apple.com/accessibility/ios/voiceover/)です。

## <a name="platform-specific-apis"></a>プラットフォーム固有の Api

このドキュメントでのガイドラインを実装するには、各プラットフォームで提供される Api を使用します。

- [**Android のユーザー補助機能**](~/android/app-fundamentals/accessibility.md)
- [**iOS ユーザー補助機能**](~/ios/app-fundamentals/accessibility.md)
- [**OS X ユーザー補助機能**](~/mac/app-fundamentals/accessibility.md)
- [**Xamarin.Forms**](~/xamarin-forms/app-fundamentals/accessibility/index.md)

<a name="checklist" />

## <a name="accessibility-checklist"></a>ユーザー補助のチェックリスト

次の次のヒントをアプリが可能な最も幅の広い対象ユーザーにアクセスできることを確認してください。 チェック アウト、 [Android のユーザー補助機能テスト チェックリスト](http://developer.android.com/training/accessibility/testing.html)と[Apple のユーザー補助に関するページ](http://www.apple.com/accessibility/)の詳細。

### <a name="support-large-fonts-and-high-contrast"></a>大きなフォントおよびハイ コントラストをサポートします。

ハードコーディングするコントロールのサイズを回避し、代わりに、大きなフォント サイズに合わせてサイズを変更するレイアウトを使用します。
読み取り可能なされることを確認するハイ コントラスト モードでは、配色をテストします。

### <a name="make-the-user-interface-self-describing"></a>ように、ユーザー インターフェイス自己記述型

タグの説明のテキストと、各プラットフォームで Api を読み取り、画面と互換性があるヒント ユーザー インターフェイスのすべての要素です。

### <a name="ensure-that-images-and-icons-have-an-alternate-text-description"></a>イメージやアイコンに代替テキストの説明があることを確認してください。

アプリケーションのユーザー インターフェイス (ボタンの例については、状態のインジケーターなど) に含まれているイメージやアイコンは、ユーザー補助の説明をタグ付ける必要があります。

### <a name="design-the-visual-tree-with-accessible-navigation-in-mind"></a>ビジュアル ツリーを念頭アクセス可能なナビゲーションとデザイン

適切なレイアウト コントロールを使用して、または Api 入力、別の方法を使用して、コントロール間を移動するようにはタッチ スクリーンを使用する場合と同じ、論理的な流れに従います。

スクリーン リーダー (装飾用の画像または既にアクセス可能であるなどのフィールドのラベル) から不要な要素を除外します。

### <a name="dont-rely-on-audio-or-color-cues-alone"></a>オーディオまたは色のキューだけに依存しないでください。

進行状況、終了した場合、またはその他の状態の唯一の指標がサウンドや色の変更状況を回避します。 いずれか (サウンドと持つ補強のみの色を)、視覚にチェック ボックスをオフまたは特定のユーザー補助のインジケーターを追加するユーザー インターフェイスを設計します。

色を選択するときに、ハード色覚にユーザーを区別するためには、パレットを避けるために再試行してください。

### <a name="captioning-for-video-text-for-audio"></a>オーディオ、ビデオ、テキストのキャプション

ビデオ コンテンツ、およびオーディオ コンテンツの読み取り可能なスクリプトのキャプションを提供します。 れても、オーディオまたはビデオ コンテンツの速度を調整するコントロールを提供し、そのボリュームを確実に役立ちますし、再生/一時停止ボタンは簡単に見つけて使用します。

### <a name="localize"></a>Localize

ユーザー補助の説明できます (および必要があります) するローカライズされたアプリケーションが複数の言語をサポートしています。



## <a name="related-links"></a>関連リンク

- [Android のユーザー補助機能](~/android/app-fundamentals/accessibility.md)
- [iOS ユーザー補助機能](~/ios/app-fundamentals/accessibility.md)
- [OS X ユーザー補助機能](~/mac/app-fundamentals/accessibility.md)
- [Xamarin.Forms のユーザー補助機能](~/xamarin-forms/app-fundamentals/accessibility/index.md)
