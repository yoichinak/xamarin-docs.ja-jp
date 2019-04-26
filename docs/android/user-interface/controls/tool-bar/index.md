---
title: ツール バー
description: 'ツールバーは既定のアクション バーよりさらに柔軟性を提供するアクション バー コンポーネント: アプリで任意の場所に配置できる、そのサイズを変更することができます、およびアプリのテーマとは異なる配色を使用できます。 また、各アプリの画面では、複数のツールバーを持つことができます。'
ms.prod: xamarin
ms.assetid: 22EE5FBD-3240-4308-AF76-EF45D72936DE
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 2c9de4058fdaee53671e65f49ad95c3af5e127d6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61082858"
---
# <a name="toolbar"></a>ツール バー

_ツールバーは既定のアクション バーよりさらに柔軟性を提供するアクション バー コンポーネント: アプリで任意の場所に配置できる、そのサイズを変更することができます、およびアプリのテーマとは異なる配色を使用できます。また、各アプリの画面では、複数のツールバーを持つことができます。_

 
## <a name="overview"></a>概要

Android アクティビティの主要な設計要素が、*操作バー*します。 操作バーは、使用されるは、ナビゲーション、検索、メニューのおよび Android アプリのブランド化である UI コンポーネントです。 Android 5.0 Lollipop、操作バーより前に、の Android のバージョンで (とも呼ばれる、*アプリ バー*) はこの機能を提供するための推奨されるコンポーネントでした。 

`Toolbar`ウィジェット (Android 5.0 Lollipop で導入された) アクション バーのインターフェイスの汎化として考えることができます&ndash;には、操作バーを代替するものです。 `Toolbar`アプリのレイアウトでの任意の場所で使用できるし、は、操作バーよりもはるかにカスタマイズ可能な。 次のスクリーン ショットを示しています、カスタマイズされた`Toolbar`このガイドで作成した例。 

[![エディット、ツールバーのスクリーン ショットの例は、保存、およびメニュー項目をオーバーフローしました](images/01-toolbar-sml.png)](images/01-toolbar.png#lightbox)

重要な相違がある、`Toolbar`と、操作バー。 

-   A`Toolbar`ユーザー インターフェイスで任意の場所に配置できます。

-   複数のツールバーは、同じ画面に表示されることができます。

-   フラグメントを使用している場合に各フラグメントが持つことができます独自`Toolbar`します。 

-   A`Toolbar`画面の部分の幅のみをまたがるように構成できます。 

-   `Toolbar`がバインドされていないアクティビティのウィンドウも親しみやすくの配色を視覚的に異なる配色ことができます。 

-   操作バーとは異なり、`Toolbar`左側のアイコンには含まれません。 右側のメニューでは、小さい領域を使用します。 

-   `Toolbar`高さが調整可能です。 

-   内で他のビューを含めることができる、`Toolbar`します。 

A`Toolbar`次の要素の 1 つ以上含めることができます。 

-   ナビゲーション ボタン

-   ブランド ロゴ イメージ

-   タイトルとサブタイトル

-   カスタム ビュー

-   [操作] メニュー

-   オーバーフロー メニュー

Google の[マテリアル デザイン ガイドライン](https://material.google.com/)(アプリケーション アイコンとタイトルだけに頼るのではなく) 個別の外観は、アプリを提供するこれらの要素の利用をお勧めします。 

このガイドでは、最もよく使われる`Toolbar`シナリオ。

-   アクティビティの既定のアクション バーでの置換を`Toolbar`します。 

-   1 秒あたりの追加`Toolbar`アクティビティにします。

-   使用して、 **Android サポート ライブラリ v7 AppCompat**ライブラリ (と呼ばれる*AppCompat*このガイドの残りの部分で) デプロイする`Toolbar`以前のバージョンの Android です。 

 
 
## <a name="requirements"></a>必要条件

`Toolbar` 以降では、Android 5.0 Lollipop (API 21) は使用できます。 Android 5.0 より前のリリース Android を対象とするときに使用して、 [Android サポート ライブラリ v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)、下位互換性を提供する`Toolbar`NuGet パッケージをサポートします。 
[ツールバーの互換性](~/android/user-interface/controls/tool-bar/toolbar-compatibility.md)このライブラリを使用する方法について説明します。 




## <a name="related-links"></a>関連リンク

- [ロリポップ ツールバー (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat ツールバー (サンプル)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
