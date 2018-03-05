---
title: "ツール バー"
description: "ツールバーは、既定のアクションのバーより高い柔軟性を提供するアクション バー コンポーネント。 アプリで任意の場所に配置できるため、そのサイズを変更することができます、およびアプリのテーマとは異なる色スキームを使用できます。 また、各アプリの画面は、複数のツールバーを持つことができます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 22EE5FBD-3240-4308-AF76-EF45D72936DE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/22/2018
ms.openlocfilehash: cf2211a572d45b7c29018d00f36cb8408484483f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="toolbar"></a>ツール バー

_ツールバーは、既定のアクションのバーより高い柔軟性を提供するアクション バー コンポーネント。 アプリで任意の場所に配置できるため、そのサイズを変更することができます、およびアプリのテーマとは異なる色スキームを使用できます。また、各アプリの画面は、複数のツールバーを持つことができます。_


<a name="overview" />
 
## <a name="overview"></a>概要

Android のアクティビティの主要な設計要素は、*操作バー*です。 操作バーは、ナビゲーション、検索、メニュー、使用されていると、Android アプリでブランド化である UI コンポーネントです。 Android 5.0 ロリポップ、操作バー前に、のバージョンの Android で (とも呼ばれる、*アプリ バー*) がこの機能を提供するための推奨されるコンポーネントです。 

`Toolbar` (Android 5.0 ロリポップで導入) ウィジェットはようなもののアクション バー インターフェイスの汎化&ndash;操作バーを置き換えるためのものにします。 `Toolbar`レイアウトでは、アプリ、どこでも使用できますであり、アクション バーよりもはるかにカスタマイズ可能な。 次のスクリーン ショットに示しています、カスタマイズされた`Toolbar`このガイドの作成の例。 

[![編集、ツールバーのサンプルのスクリーン ショットは、保存、およびオーバーフロー メニュー項目](images/01-toolbar-sml.png)](images/01-toolbar.png)

重要な相違がある、`Toolbar`と操作バー。 

-   A`Toolbar`ユーザー インターフェイスのどこにでも配置できます。

-   [同じ] 画面では、複数のツールバーを表示できます。

-   フラグメントを使用している場合に各フラグメントが持つことができます独自`Toolbar`です。 

-   A`Toolbar`画面の部分の幅のみをまたがるように構成できます。 

-   `Toolbar`がバインドされていないアクティビティのウィンドウの装飾の配色を視覚的に異なる色スキームがあります。 

-   操作バーとは異なり、`Toolbar`左側のアイコンには含まれません。 右側のメニュー領域が少ないを使用します。 

-   `Toolbar`高さが調整可能です。 

-   内の他のビューを含めることができる、`Toolbar`です。 

A`Toolbar`次の要素の 1 つ以上含めることができます。 

-   ナビゲーション ボタン

-   ブランド ロゴのイメージ

-   タイトルとサブタイトル

-   カスタム ビュー

-   [操作] メニュー

-   オーバーフロー メニュー

Google の[マテリアルのデザイン ガイドライン](https://material.google.com/)(アプリケーションのアイコンとタイトルのみに依存するのではなく) を参照して、個別にアプリを付与するには、これら要素の利用をお勧めします。 

このガイドは、最も一般的に使用されるについて説明します`Toolbar`シナリオ。

-   アクティビティの既定のアクションのバーを置き換えて、`Toolbar`です。 

-   1 秒あたりの追加`Toolbar`アクティビティにします。

-   使用して、 **Android のサポート ライブラリ v7 AppCompat**ライブラリ (と呼ばれる*AppCompat*このガイドの残りの部分) を展開する`Toolbar`Android の以前のバージョン。 

 
<a name="requirements" />
 
## <a name="requirements"></a>必要条件

`Toolbar` 以降では、Android 5.0 ロリポップ (API 21) は使用できます。 Android 5.0 より前のリリース Android を対象とするときに使用して、 [Android サポート ライブラリ v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)、下位互換性を提供する`Toolbar`NuGet パッケージでサポートします。 
[ツールバーの互換性](~/android/user-interface/controls/tool-bar/toolbar-compatibility.md)このライブラリを使用する方法について説明します。 




## <a name="related-links"></a>関連リンク

- [ロリポップ ツールバー (サンプル)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [AppCompat ツールバー (サンプル)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
