---
ms.openlocfilehash: 552251490665b673e02eb58c50c643daed9c1aed
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/09/2020
ms.locfileid: "71107279"
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
> - XAML で Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) を作成する。
> - `ListView` にデータを読み込む。
> - 選択されている `ListView` 項目に応答する。
> - `ListView` セルの外観をカスタマイズする。

[`ListView`](xref:Xamarin.Forms.ListView) の外観をカスタマイズする方法を示す簡単なアプリケーションを作成するには、Visual Studio 2019 または Visual Studio for Mac を使用します。 次のスクリーンショットは、最終的なアプリケーションです。

[![項目がデータ テンプレートでテンプレート化された ListView のスクリーンショット](../images/customize-cell-appearance-reduced.png "テンプレート化されたデータを表示する ListView")](../images/customize-cell-appearance-large.png#lightbox "テンプレート化されたデータを表示する ListView")
