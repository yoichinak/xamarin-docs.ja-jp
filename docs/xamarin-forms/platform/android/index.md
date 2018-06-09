---
title: Android プラットフォーム機能
description: この記事では、Xamarin.Forms アプリを Android 固有の機能を追加する方法について説明し、マテリアルの設計に焦点を当てています。
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/13/2016
ms.openlocfilehash: 2eada518586f222d200ec19aeddc65107d7603b3
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35242405"
---
# <a name="android-platform-features"></a>Android プラットフォーム機能

## <a name="platform-support"></a>プラットフォームのサポート

Xamarin.Forms Android プロジェクトの既定値は、Android 5.0 の前に共通したコントロール renderering の以前のスタイルを使用します。 テンプレートを使用して構築されたアプリケーションが`FormsApplicationActivity`メインのアクティビティの基本クラスとして。

## <a name="material-design-via-appcompat"></a>AppCompat 経由での素材のデザイン

Xamarin.Forms も省略可能な`FormsAppCompatActivity`を使用して**AppCompat**マテリアル デザインのテーマを実装する Android によって提供される機能です。

次の資料デザインのテーマを Xamarin.Forms Android プロジェクトを追加する、 [AppCompat のインストール手順のサポート](appcompat.md)

ここでは、 **Todo** 、既定値はサンプル`FormsApplicationActivity`:

[![](images/before-appcompat-sml.png "AppCompat なしの Todo サンプル アプリケーション")](images/before-appcompat.png#lightbox "AppCompat なしの Todo サンプル アプリケーション")

これとは、同じコードを使用するプロジェクトをアップグレードした後`FormsAppCompatActivity`(およびその他のテーマ情報を追加する)。

[![](images/post-appcompat-sml.png "AppCompat とテーマでの Todo サンプル アプリケーション")](images/post-appcompat.png#lightbox "AppCompat とテーマでの Todo サンプル アプリケーション")

> [!NOTE]
> 使用する場合`FormsAppCompatActivity`、[一部 Android のカスタム レンダラーの基本クラス](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)異なるになります。


## <a name="related-links"></a>関連リンク

- [素材のデザインのサポートを追加します。](appcompat.md)
