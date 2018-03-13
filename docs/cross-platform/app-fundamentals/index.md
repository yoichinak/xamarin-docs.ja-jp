---
title: "アプリケーションの基礎"
description: "中核となるアプリケーションの概念"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: c5b823370e5b65fbcf9ba366cb89c05e003b1a89
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="application-fundamentals"></a>アプリケーションの基礎

このセクションでは、いくつかの一般的な処理タスクまたはモバイル アプリケーションを開発するときに注意する必要がある開発者の概念のガイドを提供します。

##  <a name="building-cross-platform-applicationscross-platformapp-fundamentalsbuilding-cross-platform-applicationsindexmd"></a>[クロスプラットフォーム アプリケーションの構築](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)

Xamarin を選択し、デザインし、モバイル アプリケーションを開発するときに、いくつかの点に注意を保持する、することができます膨大なコードをモバイル プラットフォーム間で共有を実現に要する時間を短縮、既存の能力を活用してモバイル アクセスは、顧客の需要を満たすクロスプラット フォームの複雑さを軽減します。&nbsp;ユーティリティおよび生産性アプリケーションに対してこれらの利点を実現する主要なガイドラインを説明します。

## <a name="code-sharing-optionscode-sharingmd"></a>[コード共有オプション](code-sharing.md)

Xamarin プロジェクト、ポータブル クラス ライブラリ (Pcl)、共有プロジェクト、および標準の .NET ライブラリを含む使用可能なオプションを共有する別のコードについて説明します。


## <a name="accessibilityaccessibilitymd"></a>[ユーザー補助](accessibility.md)

ユーザー補助アプリケーションを構築するためのヒント。


## <a name="localizationlocalizationmd"></a>[ローカリゼーション](localization.md)

ロケールに対応するアプリのためのガイドラインは、複数の言語に変換することができます。


##  <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[ポータブル クラス ライブラリ](~/cross-platform/app-fundamentals/pcl.md)

ポータブル クラス ライブラリ プロジェクトでは、ビルドし、複数のプラットフォームで実行する共有コードを含むアセンブリを配布できます。 ポータブル クラス ライブラリ (または"PCL") を作成するには、最初を対象とし、設定は、これらのプラットフォームに対して定義されているプロファイルで使用可能な .NET Framework のサブセットに対してコードを記述するプラットフォームを選択します。 このドキュメントでは、作成して、Xamarin を使用した Pcl を使用する方法について説明します。

##  <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[共有プロジェクト](~/cross-platform/app-fundamentals/shared-projects.md)

共有プロジェクトでは、別のアプリケーション プロジェクトの数によって参照されている共通のコードを記述できます。 コードでは、各参照元のプロジェクトの一部としてコンパイルされ、共有コード ベースのプラットフォーム固有の機能を組み込むためのコンパイラ ディレクティブを含めることができます。 この記事では、共有プロジェクトのしくみ、および作成して、Xamarin のプロジェクトでそれらを使用する方法について説明します。

##  <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

.NET 標準は、プラットフォーム間でコードを共有するための新しいオプションです。 ポータブル クラス ライブラリに同様の方法で動作します。コードでは、特定のバージョン (現在 1.0 1.6 ~) に対しては構築され、以降そのレベルをサポートするその他のプロジェクトで使用できます。 標準の .NET プロジェクトでは Xamarin Studio 6.2、Visual Studio for Windows、および Visual Studio for mac

##  <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[コードを共有するための NuGet プロジェクト: マルチプラット フォーム ライブラリ](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

NuGet パッケージを PCL プロジェクトまたは .NET 標準プロジェクト; から自動的に生成できます。共有プロジェクトを個別の NuGet のプロジェクトの種類を使用して「お連絡とり」NuGet パッケージにパッケージ化できます。 このセクションでは、各コードを共有するシナリオ用の NuGet パッケージを作成する方法について説明します。

##  <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Xamarin の NuGet パッケージを手動で作成します。](~/cross-platform/app-fundamentals/nuget-manual.md)

NuGet パッケージを作成するためのヒントは、Xamarin プラットフォームで動作します。

##  <a name="cross-platform-data-accessxamarin-formsdata-cloudindexmd"></a>[クロス プラットフォームのデータ アクセス](~/xamarin-forms/data-cloud/index.md)

ほとんどのアプリケーションでは、デバイスをローカルでのデータを保存するには、いくつか必要があります。 データの量が小さい普通でない限り通常が必要に、データベースとデータベースへのアクセスを管理するアプリケーションでデータ層です。 iOS および Android の両方がある「組み込み」SQLite データベース エンジンと Xamarin のプラットフォームでデータを格納および取得のアクセスが簡素化されます。 [Android データ アクセス](~/android/data-cloud/data-access/index.md)、 [iOS データ アクセス](~/ios/data-cloud/data/index.md)、および[Xamarin.Forms データ アクセス](~/xamarin-forms/data-cloud/index.md)ガイドは、各プラットフォームで SQLite にアクセスする方法の例を示します。


##  <a name="transport-layer-securitytransport-layer-securitymd"></a>[トランスポート層セキュリティ](transport-layer-security.md)

せんたくについては、アプリのネットワーク接続をセキュリティで保護する SSL/TLS 実装を修正します。


##  <a name="notificationsxamarin-formsdata-cloudpush-notificationsindexmd"></a>[通知](~/xamarin-forms/data-cloud/push-notifications/index.md)

モバイル アプリケーションは、いくつかアプリケーションの特定のイベントが発生したことをユーザーに通知の控えめな方法として通知を使用します。 通知は、バック グラウンドで実行されているアプリケーション プロセスの状態のユーザーに通知する通常使用されます。 この例には、サイズの大きなファイルをダウンロード可能性があります。 バック グラウンドでこのアクティビティを実行するようにこのファイルをダウンロードするに時間がかかる可能性があります。 ダウンロードが完了したら、ユーザーは、通知によってファクトの通知します。
さらに、通知 are ローカル アプリケーションにのみ制限されます。 サーバー アプリケーションはモバイル アプリケーションへの通知を発行することもできます。 この記事では、Android と iOS の両方で通知を使用する方法を説明します。
