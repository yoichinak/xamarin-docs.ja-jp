---
title: "Xamarin.Forms セル"
description: "Xamarin.Forms のセルは、Listview および TableViews に追加できます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
ms.openlocfilehash: 509ecc509754bba544115c140e619f634bd64eae
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="xamarinforms-cells"></a>Xamarin.Forms セル

_Xamarin.Forms のセルは、Listview および TableViews に追加できます。_

<style>.tableimg { max-width: none !important;}</style>

## <a name="cells"></a>セル

セルは、テーブル内の項目に使用される特殊な要素であり、リスト内の各項目を描画する方法について説明します。 派生したセル[ `Element`](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/)から VisualElement も派生しています。 セルが視覚的要素ではありません、ただし同様、視覚的要素を作成するためのテンプレートを説明します。 [`Cell`](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) Xamarin.Forms のすべてのセルの基本クラスと機能を提供します。 セルに追加するように設計要素は、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)または[ `TableView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/)コントロール。

使用して、セルをカスタマイズする方法についてを参照してください、 [ListView](~/xamarin-forms/user-interface/listview/index.md)と[テーブル](~/xamarin-forms/user-interface/tableview.md)ドキュメント。

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
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/">EntryCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a>ラベルと 1 つの行テキスト入力フィールドを使用します。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="cells-images/EntryCell.png" title="EntryCell 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/">SwitchCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a>ラベルとオン/オフの切り替えを使用します。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchCellDemoPage.cs"><img src="cells-images/SwitchCell.png" title="SwitchCell 例" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">TextCell</a>
    </td>
    <td valign="top">
A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a>プライマリおよびセカンダリのテキスト。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TextCellDemoPage.cs"><img src="cells-images/TextCell.png" title="TextCell 例" class="tableimg">
    </a></td>
  </tr>
      <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/">ImageCell</a>
    </td>
    <td valign="top">
A<a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">テキスト セル</a>ことも、イメージが含まれています。
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageCellDemoPage.cs"><img src="cells-images/ImageCell.png" title="ImageCell 例" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の概要](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms ギャラリー (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API ドキュメント](https://developer.xamarin.com/api/namespace/Xamarin.Forms/)
