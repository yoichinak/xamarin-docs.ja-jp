---
title: Xamarin.iOS でのテーブルの外観のカスタマイズ
description: このドキュメントでは、Xamarin.iOS でのテーブルの外観をカスタマイズする方法について説明します。 これは、セルのスタイル、[アクセサリ]、セルの区切り記号、およびカスタムのセルのレイアウトについて説明します。
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: e1e86918d29e12d2f34dd3008b8c1d8e47471c24
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233550"
---
# <a name="customizing-a-tables-appearance-in-xamarinios"></a>Xamarin.iOS でのテーブルの外観のカスタマイズ

テーブルの外観を変更する最も簡単な方法では、別のセル スタイルを使用します。 内の各セルを作成するときに、どのセル スタイルが使用されるを変更することができます、`UITableViewSource`の`GetCell`メソッド。

## <a name="cell-styles"></a>セルのスタイル

これには 4 つの組み込みスタイルがあります。

-  **既定の**– サポート、`UIImageView`します。
-  **サブタイトル**– サポート、`UIImageView`とサブタイトル。
-  **Value1** – 適切な固定サブタイトルは、サポート、`UIImageView`します。
-  **Value2** – は右揃えにタイトルとサブタイトルは左揃え (ただし、イメージがありません)。


これらのスクリーン ショットでは、各スタイルの表示方法を示します。

 [![](customizing-table-appearance-images/image7.png "これらのスクリーン ショットは、各スタイルの表示方法を示します")](customizing-table-appearance-images/image7.png#lightbox)

サンプル**CellDefaultTable**これらの画面を生成するためにコードが含まれています。 セルのスタイルが設定されて、`UITableViewCell`コンス トラクターには、次のようにします。

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

[サポートされるプロパティ](xref:UIKit.UITableViewCell)セルのスタイルを設定できます。

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## <a name="accessories"></a>Accessories

セルには、ビューの右側に追加された次の [アクセサリ] を持つことができます。

-   **チェック マーク**– をテーブル内の複数選択を示すために使用できます。
-   **DetailButton** – セル自体をタッチするさまざまな機能を実行することができます、セルの残りの部分とは無関係にタッチに応答 (ポップアップまたは新しいウィンドウを開くなどの一部を`UINavigationController`stack)。
-   **DisclosureIndicator** – をセルの手を加えることと、別のビューが開くことを示すために通常使用されます。
-   **DetailDisclosureButton** – の組み合わせ、`DetailButton`と`DisclosureIndicator`します。


これは、外観です。

 [![](customizing-table-appearance-images/image8.png "サンプル [アクセサリ]")](customizing-table-appearance-images/image8.png#lightbox)

これらのアクセサリを設定することができますのいずれかを表示する、`Accessory`プロパティ、`GetCell`メソッド。

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

ときに、`DetailButton`または`DetailDisclosureButton`もオーバーライドする必要があります、表示されて、`AccessoryButtonTapped`アクセスがあるときに、何らかのアクションを実行します。

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

サンプル**CellAccessoryTable** [アクセサリ] を使用する例を示しています。

## <a name="cell-separators"></a>セルの区切り記号

セルの区切り記号は、テーブルのセルがテーブルを区切るために使用します。 テーブルには、プロパティを設定します。

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

区切り記号にぼかしや活気効果を追加することもします。

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

## <a name="creating-custom-cell-layouts"></a>カスタムのセルのレイアウトを作成します。

テーブルの視覚スタイルを変更するには、表示するにはカスタムのセルを指定する必要があります。 カスタムのセルには、さまざまな色とコントロールのレイアウトを持つことができます。

CellCustomTable 例では、実装、`UITableViewCell`のカスタム レイアウトを定義するサブクラス`UILabel`s と`UIImage`のフォントと色。 結果のセルは、次のようになります。

 [![](customizing-table-appearance-images/image9.png "カスタムのセルのレイアウト")](customizing-table-appearance-images/image9.png#lightbox)

カスタム セル クラスは、のみの 3 つの方法で構成されます。

-   **コンス トラクター** : UI コントロールを作成し、(例: カスタムのスタイル プロパティを設定します フォント フェイス、サイズと色)。
-   **UpdateCell** – メソッドを`UITableView.GetCell`を使用して、セルのプロパティを設定します。
-   **LayoutSubviews** – UI コントロールの位置を設定します。 例のすべてのセルは、同じレイアウトが、(特に、さまざまなサイズ) より複雑なセルが表示されている内容に応じて別のレイアウト位置にあります。


完全なサンプル コード**CellCustomTable > CustomVegeCell.cs**に従います。

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

`GetCell`のメソッド、`UITableViewSource`カスタム セルを作成するように変更する必要があります。

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
