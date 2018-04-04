---
title: 別のレイアウト ビュー
description: このトピックでは、どのようにレイアウト バージョン管理できるリソース修飾子を使用して説明します。 たとえばがありますが、デバイスを横モードでのみ使用されるレイアウトのバージョンとの縦モードのみであるレイアウト バージョン。
ms.prod: xamarin
ms.assetid: 5EBF51FC-9048-F0CF-624A-D8782A91C1FD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 08/21/2017
ms.openlocfilehash: d2228169ed5d8575c9e332c85d963fca0400dea8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="alternative-layout-views"></a>別のレイアウト ビュー

_このトピックでは、どのようにレイアウト バージョン管理できるリソース修飾子を使用して説明します。たとえばがありますが、デバイスを横モードでのみ使用されるレイアウトのバージョンとの縦モードのみであるレイアウト バージョン。_


## <a name="creating-alternative-layouts"></a>別のレイアウトを作成します。

クリックすると、**レイアウト ビューの代替**アイコン (の左側に**デバイス**)、プレビュー ウィンドウが開き、プロジェクトで使用できる別のレイアウトの一覧を表示します。 別のレイアウトが存在しない場合、**既定**ビューが表示されます。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![代替レイアウト ビュー ペイン](alternative-layout-views-images/vs/01-alt-layout-view-pane-sml.png "代替レイアウト ビュー ペイン")](alternative-layout-views-images/vs/01-alt-layout-view-pane.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![代替レイアウト ビュー ペイン](alternative-layout-views-images/xs/01-alt-layout-view-pane-sml.png)](alternative-layout-views-images/xs/01-alt-layout-view-pane.png#lightbox)

-----

横に緑色のプラス記号をクリックすると**新しいバージョン**、**作成レイアウトのバリエーション**ダイアログが開き、このレイアウトのさまざまなリソース修飾子を選択することができます。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![レイアウトのバリエーションの作成](alternative-layout-views-images/vs/02-create-layout-variation-sml.png "レイアウトのバリエーションの作成")](alternative-layout-views-images/vs/02-create-layout-variation.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![レイアウトのバリエーションを作成します。](alternative-layout-views-images/xs/02-create-layout-variation-sml.png)](alternative-layout-views-images/xs/02-create-layout-variation.png#lightbox)

-----


次の例では、リソース修飾子で**画面の向き**に設定されている**ランドス ケープ**、および**画面サイズ**に変更が**大**. これによってという名前の新しいレイアウト バージョン作成**大きな土地**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![大土地バリエーション](alternative-layout-views-images/vs/03-large-land-sml.png "大土地のバリエーション")](alternative-layout-views-images/vs/03-large-land.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![大土地のバリエーション](alternative-layout-views-images/xs/03-large-land-sml.png)](alternative-layout-views-images/xs/03-large-land.png#lightbox)

-----


左側のプレビュー ウィンドウがリソース修飾子の選択項目の効果が表示されるに注意してください。 クリックすると**追加**代替のレイアウトが作成され、そのレイアウトにデザイナーを切り替えます。 **レイアウト ビューの代替手段**プレビュー ウィンドウでは、次のスクリーン ショットに示すように小さな右のポインターを使用してデザイナーにどのレイアウトが読み込まれることを示します。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![読み込まれたレイアウト インジケーター](alternative-layout-views-images/vs/04-new-layout-sml.png "読み込まれたレイアウトのインジケーター")](alternative-layout-views-images/vs/04-new-layout.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![読み込まれたレイアウトのインジケーター](alternative-layout-views-images/xs/04-new-layout-sml.png)](alternative-layout-views-images/xs/04-new-layout.png#lightbox)

-----



## <a name="editing-alternative-layouts"></a>別のレイアウトを編集

別のレイアウトを作成するときに、レイアウトのすべての分岐したバージョンに適用される 1 つの変更を行うことが望ましい場合は多くの場合です。 たとえば、黄色のすべてのレイアウト内に、ボタンのテキストを変更する可能性があります。 多数のレイアウトがあれば、厄介で面倒とエラーが発生しやすいすべてのバージョンに 1 つの変更を反映する必要がある場合のメンテナンスはすぐにします。 

複数のレイアウトのバージョンのメンテナンスを簡素化には、デザイナーが用意されています、**マルチ編集**1 つまたは複数のレイアウトの間で、変更内容を伝達するモード。 1 つ以上のレイアウトが含まれているときに、**マルチ編集**アイコンが表示されます。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![複数の編集 アイコン](alternative-layout-views-images/vs/05-multi-layout-icon-sml.png "複数の編集 アイコン")](alternative-layout-views-images/vs/05-multi-layout-icon.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![複数の編集 アイコン](alternative-layout-views-images/xs/05-multi-layout-icon-sml.png)](alternative-layout-views-images/xs/05-multi-layout-icon.png#lightbox)

-----


クリックすると、**マルチ編集**アイコン、線が表示されます (以下の手順に従って、レイアウトがリンクされているかを示す; は、1 つのレイアウトを変更すると、その変更が反映されます、リンクのレイアウトにします。 すべてのレイアウトのリンクを解除するには、次のスクリーン ショットに示されている丸のアイコンをクリックします。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![すべてのレイアウトのリンクを解除](alternative-layout-views-images/vs/06-multi-linked-sml.png "すべてのレイアウトのリンクを解除")](alternative-layout-views-images/vs/06-multi-linked.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![すべてのレイアウトのリンクを解除します。](alternative-layout-views-images/xs/06a-linked-sml.png)](alternative-layout-views-images/xs/06a-linked.png#lightbox)

-----


複数の 2 つのレイアウトがあれば、レイアウトが相互リンクされているかを判断するには、各レイアウト プレビューの左側にある編集ボタンを選択的に切り替えることができます。 たとえばを伝達する 1 つの変更を加える場合、最初と最後の 3 つのレイアウトには最初リンクを解除する中間のレイアウトは、ここに示すようにします。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Unlink 中間レイアウト](alternative-layout-views-images/vs/07-unlink-middle-layout-sml.png "Unlink 中間レイアウト")](alternative-layout-views-images/vs/07-unlink-middle-layout.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![中央のレイアウトのリンクを解除します。](alternative-layout-views-images/xs/06b-multi-linked-sml.png)](alternative-layout-views-images/xs/06b-multi-linked.png#lightbox)
 
-----
 

この例では、変更が加えいずれかに、**既定**または**長い**を他のレイアウトにはレイアウトが反映される、**大きな土地**レイアウトです。 



### <a name="multi-edit-example"></a>複数の編集の例 

一般に、1 つのレイアウトを変更すると、その同じ変更が他のすべてのリンクのレイアウトに伝達されます。 たとえば、新しいを追加する`TextView`ウィジェットに、**既定**レイアウトとテキストを変更する文字列を`Portrait`同じ変更がすべてのリンクのレイアウトに加えられると。 表示方法をここでは、**既定**レイアウト。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![TextView を追加](alternative-layout-views-images/vs/08-add-textview-sml.png "TextView を追加")](alternative-layout-views-images/vs/08-add-textview.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![TextView を追加します。](alternative-layout-views-images/xs/07-add-textview-sml.png)](alternative-layout-views-images/xs/07-add-textview.png#lightbox)
 
-----
 

`TextView`にも追加、**大きな土地**にリンクされているために、レイアウトを表示、**既定**レイアウト。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![TextView を横](alternative-layout-views-images/vs/09-landscape-textview-sml.png "TextView を横向き")](alternative-layout-views-images/vs/09-landscape-textview.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ランドス ケープ TextView](alternative-layout-views-images/xs/08-landscape-textview-sml.png)](alternative-layout-views-images/xs/08-landscape-textview.png#lightbox)
 
-----
 

ローカルのみ 1 つのレイアウトになっている変更を行う場合は (つまり、たくないその他のレイアウトのいずれかに反映されるまで変更)? これを行うには、次に説明したように、変更する前に、それを変更するレイアウトにリンクを解除する必要があります。 



### <a name="making-local-changes"></a>ローカルの変更を行う 

両方のレイアウトに追加したいとします`TextView`、テキスト文字列を変更する必要もが、**大規模な領土に関する**レイアウトを`Landscape`なく`Portrait`です。 この変更を行う場合**大規模な土地**に変更が反映されます両方のレイアウトがリンクされているときに、**既定**レイアウトです。 そのため、お必要があります最初のリンクを解除 2 つのレイアウト変更を行う前にします。 テキストを変更するときに**大規模な土地**に`Landscape`、Designer では、変更がに対してローカルであることを示す赤いフレームを使用してこの変更をマークする、**大規模な領土に関する**レイアウトは*いない*に反映、**既定**レイアウト。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ローカルの変更](alternative-layout-views-images/vs/10-local-change-sml.png "ローカルの変更")](alternative-layout-views-images/vs/10-local-change.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ローカルの変更](alternative-layout-views-images/xs/09-local-change-sml.png)](alternative-layout-views-images/xs/09-local-change.png#lightbox)
 
-----
 

クリックすると、**既定**表示するため、レイアウト、`TextView`テキスト文字列が設定されている`Portrait`です。 



## <a name="handling-conflicts"></a>競合の処理 

内のテキストの色を変更する場合、**既定**緑にレイアウト、リンクのレイアウトで表示される警告アイコンが表示されます。 そのレイアウトをクリックすると、競合を表示するために、レイアウトが開きます。 競合の原因となったウィジェットが赤い枠で強調表示され、次のメッセージが表示されます:*最近の変更がこの代替レイアウトでは競合の原因となった*です。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![競合する変更](alternative-layout-views-images/vs/11-conflicting-change-sml.png "競合する変更")](alternative-layout-views-images/vs/11-conflicting-change.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![変更が競合してください。](alternative-layout-views-images/xs/10-conflict-sml.png)](alternative-layout-views-images/xs/10-conflict.png#lightbox)
 
-----
 

A*競合ボックス*が競合を説明する、ウィジェットの右側に表示されます。 

[![競合に関する警告](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

競合ボックスは、変更されたプロパティが一覧表示し、その値を一覧表示します。 クリックすると**無視競合**プロパティの変更をこのウィジェットにのみ適用されます。 クリックすると**適用**に関して、リンクに対応するウィジェットでもこのウィジェットにプロパティの変更を適用**既定**レイアウトです。 すべてのプロパティ変更が適用されている場合、競合は自動的に破棄されます。 


### <a name="view-group-conflicts"></a>グループの表示の競合 

プロパティの変更は、競合の唯一のソースではありません。 挿入またはウィジェットを削除すると、競合を検出できます。 たとえば、**大規模な領土に関する**からレイアウトのリンクを解除、**既定の**レイアウト、および`TextView`で、**大規模な領土に関する**レイアウトは、ドラッグ アンド ドロップの上、`Button`デザイナーは、競合を示すために移動したウィジェットをマークします。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![グループの競合を表示](alternative-layout-views-images/vs/12-view-group-conflict-sml.png "グループ競合の表示")](alternative-layout-views-images/vs/12-view-group-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ビューのグループの競合](alternative-layout-views-images/xs/12-view-group-conflict-sml.png)](alternative-layout-views-images/xs/12-view-group-conflict.png#lightbox)
 
-----
 

ただしにはありませんマーカー、`Button`です。 位置、`Button`が変更されている、`Button`に固有の変更を適用した表示されない、**大きな土地**構成します。 

場合、`CheckBox`に追加、**既定**レイアウト、別の競合が生成され、経由で警告アイコンが表示されます、**大きな土地**レイアウト。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![チェック ボックスをオン競合](alternative-layout-views-images/vs/13-checkbox-conflict-sml.png "競合のチェック ボックス")](alternative-layout-views-images/vs/13-checkbox-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![競合のチェック ボックス](alternative-layout-views-images/xs/13-checkbox-conflict-sml.png)](alternative-layout-views-images/xs/13-checkbox-conflict.png#lightbox)
 
-----
 

クリックすると、**大きな土地**レイアウトが、競合を表示します。 次のメッセージが表示されます:*最近の変更がこの代替レイアウトでは競合の原因となった*です。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Alt レイアウト競合](alternative-layout-views-images/vs/14-alt-layout-conflict-sml.png "Alt レイアウトの競合")](alternative-layout-views-images/vs/14-alt-layout-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![Alt レイアウトの競合](alternative-layout-views-images/xs/14-alt-layout-conflict-sml.png)](alternative-layout-views-images/xs/14-alt-layout-conflict.png#lightbox)
 
-----
 

さらに、競合のボックスには、次のメッセージが表示されます。

[![競合のメッセージ](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

追加する、`CheckBox`ため、競合が原因で、**大規模な領土に関する**レイアウト変更には、`LinearLayout`含まれています。 ただし、ここでは競合ボックスが表示されます、ウィジェットに挿入した、**既定**レイアウト (、 `CheckBox`)。

クリックすると**競合を無視する**、デザイナーは、ウィジェットが欠落しているドラッグ アンド ドロップをレイアウトする競合ボックスに表示されるウィジェットを許可するという、競合を解決 (ここで、**大規模な領土に関する**レイアウト)。 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![グループの競合を解決](alternative-layout-views-images/vs/15-resolved-group-conflict-sml.png "グループ競合の解決")](alternative-layout-views-images/vs/15-resolved-group-conflict.png#lightbox)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![グループの競合の解決](alternative-layout-views-images/xs/16-resolved-group-conflict-sml.png)](alternative-layout-views-images/xs/16-resolved-group-conflict.png#lightbox)
 
-----
 

前の例のように、 `Button`、`CheckBox`ため赤い変更マーカーがありませんのみ、`LinearLayout`で適用された変更が、**大規模な領土に関する**レイアウトです。



### <a name="conflict-persistence"></a>競合の永続化

競合は、次のように XML のコメントとしてレイアウト ファイルに保存されます。

```xml
<!-- Widget Inserted Conflict | id:__root__ | @+id/checkBox1 -->
```

そのため、プロジェクトをいったん閉じて再び開く、すべての競合がある場合があります&ndash;は無視されたものも含めてです。

