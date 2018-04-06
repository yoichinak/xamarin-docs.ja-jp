---
title: Mac 用の Visual Studio でのサービスの接続
description: Mac 用 Visual Studio 内からのモバイル アプリを Azure データ記憶域、認証、およびプッシュ通知を追加します。
ms.prod: xamarin
ms.assetid: ADDFB3A5-DB6A-417C-8ACE-33D107FB75C2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: b9aaa197ccf01c59d6e4bbb0476d10295a108f89
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="connected-services-walkthrough"></a>接続済みサービスのチュートリアル

接続済みサービス ワークフローは、サービスを追加するプロジェクトのままにする必要はありませんので、Azure ポータル ワークフローを Visual Studio に、for Mac によってください。

このチュートリアルでは、クラウド データ ストレージ、認証、およびクロスプラット フォーム Xamarin.Forms ポータブル クラス ライブラリ (PCL) アプリケーションにプッシュ通知を表示する、Azure のバックエンド サービスを追加する方法を示します。


1.  ダブルクリックして起動、**接続済みサービス**が起動すると、ソリューション内のノード、 **Services ギャラリー**です。
  これは、このアプリケーションの種類の使用可能なすべてのサービスの一覧です。 サービスを選択して (など**Azure App Service でモバイル バックエンド**) をクリックします。

  [![](connected-services-images/image001-sml.png "Mac 用 Visual Studio でのサービス ノードを接続しています。")](connected-services-images/image001.png#lightbox)

2. サービスの詳細 ページでは、サービスとインストールされる依存関係の説明があります。
  クリックして、**追加**アプリへの依存関係を追加する。

  [![](connected-services-images/image002-sml.png "Azure でのモバイル バックエンド")](connected-services-images/image002.png#lightbox)

3. 依存関係は、PCL およびプラットフォーム固有のプロジェクトが作業に追加する必要があります。
  サービスを (直接または間接的に)、参照するすべてのプロジェクトに追加するチェック ボックスを選択します。

  [![](connected-services-images/image003-sml.png "サービスが参照するすべてのプロジェクトをチェックします。")](connected-services-images/image003.png#lightbox)

4. 選択**Accept**上、**ライセンスの同意**NuGet パッケージのダイアログ ボックス。
  受け入れるには、1 つは、MobileClient、依存関係、および SQLiteStore、用に別のオフライン データ同期に必要な 2 つのダイアログ ボックスである可能性があります。

  [![](connected-services-images/image004-sml.png "ライセンス契約に同意します。")](connected-services-images/image004.png#lightbox)

  ![](connected-services-images/image005.png "ライセンスの同意 ウィンドウ")

5. 依存関係が追加されると、Azure との通信に使用するアカウントでログインするように求められます。
  既に Microsoft ID としてログインしている必要が、Visual Studio for Mac は、Azure サブスクリプションとそれらに関連付けられているすべてのアプリケーション サービスを取得しようとします。 任意のサブスクリプションがない、または Azure ポータルでサブスクリプションのプランを購入する無料の試用版にサインアップして、追加できます。

6. 一覧から、アプリのサービスを選択します。 テンプレートのコード値が設定されます、 `MobileServiceClient` Azure 上のサービスをアプリの対応する URL を持つオブジェクト。

  [![](connected-services-images/image006-sml.png "一覧からアプリ サービスを選択します。")](connected-services-images/image006.png#lightbox)

  サービスの一覧がない場合にクリックして、**新規**ボタン (手順 9. を参照してください)。

7. テンプレートのコードをコピー、 `MobileServiceClient` PCL にします。 ファイルの場所は app 重要ではない、インスタンスは 1 つのみがある限り、します。
  作成する方法をお勧めしています、`AzureService`を Azure のすべての対話を処理し、使用するクラス、 `MobileServiceClient`:

  ![](connected-services-images/image007.png "アプリに構成コードをコピーします。")

8. ドキュメントに従って**次の手順**追加データ、オフライン sync、認証、およびアプリへのプッシュ通知します。

  [![](connected-services-images/image008-sml.png "次の手順の指示を確認します。")](connected-services-images/image008.png#lightbox)

10. For mac Visual Studio 内から新しいサービスを作成するには、既存のアプリケーション サービスをお持ちでない場合
  クリックして、**新規**を開くにはサービスの一覧の左下にあるボタン、**新しい App Service**  ダイアログ。

  [![](connected-services-images/image009-sml.png "Mac 用 Visual Studio で新しいアプリ サービスを作成します。")](connected-services-images/image009.png#lightbox)

新しいサービスには、次のパラメーターが必要です。

-   **App service 名**– プランの一意の名前と id
-   **サブスクリプション**– サービスの支払いに使用するには、サブスクリプション
-   **リソース グループ**– 方法またはプロジェクトのすべての Azure リソースを整理します。 既存を使用するか、または新しく作成するオプション。 これは、最初の Azure サービスである場合、新しいものを作成します。
-   **サービス プラン**– それを使用するすべてのリソースのコストと場所を決定します。 既存を使用するか、または新しく作成するオプション。 最初の Azure サービスの場合は、既定のいずれかを使用または無料プラン (F1) で新しいものを作成します。

参照してください、 [Azure App Service のマニュアル](https://docs.microsoft.com/azure/app-service/)詳細については


## <a name="related-links"></a>関連リンク

- [Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/)
- [リリース ノート](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Connected_Services)
