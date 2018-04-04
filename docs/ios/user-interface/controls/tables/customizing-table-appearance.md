---
title: テーブルの外観のカスタマイズ
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: a447c59e7384ce7da168efdd018bc23c2abb25c2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="customizing-a-tables-appearance"></a>テーブルの外観のカスタマイズ

テーブルの外観を変更する最も簡単な方法では、別のセル スタイルを使用します。 内の各セルを作成するときにどのセル スタイルは使用を変更することができます、`UITableViewSource`の`GetCell`メソッドです。

## <a name="cell-styles"></a>セルのスタイル

次の 4 つの組み込みスタイルがあります。

-  **既定の**– をサポートしている、`UIImageView`です。
-  **字幕**– をサポートしている、`UIImageView`とサブタイトルです。
-  **Value1**右 – 整列の字幕をサポートしている、`UIImageView`です。
-  **Value2** – が右揃えにタイトルとサブタイトルを左揃えの (ただし、イメージがない)。


これらのスクリーン ショットは、各スタイルの表示方法を示しています。

 [![](customizing-table-appearance-images/image7.png "これらのスクリーン ショットは、各スタイルの外観を表示します。")](customizing-table-appearance-images/image7.png#lightbox)

サンプル**CellDefaultTable**これらの画面を生成するためにコードが含まれています。 セルのスタイルを設定、`UITableViewCell`コンス トラクターには、次のようにします。

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

[プロパティはサポートされて](http://developer.xamarin.com/api/type/UIKit.UITableViewCell/)セルのスタイルを設定できます。

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## <a name="accessories"></a>アクセサリ

セルには、ビューの右側に追加される次のアクセサリを持つことができます。

-   **チェック マーク**– テーブル内の複数選択を示すために使用できます。
-   **DetailButton** – セル自体と接触している別の関数を実行することができる、セルの残りの部分とは無関係にタッチする応答 (ポップアップやが新しいウィンドウを開くなどの一部、`UINavigationController`スタック)。
-   **DisclosureIndicator** – と接触しているセルが開かれる他のビューを示すために通常使用されます。
-   **DetailDisclosureButton** – の組み合わせ、`DetailButton`と`DisclosureIndicator`です。


これは、どのように見えます。

 [![](customizing-table-appearance-images/image8.png "サンプルのアクセサリ")](customizing-table-appearance-images/image8.png#lightbox)

設定できるアクセサリのいずれかを表示する、`Accessory`プロパティに、`GetCell`メソッド。

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

ときに、`DetailButton`または`DetailDisclosureButton`もオーバーライドする必要があります、表示されて、`AccessoryButtonTapped`アクセスがあるときに何らかのアクションを実行します。

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

サンプル**CellAccessoryTable**アクセサリを使用する例を示しています。

## <a name="cell-separators"></a>セルの区切り記号

セルの区切り記号は、テーブルを区切るために使用するテーブル セルです。 テーブルには、プロパティを設定します。

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

区切り記号にぼかしまたは活気の効果を追加することも。

```csharp
// blur effect
TableView.SeparatorEffect =
    UIBlurEffect.FromStyle(UIBlurEffectStyle.Dark);

//vibrancy effect
var effect = UIBlurEffect.FromStyle(UIBlurEffectStyle.Light);
TableView.SeparatorEffect = UIVibrancyEffect.FromBlurEffect(effect);
```

区切り記号、埋め込みこともできます。

```csharp
TableView.SeparatorInset.InsetRect(new CGRect(4, 4, 150, 2));
```

## <a name="creating-custom-cell-layouts"></a>セルのカスタム レイアウトを作成します。

テーブルの visual スタイルを変更するには、表示するにはカスタムのセルを指定する必要があります。 カスタムのセルには、さまざまな色とコントロールのレイアウトを持つことができます。

CellCustomTable 例では、実装、`UITableViewCell`のカスタム レイアウトを定義するサブクラス`UILabel`s と`UIImage`のフォントと色。 結果として得られるセルは、次のようになります。

 [![](customizing-table-appearance-images/image9.png "セルのカスタム レイアウト")](customizing-table-appearance-images/image9.png#lightbox)

カスタム セル クラスは、のみの 3 つの方法で構成されます。

-   **コンス トラクター** – UI コントロールを作成し、(カスタム スタイル プロパティを設定します フォントのフォント フェイス、サイズと色)。
-   **UpdateCell** – メソッドを`UITableView.GetCell`を使用して、セルのプロパティを設定します。
-   **LayoutSubviews** – UI コントロールの場所を設定します。 例では、すべてのセルに、同じレイアウトが (特に、さまざまなサイズで) さらに複雑なセルが表示されている内容に応じたさまざまなレイアウトの配置に必要があります。


完全なサンプル コードで**CellCustomTable > CustomVegeCell.cs**に従います。

```csharp
public class CustomVegeCell : UITableViewCell  {
    UILabel headingLabel, subheadingLabel;
    UIImageView imageView;
    public CustomVegeCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
    {
        SelectionStyle = UITableViewCellSelectionStyle.Gray;
        ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);
        imageView = new UIImageView();
        headingLabel = new UILabel () {
            Font = UIFont.FromName("Cochin-BoldItalic", 22f),
            TextColor = UIColor.FromRGB (127, 51, 0),
            BackgroundColor = UIColor.Clear
        };
        subheadingLabel = new UILabel () {
            Font = UIFont.FromName("AmericanTypewriter", 12f),
            TextColor = UIColor.FromRGB (38, 127, 0),
            TextAlignment = UITextAlignment.Center,
            BackgroundColor = UIColor.Clear
        };
        ContentView.AddSubviews(new UIView[] {headingLabel, subheadingLabel, imageView});

    }
    public void UpdateCell (string caption, string subtitle, UIImage image)
    {
        imageView.Image = image;
        headingLabel.Text = caption;
        subheadingLabel.Text = subtitle;
    }
    public override void LayoutSubviews ()
    {
        base.LayoutSubviews ();
        imageView.Frame = new CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
        headingLabel.Frame = new CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
        subheadingLabel.Frame = new CGRect (100, 18, 100, 20);
    }
}
```

`GetCell`のメソッド、`UITableViewSource`カスタムのセルを作成するように変更する必要があります。

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (cellIdentifier) as CustomVegeCell;
    if (cell == null)
        cell = new CustomVegeCell (cellIdentifier);
    cell.UpdateCell (tableItems[indexPath.Row].Heading
            , tableItems[indexPath.Row].SubHeading
            , UIImage.FromFile ("Images/" + tableItems[indexPath.Row].ImageName) );
    return cell;
}
```



## <a name="related-links"></a>関連リンク

- [WorkingWithTables (サンプル)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
