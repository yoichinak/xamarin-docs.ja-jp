---
title: 一般的によく寄せられる質問
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: 0e49ef8fa0bf00d5ed41f3411393ffaf4891c1b8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013989"
---
# <a name="general-frequently-asked-questions"></a>一般的によく寄せられる質問

## <a name="portable-class-libraries"></a>ポータブル クラス ライブラリ

### <a name="how-can-i-view-what-libraries-are-supported-in-a-pclpcl-support-librariesmd"></a>[PCL でサポートされているライブラリを確認する方法を教えてください](pcl-support-libraries.md)
このガイドでは、既存のライブラリがさまざまな PCL ターゲットプラットフォームでサポートされているかどうか、または PCL プロファイルに変換できるかどうかを判断するためのリソースと方法を示します。

### <a name="pcl-reflection-apipcl-reflectionmd"></a>[PCL Reflection API](pcl-reflection.md)
Microsoft は、ポータブルクラスライブラリで使用するための新しいリフレクション API を開発しました。 PCL に移動する既存のリフレクションコードがある場合は、動作しない可能性があります。

### <a name="pcl-case-study-how-can-i-resolve-problems-related-to-systemdiagnosticstracing-for-the-microsoft-tpl-dataflow-nuget-packagepcl-case-studymd"></a>[PCL のケース スタディ: Microsoft TPL Dataflow NuGet パッケージの System.Diagnostics.Tracing に関連する問題の解決方法](pcl-case-study.md)
Xamarin iOS と Xamarin Android は、参照として許可されているすべての PCL プロファイルの100% を実装していません。 Visual Studio for Mac、Visual Studio、NuGet パッケージマネージャーでの実用的な利便性のために、Xamarin プロジェクトでは、実装が不完全な複数のプロファイルを使用できます。 たとえば、現在、Xamarin iOS と Xamarin Android のいずれにも、`System.Diagnostics.Tracing` PCL 名前空間の型の完全な実装が含まれていません。 この問題を回避するには、アプリプロジェクトを切り替えて、TPL データフローライブラリの net45 + win8 + wp8 + wpa81 バージョンを参照します。

## <a name="nuget-packages--xamarin-components"></a>Xamarin コンポーネント & NuGet パッケージ
### <a name="how-can-i-update-nugetnuget-updatemd"></a>[NuGet の更新方法を教えてください](nuget-update.md)
NuGet の更新プログラム、拡張機能、アドインは、 **Nuget パッケージマネージャー**の **[更新プログラム]** タブにあります。 Visual Studio for Mac & Visual Studio の更新プログラムを検索するための詳細なナビゲーションについては、このガイドを参照してください。

### <a name="how-do-i-downgrade-a-nuget-packagenuget-package-downgrademd"></a>[NuGet パッケージをダウングレードする方法を教えてください](nuget-package-downgrade.md)
Visual Studio for Mac & Visual Studio には、古いバージョンのパッケージを選択して自動的にインストールする機能があります。更新パッケージの動作と同様です。

### <a name="missing-packages-error-after-updating-nuget-packagesnuget-packages-missingmd"></a>[Nuget パッケージを更新した後の不足しているパッケージのエラー](nuget-packages-missing.md)
この問題は、主に Xamarin. Forms サンプルアプリソリューションで報告されていますが、この問題の可能性は、NuGet パッケージを使用するプロジェクトで発生する可能性があります。

### <a name="unifying-google-play-services-components-and-nugetgps-components-nugetmd"></a>[Google Play 開発者サービス コンポーネントと NuGet の統合](gps-components-nuget.md)
いくつかの Google Play 開発者サービスコンポーネントと NuGet パッケージが使用されていましたが、開発者にとって簡単にするために、ここでは、コンポーネントと NuGet パッケージを2つに統合しました。 ほとんどの場合、Google Play 開発者サービスを使用する必要があります。 (Froyo) パッケージを使用する唯一の理由は、Froyo を積極的に対象としている場合です。

### <a name="where-are-the-components-stored-on-my-machinecomponent-storagemd"></a>[コンポーネントはマシンのどこに保存されていますか](component-storage.md)
Xamarin コンポーネントをアプリプロジェクトにインストールするたびに、このガイドに記載されている2つの場所に配置されます。

## <a name="troubleshooting"></a>トラブルシューティング
### <a name="where-can-i-find-my-version-information-and-logsversion-logsmd"></a>[バージョン情報とログはどこにありますか](version-logs.md)
このガイドでは、Xamarin の問題のトラブルシューティングに使用できるほとんどの診断情報を確認する方法について詳しく説明します。

### <a name="when-and-how-should-i-file-a-bug-reporthowto-file-bugmd"></a>[バグを報告する場合と場所を教えてください](howto-file-bug.md)
このガイドでは、品質の高いバグレポートを提出するためのヒントを提供します。これにより、エンジニアは問題の原因 (および潜在的な修正) をより効率的に特定できます。

### <a name="why-isnt-jenkins-supported-by-xamarinxamarin-jenkinsmd"></a>[Xenarin が Jenkins をサポートしていないのはなぜですか](xamarin-jenkins.md)
Jenkins はオープンソースの CI スイートです。このため、Jenkins*自体*によって直接発生する多くの問題は、コードを入手した場所に対する問題としてファイリングする必要があります。たとえば、[メインの Jenkins リポジトリ](https://github.com/jenkinsci/jenkins)や[Jenkins](https://github.com/stisti/jenkins-app)のリポジトリなどです。

### <a name="what-project-settings-are-required-for-the-debuggerdebugger-settingsmd"></a>[デバッガーに必要なプロジェクト設定を教えてください](debugger-settings.md)
デバッガーが想定どおりに動作するようにするには (ブレークポイントにヒットし、デバッグログを表示するなど)、開発者のインストルメンテーションとデバッグ情報の表示を両方とも有効にする必要があります。 このガイドでは、これらの設定を検索してアクティブ化する方法について詳しく説明します。
