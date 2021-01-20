---
ms.openlocfilehash: b91538b7eff340315e04dd2972bb181e34310b2b
ms.sourcegitcommit: b75c369adb8e02a429b6c0fed8ba4a855099bf01
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98558908"
---
このチュートリアルを試行する前に、以下を正常に完了しておく必要があります。

- [最初の Xamarin.Forms アプリのビルド](~/get-started/first-app/index.md)のクイック スタート。
- [StackLayout](~/get-started/tutorials/stacklayout/index.yml) のチュートリアル。
- [Grid](~/get-started/tutorials/grid/index.yml) のチュートリアル。
- [Label](~/get-started/tutorials/label/index.yml) のチュートリアル。
- [Image](~/get-started/tutorials/image/index.yml) のチュートリアル。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> - XAML で Xamarin.Forms [`CollectionView`](xref:Xamarin.Forms.CollectionView) を作成する。
> - `CollectionView` にデータを読み込む。
> - 選択されている `CollectionView` 項目に応答する。
> - `CollectionView` 項目の外観をカスタマイズする。

[`CollectionView`](xref:Xamarin.Forms.CollectionView) の外観をカスタマイズする方法を示す簡単なアプリケーションを作成するには、Visual Studio 2019 または Visual Studio for Mac を使用します。 次のスクリーンショットは、最終的なアプリケーションです。

[![項目がデータ テンプレートでテンプレート化された CollectionView のスクリーンショット](../images/customize-item-appearance-reduced.png "テンプレート化されたデータを表示する CollectionView")](../images/customize-item-appearance-large.png#lightbox "テンプレート化されたデータを表示する CollectionView")
