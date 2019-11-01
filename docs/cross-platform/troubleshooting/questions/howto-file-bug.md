---
title: バグを報告する場合と方法を教えてください
description: このドキュメントでは、バグレポートを作成するタイミング、場所、および方法について説明します。 また、エンジニアが問題を最適に診断できるようにするバグレポートのベストプラクティスについても説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: davidortinau
ms.author: daortin
ms.date: 08/01/2018
ms.openlocfilehash: df00eebe682d2d06b99721a2d3c3b90d13454c75
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013999"
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>バグを報告する場合と方法を教えてください

> [!TIP]
> Visual Studio の **[問題の報告]** メニュー項目を使用する &ndash;、問題の解決に役立つ診断情報とバグレポートが送信されます。
>
> 詳細な手順については、 [Visual studio 2019 または Visual studio 2017](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio)および[Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/report-a-problem)を参照してください。
>
> [Visual Studio 開発者コミュニティ](https://developercommunity.visualstudio.com/)の web サイトで、既存のレポートを検索することができます。

## <a name="file-a-bug-if"></a>If...

エンジニアが問題を再現するために使用できると考えられる一連の手順があります。

OR

問題の表示される現象を慎重に説明することができます。特に、問題に関連する正確な状況をいくつか説明することもできます。<sup> [[1]](#note-1)</sup>

## <a name="best-practices-to-help-address-bugs-quickly-and-efficiently"></a>バグを迅速かつ効率的に解決するためのベストプラクティス

1. [Visual Studio 開発者コミュニティ](https://developercommunity.visualstudio.com/)および web で、問題を直接解決する可能性のある既存のバグレポートや使用方法の提案を <a name="ref-1" />検索します。<sup>[[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. 問題について説明し、発生することが予想されているかどうかなど、問題をできるだけ明確かつ簡潔に説明 <a name="ref-2" />ます。

1. 関連するスタックトレース、エラーメッセージテキスト、またはクラッシュログを含める <a name="ref-3" />ます ( **[問題の報告]** 機能を使用する場合は、これらを自動的に含めることができます)。 <sup>[4/4](#note-4)</sup>

1. スクリーンショットの添付ファイルに表示される重要なエラーメッセージをプレーンテキストとして書き出す <a name="ref-4" />ます。

1. <a name="ref-5" />には、できるだけ少ないコードでバグを再現する、自己完結型の小規模なテストケースが含まれています。  (組み込みテンプレートのいずれかを使用して作成された) 新しいプロジェクトで問題を再現できない場合は、問題を示すプロジェクトを zip 圧縮し、バグレポートに添付してください。  サンプルプロジェクトをアタッチする前に、できるだけ単純なものにしてください。<sup>[[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />、オペレーティングシステム、 [Xamarin のバージョン](~/cross-platform/troubleshooting/questions/version-logs.md)、依存関係など、バグが発生した環境について説明します。

## <a name="additional-details"></a>追加の詳細

1. <a name="note-1" />[ *^* ](#ref-1)では、"表示される現象" の説明に十分な詳細情報が含まれている必要があります。これにより、他のお客様は同じ問題が発生しているかどうかを確認できるようになります (同じエラーメッセージ、パフォーマンスの低下、同じスタックトレースクラッシュなど _)。_ "正確な状況" の例の1つとして、たとえば、"私は通常、問題を75% にしていますが、この1つを変更した場合、問題を完全に回避することができます" といったことが考えられます。 "正確な状況" のもう1つの例として、以前のバージョンの Xamarin にダウングレードすると問題が発生しなくなります。

1. 期待どおりに[ *^* ](#ref-2) <a name="note-2" />、通常、エラーテキスト (またはその他の一意の説明文) のスニペットが最適な検索語になります。 既存のバグレポートが不完全な場合は、詳細を追加したり、新しい優れたバグレポートをファイルに追加したりすることができます。

1. もう1つの問題は、Java、目的 C、Swift のいずれのアプリでも同じ問題が報告されているかどうかという <a name="note-3" />[ *^* ](#ref-3)ます。 その場合、問題は、Xamarin の一部ではなく、Android または iOS 自体の一部である可能性が高くなります。

1. <a name="note-4" />には、次のような情報の例が[ *^* ](#ref-4)ます。

    1. プロジェクトのビルド時に発生したエラーについては、完全な[診断ビルド出力](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)をバグレポートに含めてください。

    1. Visual Studio から iOS プロジェクトをビルドまたはデバッグするときに発生するエラーについては、エラーが発生した後、 **Xamarin > Zip ログ > のヘルプ**を実行し、生成された .zip ファイルをバグレポートに含めるようにしてください。

    1. Android または iOS アプリの例外またはクラッシュについては、 [Xamarin android アプリと Xamarin ios アプリ](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps)に関連するデバッグログを含めてください。

1. [ *^* ](#ref-5) <a name="note-5" />、特定の問題に対して可能な場合は、元のソリューションの少数のファイルを新しいソリューションに追加して問題を再作成する方法があります。 多くの場合、Xamarin チームは大規模なテストケースでも問題を調査できます (再現する手順については明確に説明します) が、単純なテストケースでは、バグが迅速に解決される可能性が高くなります。

1. [ *^* ](#ref-6) <a name="note-6" />、新しいソリューションに少数のファイルを追加することによって問題を再現でき_ない_場合は、完全なアプリのソリューションフォルダー全体を zip アップしてアタッチすることができます。 `bin`、`obj`、`Components`、および `packages` フォルダーを削除して、zip ファイルを小さくしてください。 (通常、IDE とビルドプロセスでは、必要に応じてこれらのフォルダーの内容を復元または再作成します)。また、必要な数のコードおよびリソースファイルをプロジェクトから削除することもできます。ただし、最終的には、そのソリューションが元の問題を示している限りです。
