---
title: ToolBar
description: ツールバーは、既定の操作バーよりも柔軟な操作バーコンポーネントです。アプリ内の任意の場所に配置でき、そのサイズを変更できます。また、アプリのテーマとは異なる配色を使用できます。 また、各アプリの画面には複数のツールバーがあります。
ms.prod: xamarin
ms.assetid: 22EE5FBD-3240-4308-AF76-EF45D72936DE
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 3f4a9393d8ef6ef54ba3dba29f1109080f1e321a
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029103"
---
# <a name="toolbar"></a>ToolBar

_ツールバーは、既定の操作バーよりも柔軟な操作バーコンポーネントです。アプリ内の任意の場所に配置でき、そのサイズを変更できます。また、アプリのテーマとは異なる配色を使用できます。また、各アプリの画面には複数のツールバーがあります。_

## <a name="overview"></a>概要

Android アクティビティの主要なデザイン要素は、*アクションバー*です。 アクションバーは、Android アプリのナビゲーション、検索、メニュー、およびブランド化に使用される UI コンポーネントです。 Android 5.0 をロリポップにする前の Android バージョンでは、操作バー (*アプリバー*とも呼ばれます) が、この機能を提供するために推奨されるコンポーネントでした。 

(Android 5.0 ロリポップで導入された) `Toolbar` ウィジェットは、アクションバーのインターフェイスの一般化として考えることができ &ndash; 操作バーを置き換えることを意図しています。 `Toolbar` は、アプリレイアウト内の任意の場所で使用できます。また、操作バーよりもはるかにカスタマイズできます。 次のスクリーンショットは、このガイドで作成したカスタマイズされた `Toolbar` の例を示しています。 

[![編集、保存、およびオーバーフローの各メニュー項目を含むツールバーのスクリーンショットの例](images/01-toolbar-sml.png)](images/01-toolbar.png#lightbox)

`Toolbar` とアクションバーには、いくつかの重要な違いがあります。 

- `Toolbar` は、ユーザーインターフェイス内の任意の場所に配置できます。

- 複数のツールバーを同じ画面に表示できます。

- フラグメントが使用されている場合、各フラグメントは独自の `Toolbar`を持つことができます。 

- `Toolbar` は、画面の一部の幅にのみまたがるように構成できます。 

- `Toolbar` は、アクティビティのウィンドウ décor の配色にバインドされていないため、視覚的に区別される配色を持つことができます。 

- 操作バーとは異なり、`Toolbar` には左側にアイコンが含まれていません。 右側のメニューの使用領域が少なくなります。 

- `Toolbar` の高さは調整可能です。 

- その他のビューは `Toolbar`内に含めることができます。 

`Toolbar` には、次の要素を1つ以上含めることができます。 

- ナビゲーションボタン

- ブランド化ロゴイメージ

- タイトルとサブタイトル

- カスタムビュー

- [アクション] メニュー

- オーバーフローメニュー

Google の[マテリアルデザインガイドライン](https://material.google.com/)では、これらの要素を利用して、アプリを (アプリケーションのアイコンとタイトルだけではなく) 個別の外観にすることを推奨しています。 

このガイドでは、最も一般的に使用される `Toolbar` シナリオについて説明します。

- アクティビティの既定のアクションバーを `Toolbar`に置き換えます。 

- アクティビティに2つ目の `Toolbar` を追加します。

- Android**サポートライブラリ V7 AppCompat**ライブラリ (このガイドの残りの部分では*appcompat*と呼ばれます) を使用して、以前のバージョンの android に `Toolbar` をデプロイします。 

## <a name="requirements"></a>［要件］

`Toolbar` は、Android 5.0 ロリポップ (API 21) 以降で使用できます。 Android 5.0 より前の Android リリースを対象としている場合は、 [Android サポートライブラリ V7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)を使用します。これにより、NuGet パッケージで互換性のある下位互換性 `Toolbar` サポートが提供されます。 
[ツールバーの互換性](~/android/user-interface/controls/tool-bar/toolbar-compatibility.md)このライブラリの使用方法について説明します。 

## <a name="related-links"></a>関連リンク

- [ロリポップツールバー (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-toolbar)
- [AppCompat ツールバー (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/supportv7-appcompat-toolbar)
