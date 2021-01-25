---
ms.openlocfilehash: d5acf27dfbda0dcd31b00600cee2f16b98aaa70a
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98634778"
---
このチュートリアルを試行する前に、以下を正常に完了しておく必要があります。

- [最初の Xamarin.Forms アプリのビルド](~/get-started/first-app/index.md)のクイック スタート。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> - XAML で Xamarin.Forms [`StackLayout`](xref:Xamarin.Forms.StackLayout) を作成する。
> - `StackLayout` の向きを指定する。
> - `StackLayout` 内での子ビューの配置と展開を制御する。

[`StackLayout`](xref:Xamarin.Forms.StackLayout) 内にコントロールを配置する方法を示す簡単なアプリケーションを作成するには、Visual Studio 2019 または Visual Studio for Mac を使用します。 次のスクリーンショットは、最終的なアプリケーションです。

[![iOS および Android 上の、配置オプションと展開オプションを設定した StackLayout の子ビューのスクリーンショット](../images/alignment-expansion-reduced.png "配置と展開を設定した、Label インスタンスを含む StackLayout")](../images/alignment-expansion-large.png#lightbox "配置と展開を設定した、Label インスタンスを含む StackLayout")

また、[Xamarin.Forms 向け XAML ホット リロード](~/xamarin-forms/xaml/hot-reload.md)を使用して、アプリケーションをリビルドせずに UI の変更を確認することもできます。
