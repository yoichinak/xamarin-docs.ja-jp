---
title: タイミングと方法は、バグ レポートをファイルする必要がありますか。
description: このドキュメントが説明される場所、および方法にバグを報告します。 バグ レポートの問題を診断する最適なエンジニアを有効にするベスト プラクティスも提供します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: asb3993
ms.author: amburns
ms.date: 06/05/2018
ms.openlocfilehash: b70fe29a79e099f1141c1295d907b48afaa2c3c7
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351607"
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>タイミングと方法は、バグ レポートをファイルする必要がありますか。

Xamarin の Bugzilla バグ トラッカーは、ここでのバグの報告: [ https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all](https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all)します。

## <a name="file-a-bug-if"></a>場合は、バグをファイルにしてください.

Xamarin のエンジニアが Xamarin によって引き起こされる問題を再現するために使用できると思うの手順のセットがあります。

OR

問題の目に見える現象は、問題に関連するいくつかの正確な状況を記述することもできます。 場合に特に慎重に記述できます。<sup> [[1]](#note-1)</sup>


## <a name="best-practices-to-help-xamarin-address-bugs-quickly-and-efficiently"></a>Xamarin のアドレスのバグを迅速かつ効率的に役立つベスト プラクティス


1. <a name="ref-1" />検索[Bugzilla](https://bugzilla.xamarin.com/query.cgi?format=specific&amp;bug_status=__all__)とバグのレポートや、使用状況の提案、問題を直接解決する可能性がありますを既存の web<sup> 。[[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2" />に従って、[ガイドラインを作成するバグ](https://bugzilla.xamarin.com/page.cgi?id=bug-writing.html)を明確にし、できるだけ簡潔の行われる可能性が発生したし、は何の説明を含むが、問題について説明します。

1. <a name="ref-3" />すべての関連するスタック トレース、エラー メッセージのテキストを含めるか、クラッシュ ログします。 <sup>[[4]](#note-4)</sup>

1. <a name="ref-4" />プレーン テキストとしても添付ファイルのスクリーン ショットに表示されるすべての重要なエラー メッセージを書き留めます。

1. <a name="ref-5" />できるだけ小さなコードとしてのバグを再現する、小さい自己完結型のテスト ケースが含まれます。  (いずれかの組み込みのテンプレートを使用して作成した) 新しいプロジェクト、問題を再現することはできません、問題を再現するプロジェクトを zip くださいし、バグ レポートに添付します。  アタッチする前にことプロジェクト例をできるだけ簡単に。<sup> [[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />オペレーティング システムを含むバグが見つかった場所の環境について説明し、[のバージョンの Xamarin](~/cross-platform/troubleshooting/questions/version-logs.md)とすべての依存関係。

---

## <a name="additional-details"></a>追加の詳細

1. <a name="note-1" />[*^*](#ref-1) 理想的には「に見える現象」の説明含める必要があります十分な情報を他の顧客が同じ問題が発生するかどうかを確認することができます (同じエラー メッセージ、同じパフォーマンスの低下、同じのスタック トレース、クラッシュから_など。_). 「環境」、1 つの良い例になります。 場合、ようなものを言うことができます:"は通常問題が発生、時間の 75% が、この 1 つを変更した場合、は問題を回避できます、完全に"。 「正確な状況」のような別の例では、Xamarin の以前のバージョンにダウン グレードする問題を停止するかどうかです。

1. <a name="note-2" />[*^*](#ref-2) 予想どおり、エラーのテキスト (またはその他の一意のわかりやすいテキスト) のスニペットは最適な検索用語では、通常は。 既存のバグのレポートが不完全な場合は、詳細を追加またはファイルの新しいバグ レポートを強化する [ようこそ] は。

1. <a name="note-3" />[*^*](#ref-3) もう 1 つ良い質問は、java の場合、同じ問題が報告されたかどうか、OBJECTIVE-C または Swift のアプリ。 場合は、問題が Xamarin の一部ではなく、Android または iOS 自体のほとんどの一部です。

1. <a name="note-4" />[*^*](#ref-4) 含める情報のいくつかの例:

    1. プロジェクトを作成するときに発生したエラーを記入して、完了の[診断のビルド出力](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)バグ レポートにします。
    
    1. ビルドまたは Visual Studio から iOS プロジェクトをデバッグするときに発生するエラーの場合は、実行_ヘルプ > Xamarin > ログの Zip_後エラーに達すると、バグ レポートの結果として得られる .zip ファイルが含まれます。
    
    1. 例外または Android や iOS のアプリでのクラッシュの場合を含めてください、関連する"[Xamarin.Android と Xamarin.iOS アプリのログをデバッグ](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps)"。

1. <a name="note-5" />[*^*](#ref-5) 可能であれば、特定の問題では、1 つの優れたオプションは、元のソリューションから新しいソリューションに少数のファイルを追加することで、問題を再現するがします。 Xamarin のチームは (を再現する手順が明確に説明したと仮定した場合、) 大規模なテスト_ケースが単純なテスト_ケースことが最適な機会、バグを迅速に解決することであっても問題を調査する多くの場合、できます。


1. <a name="note-6" />[*^*](#ref-6) 場合は_いない_少数のファイルを新しいソリューションに追加することで、問題の再現可能であれば、ことができますを zip 圧縮し、完全なアプリのソリューション全体のフォルダーをアタッチします。 削除してください、 `bin`、 `obj`、 `Components`、および`packages`する郵便番号の小さいファイル フォルダー。 (IDE とのビルド プロセスを通常の復元または必要に応じて、これらのフォルダーの内容を再作成します。)限り、結果として得られるソリューションには、元の問題が示されています同数コードおよびリソース ファイル プロジェクトから必要なを削除することもできます。

