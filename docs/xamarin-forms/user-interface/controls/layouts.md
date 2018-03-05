---
title: "Xamarin.Forms レイアウト"
description: "Xamarin.Forms のレイアウトを使用して、論理構造体にユーザー インターフェイス コントロールを作成します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: F4180997-BA21-453A-9958-D1E2940DF050
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: ecea0f55360fcde7a50c52bb33c45a2c5fff5eeb
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-layouts"></a>Xamarin.Forms レイアウト

_Xamarin.Forms のレイアウトを使用して、論理構造体にユーザー インターフェイス コントロールを作成します。_

<style>.tableimg { max-width: none !important;}</style>

## <a name="layouts"></a>レイアウト

[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout) Xamarin.Forms 内のクラス ビューのレイアウトまたはその他のコンテナーとして機能するビューの特殊なサブタイプであります。 通常、Xamarin.Forms アプリケーションで、子要素のサイズと位置を設定するためのロジックが含まれています。

 [ ![](layouts-images/layouts-sml.png "Xamarin.Forms レイアウト型")](layouts-images/layouts.png "Xamarin.Forms レイアウトの種類")

<br clear="all" />

Xamarin.Forms をサポートします。

<table align="center" border="1" cellpadding="1" cellspacing="1">
<thead>
    <th>
      <strong>型</strong>
    </th>
    <th>
      <strong>説明</strong>
    </th>
    <th style="min-width:400px">
      <strong>スクリーン ショット</strong>
    </th>
  </thead>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/">ContentPresenter</a>
    </td>
    <td valign="top">
テンプレート化されたビューのレイアウト マネージャー。 内で使用される、 <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/">ControlTemplate</a>を提示するコンテンツの表示位置をマークします。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/Templates/ControlTemplates/SimpleTheme/SimpleTheme/App.xaml"><img src="layouts-images/ContentPresenter.png" title="ContentPresenter 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/">ContentView</a>
    </td>
    <td valign="top">
単一のコンテンツを持つ要素。 ContentView が、非常に簡単に使用します。 その目的は、ユーザー定義の複合ビューの基底クラスとして機能します。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ContentViewDemoPage.cs"><img src="layouts-images/ContentView.png" title="ContentView 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/">フレーム</a>
    </td>
    <td valign="top">
フレーム オプションの 1 つの子を含む要素です。 フレームは、既定値を持つ<a href="https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/">Xamarin.Forms.Layout.Padding</a> 20 です。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/FrameDemoPage.cs"><img src="layouts-images/Frame.png" title="フレームの例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a>
    </td>
    <td valign="top">
コンテンツである場合にスクロール可能な要素が必要です。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ScrollViewDemoPage.cs"><img src="layouts-images/ScrollView.png" title="ScrollView 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/">TemplatedView</a>
    </td>
    <td valign="top">
コントロール テンプレートとの基本クラスのコンテンツを表示する要素<a href=""/api/type/Xamarin.Forms.ContentView/">ContentView</a>です。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/tree/master/Templates/ControlTemplates/"><img src="layouts-images/TemplatedView.png" title="TemplatedView 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/">AbsoluteLayout</a>
    </td>
    <td valign="top">
絶対要求された位置にある子要素を配置します。 アンカーと境界を割り当てられているユーザーは、コントロールのサイズと位置を定義します。
    </td>
    <td valign="top">
      <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/AbsoluteLayoutDemoPage.cs"><img src="layouts-images/AbsoluteLayout.png" title="AbsoluteLayout 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/">Grid</a>
    </td>
    <td valign="top">
行と列に配置されたビューを含むレイアウトです。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/GridDemoPage.cs"><img src="layouts-images/Grid.png" title="グリッドの例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/">RelativeLayout</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/%601">レイアウト</a>を使用して<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Constraint/">制約</a>レイアウトの子。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/RelativeLayoutDemoPage.cs"><img src="layouts-images/RelativeLayout.png" title="[相対レイアウト] の使用例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/">レイアウト</a>垂直または水平方向に配置できますが、1 行に子要素を配置します。 このレイアウトは設定、子境界に自動的にレイアウト サイクル中に。 境界を割り当てられているユーザーが上書きされ、したがって設定しないで、子要素で、ユーザーがします。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StackLayoutDemoPage.cs"><img src="layouts-images/StackLayout.png" title="StackLayout 例" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の概要](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms ギャラリー (サンプル)](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Xamarin.Forms API ドキュメント](https://developer.xamarin.com/api/namespace/Xamarin.Forms)
