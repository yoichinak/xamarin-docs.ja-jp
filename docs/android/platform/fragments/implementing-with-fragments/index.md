---
title: フラグメントのチュートリアルの実装
description: この記事のフラグメントを使用して Xamarin.Android アプリケーションを開発する方法について説明します。
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: b068169ee3f44932f8ee13d2546804f7b2d2a645
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103063"
---
# <a name="implementing-fragments---walkthrough"></a>フラグメントのチュートリアルの実装

_フラグメントは、さまざまな画面サイズを持つデバイスを対象とする Android アプリの複雑さに対処できる自己完結型のモジュラー コンポーネントです。この記事で作成して Xamarin.Android アプリケーションを開発する際に、フラグメントを使用する方法について説明します。_

## <a name="overview"></a>概要

このセクションで作成して、Xamarin.Android アプリケーションでフラグメントを使用する方法を説明します。 このアプリケーションは、ウィリアム シェイクスピアーによって、いくつかのアプローチのタイトルを一覧に表示されます。 ユーザーが、プレイのタイトルをタップするし、アプリは、別のアクティビティに再生されたからの引用が表示されます。

[![Android フォンで縦モードで実行されているアプリ](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

電話を回転すると、横長表示モードと、アプリの外観が変更されます: 同じアクティビティに両方のアプローチと引用符の一覧が表示されます。 再生がオンの場合、引用符が同じアクティビティ内の表示になります。

[![Android フォンで横モードで実行されているアプリ](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

最後に、アプリがタブレットで実行されている場合。

[![Android タブレットで実行されているアプリ](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

このサンプル アプリケーションをフラグメントを使用して、さまざまなフォーム ファクターと最小限のコード変更と向きを調整できます簡単にし、[代替レイアウト](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources)します。

アプリケーションのデータとしてアプリにハードコーディングされている 2 つの文字列配列内に存在C#文字列の配列。 各配列は、1 つのフラグメント データ ソースとして使用されます。  1 つの配列は、いくつかのアプローチでシェイクスピアの名前を保持して、その他の配列が再生されたからの引用を保持します。 アプリの起動時と、では、play 名が表示されます、`ListFragment`します。 再生で、ユーザーがクリックしたとき、`ListFragment`アプリは、見積もりを表示する別のアクティビティが開始されます。

アプリのユーザー インターフェイスは、2 つのレイアウト、縦と横モード用に 1 つで構成されます。 実行時に、Android を読み込むには、どのようなレイアウトが、デバイスの向きに基づいて決定され、表示するためにアクティビティをそのレイアウトを提供します。 すべてのユーザーのクリックに応答して、データを表示するためのロジックは、フラグメントに含まれます。 アプリ内のアクティビティは、フラグメントをホストするコンテナーとしてのみ存在します。

このチュートリアルは、2 つのガイドに分割されます。 [第 1 部](./walkthrough.md)アプリケーションのコア部分に注目されます。 2 つのフラグメントと 2 つのアクティビティと共に、レイアウト (縦向きモード用に最適化) の 1 つのセットが作成されます。

1. `MainActivity` &nbsp; これは、アプリのスタートアップ アクティビティです。
1. `TitlesFragment` &nbsp; このフラグメント ウィリアム シェイクスピアーによって書き込まれたアプローチのタイトルの一覧が表示されます。 によってホストされる`MainActivity`します。
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` 開始、`PlayQuoteActivity`で再生を選択すると、ユーザーへの応答で`TitlesFragment`します。
1. `PlayQuoteFragment` &nbsp; このフラグメントいえいえ、play から見積もりが表示されます。 によってホストされる`PlayQuoteActivity`します。

[このチュートリアルのパート 2 つ目の](./walkthrough-landscape.md)は両方のフラグメントが画面に表示されます (横モード用に最適化) 異なるレイアウトを追加することについて説明します。 また、いくつかのコードの軽微な変更になりますコードに、アプリは、画面に同時に表示されるフラグメントの数には、その動作を調整できるようにします。

## <a name="related-links"></a>関連リンク

- [FragmentsWalkthrough (サンプル)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [デザイナーの概要](~/android/user-interface/android-designer/index.md)
- [フラグメントの実装](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [サポート パッケージ](http://developer.android.com/sdk/compatibility-library.html)
