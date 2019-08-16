---
title: ツール バー
description: ツールバーは、既定の操作バーよりも柔軟な操作バーコンポーネントです。アプリ内の任意の場所に配置でき、そのサイズを変更できます。また、アプリのテーマとは異なる配色を使用できます。 また、各アプリの画面には複数のツールバーがあります。
ms.prod: xamarin
ms.assetid: 22EE5FBD-3240-4308-AF76-EF45D72936DE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 065c68ce6fbe816e503ace0ab42b3e86aa6916a7
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522840"
---
# <a name="toolbar"></a>ツール バー

_ツールバーは、既定の操作バーよりも柔軟な操作バーコンポーネントです。アプリ内の任意の場所に配置でき、そのサイズを変更できます。また、アプリのテーマとは異なる配色を使用できます。また、各アプリの画面には複数のツールバーがあります。_

 
## <a name="overview"></a>概要

Android アクティビティの主要なデザイン要素は、*アクションバー*です。 アクションバーは、Android アプリのナビゲーション、検索、メニュー、およびブランド化に使用される UI コンポーネントです。 Android 5.0 をロリポップにする前の Android バージョンでは、操作バー (*アプリバー*とも呼ばれます) が、この機能を提供するために推奨されるコンポーネントでした。 

( `Toolbar` Android 5.0 ロリポップで導入された) ウィジェットは、アクションバーインターフェイス&ndash;の一般化として考えることができます。これは、操作バーを置き換えることを目的としています。 は`Toolbar` 、アプリレイアウト内の任意の場所で使用でき、操作バーよりもはるかにカスタマイズできます。 次のスクリーンショットは、 `Toolbar`このガイドで作成したカスタマイズされた例を示しています。 

[![編集、保存、およびオーバーフローメニュー項目があるツールバーの例のスクリーンショット](images/01-toolbar-sml.png)](images/01-toolbar.png#lightbox)

`Toolbar`とアクションバーには、いくつかの重要な違いがあります。 

- は`Toolbar` 、ユーザーインターフェイス内の任意の場所に配置できます。

- 複数のツールバーを同じ画面に表示できます。

- フラグメントが使用されている場合、各フラグメント`Toolbar`は独自のを持つことができます。 

- は`Toolbar` 、画面の一部の幅にのみまたがるように構成できます。 

- は、 `Toolbar`アクティビティのウィンドウ décor の配色にバインドされていないため、視覚的に区別される配色を持つことができます。 

- 操作バーとは異なり、 `Toolbar`には左側にアイコンが含まれていません。 右側のメニューの使用領域が少なくなります。 

- `Toolbar`高さは調整可能です。 

- その他のビューは、内`Toolbar`に含めることができます。 

に`Toolbar`は、次の要素を1つ以上含めることができます。 

- ナビゲーションボタン

- ブランド化ロゴイメージ

- タイトルとサブタイトル

- カスタムビュー

- [アクション] メニュー

- オーバーフローメニュー

Google の[マテリアルデザインガイドライン](https://material.google.com/)では、これらの要素を利用して、アプリを (アプリケーションのアイコンとタイトルだけではなく) 個別の外観にすることを推奨しています。 

このガイドでは、最も一般的に`Toolbar`使用されるシナリオについて説明します。

- アクティビティの既定のアクションバー `Toolbar`をに置き換える。 

- アクティビティに秒`Toolbar`を追加します。

- Android**サポートライブラリ v7 AppCompat**ライブラリ (このガイドの残りの部分では*appcompat*と呼ばれます) `Toolbar`を使用して、以前のバージョンの android にデプロイします。 

 
 
## <a name="requirements"></a>必要条件

`Toolbar`は、Android 5.0 ロリポップ (API 21) 以降で使用できます。 Android 5.0 より前の android リリースを対象としている場合は、 [android サポートライブラリ v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)を`Toolbar`使用します。これにより、NuGet パッケージでの下位互換性がサポートされます。 
[ツールバーの互換性](~/android/user-interface/controls/tool-bar/toolbar-compatibility.md)このライブラリの使用方法について説明します。 




## <a name="related-links"></a>関連リンク

- [ロリポップツールバー (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat ツールバー (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
