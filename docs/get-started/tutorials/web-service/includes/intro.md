---
ms.openlocfilehash: e46c86def8a22450cf8087cf36a6bb05dd24ef70
ms.sourcegitcommit: 4d260b655cb52b990dda79c239a9721f2e964625
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2021
ms.locfileid: "98570772"
---
このチュートリアルを試行する前に、以下を正常に完了しておく必要があります。

- [最初の Xamarin.Forms アプリのビルド](~/get-started/first-app/index.md)のクイック スタート。
- [StackLayout](~/get-started/tutorials/stacklayout/index.yml) のチュートリアル。
- [Label](~/get-started/tutorials/label/index.yml) のチュートリアル。
- [Button](~/get-started/tutorials/button/index.yml) のチュートリアル。
- [CollectionView](~/get-started/tutorials/collectionview/index.yml) のチュートリアル。

このチュートリアルでは、以下の内容を学習します。

> [!div class="checklist"]
>
> - NuGet パッケージ マネージャーを使用して、Newtonsoft.Json を Xamarin.Forms プロジェクトに追加する。
> - Web サービス クラスを作成する。
> - Web サービス クラスを使用する。

GitHub Web API から .NET リポジトリ データを取得する方法を示す簡単なアプリケーションを作成するには、Visual Studio 2019 または Visual Studio for Mac を使用します。 取得したデータは、[`CollectionView`](xref:Xamarin.Forms.CollectionView) に表示されます。 次のスクリーンショットは、最終的なアプリケーションです。

[![iOS と Android における GitHub .NET リポジトリのスクリーンショット](../images/consume-web-service.png)](../images/consume-web-service-large.png#lightbox)
