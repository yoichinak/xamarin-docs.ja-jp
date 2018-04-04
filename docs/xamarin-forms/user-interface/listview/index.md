---
title: ListView
description: 美しい、対話型のリストで、データを表示します。
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2015
ms.openlocfilehash: a153791893f99a472c3fcf91a205bf91ed971e13
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="listview"></a>ListView

リスト ビューは、データ、特に長い一覧をスクロールを必要とするリストを表すためのビューです。 このガイドでは、リスト ビューを使用する方法を示します。

1. **[データ ソース](data-and-databinding.md)** &ndash;データ、データ バインディングの有無を ListView を設定します。
2. **[セルの外観](customizing-cell-appearance.md)** &ndash;組み込みのセルの外観をカスタマイズまたは独自のカスタムのセルを作成します。
3. **[外観を一覧表示](customizing-list-appearance.md)** &ndash; ListView の外観をカスタマイズします。 ヘッダーとフッターを追加、グループを有効にして行の高さを変更します。
4. **[対話機能](interactivity.md)** &ndash;タップし、選択内容を処理し、更新するプルを実装し、コンテキストのアクションを追加します。
5. **[パフォーマンス](performance.md)** &ndash;パフォーマンスの問題を回避します。

## <a name="use-cases"></a>ユース ケース
ListView がニーズに右側の制御を確認します。 どのような状況データのスクロール可能なリストを表示するのには、リスト ビューを使用できます。 Listview では、コンテキストのアクションおよびデータ バインディングをサポートします。

ListView 混同しないようにで[テーブル](~/xamarin-forms/user-interface/tableview.md)です。 テーブル コントロールは、オプションまたはデータの非バインドの一覧があるたびより優れたオプションです。 たとえば、ほとんどの場合に定義済みオプションのセットにある場合、iOS 設定アプリは、ListView よりテーブルを使用する適しています。

同種データも ListView は最適ですが適しています&ndash;は、すべてのデータは、同じ種類のする必要があります。 これは、リスト内の行ごとのセルの 1 つだけの種類を使用できるためです。 TableViews より良いオプションのビューを混在させる必要がある場合は、複数のセルの種類をサポートできます。


## <a name="components"></a>コンポーネント
ListView では、各プラットフォームのネイティブ機能を実行するために使用可能なコンポーネントの数があります。 これらの各コンポーネントについて、次に示します。

- **[ヘッダーとフッター](customizing-list-appearance.md#Headers_and_Footers)**  &ndash;の先頭と末尾の一覧に表示するには、テキストまたはビューがリストのデータから分離します。 ヘッダーとフッターできますバインドされていないデータ ソースに個別に ListView のデータ ソースからです。
- **[グループ](customizing-list-appearance.md#Grouping)** &ndash;簡単に移動、ListView でデータをグループ化することができます。 通常はグループのデータ バインドです。

![](images/grouping-depth.png "グループ化されたデータを含む ListView")

- **[セル](customizing-cell-appearance.md)** &ndash;セルで ListView でデータが表示されます。 各セルは、データの行に対応します。 選択する組み込みのセルがまたは独自のカスタムのセルを定義することができます。 組み込みとカスタムの両方のセルには、XAML またはコードで使用される定義を指定できます。
  - **[組み込み](customizing-cell-appearance.md#Built_in_Cells)** &ndash; TextCell と ImageCell、特に、セルに組み込まれているために、設定できます優れたパフォーマンスは、各プラットフォームでネイティブ コントロールに対応します。
    - **[TextCell](customizing-cell-appearance.md#TextCell)**  &ndash;詳細テキストを必要に応じて、テキストの文字列を表示します。 詳細テキストは強調色とフォント サイズを小さくの 2 番目の行として表示されます。
    - **[ImageCell](customizing-cell-appearance.md#ImageCell)**  &ndash;テキストとイメージを表示します。 左上のイメージに TextCell として表示されます。
  - **[カスタムのセル](customizing-cell-appearance.md#customcells)** &ndash;複雑なデータを表示する必要がある場合は、カスタムのセルはすばらしいです。 たとえば、曲、アルバム、アーティストなどの一覧を提供するカスタム ビューを使用できます。

![](images/image-cell-default.png "ImageCells を含む ListView")

ListView 内のセルのカスタマイズの詳細については、次を参照してください。 [ListView セルの外観のカスタマイズ](customizing-cell-appearance.md)です。

## <a name="functionality"></a>機能
リスト ビューには、相互作用のスタイル (など) の数がサポートされています。

- **[更新するプル](interactivity.md#Pull_to_Refresh)** &ndash; ListView は、各プラットフォームで更新するプルをサポートしています。
- **[コンテキストのアクション](interactivity.md#Context_Actions)** &ndash; ListView をリスト内の個々 の項目に行動をサポートしています。 たとえば、iOS にスワイプして、操作を実装したり時間の長い tap Android と Windows Phone のアクションをできます。
- **[選択範囲](interactivity.md#selectiontaps)** &ndash;選択内容であり、行がタップされたときにアクションを実行する deselections をリッスンすることができます。

![](images/context-default.png "コンテキストのアクションを含む ListView")

ListView の対話機能の詳細については、次を参照してください。 [ListView の対話機能 (&)、アクション](interactivity.md)です。


## <a name="related-links"></a>関連リンク

- [ListView での操作 (サンプル)](https://developer.xamarin.com/samples/WorkingWithListview)
- [双方向バインディング (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [セルでビルドされた (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/BuiltInCells)
- [カスタムのセル (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/CustomCells)
- [グループ化 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [カスタム レンダラー ビュー (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/WorkingWithListviewNative)
- [ListView の対話機能 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/interactivity)
- [iOS ブック](https://developer.xamarin.com/workbooks/xamarin-forms/user-interface/listview/ListView1-ios.workbook)
- [Android のブック](https://developer.xamarin.com/workbooks/xamarin-forms/user-interface/listview/ListView1-android.workbook)
