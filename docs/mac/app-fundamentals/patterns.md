---
title: 共通のパターンと成句 Xamarin.Mac で
description: このドキュメントでは、Xamarin.Mac アプリを構築するときに使用される共通のデザイン パターンについて説明します。 これは、モデル-ビュー-コント ローラー パターンでは、データ ソースとデリゲート パターン、およびプロトコルについて説明します。
ms.prod: xamarin
ms.assetid: BF0A3517-17D8-453D-87F7-C8A34BEA8FF5
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 06/17/2016
ms.openlocfilehash: b4266582ce0cec522e207cd06987f2d0eeff8b2d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61090889"
---
# <a name="common-patterns-and-idioms-in-xamarinmac"></a>共通のパターンと成句 Xamarin.Mac で

C# を使用して公開されている Apple Api、全体で特定の表現形式とパターンが何度も。 Xamarin.iOS を使用したプログラミングの経験がある場合は、これらが使い慣れた見えることがあります。 ドキュメントは多くの場合を参照してこれらのパターンと成句、繰り返しがそれらを確実に理解に役立つ検索するドキュメントを理解するため。

## <a name="mvc---model-view-controller"></a>MVC のモデル ビュー コント ローラー

モデル ビュー コント ローラー、または略してについては、MVC は、Cocoa 全体で非常に一般的なパターンです。 詳細についてがこのドキュメントの範囲を超えていますが、コンポーネントへのアプリケーションを構築する方法は簡単に説明します。

- **モデル**オブジェクトが表示および操作 (アドレス帳のアドレス) と同様に行われる基になるデータを表します
- **ビュー**オブジェクトを処理し、画面上の特定のオブジェクトの描画 (画面上のアドレスの表示テキスト フィールド) のユーザーとの対話を処理
- **コント ローラー**オブジェクトは、モデルとビューの間の対話を処理します。 モデルの変更をプッシュするビューを更新し、ユーザーが UI で変更したときに、ビューからのプッシュ「ダウン」変更「上」です。

MVVM (Model View ViewModel) で馴染み WPF などの他のライブラリの場合は、コント ローラー、ビューモデルのような機能しますが、特定の UI 要素をより厳密にバインドされた多くの場合。

詳細についてはここにあります。

- [Apple の web サイトで MVC の学習](https://developer.apple.com/library/ios/documentation/general/conceptual/devpedia-cocoacore/MVC.html)

- [Objective C でのモデル ビュー コント ローラー](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
- [Windows の操作](~/mac/user-interface/window.md)

## <a name="data-source--delegate--subclassing"></a>データ ソース委任/サブクラス化

Cocoa のもう 1 つの非常に一般的なパターンは、UI 要素にデータを提供して、ユーザーの操作への対応について説明します。 使用して`NSTableView`例として、何らかの理由で行ごとにデータを提供する必要がある、いくつか設定 UI 要素のいくつかの行を表す一連のユーザーの操作や可能性があるいくつかのかなりのカスタマイズに対応する動作。 サブクラスに頼ることがなく、ほとんどの場合を処理するためのデータ ソースとデリゲート パターン`NSTableView`自分でします。

- `DataSource`プロパティには、カスタムのサブクラスのインスタンスが割り当てられている`NSTableViewDataSource`テーブルにデータを設定すると呼ばれる (を使用して`GetRowCount`と`GetObjectValue`)。

- `Delegate`プロパティには、カスタムのサブクラスのインスタンスが割り当てられている`NSTableViewDelegate`特定のモデル オブジェクトのビューを提供する (を使用して`GetViewForItem`) し、UI 操作の処理 (を使用して`DidClickTableColumn`、`MouseDownInHeaderOfTableColumn`など)。

場合によっては、デリゲートまたはデータ ソースで指定されたフック以外の方法でコントロールをカスタマイズしたいし、直接サブクラス ビューことができます。 しかし注意場合既定値を上書きする多くの場合の動作し、必要はその動作のすべてを処理する (選択の動作をカスタマイズする必要がありますすべての選択動作を実装)。

Xamarin.iOS では、一部の Api でなど`UITableView`デリゲートと、データ ソースの両方を実装するプロパティが拡張され (`UITableViewSource`)。 一般的な制限を回避するには、この it を 1 つC#クラスは、1 つの基本クラスを持つことができますのみ、基本クラスを使用して、プロトコルの表示は行われます。

Xamarin.Mac アプリケーションでのテーブル ビューの使用の詳細についてを参照してください、[テーブル ビュー](~/mac/user-interface/table-view.md)ドキュメント。

## <a name="protocols"></a>プロトコル

Objective C でのプロトコルをインターフェイスに比較できるようにC#、多くの場合と同様の状況で使用されます。 たとえば、`NSTableView`上記の例、デリゲートと、データ ソースの両方が実際にはプロトコル。 Xamarin.Mac は、オーバーライド可能な仮想メソッドと基底クラスとしてこれらを公開します。 主な違いC#インターフェイスと OBJECTIVE-C プロトコル、プロトコルの一部のメソッドを実装するために省略可能な可能性があります。 ドキュメントおよび省略可能な新機能を決定するための API の定義を確認する必要があります。

詳細についてを参照してください、[デリゲート、プロトコル、およびイベント](~/ios/app-fundamentals/delegates-protocols-and-events.md)ドキュメント。



## <a name="related-links"></a>関連リンク

- [テーブル ビュー](~/mac/user-interface/table-view.md)
- [Windows の操作](~/mac/user-interface/window.md)
- [デリゲート、プロトコル、およびイベント](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Model-View-Controller](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
