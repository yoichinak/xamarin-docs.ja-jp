---
title: テキストと検索のフィールドの操作
description: この記事では、設計と Xamarin.tvOS アプリ内でテキストと検索のフィールドの操作について説明します。
ms.prod: xamarin
ms.assetid: 9EE63CA6-2F31-4EE0-AAE5-82E18CFAC06C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 220c6e3d1c6f358c67a2f596c977f4d2132298a8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-text-and-search-fields"></a>テキストと検索のフィールドの操作

_この記事では、設計と Xamarin.tvOS アプリ内でテキストと検索のフィールドの操作について説明します。_



Xamarin.tvOS アプリが (ユーザー Id とパスワード) などのユーザーからの一部分を要求できます必要に応じて、テキスト フィールドを使用して、スクリーン キーボードします。

[![](text-fields-and-search-images/intro01.png "サンプルの検索 フィールド")](text-fields-and-search-images/intro01.png#lightbox)

必要に応じて、検索フィールドを使用して、アプリのコンテンツのキーワード検索機能を指定できます。

[![](text-fields-and-search-images/intro02.png "サンプルの検索結果")](text-fields-and-search-images/intro02.png#lightbox)

このドキュメントでは、テキストと検索のフィールド Xamarin.tvOS アプリで操作の詳細を説明します。

<a name="About-Text-and-Search-Fields" />

## <a name="about-text-and-search-fields"></a>テキストと検索のフィールドについて

説明したように、必要な場合、Xamarin.tvOS できますがあれば、少量のテキストを使用して、ユーザーから収集する 1 つまたは複数のテキスト フィールド、画面に表示される (または、ユーザーがインストールされている tvOS のバージョンに応じて、省略可能な bluetooth キーボード)。 

さらに、アプリでは、大量のコンテンツを表示 (音楽、映画画像コレクションなど) のユーザーに、可能性がある場合は、ユーザーに利用可能な項目の一覧を絞り込むテキストを少量の入力を許可する検索のフィールドが含まれます。

<a name="Text-Fields" />

## <a name="text-fields"></a>テキスト フィールド

TvOS、テキスト フィールドが表示され、として、固定サイズ、丸め、コーナーのエントリのボックスが表示されますが、スクリーン キーボードのユーザーがクリックしたときに。

[![](text-fields-and-search-images/text01.png "テキスト フィールドで tvOS")](text-fields-and-search-images/text01.png#lightbox)

ユーザーが移動すると[フォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)特定のテキスト フィールドに拡大するにつれてされディープ シャドウを表示します。 これに留意してください、ユーザー インターフェイスのデザイン時テキスト フィールドにはフォーカス設定時に他の UI 要素は重複させるようにする必要があります。

Apple では、テキスト フィールドの操作の次の方法があります。

- **エントリを多用しないようにテキスト**- の性質があるため、スクリーン キーボード、長いテキストのセクションを入力するか、複数のテキスト フィールドに入力がユーザーに面倒です。 良いソリューションは、選択リストを使用してテキスト入力の量を制限するまたは[ボタン](~/ios/tvos/user-interface/buttons.md)です。
- **目的の通信にヒントを使用して**-テキスト フィールドが空の場合にプレース ホルダー「ヒント」を表示します。 該当する場合、ヒントを使用して、別のラベルではなく、テキスト フィールドの用途について説明します。
- **適切な既定のキーボードの種類を選択して**-tvOS、テキスト フィールドに対して指定できるいくつかの異なる、特化したキーボード型を提供します。 たとえば、電子メール アドレスのキーボードが簡単になりますエントリにより、ユーザーが最後に入力したアドレスの一覧から選択します。
- **適切な場合は、セキュリティで保護されたテキスト フィールドを使用して**-A セキュリティで保護されたテキスト フィールドが (実際の文字) ではなく、ドットで入力した文字を表示します。 常にパスワードなどの機密情報を収集するときは、セキュリティで保護されたテキスト フィールドを使用します。

<a name="Keyboards" />

## <a name="keyboards"></a>キーボード

ユーザーがクリックすると、ユーザー インターフェイスは、線形のテキスト フィールドにスクリーン キーボードが表示されます。 ユーザーは、タッチ画面を使用して、 [Siri リモート](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)をキーボードから個々 の文字を選択し、必要な情報を入力します。

[![](text-fields-and-search-images/keyboard01.png "Siri リモート キーボード")](text-fields-and-search-images/keyboard01.png#lightbox)

現在のビューでは、1 つ以上のテキスト フィールドがある場合、**次**ボタンがクリックすると、次のテキスト フィールドに自動的に表示されます。 A**完了**はテキスト入力を終了し、前の画面にユーザーを返される最後のテキスト フィールドのボタンが表示されます。 

いつでも、ユーザーがキーを押しても、**メニュー**テキスト入力を終了し、もう一度前の画面に戻ります Siri リモコンのボタンをクリックします。

Apple には、次の提案を扱うのためスクリーン キーボード。

- **適切な既定のキーボードの種類を選択して**-tvOS、テキスト フィールドに対して指定できるいくつかの異なる、特化したキーボード型を提供します。 たとえば、電子メール アドレスのキーボードが簡単になりますエントリにより、ユーザーが最後に入力したアドレスの一覧から選択します。
- **必要に応じて、ビューを使用してキーボードのアクセサリ**- に加え、標準には、常に表示される、省略可能なのアクセサリ ビュー (イメージやラベル) などの情報を追加できる、テキスト入力の目的を明確にスクリーン キーボード必要な情報を入力するユーザーを支援します。

操作の詳細については、スクリーン キーボード、Apple を参照してください[UIKeyboardType](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITextInputTraits_Protocol/index.html#//apple_ref/c/tdef/UIKeyboardType)、[キーボードを管理する](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1)、[データ入力用のカスタム ビュー](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html#//apple_ref/doc/uid/TP40009542-CH12-SW1)と[IOS にテキスト プログラミング ガイド](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html)ドキュメント。

<a name="Search" />

## <a name="search"></a>検索

検索フィールドがテキスト フィールドを提供する特殊な画面を表示し、スクリーン キーボードをキーボードの下に表示される項目のコレクションをフィルター処理することができます。

[![](text-fields-and-search-images/search01.png "サンプルの検索結果")](text-fields-and-search-images/search01.png#lightbox)

ように、[検索] フィールドに文字を入力すると、次の結果は、検索の結果を自動的に反映されます。 いつでもは、ユーザーは、結果にフォーカスを移動し、表示されている項目のいずれかを選択します。

Apple では、検索のフィールドの操作の次の方法があります。

- **最近実行した検索の提供**のため面倒になる可能性が Siri リモートとテキストを入力して、ユーザーが検索要求を再実行し、キーボードの下の現在の結果の前に最新の検索結果のセクションの追加を検討する傾向があります。
- **可能であれば、結果の数を制限する**のため、項目の大規模な一覧をユーザーが解析、移動し、返される結果の数を制限することを検討するため困難になります。
- **指定して検索結果をフィルター処理、必要に応じて**- 場合は、アプリによって提供されるコンテンツ適している、返される検索結果をさらにフィルター処理できるようにするスコープ バーを追加することを検討してください。

詳細については、Apple を参照してください[UISearchController クラス参照](https://developer.apple.com/library/tvos/documentation/UIKit/Reference/UISearchController/index.html)です。

<a name="Working-with-Text-Fields" />

## <a name="working-with-text-fields"></a>テキスト フィールドの操作

Xamarin.tvOS アプリでは、テキスト フィールドを使用する最も簡単な方法では、iOS デザイナーを使用してユーザー インターフェイスのデザインに追加します。

次の手順で行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **ソリューション パッド**をダブルクリックして、`Main.storyboard`ファイルを開いて編集するファイル。
1. 1 つまたは複数のドラッグ**テキスト フィールド**int ビューに、デザイン画面。 

    [![](text-fields-and-search-images/text02.png "テキスト フィールド")](text-fields-and-search-images/text02.png#lightbox)
1. 選択、**テキスト フィールド**を行い、それぞれ一意な**名前**で、**ウィジェット**のタブ、**プロパティ パッド**: 

    [![](text-fields-and-search-images/text03.png "プロパティのパッドのウィジェット タブ")](text-fields-and-search-images/text03.png#lightbox)
1. **テキスト フィールド** セクションなどの要素を定義することができます、**プレース ホルダー**ヒントと既定**値**: 

    [![](text-fields-and-search-images/text04.png "テキストのフィールド セクション")](text-fields-and-search-images/text04.png#lightbox)
1. スクロール ダウンしてなどのプロパティの定義を**スペル チェック**、**大文字と小文字**と既定**キーボードの種類**: 

    [![](text-fields-and-search-images/text05.png "スペル チェック、大文字と小文字および既定のキーボードの種類")](text-fields-and-search-images/text05.png#lightbox) 
1. ストーリー ボードに変更を保存します。
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. **ソリューション エクスプローラー**で `Main.storyboard` ファイルをダブルクリックして、編集用に開きます。
1. 1 つまたは複数のドラッグ**テキスト フィールド**int ビューに、デザイン画面。 

    [![](text-fields-and-search-images/text02-vs.png "テキスト フィールド")](text-fields-and-search-images/text02-vs.png#lightbox)
1. 選択、**テキスト フィールド**を行い、それぞれ一意な**名前**で、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**: 

    [![](text-fields-and-search-images/text03-vs.png "ウィジェット タブ")](text-fields-and-search-images/text03-vs.png#lightbox)
1. **テキスト フィールド** セクションなどの要素を定義することができます、**プレース ホルダー**ヒントと既定**値**: 

    [![](text-fields-and-search-images/text04-vs.png "テキストのフィールド セクション")](text-fields-and-search-images/text04-vs.png#lightbox)
1. スクロール ダウンしてなどのプロパティの定義を**スペル チェック**、**大文字と小文字**と既定**キーボードの種類**: 

    [![](text-fields-and-search-images/text05-vs.png "スペル チェック、大文字と小文字および既定のキーボードの種類")](text-fields-and-search-images/text05-vs.png#lightbox) 
1. ストーリー ボードに変更を保存します。
    
-----

コードでは、取得または使用して、テキスト フィールドの値を設定することができます、`Text`プロパティ。

```csharp
Console.WriteLine ("User ID {0} and Password {1}", UserId.Text, Password.Text);
```

必要に応じて使用することができます、`Started`と`Ended`テキスト入力を開始および終了に応答するイベントをテキスト フィールド。

<a name="Working-with-Search-Fields" />

## <a name="working-with-search-fields"></a>検索のフィールドの操作

Xamarin.tvOS アプリでは、検索のフィールドを使用する最も簡単な方法では、インターフェイス デザイナーを使用してユーザー インターフェイスのデザインに追加します。

次の手順で行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. **ソリューション パッド**をダブルクリックして、`Main.storyboard`ファイルを開いて編集するファイル。
1. ユーザーの検索の結果を提示するストーリー ボードに新しいコレクション ビュー コント ローラーをドラッグします。 

    [![](text-fields-and-search-images/search02.png "コレクション ビューのコント ローラー")](text-fields-and-search-images/search02.png#lightbox)
1. **ウィジェット**のタブ、**プロパティ パッド**を使用して`SearchResultsViewController`の**クラス**と`SearchResults`の**ストーリー ボード ID**: 

    [![](text-fields-and-search-images/search03.png "ウィジェット タブ")](text-fields-and-search-images/search03.png#lightbox)
1. 選択、**セル プロトタイプ**デザイン サーフェイスです。
1. **ウィジェット**のタブ、**プロパティ エクスプ ローラー**を使用して`SearchResultCell`の**クラス**と`ImageCell`の**識別子**: 

    [![](text-fields-and-search-images/search04.png "ウィジェット タブ")](text-fields-and-search-images/search04.png#lightbox)
1. レイアウトのデザインの**セル プロトタイプ**、一意の各要素を公開および**名前**で、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**: 

    [![](text-fields-and-search-images/search05.png "レイアウト、セルのプロトタイプのデザイン")](text-fields-and-search-images/search05.png#lightbox)
1. ストーリー ボードに変更を保存します。
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. **ソリューション エクスプローラー**で `Main.storyboard` ファイルをダブルクリックして、編集用に開きます。
1. ユーザーの検索の結果を提示するストーリー ボードに新しいコレクション ビュー コント ローラーをドラッグします。 

    [![](text-fields-and-search-images/seach02-vs.png "コレクション ビューのコント ローラー")](text-fields-and-search-images/seach02-vs.png#lightbox)
1. **ウィジェット**のタブ、**プロパティ エクスプ ローラー**を使用して`SearchResultsViewController`の**クラス**と`SearchResults`の**ストーリー ボード ID**: 

    [![](text-fields-and-search-images/search03-vs.png "ウィジェット タブ")](text-fields-and-search-images/search03-vs.png#lightbox)
1. 選択、**セル プロトタイプ**デザイン サーフェイスです。
1. **ウィジェット**のタブ、**プロパティ エクスプ ローラー**を使用して`SearchResultCell`の**クラス**と`ImageCell`の**識別子**: 

    [![](text-fields-and-search-images/search04-vs.png "ウィジェット タブ")](text-fields-and-search-images/search04-vs.png#lightbox)
1. レイアウトのデザインの**セル プロトタイプ**、一意の各要素を公開および**名前**で、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**: 

    [![](text-fields-and-search-images/search05-vs.png "レイアウト、セルのプロトタイプのデザイン")](text-fields-and-search-images/search05-vs.png#lightbox)
1. ストーリー ボードに変更を保存します。
    
-----

<a name="Provide-a-Data-Model" />

### <a name="provide-a-data-model"></a>データ モデルを提供します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

次に、ユーザーを探している結果のデータ モデルとして機能するクラスを提供する必要があります。 **ソリューション エクスプ ローラー**、プロジェクト名を右クリックし **追加** > **新しいファイル.**  > **全般** > **空のクラス**を提供し、**名前**: 

[![](text-fields-and-search-images/search06.png "空のクラスを選択し、名前を指定")](text-fields-and-search-images/search06.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

次に、ユーザーを探している結果のデータ モデルとして機能するクラスを提供する必要があります。 **ソリューション エクスプ ローラー**、プロジェクト名を右クリックし **追加** > **新しい項目の追加.**  >  **Apple** > **その他** > **クラス**を提供し、**名前**: 

[![](text-fields-and-search-images/search06-vs.png "クラスを選択し、名前を指定")](text-fields-and-search-images/search06-vs.png#lightbox)

-----

たとえば、タイトルとキーワードが、画像のコレクションを検索するユーザーを許可するアプリを次のようになります。

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

### <a name="the-collection-view-cell"></a>コレクション ビューのセル

モデルでは、データの場所で、編集、**プロトタイプ セル**(`SearchResultViewCell.cs`) し、外観が次にあります。

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

`UpdateUI`の個別のフィールドの表示に使用されるメソッド、 **PictureInformation**項目 (、`PictureInfo`プロパティ) プロパティを更新するたびに、名前付きの UI 要素。 たとえば、イメージと、画像に関連付けられているタイトル。

<a name="The-Collection-View-Controller" />

### <a name="the-collection-view-controller"></a>コレクション ビューのコント ローラー

結果のコレクション ビューを検索するコント ローラーを次に、編集 (`SearchResultsViewController.cs`)、次のような外観にするとします。

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

最初に、`IUISearchResultsUpdating`インターフェイスがユーザーによって更新されるコント ローラーの検索フィルターを処理するクラスに追加します。

```csharp
public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
```

ID を指定する定数が定義されても、**プロトタイプ セル**(インターフェイス デザイナー上で定義されている ID と一致する) を使用する後で、コレクションのコント ローラーが新しいセルを要求するとき。

```csharp
public const string CellID = "ImageCell";
```

キーワード フィルター、検索対象のアイテムの完全な一覧のストレージが作成され、その用語と一致するアイテムの一覧。

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

ときに、`SearchFilter`が変更されると、一致する項目のリストが更新され、コレクション ビューのコンテンツを再読み込みします。 `FindPictures`ルーチンは、新しい検索用語と一致する項目を検索するを担当します。

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

値、`SearchFilter`が更新されます (これはビューを更新結果コレクション)、ユーザーが検索コント ローラーのフィルターを変更する場合。

```csharp
public void UpdateSearchResultsForSearchController (UISearchController searchController)
{
    // Save the search filter and update the Collection View
    SearchFilter = searchController.SearchBar.Text ?? string.Empty;
}
```

`PopulatePictures`メソッドが最初に利用可能な項目のコレクションを設定します。

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

ここではこの例では、すべてのサンプル データで作成されているメモリ コレクション ビューのコント ローラーを読み込むときにします。 実際のアプリでこのデータが、データベースまたは web サービスから読み取られた可能性し、限られたメモリを Apple TV のオーバーランを防ぐ必要な場合のみです。

`NumberOfSections`と`GetItemsCount`メソッドは、一致した項目の数を提供します。

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

`GetCell`メソッドは、新しいを返します**プロトタイプ セル**(に基づいて、`CellID`上で定義したストーリー ボードの) コレクション ビューの項目ごとに。

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Get a new cell and return it
    var cell = collectionView.DequeueReusableCell (CellID, indexPath);
    return (UICollectionViewCell)cell;
}
```

`WillDisplayCell`構成できるように表示されているセルの前にメソッドが呼び出されます。

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

`DidUpdateFocus`メソッドは、コレクションの結果ビューで項目を強調表示するようユーザーに視覚的なフィードバックを提供します。

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

最後に、`ItemSelected`メソッドは結果コレクション ビューに (Siri リモートとタッチ画面をクリックすると)、項目を選択すると、ユーザーを処理します。

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

[検索] フィールドが表示される場合、モーダル ダイアログ ビューとして (経由で、ビューの最上位の呼び出し)、使用、`DismissViewController`メソッドをユーザーが項目を選択したときの検索ビューを閉じます。 この例では、ここで、消去されていないために、検索フィールドは、タブの表示 タブの内容として表示されます。

コレクション ビューの詳細についてを参照してください、[コレクション ビューを使用する](~/ios/tvos/user-interface/collection-views.md)ドキュメント。

<a name="Presenting the Search Field" />

### <a name="presenting-the-search-field"></a>検索フィールドを表示します。

2 つの主な方法を検索フィールド (およびそれに関連するスクリーン キーボードし、検索結果) tvOS 内のユーザーに提示することができます。 

- **モーダル ダイアログ ビュー** -[検索] フィールドを提示する経由での現在のビューおよび全画面表示のモーダル ダイアログ ビューとしてコント ローラーを表示します。 これは通常、ボタンやその他の UI 要素をクリックすると、ユーザーへの応答で行われます。 ユーザーが検索結果から項目を選択すると、ダイアログが終了します。
- **内容を表示**-[検索] フィールドは、指定されたビューの直接の一部です。 たとえば、としてタブ ビュー コント ローラーの検索 タブの内容。

上記の画像の検索可能な一覧の例では、検索フィールドは、[検索] タブで [内容の表示として表示され、検索] タブのビュー コント ローラーは、次のようになります。

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

最初に、定数が定義されている一致する、**ストーリー ボード識別子**インターフェイス デザイナーで、検索結果のコレクション ビューのコント ローラーに割り当てられました。

```csharp
public const string SearchResultsID = "SearchResults";
```

次に、`ShowSearchController`メソッドは、新しい検索ビュー コレクション コント ローラーとそれを必要とされた表示を作成します。

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

メソッドでは、上、1 回、 `SearchResultsViewController` 、ストーリー ボードからインスタンス化された新しい`UISearchController`検索フィールドを表示し、スクリーン キーボードのユーザーを作成します。 検索結果のコレクション (で定義されている、 `SearchResultsViewController`) このキーボード下に表示されます。

次に、`SearchBar`が構成されている情報など、**プレース ホルダー**ヒント。 これは、実行されている検索の種類についてユーザーに情報を提供します。

[検索] フィールドは、2 つの方法のいずれかでユーザーに表示されます。

- **モーダル ダイアログ ビュー** -`PresentViewController`メソッドを呼び出して、既存のビューの検索の提示を全画面表示します。
- **内容を表示**- a`UISearchContainerViewController`を含む検索コント ローラーが作成されます。 A`UINavigationController`ナビゲーション コント ローラーがビュー コント ローラーに追加しを含む検索コンテナーが作成`AddChildViewController (navController)`と表示されるビュー`View.Add (navController.View)`です。

最後をもう一度、プレゼンテーションの種類に基づいて、いずれか、`ViewDidLoad`または`ViewDidAppear`メソッドの呼び出しは、`ShowSearchController`検索をユーザーに提示するメソッド。

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

アプリケーションが実行されると、ユーザーが選択した検索 タブの項目の完全なフィルター処理されていない一覧は、ユーザーに表示されます。

[![](text-fields-and-search-images/intro02.png "既定の検索結果")](text-fields-and-search-images/intro02.png#lightbox)

ユーザーが検索用語を入力すると、結果の一覧がその語句でフィルター処理し、自動的に更新します。

[![](text-fields-and-search-images/intro03.png "フィルター選択された検索結果")](text-fields-and-search-images/intro03.png#lightbox)

いつでもは、ユーザーは、検索結果内の項目にフォーカスを移動され、選択 Siri リモコンのタッチ画面をクリックしています。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事は、設計と Xamarin.tvOS アプリ内でテキストと検索のフィールドの操作について説明しました。 インターフェイス デザイナーでのテキストと検索のコレクションの内容を作成する方法を示して、tvOS でユーザーに表示する場合、[検索] フィールドを 2 つの方法を示しましたがします。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
