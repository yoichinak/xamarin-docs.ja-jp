---
title: '[全般] よく寄せられる質問'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 06aa6569301d1bfdbf9f6fd1e7397a38a9beb6f6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61157194"
---
# <a name="general-frequently-asked-questions"></a>[全般] よく寄せられる質問

## <a name="portable-class-libraries"></a>ポータブル クラス ライブラリ

### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[PCL でサポートされているライブラリを確認する方法を教えてください](pcl-support-libraries.md)
このガイドには、リソースと、既存のライブラリは、さまざまな PCL ターゲット プラットフォームでサポートされてまたは PCL プロファイルに変換できる場合を決定するためのメソッドが一覧表示します。

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[PCL Reflection API](pcl-reflection.md)
Microsoft は、ポータブル クラス ライブラリで使用するための新しいリフレクション API を開発しました。 PCL に移動したい既存のリフレクション コードがあれば、これが機能しません。

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[PCL のケース スタディ:Microsoft TPL Dataflow NuGet パッケージの System.Diagnostics.Tracing に関連する問題を解決する方法](pcl-case-study.md)
Xamarin.iOS および Xamarin.Android では、参照として、すべての PCL プロファイルの 100% は実装されていません。 Visual Studio for Mac、Visual Studio、および NuGet パッケージ マネージャーで実用的な利便性のためは、Xamarin プロジェクトは、不完全な実装のみを持ついくつかのプロファイルの使用を許可します。 たとえば、Xamarin.iOS と Xamarin.Android のどちらも現在が含まれます内の型の完全な実装には、 `System.Diagnostics.Tracing` PCL 名前空間。 ポータブル net45 + win8 + wp8 + wpa81 に参照するアプリのプロジェクトを切り替えることでこの問題を回避機能は、TPL データフロー ライブラリのバージョン。

## <a name="nuget-packages--xamarin-components"></a>NuGet パッケージと Xamarin コンポーネント
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[NuGet の更新方法を教えてください](nuget-update.md)
NuGet の更新プログラム、拡張機能、およびアドインを参照して、**更新** タブで、 **NuGet パッケージ マネージャー**します。 Mac と Visual Studio の Visual Studio で、更新プログラムを検索する詳細なナビゲーションは、このガイドでです。

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[NuGet パッケージをダウングレードする方法を教えてください](nuget-package-downgrade.md)
Visual Studio for Mac と Visual Studio の以前のバージョンのパッケージを選択して、自動的にインストールする機能があります。パッケージの更新の動作に似ています。

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Nuget パッケージを更新した後の不足しているパッケージのエラー](nuget-packages-missing.md)
この問題は、Xamarin.Forms サンプル アプリのソリューションで主に報告されていますが、この問題が発生する可能性が NuGet パッケージを使用するプロジェクトで発生することができます。

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Google Play 開発者サービス コンポーネントと NuGet の統合](gps-components-nuget.md)
わかりやすくするための開発者が、いくつかの Google Play Services のコンポーネントと、NuGet パッケージを使用したようになりました統合しました、コンポーネントと NuGet パッケージを 2 つにします。 ほとんどの場合、Google play 開発者サービスを使用する必要があります。 (Froyo) パッケージを使用する唯一の理由は、Froyo は積極的に対象とするかどうかです。

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[コンポーネントはマシンのどこに保存されていますか](component-storage.md)
アプリのプロジェクトには、Xamarin コンポーネントをインストールするときに、このガイドに記載の 2 つの場所に配置を取得します。


## <a name="troubleshooting"></a>トラブルシューティング
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[バージョン情報とログはどこにありますか](version-logs.md)
このガイドでは、Xamarin の問題のトラブルシューティングに使用できるほとんどの診断情報の入手先について説明します。

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[バグを報告する場合と場所を教えてください](howto-file-bug.md)
このガイドは、マイクロソフトのエンジニアが問題の原因 (と任意の考えられる修正内容) をより効率的に決定できるように、高品質なバグのレポートを提出するためのヒントを提供します。

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Xenarin が Jenkins をサポートしていないのはなぜですか](xamarin-jenkins.md)
Jenkins は、オープン ソースの CI スイートです。Jenkins によって直接発生する多くの問題があるため*自体*に対してコードを取得した; などの問題として提出する必要があります、 [Jenkins のメイン リポジトリ](https://github.com/jenkinsci/jenkins)、または、リポジトリの[Jenkins.app](https://github.com/stisti/jenkins-app)します。

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[デバッガーに必要なプロジェクト設定を教えてください](debugger-settings.md)
デバッガー (ブレークポイントのヒット、デバッグ ログの表示など) が期待どおりに動作するためには、開発者のインストルメンテーションとデバッグ情報の表示する必要がありますどちらも有効にします。 このガイドでは、検索してこれらの設定をアクティブ化する方法について説明します。

