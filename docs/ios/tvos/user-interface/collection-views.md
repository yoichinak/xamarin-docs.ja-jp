---
title: "コレクション ビューの操作"
description: "この記事では、設計と Xamarin.tvOS アプリ内でコレクション ビューの操作について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5125C4C7-2DDF-4C19-A362-17BB2B079178
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: f0201e114f55e0610aceb68f98fae60a801afc68
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-collection-views"></a>コレクション ビューの操作

_この記事では、設計と Xamarin.tvOS アプリ内でコレクション ビューの操作について説明します。_

コレクション ビューは、任意のレイアウトを使用して表示するコンテンツのグループにすること。 組み込みサポートを使用して、これにより、簡単に作成のグリッドのようなまたは線形レイアウトもカスタム レイアウトをサポートします。

[ ![](collection-views-images/collection01.png "コレクション ビューのサンプル")](collection-views-images/collection01.png)

コレクション ビューは、ユーザーとのやり取り、およびコレクションの内容を提供する、デリゲートと、データ ソースの両方を使用して項目のコレクションを保持します。 コレクション ビューはビュー自体から独立しているレイアウト サブシステムに基づいているため、別のレイアウトを提供する簡単に変更できますコレクション ビューのデータの場でのプレゼンテーションです。

<a name="About-Collection-Views" />

## <a name="about-collection-views"></a>コレクション ビューの概要

前述のコレクション ビュー (`UICollectionView`) 項目の順序付けられたコレクションを管理し、カスタマイズ可能なレイアウトでそれらの項目を表示します。 コレクション ビューは、テーブルのビューと同様の方法で作業 (`UITableView`) レイアウト アイテムが存在する 1 つの列だけに使用されるおそれを除き、します。

アプリがデータ ソースを使用して、コレクションに関連付けられているデータを提供する責任を tvOS のコレクション ビューを使用する場合 (`UICollectionViewDataSource`)。 コレクション ビューのデータを編成必要に応じてと別のグループ (セクション) に表示されます。

コレクション ビューは、セルを使用する画面に表示される個々 の項目を示します (`UICollectionViewCell`) (イメージやタイトル) などのコレクションからの情報の指定されたピースのプレゼンテーションを提供します。

必要に応じて、補助ビューは、セクションとセルのヘッダーとフッターとして機能する、コレクション ビューのプレゼンテーションに追加できます。 コレクション ビューのレイアウトは、個々 のセルと共にこれらのビューの位置を定義します。

コレクション ビューは、デリゲートを使用するユーザーとの対話に応答できる (`UICollectionViewDelegate`)。 このデリゲートは、特定のセルはセルが強調表示されている場合、フォーカスを取得できる場合、またはいずれかが選択されている場合を判断するもできます。 場合によっては、デリゲートは、個々 のセルのサイズを決定します。

<a name="Collection-View-Layouts" />

## <a name="collection-view-layouts"></a>コレクション ビューのレイアウト

コレクション ビューの主な機能は、その分離、表示するデータとレイアウトの間です。 コレクション ビューのレイアウト (`UICollectionViewLayout`) とコレクション ビューの画面上のプレゼンテーションの組織とセル (および任意の補助ビュー) の場所を提供します。

個々 のセルは、アタッチされたデータ ソースとコレクション ビューによって作成し、配置し、は特定のコレクション ビューのレイアウトで表示されます。

コレクション ビューのレイアウトは通常、コレクション ビューが作成されるときに提供されます。 ただし、コレクション ビューのレイアウトを変更するにはいつでも、コレクション ビューのデータの画面に表示される表示が自動的に更新されます提供される新しいレイアウトを使用します。

コレクション ビューのレイアウトは、(既定でアニメーションは実行されません) の 2 つの異なるレイアウトの切り替えをアニメーション化するために使用できるいくつかのメソッドを提供します。 さらに、コレクション ビューのレイアウトは、レイアウトに変更されます。 ユーザー操作をさらにアニメーション化するジェスチャ レコグナイザーを操作できます。

<a name="Creating-Cells-and-Supplementary-Views" />

## <a name="creating-cells-and-supplementary-views"></a>セルと補助ビューの作成

コレクション ビューのデータ ソースはのみコンテンツを表示する使用されているセル コレクションのバックアップを作成しても項目、データを提供するための責任ではありません。

コレクション ビューは、項目の大規模なコレクションを処理するように設計された、ため個々 のセルのデキューおよびメモリの制限をオーバーランを防止する再利用できます。 行うビューの 2 つのさまざまな方法はあります。

- `DequeueReusableCell` -作成または指定されているアプリのストーリー ボードで) 指定された型のセルを取得します。
- `DequeueReusableSupplementaryView` -作成または指定されているアプリのストーリー ボードで) 指定された型の補助ビューを取得します。

これらのメソッドのいずれかを呼び出す前に、クラスを登録する必要がありますストーリー ボードまたは`.xib`ファイル コレクション ビューを持つセルのビューを作成するために使用します。 例:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    ...
}
```

ここで`typeof(CityCollectionViewCell)`ビューをサポートするクラスを提供し、`CityViewDatasource.CardCellId`セル (またはビュー) がデキューされたときに使用する ID を提供します。

セルがデキューされた後は、データが表す項目を構成し、画面のコレクション ビューに戻ります。

<a name="About-Collection-View-Controllers" />

## <a name="about-collection-view-controllers"></a>コレクション ビューのコント ローラーについて

コレクション ビューのコント ローラー (`UICollectionViewController`) は、特殊なビュー コント ローラー (`UIViewController`)、次の動作を提供します。

- そのストーリー ボードから、コレクション ビューの読み込みをまたは`.xib`ファイルおよびビューをインスタンス化します。 コードで作成された場合は、新規の未構成コレクション ビューが自動的に作成します。
- コント ローラーがストーリー ボードから、データ ソースとデリゲートを読み込もうとコレクション ビューが読み込まれると、または`.xib`ファイル。 使用しない場合、両方のソースとして自体設定します。
- により、データが、コレクション ビューが表示されたら、最初に設定する前に読み込まれると再読み込み後続の各ディスプレイ上の選択をオフになります。

コレクション ビューのコント ローラーがなど、コレクション ビューのライフ サイクルの管理に使用できるメソッドをオーバーライドできるを提供するさらに、`AwakeFromNib`と`ViewWillDisplay`です。

<a name="Collection-Views-and-Storyboards" />

## <a name="collection-views-and-storyboards"></a>コレクション ビューとストーリー ボード

Xamarin.tvOS アプリでは、コレクション ビューを使用する最も簡単な方法では、そのストーリー ボードに追加します。 簡単な例として行う、画像、タイトルと選択 ボタンを表示するサンプル アプリを作成します。 場合は、ユーザーは、選択ボタンをクリックして、ユーザーが新しいイメージを選択できるように、コレクション ビューが表示されます。 イメージを選択するときは、コレクション ビューは閉じられ、新しいイメージとタイトルが表示されます。

それでは、次の操作を行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

    
1. 新しい開始**1 つのビュー tvOS アプリ**for mac の Visual Studio で
1. **ソリューション エクスプ ローラー**をダブルクリックして、`Main.storyboard`ファイルし、iOS デザイナーで開きます。
1. 既存のビューにイメージの表示、ラベル、およびボタンを追加し、次のようにするように構成します。 

    [ ![](collection-views-images/collection02.png "サンプルのレイアウト")](collection-views-images/collection02.png)
1. 割り当てる、**名前**イメージ ビューとのラベルに、**ウィジェット タブ**の**プロパティ エクスプ ローラー**です。 例: 

    [ ![](collection-views-images/collection03.png "名の設定")](collection-views-images/collection03.png)
1. 次に、コレクション ビューのコント ローラーをストーリー ボード上にドラッグします。 

    [ ![](collection-views-images/collection04.png "コレクション ビューのコント ローラー")](collection-views-images/collection04.png)
1. コレクション ビューのコント ローラーに、ボタン コントロール ドラッグ アンド選択**プッシュ**ポップアップから。 

    [ ![](collection-views-images/collection05.png "ポップアップからのプッシュを選択します。")](collection-views-images/collection05.png)
1. アプリの実行時にこれをユーザーがボタンをクリックするたびに、表示をする、コレクション ビューとなります。
1. コレクション ビューを選択しの次の値を入力、**レイアウト タブの**の**プロパティ エクスプ ローラー**: 

    [ ![](collection-views-images/collection06.png "プロパティ エクスプ ローラー")](collection-views-images/collection06.png)
1. これは、個々 のセルとセルと外側のエッジ コレクション ビューの間の境界線のサイズを制御します。
1. コレクション ビューのコント ローラーを選択し、そのクラスに設定`CityCollectionViewController`で、**ウィジェット タブ**: 

    [ ![](collection-views-images/collection07.png "クラスを CityCollectionViewController に設定します。")](collection-views-images/collection07.png)
1. コレクション ビューを選択し、そのクラスに設定`CityCollectionView`で、**ウィジェット タブ**: 

    [ ![](collection-views-images/collection08.png "クラスを CityCollectionView に設定します。")](collection-views-images/collection08.png)
1. コレクション ビューのセルを選択し、そのクラスに設定`CityCollectionViewCell`で、**ウィジェット タブ**: 

    [ ![](collection-views-images/collection09.png "クラスを CityCollectionViewCell に設定します。")](collection-views-images/collection09.png)
1. **ウィジェット タブ**いることを確認、**レイアウト**は`Flow`と**方向にスクロールする**は`Vertical`コレクション ビューの。 

    [ ![](collection-views-images/collection10.png "ウィジェット タブ")](collection-views-images/collection10.png)
1. コレクション ビューのセルを選択し、設定、 **Identity**に`CityCell`で、**ウィジェット タブ**: 

    [ ![](collection-views-images/collection11.png "CityCell に Id を設定します。")](collection-views-images/collection11.png)
1. 変更内容を保存します。
    

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

    
1. 新しい開始**1 つのビュー tvOS アプリ**Visual Studio でします。
1. **ソリューション エクスプ ローラー**をダブルクリックして、`Main.storyboard`ファイルし、iOS デザイナーで開きます。
1. 既存のビューにイメージの表示、ラベル、およびボタンを追加し、次のようにするように構成します。 

    [ ![](collection-views-images/collection02vs.png "レイアウトを構成します。")](collection-views-images/collection02vs.png)
1. 割り当てる、**名前**イメージ ビューとのラベルに、**ウィジェット タブ**の**プロパティ エクスプ ローラー**です。 例: 

    [ ![](collection-views-images/collection03vs.png "プロパティ エクスプ ローラー")](collection-views-images/collection03vs.png)
1. 次に、コレクション ビューのコント ローラーをストーリー ボード上にドラッグします。 

    [ ![](collection-views-images/collection04vs.png "コレクション ビューのコント ローラー")](collection-views-images/collection04vs.png)
1. コレクション ビューのコント ローラーに、ボタン コントロール ドラッグ アンド選択**プッシュ**ポップアップから。 

    [ ![](collection-views-images/collection05vs.png "ポップアップからのプッシュを選択します。")](collection-views-images/collection05vs.png)
1. アプリの実行時にこれをユーザーがボタンをクリックするたびに、表示をする、コレクション ビューとなります。
1. コレクション ビューを選択し、[、**レイアウト] タブの**の**プロパティ エクスプ ローラー**入力、**幅**として_361_と**高さ**として_256_ 
1. これは、個々 のセルとセルと外側のエッジ コレクション ビューの間の境界線のサイズを制御します。
1. コレクション ビューのコント ローラーを選択し、そのクラスに設定`CityCollectionViewController`で、**ウィジェット タブ**: 

    [ ![](collection-views-images/collection07vs.png "クラスを CityCollectionViewController に設定します。")](collection-views-images/collection07vs.png)
1. コレクション ビューを選択し、そのクラスに設定`CityCollectionView`で、**ウィジェット タブ**: 

    [ ![](collection-views-images/collection08vs.png "クラスを CityCollectionView に設定します。")](collection-views-images/collection08vs.png)
1. コレクション ビューのセルを選択し、そのクラスに設定`CityCollectionViewCell`で、**ウィジェット タブ**: 

    [ ![](collection-views-images/collection09vs.png "クラスを CityCollectionViewCell に設定します。")](collection-views-images/collection09vs.png)
1. **ウィジェット タブ**いることを確認、**レイアウト**は`Flow`と**方向にスクロールする**は`Vertical`コレクション ビューの。 

    [ ![](collection-views-images/collection10vs.png "%T ウィジェット タブ")](collection-views-images/collection10vs.png)
1. コレクション ビューのセルを選択し、設定、 **Identity**に`CityCell`で、**ウィジェット タブ**: 

    [ ![](collection-views-images/collection11vs.png "CityCell に Id を設定します。")](collection-views-images/collection11vs.png)
1. 変更内容を保存します。
    

-----

選択した場合は`Custom`コレクション ビューの**レイアウト**、カスタム レイアウトを指定できます。 Apple 提供組み込み`UICollectionViewFlowLayout`と`UICollectionViewDelegateFlowLayout`グリッド ベースのレイアウトでデータを簡単に表すことができますを (それらで使用されます、`flow`レイアウト スタイル)。 

ストーリー ボードの使用の詳細についてを参照してください、[こんにちは、tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)です。

<a name="Providing-Data-for-the-Collection-View" />

## <a name="providing-data-for-the-collection-view"></a>コレクション ビューのデータを提供します。

当社のストーリー ボードに追加された、コレクション ビュー (とコレクション ビューのコント ローラー) ができました、データ コレクションを提供する必要があります。 

<a name="The-Data-Model" />

### <a name="the-data-model"></a>データ モデル

まず、しようとして、タイトルとを選択する市区町村を許可する場合にフラグを表示する画像のファイル名を保持するデータのモデルを作成します。

作成、`CityInfo`クラスし、次のようになります。

```csharp
using System;

namespace tvCollection
{
    public class CityInfo
    {
        #region Computed Properties
        public string ImageFilename { get; set; }
        public string Title { get; set; }
        public bool CanSelect{ get; set; }
        #endregion

        #region Constructors
        public CityInfo (string filename, string title, bool canSelect)
        {
            // Initialize
            this.ImageFilename = filename;
            this.Title = title;
            this.CanSelect = canSelect;
        }
        #endregion
    }
}
```

### <a name="the-collection-view-cell"></a>コレクション ビューのセル

各セルのデータの表示方法を定義する必要があります。 編集、 `CityCollectionViewCell.cs` (から作成したファイルを自動的に、ストーリー ボード ファイル) され、次のようになります。

```csharp
using System;
using Foundation;
using UIKit;
using CoreGraphics;

namespace tvCollection
{
    public partial class CityCollectionViewCell : UICollectionViewCell
    {
        #region Private Variables
        private CityInfo _city;
        #endregion

        #region Computed Properties
        public UIImageView CityView { get ; set; }
        public UILabel CityTitle { get; set; }

        public CityInfo City {
            get { return _city; }
            set {
                _city = value;
                CityView.Image = UIImage.FromFile (City.ImageFilename);
                CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
                CityTitle.Text = City.Title;
            }
        }
        #endregion

        #region Constructors
        public CityCollectionViewCell (IntPtr handle) : base (handle)
        {
            // Initialize
            CityView = new UIImageView(new CGRect(22, 19, 320, 171));
            CityView.AdjustsImageWhenAncestorFocused = true;
            AddSubview (CityView);

            CityTitle = new UILabel (new CGRect (22, 209, 320, 21)) {
                TextAlignment = UITextAlignment.Center,
                TextColor = UIColor.White,
                Alpha = 0.0f
            };
            AddSubview (CityTitle);
        }
        #endregion
    

    }
}
```

TvOS アプリ用おが表示されているイメージと省略可能なタイトルです。 特定の市区町村をオンにできない場合は、次のコードを使用してイメージ ビューを暗転おは。

```csharp
CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
```

組み込みを使用するイメージを含むセルには、ユーザーがフォーカス設定が読み込まれると、に視差効果は、次のプロパティを設定します。

```csharp
CityView.AdjustsImageWhenAncestorFocused = true;
```

ナビゲーションとフォーカスの詳細についてを参照してください、[ナビゲーションとフォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)と[Siri リモート コンピューターと Bluetooth コント ローラー](~/ios/tvos/platform/remote-bluetooth.md)ドキュメント。


<a name="The-Collection-View-Data-Provider" />

### <a name="the-collection-view-data-provider"></a>コレクション ビューのデータ プロバイダー

作成されたデータ モデルと、定義されているセルのレイアウトでは、このコレクション ビューのデータ ソースを作成してみましょう。 データ ソースは担当だけでなく、バックアップ データも行う画面上の個々 のセルを表示するセルになります。

作成、`CityViewDatasource`クラスし、次のようになります。

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;
using ObjCRuntime;

namespace tvCollection
{
    public class CityViewDatasource : UICollectionViewDataSource
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Static Constants
        public static NSString CardCellId = new NSString ("CityCell");
        #endregion

        #region Computed Properties
        public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
        public CityCollectionView ViewController { get; set; }
        #endregion

        #region Constructors
        public CityViewDatasource (CityCollectionView controller)
        {
            // Initialize
            this.ViewController = controller;
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities() {

            // Clear existing cities
            Cities.Clear();

            // Add new cities
            Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
            Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
            Cities.Add(new CityInfo("City03.jpg", "Skyline at Night", true));
            Cities.Add(new CityInfo("City04.jpg", "Golden Gate Bridge", true));
            Cities.Add(new CityInfo("City05.jpg", "Roads by Night", true));
            Cities.Add(new CityInfo("City06.jpg", "Church Domes", true));
            Cities.Add(new CityInfo("City07.jpg", "Mountain Lights", true));
            Cities.Add(new CityInfo("City08.jpg", "City Scene", false));
            Cities.Add(new CityInfo("City09.jpg", "House in Winter", true));
            Cities.Add(new CityInfo("City10.jpg", "By the Lake", true));
            Cities.Add(new CityInfo("City11.jpg", "At the Dome", true));
            Cities.Add(new CityInfo("City12.jpg", "Cityscape", true));
            Cities.Add(new CityInfo("City13.jpg", "Model City", true));
            Cities.Add(new CityInfo("City14.jpg", "Taxi, Taxi!", true));
            Cities.Add(new CityInfo("City15.jpg", "On the Sidewalk", true));
            Cities.Add(new CityInfo("City16.jpg", "Midnight Walk", true));
            Cities.Add(new CityInfo("City17.jpg", "Lunchtime Cafe", true));
            Cities.Add(new CityInfo("City18.jpg", "Coffee Shop", true));
            Cities.Add(new CityInfo("City19.jpg", "Rustic Tavern", true));
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections (UICollectionView collectionView)
        {
            return 1;
        }

        public override nint GetItemsCount (UICollectionView collectionView, nint section)
        {
            return Cities.Count;
        }

        public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
            var city = Cities [indexPath.Row];

            // Initialize city
            cityCell.City = city;

            return cityCell;
        }
        #endregion
    }
}
```

このクラスの詳細を見ることができます。 継承して最初に、`UICollectionViewDataSource`し (iOS デザイナーで割り当てられている) にあるセル ID にショートカットを提供します。

```csharp
public static NSString CardCellId = new NSString ("CityCell");
```

次におコレクションのデータについての記憶域を提供し、データを入力するクラスを提供します。

```csharp
public List<CityInfo> Cities { get; set; } = new List<CityInfo>();
...

public void PopulateCities() {

    // Clear existing cities
    Cities.Clear();

    // Add new cities
    Cities.Add(new CityInfo("City01.jpg", "Houses by Water", false));
    Cities.Add(new CityInfo("City02.jpg", "Turning Circle", true));
    ...
}
```

無効にしてから、`NumberOfSections`メソッドと戻り値のコレクションを表示するセクション (項目のグループ) の数がします。 この例では、ある 1 つのみ。

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    return 1;
}
```

次に、次のコードを使用してコレクション内の項目数を返します。

```csharp
public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    return Cities.Count;
}
```

最後に、コレクション ビューは、次のコードで要求時に再利用可能なセルおデキューします。

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    var cityCell = (CityCollectionViewCell)collectionView.DequeueReusableCell (CardCellId, indexPath);
    var city = Cities [indexPath.Row];

    // Initialize city
    cityCell.City = city;

    return cityCell;
}
```

コレクション ビューのセルを取得した後、`CityCollectionViewCell`型、おそこに指定された項目。

<a name="Responding-to-User-Events" />

## <a name="responding-to-user-events"></a>ユーザー イベントに応答します。

ため、ユーザーをコレクションから項目を選択できるように、この対話を処理するコレクション ビュー デリゲートを指定する必要があります。 呼び出し側のビューをユーザーにどのような項目を把握できるようにする方法の選択を提供する必要があります。

<a name="The-App-Delegate" />

### <a name="the-app-delegate"></a>アプリのデリゲート

コレクション ビューから、呼び出し元のビューに、現在選択されている項目を関連付けることが必要です。 使用するカスタム プロパティで、`AppDelegate`です。 編集、`AppDelegate.cs`ファイルし、次のコードを追加します。

```csharp
public CityInfo SelectedCity { get; set;} = new CityInfo("City02.jpg", "Turning Circle", true);
```

これにより、プロパティを定義し、最初に表示される既定の市区町村を設定します。 後で、ユーザーの選択を表示し、変更することを選択できるようにするには、このプロパティを利用するおします。

<a name="The-Collection-View-Delegate" />

### <a name="the-collection-view-delegate"></a>コレクション ビューのデリゲート

次に、追加、新しい`CityViewDelegate`をプロジェクトにクラスし、次のようになります。


```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace tvCollection
{
    public class CityViewDelegate : UICollectionViewDelegateFlowLayout
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public CityViewDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
        {
            return new CGSize (361, 256);
        }

        public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
        {
            if (indexPath == null) {
                return false;
            } else {
                var controller = collectionView as CityCollectionView;
                return controller.Source.Cities[indexPath.Row].CanSelect;
            }
        }

        public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
        {
            var controller = collectionView as CityCollectionView;
            App.SelectedCity = controller.Source.Cities [indexPath.Row];

            // Close Collection
            controller.ParentController.DismissViewController(true,null);
        }
        #endregion
    }
}
```

このクラスについて詳しく見てをみましょう。 継承して最初に、`UICollectionViewDelegateFlowLayout`です。 このクラスから継承している理由および not、`UICollectionViewDelegate`組み込みが使用されているの`UICollectionViewFlowLayout`アイテムとカスタム レイアウト型ではなく指定します。

次に、このコードを使用して個々 のアイテムのサイズを返します。

```csharp
public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
{
    return new CGSize (361, 256);
}
```

次にかどうか、指定されたセルは、次のコードを使用してフォーカスを取得できます決定します。 

```csharp
public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
{
    if (indexPath == null) {
        return false;
    } else {
        var controller = collectionView as CityCollectionView;
        return controller.Source.Cities[indexPath.Row].CanSelect;
    }
}
```

個々 のバックアップ データが含まれている確認その`CanSelect`フラグに設定されて`true`しその値を返します。 ナビゲーションとフォーカスの詳細についてを参照してください、[ナビゲーションとフォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)と[Siri リモート コンピューターと Bluetooth コント ローラー](~/ios/tvos/platform/remote-bluetooth.md)ドキュメント。

最後に、私たちは、次のコードを持つ項目を選択すると、ユーザーに応答します。

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    var controller = collectionView as CityCollectionView;
    App.SelectedCity = controller.Source.Cities [indexPath.Row];

    // Close Collection
    controller.ParentController.DismissViewController(true,null);
}
```

ここでは設定、`SelectedCity`のプロパティ、`AppDelegate`と選択したユーザーを閉じることコレクション ビュー、コント ローラーと呼ばれることをビューに戻る項目にします。 定義していない、`ParentController`まだ、コレクション ビューのプロパティ、やって次にそのします。

<a name="Configuring-the-Collection-View" />

## <a name="configuring-the-collection-view"></a>コレクション ビューを構成します。

これで、コレクション ビューを編集して、マイクロソフトのデータ ソースとデリゲートを割り当てる必要があります。 編集、`CityCollectionView.cs`ファイル (ご利用の米国から自動的に作成、ストーリー ボード) され、次のようになります。

```csharp
using System;
using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionView : UICollectionView
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public CityViewDatasource Source {
            get { return DataSource as CityViewDatasource;}
        }

        public CityCollectionViewController ParentController { get; set;}
        #endregion

        #region Constructors
        public CityCollectionView (IntPtr handle) : base (handle)
        {
            // Initialize
            RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
            DataSource = new CityViewDatasource (this);
            Delegate = new CityViewDelegate ();
        }
        #endregion

        #region Override Methods
        public override nint NumberOfSections ()
        {
            return 1;
        }

        public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
        {
            var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
            if (previousItem != null) {
                Animate (0.2, () => {
                    previousItem.CityTitle.Alpha = 0.0f;
                });
            }

            var nextItem = context.NextFocusedView as CityCollectionViewCell;
            if (nextItem != null) {
                Animate (0.2, () => {
                    nextItem.CityTitle.Alpha = 1.0f;
                });
            }
        }
        #endregion
    }
}
```

アクセスするショートカットを提供して最初に、当社`AppDelegate`: 

```csharp
public static AppDelegate App {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
```

次に、(ときに終了する、コレクション、ユーザーは、選択、デリゲート上で使用)、コレクション ビューのコント ローラーにアクセスするには、コレクション ビューのデータ ソースとプロパティをショートカットを提供します。

```csharp
public CityViewDatasource Source {
    get { return DataSource as CityViewDatasource;}
}

public CityCollectionViewController ParentController { get; set;}
```

次に、次のコードを使用してコレクション ビューを初期化し、セル クラス、データ ソースおよびデリゲートを割り当てます。

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    DataSource = new CityViewDatasource (this);
    Delegate = new CityViewDelegate ();
}
```

最後に、必要にのみ表示したユーザーが強調表示されている場合に、イメージの下のタイトル (フォーカス設定) です。 次のコードを実行します。

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    var previousItem = context.PreviouslyFocusedView as CityCollectionViewCell;
    if (previousItem != null) {
        Animate (0.2, () => {
            previousItem.CityTitle.Alpha = 0.0f;
        });
    }

    var nextItem = context.NextFocusedView as CityCollectionViewCell;
    if (nextItem != null) {
        Animate (0.2, () => {
            nextItem.CityTitle.Alpha = 1.0f;
        });
    }
}
```

Transparence 0 (ゼロ) にフォーカスを失う前の項目の設定し、次の項目の transparence が 100% にフォーカスを獲得します。 これらの遷移をもアニメーションを取得します。


## <a name="configuring-the-collection-view-controller"></a>コレクション ビューのコント ローラーを構成します。

コレクション ビューに最終的な構成を行い、ユーザーは、選択した後、コレクション ビューを閉じることができるように定義したプロパティを設定するコント ローラーを使用する必要があります。

編集、`CityCollectionViewController.cs`ファイル (このストーリー ボードから自動的に作成された) され、次のようになります。

```csharp
// This file has been autogenerated from a class added in the UI designer.

using System;

using Foundation;
using UIKit;

namespace tvCollection
{
    public partial class CityCollectionViewController : UICollectionViewController
    {
        #region Computed Properties
        public CityCollectionView Collection {
            get { return CollectionView as CityCollectionView; }
        }
        #endregion

        #region Constructors
        public CityCollectionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Save link to controller
            Collection.ParentController = this;
        }
        #endregion
    }
}

```

## <a name="putting-it-all-together"></a>まとめ 

これでがすべてをまとめる部分を作成し、コレクション ビューを制御いただくためにすべてをまとめるためのメイン ビューに、最終的な編集を行います。

編集、`ViewController.cs`ファイル (このストーリー ボードから自動的に作成された) され、次のようになります。

```csharp
using System;
using Foundation;
using UIKit;
using tvCollection;

namespace MySingleView
{
    public partial class ViewController : UIViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();
            // Perform any additional setup after loading the view, typically from a nib.
        }

        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Update image with the currently selected one
            CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
            BackgroundView.Image = CityView.Image;
            CityTitle.Text = App.SelectedCity.Title;
        }

        public override void DidReceiveMemoryWarning ()
        {
            base.DidReceiveMemoryWarning ();
            // Release any cached data, images, etc that aren't in use.
        }
        #endregion
    }
}
```

次のコードから選択した項目を最初に表示する、`SelectedCity`のプロパティ、`AppDelegate`し、ユーザーが、コレクション ビューから選択を行うときに再表示されます。

```csharp
public override void ViewWillAppear (bool animated)
{
    base.ViewWillAppear (animated);

    // Update image with the currently selected one
    CityView.Image = UIImage.FromFile(App.SelectedCity.ImageFilename);
    BackgroundView.Image = CityView.Image;
    CityTitle.Text = App.SelectedCity.Title;
}
```

<a name="Testing-the-app" />

## <a name="testing-the-app"></a>アプリのテスト

配置内のすべての場合をビルドしてアプリを実行のメイン ビューが表示され、既定の市区町村。

[ ![](collection-views-images/run01.png "メイン画面")](collection-views-images/run01.png)

場合は、ユーザーがクリックして、**ビューの選択**ボタン、コレクション ビューが表示されます。

[ ![](collection-views-images/run02.png "コレクション ビュー")](collection-views-images/run02.png)

持つ都市はすべてその`CanSelect`プロパティに設定`false`が表示されます淡色表示にし、ユーザーは、フォーカスを設定するにはできません。 ユーザーが項目を強調表示 (ようにフォーカス設定) タイトルが表示され、3 D、視差効果注意傾き、イメージを使用できます。

ユーザーをクリックすると、イメージを選択してください、コレクション ビューが閉じられのメイン ビューには、新しいイメージが再表示されます。

[ ![](collection-views-images/run03.png "ホーム画面で、新しいイメージ")](collection-views-images/run03.png)

<a name="Creating-Custom-Layout-and-Reordering-Items" />

## <a name="creating-custom-layout-and-reordering-items"></a>カスタム レイアウトを作成および項目の並べ替え

コレクション ビューを使用しての主な機能の 1 つは、カスタム レイアウトを作成する機能です。 IOS から tvOS を継承するので、カスタム レイアウトを作成するプロセスは同じです。 参照してください、[コレクション ビューの概要](~/ios/user-interface/controls/uicollectionview.md)詳細についてはドキュメントです。

IOS 用コレクション ビューに最近追加 9 が、簡単に使用できる機能のコレクションの項目の並べ替えを実行しました。 もう一度、tvOS 9 が iOS 9 のサブセットであるため、これは、これらと同じ方法です。 参照してください、[コレクションの変更の表示](~/ios/user-interface/controls/uicollectionview.md)詳細についてはドキュメントです。


<a name="Summary" />

## <a name="summary"></a>まとめ

この記事は、設計と Xamarin.tvOS アプリ内でコレクション ビューの操作について説明しました。 最初に、すべてのコレクション ビューを構成する要素を説明します。 次に、設計およびストーリー ボードを使用して、コレクション ビューを実装する方法を示しました。 カスタム レイアウトを作成して、項目を並べ替えることで、情報へのリンクが最後に、提供されます。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
