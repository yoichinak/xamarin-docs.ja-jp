---
title: バインドのトラブルシューティング
description: このガイドでは、OBJECTIVE-C ライブラリのバインドできない場合の対処方法について説明します。 具体的には、不足しているバインディング、バインディング、およびバグ レポートに null を渡すとき、引数の例外について説明します。
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
author: asb3993
ms.author: amburns
ms.date: 10/19/2016
ms.openlocfilehash: aaceada84b151856506ede66907274e2457c23d4
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854806"
---
# <a name="binding-troubleshooting"></a>バインドのトラブルシューティング

MacOS (旧称 OS X) へのバインドのトラブルシューティングのヒントをいくつか Xamarin.Mac で Api。

## <a name="missing-bindings"></a>バインドがありません。

Xamarin.Mac は、Apple Api の多くをカバーしているときに必要がある場合、バインドがない一部の Apple API を呼び出すまだします。 それ以外の場合は、サード パーティ製 C/Objective C を呼び出す必要があります、Xamarin.Mac のバインドの対象外のことです。

Apple API を扱う場合、最初の手順は、Xamarin のことに達している、API のセクションをまだカバレッジがないことを把握できるようにします。 [バグを報告](#reporting-bugs)不足している API を注意してください。 お客様からのレポートを使用して次の作業 Api の優先順位を付けます。 さらに、Business または Enterprise ライセンスがあり、このバインディングが不足しているが、進行状況をブロックしている場合もある手順に従って[サポート](http://xamarin.com/support)チケットを提出します。 バインディング、約束できませんが場合によってはことができますを得する、作業します。

場合に通知する Xamarin (該当する場合)、不足しているバインディングのしたら次にはバインドことを検討してください。 完全なガイドがある[ここ](~/cross-platform/macios/binding/overview.md)と非公式のドキュメントはいくつか[ここ](http://brendanzagaeski.appspot.com/xamarin/0002.html)OBJECTIVE-C のバインドを手動でラップするためです。 C API を呼び出す場合は、# の P/invoke メカニズムを使用して、ドキュメントは[ここ](http://www.mono-project.com/docs/advanced/pinvoke/)します。

作業する場合、バインディング、自分でするバインディングの誤りがあらゆる種類のネイティブ ランタイムで興味深いクラッシュを生成できるに注意してください。 具体的には、c# では、シグネチャに引数の数と各引数のサイズでネイティブのシグネチャと一致することは十分に注意します。 これに失敗には、メモリまたはスタックが破損している可能性があり、直ちに、または任意の時点での今後のクラッシュまたはデータが破損する可能性があります。

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>バインドする場合は null を渡すときに、引数の例外

高を提供する Xamarin のしくみの中に品質と、Apple Api のバインドを十分にテストされたことがありますは誤りし、の明細をバグします。 最も一般的な問題に実行する可能性のある API は、スロー`ArgumentNullException`渡すと null を受け入れると、基になる API`nil`します。 多くの場合、API を定義するネイティブのヘッダー ファイルでは、API は nil をそのまま適用し、値を渡す場合がクラッシュするのに十分な情報は提供されません。

場合に発生した場合に渡すことが`null`スロー、`ArgumentNullException`動作に次の手順に従ってする必要がありますと考えられるが。

1. Apple のドキュメントやを受け取って実証見つかりますを表示する例を確認`nil`します。 Objective C に慣れている場合は、検証のための小規模なテスト プログラムを記述できます。
2. [バグを報告](#reporting-bugs)します。
3. バグを回避することができますか。 API を呼び出さない場合`null`呼び出しを囲む場合は、単純な null チェックが簡単に回避できます。
4. ただし、一部の Api では、オフにするか、一部の機能を無効にするには null を渡すことが必要です。 アセンブリ ブラウザーを導入することでこのような場合は、問題を回避することができます (を参照してください[c# メンバーの指定されたセレクターの検索](~/mac/app-fundamentals/mac-apis.md#finding_selector))、コピー、バインディング、および null チェックを削除します。 更新プログラムと修正を行うものでは、Xamarin.Mac でコピーしたバインドが受信されませんとしてこれを行うし、短期的な対応策を考慮するこの場合、バグを (ステップ 2) を報告してください。

<a name="reporting-bugs"/>

## <a name="reporting-bugs"></a>バグの報告

お客様のフィードバックは弊社にとって重要です。 Xamarin.Mac を使って問題を検出する: 場合

- [Xamarin.Mac フォーラム](https://forums.xamarin.com/categories/mac)を確認する
- [問題リポジトリ](https://github.com/xamarin/xamarin-macios/issues)を検索する 
- GitHub の問題に切り替わる前に、Xamarin の問題は [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi) で追跡されていました。 そこで一致する問題を検索してください。
- 一致する問題が見つからない場合は、[GitHub の問題リポジトリ](https://github.com/xamarin/xamarin-macios/issues/new)に新しい問題を提出してください。

GitHub の問題はすべて公開されています。 コメントまたは添付ファイルを非表示にすることはできません。 

次の情報について、できるだけ多くを含めてください。

- 問題を再現する簡単な例。 これは**重要**です (可能な場合)。 
- クラッシュの完全なスタック トレース。
- クラッシュの周囲の C# コード。 

## <a name="related-links"></a>関連リンク

- [Xamarin University のコース: OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University のコース: 目標油性、OBJECTIVE-C のバインド ライブラリをビルドします。](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
