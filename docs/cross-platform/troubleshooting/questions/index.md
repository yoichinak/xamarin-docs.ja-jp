---
title: よく寄せられる質問 [全般]
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 579393a8830e73f7ab64f750c0176eb4c4e36029
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
# <a name="general-frequently-asked-questions"></a>よく寄せられる質問 [全般]

## <a name="portable-class-libraries"></a>ポータブル クラス ライブラリ
### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[PCL でサポートされているライブラリを確認する方法を教えてください](pcl-support-libraries.md)
このガイドには、リソースと、既存のライブラリは、さまざまな PCL ターゲット プラットフォームでサポートされてまたは PCL プロファイルに変換できる場合を判断するためのメソッドが一覧表示します。

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[PCL Reflection API](pcl-reflection.md)
Microsoft は、ポータブル クラス ライブラリで使用するための新しいリフレクション API を開発しました。 場合は、PCL に移動する既存リフレクションのコードがある場合は、その動かないことがあります。

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[PCL のケース スタディ: Microsoft TPL Dataflow NuGet パッケージの System.Diagnostics.Tracing に関連する問題の解決方法](pcl-case-study.md)
Xamarin.iOS および Xamarin.Android には、参照として、すべての PCL プロファイルの 100% が実装していません。 Mac、Visual Studio、および、NuGet package manager 用 Visual Studio で実用的な便宜上は、Xamarin のプロジェクトは、のみ不完全な実装を持つ複数のプロファイルの使用を許可します。 たとえば、Xamarin.iOS と Xamarin.Android のどちらも現在が含まれています、完全な実装型のには`System.Diagnostics.Tracing`PCL 名前空間。 ポータブル net45 + win8 + wp8 + wpa81 を参照する、アプリ プロジェクトを切り替えることによってこれを回避することができます、TPL データ フロー ライブラリのバージョン。

## <a name="nuget-packages--xamarin-components"></a>NuGet パッケージの管理と Xamarin コンポーネント
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[NuGet の更新方法を教えてください](nuget-update.md)
NuGet の更新プログラム、拡張、およびアドインで見つかります、**更新** タブで、 **NuGet Package Manager**です。 Mac と Visual Studio の Visual Studio での更新プログラムを検索する詳細なナビゲーションは、このガイドでです。

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[NuGet パッケージをダウングレードする方法を教えてください](nuget-package-downgrade.md)
Visual Studio for Mac と Visual Studio の両方を古いバージョンのパッケージを選択し、それらを自動的にインストールする機能があります。パッケージの更新の動作に似ています。

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Nuget パッケージを更新した後の不足しているパッケージのエラー](nuget-packages-missing.md)
この問題は主に、Xamarin.Forms サンプル アプリのソリューションで報告されてが NuGet パッケージを使用するプロジェクトでこの問題が発生する可能性が起こります。

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Google Play 開発者サービス コンポーネントと NuGet の統合](gps-components-nuget.md)
わかりやすくするための開発者が、いくつかの Google プレイ サービス コンポーネントと NuGet パッケージを使用してしたようになりました、統合された、コンポーネントおよび NuGet パッケージを 2 つにします。 ほぼすべてのケースでは、Google Play サービスを使用してください。 (Froyo) パッケージを使用する唯一の理由は、Froyo は積極的に対象とするかどうかです。

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[コンポーネントはマシンのどこに保存されていますか](component-storage.md)
アプリのプロジェクトには、Xamarin コンポーネントをインストールするときに、このガイドに表示されている 2 つの場所に配置を取得します。


## <a name="troubleshooting"></a>トラブルシューティング
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[バージョン情報とログはどこにありますか](version-logs.md)
このガイドでは、Xamarin の問題のトラブルシューティングに使用できるほとんどの診断情報の入手先について説明します。

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[バグを報告する場合と場所を教えてください](howto-file-bug.md)
このガイドは、マイクロソフトのエンジニアがより効率的に問題の原因 (とその潜在的な修正) を決定することができるように質の高いバグのレポートを提出するためのヒントを提供します。

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Xenarin が Jenkins をサポートしていないのはなぜですか](xamarin-jenkins.md)
Jenkins は、オープン ソースの CI スイートです。Jenkins によって直接発生は、このような多数の問題があるため*自体*に対してコードを取得した; などの問題としては扱わする必要があります、[メインの Jenkins リポジトリ](https://github.com/jenkinsci/jenkins)、またはのリポジトリ[Jenkins.app](https://github.com/stisti/jenkins-app)です。

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[デバッガーに必要なプロジェクト設定を教えてください](debugger-settings.md)
デバッガー (ヒット ブレークポイント、デバッグ ログの表示など) を正常に動作するためには、開発者のインストルメンテーションとデバッグ情報の表示する必要がありますどちらも有効にします。 このガイドでは、検索して、これらの設定をアクティブ化する方法について説明します。

