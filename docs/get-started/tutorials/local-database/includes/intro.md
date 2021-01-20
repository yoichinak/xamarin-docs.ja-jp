---
ms.openlocfilehash: 62706082496732f60136e4283d5ff7a087488dd3
ms.sourcegitcommit: b75c369adb8e02a429b6c0fed8ba4a855099bf01
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/18/2021
ms.locfileid: "98557138"
---
このチュートリアルを試行する前に、以下を正常に完了しておく必要があります。

- [最初の Xamarin.Forms アプリのビルド](~/get-started/first-app/index.md)のクイック スタート。
- [StackLayout](~/get-started/tutorials/stacklayout/index.yml) のチュートリアル。
- [Button](~/get-started/tutorials/button/index.yml) のチュートリアル。
- [Entry](~/get-started/tutorials/entry/index.yml) のチュートリアル。
- [CollectionView](~/get-started/tutorials/collectionview/index.yml) のチュートリアル。

このチュートリアルでは、以下の内容を学習します。

> [!div class="checklist"]
>
> - NuGet パッケージ マネージャーを使用して、SQLite.NET を Xamarin.Forms プロジェクトに追加する。
> - データ アクセス クラスを作成する。
> - データ アクセス クラスを使用する。

ローカルの SQLite.NET データベースにデータを格納する方法を示す簡単なアプリケーションを作成するには、Visual Studio 2019 または Visual Studio for Mac を使用します。 次のスクリーンショットは、最終的なアプリケーションです。

[![iOS および Android での、ローカル SQLite.NET データベースのデータ永続化のスクリーンショット](../images/consume-data-access-classes-reduced.png "ローカル データベースのデータ永続化")](../images/consume-data-access-classes-large.png#lightbox "ローカル データベースのデータ永続化")
