---
title: " Xamarin.Forms コントロールクラス階層" の説明: "開発者は、アプリケーションのユーザーインターフェイスを作成するために使用される型の階層について理解している必要があり Xamarin.Forms ます。"
ms. 製品: xamarin ms. assetid: C89E6B98-464D-4BBE-BF11-13A5FCBBF420: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 01/07/2020 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-controls-class-hierarchy"></a>Xamarin.Formsコントロールクラスの階層構造

Xamarin.Formsは、複数の名前空間に対して数百の型で構成されています。 開発者は、 Xamarin.Forms 名前空間に存在するアプリケーションのユーザーインターフェイスを作成するために使用される型の階層を最もよく理解している必要があり `Xamarin.Forms` ます。

これらの型は、ページ、レイアウト、ビュー、およびセルに分割できます。 Xamarin.Formsページは通常、画面全体を占め、すべてのページの種類はクラスから派生し [`Page`](xref:Xamarin.Forms.Page) ます。 通常、ページにはレイアウトが含まれ、すべてのレイアウトの種類はクラスから派生し [`Layout`](xref:Xamarin.Forms.Layout) ます。 通常、レイアウトにはビューとその他のレイアウトが含まれ、すべてのビューの種類は最終的にはクラスから派生し [`View`](xref:Xamarin.Forms.View) ます。 最後に、セルとコントロールの表示データで使用される特殊なコントロールです [`TableView`](xref:Xamarin.Forms.TableView) [`ListView`](xref:Xamarin.Forms.ListView) 。 ページ、レイアウト、ビュー、およびセルはすべて、最終的にはクラスから派生し [`Element`](xref:Xamarin.Forms.Element) ます。

次のクラス図は、でユーザーインターフェイスを構築するために通常使用される型の階層を示してい Xamarin.Forms ます。

[![Xamarin.Formsコントロールクラスの図](class-hierarchy-images/class-diagram.png "[!ファンド.NO LOC (Xamarin. Forms)] コントロールクラスダイアグラム")](class-hierarchy-images/class-diagram-large.png#lightbox "[!ファンド.NO LOC (Xamarin. Forms)] コントロールクラスダイアグラム")

> [!NOTE]
> クラスダイアグラムの高解像度バージョンは、[こちら](class-hierarchy-images/class-diagram-high-resolution.png)からダウンロードできます。 ただし、この図にはシェルの種類が1つしか表示されないことに注意してください。

## <a name="related-links"></a>関連リンク

- [Xamarin.Formsコントロールのリファレンス](~/xamarin-forms/user-interface/controls/index.md)
