---
ms.openlocfilehash: 1ff81c0399a0abb321831f8a72dfb2a8560816c4
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98689878"
---
このチュートリアルを試行する前に、以下を正常に完了しておく必要があります。

- [最初の Xamarin.Forms アプリのビルド](~/get-started/first-app/index.md)のクイック スタート。
- [StackLayout](~/get-started/tutorials/stacklayout/index.yml) のチュートリアル。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> - XAML で Xamarin.Forms [`Entry`](xref:Xamarin.Forms.Entry) を作成する。
> - `Entry` の変更中にテキストに応答する。
> - `Entry` の動作をカスタマイズする。

[`Entry`](xref:Xamarin.Forms.Entry) の動作をカスタマイズする方法を示す簡単なアプリケーションを作成するには、Visual Studio 2019 または Visual Studio for Mac を使用します。 次のスクリーンショットは、最終的なアプリケーションです。

[![iOS および Android での、パスワード文字によってテキストがマスクされた Entry のスクリーンショット](../images/customize-behavior.png "マスクされたパスワード文字を使用した Entry")](../images/customize-behavior-large.png#lightbox "マスクされたパスワード文字を使用した Entry")

また、[Xamarin.Forms 向け XAML ホット リロード](~/xamarin-forms/xaml/hot-reload.md)を使用して、アプリケーションをリビルドせずに UI の変更を確認することもできます。
