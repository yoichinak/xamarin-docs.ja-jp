---
title: 代替レイアウト ビュー
description: このトピックでは、リソース修飾子を使用してレイアウトをバージョン管理する方法について説明します。 たとえば、デバイスが横モードの場合にのみ使用されるレイアウトのバージョンと、縦モード専用のレイアウトバージョンがあります。
ms.prod: xamarin
ms.assetid: 5EBF51FC-9048-F0CF-624A-D8782A91C1FD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/25/2018
ms.openlocfilehash: 017d2d05c04dfaf2378ad1b0129eb4a75be2e777
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019489"
---
# <a name="alternative-layout-views"></a>代替レイアウトビュー

_このトピックでは、リソース修飾子を使用してレイアウトを作成する方法について説明します。たとえば、デバイスが横モードの場合にのみ使用されるレイアウトのバージョンを作成したり、縦モード専用のレイアウトバージョンを作成したりすることができます。_

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

## <a name="creating-alternative-layouts"></a>代替レイアウトの作成

( **[デバイス]** の左側にある)**別のレイアウトビュー**アイコンをクリックすると、プレビューウィンドウが開き、プロジェクトで使用できる代替レイアウトが一覧表示されます。 代替レイアウトがない場合は、**既定**のビューが表示されます。 

[![代替レイアウトビューペイン](alternative-layout-views-images/vs/01-alt-layout-view-pane-sml.png "代替レイアウトビューペイン")](alternative-layout-views-images/vs/01-alt-layout-view-pane.png#lightbox)

**[新しいバージョン]** の横にある緑色の正符号をクリックすると、 **[レイアウトバリエーションの作成]** ダイアログボックスが開き、このレイアウトバリエーションのリソース修飾子を選択できます。 

[![レイアウトバリエーションの作成](alternative-layout-views-images/vs/02-create-layout-variation-sml.png "レイアウトバリエーションの作成")](alternative-layout-views-images/vs/02-create-layout-variation.png#lightbox)

次の例では、**画面の向き**のリソース修飾子が**横向き**に設定されており、**画面サイズ**が**Large**に変更されています。 これに**より、large 土地**という名前の新しいレイアウトバージョンが作成されます。

[![大規模なバリエーション](alternative-layout-views-images/vs/03-large-land-sml.png "大規模なバリエーション")](alternative-layout-views-images/vs/03-large-land.png#lightbox)

左側のプレビューウィンドウに、リソース修飾子の選択の効果が表示されることに注意してください。 **[追加]** をクリックすると、代替レイアウトが作成され、デザイナーがそのレイアウトに切り替わります。 **代替レイアウトビュー**のプレビューペインでは、次のスクリーンショットに示すように、小さい右のポインターを使用してデザイナーに読み込まれるレイアウトが示されます。 

[![読み込まれたレイアウトインジケーター](alternative-layout-views-images/vs/04-new-layout-sml.png "読み込まれたレイアウトインジケーター")](alternative-layout-views-images/vs/04-new-layout.png#lightbox)

## <a name="editing-alternative-layouts"></a>代替レイアウトの編集

代替レイアウトを作成する場合、多くの場合、すべてのフォークされたバージョンのレイアウトに適用する1つの変更を加えることをお勧めします。 たとえば、すべてのレイアウトでボタンのテキストを黄色に変更することができます。 多数のレイアウトがあり、すべてのバージョンに1つの変更を反映する必要がある場合、メンテナンスが煩雑になり、エラーが発生しやすくなります。

複数のレイアウトバージョンの保守を簡単にするために、デザイナーには1つ以上のレイアウトに対して変更を反映する**マルチ編集**モードが用意されています。 複数のレイアウトがある場合は、**マルチ編集**アイコンが表示されます。 

[![複数編集アイコン](alternative-layout-views-images/vs/05-multi-layout-icon-sml.png "複数編集アイコン")](alternative-layout-views-images/vs/05-multi-layout-icon.png#lightbox)

**マルチ編集**アイコンをクリックすると、次に示すように、レイアウトがリンクされていることを示す行が表示されます。つまり、1つのレイアウトに変更を加えると、その変更はリンクされたレイアウトに反映されます。 次のスクリーンショットに示されている丸のアイコンをクリックすると、すべてのレイアウトのリンクを解除できます。 

[![すべてのレイアウトのリンク解除](alternative-layout-views-images/vs/06-multi-linked-sml.png "すべてのレイアウトのリンク解除")](alternative-layout-views-images/vs/06-multi-linked.png#lightbox)

3つ以上のレイアウトがある場合は、各レイアウトプレビューの左側にある [編集] ボタンを選択して切り替えることで、どのレイアウトがリンクされているかを判断できます。 たとえば、3つのレイアウトの最初と最後に反映される1つの変更を行う場合は、次に示すように、最初に中央レイアウトのリンクを解除します。 

[![中央レイアウトのリンク解除](alternative-layout-views-images/vs/07-unlink-middle-layout-sml.png "中央レイアウトのリンク解除")](alternative-layout-views-images/vs/07-unlink-middle-layout.png#lightbox)

この例では、**既定**のレイアウトまたは**長い**レイアウトに加えられた変更は、他のレイアウトに反映されますが、**大規模な**レイアウトには反映されません。

### <a name="multi-edit-example"></a>複数編集の例 

一般に、1つのレイアウトを変更すると、その同じ変更が他のすべてのリンクレイアウトに反映されます。 たとえば、新しい `TextView` ウィジェットを**既定**のレイアウトに追加し、そのテキスト文字列を `Portrait` に変更すると、すべてのリンクレイアウトに対して同じ変更が行われます。 **既定**のレイアウトでは、次のように表示されます。 

[![TextView の追加](alternative-layout-views-images/vs/08-add-textview-sml.png "TextView の追加")](alternative-layout-views-images/vs/08-add-textview.png#lightbox)

`TextView` は、**既定**のレイアウトにリンクされているため、**大規模な**レイアウトビューにも追加されます。 

[![横の TextView](alternative-layout-views-images/vs/09-landscape-textview-sml.png "横の TextView")](alternative-layout-views-images/vs/09-landscape-textview.png#lightbox)

しかし、1つのレイアウトのみに対してローカルな変更を行う場合はどうすればよいでしょうか。つまり、変更を他のどのレイアウトにも反映させたくありませんか。 これを行うには、次に説明するように、変更する前に変更するレイアウトのリンクを解除する必要があります。 

### <a name="making-local-changes"></a>ローカルでの変更 

どちらのレイアウトにも追加された `TextView` が必要であるとしますが、**大規模な**レイアウトのテキスト文字列を `Portrait` ではなく `Landscape` に変更することもできます。 両方のレイアウトがリンクされている間にこの変更を**大土地**に変更すると、変更が**既定**のレイアウトに反映されます。 そのため、最初に2つのレイアウトのリンクを解除してから、変更を行ってください。 **大土地**のテキストを `Landscape` に変更すると、デザイナーはこの変更を赤いフレームでマークして、変更が**大規模な**レイアウトに対してローカルであり、**既定**のレイアウトに反映され*ない*ことを示します。 

[![ローカルの変更](alternative-layout-views-images/vs/10-local-change-sml.png "ローカルの変更")](alternative-layout-views-images/vs/10-local-change.png#lightbox)

**既定**のレイアウトをクリックして表示すると、`TextView` のテキスト文字列は `Portrait` に設定されたままになります。 

## <a name="handling-conflicts"></a>競合の処理 

**既定**のレイアウトのテキストの色を緑色に変更すると、リンクされたレイアウトに警告アイコンが表示されます。 レイアウトをクリックすると、レイアウトが開き、競合が表示されます。 競合の原因となったウィジェットが赤い枠で強調表示され、次のメッセージが表示されます。*最近の変更により、この代替レイアウトで競合が発生しました*。 

[![競合する変更](alternative-layout-views-images/vs/11-conflicting-change-sml.png "競合する変更")](alternative-layout-views-images/vs/11-conflicting-change.png#lightbox)

競合を説明するために、ウィジェットの右側に [*競合] ボックス*が表示されます。 

[![の競合の警告](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

[競合] ボックスには、変更されたプロパティの一覧が表示され、その値が一覧表示されます。 [**競合を無視**する] をクリックすると、プロパティの変更がこのウィジェットにのみ適用されます。 **[適用]** をクリックすると、このウィジェットだけでなく、リンクされた**既定**のレイアウトの対応するウィジェットにもプロパティの変更が適用されます。 すべてのプロパティの変更が適用されると、競合は自動的に破棄されます。 

### <a name="view-group-conflicts"></a>グループの競合の表示 

プロパティの変更は、競合の唯一のソースではありません。 ウィジェットの挿入時または削除時に競合が検出されることがあります。 たとえば、**大規模な**レイアウトが**既定**のレイアウトとリンクされていない場合に、**大規模な**レイアウトの `TextView` が `Button` の上にドラッグアンドドロップされると、デザイナーは移動したウィジェットに競合があることをマークします。

[![グループの競合の表示](alternative-layout-views-images/vs/12-view-group-conflict-sml.png "グループの競合の表示")](alternative-layout-views-images/vs/12-view-group-conflict.png#lightbox)

ただし、`Button` にマーカーはありません。 `Button` の位置は変更されていますが、`Button` には、**大規模な**構成に固有の適用済みの変更は表示されません。 

`CheckBox` が**既定**のレイアウトに追加されると、別の競合が生成され、**大規模な**レイアウトに対して警告アイコンが表示されます。 

[![チェックボックスの競合](alternative-layout-views-images/vs/13-checkbox-conflict-sml.png "チェックボックスの競合")](alternative-layout-views-images/vs/13-checkbox-conflict.png#lightbox)

**大規模な**レイアウトをクリックすると、競合が明らかになります。 次のメッセージが表示されます。*最近の変更により、この代替レイアウトで競合が発生しました*: 

[![Alt レイアウトの競合](alternative-layout-views-images/vs/14-alt-layout-conflict-sml.png "Alt レイアウトの競合")](alternative-layout-views-images/vs/14-alt-layout-conflict.png#lightbox)

さらに、[競合] ボックスには次のメッセージが表示されます。

[![の競合メッセージ](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

`CheckBox` を追加すると、それを含む `LinearLayout` で**大規模な**レイアウトが変更されるため、競合が発生します。 ただし、この場合、[競合] ボックスには、**既定**のレイアウトに挿入されたウィジェットが表示されます (`CheckBox`)。

[競合を**無視**] をクリックすると、デザイナーによって競合が解決され、[競合] ボックスに表示されているウィジェットをドラッグして、ウィジェットが存在しないレイアウト (この場合は**大規模な**レイアウト) にドロップできます。 

[![解決されたグループの競合](alternative-layout-views-images/vs/15-resolved-group-conflict-sml.png "解決されたグループの競合")](alternative-layout-views-images/vs/15-resolved-group-conflict.png#lightbox)

前の例で示したように、`Button` では、`CheckBox` に赤い変更マーカーがありません。これは、`LinearLayout` のみが**大規模な**レイアウトで適用された変更を持っているためです。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

## <a name="creating-alternative-layouts"></a>代替レイアウトの作成

( **[デバイス]** の左側にある)**別のレイアウトビュー**アイコンをクリックすると、プレビューウィンドウが開き、プロジェクトで使用できる代替レイアウトが一覧表示されます。 代替レイアウトがない場合は、**既定**のビューが表示されます。 

[代替レイアウトビューペイン![](alternative-layout-views-images/xs/01-alt-layout-view-pane-sml.png)](alternative-layout-views-images/xs/01-alt-layout-view-pane.png#lightbox)

**[新しいバージョン]** の横にある緑色の正符号をクリックすると、 **[レイアウトバリエーションの作成]** ダイアログボックスが開き、このレイアウトバリエーションのリソース修飾子を選択できます。 

[レイアウトのバリエーションを作成![には](alternative-layout-views-images/xs/02-create-layout-variation-sml.png)](alternative-layout-views-images/xs/02-create-layout-variation.png#lightbox)

次の例では、**画面の向き**のリソース修飾子が**横向き**に設定されており、**画面サイズ**が**Large**に変更されています。 これに**より、large 土地**という名前の新しいレイアウトバージョンが作成されます。

[![大規模なバリエーション](alternative-layout-views-images/xs/03-large-land-sml.png)](alternative-layout-views-images/xs/03-large-land.png#lightbox)

左側のプレビューウィンドウに、リソース修飾子の選択の効果が表示されることに注意してください。 **[追加]** をクリックすると、代替レイアウトが作成され、デザイナーがそのレイアウトに切り替わります。 **代替レイアウトビュー**のプレビューペインでは、次のスクリーンショットに示すように、小さい右のポインターを使用してデザイナーに読み込まれるレイアウトが示されます。 

[![読み込まれたレイアウトインジケーター](alternative-layout-views-images/xs/04-new-layout-sml.png)](alternative-layout-views-images/xs/04-new-layout.png#lightbox)

## <a name="editing-alternative-layouts"></a>代替レイアウトの編集

代替レイアウトを作成する場合、多くの場合、すべてのフォークされたバージョンのレイアウトに適用する1つの変更を加えることをお勧めします。 たとえば、すべてのレイアウトでボタンのテキストを黄色に変更することができます。 多数のレイアウトがあり、すべてのバージョンに1つの変更を反映する必要がある場合、メンテナンスが煩雑になり、エラーが発生しやすくなります。

複数のレイアウトバージョンの保守を簡単にするために、デザイナーには1つ以上のレイアウトに対して変更を反映する**マルチ編集**モードが用意されています。 複数のレイアウトがある場合は、**マルチ編集**アイコンが表示されます。 

[![複数編集 アイコン](alternative-layout-views-images/xs/05-multi-layout-icon-sml.png)](alternative-layout-views-images/xs/05-multi-layout-icon.png#lightbox)

**マルチ編集**アイコンをクリックすると、次に示すように、レイアウトがリンクされていることを示す行が表示されます。つまり、1つのレイアウトに変更を加えると、その変更はリンクされたレイアウトに反映されます。 次のスクリーンショットに示されている丸のアイコンをクリックすると、すべてのレイアウトのリンクを解除できます。 

[すべてのレイアウトのリンクを解除![には](alternative-layout-views-images/xs/06a-linked-sml.png)](alternative-layout-views-images/xs/06a-linked.png#lightbox)

3つ以上のレイアウトがある場合は、各レイアウトプレビューの左側にある [編集] ボタンを選択して切り替えることで、どのレイアウトがリンクされているかを判断できます。 たとえば、3つのレイアウトの最初と最後に反映される1つの変更を行う場合は、次に示すように、最初に中央レイアウトのリンクを解除します。 

[中央レイアウト![リンク解除](alternative-layout-views-images/xs/06b-multi-linked-sml.png)](alternative-layout-views-images/xs/06b-multi-linked.png#lightbox)

この例では、**既定**のレイアウトまたは**長い**レイアウトに加えられた変更は、他のレイアウトに反映されますが、**大規模な**レイアウトには反映されません。 

### <a name="multi-edit-example"></a>複数編集の例 

一般に、1つのレイアウトを変更すると、その同じ変更が他のすべてのリンクレイアウトに反映されます。 たとえば、新しい `TextView` ウィジェットを**既定**のレイアウトに追加し、そのテキスト文字列を `Portrait` に変更すると、すべてのリンクレイアウトに対して同じ変更が行われます。 **既定**のレイアウトでは、次のように表示されます。 

[TextView を追加![には](alternative-layout-views-images/xs/07-add-textview-sml.png)](alternative-layout-views-images/xs/07-add-textview.png#lightbox)

`TextView` は、**既定**のレイアウトにリンクされているため、**大規模な**レイアウトビューにも追加されます。 

[![横 TextView](alternative-layout-views-images/xs/08-landscape-textview-sml.png)](alternative-layout-views-images/xs/08-landscape-textview.png#lightbox)

しかし、1つのレイアウトのみに対してローカルな変更を行う場合はどうすればよいでしょうか。つまり、変更を他のどのレイアウトにも反映させたくありませんか。 これを行うには、次に説明するように、変更する前に変更するレイアウトのリンクを解除する必要があります。 

### <a name="making-local-changes"></a>ローカルでの変更 

どちらのレイアウトにも追加された `TextView` が必要であるとしますが、**大規模な**レイアウトのテキスト文字列を `Portrait` ではなく `Landscape` に変更することもできます。 両方のレイアウトがリンクされている間にこの変更を**大土地**に変更すると、変更が**既定**のレイアウトに反映されます。 そのため、最初に2つのレイアウトのリンクを解除してから、変更を行ってください。 **大土地**のテキストを `Landscape` に変更すると、デザイナーはこの変更を赤いフレームでマークして、変更が**大規模な**レイアウトに対してローカルであり、**既定**のレイアウトに反映され*ない*ことを示します。 

[ローカルの変更を![](alternative-layout-views-images/xs/09-local-change-sml.png)](alternative-layout-views-images/xs/09-local-change.png#lightbox)

**既定**のレイアウトをクリックして表示すると、`TextView` のテキスト文字列は `Portrait` に設定されたままになります。 

## <a name="handling-conflicts"></a>競合の処理 

**既定**のレイアウトのテキストの色を緑色に変更すると、リンクされたレイアウトに警告アイコンが表示されます。 レイアウトをクリックすると、レイアウトが開き、競合が表示されます。 競合の原因となったウィジェットが赤い枠で強調表示され、次のメッセージが表示されます。*最近の変更により、この代替レイアウトで競合が発生しました*。 

[競合する変更の![](alternative-layout-views-images/xs/10-conflict-sml.png)](alternative-layout-views-images/xs/10-conflict.png#lightbox)

競合を説明するために、ウィジェットの右側に [*競合] ボックス*が表示されます。 

[![の競合の警告](alternative-layout-views-images/xs/11-warning-sml.png)](alternative-layout-views-images/xs/11-warning.png#lightbox)

[競合] ボックスには、変更されたプロパティの一覧が表示され、その値が一覧表示されます。 [**競合を無視**する] をクリックすると、プロパティの変更がこのウィジェットにのみ適用されます。 **[適用]** をクリックすると、このウィジェットだけでなく、リンクされた**既定**のレイアウトの対応するウィジェットにもプロパティの変更が適用されます。 すべてのプロパティの変更が適用されると、競合は自動的に破棄されます。 

### <a name="view-group-conflicts"></a>グループの競合の表示 

プロパティの変更は、競合の唯一のソースではありません。 ウィジェットの挿入時または削除時に競合が検出されることがあります。 たとえば、**大規模な**レイアウトが**既定**のレイアウトとリンクされていない場合に、**大規模な**レイアウトの `TextView` が `Button` の上にドラッグアンドドロップされると、デザイナーは移動したウィジェットに競合があることをマークします。

[グループの競合を表示![](alternative-layout-views-images/xs/12-view-group-conflict-sml.png)](alternative-layout-views-images/xs/12-view-group-conflict.png#lightbox)

ただし、`Button` にマーカーはありません。 `Button` の位置は変更されていますが、`Button` には、**大規模な**構成に固有の適用済みの変更は表示されません。 

`CheckBox` が**既定**のレイアウトに追加されると、別の競合が生成され、**大規模な**レイアウトに対して警告アイコンが表示されます。 

[![チェックボックスの競合](alternative-layout-views-images/xs/13-checkbox-conflict-sml.png)](alternative-layout-views-images/xs/13-checkbox-conflict.png#lightbox)

**大規模な**レイアウトをクリックすると、競合が明らかになります。 次のメッセージが表示されます。*最近の変更により、この代替レイアウトで競合が発生しました*。 

[Alt レイアウトの競合![](alternative-layout-views-images/xs/14-alt-layout-conflict-sml.png)](alternative-layout-views-images/xs/14-alt-layout-conflict.png#lightbox)

さらに、[競合] ボックスには次のメッセージが表示されます。

[![の競合メッセージ](alternative-layout-views-images/xs/15-conflict-message-sml.png)](alternative-layout-views-images/xs/15-conflict-message.png#lightbox)

`CheckBox` を追加すると、それを含む `LinearLayout` で**大規模な**レイアウトが変更されるため、競合が発生します。 ただし、この場合、[競合] ボックスには、**既定**のレイアウトに挿入されたウィジェットが表示されます (`CheckBox`)。

[競合を**無視**] をクリックすると、デザイナーによって競合が解決され、[競合] ボックスに表示されているウィジェットをドラッグして、ウィジェットが存在しないレイアウト (この場合は**大規模な**レイアウト) にドロップできます。 

[解決されたグループの競合![](alternative-layout-views-images/xs/16-resolved-group-conflict-sml.png)](alternative-layout-views-images/xs/16-resolved-group-conflict.png#lightbox)

前の例で示したように、`Button` では、`CheckBox` に赤い変更マーカーがありません。これは、`LinearLayout` のみが**大規模な**レイアウトで適用された変更を持っているためです。

-----

### <a name="conflict-persistence"></a>競合の永続化

次に示すように、競合は、XML コメントとしてレイアウトファイルに保持されます。

```xml
<!-- Widget Inserted Conflict | id:__root__ | @+id/checkBox1 -->
```

このため、プロジェクトを閉じてからもう一度開くと、無視された競合も含めて、すべての競合が引き続き &ndash; 存在します。
