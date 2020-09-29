---
title: Xamarin でのテーブルの外観のカスタマイズ
description: このドキュメントでは、Xamarin. iOS でテーブルの外観をカスタマイズする方法について説明します。 セルのスタイル、アクセサリ、セルの区切り記号、およびカスタムセルのレイアウトについて説明します。
ms.prod: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: dd79c2059d53702d947a966327bf22b2043d36c5
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91429945"
---
# <a name="customizing-a-tables-appearance-in-xamarinios"></a>Xamarin でのテーブルの外観のカスタマイズ

テーブルの外観を変更する最も簡単な方法は、別のセルスタイルを使用することです。 のメソッドで各セルを作成するときに使用されるセルスタイルを変更でき `UITableViewSource` `GetCell` ます。

## <a name="cell-styles"></a>セルのスタイル

次の4つの組み込みスタイルがあります。

- **Default** –をサポート `UIImageView` します。
- **サブタイトル** –とのサブタイトルをサポートし `UIImageView` ます。
- **Value1** –右にアラインされたサブタイトルは、をサポート `UIImageView` します。
- **Value2** –タイトルは右揃えになり、サブタイトルは左揃えになります (ただし、イメージは含まれません)。

これらのスクリーンショットは、各スタイルの表示方法を示しています。

 [![これらのスクリーンショットは、各スタイルの表示方法を示しています。](customizing-table-appearance-images/image7.png)](customizing-table-appearance-images/image7.png#lightbox)

サンプルの **Celldefaulttable** には、これらの画面を生成するためのコードが含まれています。 セルスタイルは、次のようにコンストラクターで設定され `UITableViewCell` ます。

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

[サポートされている](xref:UIKit.UITableViewCell) セルスタイルのプロパティは、次のように設定できます。

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## <a name="accessories"></a>Accessories

セルでは、次のアクセサリをビューの右側に追加できます。

- **チェックマーク** –テーブルの複数選択を示すために使用できます。
- **[入力] ボタン** –セルの残りの部分とは無関係にタッチに応答します。これにより、別の関数を実行してセル自体に触れることができます (たとえば、ポップアップやスタックの一部ではない新しいウィンドウを開くなど `UINavigationController` )。
- **DisclosureIndicator** –通常、セルをタッチすると別のビューが開くことを示すために使用します。
- **DetailDisclosureButton** –との組み合わせ `DetailButton` `DisclosureIndicator` 。

次のようになります。

 [![サンプルのアクセサリ](customizing-table-appearance-images/image8.png)](customizing-table-appearance-images/image8.png#lightbox)

これらのアクセサリのいずれかを表示するには、 `Accessory` メソッドでプロパティを設定し `GetCell` ます。

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

`DetailButton`またはが表示されたら、を `DetailDisclosureButton` オーバーライドし `AccessoryButtonTapped` て、操作が行われたときにアクションを実行する必要もあります。

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

サンプル **CellAccessoryTable** は、アクセサリを使用した例を示しています。

## <a name="cell-separators"></a>セル区切り記号

セル区切り記号はテーブルを区切るために使用されるテーブルセルです。 これらのプロパティは、テーブルに設定されます。

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

また、区切り記号にぼかしまたは vibrancy 効果を追加することもできます。

```csharp
// blur effect
TableView.SeparatorEffect =
    UIBlurEffect.FromStyle(UIBlurEffectStyle.Dark);

//vibrancy effect
var effect = UIBlurEffect.FromStyle(UIBlurEffectStyle.Light);
TableView.SeparatorEffect = UIVibrancyEffect.FromBlurEffect(effect);
```

区切り記号には、インセットを含めることもできます。

```csharp
TableView.SeparatorInset.InsetRect(new CGRect(4, 4, 150, 2));
```

## <a name="creating-custom-cell-layouts"></a>カスタムセルレイアウトの作成

テーブルの視覚スタイルを変更するには、表示するカスタムセルを指定する必要があります。 カスタムセルの色とコントロールのレイアウトは異なる場合があります。

CellCustomTable の例では、 `UITableViewCell` のカスタムレイアウトを定義するサブクラスを実装して `UILabel` おり、 `UIImage` フォントと色が異なるを使用しています。 結果として得られるセルは次のようになります。

 [![カスタムセルレイアウト](customizing-table-appearance-images/image9.png)](customizing-table-appearance-images/image9.png#lightbox)

カスタムセルクラスは、次の3つのメソッドのみで構成されます。

- **コンストラクター** – UI コントロールを作成し、カスタムスタイルプロパティを設定します (例: フォントフェイス、サイズ、および色)。
- **UpdateCell** –  `UITableView.GetCell` セルのプロパティを設定するために使用するのメソッド。
- **Layoutsubviews** ビュー– UI コントロールの位置を設定します。 この例では、すべてのセルに同じレイアウトが使用されていますが、表示されるコンテンツによっては、より複雑なセル (特にサイズが変化する) でレイアウト位置が異なる場合があります。

**Cellcustomtable > CustomVegeCell.cs**の完全なサンプルコードは次のとおりです。

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

`GetCell`カスタムセルを作成するには、のメソッドを `UITableViewSource` 変更する必要があります。

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

- [WorkingWithTables (サンプル)](/samples/xamarin/ios-samples/workingwithtables)