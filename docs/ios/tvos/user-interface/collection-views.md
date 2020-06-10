---
title: Xamarin での tvOS Collection ビューの使用
description: このドキュメントでは、Xamarin でビルドされた tvOS アプリでコレクションビューを操作する方法について説明します。 コレクションビューのレイアウト、セルと補助ビューの作成、ユーザーイベントへの対応などについて説明します。
ms.prod: xamarin
ms.assetid: 5125C4C7-2DDF-4C19-A362-17BB2B079178
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: aa03ab7a3663fa5e0704a605116b19147f14a10b
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572586"
---
# <a name="working-with-tvos-collection-views-in-xamarin"></a>Xamarin での tvOS Collection ビューの使用

コレクションビューでは、任意のレイアウトを使用してコンテンツのグループを表示できます。 組み込みのサポートを使用すると、簡単に作成できるグリッドのようなレイアウトや、カスタムレイアウトをサポートすることができます。

[![](collection-views-images/collection01.png "Sample collection view")](collection-views-images/collection01.png#lightbox)

コレクションビューでは、ユーザーの操作とコレクションの内容を提供するために、デリゲートとデータソースの両方を使用して項目のコレクションを保持します。 コレクションビューはビュー自体に依存しないレイアウトサブシステムに基づいているため、異なるレイアウトを指定すると、コレクションビューのデータの表示を即座に変更できます。

<a name="About-Collection-Views"></a>

## <a name="about-collection-views"></a>コレクションビューについて

既に説明したように、コレクションビュー () は、 `UICollectionView` 順序付けられた項目のコレクションを管理し、カスタマイズ可能なレイアウトでそれらの項目を表示します。 コレクションビューは、テーブルビュー () と同様に機能し `UITableView` ますが、1つの列だけでなく、レイアウトを使用して項目を表示することもできます。

TvOS でコレクションビューを使用する場合、アプリは、データソース () を使用してコレクションに関連付けられたデータを提供する役割を担い `UICollectionViewDataSource` ます。 コレクションビューデータは、必要に応じて、さまざまなグループ (セクション) に整理して表示できます。

コレクションビューでは、オブジェクト () を使用して個々の項目が画面上に表示されます。このセルは、 `UICollectionViewCell` コレクションの特定の情報 (イメージやタイトルなど) を表示します。

必要に応じて、補足ビューをコレクションビューのプレゼンテーションに追加して、セクションとセルのヘッダーとフッターとして機能させることができます。 コレクションビューのレイアウトでは、個々のセルと共にこれらのビューの配置を定義します。

コレクションビューは、デリゲート () を使用したユーザーの操作に応答でき `UICollectionViewDelegate` ます。 このデリゲートは、セルが強調表示されているかどうか、またはセルが選択されているかどうかを判断する役割も担います。 場合によっては、デリゲートが個々のセルのサイズを決定します。

<a name="Collection-View-Layouts"></a>

## <a name="collection-view-layouts"></a>コレクションビューのレイアウト

コレクションビューの主な機能は、表示されているデータとそのレイアウトを分離することです。 コレクションビューのレイアウト ( `UICollectionViewLayout` ) は、コレクションビューの画面表示で、を使用して、組織とセルの場所 (およびすべての補助ビュー) を提供します。

個々のセルは、割り当てられたデータソースからコレクションビューによって作成され、指定されたコレクションビューのレイアウトによって並べ替えられて表示されます。

コレクションビューのレイアウトは、通常、コレクションビューの作成時に提供されます。 ただし、コレクションビューのレイアウトはいつでも変更でき、コレクションビューのデータの画面上の表示は、提供された新しいレイアウトを使用して自動的に更新されます。

コレクションビューレイアウトには、2つの異なるレイアウト間の遷移をアニメーション化するために使用できるいくつかのメソッドが用意されています (既定では、アニメーションは実行されません)。 また、コレクションビューのレイアウトでは、ジェスチャレコグナイザーを使用して、ユーザーの操作をさらにアニメーション化し、レイアウトを変更することができます。

<a name="Creating-Cells-and-Supplementary-Views"></a>

## <a name="creating-cells-and-supplementary-views"></a>セルと補助ビューの作成

コレクションビューのデータソースは、コレクションのアイテムをバッキングするデータを提供するだけでなく、コンテンツの表示に使用されるセルにも対応します。

コレクションビューは、アイテムの大きなコレクションを処理するように設計されているため、個別のセルをデキューして再利用し、オーバーランのメモリ制限を維持することができます。 ビューを解除するには、次の2つの方法があります。

- `DequeueReusableCell`-指定された型 (アプリのストーリーボードで指定) のセルを作成または返します。
- `DequeueReusableSupplementaryView`-指定された種類の補助ビューを作成または返します (アプリのストーリーボードで指定)。

これらのメソッドのいずれかを呼び出す前に、 `.xib` コレクションビューでセルのビューを作成するために使用されるクラス、ストーリーボード、またはファイルを登録する必要があります。 次に例を示します。

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    ...
}
```

ここ `typeof(CityCollectionViewCell)` で、はビューをサポートするクラスを提供し、 `CityViewDatasource.CardCellId` セル (またはビュー) がデキューされるときに使用される ID を提供します。

セルをデキューした後、それが表すアイテムのデータを使用して構成し、表示するコレクションビューに戻ります。

<a name="About-Collection-View-Controllers"></a>

## <a name="about-collection-view-controllers"></a>コレクションビューコントローラーについて

コレクションビューコントローラー ( `UICollectionViewController` ) は、次の動作を提供する特殊なビューコントローラー ( `UIViewController` ) です。

- これは、ストーリーボードまたはファイルからコレクションビューを読み込んで、ビューをインスタンス化する役割を担い `.xib` ます。 コードで作成された場合は、新しい未構成のコレクションビューが自動的に作成されます。
- コレクションビューが読み込まれると、コントローラーは、そのデータソースを読み込み、ストーリーボードまたはファイルからデリゲートを試行し `.xib` ます。 使用できるものがない場合は、その両方のソースとして設定されます。
- 最初に表示されたときにコレクションビューが設定される前にデータが読み込まれるようにし、後続の各表示に対して select を再読み込みしてクリアします。

また、コレクションビューコントローラーには、やなどのコレクションビューのライフサイクルを管理するために使用できる、オーバーライド可能なメソッドが用意されて `AwakeFromNib` `ViewWillDisplay` います。

<a name="Collection-Views-and-Storyboards"></a>

## <a name="collection-views-and-storyboards"></a>コレクションビューとストーリーボード

TvOS アプリでコレクションビューを操作する最も簡単な方法は、ストーリーボードに1つを追加することです。 簡単な例として、イメージ、タイトル、および選択ボタンを表示するサンプルアプリを作成します。 ユーザーが [選択] ボタンをクリックすると、コレクションビューが表示され、ユーザーは新しい画像を選択できるようになります。 イメージを選択すると、コレクションビューが閉じられ、新しいイメージとタイトルが表示されます。

次の手順を実行してみましょう。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio for Mac で新しい**単一ビュー TvOS アプリ**を開始します。
1. **ソリューションエクスプローラー**で、ファイルをダブルクリックして、 `Main.storyboard` iOS Designer で開きます。
1. イメージビュー、ラベル、ボタンを既存のビューに追加し、次のように構成します。 

    [![](collection-views-images/collection02.png "Sample layout")](collection-views-images/collection02.png#lightbox)
1. [**プロパティエクスプローラー**] の [**ウィジェット] タブ**で、イメージビューとラベルに**名前**を割り当てます。 次に例を示します。 

    [![](collection-views-images/collection03.png "Setting the name")](collection-views-images/collection03.png#lightbox)
1. 次に、コレクションビューコントローラーをストーリーボードにドラッグします。 

    [![](collection-views-images/collection04.png "A Collection View Controller")](collection-views-images/collection04.png#lightbox)
1. コントロール-ボタンからコレクションビューコントローラーにドラッグし、ポップアップから [**プッシュ**] を選択します。 

    [![](collection-views-images/collection05.png "Select Push from the popup")](collection-views-images/collection05.png#lightbox)
1. アプリを実行すると、ユーザーがボタンをクリックするたびにコレクションビューが表示されるようになります。
1. コレクションビューを選択し、**プロパティエクスプローラー**の [**レイアウト] タブ**で次の値を入力します。 

    [![](collection-views-images/collection06.png "The Properties Explorer")](collection-views-images/collection06.png#lightbox)
1. これにより、個々のセルのサイズと、コレクションビューの外部境界との間の境界線を制御します。
1. コレクションビューコントローラーを選択し、[ `CityCollectionViewController` **ウィジェット] タブ**でクラスをに設定します。 

    [![](collection-views-images/collection07.png "Set the class to CityCollectionViewController")](collection-views-images/collection07.png#lightbox)
1. コレクションビューを選択し、[ `CityCollectionView` **ウィジェット] タブ**でクラスをに設定します。 

    [![](collection-views-images/collection08.png "Set the class to CityCollectionView")](collection-views-images/collection08.png#lightbox)
1. [コレクションビュー] セルを選択し、[ `CityCollectionViewCell` **ウィジェット] タブ**でクラスをに設定します。 

    [![](collection-views-images/collection09.png "Set the class to CityCollectionViewCell")](collection-views-images/collection09.png#lightbox)
1. [**ウィジェット] タブ**で、**レイアウト**がであること、 `Flow` およびコレクションビューの**スクロール方向**がであることを確認し `Vertical` ます。 

    [![](collection-views-images/collection10.png "The Widget Tab")](collection-views-images/collection10.png#lightbox)
1. [ **Identity** `CityCell` **ウィジェット] タブ**で、[コレクションビュー] セルを選択し、その id をに設定します。 

    [![](collection-views-images/collection11.png "Set the Identity to CityCell")](collection-views-images/collection11.png#lightbox)
1. 変更内容を保存します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Visual Studio で新しい**単一ビュー TvOS アプリ**を開始します。
1. **ソリューションエクスプローラー**で、ファイルをダブルクリックして、 `Main.storyboard` iOS Designer で開きます。
1. イメージビュー、ラベル、ボタンを既存のビューに追加し、次のように構成します。 

    [![](collection-views-images/collection02vs.png "Configure the layout")](collection-views-images/collection02vs.png#lightbox)
1. [**プロパティエクスプローラー**] の [**ウィジェット] タブ**で、イメージビューとラベルに**名前**を割り当てます。 次に例を示します。 

    [![](collection-views-images/collection03vs.png "The Properties Explorer")](collection-views-images/collection03vs.png#lightbox)
1. 次に、コレクションビューコントローラーをストーリーボードにドラッグします。 

    [![](collection-views-images/collection04vs.png "A Collection View Controller")](collection-views-images/collection04vs.png#lightbox)
1. コントロール-ボタンからコレクションビューコントローラーにドラッグし、ポップアップから [**プッシュ**] を選択します。 

    [![](collection-views-images/collection05vs.png "Select Push from the popup")](collection-views-images/collection05vs.png#lightbox)
1. アプリを実行すると、ユーザーがボタンをクリックするたびにコレクションビューが表示されるようになります。
1. コレクションビューを選択し、**プロパティエクスプローラー**の [**レイアウト] タブ**で、**幅**を_361_ 、**高さ**を_256_として入力します。 
1. これにより、個々のセルのサイズと、コレクションビューの外部境界との間の境界線を制御します。
1. コレクションビューコントローラーを選択し、[ `CityCollectionViewController` **ウィジェット] タブ**でクラスをに設定します。 

    [![](collection-views-images/collection07vs.png "Set the class to CityCollectionViewController")](collection-views-images/collection07vs.png#lightbox)
1. コレクションビューを選択し、[ `CityCollectionView` **ウィジェット] タブ**でクラスをに設定します。 

    [![](collection-views-images/collection08vs.png "Set the class to CityCollectionView")](collection-views-images/collection08vs.png#lightbox)
1. [コレクションビュー] セルを選択し、[ `CityCollectionViewCell` **ウィジェット] タブ**でクラスをに設定します。 

    [![](collection-views-images/collection09vs.png "Set the class to CityCollectionViewCell")](collection-views-images/collection09vs.png#lightbox)
1. [**ウィジェット] タブ**で、**レイアウト**がであること、 `Flow` およびコレクションビューの**スクロール方向**がであることを確認し `Vertical` ます。 

    [![](collection-views-images/collection10vs.png "Tthe Widget Tab")](collection-views-images/collection10vs.png#lightbox)
1. [ **Identity** `CityCell` **ウィジェット] タブ**で、[コレクションビュー] セルを選択し、その id をに設定します。 

    [![](collection-views-images/collection11vs.png "Set the Identity to CityCell")](collection-views-images/collection11vs.png#lightbox)
1. 変更内容を保存します。

-----

`Custom`コレクションビューの**レイアウト**を選択した場合は、カスタムレイアウトを指定することもできます。 Apple には組み込みのが用意されて `UICollectionViewFlowLayout` おり、 `UICollectionViewDelegateFlowLayout` グリッドベースのレイアウトでデータを簡単に表示できます (レイアウトスタイルで使用され `flow` ます)。 

ストーリーボードの操作の詳細については、「 [Hello, tvOS クイックスタートガイド](~/ios/tvos/get-started/hello-tvos.md)」を参照してください。

<a name="Providing-Data-for-the-Collection-View"></a>

## <a name="providing-data-for-the-collection-view"></a>コレクションビューにデータを提供しています

これで、コレクションビュー (およびコレクションビューコントローラー) がストーリーボードに追加されたので、コレクションのデータを指定する必要があります。 

<a name="The-Data-Model"></a>

### <a name="the-data-model"></a>データモデル

まず、表示するイメージのファイル名、タイトル、および市区町村を選択できるようにするためのフラグを保持するデータのモデルを作成します。

クラスを作成 `CityInfo` し、次のようにします。

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

### <a name="the-collection-view-cell"></a>コレクションビューセル

次に、各セルにデータを表示する方法を定義する必要があります。 `CityCollectionViewCell.cs`(ストーリーボードファイルから自動的に作成された) ファイルを編集し、次のように表示します。

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

TvOS アプリでは、イメージとオプションのタイトルが表示されます。 指定した市区町村を選択できない場合は、次のコードを使用してイメージビューを淡色表示します。

```csharp
CityView.Alpha = (City.CanSelect) ? 1.0f : 0.5f;
```

イメージを含むセルがユーザーによってフォーカスされているときは、組み込みの視差効果を使用して、次のプロパティを設定する必要があります。

```csharp
CityView.AdjustsImageWhenAncestorFocused = true;
```

ナビゲーションとフォーカスの詳細については、「[ナビゲーションとフォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)、 [Siri リモートおよび Bluetooth コントローラー](~/ios/tvos/platform/remote-bluetooth.md)の操作」を参照してください。

<a name="The-Collection-View-Data-Provider"></a>

### <a name="the-collection-view-data-provider"></a>コレクションビュー Data Provider

データモデルを作成し、セルレイアウトを定義したので、コレクションビューのデータソースを作成しましょう。 データソースは、バッキングデータを提供するだけでなく、セルのキューを解除して個々のセルを画面上に表示することもできます。

クラスを作成 `CityViewDatasource` し、次のようにします。

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

このクラスを詳しく見てみましょう。 まず、から継承 `UICollectionViewDataSource` し、セル ID (IOS デザイナーで割り当てたもの) へのショートカットを提供します。

```csharp
public static NSString CardCellId = new NSString ("CityCell");
```

次に、コレクションデータのストレージを提供し、データを設定するためのクラスを提供します。

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

次に、メソッドをオーバーライド `NumberOfSections` し、コレクションビューに含まれているセクション (項目のグループ) の数を返します。 この場合は、次の1つだけです。

```csharp
public override nint NumberOfSections (UICollectionView collectionView)
{
    return 1;
}
```

次に、次のコードを使用して、コレクション内の項目の数を返します。

```csharp
public override nint GetItemsCount (UICollectionView collectionView, nint section)
{
    return Cities.Count;
}
```

最後に、コレクションビュー要求で次のコードを使用して、再利用可能なセルをデキューします。

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

型のコレクションビューのセルを取得した後 `CityCollectionViewCell` 、指定した項目をそのセルに設定します。

<a name="Responding-to-User-Events"></a>

## <a name="responding-to-user-events"></a>ユーザーイベントへの応答

ユーザーはコレクションから項目を選択できるようにするため、この相互作用を処理するコレクションビューデリゲートを指定する必要があります。 また、ユーザーが選択した項目を呼び出し元のビューが認識できるようにする方法を提供する必要があります。

<a name="The-App-Delegate"></a>

### <a name="the-app-delegate"></a>アプリのデリゲート

現在選択されている項目をコレクションビューから呼び出し元のビューに関連付ける方法が必要です。 ここでは、カスタムプロパティを使用し `AppDelegate` ます。 ファイルを編集 `AppDelegate.cs` し、次のコードを追加します。

```csharp
public CityInfo SelectedCity { get; set;} = new CityInfo("City02.jpg", "Turning Circle", true);
```

これは、プロパティを定義し、最初に表示される既定の都市を設定します。 後で、このプロパティを使用してユーザーの選択を表示し、選択を変更できるようにします。

<a name="The-Collection-View-Delegate"></a>

### <a name="the-collection-view-delegate"></a>コレクションビューデリゲート

次に、新しい `CityViewDelegate` クラスをプロジェクトに追加し、次のように表示します。

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

このクラスについて詳しく見ていきましょう。 まず、から継承 `UICollectionViewDelegateFlowLayout` します。 ではなく、このクラスから継承する理由は、 `UICollectionViewDelegate` 組み込みのを使用して、 `UICollectionViewFlowLayout` カスタムレイアウトの種類ではなく項目を表示するためです。

次に、このコードを使用して個々の項目のサイズを返します。

```csharp
public override CGSize GetSizeForItem (UICollectionView collectionView, UICollectionViewLayout layout, NSIndexPath indexPath)
{
    return new CGSize (361, 256);
}
```

次に、次のコードを使用して、特定のセルにフォーカスを移すことができるかどうかを決定します。 

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

バッキングデータの特定の部分のフラグがに設定されているかどうかを確認 `CanSelect` `true` し、その値を返します。 ナビゲーションとフォーカスの詳細については、「[ナビゲーションとフォーカス](~/ios/tvos/app-fundamentals/navigation-focus.md)、 [Siri リモートおよび Bluetooth コントローラー](~/ios/tvos/platform/remote-bluetooth.md)の操作」を参照してください。

最後に、次のコードを使用して項目を選択するユーザーに応答します。

```csharp
public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    var controller = collectionView as CityCollectionView;
    App.SelectedCity = controller.Source.Cities [indexPath.Row];

    // Close Collection
    controller.ParentController.DismissViewController(true,null);
}
```

ここでは、 `SelectedCity` `AppDelegate` ユーザーが選択した項目にのプロパティを設定し、コレクションビューコントローラーを閉じて、us という名前のビューに戻ります。 `ParentController`コレクションビューのプロパティはまだ定義されていませんが、次のようにします。

<a name="Configuring-the-Collection-View"></a>

## <a name="configuring-the-collection-view"></a>コレクションビューの構成

ここで、コレクションビューを編集し、データソースとデリゲートを割り当てる必要があります。 `CityCollectionView.cs`(ストーリーボードから自動的に作成された) ファイルを編集し、次のように表示します。

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

まず、にアクセスするためのショートカットが用意されてい `AppDelegate` ます。 

```csharp
public static AppDelegate App {
    get { return (AppDelegate)UIApplication.SharedApplication.Delegate; }
}
```

次に、コレクションビューのデータソースへのショートカットと、コレクションビューコントローラーにアクセスするためのプロパティを提供します (ユーザーが選択したときに、上記のデリゲートによってコレクションを閉じるために使用されます)。

```csharp
public CityViewDatasource Source {
    get { return DataSource as CityViewDatasource;}
}

public CityCollectionViewController ParentController { get; set;}
```

次に、次のコードを使用してコレクションビューを初期化し、セルクラス、データソース、およびデリゲートを割り当てます。

```csharp
public CityCollectionView (IntPtr handle) : base (handle)
{
    // Initialize
    RegisterClassForCell (typeof(CityCollectionViewCell), CityViewDatasource.CardCellId);
    DataSource = new CityViewDatasource (this);
    Delegate = new CityViewDelegate ();
}
```

最後に、イメージの下のタイトルは、ユーザーが強調表示されている場合にのみ表示されるようにします (フォーカス)。 これを行うには、次のコードを使用します。

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

前の項目の transparence を0に設定し、次の項目の transparence にフォーカスを100% に設定します。 これらの移行もアニメーション化されます。

## <a name="configuring-the-collection-view-controller"></a>コレクションビューコントローラーの構成

次に、コレクションビューの最後の構成を行い、ユーザーが選択した後にコレクションビューを閉じることができるように定義したプロパティをコントローラーが設定できるようにする必要があります。

`CityCollectionViewController.cs`(ストーリーボードから自動的に作成された) ファイルを編集し、次のように表示します。

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

すべてのパーツをまとめて、コレクションビューを設定し、制御します。次に、すべてをまとめて表示するために、メインビューに対して最後の編集を行う必要があります。

`ViewController.cs`(ストーリーボードから自動的に作成された) ファイルを編集し、次のように表示します。

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

次のコードは、最初にのプロパティから選択された項目を表示 `SelectedCity` `AppDelegate` し、ユーザーがコレクションビューから選択したときに再表示します。

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

<a name="Testing-the-app"></a>

## <a name="testing-the-app"></a>アプリのテスト

すべての場所で、アプリをビルドして実行すると、メインビューが既定の city と共に表示されます。

[![](collection-views-images/run01.png "The main screen")](collection-views-images/run01.png#lightbox)

ユーザーが [ビューの**選択**] ボタンをクリックすると、コレクションビューが表示されます。

[![](collection-views-images/run02.png "The collection view")](collection-views-images/run02.png#lightbox)

プロパティがに設定されているすべての都市 `CanSelect` `false` が淡色表示され、ユーザーはフォーカスを設定できなくなります。 ユーザーが項目を強調表示する (フォーカスを設定する) と、タイトルが表示され、視差効果を使用して3D で画像をはらみ傾けることができます。

ユーザーが選択したイメージをクリックすると、コレクションビューが閉じられ、メインビューが新しいイメージと共に再表示されます。

[![](collection-views-images/run03.png "A new image on the home screen")](collection-views-images/run03.png#lightbox)

<a name="Creating-Custom-Layout-and-Reordering-Items"></a>

## <a name="creating-custom-layout-and-reordering-items"></a>カスタムレイアウトの作成と項目の並べ替え

コレクションビューを使用する主な機能の1つに、カスタムレイアウトを作成する機能があります。 TvOS は iOS から継承するため、カスタムレイアウトを作成するプロセスは同じです。 詳細については、[コレクションビューの概要に](~/ios/user-interface/controls/uicollectionview.md)関するドキュメントを参照してください。

IOS 9 のコレクションビューに最近追加されたのは、コレクション内の項目の並べ替えを簡単に許可することでした。 ここでも、tvOS 9 は iOS 9 のサブセットであるため、同じように実行されます。 詳細については、[コレクションビューの変更](~/ios/user-interface/controls/uicollectionview.md)に関するドキュメントを参照してください。

<a name="Summary"></a>

## <a name="summary"></a>まとめ

この記事では、tvOS アプリ内のコレクションビューの設計と操作について説明しました。 まず、コレクションビューを構成するすべての要素について説明しました。 次に、ストーリーボードを使用してコレクションビューをデザインおよび実装する方法について説明しました。 最後に、カスタムレイアウトの作成と項目の並べ替えに関する情報へのリンクが用意されています。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
