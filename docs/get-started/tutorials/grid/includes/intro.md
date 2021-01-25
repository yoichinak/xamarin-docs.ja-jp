---
ms.openlocfilehash: 24fdf5849e94fb80cad40635a44cc8da434d83cc
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98634886"
---
このチュートリアルを試行する前に、以下を正常に完了しておく必要があります。

- [最初の Xamarin.Forms アプリのビルド](~/get-started/first-app/index.md)のクイック スタート。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> - XAML で Xamarin.Forms [`Grid`](xref:Xamarin.Forms.Grid) を作成する。
> - `Grid` の列と行を指定する。
> - `Grid` でコンテンツを複数の列または複数の行にまたがって配置する。

[`Grid`](xref:Xamarin.Forms.Grid) 内にコントロールをレイアウトする方法を示す簡単なアプリケーションを作成するには、Visual Studio 2019 または Visual Studio for Mac を使用します。 次のスクリーンショットは、最終的なアプリケーションです。

[![iOS と Android での、複数の列と行にまたがるコンテンツがある Grid のスクリーンショット](../images/span-columns-rows.png "列と行にまたがるコンテンツがある Grid")](../images/span-columns-rows-large.png#lightbox "列と行にまたがるコンテンツがある Grid")

また、[Xamarin.Forms 向け XAML ホット リロード](~/xamarin-forms/xaml/hot-reload.md)を使用して、アプリケーションをリビルドせずに UI の変更を確認することもできます。
