---
title: TvOS Xamarin でのコレクション ビューの操作
description: このドキュメントでは、Xamarin でビルドされた tvOS アプリでのコレクション ビューを操作する方法について説明します。 コレクション ビューのレイアウト、セルと補助ビュー、ユーザー イベント、および詳細への応答の作成について説明します。
ms.prod: xamarin
ms.assetid: 5125C4C7-2DDF-4C19-A362-17BB2B079178
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: f815afa6b1abb15348019b0c53333b4acb054008
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108029"
---
# <a name="working-with-tvos-collection-views-in-xamarin"></a>TvOS Xamarin でのコレクション ビューの操作

コレクション ビューは、コンテンツのグループを任意のレイアウトで表示されます。 組み込みのサポートを使用して可能になる、簡単に作成グリッドに似たまたは線形レイアウトもカスタム レイアウトをサポートします。

[![](collection-views-images/collection01.png "コレクション ビューのサンプル")](collection-views-images/collection01.png#lightbox)

コレクション ビューは、ユーザーとのやり取りやコレクションのコンテンツを提供するデリゲートとデータ ソースの両方を使用して、項目のコレクションを保持します。 コレクション ビューはビュー自体に依存しないレイアウト サブシステムに基づいているため、別のレイアウトを提供することができます簡単に変更コレクション ビューのデータにその場でのプレゼンテーション。

<a name="About-Collection-Views" />

## <a name="about-collection-views"></a>コレクション ビューについて

前述のコレクション ビュー (`UICollectionView`) 項目の順序付きのコレクションを管理し、カスタマイズ可能なレイアウトでそれらの項目を表示します。 コレクション ビューは、テーブルのビューと同様の方法で作業 (`UITableView`) を除き、1 つの列だけに存在する項目のレイアウトを使用できます。

アプリがデータ ソースを使用して、コレクションに関連付けられているデータを提供する責任を tvOS でコレクション ビューを使用する場合 (`UICollectionViewDataSource`)。 コレクション ビューのデータを編成必要に応じてと別のグループ (セクション) に表示されます。

コレクション ビュー セルを使用して画面上の個々 の項目を表示します (`UICollectionViewCell`) 特定のコレクション (イメージ、タイトルなど) からの情報の表示を提供します。

必要に応じて、セクションやセルのヘッダーとフッターとして機能する、コレクション ビューのプレゼンテーションに補助ビューを追加できます。 コレクション ビューのレイアウトは、これらのビューと個々 のセルの配置を定義する責任を負います。

コレクション ビューは、デリゲートを使用するユーザーとの対話に応答できます (`UICollectionViewDelegate`)。 このデリゲートは、特定のセルはセルが強調表示されている場合、フォーカスを取得できる場合、またはいずれかが選択されている場合を判断する責任を負いますもできます。 場合によっては、デリゲートは、個々 のセルのサイズを決定します。

<a name="Collection-View-Layouts" />

## <a name="collection-view-layouts"></a>コレクション ビューのレイアウト

コレクション ビューの主な機能は、表示するには、データとレイアウトの間には、その分離です。 コレクション ビューのレイアウト (`UICollectionViewLayout`) は、コレクション ビューの画面のプレゼンテーションので、組織とセル (と任意の補助ビュー) の場所を提供します。

個々 のセルは、接続されているデータ ソースからコレクション ビューによって作成し、配置し、は、指定されたコレクション ビュー レイアウトで表示されます。

コレクション ビューのレイアウトは通常、コレクション ビューが作成されるときに提供されます。 ただし、コレクション ビューのレイアウトを変更するには、いつでも、画面に表示されるコレクション ビューのデータの表示は提供されている新しいレイアウトを使用して更新自動的に。

コレクション ビューのレイアウトは、(既定のアニメーションは実行されません) の 2 つの異なるレイアウトの間の移行をアニメーション化に使用できるいくつかのメソッドを提供します。 さらに、コレクション ビューのレイアウトは、レイアウトに変更されます。 ユーザー操作をさらにアニメーション化するジェスチャ レコグナイザーを操作できます。

<a name="Creating-Cells-and-Supplementary-Views" />

## <a name="creating-cells-and-supplementary-views"></a>セルと補助ビューの作成

コレクション ビューのデータ ソースは、コンテンツを表示する使用されているセルも項目をコレクションのバックアップを作成するデータを提供する担当のみです。

コレクション ビューは、項目の大規模なコレクションを処理するために設計された、ため、個々 のセルはキューから削除され、メモリの制限をオーバーランを防ぐために再利用します。 デキューのビューの 2 つのさまざまな方法はあります。

- `DequeueReusableCell` -作成するか (アプリのストーリー ボードで指定) として指定された型のセルを返します。
- `DequeueReusableSupplementaryView` -作成または補助 (アプリのストーリー ボードで指定) として指定された型のビューを返します。

これらのメソッドのいずれかを呼び出す前に、クラスを登録する必要がありますストーリー ボードまたは`.xib`ファイル コレクション ビュー セルのビューを作成するために使用します。 例えば:

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    ...
}
```

場所`typeof(CityCollectionViewCell)`ビューをサポートするクラスを提供および`CityViewDatasource.CardCellId`セル (またはビュー) がキューから削除されたときに使用する ID を提供します。

セルがキューから削除された後は、データが表す項目を構成し、コレクション ビューの表示に戻ります。

<a name="About-Collection-View-Controllers" />

## <a name="about-collection-view-controllers"></a>コレクション ビュー コント ローラーについて

コレクション ビュー コント ローラー (`UICollectionViewController`) が専用のビュー コント ローラー (`UIViewController`)、次の動作を提供します。

- そのストーリー ボードから、コレクション ビューの読み込みを担当または`.xib`ファイルと、ビューをインスタンス化します。 コードで作成する場合、新しい未構成のコレクション ビューが自動的に作成します。
- コント ローラーがストーリー ボードから、データ ソースとデリゲートを読み込もうとコレクション ビューが読み込まれると、または`.xib`ファイル。 使用可能なない場合は、両方のソースとして自体設定します。
- によって、データが、コレクション ビューが表示されたら、最初に設定する前に読み込まれるを再読み込みと後続の各ディスプレイ上の選択をオフにします。

さらに、コレクションのビュー コント ローラーはなど、コレクション ビューのライフ サイクルの管理に使用できるオーバーライド可能なメソッドを提供します。`AwakeFromNib`と`ViewWillDisplay`します。

<a name="Collection-Views-and-Storyboards" />

## <a name="collection-views-and-storyboards"></a>コレクション ビューとストーリー ボード

Xamarin.tvOS アプリでのコレクション ビューを使用する最も簡単な方法では、そのストーリー ボードを 1 つを追加します。 簡単な例としては、イメージ、タイトル、および [選択] ボタンを表示するサンプル アプリを作成するつもりです。 場合は、ユーザーは、[選択] ボタンをクリックして、ユーザーが新しいイメージを選択できるように、コレクション ビューが表示されます。 イメージを選択した場合は、コレクション ビューは閉じられ、新しいイメージとタイトルが表示されます。

それでは、次の操作を行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

    
1. 新たに開始**1 つのビューの tvOS アプリ**Visual studio for mac。
1. **ソリューション エクスプ ローラー**、ダブルクリックして、`Main.storyboard`ファイルし、iOS Designer で開きます。
1. 既存のビューをイメージの表示、ラベルとボタンを追加し、次のように構成します。 

    [![](collection-views-images/collection02.png "サンプル レイアウト")](collection-views-images/collection02.png#lightbox)
1. 割り当てる、**名前**イメージ ビューとでラベルを**ウィジェット タブ**の**プロパティ エクスプ ローラー**します。 例えば: 

    [![](collection-views-images/collection03.png "名を設定します。")](collection-views-images/collection03.png#lightbox)
1. 次に、ストーリー ボードにコレクションのビュー コント ローラーをドラッグします。 

    [![](collection-views-images/collection04.png "コレクション ビュー コント ローラー")](collection-views-images/collection04.png#lightbox)
1. コレクション ビュー コント ローラーへのボタン コントロール ドラッグ アンド選択**プッシュ**ポップアップから。 

    [![](collection-views-images/collection05.png "ポップアップ ウィンドウからプッシュを選択します。")](collection-views-images/collection05.png#lightbox)
1. アプリを実行すると、このユーザーがボタンをクリックするたびに、表示をするコレクションの表示になります。
1. コレクション ビューを選択し、次の値を入力します、**レイアウト タブ**の**プロパティ エクスプ ローラー**: 

    [![](collection-views-images/collection06.png "プロパティ エクスプ ローラー")](collection-views-images/collection06.png#lightbox)
1. これは、個々 のセルとセルと外側のエッジ コレクション ビューの間の境界線のサイズを制御します。
1. コレクション ビュー コント ローラーを選択し、そのクラスに設定`CityCollectionViewController`で、**ウィジェット タブ**: 

    [![](collection-views-images/collection07.png "クラスを CityCollectionViewController に設定します。")](collection-views-images/collection07.png#lightbox)
1. コレクション ビューを選択し、そのクラスに設定`CityCollectionView`で、**ウィジェット タブ**: 

    [![](collection-views-images/collection08.png "クラスを CityCollectionView に設定します。")](collection-views-images/collection08.png#lightbox)
1. コレクション ビュー セルを選択し、そのクラスに設定`CityCollectionViewCell`で、**ウィジェット タブ**: 

    [![](collection-views-images/collection09.png "クラスを CityCollectionViewCell に設定します。")](collection-views-images/collection09.png#lightbox)
1. **ウィジェット タブ**いることを確認、**レイアウト**は`Flow`と**方向にスクロール**は`Vertical`コレクション ビューの。 

    [![](collection-views-images/collection10.png "[ウィジェット] タブ")](collection-views-images/collection10.png#lightbox)
1. コレクション ビューのセルを選択し、設定、 **Identity**に`CityCell`で、**ウィジェット タブ**: 

    [![](collection-views-images/collection11.png "CityCell に Id を設定します。")](collection-views-images/collection11.png#lightbox)
1. 変更内容を保存します。
    

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

    
1. 新たに開始**1 つのビューの tvOS アプリ**Visual Studio でします。
1. **ソリューション エクスプ ローラー**、ダブルクリックして、`Main.storyboard`ファイルし、iOS Designer で開きます。
1. 既存のビューをイメージの表示、ラベルとボタンを追加し、次のように構成します。 

    [![](collection-views-images/collection02vs.png "レイアウトを構成します。")](collection-views-images/collection02vs.png#lightbox)
1. 割り当てる、**名前**イメージ ビューとでラベルを**ウィジェット タブ**の**プロパティ エクスプ ローラー**します。 例えば: 

    [![](collection-views-images/collection03vs.png "プロパティ エクスプ ローラー")](collection-views-images/collection03vs.png#lightbox)
1. 次に、ストーリー ボードにコレクションのビュー コント ローラーをドラッグします。 

    [![](collection-views-images/collection04vs.png "コレクション ビュー コント ローラー")](collection-views-images/collection04vs.png#lightbox)
1. コレクション ビュー コント ローラーへのボタン コントロール ドラッグ アンド選択**プッシュ**ポップアップから。 

    [![](collection-views-images/collection05vs.png "ポップアップ ウィンドウからプッシュを選択します。")](collection-views-images/collection05vs.png#lightbox)
1. アプリを実行すると、このユーザーがボタンをクリックするたびに、表示をするコレクションの表示になります。
1. コレクション ビューを選択し、**レイアウト タブ**の**プロパティ エクスプ ローラー**入力、**幅**として_361_と**高さ**として_256_ 
1. これは、個々 のセルとセルと外側のエッジ コレクション ビューの間の境界線のサイズを制御します。
1. コレクション ビュー コント ローラーを選択し、そのクラスに設定`CityCollectionViewController`で、**ウィジェット タブ**: 

    [![](collection-views-images/collection07vs.png "クラスを CityCollectionViewController に設定します。")](collection-views-images/collection07vs.png#lightbox)
1. コレクション ビューを選択し、そのクラスに設定`CityCollectionView`で、**ウィジェット タブ**: 

    [![](collection-views-images/collection08vs.png "クラスを CityCollectionView に設定します。")](collection-views-images/collection08vs.png#lightbox)
1. コレクション ビュー セルを選択し、そのクラスに設定`CityCollectionViewCell`で、**ウィジェット タブ**: 

    [![](collection-views-images/collection09vs.png "クラスを CityCollectionViewCell に設定します。")](collection-views-images/collection09vs.png#lightbox)
1. **ウィジェット タブ**いることを確認、**レイアウト**は`Flow`と**方向にスクロール**は`Vertical`コレクション ビューの。 

    [![](collection-views-images/collection10vs.png "%T Widget タブ")](collection-views-images/collection10vs.png#lightbox)
1. コレクション ビューのセルを選択し、設定、 **Identity**に`CityCell`で、**ウィジェット タブ**: 

    [![](collection-views-images/collection11vs.png "CityCell に Id を設定します。")](collection-views-images/collection11vs.png#lightbox)
1. 変更内容を保存します。
    

-----

選択した場合は`Custom`のコレクション ビューの**レイアウト**、カスタム レイアウトを指定できます。 Apple から提供されて、組み込み`UICollectionViewFlowLayout`と`UICollectionViewDelegateFlowLayout`グリッド ベースのレイアウトでデータを簡単にプレゼンテーションすることができます (これらによって使用されます、`flow`レイアウト スタイル)。 

ストーリー ボードの操作方法の詳細についてを参照してください、[はじめての tvOS クイック スタート ガイド](~/ios/tvos/get-started/hello-tvos.md)します。

<a name="Providing-Data-for-the-Collection-View" />

## <a name="providing-data-for-the-collection-view"></a>コレクション ビューのデータを提供します。

ストーリー ボードに追加された、コレクション ビュー (および Collection View Controller) したら、コレクションにデータを提供する必要があります。 

<a name="The-Data-Model" />

### <a name="the-data-model"></a>データ モデル

最初に、ここのタイトルとを選択する市区町村を許可するフラグを表示する画像のファイル名を保持するデータのモデルを作成します。

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

### <a name="the-collection-view-cell"></a>コレクション ビュー セル

各セルのデータの表示方法を定義する必要があります。 編集、 `CityCollectionViewCell.cs` (から作成されたファイルが自動的に、ストーリー ボード ファイル) と、次のようになります。

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

TvOS アプリでは、私たちは表示しているイメージとオプションのタイトル。 特定の都市を選択することはできません、次のコードを使用してイメージの表示を暗転しましたは。

```csharp
CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
```

組み込みを使用するイメージを含むセルがフォーカス設定によって、ユーザーになることを視差効果は、次のプロパティを設定します。

```csharp
CityView.AdjustsImageWhenAncestorFocused = true;
```

ナビゲーションとフォーカスの詳細についてを参照してください、[ナビゲーションとフォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)と[Siri のリモートと Bluetooth コント ローラー](~/ios/tvos/platform/remote-bluetooth.md)ドキュメント。


<a name="The-Collection-View-Data-Provider" />

### <a name="the-collection-view-data-provider"></a>コレクション ビューのデータ プロバイダー

作成、データ モデルと、定義されたセルのレイアウトでは、このコレクション ビューのデータ ソースを作成しましょう。 データ ソースは、画面上の個々 のセルを表示するセル、バックアップ データがまたデキューがだけでなく提供されます。

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

このクラスの詳細を見ることができます。 継承する最初に、 `UICollectionViewDataSource` (iOS Designer で割り当てた) するセル ID へのショートカットを提供します。

```csharp
public static NSString CardCellId = new NSString ("CityCell");
```

次に記憶域、コレクション データを提供し、データを設定するクラスを提供します。

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

無効にし、`NumberOfSections`メソッドと、コレクションを表示するセクション (項目のグループ) の数が返された場合。 この場合は、1 つしかないです。

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    return 1;
}
```

次に、次のコードを使用して、コレクション内の項目数を返します。

```csharp
public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    return Cities.Count;
}
```

最後に、再利用可能なセルは、コレクション ビューは、次のコードで要求時にデキューします。

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

Collection View Cell を取得したら、`CityCollectionViewCell`の種類そこに特定の項目。

<a name="Responding-to-User-Events" />

## <a name="responding-to-user-events"></a>ユーザー イベントに応答します。

ユーザー コレクションから項目を選択できるため、この操作を処理するコレクション ビュー デリゲートを用意する必要があります。 呼び出し元をビューにどのような項目をユーザーに通知できるようにする方法の選択を提供する必要があります。

<a name="The-App-Delegate" />

### <a name="the-app-delegate"></a>アプリ デリゲート

コレクション ビューから、呼び出し元のビューに現在選択されている項目を関連付ける方法が必要です。 使用するカスタム プロパティで、`AppDelegate`します。 編集、`AppDelegate.cs`ファイルを開き、次のコードを追加します。

```csharp
public CityInfo SelectedCity { get; set;} = new CityInfo("City02.jpg", "Turning Circle", true);
```

これにより、プロパティを定義し、最初に表示される既定の市区町村を設定します。 後で、ユーザーの選択内容を表示および変更する選択を許可するには、このプロパティを消費します。

<a name="The-Collection-View-Delegate" />

### <a name="the-collection-view-delegate"></a>コレクション ビューのデリゲート

次に、新しい追加`CityViewDelegate`をプロジェクトにクラスし、次のようになります。


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

このクラスについて詳しく見てをみましょう。 継承する最初に、`UICollectionViewDelegateFlowLayout`します。 このクラスから継承しました理由なく、`UICollectionViewDelegate`組み込みが使用されているの`UICollectionViewFlowLayout`なアイテムおよびカスタム レイアウト型ではありません。

次に、このコードを使用して個々 の項目のサイズを返します。

```csharp
public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
{
    return new CGSize (361, 256);
}
```

次にかどうか、特定のセルは、次のコードを使用してフォーカスを取得できますを決定します。 

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

特定のバックアップ データが含まれていることを確認します、`CanSelect`フラグに設定`true`値を返します。 ナビゲーションとフォーカスの詳細についてを参照してください、[ナビゲーションとフォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)と[Siri のリモートと Bluetooth コント ローラー](~/ios/tvos/platform/remote-bluetooth.md)ドキュメント。

最後に、次のコードを持つ項目を選択すると、ユーザーに対応します。

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    var controller = collectionView as CityCollectionView;
    App.SelectedCity = controller.Source.Cities [indexPath.Row];

    // Close Collection
    controller.ParentController.DismissViewController(true,null);
}
```

ここでは設定、`SelectedCity`のプロパティ、`AppDelegate`と選択したユーザーを終了するコレクションのビュー コント ローラーと呼ばれることをビューに戻る項目にします。 まだ定義していない、`ParentController`まだこのコレクション ビューのプロパティは次にそのします。

<a name="Configuring-the-Collection-View" />

## <a name="configuring-the-collection-view"></a>コレクション ビューを構成します。

コレクション ビューを編集し、データ ソースとデリゲートを割り当てる必要があります。 編集、 `CityCollectionView.cs` (から作成されたファイルを自動的に、ストーリー ボード) と、次のようになります。

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

まずにアクセスするショートカットを入力しました、 `AppDelegate`: 

```csharp
public static AppDelegate App {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
```

次に、コレクション ビューのデータ ソースとプロパティ (上記のデリゲートで、ユーザーは、選択されたときに、コレクションを閉じるの使用)、コレクションのビュー コント ローラーにアクセスするショートカットを提供します。

```csharp
public CityViewDatasource Source {
    get { return DataSource as CityViewDatasource;}
}

public CityCollectionViewController ParentController { get; set;}
```

次に、次のコードを使用をコレクション ビューを初期化して、セル クラス、データ ソース、およびデリゲートを割り当てます。

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    DataSource = new CityViewDatasource (this);
    Delegate = new CityViewDelegate ();
}
```

最後に、必要、ユーザーが強調表示されているときに表示されるのみにイメージの下のタイトル (フォーカス設定)。 次のコードを実行します。

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

前の項目 0 (ゼロ) にフォーカスを失うの transparence を設定しますし、次の項目の transparence が 100% にフォーカスを取得します。 これらの遷移の取得もアニメーション化されます。


## <a name="configuring-the-collection-view-controller"></a>コレクション ビュー コント ローラーを構成します。

コレクション ビューに最終的な構成を行い、ユーザーは、選択した後、コレクション ビューを閉じることができるように定義したプロパティを設定するコント ローラーを許可する必要があります。

編集、 `CityCollectionViewController.cs` (ストーリー ボードから自動的に作成された) ファイルと、次のようになります。

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

できたすべてまとめた部分を設定し、コレクション ビューを制御する必要がありますすべてを一緒に、メイン ビューに最終的な編集を加えます。

編集、 `ViewController.cs` (ストーリー ボードから自動的に作成された) ファイルと、次のようになります。

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

次のコードでは、選択したアイテムを最初に表示されます、`SelectedCity`のプロパティ、`AppDelegate`ユーザーがコレクション ビューから選択を行うときに再表示されるとします。

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

すべての準備をビルドして、アプリを実行する場合、既定の市区町村とメイン ビューが表示します。

[![](collection-views-images/run01.png "メイン画面")](collection-views-images/run01.png#lightbox)

ユーザーがクリックした場合、**ビューを選択します**ボタン、コレクション ビューが表示されます。

[![](collection-views-images/run02.png "コレクション ビュー")](collection-views-images/run02.png#lightbox)

任意の市がその`CanSelect`プロパティに設定`false`が表示されます淡色表示にし、ユーザーは、フォーカスを設定するにはできません。 ユーザーが項目を強調表示 (ようにインフォーカス) タイトルを表示して、3 D、視差効果はらみます傾き、イメージを使用できます。

ユーザーは、イメージの選択をクリックすると、コレクション ビューが閉じられるし、メイン ビューには、新しいイメージが再表示されます。

[![](collection-views-images/run03.png "ホーム画面で新しいイメージ")](collection-views-images/run03.png#lightbox)

<a name="Creating-Custom-Layout-and-Reordering-Items" />

## <a name="creating-custom-layout-and-reordering-items"></a>カスタム レイアウトを作成して、項目の並べ替え

コレクション ビューを使用しての主な機能の 1 つは、カスタム レイアウトを作成する機能です。 IOS から tvOS の継承元であるため、カスタム レイアウトを作成するプロセスは同じです。 参照してください、[コレクション ビューの概要](~/ios/user-interface/controls/uicollectionview.md)詳細についてはドキュメントです。

IOS 用のコレクション ビューに最近追加 9 は、コレクション内の項目を並べ替えることが簡単にする機能です。 ここでも、tvOS 9 が iOS 9 のサブセットであるため、これは、それらと同じ方法です。 参照してください、[コレクションの変更の表示](~/ios/user-interface/controls/uicollectionview.md)詳細についてはドキュメントです。


<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、設計とコレクション ビュー Xamarin.tvOS アプリ内での操作について説明しました。 最初に、すべてのコレクション ビューを構成する要素を説明します。 次に、設計およびストーリー ボードを使用してコレクション ビューを実装する方法を示しました。 カスタム レイアウトを作成して、項目の並べ替えでは、情報へのリンクが最後に、提供されます。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
