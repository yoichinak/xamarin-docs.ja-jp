---
title: "共通のパターンおよび手法"
description: "このドキュメントでは、モデル ビュー コント ローラー パターンでは、データ ソースとデリゲート パターン、およびプロトコルについて説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: BF0A3517-17D8-453D-87F7-C8A34BEA8FF5
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/17/2016
ms.openlocfilehash: 4926670a0cd0960c9a390bd388d3530929634153
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="common-patterns-and-idioms"></a>共通のパターンおよび手法

C# を使用して公開されている Apple Api、全体で特定の表現形式とパターンつなげて何度もします。 Xamarin.iOS を使用したプログラミングの経験があれば、これらはおなじみです。 ドキュメントは多くの場合を参照してこれらのパターンと表現方法、繰り返しため、それらの実線に理解することはできますが見つかったら、ドキュメントの意味をなします。

## <a name="mvc---model-view-controller"></a>MVC のモデル ビュー コント ローラー

モデル ビュー コント ローラー、または略して、MVC、Cocoa 全体で非常に一般的なパターンです。 詳細については、このドキュメントの範囲を超えては簡単に説明は、アプリケーションのコンポーネントを構成する方法。

- **モデル**オブジェクトが表示され (アドレス帳のアドレス) のように操作される基になるデータを表します
- **ビュー**オブジェクト処理画面上の指定したオブジェクトの描画、ユーザーとの対話 (画面に、アドレスを示すテキスト フィールド) の処理
- **コント ローラー**オブジェクトは、モデルとビューの間の対話を処理します。 モデルの変更をプッシュするビューを更新し、ユーザーが UI に変更したときに、ビューからのプッシュ「ダウン」変更「最大」です。

慣れて MVVM (モデル View-viewmodel) WPF などの他のライブラリから、コント ローラーは、ViewModel のような機能しますは多くの場合より密接にバインドされている特定の UI 要素。

詳細についてはここにあります。

- [Apple の web サイトでの MVC の学習](https://developer.apple.com/library/ios/documentation/general/conceptual/devpedia-cocoacore/MVC.html)

- [Objective C のモデル ビュー コント ローラー](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
- [ウィンドウの操作](~/mac/user-interface/window.md)

## <a name="data-source--delegate--subclassing"></a>データ ソースに委任//サブクラス化

Cocoa におけるもう 1 つの非常に一般的なパターンは、UI 要素にデータを提供して、ユーザーとのやり取りに反応するについて説明します。 使用して`NSTableView`例として、何らかの理由で行ごとにデータを提供する必要がある、いくつか設定の UI 要素の一部の行を表す一連のユーザーとのやり取り、および可能性のある程度のカスタマイズに対応するために動作します。 データ ソースとデリゲート パターンでは、サブクラスに頼ることがなくほとんどの場合を処理できます。`NSTableView`自分でします。

- `DataSource`プロパティには、カスタムのサブクラスのインスタンスが割り当てられている`NSTableViewDataSource`テーブルにデータを設定すると呼ばれる (を介して`GetRowCount`と`GetObjectValue`)。

- `Delegate`プロパティには、カスタムのサブクラスのインスタンスが割り当てられている`NSTableViewDelegate`特定のモデル オブジェクトのビューを提供する (を介して`GetViewForItem`) し、UI の相互作用を処理 (経由で`DidClickTableColumn`、`MouseDownInHeaderOfTableColumn`など)。

場合によっては、デリゲート、またはデータ ソースで指定されたフック以外の方法でコントロールのカスタマイズにしておくし、直接サブクラス ビューことができます。 ただしように注意する必要を既定をオーバーライドする多くの場合の動作はし、必要とするすべての動作を処理する (選択の動作をカスタマイズする必要がありますすべての選択動作を実装)。

Xamarin.iOS、一部の Api でなど`UITableView`デリゲートと、データ ソースの両方を実装するプロパティによって拡張されています (`UITableViewSource`)。 1 つの c# クラスには、1 つの基本クラス、およびマイクロソフトのプロトコルの表示のみを持つことが一般的な制限を回避するには基本クラスを使用しています。

表形式ビュー Xamarin.Mac アプリケーションでの操作の詳細についてを参照してください、[テーブル ビュー](~/mac/user-interface/table-view.md)ドキュメント。

## <a name="protocols"></a>プロトコル

Objective c プロトコルは、C# の場合は、インターフェイスと比較することし、多くの場合は、同じような状況で使用されます。 たとえば、`NSTableView`上記の例、デリゲートと、データ ソースの両方が実際にプロトコルです。 Xamarin.Mac は、仮想メソッドをオーバーライドすることができますを基底クラスとしてこれらを公開します。 C# のインターフェイスと OBJECTIVE-C プロトコルの主な違いは、プロトコルの一部のメソッドを実装する省略可能な可能性があります。 ドキュメントおよび省略可能なを判別するために API の定義を確認する必要があります。

詳細についてを参照してください、[デリゲート、プロトコル、およびイベント](~/ios/app-fundamentals/delegates-protocols-and-events.md)ドキュメント。



## <a name="related-links"></a>関連リンク

- [テーブルのビュー](~/mac/user-interface/table-view.md)
- [ウィンドウの操作](~/mac/user-interface/window.md)
- [デリゲート、プロトコル、およびイベント](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Model-View-Controller](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
