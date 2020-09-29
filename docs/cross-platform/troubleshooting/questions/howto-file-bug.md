---
title: バグを報告する場合と方法を教えてください
description: このドキュメントでは、バグレポートを作成するタイミング、場所、および方法について説明します。 また、エンジニアが問題を最適に診断できるようにするバグレポートのベストプラクティスについても説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: davidortinau
ms.author: daortin
ms.date: 08/01/2018
ms.openlocfilehash: 38a96f5f499b760aedcfc51dc9e6326c9c62e95b
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457875"
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>バグを報告する場合と方法を教えてください

> [!TIP]
> Visual Studio の [ **問題の報告** ] メニュー項目を使用すると、 &ndash; 問題の解決に役立つ診断情報がバグレポートと共に送信されます。
>
> 詳細な手順については、 [Visual studio 2019 または Visual studio 2017](/visualstudio/ide/how-to-report-a-problem-with-visual-studio) および [Visual Studio for Mac](/visualstudio/mac/report-a-problem)を参照してください。
>
> [Visual Studio 開発者コミュニティ](https://developercommunity.visualstudio.com/)の web サイトで、既存のレポートを検索することができます。

## <a name="file-a-bug-if"></a>If...

エンジニアが問題を再現するために使用できると考えられる一連の手順があります。

OR

問題の表示される現象を慎重に説明することができます。特に、問題に関連する正確な状況をいくつか説明することもできます。<sup> [[1]](#note-1)</sup>

## <a name="best-practices-to-help-address-bugs-quickly-and-efficiently"></a>バグを迅速かつ効率的に解決するためのベストプラクティス

1. <a name="ref-1"></a>[Visual Studio 開発者コミュニティ](https://developercommunity.visualstudio.com/)と web を検索して、既存のバグレポートや、問題を直接解決できる可能性のある使用方法を探します。<sup>[[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2"></a>発生した原因と予想される問題についての説明など、問題をできるだけ明確かつ簡潔に説明します。

1. <a name="ref-3"></a>関連するスタックトレース、エラーメッセージテキスト、またはクラッシュログを含めます ([ **問題の報告** ] 機能を使用する場合、これらは自動的に含めることができます)。 <sup>[4/4](#note-4)</sup>

1. <a name="ref-4"></a>スクリーンショットの添付ファイルに表示される重要なエラーメッセージをプレーンテキストとして書き留めます。

1. <a name="ref-5"></a>できるだけ少ないコードでバグを再現する、自己完結型の小規模なテストケースを含めます。  (組み込みテンプレートのいずれかを使用して作成された) 新しいプロジェクトで問題を再現できない場合は、問題を示すプロジェクトを zip 圧縮し、バグレポートに添付してください。  サンプルプロジェクトをアタッチする前に、できるだけ単純なものにしてください。<sup>[[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6"></a>オペレーティングシステム、 [Xamarin のバージョン](~/cross-platform/troubleshooting/questions/version-logs.md) 、依存関係など、バグが発生した環境について説明します。

## <a name="additional-details"></a>追加情報

1. <a name="note-1"></a>[*^*](#ref-1) 理想的には、"表示される現象" の説明に十分な詳細が含まれている必要があります。これにより、他のお客様は同じ問題が発生しているかどうかを確認できるようになります (同じエラーメッセージ、パフォーマンスの低下、クラッシュからのスタックトレース _など_)。 "正確な状況" の例の1つとして、たとえば、"私は通常、問題を75% にしていますが、この1つを変更した場合、問題を完全に回避することができます" といったことが考えられます。 "正確な状況" のもう1つの例として、以前のバージョンの Xamarin にダウングレードすると問題が発生しなくなります。

1. <a name="note-2"></a>[*^*](#ref-2) 予想どおりに、エラーテキスト (またはその他の一意の説明文) のスニペットは、通常、最適な検索用語です。 既存のバグレポートが不完全な場合は、詳細を追加したり、新しい優れたバグレポートをファイルに追加したりすることができます。

1. <a name="note-3"></a>[*^*](#ref-3) もう1つの良い質問は、Java、目的 C、または Swift アプリで同じ問題が報告されているかどうかです。 その場合、問題は、Xamarin の一部ではなく、Android または iOS 自体の一部である可能性が高くなります。

1. <a name="note-4"></a>[*^*](#ref-4) いくつかの情報の例を次に示します。

    1. プロジェクトのビルド時に発生したエラーについては、完全な [診断ビルド出力](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) をバグレポートに含めてください。

    1. Visual Studio から iOS プロジェクトをビルドまたはデバッグするときに発生するエラーについては、エラーが発生した後、 **Xamarin > Zip ログ > のヘルプ** を実行し、生成された .zip ファイルをバグレポートに含めるようにしてください。

    1. Android または iOS アプリの例外またはクラッシュについては、 [Xamarin android アプリと Xamarin ios アプリ](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps)に関連するデバッグログを含めてください。

1. <a name="note-5"></a>[*^*](#ref-5) 特定の問題に対して可能な場合は、元のソリューションの少数のファイルを新しいソリューションに追加して問題を再作成する方法があります。 多くの場合、Xamarin チームは大規模なテストケースでも問題を調査できます (再現する手順については明確に説明します) が、単純なテストケースでは、バグが迅速に解決される可能性が高くなります。

1. <a name="note-6"></a>[*^*](#ref-6) 新しいソリューションに少数のファイルを追加することによって問題を再現でき _ない_ 場合は、完全なアプリのソリューションフォルダー全体を zip アップしてアタッチすることができます。 、、、の各フォルダーを削除して、 `bin` `obj` `Components` zip ファイルを小さくしてください `packages` 。 (通常、IDE とビルドプロセスでは、必要に応じてこれらのフォルダーの内容を復元または再作成します)。また、必要な数のコードおよびリソースファイルをプロジェクトから削除することもできます。ただし、最終的には、そのソリューションが元の問題を示している限りです。