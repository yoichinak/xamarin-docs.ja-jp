---
title: フラグメントの実装 - チュートリアル
description: この記事では、フラグメントを使用して Xamarin.Android アプリケーションを開発する手順について説明します。
ms.topic: tutorial
ms.prod: xamarin
ms.assetid: A71E9D87-CB69-10AB-CE51-357A05C76BCD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/26/2018
ms.openlocfilehash: ab680020f62548eed4d1da0e4dbb13434a6d8ce7
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453949"
---
# <a name="implementing-fragments---walkthrough"></a>フラグメントの実装 - チュートリアル

_フラグメントは自己完結型のモジュール コンポーネントであり、さまざまな画面サイズのデバイスを対象とする Android アプリの複雑さに対処するのに役立ちます。この記事では、Xamarin.Android アプリケーションを開発するときにフラグメントを作成して使用する方法について説明します。_

## <a name="overview"></a>概要

このセクションでは、Xamarin.Android アプリケーションでフラグメントを作成して使用する方法について説明します。 このアプリケーションでは、ウィリアム シェイクスピアのいくつかの芝居のタイトルが一覧に表示されます。 ユーザーが芝居のタイトルをタップすると、アプリではその芝居の引用が別のアクティビティに表示されます。

[![建てモードの Android フォンで実行されているアプリ](./images/intro-screenshot-phone-sml.png)](./images/intro-screenshot-phone.png#lightbox)

スマートフォンを横モードに回転すると、アプリの外観が変わります。芝居の一覧と引用の両方が同じアクティビティに表示されます。 芝居を選択すると、同じアクティビティに引用が表示されます。

[![横モードの Android フォンで実行されているアプリ](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

最後に、アプリがタブレットで実行されている場合は、次のようになります。

[![Android タブレットで実行されているアプリ](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)

フラグメントと[代替レイアウト](../../../app-fundamentals/resources-in-android/alternate-resources.md)を使用することにより、このサンプル アプリケーションは、最小限のコード変更で、さまざまなフォーム ファクターや向きに簡単に適応できます。

アプリケーションのデータは、C# の文字列配列としてアプリにハードコーディングされた 2 つの文字列配列に存在します。 各配列は、1 つのフラグメントに対するデータ ソースとして機能します。  1 つの配列にはシェイクスピアの芝居の名前がいくつか保持されており、もう 1 つの配列にはその芝居からの引用が保持されます。 アプリが起動すると、`ListFragment` に芝居の名前が表示されます。 ユーザーが `ListFragment` で芝居をクリックすると、別のアクティビティが開始されて、その引用が表示されます。

アプリのユーザー インターフェイスは 2 つのレイアウトで構成されており、1 つは縦モード、もう 1 つは横モードです。 Android では、実行時に、デバイスの向きに基づいて読み込むレイアウトが決定され、レンダリングするアクティビティにそのレイアウトが提供されます。 ユーザーのクリックに対応してデータを表示するためのすべてのロジックは、フラグメントに含まれます。 アプリ内のアクティビティは、フラグメントをホストするコンテナーとしてのみ存在します。

このチュートリアルは、2 つのガイドに分かれています。 [1 番目のパート](./walkthrough.md)では、アプリケーションの核となる部分に注目します。 (縦モード用に最適化された) 1 つのレイアウト セットが、2 つのフラグメントおよび 2 つのアクティビティと共に作成されます。

1. `MainActivity` &nbsp; これは、アプリのスタートアップ アクティビティです。
1. `TitlesFragment` &nbsp; このフラグメントには、ウィリアム シェイクスピアによって書かれた芝居のタイトルの一覧が表示されます。 `MainActivity` によってホストされます。
1. `PlayQuoteActivity` &nbsp; ユーザーが `TitlesFragment` で芝居を選択すると、`TitlesFragment` によって `PlayQuoteActivity` が開始されます。
1. `PlayQuoteFragment` &nbsp; このフラグメントには、ウィリアム シェイクスピアによる芝居からの引用が表示されます。 `PlayQuoteActivity` によってホストされます。

[このチュートリアルの 2 番目のパート](./walkthrough-landscape.md)では、画面に両方のフラグメントを表示する (横モード用に最適化された) 代替レイアウトの追加について説明します。 また、画面に同時に表示されるフラグメントの数に合わせてアプリで動作を調整できるように、コードにいくつかの小さな変更を行います。

## <a name="related-links"></a>関連リンク

- [FragmentsWalkthrough (サンプル)](/samples/xamarin/monodroid-samples/fragmentswalkthrough)
- [デザイナーの概要](~/android/user-interface/android-designer/index.md)
- [フラグメントの実装](https://developer.android.com/guide/topics/fundamentals/fragments.html)
- [サポート パッケージ](https://developer.android.com/sdk/compatibility-library.html)