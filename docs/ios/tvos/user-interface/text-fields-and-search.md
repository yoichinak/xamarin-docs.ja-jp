---
title: Xamarin での tvOS Text フィールドと検索フィールドの操作
description: このドキュメントでは、Xamarin でビルドされた tvOS アプリでテキストフィールドと検索フィールドを操作する方法について説明します。 テキストフィールドと検索フィールドの概要を説明し、キーボード、ストーリーボードの統合、検索データモデルなどについて説明します。
ms.prod: xamarin
ms.assetid: 9EE63CA6-2F31-4EE0-AAE5-82E18CFAC06C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: b1f4712e87762acb81a366700174db33e0c557bf
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/03/2019
ms.locfileid: "70226688"
---
# <a name="working-with-tvos-text-and-search-fields-in-xamarin"></a>Xamarin での tvOS Text フィールドと検索フィールドの操作

必要に応じて、tvOS アプリはテキストフィールドとスクリーンキーボードを使用して、ユーザー (ユーザー Id やパスワードなど) から小さなテキストを要求できます。

[![](text-fields-and-search-images/intro01.png "サンプル検索フィールド")](text-fields-and-search-images/intro01.png#lightbox)

必要に応じて、検索フィールドを使用して、アプリのコンテンツのキーワード検索機能を提供することができます。

[![](text-fields-and-search-images/intro02.png "検索結果のサンプル")](text-fields-and-search-images/intro02.png#lightbox)

このドキュメントでは、tvOS アプリでのテキストフィールドと検索フィールドの操作の詳細について説明します。

<a name="About-Text-and-Search-Fields" />

## <a name="about-text-and-search-fields"></a>テキストフィールドと検索フィールドについて

前述のように、必要に応じて、tvOS は1つまたは複数のテキストフィールドを提示して、画面上 (ユーザーがインストールした tvOS のバージョンに応じて、オプションの bluetooth キーボード) を使用してユーザーから少量のテキストを収集できます。

また、アプリがユーザーに大量のコンテンツ (音楽、映画、画像のコレクションなど) を提示している場合は、ユーザーが少量のテキストを入力して使用可能な項目の一覧をフィルター処理できるようにする検索フィールドを含めることもできます。

<a name="Text-Fields" />

## <a name="text-fields"></a>テキスト フィールド

TvOS では、テキストフィールドは、ユーザーがクリックしたときにスクリーンキーボードを起動する、固定高さの角の丸い角の入力ボックスとして表示されます。

[![](text-fields-and-search-images/text01.png "TvOS のテキストフィールド")](text-fields-and-search-images/text01.png#lightbox)

ユーザーが特定のテキストフィールドに[フォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)を移動すると、拡大して詳細な影が表示されます。 テキストフィールドは、フォーカスがあるときに他の UI 要素と重複する可能性があるため、ユーザーインターフェイスをデザインするときはこの点に注意する必要があります。

Apple には、テキストフィールドを操作するための次のような推奨事項があります。

- **テキスト入力を控えめに使用する**-スクリーンキーボードの性質上、テキストの長いセクションの入力、または複数のテキストフィールドの入力は、ユーザーにとって面倒です。 より良い解決策は、選択リストまたは[ボタン](~/ios/tvos/user-interface/buttons.md)を使用してテキストエントリの量を制限することです。
- **[ヒントを使用して目的を伝える]** : 空の場合は、プレースホルダー "ヒント" を表示できます。 必要に応じて、ヒントを使用して、別のラベルではなくテキストフィールドの目的を記述します。
- **適切な既定のキーボードの種類を選択し**ます。 tvOS には、テキストフィールドに指定できるさまざまな用途に特化したキーボードの種類が用意されています。 たとえば、電子メールアドレスのキーボードは、ユーザーが最近入力したアドレスの一覧から選択できるようにすることで、入力を簡単にすることができます。
- **必要に応じて、セキュリティで保護されたテキストフィールドを使用**します。セキュリティで保護されたテキストフィールドは、(実際の文字ではなく) ドットで入力された文字を示します。 パスワードなどの機密情報を収集するときは、常にセキュリティで保護されたテキストフィールドを使用します。

<a name="Keyboards" />

## <a name="keyboards"></a>キーボード

ユーザーがユーザーインターフェイスのテキストフィールドをクリックするたびに、線形のスクリーンキーボードが表示されます。 ユーザーは、 [Siri リモート](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)のタッチ画面を使用して、キーボードから個々の文字を選択し、要求された情報を入力します。

[![](text-fields-and-search-images/keyboard01.png "Siri リモートキーボード")](text-fields-and-search-images/keyboard01.png#lightbox)

現在のビューに複数のテキストフィールドがある場合は、[**次**へ] ボタンが自動的に表示され、ユーザーは次のテキストフィールドに移動します。 最後のテキストフィールドに **[完了]** ボタンが表示され、テキスト入力が終了して、前の画面にユーザーが返されます。

ユーザーはいつでも、Siri リモコンの**メニュー**ボタンを押してテキスト入力を終了し、前の画面に戻ることができます。

Apple には、スクリーンキーボードでの作業に関して次のような推奨事項があります。

- **適切な既定のキーボードの種類を選択し**ます。 tvOS には、テキストフィールドに指定できるさまざまな用途に特化したキーボードの種類が用意されています。 たとえば、電子メールアドレスのキーボードは、ユーザーが最近入力したアドレスの一覧から選択できるようにすることで、入力を簡単にすることができます。
- **必要に応じて、キーボードアクセサリビューを使用**します。常に表示される標準情報に加えて、オプションのアクセサリビュー (画像やラベルなど) をスクリーンキーボードに追加して、テキスト入力の目的を明確にしたり、サポートしたりすることができます。必要な情報を入力するユーザー。

スクリーンキーボードの操作の詳細については、Apple の[Uikeyboard type](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITextInputTraits_Protocol/index.html#//apple_ref/c/tdef/UIKeyboardType)、[キーボードの管理](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1)、[データ入力用のカスタムビュー](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html#//apple_ref/doc/uid/TP40009542-CH12-SW1) 、および[iOS ドキュメントへのテキストプログラミングガイド](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html)に関するページを参照してください。

<a name="Search" />

## <a name="search"></a>検索

検索フィールドには、ユーザーがキーボードの下に表示される項目のコレクションをフィルター処理できるようにする、テキストフィールドとスクリーンキーボードを提供する特殊な画面が用意されています。

[![](text-fields-and-search-images/search01.png "検索結果のサンプル")](text-fields-and-search-images/search01.png#lightbox)

ユーザーが検索フィールドに文字を入力すると、以下の結果には検索結果が自動的に反映されます。 ユーザーはいつでも、結果にフォーカスを移動して、表示される項目のいずれかを選択できます。

Apple では、検索フィールドの操作に関して次のような推奨事項があります。

- **最近の検索を提供**する-Siri リモートでテキストを入力すると、ユーザーが検索要求を繰り返す傾向があるため、最近の検索結果のセクションをキーボード領域の下の現在の結果の前に追加することを検討してください。
- **可能な場合は、結果の数を制限**します。ユーザーが解析したり移動したりすることが困難になる可能性があるため、返される結果の数を制限することを検討してください。
- 必要に応じて **、検索結果フィルターを指定**します。アプリによって提供されるコンテンツ自体が使用できる場合は、ユーザーが返された検索結果をさらにフィルター処理するために、スコープバーを追加することを検討してください。

詳細については、「Apple の[Uisearchcontroller クラスのリファレンス](https://developer.apple.com/library/tvos/documentation/UIKit/Reference/UISearchController/index.html)」を参照してください。

<a name="Working-with-Text-Fields" />

## <a name="working-with-text-fields"></a>テキストフィールドの操作

TvOS アプリでテキストフィールドを操作する最も簡単な方法は、iOS Designer を使用してユーザーインターフェイスのデザインに追加することです。

次の手順で行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**で、 `Main.storyboard`ファイルをダブルクリックして編集用に開きます。
1. 1つまたは複数の**テキストフィールド**をドラッグして、デザインサーフェイスをビューに表示します。

    [![](text-fields-and-search-images/text02.png "テキストフィールド")](text-fields-and-search-images/text02.png#lightbox)
1. **テキストフィールド**を選択し、 **Properties Pad**の **[ウィジェット]** タブにそれぞれ一意の**名前**を付けます。

    [![](text-fields-and-search-images/text03.png "Properties Pad の [ウィジェット] タブ")](text-fields-and-search-images/text03.png#lightbox)
1. **[テキストフィールド]** セクションでは、**プレースホルダー**ヒントや既定**値**などの要素を定義できます。

    [![](text-fields-and-search-images/text04.png "テキストフィールドセクション")](text-fields-and-search-images/text04.png#lightbox)
1. 下にスクロールして**スペルチェック**、**大文字小文字**、キーボードの既定の**種類**などのプロパティを定義します。

    [![](text-fields-and-search-images/text05.png "スペルチェック、大文字小文字、および既定のキーボードの種類")](text-fields-and-search-images/text05.png#lightbox)
1. 変更内容をストーリーボードに保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューション エクスプローラー**で `Main.storyboard` ファイルをダブルクリックして、編集用に開きます。
1. 1つまたは複数の**テキストフィールド**をドラッグして、デザインサーフェイスをビューに表示します。

    [![](text-fields-and-search-images/text02-vs.png "テキストフィールド")](text-fields-and-search-images/text02-vs.png#lightbox)
1. **テキストフィールド**を選択し、**プロパティエクスプローラー**の **[ウィジェット]** タブにそれぞれ一意の**名前**を付けます。

    [![](text-fields-and-search-images/text03-vs.png "[ウィジェット] タブ")](text-fields-and-search-images/text03-vs.png#lightbox)
1. **[テキストフィールド]** セクションでは、**プレースホルダー**ヒントや既定**値**などの要素を定義できます。

    [![](text-fields-and-search-images/text04-vs.png "テキストフィールドセクション")](text-fields-and-search-images/text04-vs.png#lightbox)
1. 下にスクロールして**スペルチェック**、**大文字小文字**、キーボードの既定の**種類**などのプロパティを定義します。

    [![](text-fields-and-search-images/text05-vs.png "スペルチェック、大文字小文字、および既定のキーボードの種類")](text-fields-and-search-images/text05-vs.png#lightbox)
1. 変更内容をストーリーボードに保存します。

-----

コードでは、 `Text`プロパティを使用してテキストフィールドの値を取得または設定できます。

```csharp
Console.WriteLine ("User ID {0} and Password {1}", UserId.Text, Password.Text);
```

必要に応じて、 `Started`および`Ended`テキストフィールドイベントを使用して、テキストエントリの開始と終了に応答できます。

<a name="Working-with-Search-Fields" />

## <a name="working-with-search-fields"></a>検索フィールドの操作

TvOS アプリで検索フィールドを操作する最も簡単な方法は、インターフェイスデザイナーを使用してそれらをユーザーインターフェイスのデザインに追加することです。

次の手順で行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**で、 `Main.storyboard`ファイルをダブルクリックして編集用に開きます。
1. 新しいコレクションビューコントローラーをストーリーボードにドラッグして、ユーザーの検索結果を表示します。

    [![](text-fields-and-search-images/search02.png "コレクションビューコントローラー")](text-fields-and-search-images/search02.png#lightbox)
1. **Properties Pad**の **[ウィジェット**] タブで、 `SearchResultsViewController` **クラス**と`SearchResults` **ストーリーボード ID**にを使用します。

    [![](text-fields-and-search-images/search03.png "[ウィジェット] タブ")](text-fields-and-search-images/search03.png#lightbox)
1. デザイン画面で**セルプロトタイプ**を選択します。
1. **プロパティエクスプローラー**の **[ウィジェット]** タブで、 `SearchResultCell` **クラス** `ImageCell`にを、**識別子**にを使用します。

    [![](text-fields-and-search-images/search04.png "[ウィジェット] タブ")](text-fields-and-search-images/search04.png#lightbox)
1. **セルプロトタイプ**のデザインをレイアウトし、**プロパティエクスプローラー**の **[ウィジェット]** タブで、各要素を一意の**名前**で公開します。

    [![](text-fields-and-search-images/search05.png "レイアウト (セルプロトタイプのデザインを)")](text-fields-and-search-images/search05.png#lightbox)
1. 変更内容をストーリーボードに保存します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **ソリューション エクスプローラー**で `Main.storyboard` ファイルをダブルクリックして、編集用に開きます。
1. 新しいコレクションビューコントローラーをストーリーボードにドラッグして、ユーザーの検索結果を表示します。

    [![](text-fields-and-search-images/seach02-vs.png "コレクションビューコントローラー")](text-fields-and-search-images/seach02-vs.png#lightbox)
1. **プロパティエクスプローラー**の **[ウィジェット]** タブで、 `SearchResultsViewController` **クラス**と`SearchResults` **ストーリーボード ID**にを使用します。

    [![](text-fields-and-search-images/search03-vs.png "[ウィジェット] タブ")](text-fields-and-search-images/search03-vs.png#lightbox)
1. デザイン画面で**セルプロトタイプ**を選択します。
1. **プロパティエクスプローラー**の **[ウィジェット]** タブで、 `SearchResultCell` **クラス** `ImageCell`にを、**識別子**にを使用します。

    [![](text-fields-and-search-images/search04-vs.png "[ウィジェット] タブ")](text-fields-and-search-images/search04-vs.png#lightbox)
1. **セルプロトタイプ**のデザインをレイアウトし、**プロパティエクスプローラー**の **[ウィジェット]** タブで、各要素を一意の**名前**で公開します。

    [![](text-fields-and-search-images/search05-vs.png "レイアウト (セルプロトタイプのデザインを)")](text-fields-and-search-images/search05-vs.png#lightbox)
1. 変更内容をストーリーボードに保存します。

-----

<a name="Provide-a-Data-Model" />

### <a name="provide-a-data-model"></a>データモデルを提供する

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

次に、ユーザーが検索する結果のデータモデルとして機能するクラスを指定する必要があります。 **ソリューションエクスプローラー**で、プロジェクト名を右クリックし、[新しいファイルの**追加** >  **...** ] を選択します。一般に空のクラスと名前を指定します。 >   > 

[![](text-fields-and-search-images/search06.png "空のクラスを選択し、名前を指定します")](text-fields-and-search-images/search06.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

次に、ユーザーが検索する結果のデータモデルとして機能するクラスを指定する必要があります。 **ソリューションエクスプローラー**で、プロジェクト名を右クリックし、[**新しい項目**の**追加** > ] を選択します。 Apple のその他の > クラスと名前を指定します。  >  > 

[![](text-fields-and-search-images/search06-vs.png "クラスを選択して名前を指定します")](text-fields-and-search-images/search06-vs.png#lightbox)

-----

たとえば、ユーザーがタイトルとキーワードで画像のコレクションを検索できるようにするアプリは、次のようになります。

```csharp
using System;
using Foundation;

namespace tvText
{
    public class PictureInformation : NSObject
    {
        #region Computed Properties
        public string Title { get; set;}
        public string ImageName { get; set;}
        public string Keywords { get; set;}
        #endregion

        #region Constructors
        public PictureInformation (string title, string imageName, string keywords)
        {
            // Initialize
            this.Title = title;
            this.ImageName = imageName;
            this.Keywords = keywords;
        }
        #endregion
    }
}
```

<a name="The-Collection-View-Cell" />

### <a name="the-collection-view-cell"></a>コレクションビューセル

データモデルを配置した状態で、**プロトタイプセル**(`SearchResultViewCell.cs`) を編集し、次のように表示されるようにします。

```csharp
using Foundation;
using System;
using UIKit;

namespace tvText
{
    public partial class SearchResultViewCell : UICollectionViewCell
    {
        #region Private Variables
        private PictureInformation _pictureInfo = null;
        #endregion

        #region Computed Properties
        public PictureInformation PictureInfo {
            get { return _pictureInfo; }
            set {
                _pictureInfo = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            UpdateUI ();
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Anything to process?
            if (PictureInfo == null) return;

            try {
                Picture.Image = UIImage.FromBundle (PictureInfo.ImageName);
                Picture.AdjustsImageWhenAncestorFocused = true;
                Title.Text = PictureInfo.Title;
                TextColor = UIColor.LightGray;
            } catch {
                // Ignore errors if view isn't fully loaded
            }
        }
        #endregion
    }

}
```

メソッドは、プロパティが更新されるたびに、名前付き UI 要素の`PictureInfo` **画像情報**項目 (プロパティ) の個々のフィールドを表示するために使用されます。 `UpdateUI` たとえば、画像に関連付けられている画像やタイトルなどです。

<a name="The-Collection-View-Controller" />

### <a name="the-collection-view-controller"></a>コレクションビューコントローラー

次に、検索結果コレクションビューコントローラー (`SearchResultsViewController.cs`) を編集して、次のように表示します。

```csharp
using Foundation;
using System;
using UIKit;
using System.Collections.Generic;

namespace tvText
{
    public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
    {
        #region Constants
        public const string CellID = "ImageCell";
        #endregion

        #region Private Variables
        private string _searchFilter = "";
        #endregion

        #region Computed Properties
        public List<PictureInformation> AllPictures { get; set;}
        public List<PictureInformation> FoundPictures { get; set; }
        public string SearchFilter {
            get { return _searchFilter; }
            set {
                _searchFilter = value.ToLower();
                FindPictures ();
                CollectionView?.ReloadData ();
            }
        }
        #endregion

        #region Constructors
        public SearchResultsViewController (IntPtr handle) : base (handle)
        {
            // Initialize
            this.AllPictures = new List<PictureInformation> ();
            this.FoundPictures = new List<PictureInformation> ();
            PopulatePictures ();
            FindPictures ();

        }
        #endregion

        #region Private Methods
        private void PopulatePictures ()
        {
            // Clear list
            AllPictures.Clear ();

            // Add images
            AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
            AllPictures.Add (new PictureInformation ("Cheese Plate", "CheesePlate", "cheese,plate,bread"));
            AllPictures.Add (new PictureInformation ("Coffee House", "CoffeeHouse", "coffee,people,menu,restaurant,cafe"));
            AllPictures.Add (new PictureInformation ("Computer and Expresso", "ComputerExpresso", "computer,coffee,expresso,phone,notebook"));
            AllPictures.Add (new PictureInformation ("Hamburger", "Hamburger", "meat,bread,cheese,tomato,pickle,lettus"));
            AllPictures.Add (new PictureInformation ("Lasagna Dinner", "Lasagna", "salad,bread,plate,lasagna,pasta"));
            AllPictures.Add (new PictureInformation ("Expresso Meeting", "PeopleExpresso", "people,bag,phone,expresso,coffee,table,tablet,notebook"));
            AllPictures.Add (new PictureInformation ("Soup and Sandwich", "SoupAndSandwich", "soup,sandwich,bread,meat,plate,tomato,lettus,egg"));
            AllPictures.Add (new PictureInformation ("Morning Coffee", "TabletCoffee", "tablet,person,man,coffee,magazine,table"));
            AllPictures.Add (new PictureInformation ("Evening Coffee", "TabletMagCoffee", "tablet,magazine,coffee,table"));
        }

        private void FindPictures ()
        {
            // Clear list
            FoundPictures.Clear ();

            // Scan each picture for a match
            foreach (PictureInformation picture in AllPictures) {
                if (SearchFilter == "") {
                    // If no search term, everything matches
                    FoundPictures.Add (picture);
                } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
                    // If the search term is in the title or keywords, we've found a match
                    FoundPictures.Add (picture);
                }
            }
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            // Only one section in this collection
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            // Return the number of matching pictures
            return FoundPictures.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // Get a new cell and return it
            var cell = collectionView.DequeueReusableCell (CellID, indexPath);
            return (UICollectionViewCell)cell;
        }

        public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
        {
            // Grab the cell
            var currentCell = cell as SearchResultViewCell;
            if (currentCell == null)
                throw new Exception ("Expected to display a `SearchResultViewCell`.");

            // Display the current picture info in the cell
            var item = FoundPictures [indexPath.Row];
            currentCell.PictureInfo = item;
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            // If this Search Controller was presented as a modal view, close
            // it before continuing
            // DismissViewController (true, null);

            // Grab the picture being selected and report it
            var picture = FoundPictures [indexPath.Row];
            Console.WriteLine ("Selected: {0}", picture.Title);
        }

        public void UpdateSearchResultsForSearchController (UISearchController searchController)
        {
            // Save the search filter and update the Collection View
            SearchFilter = searchController.SearchBar.Text ?? string.Empty;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
            if (previousItem != null) {
                UIView.Animate (0.2, () => {
                    previousItem.TextColor = UIColor.LightGray;
                });
            }

            var nextItem = context.NextFocusedView as SearchResultViewCell;
            if (nextItem != null) {
                UIView.Animate (0.2, () => {
                    nextItem.TextColor = UIColor.Black;
                });
            }
        }
        #endregion
    }
}
```

まず、 `IUISearchResultsUpdating`ユーザーによって更新される検索コントローラーフィルターを処理するために、クラスにインターフェイスを追加します。

```csharp
public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
```

定数は、後でコレクションコントローラーが新しいセルを要求したときに使用される**プロトタイプセル**(上のインターフェイスデザイナーで定義されている id に一致する) の id を指定するためにも定義されています。

```csharp
public const string CellID = "ImageCell";
```

検索対象の項目の完全な一覧、検索フィルター用語、およびその用語と一致する項目の一覧に対して、ストレージが作成されます。

```csharp
private string _searchFilter = "";
...

public List<PictureInformation> AllPictures { get; set;}
public List<PictureInformation> FoundPictures { get; set; }
public string SearchFilter {
    get { return _searchFilter; }
    set {
        _searchFilter = value.ToLower();
        FindPictures ();
        CollectionView?.ReloadData ();
    }
}
```

`SearchFilter`が変更されると、一致する項目の一覧が更新され、コレクションビューのコンテンツが再読み込みされます。 ルーチン`FindPictures`は、新しい検索用語に一致する項目を検索します。

```csharp
private void FindPictures ()
{
    // Clear list
    FoundPictures.Clear ();

    // Scan each picture for a match
    foreach (PictureInformation picture in AllPictures) {
        if (SearchFilter == "") {
            // If no search term, everything matches
            FoundPictures.Add (picture);
        } else if (picture.Title.Contains (SearchFilter) || picture.Keywords.Contains (SearchFilter)) {
            // If the search term is in the title or keywords, we've found a match
            FoundPictures.Add (picture);
        }
    }
}
```

ユーザーが検索コントローラー `SearchFilter`でフィルターを変更すると、の値が更新されます (結果コレクションビューが更新されます)。

```csharp
public void UpdateSearchResultsForSearchController (UISearchController searchController)
{
    // Save the search filter and update the Collection View
    SearchFilter = searchController.SearchBar.Text ?? string.Empty;
}
```

メソッド`PopulatePictures`は、最初に使用可能な項目のコレクションを設定します。

```csharp
private void PopulatePictures ()
{
    // Clear list
    AllPictures.Clear ();

    // Add images
    AllPictures.Add (new PictureInformation ("Antipasta Platter","Antipasta","cheese,grapes,tomato,coffee,meat,plate"));
    ...
}
```

この例では、コレクションビューコントローラーが読み込まれるときに、すべてのサンプルデータがメモリ内に作成されています。 実際のアプリでは、このデータはデータベースまたは web サービスから読み取られる可能性があり、Apple TV の制限されたメモリのオーバーランを防ぐために必要なだけです。

メソッド`NumberOfSections` と`GetItemsCount`メソッドは、一致する項目の数を提供します。

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    // Only one section in this collection
    return 1;
}

public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    // Return the number of matching pictures
    return FoundPictures.Count;
}
```

メソッド`GetCell`は、コレクションビューの各項目について`CellID` 、(ストーリーボードで定義されているに基づいて) 新しい**プロトタイプセル**を返します。

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Get a new cell and return it
    var cell = collectionView.DequeueReusableCell (CellID, indexPath);
    return (UICollectionViewCell)cell;
}
```

`WillDisplayCell`メソッドは、セルが表示される前に呼び出され、構成できるようになります。

```csharp
public override void WillDisplayCell (UICollectionView collectionView, UICollectionViewCell cell, NSIndexPath indexPath)
{
    // Grab the cell
    var currentCell = cell as SearchResultViewCell;
    if (currentCell == null)
        throw new Exception ("Expected to display a `SearchResultViewCell`.");

    // Display the current picture info in the cell
    var item = FoundPictures [indexPath.Row];
    currentCell.PictureInfo = item;
}
```

メソッド`DidUpdateFocus`は、結果コレクションビューで項目を強調表示するときに、視覚的なフィードバックをユーザーに提供します。

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as SearchResultViewCell;
    if (previousItem != null) {
        UIView.Animate (0.2, () => {
            previousItem.TextColor = UIColor.LightGray;
        });
    }

    var nextItem = context.NextFocusedView as SearchResultViewCell;
    if (nextItem != null) {
        UIView.Animate (0.2, () => {
            nextItem.TextColor = UIColor.Black;
        });
    }
}
```

最後に、 `ItemSelected`メソッドは、結果コレクションビューの項目を選択するユーザー (siri リモートのタッチサーフェイスをクリック) を処理します。

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // If this Search Controller was presented as a modal view, close
    // it before continuing
    // DismissViewController (true, null);

    // Grab the picture being selected and report it
    var picture = FoundPictures [indexPath.Row];
    Console.WriteLine ("Selected: {0}", picture.Title);
}
```

検索フィールドを、それを呼び出しているビューの上部にモーダルダイアログビューとして表示した場合は`DismissViewController` 、ユーザーが項目を選択したときに、メソッドを使用して検索ビューを閉じます。 この例では、[検索] フィールドは [タブビュー] タブのコンテンツとして表示されるため、ここでは無視されません。

コレクションビューの詳細については、[コレクションビュー](~/ios/tvos/user-interface/collection-views.md)の使用に関するドキュメントを参照してください。

<a name="Presenting the Search Field" />

### <a name="presenting-the-search-field"></a>検索フィールドの表示

TvOS では、検索フィールド (およびそれに関連付けられているスクリーンキーボードと検索結果) をユーザーに提示する主な方法が2つあります。

- **モーダルダイアログビュー** -検索フィールドは、現在のビューに表示できます。また、ビューコントローラーは全画面のモーダルダイアログビューとして表示されます。 これは通常、ユーザーがボタンまたはその他の UI 要素をクリックしたときに実行されます。 ユーザーが検索結果から項目を選択すると、ダイアログは閉じられます。
- **[内容の表示]** -検索フィールドは、特定のビューの直接の部分です。 たとえば、タブビューコントローラーの [検索] タブの内容。

上で指定した画像の検索可能な一覧の例では、検索フィールドは [検索] タブに表示コンテンツとして表示され、[検索] タブビューコントローラーは次のようになります。

```csharp
using System;
using UIKit;

namespace tvText
{
    public partial class SecondViewController : UIViewController
    {
        #region Constants
        public const string SearchResultsID = "SearchResults";
        #endregion

        #region Computed Properties
        public SearchResultsViewController ResultsController { get; set;}
        #endregion

        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        public void ShowSearchController ()
        {
            // Build an instance of the Search Results View Controller from the Storyboard
            ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
            if (ResultsController == null)
                throw new Exception ("Unable to instantiate a SearchResultsViewController.");

            // Create an initialize a new search controller
            var searchController = new UISearchController (ResultsController) {
                SearchResultsUpdater = ResultsController,
                HidesNavigationBarDuringPresentation = false
            };

            // Set any required search parameters
            searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

            // The Search Results View Controller can be presented as a modal view
            // PresentViewController (searchController, true, null);

            // Or in the case of this sample, the Search View Controller is being
            // presented as the contents of the Search Tab directly. Use either one
            // or the other method to display the Search Controller (not both).
            var container = new UISearchContainerViewController (searchController);
            var navController = new UINavigationController (container);
            AddChildViewController (navController);
            View.Add (navController.View);
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // If the Search Controller is being displayed as the content
            // of the search tab, include it here.
            ShowSearchController ();
        }

        public override void ViewDidAppear (bool animated)
        {
            base.ViewDidAppear (animated);

            // If the Search Controller is being presented as a modal view,
            // call it here to display it over the contents of the Search
            // tab.
            // ShowSearchController ();
        }
        #endregion
    }
}
```

まず、インターフェイスデザイナーで検索結果コレクションビューコントローラーに割り当てられた**ストーリーボード識別子**と一致する定数を定義します。

```csharp
public const string SearchResultsID = "SearchResults";
```

次に、 `ShowSearchController`メソッドは新しい検索ビューコレクションコントローラーを作成し、必要なものを表示します。

```csharp
public void ShowSearchController ()
{
    // Build an instance of the Search Results View Controller from the Storyboard
    ResultsController = Storyboard.InstantiateViewController (SearchResultsID) as SearchResultsViewController;
    if (ResultsController == null)
        throw new Exception ("Unable to instantiate a SearchResultsViewController.");

    // Create an initialize a new search controller
    var searchController = new UISearchController (ResultsController) {
        SearchResultsUpdater = ResultsController,
        HidesNavigationBarDuringPresentation = false
    };

    // Set any required search parameters
    searchController.SearchBar.Placeholder = "Enter keyword (e.g. coffee)";

    // The Search Results View Controller can be presented as a modal view
    // PresentViewController (searchController, true, null);

    // Or in the case of this sample, the Search View Controller is being
    // presented as the contents of the Search Tab directly. Use either one
    // or the other method to display the Search Controller (not both).
    var container = new UISearchContainerViewController (searchController);
    var navController = new UINavigationController (container);
    AddChildViewController (navController);
    View.Add (navController.View);
}
```

上のメソッド`SearchResultsViewController`では、がストーリーボードからインスタンス化されると、 `UISearchController`検索フィールドとスクリーンキーボードをユーザーに表示する新しいが作成されます。 (で`SearchResultsViewController`定義されているように) 検索結果のコレクションがこのキーボードに表示されます。

次に`SearchBar` 、は**プレースホルダー**ヒントなどの情報で構成されます。 これにより、処理中の検索の種類に関する情報がユーザーに提供されます。

その後、次の2つの方法のいずれかで、検索フィールドがユーザーに表示されます。

- **モーダルダイアログビュー** -既存`PresentViewController`のビューの全画面表示で検索を表示するために、メソッドが呼び出されます。
- [**内容**の表示`UISearchContainerViewController` ]-検索コントローラーを含むが作成されます。 検索コンテナーを格納するための`AddChildViewController (navController)` `View.Add (navController.View)`が作成された後、ナビゲーションコントローラーがビューコントローラーに追加され、ビューが表示`UINavigationController`されます。

最後に、プレゼンテーションの種類`ViewDidLoad`に基づいて、 `ShowSearchController`メソッドまたは`ViewDidAppear`メソッドがメソッドを呼び出して、検索をユーザーに提示します。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // If the Search Controller is being displayed as the content
    // of the search tab, include it here.
    ShowSearchController ();
}

public override void ViewDidAppear (bool animated)
{
    base.ViewDidAppear (animated);

    // If the Search Controller is being presented as a modal view,
    // call it here to display it over the contents of the Search
    // tab.
    // ShowSearchController ();
}
```

ユーザーがアプリを実行し、[検索] タブを選択すると、フィルター処理されていない項目の完全な一覧がユーザーに表示されます。

[![](text-fields-and-search-images/intro02.png "既定の検索結果")](text-fields-and-search-images/intro02.png#lightbox)

ユーザーが検索語句の入力を開始すると、結果の一覧がその用語によってフィルター処理され、自動的に更新されます。

[![](text-fields-and-search-images/intro03.png "フィルター処理された検索結果")](text-fields-and-search-images/intro03.png#lightbox)

ユーザーはいつでも、検索結果の項目にフォーカスを切り替え、Siri リモートのタッチ画面をクリックして選択できます。

<a name="Summary" />

## <a name="summary"></a>Summary

この記事では、tvOS アプリ内のテキストフィールドと検索フィールドのデザインと操作について説明しました。 ここでは、インターフェイスデザイナーでテキストを作成し、コレクションの内容を検索する方法を示しました。この例では、tvOS で検索フィールドをユーザーに提示する2つの方法について説明しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
