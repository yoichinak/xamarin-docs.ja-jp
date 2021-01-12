---
title: Xamarin.Forms コントロールクラスの階層構造
description: 開発者は、アプリケーションのユーザーインターフェイスを作成するために使用される型の階層について理解している必要があり Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: C89E6B98-464D-4BBE-BF11-13A5FCBBF420
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ed3fc6d7aab48886f0c6166390b0ff224f66da60
ms.sourcegitcommit: 1decf2c65dc4c36513f7dd459a5df01e170a036f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/12/2021
ms.locfileid: "98115193"
---
# <a name="no-locxamarinforms-controls-class-hierarchy"></a>Xamarin.Forms コントロールクラスの階層構造

Xamarin.Forms は、複数の名前空間に対して数百の型で構成されています。 開発者は、 Xamarin.Forms 名前空間に存在するアプリケーションのユーザーインターフェイスを作成するために使用される型の階層を最もよく理解している必要があり `Xamarin.Forms` ます。

これらの型は、ページ、レイアウト、ビュー、およびセルに分割できます。 Xamarin.Formsページは通常、画面全体を占め、すべてのページの種類はクラスから派生し [`Page`](xref:Xamarin.Forms.Page) ます。 通常、ページにはレイアウトが含まれ、すべてのレイアウトの種類はクラスから派生し [`Layout`](xref:Xamarin.Forms.Layout) ます。 通常、レイアウトにはビューとその他のレイアウトが含まれ、すべてのビューの種類は最終的にはクラスから派生し [`View`](xref:Xamarin.Forms.View) ます。 最後に、セルとコントロールの表示データで使用される特殊なコントロールです [`TableView`](xref:Xamarin.Forms.TableView) [`ListView`](xref:Xamarin.Forms.ListView) 。 ページ、レイアウト、ビュー、およびセルはすべて、最終的にはクラスから派生し [`Element`](xref:Xamarin.Forms.Element) ます。

次のクラス図は、でユーザーインターフェイスを構築するために通常使用される型の階層を示してい Xamarin.Forms ます。

[![::: no-loc (Xamarin. Forms)::: Controls クラスの図](class-hierarchy-images/class-diagram.png "::: no-loc (Xamarin. Forms)::: controls クラスの図")](class-hierarchy-images/class-diagram-large.png#lightbox "::: no-loc (Xamarin. Forms)::: controls クラスの図")

ただし、この図にはシェルの種類が1つしか表示されないことに注意してください。

> [!NOTE]
> クラスダイアグラムの高解像度バージョンは、 [こちら](class-hierarchy-images/class-diagram-high-resolution.png)からダウンロードできます。

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms コントロールのリファレンス](~/xamarin-forms/user-interface/controls/index.md)
