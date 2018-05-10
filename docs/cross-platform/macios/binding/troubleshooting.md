---
title: バインドのトラブルシューティング
description: このガイドでは、バインド Objective C ライブラリが難しい場合の対処方法について説明します。
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
author: asb3993
ms.author: amburns
ms.date: 10/19/2016
ms.openlocfilehash: cb685bb60d49615c69925d17f69b0342d4f0a1a6
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="binding-troubleshooting"></a>バインドのトラブルシューティング

(旧称 OS X) macOS へのバインドのトラブルシューティングのヒントをいくつか Xamarin.Mac の Api です。

## <a name="missing-bindings"></a>バインドがありません。

Xamarin.Mac は、Apple の Api の多くをカバーしているときに必要がある場合、バインドがない一部の Apple API の呼び出しにまだです。 それ以外の場合に呼び出す必要がサード パーティ製 C/OBJECTIVE-C ある Xamarin.Mac バインディングのスコープ外になります。

Apple API を使用して処理する場合、最初の手順は、Xamarin があるヒット API のセクションでまだの範囲がないことを確認できるようにするは。 [バグを](#reporting-bugs)不足している API を注意してください。 お客様からのレポートを使用する Api を次の優先順位を設定します。 さらに、Business または Enterprise のライセンスがあり、このバインディングが不足しているが、進行状況をブロックしている場合もある手順に従って[サポート](http://xamarin.com/support)チケットをファイルにします。 バインディング、ことをお約束ことはできませんが、場合によってはを回避する、作業します。

Xamarin (該当する場合)、不足しているバインディングの通知した後、次の手順は、自分でバインディングを考慮する必要はします。 詳細なガイドがある[ここ](~/cross-platform/macios/binding/overview.md)といくつかの非公式なドキュメント[ここ](http://brendanzagaeski.appspot.com/xamarin/0002.html)Objective C のバインドを手動でラップするためです。 # の P/invoke 機構を使用すると、ドキュメントは C API を呼び出す場合[ここ](http://www.mono-project.com/docs/advanced/pinvoke/)です。

作業をする場合、バインディングで、自分でに注意してくださいのバインドに誤りがあらゆる種類のネイティブのランタイムでの興味深いクラッシュを生成できます。 具体的には、C# の場合、署名が引数の数と各引数のサイズでネイティブのシグネチャと一致することに注意します。 そのためにはエラーには、メモリおよびスタックが破損している可能性があり、直ちに、またはある任意の時点を今後クラッシュまたはデータが破損することができます。

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>バインディングに null を渡すときに、引数の例外

高いを提供する Xamarin の動作中に品質と Apple の api もテスト済みのバインド場合があります、誤りのおよびバグの明細です。 最もよくある問題が発生した可能性がありますですが、API スロー`ArgumentNullException`を渡す場合に null を受け入れると、基になる API`nil`です。 多くの場合、API を定義するネイティブのヘッダー ファイルは、API に nil 同意を渡した場合がクラッシュするのに十分な情報を提供しません。

場合に発生した場合に渡して、`null`をスロー、`ArgumentNullException`と考えられる作業、次の手順に従って、必要がありますが。

1. Apple のドキュメントと例をどうかを受け取ることの証明を参照してください。 確認`nil`です。 Objective C に慣れている場合は、することを確認する小規模なテスト プログラムを作成することができます。
2. [バグを](#reporting-bugs)です。
3. バグを回避することができますか。 API を呼び出すことは避けることができる場合`null`呼び出しの周囲の簡単な null チェックが簡単な解決法を指定できます。
4. ただし、一部の Api では、オフにするか、一部の機能を無効にするのには null を渡すことが必要です。 アセンブリのブラウザーを戻すことによってこのような場合は、問題を回避することができます (を参照してください[指定されたセレクターの c# メンバーを検索する](~/mac/app-fundamentals/mac-apis.md#finding_selector))、コピー、バインディング、および null チェックを削除します。 バグを登録する (手順 2.) 更新プログラムと修正を行って Xamarin.Mac、コピーされたバインディングは取得されません、これを行うし、この見なす必要がある短期的な回避策を確認してください。

<a name="reporting-bugs"/>

## <a name="reporting-bugs"></a>バグの報告

お客様のフィードバックは弊社にとって重要です。 : Xamarin.Mac で問題が見つかった場合

- [Xamarin.Mac フォーラム](https://forums.xamarin.com/categories/mac)を確認する
- [問題リポジトリ](https://github.com/xamarin/xamarin-macios/issues)を検索する 
- GitHub の問題に切り替わる前に、Xamarin の問題は [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi) で追跡されていました。 そこで一致する問題を検索してください。
- 一致する問題が見つからない場合は、[GitHub の問題リポジトリ](https://github.com/xamarin/xamarin-macios/issues/new)に新しい問題を提出してください。

GitHub の問題はすべて公開されています。 コメントまたは添付ファイルを非表示にすることはできません。 

次の情報について、できるだけ多くを含めてください。

- 問題を再現する簡単な例。 これは**重要**です (可能な場合)。 
- クラッシュの完全なスタック トレース。
- クラッシュの周囲の C# コード。 
