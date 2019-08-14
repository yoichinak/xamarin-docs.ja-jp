---
title: Xamarin. Forms Controls クラスの階層構造
description: 開発者は、Xamarin. フォームアプリケーションのユーザーインターフェイスの作成に使用される型の階層について理解している必要があります。
ms.prod: xamarin
ms.assetid: C89E6B98-464D-4BBE-BF11-13A5FCBBF420
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2019
ms.openlocfilehash: f08146d4439ff1fc22edea71ab1cbb337f64c037
ms.sourcegitcommit: 41a029c69925e3a9d2de883751ebfd649e8747cd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2019
ms.locfileid: "68984394"
---
# <a name="xamarinforms-controls-class-hierarchy"></a>Xamarin. Forms Controls クラスの階層構造

Xamarin は、複数の名前空間で、数百の型で構成されています。 開発者は、 `Xamarin.Forms`名前空間に存在する Xamarin. フォームアプリケーションのユーザーインターフェイスを作成するために使用される型の階層について最もよく理解している必要があります。

これらの型は、ページ、レイアウト、ビュー、およびセルに分割できます。 通常、Xamarin のフォームページは画面全体を占め、すべてのページの種類は[`Page`](xref:Xamarin.Forms.Page)クラスから派生します。 通常、ページにはレイアウトが含まれ、すべてのレイアウトの[`Layout`](xref:Xamarin.Forms.Layout)種類はクラスから派生します。 通常、レイアウトにはビューとその他のレイアウトが含まれ、すべてのビュー [`View`](xref:Xamarin.Forms.View)の種類はクラスから派生します。 最後に、セル[`TableView`](xref:Xamarin.Forms.TableView)と[`ListView`](xref:Xamarin.Forms.ListView)コントロールの表示データで使用される特殊なコントロールです。 ページ、レイアウト、ビュー、およびセルはすべて、最終的には[`Element`](xref:Xamarin.Forms.Element)クラスから派生します。

次のクラス図は、Xamarin でユーザーインターフェイスを構築するために通常使用される型の階層を示しています。

[ ![Xamarin. Forms Controls クラス図](class-hierarchy-images/class-diagram.png "xamarin. forms controls クラス図")](class-hierarchy-images/class-diagram-large.png#lightbox "Xamarin. Forms controls クラスダイアグラム")

> [!NOTE]
> クラスダイアグラムの高解像度バージョンは、[こちら](class-hierarchy-images/class-diagram-high-resolution.png)からダウンロードできます。 ただし、この図では、型と`CarouselView` `CollectionView`型は現在表示されていないことに注意してください。 これらは、コントロールがプレビューから除外されると追加されます。 さらに、図にはシェルの種類が1つだけ表示されます。

## <a name="related-links"></a>関連リンク

- [Xamarin. フォームコントロールのリファレンス](~/xamarin-forms/user-interface/controls/index.md)
