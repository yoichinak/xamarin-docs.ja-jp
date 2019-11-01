---
title: Xamarin. Mac での一般的なパターンと表現
description: このドキュメントでは、Xamarin. Mac アプリをビルドするときに使用する一般的なデザインパターンについて説明します。 モデルビューコントローラーパターン、データソースとデリゲートパターン、およびプロトコルについて説明します。
ms.prod: xamarin
ms.assetid: BF0A3517-17D8-453D-87F7-C8A34BEA8FF5
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 06/17/2016
ms.openlocfilehash: b508cc12f468e5b9dfef91718585f61bfd633816
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030059"
---
# <a name="common-patterns-and-idioms-in-xamarinmac"></a>Xamarin. Mac での一般的なパターンと表現

を通じて公開されC#ている Apple api 全体を通じて、特定の表現とパターンが繰り返し使用されます。 Xamarin. iOS を使用したプログラミングの経験がある場合は、これらの機能がよく見られます。 ドキュメントでは、これらのパターンや表現を繰り返し参照することがよくあるため、これらを十分に理解しておくと、見つけたドキュメントを理解するのに役立ちます。

## <a name="mvc---model-view-controller"></a>MVC モデルビューコントローラー

モデルビューコントローラー (short 用 MVC) は、Cocoa 全体に見られる非常に一般的なパターンです。 詳細については、このドキュメントでは説明しませんが、簡単に言えば、アプリケーションをコンポーネントに構造化する方法です。

- **モデル**オブジェクトは、表示および操作されている基になるデータ (アドレス帳のアドレスなど) を表します。
- **ビュー**オブジェクトは、画面上の特定のオブジェクトの描画を処理し、ユーザーの操作を処理します (画面上のアドレスを示すテキストフィールド)
- **コントローラー**オブジェクトは、モデルとビュー間の相互作用を処理します。 ユーザーが UI で変更を行ったときに、ビューを更新し、ビューから "ダウン" 変更をプッシュするために、モデルの変更を "上" にプッシュします。

WPF などの他のライブラリの MVVM (モデルビュービューモデル) に慣れている場合、コントローラーはビューモデルに似た動作をしますが、多くの場合、特定の UI 要素により密接にバインドされます。

詳細については、次を参照してください。

- [Apple の web サイトでの MVC の学習](https://developer.apple.com/library/ios/documentation/general/conceptual/devpedia-cocoacore/MVC.html)

- [目標のモデルビューコントローラー-C](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
- [Windows の操作](~/mac/user-interface/window.md)

## <a name="data-source--delegate--subclassing"></a>データソース/デリゲート/サブクラス化

Cocoa のもう1つの一般的なパターンでは、UI 要素にデータを提供し、ユーザーの操作に反応します。 例として `NSTableView` を使用する場合は、各行のデータ、その行を表す一連の UI 要素、ユーザー操作に対応する一連の動作、および場合によってはカスタマイズを提供する必要があります。 データソースとデリゲートパターンを使用すると、`NSTableView` を自分でサブクラス化することなく、ほとんどのケースを処理できます。

- `DataSource` プロパティには、`NSTableViewDataSource` のカスタムサブクラスのインスタンスが割り当てられます。このインスタンスは、(`GetRowCount` および `GetObjectValue`を使用して) テーブルにデータを設定するために呼び出されます。

- `Delegate` プロパティには、`NSTableViewDelegate` のカスタムサブクラスのインスタンスが割り当てられます。このインスタンスは `GetViewForItem`を介して特定のモデルオブジェクトのビューを提供し、`DidClickTableColumn`、`MouseDownInHeaderOfTableColumn`などを使用して、UI のやり取りを処理します。

場合によっては、デリゲートまたはデータソースで指定されたフックを超える方法でコントロールをカスタマイズし、ビューを直接サブクラス化することができます。 ただし、多くの場合、既定の動作を上書きすることで、すべての動作を自分で処理する必要があります (選択動作をカスタマイズすると、すべての選択動作を自分で実装することが必要になる場合があります)。

Xamarin. iOS では、`UITableView` などの一部の Api が、デリゲートとデータソース (`UITableViewSource`) の両方を実装するプロパティを使用して拡張されています。 これは、1つC#のクラスが1つの基底クラスしか持つことができないという一般的な制限を回避するためのものであり、プロトコルの提示は基本クラスを介して行われます。

Xamarin. Mac アプリケーションでテーブルビューを操作する方法の詳細については、[表ビュー](~/mac/user-interface/table-view.md)のドキュメントを参照してください。

## <a name="protocols"></a>プロトコル

目標 C のプロトコルは、のC#インターフェイスと比較することができ、多くの場合、同様の状況で使用されます。 たとえば、上記の `NSTableView` 例では、デリゲートとデータソースの両方が実際にプロトコルです。 これらは、オーバーライド可能な仮想メソッドを使用して基本クラスとして公開されます。 C#インターフェイスと目標 C プロトコルの主な違いは、プロトコルの一部のメソッドを実装することができないことです。 オプションを決定するには、API のドキュメントや定義を確認する必要があります。

詳細について[は、デリゲート、プロトコル、およびイベントに](~/ios/app-fundamentals/delegates-protocols-and-events.md)関するドキュメントを参照してください。

## <a name="related-links"></a>関連リンク

- [テーブルビュー](~/mac/user-interface/table-view.md)
- [Windows の操作](~/mac/user-interface/window.md)
- [デリゲート、プロトコル、およびイベント](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [モデルビューコントローラー](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
