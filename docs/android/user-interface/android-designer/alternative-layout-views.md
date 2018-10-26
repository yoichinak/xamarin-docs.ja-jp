---
title: 代替レイアウト ビュー
description: このトピックでは、どのようにレイアウトをバージョン管理するリソース修飾子を使用して、について説明します。 たとえばがありますが、デバイスを横モードでのみ使用されるレイアウトのバージョンとは縦モードだけなレイアウト バージョン。
ms.prod: xamarin
ms.assetid: 5EBF51FC-9048-F0CF-624A-D8782A91C1FD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
ms.openlocfilehash: 03b80d3fb1ed7c8db108f86b3b3923c20e1d908f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109979"
---
# <a name="alternative-layout-views"></a>代替レイアウト ビュー

_このトピックでは、リソース修飾子を使用して、バージョンのレイアウトを行う方法について説明します。たとえば、バージョンの場合、デバイスを横モードでのみ使用されるレイアウトと縦向きモードに対してのみであるレイアウト バージョンを作成します。_

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="creating-alternative-layouts"></a>別のレイアウトを作成します。

クリックすると、**代替レイアウト ビュー**アイコン (の左側に**デバイス**)、プロジェクトで使用可能な別のレイアウトを一覧表示するプレビュー ウィンドウを開きます。 別のレイアウトがない場合、**既定**ビューが表示されます。 

[![代替レイアウト ビュー ペイン](alternative-layout-views-images/vs/01-alt-layout-view-pane-sml.png "代替レイアウト ビュー ペイン")](alternative-layout-views-images/vs/01-alt-layout-view-pane.png#lightbox)

横に緑色のプラス記号をクリックすると**新しいバージョン**、**作成レイアウトのバリエーション**ダイアログが開き、このレイアウトのバリエーションのリソース修飾子を選択することができます。 

[![レイアウトのバリエーションを作成する](alternative-layout-views-images/vs/02-create-layout-variation-sml.png "レイアウトのバリエーションの作成")](alternative-layout-views-images/vs/02-create-layout-variation.png#lightbox)

次の例のリソース修飾子で**画面の向き**に設定されている**ランドス ケープ**、および**画面サイズ**に変更が**Large**. という名前のレイアウトの新しいバージョンを作成しますこの**大きな land**:

[![Large land バリエーション](alternative-layout-views-images/vs/03-large-land-sml.png "Large land のバリエーション")](alternative-layout-views-images/vs/03-large-land.png#lightbox)

左側のプレビュー ウィンドウがリソース修飾子の選択項目の効果が表示されるに注意してください。 クリックすると**追加**代替レイアウトを作成し、そのレイアウトにデザイナーを切り替えます。 **代替レイアウト ビュー**プレビュー ウィンドウは、次のスクリーン ショットに記載されているレイアウトが小規模の右のポインターを使用してデザイナーに読み込まれることを示します。 

[![読み込まれたレイアウト インジケーター](alternative-layout-views-images/vs/04-new-layout-sml.png "読み込まれたレイアウトのインジケーター")](alternative-layout-views-images/vs/04-new-layout.png#lightbox)


## <a name="editing-alternative-layouts"></a>別のレイアウトの編集

別のレイアウトを作成するときに、レイアウトのすべての分岐したバージョンに適用される 1 つの変更を加えることが望ましい場合が多くの場合。 たとえば、ボタンのテキストをすべてのレイアウトに黄色に変更する可能性があります。 多数のレイアウトがあり、すべてのバージョンに 1 つの変更を反映する必要がある場合手間がかかり、エラーが発生しやすいメンテナンスはすぐに。

デザイナーでは、複数のレイアウトのバージョンのメンテナンスを簡素化する、**マルチ編集**1 つ以上のレイアウトの間で、変更を反映するモード。 1 つ以上のレイアウトが存在するときに、**マルチ編集**アイコンが表示されます。 

[![複数の編集アイコン](alternative-layout-views-images/vs/05-multi-layout-icon-sml.png "マルチの編集 アイコン")](alternative-layout-views-images/vs/05-multi-layout-icon.png#lightbox)

クリックすると、**マルチ編集**アイコン、行が表示されます (下図参照) のように、レイアウトがリンクされているかを示す; は、1 つのレイアウトを変更すると、その変更が反映されます、リンクされたレイアウトにします。 すべてのレイアウトのリンクを解除するには、次のスクリーン ショットに示されている丸のアイコンをクリックします。 

[![すべてのレイアウトをリンク解除](alternative-layout-views-images/vs/06-multi-linked-sml.png "すべてのレイアウトをリンク解除")](alternative-layout-views-images/vs/06-multi-linked.png#lightbox)

複数の 2 つのレイアウトがあれば、レイアウトが一緒にリンクを確認するには、各レイアウト プレビューの左側にある編集ボタンを選択的に切り替えることができます。 たとえば、1 つの変更を反映するようにする場合、最初と最後の 3 つのレイアウトには最初リンクを解除する中間のレイアウトは、ここに示すようにします。 

[![中央のレイアウトをリンク解除](alternative-layout-views-images/vs/07-unlink-middle-layout-sml.png "中間のレイアウトをリンク解除")](alternative-layout-views-images/vs/07-unlink-middle-layout.png#lightbox)

この例では、変更が加えいずれかに、**既定**または**長い**を他のレイアウトには、レイアウトが反映される、**大きな land**レイアウト。

### <a name="multi-edit-example"></a>複数の編集の例 

一般に、1 つのレイアウトを変更すると、その同じ変更が他のすべてのリンクされたレイアウトに反映されます。 新しいの追加など、`TextView`ウィジェットを**既定**レイアウトとテキストを変更する文字列を`Portrait`にリンクされたすべてのレイアウトになる同じ変更が発生します。 表示方法を次に示します、**既定**レイアウト。 

[![TextView を追加](alternative-layout-views-images/vs/08-add-textview-sml.png "TextView の追加")](alternative-layout-views-images/vs/08-add-textview.png#lightbox)
 
`TextView`にも追加、**大きな land**レイアウト表示にリンクされているので、**既定**レイアウト。 

[![TextView を横](alternative-layout-views-images/vs/09-landscape-textview-sml.png "TextView を横")](alternative-layout-views-images/vs/09-landscape-textview.png#lightbox)
 
ローカル レイアウトを 1 つだけになっている変更を加える場合は (つまり、その他のレイアウトのいずれかに反映される変更をしたくない) か? これには、次に説明したように変更する前に変更するレイアウトのリンクを解除する必要があります。 

### <a name="making-local-changes"></a>ローカルの変更を加える 

両方のレイアウトが、追加したとします`TextView`、テキスト文字列を変更することも必要しますが、**大きな land**レイアウトを`Landscape`なく`Portrait`します。 この変更を行った場合**大きな land**に変更を反映は両方のレイアウトをリンクすると中、**既定**レイアウト。 そのため、する必要があります最初のリンクを解除、2 つのレイアウト変更を行った前にします。 内のテキストを変更すると**大きな land**に`Landscape`、デザイナーは、変更がに対してローカルであることを示す赤いフレームを使用してこの変更をマーク、**大きな land**レイアウトであり、*いない*に反映、**既定**レイアウト。 

[![ローカルの変更が](alternative-layout-views-images/vs/10-local-change-sml.png "ローカルの変更")](alternative-layout-views-images/vs/10-local-change.png#lightbox)
 
クリックすると、**既定**、閲覧レイアウト、`TextView`テキスト文字列が設定`Portrait`します。 

## <a name="handling-conflicts"></a>競合の処理 

内のテキストの色を変更する場合、**既定**緑にレイアウトをリンクのレイアウトに表示される警告アイコンが表示されます。 そのレイアウト をクリックすると、競合を表示するレイアウトが開きます。 赤のフレームを使用して、競合の原因となったウィジェットが強調表示され、次のメッセージが表示されます:*最近の変更がこの代替レイアウトで競合の原因となった*します。 

[![競合する変更](alternative-layout-views-images/vs/11-conflicting-change-sml.png "競合する変更")](alternative-layout-views-images/vs/11-conflicting-change.png#lightbox)
 
A*競合ボックス*が競合を説明する、ウィジェットの右側に表示されます。 

[![競合の警告](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

競合のボックスには、変更されたプロパティの一覧が表示され、その値が一覧表示されます。 クリックすると**無視競合**プロパティの変更をこのウィジェットにのみ適用されます。 クリックすると**適用**に関して、リンクされた、対応するウィジェットでもこのウィジェットにプロパティの変更を適用**既定**レイアウト。 すべてのプロパティ変更が適用されている場合、競合は自動的に破棄されます。 

### <a name="view-group-conflicts"></a>グループの競合の表示 

プロパティの変更は、競合の唯一のソースではありません。 挿入またはウィジェットを削除するときに、競合を検出できます。 たとえばときに、**大きな land**からレイアウトのリンクを解除、**既定**レイアウトと`TextView`で、 **land 大きな**レイアウトがドラッグ アンド上ドロップ、`Button`デザイナーは、競合を示すために移動したウィジェットをマークします。

[![グループの競合を表示](alternative-layout-views-images/vs/12-view-group-conflict-sml.png "グループの競合の表示")](alternative-layout-views-images/vs/12-view-group-conflict.png#lightbox)
 
ただしにはありませんマーカー、`Button`します。 位置、`Button`が変更された、`Button`に固有の適用の変更が表示されない、**大きな land**構成します。 

場合、`CheckBox`に追加されます、**既定**レイアウト、もう 1 つの競合が発生すると、および警告のアイコン上に表示される、**大きな land**レイアウト。 

[![競合のチェック ボックスをオン](alternative-layout-views-images/vs/13-checkbox-conflict-sml.png "競合のチェック ボックス")](alternative-layout-views-images/vs/13-checkbox-conflict.png#lightbox)
 
クリックすると、**大きな land**レイアウトが、競合が表示されます。 次のメッセージが表示されます:*最近の変更がこの代替レイアウトで競合の原因となった*: 

[![Alt レイアウト競合](alternative-layout-views-images/vs/14-alt-layout-conflict-sml.png "Alt レイアウトの競合")](alternative-layout-views-images/vs/14-alt-layout-conflict.png#lightbox)

さらに、競合のボックスには、次のメッセージが表示されます。

[![競合のメッセージ](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

追加、`CheckBox`ため、競合が発生、**大きな land**レイアウト変更には、`LinearLayout`含まれています。 ただし、この場合、競合ボックスに表示ウィジェットに挿入した、**既定**レイアウト (、 `CheckBox`)。

をクリックすると**競合を無視する**、デザイナーは、ウィジェットが欠落しているドラッグ アンド ドロップをレイアウトする競合ボックスに表示されるウィジェットを許可という、競合を解決 (ここで、**大きな land**レイアウト)。 

[![グループの競合を解決](alternative-layout-views-images/vs/15-resolved-group-conflict-sml.png "グループの競合の解決")](alternative-layout-views-images/vs/15-resolved-group-conflict.png#lightbox)

前の例に示すよう、 `Button`、`CheckBox`ため、変更の赤いマーカーを持たないのみ、`LinearLayout`で適用された変更が、**大きな land**レイアウト。



# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="creating-alternative-layouts"></a>別のレイアウトを作成します。

クリックすると、**代替レイアウト ビュー**アイコン (の左側に**デバイス**)、プロジェクトで使用可能な別のレイアウトを一覧表示するプレビュー ウィンドウを開きます。 別のレイアウトがない場合、**既定**ビューが表示されます。 

[![代替レイアウト ビュー ペイン](alternative-layout-views-images/xs/01-alt-layout-view-pane-sml.png)](alternative-layout-views-images/xs/01-alt-layout-view-pane.png#lightbox)

横に緑色のプラス記号をクリックすると**新しいバージョン**、**作成レイアウトのバリエーション**ダイアログが開き、このレイアウトのバリエーションのリソース修飾子を選択することができます。 

[![レイアウトのバリエーションを作成します。](alternative-layout-views-images/xs/02-create-layout-variation-sml.png)](alternative-layout-views-images/xs/02-create-layout-variation.png#lightbox)

次の例のリソース修飾子で**画面の向き**に設定されている**ランドス ケープ**、および**画面サイズ**に変更が**Large**. という名前のレイアウトの新しいバージョンを作成しますこの**大きな land**:

[![Large land のバリエーション](alternative-layout-views-images/xs/03-large-land-sml.png)](alternative-layout-views-images/xs/03-large-land.png#lightbox)

左側のプレビュー ウィンドウがリソース修飾子の選択項目の効果が表示されるに注意してください。 クリックすると**追加**代替レイアウトを作成し、そのレイアウトにデザイナーを切り替えます。 **代替レイアウト ビュー**プレビュー ウィンドウは、次のスクリーン ショットに記載されているレイアウトが小規模の右のポインターを使用してデザイナーに読み込まれることを示します。 

[![読み込まれたレイアウトのインジケーター](alternative-layout-views-images/xs/04-new-layout-sml.png)](alternative-layout-views-images/xs/04-new-layout.png#lightbox)

## <a name="editing-alternative-layouts"></a>別のレイアウトの編集

別のレイアウトを作成するときに、レイアウトのすべての分岐したバージョンに適用される 1 つの変更を加えることが望ましい場合が多くの場合。 たとえば、ボタンのテキストをすべてのレイアウトに黄色に変更する可能性があります。 多数のレイアウトがあり、すべてのバージョンに 1 つの変更を反映する必要がある場合手間がかかり、エラーが発生しやすいメンテナンスはすぐに。

デザイナーでは、複数のレイアウトのバージョンのメンテナンスを簡素化する、**マルチ編集**1 つ以上のレイアウトの間で、変更を反映するモード。 1 つ以上のレイアウトが存在するときに、**マルチ編集**アイコンが表示されます。 

[![複数の編集 アイコン](alternative-layout-views-images/xs/05-multi-layout-icon-sml.png)](alternative-layout-views-images/xs/05-multi-layout-icon.png#lightbox)

クリックすると、**マルチ編集**アイコン、行が表示されます (下図参照) のように、レイアウトがリンクされているかを示す; は、1 つのレイアウトを変更すると、その変更が反映されます、リンクされたレイアウトにします。 すべてのレイアウトのリンクを解除するには、次のスクリーン ショットに示されている丸のアイコンをクリックします。 

[![すべてのレイアウトをリンク解除します。](alternative-layout-views-images/xs/06a-linked-sml.png)](alternative-layout-views-images/xs/06a-linked.png#lightbox)

複数の 2 つのレイアウトがあれば、レイアウトが一緒にリンクを確認するには、各レイアウト プレビューの左側にある編集ボタンを選択的に切り替えることができます。 たとえば、1 つの変更を反映するようにする場合、最初と最後の 3 つのレイアウトには最初リンクを解除する中間のレイアウトは、ここに示すようにします。 

[![中央のレイアウトをリンク解除します。](alternative-layout-views-images/xs/06b-multi-linked-sml.png)](alternative-layout-views-images/xs/06b-multi-linked.png#lightbox)

この例では、変更が加えいずれかに、**既定**または**長い**を他のレイアウトには、レイアウトが反映される、**大きな land**レイアウト。 

### <a name="multi-edit-example"></a>複数の編集の例 

一般に、1 つのレイアウトを変更すると、その同じ変更が他のすべてのリンクされたレイアウトに反映されます。 新しいの追加など、`TextView`ウィジェットを**既定**レイアウトとテキストを変更する文字列を`Portrait`にリンクされたすべてのレイアウトになる同じ変更が発生します。 表示方法を次に示します、**既定**レイアウト。 

[![TextView を追加します。](alternative-layout-views-images/xs/07-add-textview-sml.png)](alternative-layout-views-images/xs/07-add-textview.png#lightbox)

`TextView`にも追加、**大きな land**レイアウト表示にリンクされているので、**既定**レイアウト。 

[![横の [textview]](alternative-layout-views-images/xs/08-landscape-textview-sml.png)](alternative-layout-views-images/xs/08-landscape-textview.png#lightbox)
 
ローカル レイアウトを 1 つだけになっている変更を加える場合は (つまり、その他のレイアウトのいずれかに反映される変更をしたくない) か? これには、次に説明したように変更する前に変更するレイアウトのリンクを解除する必要があります。 

### <a name="making-local-changes"></a>ローカルの変更を加える 

両方のレイアウトが、追加したとします`TextView`、テキスト文字列を変更することも必要しますが、**大きな land**レイアウトを`Landscape`なく`Portrait`します。 この変更を行った場合**大きな land**に変更を反映は両方のレイアウトをリンクすると中、**既定**レイアウト。 そのため、する必要があります最初のリンクを解除、2 つのレイアウト変更を行った前にします。 内のテキストを変更すると**大きな land**に`Landscape`、デザイナーは、変更がに対してローカルであることを示す赤いフレームを使用してこの変更をマーク、**大きな land**レイアウトであり、*いない*に反映、**既定**レイアウト。 

[![ローカルの変更](alternative-layout-views-images/xs/09-local-change-sml.png)](alternative-layout-views-images/xs/09-local-change.png#lightbox)

クリックすると、**既定**、閲覧レイアウト、`TextView`テキスト文字列が設定`Portrait`します。 

## <a name="handling-conflicts"></a>競合の処理 

内のテキストの色を変更する場合、**既定**緑にレイアウトをリンクのレイアウトに表示される警告アイコンが表示されます。 そのレイアウト をクリックすると、競合を表示するレイアウトが開きます。 赤のフレームを使用して、競合の原因となったウィジェットが強調表示され、次のメッセージが表示されます:*最近の変更がこの代替レイアウトで競合の原因となった*します。 

[![変更が競合しています。](alternative-layout-views-images/xs/10-conflict-sml.png)](alternative-layout-views-images/xs/10-conflict.png#lightbox)

A*競合ボックス*が競合を説明する、ウィジェットの右側に表示されます。 

[![競合の警告](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

競合のボックスには、変更されたプロパティの一覧が表示され、その値が一覧表示されます。 クリックすると**無視競合**プロパティの変更をこのウィジェットにのみ適用されます。 クリックすると**適用**に関して、リンクされた、対応するウィジェットでもこのウィジェットにプロパティの変更を適用**既定**レイアウト。 すべてのプロパティ変更が適用されている場合、競合は自動的に破棄されます。 

### <a name="view-group-conflicts"></a>グループの競合の表示 

プロパティの変更は、競合の唯一のソースではありません。 挿入またはウィジェットを削除するときに、競合を検出できます。 たとえばときに、**大きな land**からレイアウトのリンクを解除、**既定**レイアウトと`TextView`で、 **land 大きな**レイアウトがドラッグ アンド上ドロップ、`Button`デザイナーは、競合を示すために移動したウィジェットをマークします。

[![ビューのグループの競合](alternative-layout-views-images/xs/12-view-group-conflict-sml.png)](alternative-layout-views-images/xs/12-view-group-conflict.png#lightbox)
 
ただしにはありませんマーカー、`Button`します。 位置、`Button`が変更された、`Button`に固有の適用の変更が表示されない、**大きな land**構成します。 

場合、`CheckBox`に追加されます、**既定**レイアウト、もう 1 つの競合が発生すると、および警告のアイコン上に表示される、**大きな land**レイアウト。 

[![競合のチェック ボックス](alternative-layout-views-images/xs/13-checkbox-conflict-sml.png)](alternative-layout-views-images/xs/13-checkbox-conflict.png#lightbox)
 
クリックすると、**大きな land**レイアウトが、競合が表示されます。 次のメッセージが表示されます:*最近の変更がこの代替レイアウトで競合の原因となった*します。 

[![Alt レイアウトの競合](alternative-layout-views-images/xs/14-alt-layout-conflict-sml.png)](alternative-layout-views-images/xs/14-alt-layout-conflict.png#lightbox)
 
さらに、競合のボックスには、次のメッセージが表示されます。

[![競合のメッセージ](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

追加、`CheckBox`ため、競合が発生、**大きな land**レイアウト変更には、`LinearLayout`含まれています。 ただし、この場合、競合ボックスに表示ウィジェットに挿入した、**既定**レイアウト (、 `CheckBox`)。

をクリックすると**競合を無視する**、デザイナーは、ウィジェットが欠落しているドラッグ アンド ドロップをレイアウトする競合ボックスに表示されるウィジェットを許可という、競合を解決 (ここで、**大きな land**レイアウト)。 

[![グループの競合の解決](alternative-layout-views-images/xs/16-resolved-group-conflict-sml.png)](alternative-layout-views-images/xs/16-resolved-group-conflict.png#lightbox)
 
前の例に示すよう、 `Button`、`CheckBox`ため、変更の赤いマーカーを持たないのみ、`LinearLayout`で適用された変更が、**大きな land**レイアウト。


-----


### <a name="conflict-persistence"></a>競合の永続化

競合は、次のように、XML のコメントとしてレイアウト ファイルに保存されます。

```xml
<!-- Widget Inserted Conflict | id:__root__ | @+id/checkBox1 -->
```

そのため、プロジェクトのデータを閉じて、すべての競合がある場合があります&ndash;は無視されているものも含みます。



