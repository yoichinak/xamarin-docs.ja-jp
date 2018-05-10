---
title: タイミングと方法はバグのレポートをファイル必要がありますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: asb3993
ms.author: amburns
ms.openlocfilehash: d5471195b5051725b41b8f28b3959db362297669
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>タイミングと方法はバグのレポートをファイル必要がありますか。


Xamarin の Bugzilla バグの追跡ツールでは、ここにバグのファイル: [ https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all](https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all)です。

## <a name="file-a-bug-if"></a>場合は、バグをファイルしてください.


Xamarin エンジニアを使用して Xamarin によって引き起こされる問題を再現できると思われるステップのセットがあります。

OR

特に問題に関連するいくつかの正確な状況を説明することも、場合、表示されている問題の現象、慎重に記述できます。<sup> [[1]](#note-1)</sup>


## <a name="best-practices-to-help-xamarin-address-bugs-quickly-and-efficiently"></a>Xamarin アドレス バグを迅速かつ効率的に役立つベスト プラクティス


1. <a name="ref-1" />検索[Bugzilla](https://bugzilla.xamarin.com/query.cgi?format=specific&amp;bug_status=__all__)と既存のバグのレポートまたは直接問題に対処する提案を使用状況 web<sup> 。[[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2" />に従って、[文書作成のガイドライン バグ](https://bugzilla.xamarin.com/page.cgi?id=bug-writing.html)を明確にし、可能な限り簡潔に何が発生してがの説明を含むで期待どおりに発生する問題を記述します。

1. <a name="ref-3" />すべての関連するスタック トレース、エラー メッセージ テキストを含めるか、クラッシュ ログします。 <sup>[[4]](#note-4)</sup>

1. <a name="ref-4" />プレーン テキストとしてすぎるスクリーン ショットの添付ファイルに表示されるすべての重要なエラー メッセージを書き留めます。

1. <a name="ref-5" />限り小さなコードのバグを再現する小さな、自己完結型のテスト ケースが含まれます。  (いずれかの組み込みのテンプレートを使用して作成した) 新しいプロジェクトでの問題を再現できない場合は、ししてください、問題を示しているプロジェクトを zip 圧縮し、バグ レポートに追加します。  アタッチすることする前に、できるだけ単純なサンプル プロジェクトを作成します。<sup> [[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />ここで、バグが見つかりましたが、オペレーティング システムを含む環境について説明し、[のバージョンの Xamarin](~/cross-platform/troubleshooting/questions/version-logs.md)と任意の依存関係。

---

## <a name="additional-details"></a>追加の詳細

1. <a name="note-1" />[*^*](#ref-1) 理想的には「表示されている現象」の説明には情報が含まれます十分なその他の顧客が、同様の問題を表示するかどうかを確認できるように (同じエラー メッセージ、同じパフォーマンスの低下、同じのスタック トレース、クラッシュの_などです。_). 「正確な状況」は、1 つの好例なるかどうかのようなものを言うことができます:「は通常問題が発生した日時の 75% が、この 1 つの点を変更する場合は問題を回避できます完全に」。 「正確な状況」のような別の例は、Xamarin の以前のバージョンにダウン グレード問題を停止するかどうかです。

1. <a name="note-2" />[*^*](#ref-2) 予想できるようにエラー テキスト (またはその他の一意のわかりやすいテキスト) のスニペットは、通常、最適なキーワードです。 既存のバグのレポートが不完全な場合、詳細を追加または新しいファイルへようこそ が、バグ レポートが向上します。

1. <a name="note-3" />[*^*](#ref-3) 別の適切な質問は、java の場合、同じ問題が報告されたかどうか、OBJECTIVE-C または Swift アプリ。 場合は、し、問題は、Xamarin の一部ではなく、Android または iOS 自体の一部が非常に高くなります。

1. <a name="note-4" />[*^*](#ref-4) 含める情報の例をいくつか:

    1. プロジェクトを作成するときに発生したエラーくださいに含まれている完全な[診断ビルド出力](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output)バグ レポートにします。
    
    1. 構築または Visual Studio から iOS プロジェクトのデバッグ時に発生するエラーの場合を実行してください_ヘルプ > Xamarin > Zip ログ_後エラー発生とバグのレポートの結果として得られる .zip ファイルをインクルードします。
    
    1. 例外またはクラッシュ Android または iOS のアプリを記入して、関連する"[Xamarin.Android および Xamarin.iOS アプリのログをデバッグ](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps)"。

1. <a name="note-5" />[*^*](#ref-5) 可能であれば、特定の問題の 1 つの優れたオプションですをまったく新しいソリューションに、元のソリューションから少数のファイルを追加することで、問題を再作成します。 Xamarin チームはより大きなテスト_ケース (を再現する手順が明確に説明されている場合) は単純なテスト_ケース付与が最適な機会がバグを迅速に解決される場合でも問題を調査する多くの場合、できます。


1. <a name="note-6" />[*^*](#ref-6) 場合は_いない_可能であれば問題を再現するファイルの数が少ないをまったく新しいソリューションに追加することによって、ことができます zip 圧縮し、アプリが完全ソリューション全体のフォルダーをアタッチします。 削除してください、 `bin`、 `obj`、 `Components`、および`packages`郵便番号の小さいファイルを作成するフォルダーです。 (IDE およびビルド プロセスは通常の復元または必要に応じて、これらのフォルダーの内容を再作成してください)。結果として得られるソリューションでは、元の問題もを示している限りは、多くのコード ファイルとリソース ファイル プロジェクトから、好きなようにも削除できます。

