---
title: フラグメントのチュートリアルを実装します。
description: この記事で Xamarin.Android アプリケーションを開発するフラグメントを使用する方法について説明します。
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: 92c68298d7abd2570efd89e12d7cfb6364e90972
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
ms.locfileid: "33798357"
---
# <a name="implementing-fragments---walkthrough"></a>フラグメントのチュートリアルを実装します。

_フラグメントは、さまざまな画面サイズとデバイスを対象とする Android アプリの複雑さに対処できる自己完結型、モジュール型のコンポーネントです。この記事で作成および Xamarin.Android アプリケーションを開発するときに、フラグメントを使用する方法について説明します。_

## <a name="overview"></a>概要

このセクションで作成および Xamarin.Android アプリケーションでフラグメントを使用する方法を説明します。 このアプリケーションは、いえいえ、いくつかのアプローチのタイトルを一覧に表示されます。 ユーザーは、再生のタイトルでタップ、したときに、アプリに表示されます引用形再生された、別のアクティビティで。

[![Android フォンの縦モードで実行されているアプリ](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

電話がモードを横向きに回転させると、アプリの外観が変わります: 同じアクティビティに両方のアプローチと引用符の一覧が表示されます。 再生がオンの場合、引用符が同じアクティビティに表示になります。

[![Android フォンの横モードで実行されているアプリ](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

最後に、アプリがタブレットで実行されている場合。

[![Android タブレットで実行されているアプリ](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

このサンプル アプリケーションに適合させること、さまざまなフォーム ファクターと最小限のコード変更の向きにフラグメントを使用して、[代替レイアウト](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources)です。

アプリケーションのデータは、c# の文字列の配列としてハードコーディングされたアプリでは、2 つの文字列配列で存在します。 各配列は、1 つのフラグメントのデータ ソースとして使用されます。  1 つの配列は、いくつかのアプローチ シェイクスピアーでの名前を保持する、もう一方の配列から再生された見積もり持ちます。 アプリの起動時にでは、再生名が表示されます、`ListFragment`です。 再生で、ユーザーがクリックしたとき、`ListFragment`見積もりを表示する別のアクティビティをアプリケーションが開始されます。

アプリのユーザー インターフェイスは、2 つのレイアウト用縦と横モード用の 1 つで構成されます。 実行時に、Android を読み込むには、どのようなレイアウトが、デバイスの向きをに基づいて決定されますおよび表示するために、アクティビティにそのレイアウトを提供します。 すべてのユーザーのクリックに応答して、データを表示するためのロジックが含まれているフラグメント。 アプリ内のアクティビティは、フラグメントをホストするためのコンテナーとしてのみ存在します。

このチュートリアルは、2 つのガイドに細分化をされます。 [第 1 部](./walkthrough.md)アプリケーションのコア部分に焦点を当てます。 2 つのフラグメントと 2 つのアクティビティと共に、レイアウト (縦向きモードの最適化) の 1 つのセットが作成されます。

1. `MainActivity` &nbsp; これは、アプリの起動アクティビティです。
1. `TitlesFragment` &nbsp; このフラグメントいえいえ記述されている再生のタイトルの一覧が表示されます。 によってホストされている`MainActivity`です。
1. `PlayQuoteActivity` &nbsp; `TitlesFragment` 起動、`PlayQuoteActivity`で再生を選択すると、ユーザーへの応答`TitlesFragment`です。
1. `PlayQuoteFragment` &nbsp; このフラグメントいえいえ引用形再生が表示されます。 によってホストされている`PlayQuoteActivity`です。

[2 番目のこのチュートリアルのパート](./walkthrough-landscape.md)両方のフラグメントを画面に表示されます (横モード用に最適化) 異なるレイアウトを追加することについて説明します。 また、いくつかのコードの軽微な変更になります、コードに、アプリが画面に同時に表示されているフラグメントの数には、その動作を適応させるようにします。

## <a name="related-links"></a>関連リンク

- [FragmentsWalkthrough (サンプル)](https://developer.xamarin.com/samples/monodroid/FragmentsWalkthrough/)
- [デザイナーの概要](~/android/user-interface/android-designer/index.md)
- [フラグメントを実装します。](http://developer.android.com/guide/topics/fundamentals/fragments.html)
- [サポート パッケージ](http://developer.android.com/sdk/compatibility-library.html)
