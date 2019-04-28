---
title: 表形式ビュー Xamarin で tvOS の操作
description: この記事では、設計と Xamarin.tvOS アプリ内でテーブルのビューおよびテーブル ビュー コント ローラーの操作について説明します。
ms.prod: xamarin
ms.assetid: D8F80FA9-6400-4DB7-AFC9-A28A54AD04E8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 3e7fc3d627b5d7a1dc73caa395a9181efb0b5f08
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61356136"
---
# <a name="working-with-tvos-table-views-in-xamarin"></a>表形式ビュー Xamarin で tvOS の操作

_この記事では、設計と Xamarin.tvOS アプリ内でテーブルのビューおよびテーブル ビュー コント ローラーの操作について説明します。_

TvOS では、テーブル ビューがグループまたはセクションに整理できます必要に応じて行のスクロールの 1 つの列として表示されます。 方法を理解するクリア テキストで、ユーザーに効率的に大量のデータを表示する必要がある場合、テーブル ビューを使用する必要があります。

テーブルのビューは通常の一方の側で表示されます、[分割ビュー](~/ios/tvos/user-interface/split-views.md)反対側に表示される選択した項目の詳細のナビゲーションとして。

[![](table-views-images/intro01.png "サンプル テーブルの表示")](table-views-images/intro01.png#lightbox)

<a name="About-Table-Views" />

## <a name="about-table-views"></a>テーブルのビューについて

A`UITableView`スクロール可能な行の 1 つの列をグループまたはセクションに整理できます必要に応じて情報の階層リストとして表示されます。 

[![](table-views-images/table01.png "選択された項目")](table-views-images/table01.png#lightbox)

Apple では、テーブルを操作するための次の推奨事項があります。

- **幅の対応する**-テーブルの幅のバランスを取るように再試行してください。 テーブルが広すぎる場合は、難しいを離れた場所からスキャンして、使用可能なコンテンツ エリアから除外すべきことができます。 テーブルが狭すぎる場合は、情報が切り捨てられることになりますか、ラップ、もう一度この困難であるユーザーは、部屋から読み取る。
- **内容を簡単にテーブルを表示する**- データの大きなリストの遅延読み込み、コンテンツと、テーブルが、ユーザーに表示されるとすぐに情報を表示を開始します。 この表には読み込みに時間をユーザーはアプリや待ち時間のロックされていることに関心を失う可能性があります。
- **ユーザーの長いコンテンツの読み込みに通知**- 長いテーブルの読み込み時間が避けられない、存在する場合は、[進行状況バーやアクティビティのインジケーター](~/ios/tvos/user-interface/progress-indicators.md)アプリを把握できるようにはしていないロックします。

<a name="Table-Cell-Types" />

## <a name="table-view-cell-types"></a>テーブル セルの種類を表示

A`UITableViewCell`個々 のテーブル ビューでのデータ行を表すために使用します。 Apple では、既定のテーブル セルの種類をいくつか定義されています。

- **既定の**- このオプションは、セルと右側のタイトルを左揃えの左側にあるイメージの種類を表示します。 
- **サブタイトル**- 左揃えのタイトルを最初の行より小さい左揃えサブタイトル次の行でこの型を表示します。
- **値 1** -この種類は、同じ行に左揃え、右揃えの軽量の色付きのサブタイトルを含むタイトルを表示します。
- **値 2** -この種類は、同じ行に、左揃えの軽量の色付きの字幕を右揃えのタイトルを表示します。

既定のテーブル ビューのセルの種類のすべても漏えいインジケーターまたはチェック マークなどのグラフィック要素をサポートします。 

また、定義することができます、**カスタム**テーブル ビューのセルの種類と現在のところ、_プロトタイプ セル_、いずれかまたは作成することでインターフェイス デザイナー コードを使用して。

Apple では、テーブル セルの表示を操作するための次の推奨事項があります。

- **テキストのクリッピングを回避**-切り捨てられたをできるように、それらが最終的に短いテキストの個々 の行を保持します。 切り捨てられた単語や語句は、ユーザーが、部屋から解析するため困難です。
- **Focused 行の状態を検討してください。** ため、行のサイズ、さらに丸められますが、フォーカスがあるときのコーナー セルの外観をすべての状態でテストする必要があります。 画像やテキスト クリップになる可能性があります。 または不適切なフォーカス状態になります。
- **編集可能なテーブル控えめに使用**-より時間がかかり tvOS で iOS は、移動またはテーブルの行を削除します。 この機能を追加するまたは tvOS アプリから注意をそらすかどうかは慎重に決定する必要があります。
- **カスタムのセルの種類が適切な作成**- 組み込みのテーブル ビューのセルの種類は、多くの状況に適していますをより詳細に制御を提供し、読みやすくする情報を表示、非標準の情報をカスタム セル型の作成を検討してください。ユーザー。

<a name="Working-With-Table-Views" />

## <a name="working-with-table-views"></a>テーブル ビューの使用

Xamarin.tvOS アプリでのテーブル ビューを使用する最も簡単な方法では、作成およびインターフェイス デザイナーの外観を変更します。

開始するには、次の操作を行います。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
    
1. Visual studio for Mac では、新しい tvOS アプリ プロジェクトを開始し、選択**tvOS** > **アプリ** > **単一ビュー アプリ** をクリックし、 **次へ**ボタンをクリックします。 

    [![](table-views-images/table02.png "単一ビュー アプリを選択します。")](table-views-images/table02.png#lightbox)
1. 入力、**名前**のアプリをクリック**次**: 

    [![](table-views-images/table03.png "アプリの名前を入力します。")](table-views-images/table03.png#lightbox)
1. いずれかを調整、**プロジェクト名**と**ソリューション名**または既定値をそのまま使用しをクリックして、**作成**新しいソリューションを作成するボタン。 

    [![](table-views-images/table04.png "ソリューション名とプロジェクトの名前")](table-views-images/table04.png#lightbox)
1. **Solution Pad**、ダブルクリックして、 `Main.storyboard` iOS Designer で開くファイル。 

    [![](table-views-images/table05.png "Main.storyboard ファイル")](table-views-images/table05.png#lightbox)
1. 選択し、削除、**ビュー コント ローラーの既定の**: 

    [![](table-views-images/table06.png "選択し、既定のビュー コント ローラーの削除")](table-views-images/table06.png#lightbox)
1. 選択、**分割ビュー コント ローラー**から、**ツールボックス**デザイン サーフェイスにドラッグします。
1. 既定が表示されます、[分割ビュー](~/ios/tvos/user-interface/split-views.md)で、**ビュー コント ローラーのナビゲーション**と**テーブル ビュー コント ローラー**で左側にある、 **のビューコントローラー**の右側にあります。 これは、Apple の tvOS でのテーブル ビューの推奨される使用状況です。 

    [![](table-views-images/table08.png "分割ビューを追加します。")](table-views-images/table08.png#lightbox)
1. テーブル ビューのすべての部分を選択し、カスタムを割り当てる必要があります**クラス名**で、**ウィジェット**のタブ、**プロパティ エクスプ ローラー** C#コード。 たとえば、**テーブル ビュー コント ローラー**: 

    [![](table-views-images/table09.png "クラス名を割り当てる")](table-views-images/table09.png#lightbox)
1. カスタム クラスを作成することを確認、**テーブル ビュー コント ローラー**、**テーブル ビュー**任意と**プロトタイプ セル**します。 Visual Studio for Mac が作成されるときは、プロジェクト ツリーにカスタム クラスを追加します。 

    [![](table-views-images/table10.png "プロジェクト ツリー内のカスタム クラス")](table-views-images/table10.png#lightbox)
1. 次に、デザイン画面で、テーブル ビューを選択し、必要に応じて、そのプロパティを調整します。 数など**プロトタイプ セル**と**スタイル**(プレーンまたはグループ化)。 

    [![](table-views-images/table11.png "[ウィジェット] タブ")](table-views-images/table11.png#lightbox)
1. 各**プロトタイプ セル**選択し、一意の割り当て、**識別子**で、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**します。 この手順は_非常に重要な_この識別子を後で必要なデータを読み込む場合、テーブル。 たとえば`AttrCell`: 

    [![](table-views-images/table12.png "[ウィジェット] タブ")](table-views-images/table12.png#lightbox)
1. 1 つとして、セルを表示する選択することもできます、[テーブル ビュー セルの種類を既定の](#table-view-cell-types)を使用して、**スタイル**ドロップダウンかに設定して**カスタム**セルのレイアウトをデザイン画面を使用して、。内から他の UI ウィジェットをドラッグして、**ツールボックス**: 

    [![](table-views-images/table13.png "セルのレイアウト")](table-views-images/table13.png#lightbox)
1. 割り当てる一意**名前**プロトタイプのセルの設計では、各 UI 要素に、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**後でアクセスできるようにC#コード。 

    [![](table-views-images/table14.png "名前を割り当てる")](table-views-images/table14.png#lightbox)
1. すべてのテーブル ビュー内のプロトタイプのセルの前の手順を繰り返します。
1. 次に、カスタム クラスを UI デザイン、レイアウト、詳細ビューと割り当ての一意の残りの部分に割り当てる**名**の詳細には、各 UI 要素に表示でそれらをアクセスできるようにC#もします。 たとえば、次のように入力します。 

    [![](table-views-images/table15.png "UI のレイアウト")](table-views-images/table15.png#lightbox)
1. ストーリー ボードに変更を保存します。
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. Visual Studio で新しい tvOS アプリ プロジェクトを開始し、選択**tvOS** > **単一ビュー アプリ**アプリの名前を入力します。 をクリックして、**わかりました**新しいソリューションを作成するボタンをクリックします。 

    [![](table-views-images/table02-vs.png "単一ビュー アプリを選択します。")](table-views-images/table02-vs.png#lightbox)
1. **ソリューション エクスプ ローラー**、ダブルクリックして、 `Main.storyboard` iOS Designer で開くファイル。 

    [![](table-views-images/table05-vs.png "Main.storyboard ファイル")](table-views-images/table05-vs.png#lightbox)
1. 選択し、削除、**ビュー コント ローラーの既定の**: 

    [![](table-views-images/table06-vs.png "選択し、既定のビュー コント ローラーの削除")](table-views-images/table06-vs.png#lightbox)
1. 選択、**分割ビュー コント ローラー**から、**ツールボックス**デザイン サーフェイスにドラッグします。 

    [![](table-views-images/table07-vs.png "分割ビュー コント ローラー")](table-views-images/table07-vs.png#lightbox)
1. 既定が表示されます、[分割ビュー](~/ios/tvos/user-interface/split-views.md)で、**ビュー コント ローラーのナビゲーション**と**テーブル ビュー コント ローラー**で左側にある、 **のビューコントローラー**の右側にあります。 これは、Apple の tvOS でのテーブル ビューの推奨される使用状況です。 

    [![](table-views-images/table08-vs.png "UI のレイアウト")](table-views-images/table08-vs.png#lightbox)
1. テーブル ビューのすべての部分を選択し、カスタムを割り当てる必要があります**クラス名**で、**ウィジェット**のタブ、**プロパティ エクスプ ローラー** C#コード。 たとえば、**テーブル ビュー コント ローラー**: 

    [![](table-views-images/table09-vs.png "[ウィジェット] タブ")](table-views-images/table09-vs.png#lightbox)
1. カスタム クラスを作成することを確認、**テーブル ビュー コント ローラー**、**テーブル ビュー**任意と**プロトタイプ セル**します。 Visual Studio for Mac が作成されるときは、プロジェクト ツリーにカスタム クラスを追加します。 

    [![](table-views-images/table10-vs.png "プロジェクト ツリー内のカスタム クラス")](table-views-images/table10-vs.png#lightbox)
1. 次に、デザイン画面で、テーブル ビューを選択し、必要に応じて、そのプロパティを調整します。 数など**プロトタイプ セル**と**スタイル**(プレーンまたはグループ化)。 

    [![](table-views-images/table11-vs.png "[ウィジェット] タブ")](table-views-images/table11-vs.png#lightbox)
1. 各**プロトタイプ セル**選択し、一意の割り当て、**識別子**で、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**します。 この手順は_非常に重要な_この識別子を後で必要なデータを読み込む場合、テーブル。 たとえば`AttrCell`: 

    [![](table-views-images/table12-vs.png "識別子を割り当てる")](table-views-images/table12-vs.png#lightbox)
1. 1 つとして、セルを表示する選択することもできます、[テーブル ビュー セルの種類を既定の](#table-view-cell-types)を使用して、**スタイル**ドロップダウンかに設定して**カスタム**セルのレイアウトをデザイン画面を使用して、。内から他の UI ウィジェットをドラッグして、**ツールボックス**: 

    [![](table-views-images/table13-vs.png "スタイル ドロップダウン")](table-views-images/table13-vs.png#lightbox)
1. 割り当てる一意**名前**プロトタイプのセルの設計では、各 UI 要素に、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**後でアクセスできるようにC#コード。 

    [![](table-views-images/table14-vs.png "[ウィジェット] タブ")](table-views-images/table14-vs.png#lightbox)
1. すべてのテーブル ビュー内のプロトタイプのセルの前の手順を繰り返します。
1. 次に、カスタム クラスを UI デザイン、レイアウト、詳細ビューと割り当ての一意の残りの部分に割り当てる**名**の詳細には、各 UI 要素に表示でそれらをアクセスできるようにC#もします。 たとえば、次のように入力します。 

    [![](table-views-images/table15.png "UI のレイアウト")](table-views-images/table15.png#lightbox)
1. ストーリー ボードに変更を保存します。
    
-----

<a name="Designing-a-Data-Model" />

## <a name="designing-a-data-model"></a>データ モデルの設計

テーブル ビューを簡単に表示される情報を操作できるようにして容易になります (ユーザーが選択またはテーブル ビュー内の行を強調表示) と、詳細な情報の表示、カスタム クラスや情報については、データ モデルとして機能するクラスを作成する次のように表示されます.

旅行の予約アプリケーションの一覧を含むの例を見て**都市**、それぞれの一意のリストを含む**アトラクション**ユーザーが選択できます。 ユーザーは、引き寄せる力としてマークできる、*お気に入り*選択を取得すると、*方向*、引き寄せる力と*航空便の*に特定の都市。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

データ モデルを作成する、**引き寄せる力**でプロジェクト名を右クリックし、 **Solution Pad**選択**追加** > **新しいファイル.**.入力`AttractionInformation`の**名前** をクリックし、**新規**ボタン。 

[![](table-views-images/data01.png "名前の AttractionInformation を入力します。")](table-views-images/data01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

データ モデルを作成する、**引き寄せる力**でプロジェクト名を右クリックし、**ソリューション エクスプ ローラー**選択**追加** > **新しい項目...**.選択**クラス**入力と`AttractionInformation`の**名前** をクリックし、**追加**ボタン。 

[![](table-views-images/data01-vs.png "クラスを選択し、名前の AttractionInformation を入力します。")](table-views-images/data01-vs.png#lightbox)

-----

編集、`AttractionInformation.cs`ファイルを開き、次のようになります。

```csharp
using System;
using Foundation;

namespace tvTable
{
    public class AttractionInformation : NSObject
    {
        #region Computed Properties
        public CityInformation City { get; set;}
        public string Name { get; set;}
        public string Description { get; set;}
        public string ImageName { get; set;}
        public bool IsFavorite { get; set;}
        public bool AddDirections { get; set;}
        #endregion

        #region Constructors
        public AttractionInformation (string name, string description, string imageName)
        {
            // Initialize
            this.Name = name;
            this.Description = description;
            this.ImageName = imageName;
        }
        #endregion
    }
}
```

このクラスは、プロパティに関する情報を格納する、指定された**引き寄せる力**します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

次でプロジェクト名を右クリックし、 **Solution Pad**もう一度選択と**追加** > **新しいファイル.**.入力`CityInformation`の**名前** をクリックし、**新規**ボタン。 

[![](table-views-images/data02.png "名前の CityInformation を入力します。")](table-views-images/data02.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

プロジェクト名を次に、右クリックし、**ソリューション エクスプ ローラー**もう一度選択**追加** > **新しい項目.**.入力`CityInformation`の**名前** をクリックし、**追加**ボタン。 

[![](table-views-images/data02-vs.png "名前の CityInformation を入力します。")](table-views-images/data02-vs.png#lightbox)

-----

編集、`CityInformation.cs`ファイルを開き、次のようになります。

```csharp
using System;
using System.Collections.Generic;
using Foundation;

namespace tvTable
{
    public class CityInformation : NSObject
    {
        #region Computed Properties
        public string Name { get; set; }
        public List<AttractionInformation> Attractions { get; set;}
        public bool FlightBooked { get; set;}
        #endregion

        #region Constructors
        public CityInformation (string name)
        {
            // Initialize
            this.Name = name;
            this.Attractions = new List<AttractionInformation> ();
        }
        #endregion

        #region Public Methods
        public void AddAttraction (AttractionInformation attraction)
        {
            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }

        public void AddAttraction (string name, string description, string imageName)
        {
            // Create attraction
            var attraction = new AttractionInformation (name, description, imageName);

            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }
        #endregion
    }
}
```

このクラスの変換先に関するすべての情報が保持**市区町村**、一連の**アトラクション**その都市の 2 つのヘルパー メソッドを提供します (`AddAttraction`) にアトラクションを追加しやすく、市区町村。

<a name="The-Table-Data-Source" />

## <a name="the-table-view-data-source"></a>データ ソースには、テーブルの表示

各テーブルのビューには、データ ソースが必要です (`UITableViewDataSource`) テーブル ビューで必要なテーブルにデータを提供し、として必要な行を生成します。

上記の例でプロジェクト名を右クリックし、**ソリューション エクスプ ローラー**、**追加** > **新しいファイル.** 、呼び出す`AttractionTableDatasource` をクリックし、**新規**を作成するボタンをクリックします。 次に、編集、`AttractionTableDatasource.cs`ファイルを開き、次のようになります。

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDatasource : UITableViewDataSource
    {
        #region Constants
        const string CellID = "AttrCell";
        #endregion

        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        public List<CityInformation> Cities { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDatasource (AttractionTableViewController controller)
        {
            // Initialize
            this.Controller = controller;
            this.Cities = new List<CityInformation> ();
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities ()
        {
            // Clear existing
            Cities.Clear ();

            // Define cities and attractions
            var Paris = new CityInformation ("Paris");
            Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
            Paris.AddAttraction ("Musée du Louvre", "is one of the world's largest museums and a historic monument in Paris, France.", "Louvre");
            Paris.AddAttraction ("Moulin Rouge", "French for 'Red Mill', is a cabaret in Paris, France.", "MoulinRouge");
            Paris.AddAttraction ("La Seine", "Is a 777-kilometre long river and an important commercial waterway within the Paris Basin.", "RiverSeine");
            Cities.Add (Paris);

            var SanFran = new CityInformation ("San Francisco");
            SanFran.AddAttraction ("Alcatraz Island", "Is located in the San Francisco Bay, 1.25 miles (2.01 km) offshore from San Francisco.", "Alcatraz");
            SanFran.AddAttraction ("Golden Gate Bridge", "Is a suspension bridge spanning the Golden Gate strait between San Francisco Bay and the Pacific Ocean", "GoldenGateBridge");
            SanFran.AddAttraction ("San Francisco", "Is the cultural, commercial, and financial center of Northern California.", "SanFrancisco");
            SanFran.AddAttraction ("Telegraph Hill", "Is primarily a residential area, much quieter than adjoining North Beach.", "TelegraphHill");
            Cities.Add (SanFran);

            var Houston = new CityInformation ("Houston");
            Houston.AddAttraction ("City Hall", "It was constructed in 1938-1939, and is located in Downtown Houston.", "CityHall");
            Houston.AddAttraction ("Houston", "Is the most populous city in Texas and the fourth most populous city in the US.", "Houston");
            Houston.AddAttraction ("Texas Longhorn", "Is a breed of cattle known for its characteristic horns, which can extend to over 6 ft.", "LonghornCattle");
            Houston.AddAttraction ("Saturn V Rocket", "was an American human-rated expendable rocket used by NASA between 1966 and 1973.", "Rocket");
            Cities.Add (Houston);
        }
        #endregion

        #region Override Methods
        public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Get cell
            var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

            // Populate cell
            cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

            // Return new cell
            return cell;
        }

        public override nint NumberOfSections (UITableView tableView)
        {
            // Return number of cities
            return Cities.Count;
        }

        public override nint RowsInSection (UITableView tableView, nint section)
        {
            // Return the number of attractions in the given city
            return Cities [(int)section].Attractions.Count;
        }

        public override string TitleForHeader (UITableView tableView, nint section)
        {
            // Get the name of the current city
            return Cities [(int)section].Name;
        }
        #endregion
    }
}
```

詳しくは、クラスのいくつかのセクションを参照してくださいを見てみましょう。

まず、(これは、上記のインターフェイス デザイナーで割り当てられている同じ識別子)、プロトタイプのセルの一意の識別子を保持するために定数を定義がテーブル ビュー コント ローラーに追加し、ショートカット、データのストレージを作成し、。

```csharp
const string CellID = "AttrCell";
public AttractionTableViewController Controller { get; set;}
public List<CityInformation> Cities { get; set;}
```

テーブル ビュー コント ローラーで、保存にビルドを (上記で定義されたデータ モデルを使用) については、データ ソースを設定します。 次に、クラスが作成されたとき。

```csharp
public AttractionTableDatasource (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
    this.Cities = new List<CityInformation> ();
    PopulateCities ();
}
```

この例では、`PopulateCities`メソッドがこれらは実際のアプリでのデータベースまたは web サービスから簡単に読み取るでしたが、メモリ内データ モデル オブジェクトが単に作成します。

```csharp
public void PopulateCities ()
{
    // Clear existing
    Cities.Clear ();

    // Define cities and attractions
    var Paris = new CityInformation ("Paris");
    Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
    ...
}
```

`NumberOfSections`メソッドは、テーブル内のセクションの数を返します。

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    // Return number of cities
    return Cities.Count;
}
```

**プレーンな**テーブル ビューのスタイル、常に 1 を返します。

`RowsInSection`メソッドは、現在のセクションの行の数を返します。

```csharp
public override nint RowsInSection (UITableView tableView, nint section)
{
    // Return the number of attractions in the given city
    return Cities [(int)section].Attractions.Count;
}
```

用にもう一度、**プレーンな**テーブル ビューでは、データ ソースの項目の合計数を返します。

`TitleForHeader`メソッドは指定されたタイトルを返しますセクション。

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    // Get the name of the current city
    return Cities [(int)section].Name;
}
```

**プレーンな**テーブル ビューのタイトルを空白のままに、入力 (`""`)。

最後に、テーブル ビューで要求されると、作成し、設定を使用して、プロトタイプのセル、`GetCell`メソッド。 

```csharp
public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Get cell
    var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

    // Populate cell
    cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

    // Return new cell
    return cell;
}
```

操作の詳細については、 `UITableViewDatasource`、Apple を参照してください[UITableViewDatasource](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40006941)ドキュメント。

<a name="The-Table-View-Delegate" />

## <a name="the-table-view-delegate"></a>テーブル ビューのデリゲート

各テーブルのビューには、デリゲートが必要です (`UITableViewDelegate`) ユーザーの操作や、テーブルの他のシステム イベントに応答します。

上記の例でプロジェクト名を右クリックし、**ソリューション エクスプ ローラー**、**追加** > **新しいファイル.** 、呼び出す`AttractionTableDelegate` をクリックし、**新規**を作成するボタンをクリックします。 次に、編集、`AttractionTableDelegate.cs`ファイルを開き、次のようになります。

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDelegate : UITableViewDelegate
    {
        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDelegate (AttractionTableViewController controller)
        {
            // Initializw
            this.Controller = controller;
        }
        #endregion

        #region Override Methods
        public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
            attraction.IsFavorite = (!attraction.IsFavorite);

            // Update UI
            Controller.TableView.ReloadData ();
        }

        public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Inform caller of highlight change
            RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
            return true;
        }
        #endregion

        #region Events
        public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
        public event AttractionHighlightedDelegate AttractionHighlighted;

        internal void RaiseAttractionHighlighted (AttractionInformation attraction)
        {
            // Inform caller
            if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
        }
        #endregion
    }
}
```

詳細には、このクラスのいくつかのセクションを参照してくださいを見てみましょう。

最初に、テーブル ビュー コント ローラーへのショートカットは、クラスが作成されたときに作成します。

```csharp
public AttractionTableViewController Controller { get; set;}
...

public AttractionTableDelegate (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
}
```

行を選択し、(Apple リモートのタッチ画面で、ユーザーがクリックする) をマークする、**引き寄せる力**をお気に入りとして選択した行で表されます。

```csharp
public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
    attraction.IsFavorite = (!attraction.IsFavorite);

    // Update UI
    Controller.TableView.ReloadData ();
}
```

次に、する場合、ユーザーは、(Apple リモート タッチ画面を使用してフォーカスを付けること) により、行を強調表示することの詳細を表示、**引き寄せる力**分割ビュー コント ローラーの詳細 セクションでは、その行によって表されます。

```csharp
public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Inform caller of highlight change
    RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
    return true;
}
...

public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
public event AttractionHighlightedDelegate AttractionHighlighted;

internal void RaiseAttractionHighlighted (AttractionInformation attraction)
{
    // Inform caller
    if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
}
```

`CanFocusRow`テーブル ビューにフォーカスが移動される行ごとにメソッドが呼び出されます。 返す`true`それ以外の場合を返す場合は、行は、フォーカスを取得できます、`false`します。 この例では、場合は、カスタムを作成した`AttractionHighlighted`フォーカスを受け取るように、行ごとに発生するイベントです。

操作の詳細については、 `UITableViewDelegate`、Apple を参照してください[UITableViewDelegate](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40006942)ドキュメント。

<a name="The-Table-View-Cell" />

## <a name="the-table-view-cell"></a>テーブル ビューのセル

テーブル ビューのセルのカスタム インスタンスの作成もインターフェイス デザイナーでテーブルのビューに追加した各プロトタイプ セル (`UITableViewCell`) に作成されると、新しいセル (行) を設定することを許可します。

例のアプリをダブルクリックして、`AttractionTableCell.cs`ファイルを開き、編集し、次のようになります。

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableCell : UITableViewCell
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public AttractionTableCell (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Trap all errors
            try {
                Title.Text = Attraction.Name;
                Favorite.Hidden = (!Attraction.IsFavorite);
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion
    }
}
```

このクラスは、ストレージを引き寄せる力データ モデル オブジェクトを提供します (`AttractionInformation`の上に定義されている) 特定の行に表示されます。

```csharp
private AttractionInformation _attraction = null;
...

public AttractionInformation Attraction {
    get { return _attraction; }
    set {
        _attraction = value;
        UpdateUI ();
    }
}
```

`UpdateUI`メソッドは必要に応じて UI ウィジェット (つまり、インターフェイス デザイナー内のセルのプロトタイプに追加された) を設定します。

```csharp
private void UpdateUI ()
{
    // Trap all errors
    try {
        Title.Text = Attraction.Name;
        Favorite.Hidden = (!Attraction.IsFavorite);
    } catch {
        // Since the UI might not be fully loaded, ignore
        // all errors at this point
    }
}
```

操作の詳細については、 `UITableViewCell`、Apple を参照してください[UITableViewCell](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewCell_Class/index.html#//apple_ref/doc/uid/TP40006938)ドキュメント。

<a name="The-Table-View-Controller" />

## <a name="the-table-view-controller"></a>テーブル ビュー コント ローラー

テーブル ビュー コント ローラー (`UITableViewController`) インターフェイス デザイナーを使用してストーリー ボードに追加されているテーブル ビューを管理します。

例のアプリをダブルクリックして、`AttractionTableViewController.cs`ファイルを開き、編集し、次のようになります。

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableViewController : UITableViewController
    {
        #region Computed Properties
        public AttractionTableDatasource Datasource {
            get { return TableView.DataSource as AttractionTableDatasource; }
        }

        public AttractionTableDelegate TableDelegate {
            get { return TableView.Delegate as AttractionTableDelegate; }
        }
        #endregion

        #region Constructors
        public AttractionTableViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Setup table
            TableView.DataSource = new AttractionTableDatasource (this);
            TableView.Delegate = new AttractionTableDelegate (this);
            TableView.ReloadData ();
        }
        #endregion
    }
}
```

このクラスについて詳しく見てをみましょう。 テーブル ビューのアクセスを容易にするショートカットを作成した最初に、`DataSource`と`TableDelegate`します。 使用します、後で分割ビューの左側にあるテーブルのビューと右側の詳細ビューの間で通信します。

最後に、テーブル ビューがメモリに読み込まれると、私たちのインスタンスを作成、`AttractionTableDatasource`と`AttractionTableDelegate`(上記で作成した両方) およびテーブル ビューにアタッチします。

操作の詳細については、 `UITableViewController`、Apple を参照してください[UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)ドキュメント。

<a name="Pulling-it-All-Together" />

## <a name="pulling-it-all-together"></a>すべてをまとめてプル

テーブルのビューが通常の一方の側で表示されますこのドキュメントの先頭に述べたように、[分割ビュー](~/ios/tvos/user-interface/split-views.md)反対側に表示される選択した項目の詳細のナビゲーションとして。 例えば: 

[![](table-views-images/intro01.png "サンプル アプリの実行")](table-views-images/intro01.png#lightbox)

これが tvOS で標準的なパターンであるため、すべてをまとめて表示する最後の手順を見てみましょうと分割ビューの左と右の辺は相互作用があります。

<a name="The-Detail-View" />

### <a name="the-detail-view"></a>詳細の表示

旅行アプリの例については、カスタム クラス上で示した (`AttractionViewController`) が定義されている標準のビュー コント ローラーが、詳細ビューと分割ビューの右側に表示されます。

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionViewController : UIViewController
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }

        public MasterSplitView SplitView { get; set;}
        #endregion

        #region Constructors
        public AttractionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void UpdateUI ()
        {
            // Trap all errors
            try {
                City.Text = Attraction.City.Name;
                Title.Text = Attraction.Name;
                SubTitle.Text = Attraction.Description;

                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
                IsFavorite.Hidden = (!Attraction.IsFavorite);
                IsDirections.Hidden = (!Attraction.AddDirections);
                BackgroundImage.Image = UIImage.FromBundle (Attraction.ImageName);
                AttractionImage.Image = BackgroundImage.Image;
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Ensure the UI Updates
            UpdateUI ();
        }
        #endregion

        #region Actions
        partial void BookFlight (NSObject sender)
        {
            // Ask user to book flight
            AlertViewController.PresentOKCancelAlert ("Book Flight",
                                                      string.Format ("Would you like to book a flight to {0}?", Attraction.City.Name),
                                                      this,
                                                      (ok) => {
                Attraction.City.FlightBooked = ok;
                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
            });
        }

        partial void GetDirections (NSObject sender)
        {
            // Ask user to add directions
            AlertViewController.PresentOKCancelAlert ("Add Directions",
                                                     string.Format ("Would you like to add directions to {0} to you itinerary?", Attraction.Name),
                                                     this,
                                                     (ok) => {
                                                         Attraction.AddDirections = ok;
                                                         IsDirections.Hidden = (!Attraction.AddDirections);
                                                     });
        }

        partial void MarkFavorite (NSObject sender)
        {
            // Flip favorite state
            Attraction.IsFavorite = (!Attraction.IsFavorite);
            IsFavorite.Hidden = (!Attraction.IsFavorite);

            // Reload table
            SplitView.Master.TableController.TableView.ReloadData ();
        }
        #endregion
    }
}
```

ここでは、用意されて、**引き寄せる力**(`AttractionInformation`) をプロパティとして表示され、作成されている、 `UpdateUI` UI ウィジェットを設定するメソッドがインターフェイス デザイナー、ビューに追加します。

分割ビュー コント ローラーのショートカットも定義しました (`SplitView`) テーブルのビューに変更しましたが通信するために使用する (`AcctractionTableView`)。

最後に、カスタム アクション (イベント) は、3 つに追加された`UIButton`、ユーザー マークとして、引き寄せる力を使用できるインターフェイス デザイナーで作成されたインスタンスを_お気に入り_、取得_方向_に、引き寄せる力と_航空便の_に特定の都市。

<a name="The-Navigation-View-Controller" />

### <a name="the-navigation-view-controller"></a>ナビゲーション ビュー コント ローラー

ナビゲーション ビュー コント ローラーのカスタム クラスが割り当てられたテーブル ビュー コント ローラーが分割ビューの左側のナビゲーションのビュー コント ローラーで入れ子になっているため、(`MasterNavigationController`) インターフェイス デザイナーで次のように定義されているとします。

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterNavigationController : UINavigationController
    {
        #region Computed Properties
        public MasterSplitView SplitView { get; set;}
        public AttractionTableViewController TableController {
            get { return TopViewController as AttractionTableViewController; }
        }
        #endregion

        #region Constructors
        public MasterNavigationController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

ここでも、このクラスには、分割ビュー コント ローラーの 2 つの辺の間で通信を容易にできるようにいくつかのショートカットだけを定義します。

* `SplitView` -分割ビュー コント ローラーへのリンクは、(`MainSpiltViewController`) ナビゲーション ビュー コント ローラーが属しています。
* `TableController` -テーブル ビュー コント ローラーを取得します (`AttractionTableViewController`) ナビゲーション ビュー コント ローラーの上部のビューとして表示されます。

<a name="The-Split-View-Controller" />

### <a name="the-split-view-controller"></a>分割ビュー コント ローラー

カスタム クラスを作成した分割ビュー コント ローラーは、アプリケーションのベースであるため (`MasterSplitViewController`) インターフェイス デザイナーで次のように定義されているとします。

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterSplitView : UISplitViewController
    {
        #region Computed Properties
        public AttractionViewController Details {
            get { return ViewControllers [1] as AttractionViewController; }
        }

        public MasterNavigationController Master {
            get { return ViewControllers [0] as MasterNavigationController; }
        }
        #endregion

        #region Constructors
        public MasterSplitView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            Master.SplitView = this;
            Details.SplitView = this;

            // Wire-up events
            Master.TableController.TableDelegate.AttractionHighlighted += (attraction) => {
                // Display new attraction
                Details.Attraction = attraction;
            };
        }
        #endregion
    }
}
```

まずへのショートカットを作成します、**詳細**分割ビューの側 (`AttractionViewController`) と、**マスター**側 (`MasterNavigationController`)。 ここでも、これによって、2 つの辺を後で間の通信に簡単に。

次に、分割ビューがメモリに読み込まれると、分割ビュー コント ローラーを分割ビューの両方の側にアタッチし、テーブル ビューで、引き寄せる力を強調表示したユーザーに応答 (`AttractionHighlighted`) で新しい引き寄せる力を表示することによって、**の詳細**分割ビューの左右します。

参照してください、 [tvTables](https://developer.xamarin.com/samples/monotouch/tvos/tvTable/)分割ビュー内のテーブル ビューの完全な実装用のサンプル アプリケーション。

## <a name="table-views-in-detail"></a>テーブル ビューの詳細

TvOS は iOS に基づいて、ためにように設計されたテーブルのビューおよびテーブル ビュー コント ローラーと同様の方法で動作します。 詳細については、Xamarin アプリでのテーブル ビューの操作についてを参照してください、iOS[テーブルとセル操作](~/ios/user-interface/controls/tables/index.md)ドキュメント。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、設計と Xamarin.tvOS アプリ内でテーブルのビューの操作について説明しました。 TvOS アプリでのテーブル ビューの一般的な使用方法は、分割ビュー内のテーブル ビューを使用した作業の例を提示したとします。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 用のアプリのプログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
