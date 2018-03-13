---
title: "テーブル ビューを使用します。"
description: "この記事では、設計と Xamarin.tvOS アプリ内でテーブルのビューおよびテーブル ビューのコント ローラーの操作について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: D8F80FA9-6400-4DB7-AFC9-A28A54AD04E8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: af562ac03f2cd5f293f99c7509000499ad5deaa4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-table-views"></a>テーブル ビューを使用します。

_この記事では、設計と Xamarin.tvOS アプリ内でテーブルのビューおよびテーブル ビューのコント ローラーの操作について説明します。_

TvOS でのテーブル ビューを必要に応じてグループまたはセクションに整理する行のスクロールの 1 つの列として表示されます。 方法を理解するクリア テキストで、ユーザーに効率的に大量のデータを表示する必要がある場合は、テーブルのビューを使用してください。

テーブルのビューは、通常の一方の側で表示されます、[分割ビュー](~/ios/tvos/user-interface/split-views.md)によるナビゲーション、反対側に表示される選択した項目の詳細。

[![](table-views-images/intro01.png "サンプル テーブルの表示")](table-views-images/intro01.png#lightbox)

<a name="About-Table-Views" />

## <a name="about-table-views"></a>テーブルのビューについて

A`UITableView`を必要に応じてグループまたはセクションに整理する情報の階層リストとしてスクロール可能な行の 1 つの列が表示されます。 

[![](table-views-images/table01.png "選択された項目")](table-views-images/table01.png#lightbox)

Apple では、テーブルの操作の次の方法があります。

- **幅の対応する**-テーブルの幅に適切なバランスを取るしようとしています。 テーブルが広すぎる場合は、距離をスキャンするが困難にすることができ、使用可能なコンテンツ エリアから離れた場所がかかることができます。 テーブルが狭すぎる場合は、切り捨てられるように情報を引き起こす可能性がまたはラップ、もう一度このできます部屋の反対側から読みにくくなります。
- **内容を簡単にテーブルを表示する**- データの大規模なリストの遅延読み込みコンテンツと、テーブルは、ユーザーに提示するとすぐに情報を表示を開始します。 テーブルに読み込みに長い場合は、ユーザーは、アプリや待ち時間をロックされていることに関心を失う可能性があります。
- **ユーザーの長いコンテンツ負荷を通知**: 長いテーブルの読み込み時間は避け、現在の場合、[プログレス バーまたはアクティビティのインジケーター](~/ios/tvos/user-interface/progress-indicators.md)ロックしていないアプリを把握できるようにします。

<a name="Table-Cell-Types" />

## <a name="table-view-cell-types"></a>テーブル ビューのセルの種類

A`UITableViewCell`テーブル ビューでデータの個々 の行を表すために使用します。 Apple 既定テーブル セルのいくつかの型が定義されます。

- **既定の**- オプション 画像の右上のタイトルを左揃え、セルの左側にあるこの型を紹介します。 
- **字幕**- 最初の行をより小さなで左揃えのタイトルを左揃え字幕次の行にこの型を紹介します。
- **値 1** -この型は、同じ行に左揃え、右揃えの軽量色付きの字幕のタイトルを表示します。
- **値 2** -この型が同じ行に、右揃えのタイトルが、左揃えの軽量色付きの字幕を表示します。

既定のテーブル ビューのセルの種類のすべても漏えいインジケーターまたはチェック マークなどのグラフィック要素をサポートします。 

さらに、定義することができます、**カスタム**テーブル ビューのセルの種類および現在のところ、_プロトタイプ セル_、いずれかを作成するインターフェイス デザイナーまたはコードを使用します。

Apple では、テーブル セルの表示を操作するための以下の推奨事項があります。

- **テキストのクリッピングを回避**の切り捨て終了しないように短いテキストの個々 の行を保持します。 切り捨てられた単語や語句が困難なユーザーが部屋の反対側から解析するためです。
- **Focused 行の状態を考慮**行が丸められますより多くの大きいになるため - フォーカスがあるとき、コーナー セルの外観をすべての状態でテストする必要があります。 イメージやテキストは、クリップになる可能性があります。 または Focused 状態の正しくない検索します。
- **編集可能なテーブルなので、慎重に使用して**-移動または削除するテーブルの行より時間がかかり tvOS で iOS です。 この機能は、追加または tvOS アプリがあいまいになるかどうかは慎重に決定する必要があります。
- **カスタムのセルの種類が適切な作成**組み込みテーブル ビューのセルの種類は、多くの状況に適して - より詳細に制御を提供し、読みやすくする情報を表示する標準についてのカスタム セルの型の作成を検討してくださいユーザー。

<a name="Working-With-Table-Views" />

## <a name="working-with-table-views"></a>テーブル ビューを使用します。

Xamarin.tvOS アプリでは、テーブルのビューを使用する最も簡単な方法を作成し、インターフェイスのデザイナーでその外観を変更します。

開始するには、次の操作を行います。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
1. Mac 用 Visual Studio で新しい tvOS アプリ プロジェクトを開始し、選択**tvOS** > **アプリ** > **1 つのアプリの表示** をクリックし、 **次へ**ボタンをクリックします。 

    [![](table-views-images/table02.png "アプリの 1 つのビューを選択します")](table-views-images/table02.png#lightbox)
1. 入力、**名前**をクリックして、アプリの**次**: 

    [![](table-views-images/table03.png "アプリの名前を入力します。")](table-views-images/table03.png#lightbox)
1. 調整するか、**プロジェクト名**と**ソリューション名**または既定値を受け入れるし、をクリックして、**作成**新しいソリューションを作成するにはボタン。 

    [![](table-views-images/table04.png "プロジェクト名およびソリューション名")](table-views-images/table04.png#lightbox)
1. **ソリューション パッド**をダブルクリックして、 `Main.storyboard` iOS デザイナーで開くファイル。 

    [![](table-views-images/table05.png "Main.storyboard ファイル")](table-views-images/table05.png#lightbox)
1. 選択し、削除、**ビュー コント ローラーの既定の**: 

    [![](table-views-images/table06.png "選択してビューの既定のコント ローラーの削除")](table-views-images/table06.png#lightbox)
1. 選択、**分割ビュー コント ローラー**から、**ツールボックス**し、デザイン画面にドラッグします。
1. 既定が表示されます、[分割ビュー](~/ios/tvos/user-interface/split-views.md)で、**ビュー コント ローラーのナビゲーション**と**テーブル ビューのコント ローラー**左側で、**ビュー コント ローラー**右側にあるのです。 これは、Apple の推奨される使用量を tvOS 内のテーブル ビューです。 

    [![](table-views-images/table08.png "分割ビューを追加します。")](table-views-images/table08.png#lightbox)
1. テーブル ビューのすべての部分を選択し、カスタムを割り当てる必要があります**クラス名**で、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**それを c# で後でアクセスできるようにします。コードです。 たとえば、**テーブル ビューのコント ローラー**: 

    [![](table-views-images/table09.png "クラス名を割り当てます")](table-views-images/table09.png#lightbox)
1. カスタム クラスを作成することを確認してください、**テーブル ビューのコント ローラー**、**テーブル ビュー**と任意**プロトタイプ セル**です。 Visual Studio for Mac が作成するときは、プロジェクト ツリーをカスタム クラスを追加します。 

    [![](table-views-images/table10.png "プロジェクト ツリー内のカスタム クラス")](table-views-images/table10.png#lightbox)
1. 次に、デザイン画面で、テーブル ビューを選択し、必要に応じて、そのプロパティを調整します。 数など**プロトタイプ セル**と**スタイル**(プレーンまたはグループ化)。 

    [![](table-views-images/table11.png "ウィジェット タブ")](table-views-images/table11.png#lightbox)
1. 各**プロトタイプ セル**して選択し、一意の割り当て**識別子**で、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**です。 この手順は_非常に重要な_後でこの識別子が必要とするデータを読み込む場合、テーブルです。 たとえば`AttrCell`: 

    [![](table-views-images/table12.png "ウィジェット タブ")](table-views-images/table12.png#lightbox)
1. 1 つとして、セルの表示を選択することも、[テーブル ビューのセルの種類の既定の](#Table-View-Cell-Types)経由で、**スタイル**ドロップダウンまたはに設定する**カスタム**セルのレイアウトをデザイン画面を使用して内から他の UI ウィジェットをドラッグして、**ツールボックス**: 

    [![](table-views-images/table13.png "セルのレイアウト")](table-views-images/table13.png#lightbox)
1. 割り当てる一意な**名前**でプロトタイプ セル設計では、各 UI 要素に、**ウィジェット**のタブ、**プロパティ エクスプ ローラー** c# コードで後でアクセスできるように。 

    [![](table-views-images/table14.png "名前を割り当てる")](table-views-images/table14.png#lightbox)
1. すべてのテーブル ビューでのプロトタイプのセルを上記の手順を繰り返します。
1. 次に、カスタム クラスを UI 設計、詳細ビューのレイアウトおよび割り当ての一意の残りの部分に割り当てる**名**各 UI 要素の詳細に表示 c# で同様にアクセスできるようにします。 たとえば、次のように入力します。 

    [![](table-views-images/table15.png "UI のレイアウト")](table-views-images/table15.png#lightbox)
1. ストーリー ボードに変更を保存します。
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. Visual Studio で、新しい tvOS アプリ プロジェクトを開始し、選択**tvOS** > **1 つのアプリの表示**し、アプリの名前を入力します。 をクリックして、**わかりました**クリックすると、新しいソリューションを作成します。 

    [![](table-views-images/table02-vs.png "アプリの 1 つのビューを選択します")](table-views-images/table02-vs.png#lightbox)
1. **ソリューション エクスプ ローラー**をダブルクリックして、 `Main.storyboard` iOS デザイナーで開くファイル。 

    [![](table-views-images/table05-vs.png "Main.storyboard ファイル")](table-views-images/table05-vs.png#lightbox)
1. 選択し、削除、**ビュー コント ローラーの既定の**: 

    [![](table-views-images/table06-vs.png "選択してビューの既定のコント ローラーの削除")](table-views-images/table06-vs.png#lightbox)
1. 選択、**分割ビュー コント ローラー**から、**ツールボックス**し、デザイン画面にドラッグします。 

    [![](table-views-images/table07-vs.png "分割ビュー コント ローラー")](table-views-images/table07-vs.png#lightbox)
1. 既定が表示されます、[分割ビュー](~/ios/tvos/user-interface/split-views.md)で、**ビュー コント ローラーのナビゲーション**と**テーブル ビューのコント ローラー**左側で、**ビュー コント ローラー**右側にあるのです。 これは、Apple の推奨される使用量を tvOS 内のテーブル ビューです。 

    [![](table-views-images/table08-vs.png "UI のレイアウト")](table-views-images/table08-vs.png#lightbox)
1. テーブル ビューのすべての部分を選択し、カスタムを割り当てる必要があります**クラス名**で、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**それを c# で後でアクセスできるようにします。コードです。 たとえば、**テーブル ビューのコント ローラー**: 

    [![](table-views-images/table09-vs.png "ウィジェット タブ")](table-views-images/table09-vs.png#lightbox)
1. カスタム クラスを作成することを確認してください、**テーブル ビューのコント ローラー**、**テーブル ビュー**と任意**プロトタイプ セル**です。 Visual Studio for Mac が作成するときは、プロジェクト ツリーをカスタム クラスを追加します。 

    [![](table-views-images/table10-vs.png "プロジェクト ツリー内のカスタム クラス")](table-views-images/table10-vs.png#lightbox)
1. 次に、デザイン画面で、テーブル ビューを選択し、必要に応じて、そのプロパティを調整します。 数など**プロトタイプ セル**と**スタイル**(プレーンまたはグループ化)。 

    [![](table-views-images/table11-vs.png "ウィジェット タブ")](table-views-images/table11-vs.png#lightbox)
1. 各**プロトタイプ セル**して選択し、一意の割り当て**識別子**で、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**です。 この手順は_非常に重要な_後でこの識別子が必要とするデータを読み込む場合、テーブルです。 たとえば`AttrCell`: 

    [![](table-views-images/table12-vs.png "識別子を割り当てる")](table-views-images/table12-vs.png#lightbox)
1. 1 つとして、セルの表示を選択することも、[テーブル ビューのセルの種類の既定の](#Table-View-Cell-Types)経由で、**スタイル**ドロップダウンまたはに設定する**カスタム**セルのレイアウトをデザイン画面を使用して内から他の UI ウィジェットをドラッグして、**ツールボックス**: 

    [![](table-views-images/table13-vs.png "スタイルのドロップダウン リスト")](table-views-images/table13-vs.png#lightbox)
1. 割り当てる一意な**名前**でプロトタイプ セル設計では、各 UI 要素に、**ウィジェット**のタブ、**プロパティ エクスプ ローラー** c# コードで後でアクセスできるように。 

    [![](table-views-images/table14-vs.png "ウィジェット タブ")](table-views-images/table14-vs.png#lightbox)
1. すべてのテーブル ビューでのプロトタイプのセルを上記の手順を繰り返します。
1. 次に、カスタム クラスを UI 設計、詳細ビューのレイアウトおよび割り当ての一意の残りの部分に割り当てる**名**各 UI 要素の詳細に表示 c# で同様にアクセスできるようにします。 たとえば、次のように入力します。 

    [![](table-views-images/table15.png "UI のレイアウト")](table-views-images/table15.png#lightbox)
1. ストーリー ボードに変更を保存します。
    
-----

<a name="Designing-a-Data-Model" />

## <a name="designing-a-data-model"></a>データ モデルの設計

テーブル ビューを簡単に表示される情報に扱うことおよび (と、ユーザーを選択したり、テーブル ビューに行を強調表示)、詳細な情報の表示が容易になります、カスタムのクラスまたはについては、データ モデルとして機能するクラスを作成する次のように表示されます.

たとえば旅行の予約アプリケーションの一覧を含む**都市**の一意の一覧を含む各**観光**ユーザーが選択できます。 ユーザーが、引力としてのマークを付けることが、*お気に入り*取得を選択して*方向*、引力と*、フライトの予約*特定の都市にします。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

データ モデルを作成する、**引力**でプロジェクト名を右クリックし、**ソリューション パッド**選択**追加** > **新しいファイル.**.入力`AttractionInformation`の**名前** をクリックし、**新規**ボタン。 

[![](table-views-images/data01.png "AttractionInformation を名を入力します。")](table-views-images/data01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

データ モデルを作成する、**引力**でプロジェクト名を右クリックし、**ソリューション エクスプ ローラー**選択**追加** > **新しい項目の追加...**.選択**クラス**入力と`AttractionInformation`の**名前** をクリックし、**追加**ボタン。 

[![](table-views-images/data01-vs.png "クラスを選択し、名前の AttractionInformation を入力")](table-views-images/data01-vs.png#lightbox)

-----

編集、`AttractionInformation.cs`ファイルし、次のようになります。

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

このクラスは、プロパティに関する情報を格納する、指定された**引力**です。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

プロジェクト名を次に、右クリックし、**ソリューション パッド**もう一度選択**追加** > **新しいファイル.**.入力`CityInformation`の**名前** をクリックし、**新規**ボタン。 

[![](table-views-images/data02.png "CityInformation を名を入力します。")](table-views-images/data02.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

プロジェクト名を次に、右クリックし、**ソリューション エクスプ ローラー**もう一度選択**追加** > **新しい項目の追加.**.入力`CityInformation`の**名前** をクリックし、**追加**ボタン。 

[![](table-views-images/data02-vs.png "CityInformation を名を入力します。")](table-views-images/data02-vs.png#lightbox)

-----

編集、`CityInformation.cs`ファイルし、次のようになります。

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

このクラスでは、すべての情報を保持の保存先は**市区町村**のコレクション**観光**その市の 2 つのヘルパー メソッドを提供し、(`AddAttraction`) に観光の追加を容易にできるように、市区町村。

<a name="The-Table-Data-Source" />

## <a name="the-table-view-data-source"></a>詳細については、テーブルの データ ソースの表示

各テーブルのビューには、データ ソースが必要です (`UITableViewDataSource`) テーブル ビューで必要なテーブルにデータを提供し、として必要な行を生成します。

上記の例でプロジェクト名を右クリックし、**ソリューション エクスプ ローラー**[**追加** > **新しいファイル.**および呼び出し`AttractionTableDatasource`] をクリックし、**新規**を作成するボタンをクリックします。 次に、編集、`AttractionTableDatasource.cs`ファイルし、次のようになります。

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

詳しくは、クラスのセクションでは、いくつか見てを見てみましょう。

最初に、(これは、インターフェイスのデザイナー上で割り当てられている同じ識別子) プロトタイプ セルの一意の識別子を保持するために定数を定義、テーブル ビュー コント ローラーに追加し、ショートカット、および記憶域をデータの作成がお。

```csharp
const string CellID = "AttrCell";
public AttractionTableViewController Controller { get; set;}
public List<CityInformation> Cities { get; set;}
```

テーブルのビュー コント ローラーを保存し、ビルドおよび (上記で定義されたデータ モデルの使用)、データ ソースへの追加お次に、クラスが作成されたとき。

```csharp
public AttractionTableDatasource (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
    this.Cities = new List<CityInformation> ();
    PopulateCities ();
}
```

この例では、`PopulateCities`メソッドだけでデータ モデル オブジェクトをメモリに作成ただし、これらは実際のアプリでのデータベースまたは web サービスから簡単に読み取る。

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

`NumberOfSections`メソッドが、テーブル内のセクションの数を返します。

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    // Return number of cities
    return Cities.Count;
}
```

**Plain**テーブルのビューのスタイル、常に 1 を返します。

`RowsInSection`メソッドは、現在のセクションの行の数を返します。

```csharp
public override nint RowsInSection (UITableView tableView, nint section)
{
    // Return the number of attractions in the given city
    return Cities [(int)section].Attractions.Count;
}
```

用にもう一度、 **Plain**テーブルのビューは、データ ソースの項目の合計数を返します。

`TitleForHeader`メソッドでは、指定されたタイトルが返されますセクション。

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    // Get the name of the current city
    return Cities [(int)section].Name;
}
```

**Plain**テーブル ビューを入力、タイトルを空白のままに (`""`)。

最後に、要求された場合、テーブル ビューで、作成しを使用してセルのプロトタイプへの追加、`GetCell`メソッド。 

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

各テーブルのビューには、デリゲートが必要です (`UITableViewDelegate`) ユーザーの操作や、テーブルでは、他のシステム イベントに応答します。

上記の例でプロジェクト名を右クリックし、**ソリューション エクスプ ローラー**[**追加** > **新しいファイル.**および呼び出し`AttractionTableDelegate`] をクリックし、**新規**を作成するボタンをクリックします。 次に、編集、`AttractionTableDelegate.cs`ファイルし、次のようになります。

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

詳細に見て、このクラスのいくつかのセクションを見てみましょう。

最初に、テーブル ビュー コント ローラーへのショートカットを作成お、クラスが作成されるとき。

```csharp
public AttractionTableViewController Controller { get; set;}
...

public AttractionTableDelegate (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
}
```

行を選択し、(Apple リモートのタッチ画面で、ユーザーがクリックした) をマークする、**引力**をお気に入りとして選択した行で表されます。

```csharp
public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
    attraction.IsFavorite = (!attraction.IsFavorite);

    // Update UI
    Controller.TableView.ReloadData ();
}
```

次に、ユーザー (設定することによりフォーカス Apple リモート タッチ画面を使用して)、行を強調表示したときたいの詳細を提示する、**引力**分割ビュー コント ローラーの詳細 セクションでは、その行によって表されます。

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

`CanFocusRow`テーブル ビューにフォーカスを取得しようとしてが行ごとにメソッドが呼び出されます。 返す`true`それ以外の場合を返す場合は、行は、フォーカスを取得できます、`false`です。 この例の場合は、カスタムを作成した`AttractionHighlighted`フォーカスを受け取るように、行ごとに発生するイベントです。

操作の詳細については、 `UITableViewDelegate`、Apple を参照してください[UITableViewDelegate](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40006942)ドキュメント。

<a name="The-Table-View-Cell" />

## <a name="the-table-view-cell"></a>テーブル ビューのセル

テーブル ビューのセルのカスタム インスタンスの作成もプロトタイプに各セルのインターフェイス デザイナー内のテーブル ビューに追加した (`UITableViewCell`) が作成されると、新しいセル (行) を設定するように要求します。

例のアプリをダブルクリックして、`AttractionTableCell.cs`ファイルをファイルを開いて編集し、次のようになります。

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

このクラスは、引力データ モデル オブジェクトの記憶域を提供 (`AttractionInformation`上に定義されている) 特定の行に表示されます。

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

`UpdateUI`メソッドは、必要に応じて (つまり、インターフェイス デザイナー内のセルのプロトタイプに追加された) UI ウィジェットを追加します。

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

例のアプリをダブルクリックして、`AttractionTableViewController.cs`ファイルをファイルを開いて編集し、次のようになります。

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

このクラスについて詳しく見てをみましょう。 テーブルのビューにアクセスしやすくへのショートカットを作成した最初に、`DataSource`と`TableDelegate`です。 使用します、後で分割ビューの左側にある内のテーブル ビューと右側の詳細ビューの間で通信します。

最後に、テーブルのビューがメモリに読み込まれると、私たちのインスタンスを作成、`AttractionTableDatasource`と`AttractionTableDelegate`(どちらも上記で作成) し、テーブルのビューに接続します。

操作の詳細については、 `UITableViewController`、Apple を参照してください[UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)ドキュメント。

<a name="Pulling-it-All-Together" />

## <a name="pulling-it-all-together"></a>すべての要素をプルし

このドキュメントの開始時に説明したように、テーブルのビューがの一方の側に通常表示、[分割ビュー](~/ios/tvos/user-interface/split-views.md)によるナビゲーション、反対側に表示される選択された項目の詳細を含むです。 例: 

[![](table-views-images/intro01.png "サンプル アプリの実行")](table-views-images/intro01.png#lightbox)

最後の手順すべてを一緒に見てみましょうと tvOS で標準的なパターンは、ため、分割ビューの左と右の辺は相互にやり取りができます。

<a name="The-Detail-View" />

### <a name="the-detail-view"></a>詳細表示

旅行のアプリの例については、カスタム クラス上に示した (`AttractionViewController`) は、標準的なビュー コント ローラーの詳細の表示と分割ビューの右側に表示される定義します。

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

ここが用意されて、**引力**(`AttractionInformation`) をプロパティとして表示され、作成、 `UpdateUI` UI ウィジェットを追加するメソッドがインターフェイス デザイナーのビューに追加します。

分割ビュー コント ローラーのショートカットも定義しています (`SplitView`) テーブルのビューに変更を通信するために使用されます (`AcctractionTableView`)。

最後に、カスタム アクション (イベント) は、3 つに追加された`UIButton`インターフェイス デザイナーで作成されたインスタンス、するアクセス許可として、引力をマークする、_お気に入り_、取得_方向_に、引力と_、フライトの予約_特定の都市にします。

<a name="The-Navigation-View-Controller" />

### <a name="the-navigation-view-controller"></a>ナビゲーション ビュー コント ローラー

ナビゲーション ビュー コント ローラーがカスタム クラスに割り当てられたテーブル ビュー コント ローラーは分割ビューの左側のナビゲーション ビューのコント ローラーで入れ子になっているため (`MasterNavigationController`) インターフェイス デザイナーで次のように定義されているとします。

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

ここでも、このクラスでは、分割ビュー コント ローラーの 2 つの辺を経由して通信を容易にできるように、いくつかのショートカットだけを定義します。

* `SplitView` -分割ビュー コント ローラーへのリンクです (`MainSpiltViewController`) がナビゲーション ビュー コント ローラーが属しています。
* `TableController` -テーブル ビュー コント ローラーを取得する (`AttractionTableViewController`) ナビゲーション ビュー コント ローラーの上位のビューとして表示されます。

<a name="The-Split-View-Controller" />

### <a name="the-split-view-controller"></a>分割ビュー コント ローラー

カスタム クラスを作成した分割ビュー コント ローラーは、アプリケーションのベースであるため (`MasterSplitViewController`) インターフェイス デザイナーでの次のように定義されているとします。

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

ショートカットを作成したり最初に、**詳細**分割ビューの側 (`AttractionViewController`) および、**マスター**側 (`MasterNavigationController`)。 ここでも、この場合、2 つの辺を後で間の通信にやすくなります。

次に、分割ビューがメモリに読み込まれると、分割ビュー コント ローラーを分割ビューの両方の側にアタッチし、表示するには、テーブル ビューで、引力を強調表示 (`AttractionHighlighted`) で新しい引力を表示することによって、**の詳細**面を分割ビュー。

参照してください、 [tvTables](https://developer.xamarin.com/samples/monotouch/tvos/tvTable/)分割ビュー内でテーブルのビューの完全な実装用のサンプル アプリケーションです。

## <a name="table-views-in-detail"></a>テーブルのビューの詳細

TvOS は iOS に基づいて、以降は設計されていますテーブルのビューおよびテーブル ビューのコント ローラーと同様の方法で動作します。 Xamarin アプリでのテーブル ビューの操作に関する情報の詳細は、iOS を参照してください[テーブルとセル操作](~/ios/user-interface/controls/tables/index.md)ドキュメント。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事は、設計と Xamarin.tvOS アプリ内でテーブルのビューの操作について説明しました。 分割ビューを tvOS アプリ内のテーブル ビューの一般的な使用法は、内部テーブルのビューの操作の例を提示したとします。



## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](https://developer.xamarin.com/samples/tvos/all/)
- [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマン インターフェイス ガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリケーション プログラミング ガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
