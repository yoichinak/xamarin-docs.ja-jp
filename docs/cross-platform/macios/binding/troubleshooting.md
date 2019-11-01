---
title: バインディングのトラブルシューティング
description: このガイドでは、目的 C ライブラリをバインドすることが困難な場合の対処方法について説明します。 特に、欠落しているバインド、バインドに null を渡す場合の引数の例外、およびバグの報告について説明します。
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
author: davidortinau
ms.author: daortin
ms.date: 10/19/2016
ms.openlocfilehash: fe4b9ade9e6e462c3472a8bb3bb8750ed6cac326
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73015892"
---
# <a name="binding-troubleshooting"></a>バインディングのトラブルシューティング

Xamarin. Mac で macOS (旧称 OS X) Api へのバインドのトラブルシューティングを行うためのヒントをいくつか紹介します。

## <a name="missing-bindings"></a>バインドがありません

Xamarin. Mac では Apple Api の多くがカバーされていますが、バインドがまだない Apple API を呼び出す必要がある場合があります。 それ以外の場合は、Xamarin. Mac バインドの範囲外であるサードパーティの C/目標 c を呼び出す必要があります。

Apple API を使用している場合、最初の手順は、まだカバーしていない API のセクションにヒットしたことを Xamarin に知らせることです。 見つからない API に関する[バグをファイルに](#reporting-bugs)記録します。 お客様からのレポートを使用して、次に作業する Api に優先順位を付けます。 また、ビジネスまたはエンタープライズライセンスを持っていて、バインドの不足によって進行状況がブロックされている場合は、[サポート](https://visualstudio.microsoft.com/vs/support/)の指示に従ってチケットを作成してください。 バインドについて約束することはできませんが、場合によっては回避策が考えられます。

不足しているバインドの Xamarin (該当する場合) に通知したら、次の手順として、自分でバインドすることを検討します。 ここでは、目的の C のバインドを手動でラップするための完全[なガイドと](~/cross-platform/macios/binding/overview.md)非公式[のドキュメントを](https://brendanzagaeski.appspot.com/xamarin/0002.html)紹介します。 C API を呼び出す場合は、P/Invoke C#機構を使用できます。ドキュメントは[ここ](https://www.mono-project.com/docs/advanced/pinvoke/)にあります。

自分でバインドを使用する場合は、バインディングの誤りによって、ネイティブランタイムであらゆる種類の興味深いクラッシュが生成される可能性があることに注意してください。 特に、のC#シグネチャが、引数の数と各引数のサイズでのネイティブシグネチャと一致することに注意してください。 そうしないと、メモリやスタックが破損し、直ちに、または将来の任意の時点でクラッシュし、データが破損する可能性があります。

## <a name="argument-exceptions-when-passing-null-to-a-binding"></a>Null をバインドに渡すときの引数の例外

Xamarin は Apple Api の高品質で十分にテストされたバインディングを提供しますが、誤りやバグが発生することもあります。 これまでに発生する可能性がある最も一般的な問題は、基になる API が `nil`を受け入れるときに null を渡すと、API が `ArgumentNullException` をスローすることです。 多くの場合、API を定義するネイティブヘッダーファイルには、どの Api が nil を受け入れ、それを渡した場合にクラッシュするかについての十分な情報が提供されていません。

`null` を渡すときに `ArgumentNullException` がスローされるが、動作すると思われる場合は、次の手順を実行します。

1. Apple のドキュメントや例を参照して、`nil`が受け入れていることがわかっているかどうかを確認してください。 目標 C を使い慣れている場合は、小さなテストプログラムを作成して検証することができます。
2. [バグを報告](#reporting-bugs)します。
3. バグを回避することはできますか。 `null`を使用した API の呼び出しを回避できる場合は、簡単な呼び出しを回避することができます。
4. ただし、一部の Api では、一部の機能を無効にしたり無効にしたりするために、null を渡す必要があります。 このような場合は、アセンブリブラウザーを起動し ([特定のセレクター C#のメンバーを検索](~/mac/app-fundamentals/mac-apis.md#finding_selector)する方法を参照)、バインドをコピーし、null チェックを削除することで、この問題を回避できます。 コピーしたバインドは、Xamarin. Mac で作成した更新プログラムや修正プログラムを受信しないため、これを回避するには、バグを必ずファイルに登録してください (ステップ 2)。

<a name="reporting-bugs"/>

## <a name="reporting-bugs"></a>バグの報告

フィードバックは microsoft にとって重要です。 Xamarin. Mac で問題が見つかった場合は、次のようにします。

- [Xamarin.Mac フォーラム](https://forums.xamarin.com/categories/mac)を確認する
- [問題リポジトリ](https://github.com/xamarin/xamarin-macios/issues)を検索する 
- GitHub の問題に切り替わる前に、Xamarin の問題は [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi) で追跡されていました。 そこで一致する問題を検索してください。
- 一致する問題が見つからない場合は、[GitHub の問題リポジトリ](https://github.com/xamarin/xamarin-macios/issues/new)に新しい問題を提出してください。

GitHub の問題はすべて公開されています。 コメントまたは添付ファイルを非表示にすることはできません。 

次の情報について、できるだけ多くを含めてください。

- 問題を再現する簡単な例。 これは**重要**です (可能な場合)。 
- クラッシュの完全なスタック トレース。
- クラッシュの周囲の C# コード。
