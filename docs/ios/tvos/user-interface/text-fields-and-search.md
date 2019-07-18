---
title: TvOS テキスト フィールドおよび Xamarin での検索フィールドの操作
description: このドキュメントでは、Xamarin でビルドされた tvOS アプリのテキストと検索のフィールドを操作する方法について説明します。 テキストと検索のフィールドの概要を提供し、キーボード、ストーリー ボードの統合、データ モデルの検索、およびについて説明します。
ms.prod: xamarin
ms.assetid: 9EE63CA6-2F31-4EE0-AAE5-82E18CFAC06C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: f1085044f2147bec0910a87f8a6174c4648ef9b8
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61162433"
---
# <a name="working-with-tvos-text-and-search-fields-in-xamarin"></a>TvOS テキスト フィールドおよび Xamarin での検索フィールドの操作

Xamarin.tvOS アプリが (ユーザー Id とパスワード) など、ユーザーから小規模なテキストを要求できます必要に応じて、テキスト フィールドを使用して、スクリーン キーボードします。

[![](text-fields-and-search-images/intro01.png "サンプルの検索フィールド")](text-fields-and-search-images/intro01.png#lightbox)

必要に応じて、検索フィールドを使用して、アプリのコンテンツのキーワード検索機能を提供できます。

[![](text-fields-and-search-images/intro02.png "サンプルの検索結果")](text-fields-and-search-images/intro02.png#lightbox)

このドキュメントでは、テキストと検索フィールド Xamarin.tvOS アプリで操作の詳細を説明します。

<a name="About-Text-and-Search-Fields" />

## <a name="about-text-and-search-fields"></a>テキスト フィールドおよびフィールドの検索について

使用して、ユーザーからの少量のテキストを収集する 1 つまたは複数のテキスト フィールドを Xamarin.tvOS が提示できる、前述のように必要な場合、画面に表示される (または、ユーザーがインストールされている tvOS のバージョンによっては省略可能な bluetooth キーボード)。 

さらに、アプリでは、大量のコンテンツを表示します (など、音楽、映画、または画像のコレクション) のユーザーに、場合により、少量の利用可能な項目の一覧をフィルター処理するテキストを入力するユーザーを検索フィールドを含める。

<a name="Text-Fields" />

## <a name="text-fields"></a>テキスト フィールド

TvOS、テキスト フィールドが表示され、として、固定サイズ、丸め、コーナーの入力ボックスが表示されますが、スクリーン キーボード ユーザーがクリックしたとき。

[![](text-fields-and-search-images/text01.png "テキスト フィールドで tvOS")](text-fields-and-search-images/text01.png#lightbox)

ユーザーが移動したときに[フォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)特定のテキスト フィールドを大きくし、ディープ シャドウを表示します。 これに注意してください、ユーザー インターフェイスを設計するときにテキスト フィールドは他の UI 要素とフォーカスの重複が可能にする必要があります。

Apple では、テキスト フィールドを操作するための次の推奨事項があります。

- **エントリを多用しないようにテキスト**- の性質のため、スクリーン キーボード、長いセクションのテキストを入力するか、複数のテキスト フィールドに入力をユーザーに面倒です。 優れたソリューションは、選択リストを使用してテキスト エントリの量を制限するまたは[ボタン](~/ios/tvos/user-interface/buttons.md)します。
- **通信の目的にヒントを使用して**-テキスト フィールドは空のプレース ホルダー「ヒント」を表示できます。 該当する場合は、ヒントを使用して、別のラベルではなく、テキスト フィールドの目的を説明します。
- **適切な既定のキーボードの種類を選択します。** 、tvOS、テキスト フィールドに指定できるいくつかの異なる、特化したキーボード型を提供します。 たとえば、電子メール アドレスのキーボードは最後に入力したアドレスの一覧から選択できるようにすることでエントリを軽減できます。
- **必要に応じて、セキュリティで保護されたテキスト フィールドを使用して、** -セキュリティで保護されたテキスト フィールド (実際の文字) ではなくドットとして入力された文字を表示します。 常にパスワードなどの機密情報を収集するときに、セキュリティで保護されたテキスト フィールドを使用します。

<a name="Keyboards" />

## <a name="keyboards"></a>キーボード

ユーザーが画面に表示されるユーザー インターフェイスでは、線形内のテキスト フィールドがクリックされるたびに、キーボードが表示されます。 ユーザーがタッチ画面を使用して、 [Siri のリモート](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)をキーボードから個々 の文字を選択し、要求された情報を入力します。

[![](text-fields-and-search-images/keyboard01.png "Siri のリモート キーボード")](text-fields-and-search-images/keyboard01.png#lightbox)

現在のビューでは、複数のテキスト フィールドがある場合、**次**ボタンがクリックすると、次のテキスト フィールドに自動的に表示されます。 A**完了**テキスト入力を終了し、前の画面に戻りますが最後のテキスト フィールドのボタンが表示されます。 

いつでも、ユーザーが押すにも、**メニュー**テキスト入力を終了し、前の画面に戻ってもう一度 Siri リモコンのボタン。

Apple が使用するための次の推奨事項スクリーン キーボード。

- **適切な既定のキーボードの種類を選択します。** 、tvOS、テキスト フィールドに指定できるいくつかの異なる、特化したキーボード型を提供します。 たとえば、電子メール アドレスのキーボードは最後に入力したアドレスの一覧から選択できるようにすることでエントリを軽減できます。
- **、適切な場合は、キーボードのアクセサリのビューを使用して**- に加えて、標準には、常に表示される、省略可能のアクセサリ ビュー (イメージやラベル) などの情報を追加できる、テキスト入力のまたは目的を明確にスクリーン キーボード必要な情報を入力するのには、ユーザーを支援します。

操作の詳細については、スクリーン キーボード、Apple を参照してください[UIKeyboardType](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITextInputTraits_Protocol/index.html#//apple_ref/c/tdef/UIKeyboardType)、[キーボードを管理する](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/KeyboardManagement/KeyboardManagement.html#//apple_ref/doc/uid/TP40009542-CH5-SW1)、[データ入力用のカスタム ビュー](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/InputViews/InputViews.html#//apple_ref/doc/uid/TP40009542-CH12-SW1)と[IOS にテキストのプログラミング ガイド](https://developer.apple.com/library/tvos/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html)ドキュメント。

<a name="Search" />

## <a name="search"></a>検索

検索フィールドがテキスト フィールドを提供する特殊な画面を表示し、スクリーン キーボードをキーボードの下に表示される項目のコレクションをフィルター処理できます。

[![](text-fields-and-search-images/search01.png "サンプルの検索結果")](text-fields-and-search-images/search01.png#lightbox)

ように、検索フィールドで文字を入力すると、次の結果は、検索の結果を自動的に反映されます。 いつでもでも、ユーザーが結果にフォーカスを移動し、表示されている項目のいずれかを選択します。

Apple では、検索フィールドを操作するための次の推奨事項があります。

- **最近の検索の提供**- Siri のリモートでのテキストを入力することができますと面倒でユーザーが検索要求を再実行し、キーボードの下の現在の結果の前に最新の検索結果のセクションの追加を検討する傾向があるためです。
- **可能であれば、結果の数を制限する**- 項目の大きい一覧のユーザー解析し移動し、返される結果の数の制限を検討するは難しいためです。
- **必要に応じて、提供する検索結果をフィルター処理**- 場合は、アプリによって提供されるコンテンツが適していると、ユーザーが返される検索結果をさらにフィルター処理できるようにするスコープ バーの追加を検討します。

詳細については、Apple を参照してください[UISearchController クラス参照](https://developer.apple.com/library/tvos/documentation/UIKit/Reference/UISearchController/index.html)します。

<a name="Working-with-Text-Fields" />

## <a name="working-with-text-fields"></a>テキスト フィールドの操作

Xamarin.tvOS アプリで、テキスト フィールドを使用する最も簡単な方法では、iOS デザイナーを使用してユーザー インターフェイスのデザインに追加します。

次の手順で行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **Solution Pad**、ダブルクリックして、`Main.storyboard`ファイルを開き、編集します。
1. 1 つまたは複数のドラッグ**テキスト フィールド**int にビューをデザイン画面。 

    [![](text-fields-and-search-images/text02.png "テキスト フィールド")](text-fields-and-search-images/text02.png#lightbox)
1. 選択、**テキスト フィールド**し、それぞれ一意**名前**で、**ウィジェット**のタブ、 **Properties Pad**: 

    [![](text-fields-and-search-images/text03.png "Properties Pad の [ウィジェット] タブ")](text-fields-and-search-images/text03.png#lightbox)
1. **テキスト フィールド**セクションなどの要素を定義する、**プレース ホルダー**ヒントと既定**値**: 

    [![](text-fields-and-search-images/text04.png "テキストのフィールド セクション")](text-fields-and-search-images/text04.png#lightbox)
1. などのプロパティを定義するスクロール**スペル チェック**、**大文字/小文字**と既定の**キーボード型**: 

    [![](text-fields-and-search-images/text05.png "スペル チェック、大文字と小文字および既定のキーボードの種類")](text-fields-and-search-images/text05.png#lightbox) 
1. ストーリー ボードには、変更を保存します。
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. **ソリューション エクスプローラー**で `Main.storyboard` ファイルをダブルクリックして、編集用に開きます。
1. 1 つまたは複数のドラッグ**テキスト フィールド**int にビューをデザイン画面。 

    [![](text-fields-and-search-images/text02-vs.png "テキスト フィールド")](text-fields-and-search-images/text02-vs.png#lightbox)
1. 選択、**テキスト フィールド**し、それぞれ一意**名前**で、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**: 

    [![](text-fields-and-search-images/text03-vs.png "[ウィジェット] タブ")](text-fields-and-search-images/text03-vs.png#lightbox)
1. **テキスト フィールド**セクションなどの要素を定義する、**プレース ホルダー**ヒントと既定**値**: 

    [![](text-fields-and-search-images/text04-vs.png "テキストのフィールド セクション")](text-fields-and-search-images/text04-vs.png#lightbox)
1. などのプロパティを定義するスクロール**スペル チェック**、**大文字/小文字**と既定の**キーボード型**: 

    [![](text-fields-and-search-images/text05-vs.png "スペル チェック、大文字と小文字および既定のキーボードの種類")](text-fields-and-search-images/text05-vs.png#lightbox) 
1. ストーリー ボードには、変更を保存します。
    
-----

コードでは、取得または使用して、テキスト フィールドの値を設定することができます、`Text`プロパティ。

```csharp
Console.WriteLine ("User ID {0} and Password {1}", UserId.Text, Password.Text);
```

必要に応じて使用することができます、`Started`と`Ended`最初と最後のテキスト入力に応答するイベントをテキスト フィールド。

<a name="Working-with-Search-Fields" />

## <a name="working-with-search-fields"></a>検索フィールドの操作

Xamarin.tvOS アプリでの検索フィールドを使用する最も簡単な方法では、それらをインターフェイス デザイナーを使用してユーザー インターフェイスのデザインを追加します。

次の手順で行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
    
1. **Solution Pad**、ダブルクリックして、`Main.storyboard`ファイルを開き、編集します。
1. ユーザーの検索の結果を表示するストーリー ボードに新しいコレクションのビュー コント ローラーをドラッグします。 

    [![](text-fields-and-search-images/search02.png "コレクション ビュー コント ローラー")](text-fields-and-search-images/search02.png#lightbox)
1. **ウィジェット**のタブ、 **Properties Pad**を使用して、`SearchResultsViewController`の**クラス**と`SearchResults`の**ストーリー ボード ID**: 

    [![](text-fields-and-search-images/search03.png "[ウィジェット] タブ")](text-fields-and-search-images/search03.png#lightbox)
1. 選択、**セル プロトタイプ**デザイン サーフェイス。
1. **ウィジェット**のタブ、**プロパティ エクスプ ローラー**を使用して、`SearchResultCell`の**クラス**と`ImageCell`の**識別子**: 

    [![](text-fields-and-search-images/search04.png "[ウィジェット] タブ")](text-fields-and-search-images/search04.png#lightbox)
1. レイアウトのデザインの**セル プロトタイプ**に一意の各要素を公開および**名前**で、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**: 

    [![](text-fields-and-search-images/search05.png "セルのプロトタイプのデザイン レイアウト")](text-fields-and-search-images/search05.png#lightbox)
1. ストーリー ボードには、変更を保存します。
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. **ソリューション エクスプローラー**で `Main.storyboard` ファイルをダブルクリックして、編集用に開きます。
1. ユーザーの検索の結果を表示するストーリー ボードに新しいコレクションのビュー コント ローラーをドラッグします。 

    [![](text-fields-and-search-images/seach02-vs.png "コレクション ビュー コント ローラー")](text-fields-and-search-images/seach02-vs.png#lightbox)
1. **ウィジェット**のタブ、**プロパティ エクスプ ローラー**を使用して、`SearchResultsViewController`の**クラス**と`SearchResults`の**ストーリー ボード ID**: 

    [![](text-fields-and-search-images/search03-vs.png "[ウィジェット] タブ")](text-fields-and-search-images/search03-vs.png#lightbox)
1. 選択、**セル プロトタイプ**デザイン サーフェイス。
1. **ウィジェット**のタブ、**プロパティ エクスプ ローラー**を使用して、`SearchResultCell`の**クラス**と`ImageCell`の**識別子**: 

    [![](text-fields-and-search-images/search04-vs.png "[ウィジェット] タブ")](text-fields-and-search-images/search04-vs.png#lightbox)
1. レイアウトのデザインの**セル プロトタイプ**に一意の各要素を公開および**名前**で、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**: 

    [![](text-fields-and-search-images/search05-vs.png "セルのプロトタイプのデザイン レイアウト")](text-fields-and-search-images/search05-vs.png#lightbox)
1. ストーリー ボードには、変更を保存します。
    
-----

<a name="Provide-a-Data-Model" />

### <a name="provide-a-data-model"></a>データ モデルを提供します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

次に、ユーザーは探している結果のデータ モデルとして機能するクラスを提供する必要があります。 **ソリューション エクスプ ローラー**でプロジェクト名を右クリックし、選択**追加** > **新しいファイル.**  > **全般** > **空のクラス**を提供し、**名前**: 

[![](text-fields-and-search-images/search06.png "空のクラスを選択し、名前を指定")](text-fields-and-search-images/search06.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

次に、ユーザーは探している結果のデータ モデルとして機能するクラスを提供する必要があります。 **ソリューション エクスプ ローラー**でプロジェクト名を右クリックし、選択**追加** > **新しい項目.**  >  **Apple** > **Misc** > **クラス**を提供し、 **名**: 

[![](text-fields-and-search-images/search06-vs.png "クラスを選択し、名前を指定")](text-fields-and-search-images/search06-vs.png#lightbox)

-----

例として、タイトル、キーワードで画像のコレクションを検索するユーザーを許可するアプリは次のようになります。

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

### <a name="the-collection-view-cell"></a>コレクション ビュー セル

データ モデルをその場で、編集、**プロトタイプ セル**(`SearchResultViewCell.cs`)、それにある、次を参照してください。

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

`UpdateUI`の個々 のフィールドの表示に使用されるメソッド、 **PictureInformation**項目 (、`PictureInfo`プロパティ) プロパティを更新するたびに、名前付きの UI 要素。 たとえば、イメージとタイトルの画像に関連付けられています。

<a name="The-Collection-View-Controller" />

### <a name="the-collection-view-controller"></a>コレクション ビュー コント ローラー

検索結果コレクション ビュー コント ローラーを次に、編集 (`SearchResultsViewController.cs`)、次のような外観にするとします。

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

まず、`IUISearchResultsUpdating`インターフェイスは、ユーザーによって更新されたコント ローラーの検索フィルターを処理するクラスに追加されます。

```csharp
public partial class SearchResultsViewController : UICollectionViewController , IUISearchResultsUpdating
```

ID を指定する定数が定義されても、**プロトタイプ セル**(上記のインターフェイス デザイナーで定義されている ID と一致する) コレクション コント ローラーが新しいセルを要求したときに後でが使用されます。

```csharp
public const string CellID = "ImageCell";
```

検索フィルターの用語を検索対象のアイテムの完全な一覧についてはストレージが作成されると、その用語と一致するアイテムの一覧。

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

ときに、`SearchFilter`が変更されると、一致する項目の一覧が更新され、コレクション ビューのコンテンツが再度読み込まれます。 `FindPictures`ルーチンは、新しい検索用語に一致する項目を検索する責任を負います。

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

値、 `SearchFilter` (結果のコレクション ビューが更新されます)、更新、ユーザーが検索コント ローラーでフィルターを変更する場合。

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

この例では、すべてのサンプル データが作成されますメモリ内コレクションのビュー コント ローラーが読み込まれる。 実際のアプリで、データベースや web サービスから、このデータを読み取る可能性がありますが、限られたメモリを Apple TV のオーバーランを防ぐために必要な場合にのみです。

`NumberOfSections`と`GetItemsCount`メソッドは、一致した項目の数を指定します。

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

`GetCell`新しいメソッドを返します**プロトタイプ セル**(に基づいて、`CellID`上記のストーリー ボードの定義) のコレクション ビュー内の各項目。

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

`DidUpdateFocus`メソッドは、結果のコレクション ビュー内の項目を強調表示するようユーザーに視覚的なフィードバックを提供します。

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

最後に、`ItemSelected`メソッドは、結果のコレクション ビューで (Siri のリモートでタッチ画面でクリックすると) 項目を選択するユーザーを処理します。

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

検索フィールドが表示される場合、モーダル ダイアログ ビューとして (経由で呼び出すと、ビューの上部)、使用、`DismissViewController`メソッドをユーザーが項目を選択すると、検索ビューを閉じます。 この例では、ため、ここで消去いるされませんが、検索フィールドには、タブの表示 タブの内容として表示されます。

コレクション ビューの詳細についてを参照してください、[コレクション ビューを使用する](~/ios/tvos/user-interface/collection-views.md)ドキュメント。

<a name="Presenting the Search Field" />

### <a name="presenting-the-search-field"></a>検索フィールドを表示します。

2 つの主な方法はあります検索フィールド (と関連スクリーン キーボード、および検索結果) tvOS でユーザーに表示することができます。 

- **モーダル ダイアログ ビュー** -ビューと全画面モーダル ダイアログのビューとしてビュー コント ローラー、現在経由で、検索フィールドを表示できます。 これは通常、ボタンやその他の UI 要素をクリックすると、ユーザーへの応答で行われます。 ユーザーが検索結果から項目を選択すると、ダイアログ ボックスが閉じられます。
- **内容を表示**-検索フィールドに、指定されたビューの直接の一部であります。 たとえば、タブのビュー コント ローラーの検索 タブの内容として。

上記の画像の検索可能なリストの例では、検索フィールドには、検索 タブの内容の表示として表示され、検索 タブのビュー コント ローラーが、次のようになります。

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

最初に、定数が定義が一致する、**ストーリー ボード識別子**インターフェイス デザイナーの検索結果のコレクション ビュー コント ローラーに割り当てられました。

```csharp
public const string SearchResultsID = "SearchResults";
```

次に、`ShowSearchController`新しい検索ビュー コレクション コント ローラーと表示されたために必要なメソッドを作成します。

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

1 回上記メソッドで、 `SearchResultsViewController` 、ストーリー ボードからインスタンス化された新しい`UISearchController`検索フィールドを表示し、ユーザーにスクリーン キーボードが作成されます。 検索結果のコレクション (で定義されている、 `SearchResultsViewController`) このキーボードが表示されます。

次に、`SearchBar`など情報によって構成されます、**プレース ホルダー**ヒント。 実行されている検索の種類についてユーザーに情報を提供します。

検索フィールドは 2 つの方法でユーザーに表示されます。

- **モーダル ダイアログ ビュー** -`PresentViewController`メソッドを呼び出して既存のビューに対する検索を提示する全画面表示します。
- **内容を表示**-`UISearchContainerViewController`を含む検索コント ローラーが作成されます。 A`UINavigationController`作成すると、ナビゲーション コント ローラーがビュー コント ローラーに追加し、検索のコンテナーを含む`AddChildViewController (navController)`、表示されるビューと`View.Add (navController.View)`します。

最後をもう一度、プレゼンテーションの種類に基づいて、か、`ViewDidLoad`または`ViewDidAppear`メソッドの呼び出しは、`ShowSearchController`検索をユーザーに提示するメソッド。

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

アプリの実行し、検索 タブ、ユーザーが選択した項目のフィルター処理されていない完全な一覧は、ユーザーに表示されます。

[![](text-fields-and-search-images/intro02.png "既定の検索結果")](text-fields-and-search-images/intro02.png#lightbox)

ユーザーが検索語句を入力すると、結果の一覧がその期間でフィルター処理され、自動更新します。

[![](text-fields-and-search-images/intro03.png "フィルター選択された検索結果")](text-fields-and-search-images/intro03.png#lightbox)

、いつでも、ユーザーが検索結果内の項目にフォーカスを切り替えてし、をオンに Siri のリモートのタッチ画面をクリックします。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、設計と Xamarin.tvOS アプリ内でテキストと検索のフィールドの操作について説明しました。 インターフェイス デザイナーでのテキストと検索のコレクションの内容を作成する方法を示しましたし、tvOS でユーザーに表示する場合、検索フィールドを 2 つのさまざまな方法を示しました。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
