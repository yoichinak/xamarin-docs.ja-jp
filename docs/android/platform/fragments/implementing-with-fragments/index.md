---
title: フラグメントの実装-チュートリアル
description: この記事では、フラグメントを使用して Xamarin Android アプリケーションを開発する方法について説明します。
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: e5a09c216f0def71efb1c3ddc0ed18672663bdfe
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68643618"
---
# <a name="implementing-fragments---walkthrough"></a>フラグメントの実装-チュートリアル

_フラグメントは、さまざまな画面サイズでデバイスを対象とする Android アプリの複雑さに対処するのに役立つ、自己完結型のモジュールコンポーネントです。この記事では、Xamarin Android アプリケーションの開発時にフラグメントを作成して使用する方法について説明します。_

## <a name="overview"></a>概要

このセクションでは、Xamarin Android アプリケーションでフラグメントを作成して使用する方法について説明します。 このアプリケーションでは、ウィリアムシェイクスピアーによっていくつかの再生のタイトルが一覧に表示されます。 ユーザーが再生のタイトルをタップすると、アプリは別のアクティビティでその再生の引用符を表示します。

[![縦モードの Android フォンで実行されているアプリ](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

電話が横モードに切り替えられると、アプリの外観が変わります。再生と引用符の両方が同じアクティビティに表示されます。 再生を選択すると、同じアクティビティに引用符が表示されます。

[![ランドスケープモードの Android フォンで実行されているアプリ](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

最後に、アプリがタブレットで実行されている場合は、次のようになります。

[![Android タブレットで実行されているアプリ](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

このサンプルアプリケーションは、フラグメントと[代替レイアウト](/xamarin/android/app-fundamentals/resources-in-android/alternate-resources)を使用することによって最小限のコード変更で、さまざまなフォームファクターや方向に簡単に適応できます。

アプリケーションのデータは、文字列配列としてC#アプリにハードコードされた2つの文字列配列に存在します。 各配列は、1つのフラグメントのデータソースとして機能します。  1つの配列はシェイクスピアーによっていくつかの再生の名前を保持し、もう一方の配列はその再生の引用符を保持します。 アプリが起動すると、再生名`ListFragment`がに表示されます。 ユーザーがの`ListFragment`再生をクリックすると、アプリによって別のアクティビティが開始され、そのコメントが表示されます。

アプリのユーザーインターフェイスは2つのレイアウトで構成されます。1つは縦向き、もう1つは横モードです。 Android では、実行時に、デバイスの向きに基づいて読み込むレイアウトが決定され、レンダリングするアクティビティにそのレイアウトが提供されます。 ユーザーがクリックしてデータを表示するためのすべてのロジックは、フラグメントに含まれます。 アプリ内のアクティビティは、フラグメントをホストするコンテナーとしてのみ存在します。

このチュートリアルは、2つのガイドに分かれています。 [最初の部分](./walkthrough.md)では、アプリケーションのコア部分に焦点を当てます。 (縦モード用に最適化された) 1 つのレイアウトセットが、2つのフラグメントと2つのアクティビティと共に作成されます。

1. `MainActivity`&nbsp;これは、アプリのスタートアップアクティビティです。
1. `TitlesFragment`&nbsp;このフラグメントにより、ウィリアムシェイクスピアーによって書き込まれた再生タイトルの一覧が表示されます。 これはによって`MainActivity`ホストされます。
1. `PlayQuoteActivity`は、ユーザー `PlayQuoteActivity`が再生を選択すると`TitlesFragment`応答してを開始します。 &nbsp; `TitlesFragment`
1. `PlayQuoteFragment`&nbsp;このフラグメントでは、ウィリアムシェイクスピアーによる play の引用符が表示されます。 これはによって`PlayQuoteActivity`ホストされます。

[このチュートリアルの2番目のパート](./walkthrough-landscape.md)では、画面に両方のフラグメントを表示する代替レイアウト (横モード用に最適化) の追加について説明します。 また、コードに多少の小さなコード変更が加えられ、アプリが画面に同時に表示されるフラグメントの数に合わせて動作が調整されるようになっています。

## <a name="related-links"></a>関連リンク

- [FragmentsWalkthrough (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fragmentswalkthrough)
- [デザイナーの概要](~/android/user-interface/android-designer/index.md)
- [実装 (フラグメントを)](https://developer.android.com/guide/topics/fundamentals/fragments.html)
- [サポートパッケージ](https://developer.android.com/sdk/compatibility-library.html)
