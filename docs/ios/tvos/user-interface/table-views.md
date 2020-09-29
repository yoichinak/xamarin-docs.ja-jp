---
title: Xamarin での tvOS テーブルビューの使用
description: この記事では、tvOS アプリ内のテーブルビューおよびテーブルビューコントローラーの設計と操作について説明します。
ms.prod: xamarin
ms.assetid: D8F80FA9-6400-4DB7-AFC9-A28A54AD04E8
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 364aa1ebc70517ee8378e603922486ae29adf6c1
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436438"
---
# <a name="working-with-tvos-table-views-in-xamarin"></a>Xamarin での tvOS テーブルビューの使用

_この記事では、tvOS アプリ内のテーブルビューおよびテーブルビューコントローラーの設計と操作について説明します。_

TvOS では、テーブルビューがスクロール行の単一の列として表示され、必要に応じてグループやセクションに整理することができます。 テーブルビューは、わかりやすい方法でユーザーに大量のデータを効率的に表示する必要がある場合に使用します。

通常、テーブルビューは [分割ビュー](~/ios/tvos/user-interface/split-views.md) の1つの側にナビゲーションとして表示され、選択した項目の詳細が反対側に表示されます。

[![サンプルテーブルビュー](table-views-images/intro01.png)](table-views-images/intro01.png#lightbox)

<a name="About-Table-Views"></a>

## <a name="about-table-views"></a>テーブルビューについて

では、スクロール可能な `UITableView` 行の単一の列が、必要に応じてグループやセクションに分類できる情報の階層リストとして表示されます。 

[![選択された項目](table-views-images/table01.png)](table-views-images/table01.png#lightbox)

Apple には、テーブルを操作するための次のような推奨事項があります。

- **幅に注意** してください。テーブルの幅で正しいバランスを取るようにしてください。 テーブルの幅が広すぎると、距離からのスキャンが困難になり、使用可能なコンテンツ領域から離れてしまう可能性があります。 テーブルの幅が狭すぎると、情報が切り捨てられたり折り返されたりする可能性があります。この場合も、ユーザーが部屋から読み取るのが困難になる可能性があります。
- **テーブルの内容をすばやく表示する** -データの大きなリストを表示するには、コンテンツを遅延読み込みし、テーブルがユーザーに表示されたらすぐに情報の表示を開始します。 テーブルの読み込みに時間がかかる場合、ユーザーはアプリの関心を失うか、ロックされていると考えられます。
- **長いコンテンツの読み込みをユーザーに通知** する-長いテーブルの読み込み時間が回避できない場合は、アプリがロックされていないことを示す [進行状況バーまたはアクティビティインジケーター](~/ios/tvos/user-interface/progress-indicators.md) を提示します。

<a name="Table-Cell-Types"></a>

## <a name="table-view-cell-types"></a>テーブルビューのセルの種類

は、 `UITableViewCell` テーブルビュー内の個々のデータ行を表すために使用されます。 Apple では、いくつかの既定のテーブルセルの種類が定義されています。

- **Default** -この型は、右側のセルと左揃えのタイトルの左側にオプションのイメージを表示します。 
- **サブタイトル** -この型では、最初の行に左揃えのタイトルが表示され、次の行に左揃えのサブタイトルが表示されます。
- **値 1** -この型は左揃えのタイトルを表し、同じ行に右揃えで表示される白のサブタイトルが表示されます。
- **値 2** -この型は、同じ行に左揃えの明るい字幕がある、右揃えのタイトルを示します。

既定のテーブルビューのすべてのセルの種類では、表示インジケーターやチェックマークなどのグラフィック要素もサポートされます。 

また、インターフェイスデザイナーまたはコードを使用して作成した **カスタム** テーブルビューのセルの種類を定義し、 _プロトタイプのセル_を表示することもできます。

Apple には、テーブルビューのセルを操作するための次のような推奨事項があります。

- **テキスト領域を避け** ます。テキストの各行を短くして、最終的には切り捨てられないようにします。 切り捨てられた単語や語句は、ユーザーが部屋を越えて解析するのは困難です。
- フォーカスさ**れている行の状態を考慮してください**。行が大きくなり、角が丸くなると、セルの外観をすべての状態でテストする必要があります。 画像またはテキストが、フォーカスされた状態でクリップされるか、正しく表示されない可能性があります。
- **編集可能なテーブルを控えめに使用する** -テーブル行の移動または削除では、iOS よりも時間がかかります tvOS。 この機能によって tvOS アプリが追加されるかどうかについては、慎重に判断する必要があります。
- **適切な場所にカスタムのセルの種類を作成** する-組み込みテーブルビューのセルの種類は多くの状況に適していますが、より高度な制御を提供し、ユーザーに情報を表示するために、標準以外の情報に対してカスタムのセル型を作成することを検討してください。

<a name="Working-With-Table-Views"></a>

## <a name="working-with-table-views"></a>テーブルビューの操作

TvOS アプリでテーブルビューを操作する最も簡単な方法は、インターフェイスデザイナーでその外観を作成および変更することです。

使用を開始するには、以下を実行します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio for Mac で、新しい tvOS app プロジェクトを開始し、[ **tvOS**  >  **app**  >  **Single View app** ] を選択し、[**次へ**] ボタンをクリックします。 

    [![単一ビューアプリを選択する](table-views-images/table02.png)](table-views-images/table02.png#lightbox)
1. アプリの **名前** を入力し、[ **次へ**] をクリックします。 

    [![アプリの名前を入力してください](table-views-images/table03.png)](table-views-images/table03.png#lightbox)
1. **プロジェクト名**と**ソリューション名**を調整するか、既定値をそのまま使用し、[**作成**] ボタンをクリックして新しいソリューションを作成します。 

    [![プロジェクト名とソリューション名](table-views-images/table04.png)](table-views-images/table04.png#lightbox)
1. **Solution Pad**で、ファイルをダブルクリックし `Main.storyboard` て、iOS Designer で開きます。 

    [![メインのストーリーボードファイル](table-views-images/table05.png)](table-views-images/table05.png#lightbox)
1. **既定のビューコントローラー**を選択して削除します。 

    [![既定のビューコントローラーを選択して削除する](table-views-images/table06.png)](table-views-images/table06.png#lightbox)
1. **ツールボックス**から**分割ビューコントローラー**を選択し、デザインサーフェイスにドラッグします。
1. 既定では、**ナビゲーションビューコントローラー** 、左側に**テーブルビュー**コントローラー、右側に**ビューコントローラー**がある[分割ビュー](~/ios/tvos/user-interface/split-views.md)が表示されます。 これは、tvOS のテーブルビューで使用される Apple の推奨される使用方法です。 

    [![分割ビューを追加する](table-views-images/table08.png)](table-views-images/table08.png#lightbox)
1. テーブルビューのすべての部分を選択し、[**プロパティエクスプローラー** ] の [**ウィジェット**] タブでカスタム**クラス名**を割り当てる必要があります。これにより、後で C# コードでアクセスできるようになります。 たとえば、 **テーブルビューコントローラー**は次のようになります。 

    [![クラス名を割り当てる](table-views-images/table09.png)](table-views-images/table09.png#lightbox)
1. **テーブルビューコントローラー**、**テーブルビュー** 、および**プロトタイプセル**のカスタムクラスを作成していることを確認します。 次のように、作成時にカスタムクラスをプロジェクトツリーに追加 Visual Studio for Mac ます。 

    [![プロジェクトツリー内のカスタムクラス](table-views-images/table10.png)](table-views-images/table10.png#lightbox)
1. 次に、デザインサーフェイスでテーブルビューを選択し、必要に応じてプロパティを調整します。 **プロトタイプセル**の数や**スタイル**(Plain またはグループ化) など。 

    [![[ウィジェット] タブ](table-views-images/table11.png)](table-views-images/table11.png#lightbox)
1. [**プロトタイプ] セル**ごとに、そのセルを選択し、[**プロパティエクスプローラー**] の [**ウィジェット**] タブで一意の**識別子**を割り当てます。 この手順は、後でテーブルを設定するときにこの識別子が必要になるため、 _非常に重要_ です。 例: `AttrCell` 

    [![[ウィジェット] タブ](table-views-images/table12.png)](table-views-images/table12.png#lightbox)
1. また、[**スタイル**] ドロップダウンを使用してセルを[既定のテーブルビュー](#table-view-cell-types)の1つとして表示するか、[**カスタム**] に設定して、デザインサーフェイスを使用して、他の UI ウィジェットを**ツールボックス**からドラッグしてセルをレイアウトすることもできます。 

    [![セルのレイアウト](table-views-images/table13.png)](table-views-images/table13.png#lightbox)
1. [**プロパティエクスプローラー** ] の [**ウィジェット**] タブで、プロトタイプセルのデザインの各 UI 要素に一意の**名前**を割り当てます。これにより、後で C# コードでアクセスできるようになります。 

    [![名前の割り当て](table-views-images/table14.png)](table-views-images/table14.png#lightbox)
1. テーブルビューのすべてのプロトタイプセルに対して上記の手順を繰り返します。
1. 次に、UI デザインの残りの部分にカスタムクラスを割り当てます。詳細ビューをレイアウトし、詳細ビューの各 UI 要素に一意の **名前** を割り当てて、C# でもアクセスできるようにします。 たとえば次のようになります。 

    [![UI レイアウト](table-views-images/table15.png)](table-views-images/table15.png#lightbox)
1. ストーリーボードへの変更を保存します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Visual Studio で、新しい tvOS app プロジェクトを開始し、[ **tvOS**  >  **Single View app] \ (シングルビューアプリ**の作成 \) を選択して、アプリの名前を入力します。 [ **Ok** ] をクリックして、新しいソリューションを作成します。 

    [![単一ビューアプリを選択する](table-views-images/table02-vs.png)](table-views-images/table02-vs.png#lightbox)
1. **ソリューションエクスプローラー**で、ファイルをダブルクリックし `Main.storyboard` て、iOS Designer で開きます。 

    [![メインのストーリーボードファイル](table-views-images/table05-vs.png)](table-views-images/table05-vs.png#lightbox)
1. **既定のビューコントローラー**を選択して削除します。 

    [![既定のビューコントローラーを選択して削除する](table-views-images/table06-vs.png)](table-views-images/table06-vs.png#lightbox)
1. **ツールボックス**から**分割ビューコントローラー**を選択し、デザインサーフェイスにドラッグします。 

    [![分割ビューコントローラー](table-views-images/table07-vs.png)](table-views-images/table07-vs.png#lightbox)
1. 既定では、**ナビゲーションビューコントローラー** 、左側に**テーブルビュー**コントローラー、右側に**ビューコントローラー**がある[分割ビュー](~/ios/tvos/user-interface/split-views.md)が表示されます。 これは、tvOS のテーブルビューで使用される Apple の推奨される使用方法です。 

    [![UI のレイアウト](table-views-images/table08-vs.png)](table-views-images/table08-vs.png#lightbox)
1. テーブルビューのすべての部分を選択し、[**プロパティエクスプローラー** ] の [**ウィジェット**] タブでカスタム**クラス名**を割り当てる必要があります。これにより、後で C# コードでアクセスできるようになります。 たとえば、 **テーブルビューコントローラー**は次のようになります。 

    [![[ウィジェット] タブ](table-views-images/table09-vs.png)](table-views-images/table09-vs.png#lightbox)
1. **テーブルビューコントローラー**、**テーブルビュー** 、および**プロトタイプセル**のカスタムクラスを作成していることを確認します。 次のように、作成時にカスタムクラスをプロジェクトツリーに追加 Visual Studio for Mac ます。 

    [![プロジェクトツリー内のカスタムクラス](table-views-images/table10-vs.png)](table-views-images/table10-vs.png#lightbox)
1. 次に、デザインサーフェイスでテーブルビューを選択し、必要に応じてプロパティを調整します。 **プロトタイプセル**の数や**スタイル**(Plain またはグループ化) など。 

    [![[ウィジェット] タブ](table-views-images/table11-vs.png)](table-views-images/table11-vs.png#lightbox)
1. [**プロトタイプ] セル**ごとに、そのセルを選択し、[**プロパティエクスプローラー**] の [**ウィジェット**] タブで一意の**識別子**を割り当てます。 この手順は、後でテーブルを設定するときにこの識別子が必要になるため、 _非常に重要_ です。 例: `AttrCell` 

    [![識別子を割り当てる](table-views-images/table12-vs.png)](table-views-images/table12-vs.png#lightbox)
1. また、[**スタイル**] ドロップダウンを使用してセルを[既定のテーブルビュー](#table-view-cell-types)の1つとして表示するか、[**カスタム**] に設定して、デザインサーフェイスを使用して、他の UI ウィジェットを**ツールボックス**からドラッグしてセルをレイアウトすることもできます。 

    [![スタイルドロップダウン](table-views-images/table13-vs.png)](table-views-images/table13-vs.png#lightbox)
1. [**プロパティエクスプローラー** ] の [**ウィジェット**] タブで、プロトタイプセルのデザインの各 UI 要素に一意の**名前**を割り当てます。これにより、後で C# コードでアクセスできるようになります。 

    [![[ウィジェット] タブ](table-views-images/table14-vs.png)](table-views-images/table14-vs.png#lightbox)
1. テーブルビューのすべてのプロトタイプセルに対して上記の手順を繰り返します。
1. 次に、UI デザインの残りの部分にカスタムクラスを割り当てます。詳細ビューをレイアウトし、詳細ビューの各 UI 要素に一意の **名前** を割り当てて、C# でもアクセスできるようにします。 たとえば次のようになります。 

    [![UI レイアウト](table-views-images/table15.png)](table-views-images/table15.png#lightbox)
1. ストーリーボードへの変更を保存します。

-----

<a name="Designing-a-Data-Model"></a>

## <a name="designing-a-data-model"></a>データモデルのデザイン

テーブルビューに表示される情報を簡単に使用できるようにするには (ユーザーがテーブルビューの行を選択または強調表示したときに)、詳細情報を簡単に表示できるようにするには、表示される情報のデータモデルとして機能するカスタムクラスまたはクラスを作成します。

旅行予約アプリの例を見てください。これには、ユーザーが選択できる**アトラクション**の一意のリストを含む**都市**の一覧が含まれています。 ユーザーは、引力を *お気に入り*としてマークすることができます。 *引力を選択* し、特定の都市に *フライトを予約* します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

**引力**のデータモデルを作成するには、 **Solution Pad**でプロジェクト名を右クリックし、[ **Add**  >  **新しいファイル**の追加] を選択します。`AttractionInformation`**名前**として「」と入力し、[**新規**] ボタンをクリックします。 

[![名前に「AttractionInformation」と入力します。](table-views-images/data01.png)](table-views-images/data01.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

**引力**のデータモデルを作成するには、**ソリューションエクスプローラー**でプロジェクト名を右クリックし、[ **Add**  >  **新しい項目**の追加] を選択します。[**クラス**] を選択し、名前として「」と入力 `AttractionInformation` して、[**追加**] ボタンをクリックします。 **Name** 

[![[クラス] を選択し、名前に「AttractionInformation」と入力します。](table-views-images/data01-vs.png)](table-views-images/data01-vs.png#lightbox)

-----

ファイルを編集 `AttractionInformation.cs` し、次のように表示します。

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

このクラスは、指定された **引力**に関する情報を格納するためのプロパティを提供します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

次に、 **Solution Pad**でプロジェクト名を右クリックし、[ **Add**  >  **新しいファイル**の追加] を選択します。`CityInformation`**名前**として「」と入力し、[**新規**] ボタンをクリックします。 

[![名前として「CityInformation」と入力します。](table-views-images/data02.png)](table-views-images/data02.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

次に、**ソリューションエクスプローラー**でプロジェクト名を右クリックし、[ **Add**  >  **新しい項目**の追加] を選択します。名前として「」と入力し、 `CityInformation` [**追加**] ボタンをクリックします。 **Name** 

[![名前として「CityInformation」と入力します。](table-views-images/data02-vs.png)](table-views-images/data02-vs.png#lightbox)

-----

ファイルを編集 `CityInformation.cs` し、次のように表示します。

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

このクラスは、宛先の **都市**に関するすべての情報を保持し、その都市の **アトラクション** のコレクションであり、2つのヘルパーメソッド () を提供して、 `AddAttraction` 都市へのアトラクションの追加を容易にします。

<a name="The-Table-Data-Source"></a>

## <a name="the-table-view-data-source"></a>テーブルビューのデータソース

各テーブルビューでは、テーブル `UITableViewDataSource` のデータを提供し、テーブルビューで必要とされる必要な行を生成するために、データソース () が必要です。

上記の例では、**ソリューションエクスプローラー**でプロジェクト名を右クリックし、[新しいファイルの**追加**...] を選択して呼び出し、  >  **New File...** `AttractionTableDatasource` [**新規**作成] ボタンをクリックします。 次に、ファイルを編集 `AttractionTableDatasource.cs` し、次のように表示します。

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

クラスのいくつかのセクションを詳しく見てみましょう。

まず、プロトタイプセルの一意の識別子 (上記のインターフェイスデザイナーで割り当てられた同じ識別子) を保持する定数を定義し、テーブルビューコントローラーにもう一度ショートカットを追加し、データ用にストレージを作成しました。

```csharp
const string CellID = "AttrCell";
public AttractionTableViewController Controller { get; set;}
public List<CityInformation> Cities { get; set;}
```

次に、テーブルビューコントローラーを保存します。次に、クラスの作成時にデータソースを作成し、データソース (上記で定義したデータモデルを使用) を設定します。

```csharp
public AttractionTableDatasource (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
    this.Cities = new List<CityInformation> ();
    PopulateCities ();
}
```

たとえば、 `PopulateCities` メソッドは単にメモリ内にデータモデルオブジェクトを作成しますが、実際のアプリのデータベースや web サービスから簡単に読み取ることができます。

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

メソッドは、 `NumberOfSections` テーブル内のセクションの数を返します。

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    // Return number of cities
    return Cities.Count;
}
```

**形式**がスタイルのテーブルビューでは、常に1を返します。

メソッドは、 `RowsInSection` 現在のセクション内の行の数を返します。

```csharp
public override nint RowsInSection (UITableView tableView, nint section)
{
    // Return the number of attractions in the given city
    return Cities [(int)section].Attractions.Count;
}
```

この場合も、表 **形式** のビューの場合は、データソース内の項目の合計数を返します。

メソッドは、 `TitleForHeader` 指定されたセクションのタイトルを返します。

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    // Get the name of the current city
    return Cities [(int)section].Name;
}
```

**単純**なテーブルビューの種類の場合は、タイトルを空白のままに `""` します ()。

最後に、テーブルビューによって要求された場合は、メソッドを使用してプロトタイプセルを作成して設定し `GetCell` ます。 

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

の使用方法の詳細については `UITableViewDatasource` 、Apple の [Uitableviewdatasource](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40006941) のドキュメントを参照してください。

<a name="The-Table-View-Delegate"></a>

## <a name="the-table-view-delegate"></a>テーブルビューデリゲート

各テーブルビュー `UITableViewDelegate` では、ユーザーの操作やテーブルのその他のシステムイベントに応答するために、デリゲート () が必要です。

上記の例では、**ソリューションエクスプローラー**でプロジェクト名を右クリックし、[新しいファイルの**追加**...] を選択して呼び出し、  >  **New File...** `AttractionTableDelegate` [**新規**作成] ボタンをクリックします。 次に、ファイルを編集 `AttractionTableDelegate.cs` し、次のように表示します。

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

詳細については、このクラスのいくつかのセクションを参照してください。

まず、クラスの作成時にテーブルビューコントローラーへのショートカットを作成します。

```csharp
public AttractionTableViewController Controller { get; set;}
...

public AttractionTableDelegate (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
}
```

次に、行が選択されている場合 (ユーザーが Apple リモコンのタッチ画面をクリックしたとき)、選択した行によって表される **引力** をお気に入りとしてマークします。

```csharp
public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
    attraction.IsFavorite = (!attraction.IsFavorite);

    // Update UI
    Controller.TableView.ReloadData ();
}
```

次に、ユーザーが (Apple リモートタッチサーフェイスを使用してフォーカスを与えることによって) 行を強調表示したときに、分割ビューコントローラーの [詳細] セクションで、その行によって表される **引力** の詳細を表示します。

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

`CanFocusRow`テーブルビューでフォーカスを取得しようとしている行ごとに、メソッドが呼び出されます。 `true`行がフォーカスを得ることができる場合はを返し、それ以外の場合はを返し `false` ます。 この例では、 `AttractionHighlighted` フォーカスを受け取るたびに各行で発生するカスタムイベントを作成しました。

の使用方法の詳細については `UITableViewDelegate` 、Apple の [Uitableviewdelegate](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40006942) に関するドキュメントを参照してください。

<a name="The-Table-View-Cell"></a>

## <a name="the-table-view-cell"></a>テーブルビューセル

インターフェイスデザイナーでテーブルビューに追加した各プロトタイプセルについても、テーブルビューのセル () のカスタムインスタンスを作成し `UITableViewCell` て、新しいセル (行) を作成時に設定できるようにしました。

このサンプルアプリでは、ファイルをダブルクリックし `AttractionTableCell.cs` て編集用に開き、次のように表示します。

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

このクラスは、指定された行に表示される引力 Data Model オブジェクト (前述のとおり) のストレージを提供し `AttractionInformation` ます。

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

メソッドは、 `UpdateUI` 必要に応じて UI ウィジェット (インターフェイスデザイナーでセルのプロトタイプに追加されたもの) を設定します。

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

の使用方法の詳細については `UITableViewCell` 、Apple の [Uitableviewcell](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewCell_Class/index.html#//apple_ref/doc/uid/TP40006938) のドキュメントを参照してください。

<a name="The-Table-View-Controller"></a>

## <a name="the-table-view-controller"></a>テーブルビューコントローラー

テーブルビューコントローラー () は、 `UITableViewController` インターフェイスデザイナーを使用してストーリーボードに追加されたテーブルビューを管理します。

このサンプルアプリでは、ファイルをダブルクリックし `AttractionTableViewController.cs` て編集用に開き、次のように表示します。

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

このクラスについて詳しく見ていきましょう。 まず、テーブルビューのおよびに簡単にアクセスできるようにショートカットを作成しました `DataSource` `TableDelegate` 。 これらを後で使用して、分割ビューの左側にあるテーブルビューと右側の詳細ビューの間の通信を行います。

最後に、テーブルビューがメモリに読み込まれるときに、とのインスタンスを作成 `AttractionTableDatasource` `AttractionTableDelegate` し (どちらも上記で作成したもの)、テーブルビューにアタッチします。

の使用方法の詳細については `UITableViewController` 、Apple の [Uitableviewcontroller](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523) のドキュメントを参照してください。

<a name="Pulling-it-All-Together"></a>

## <a name="pulling-it-all-together"></a>すべてをまとめてプルする

このドキュメントの冒頭で説明したように、通常、テーブルビューは [分割ビュー](~/ios/tvos/user-interface/split-views.md) の1つの側にナビゲーションとして表示され、選択した項目の詳細が反対側に表示されます。 次に例を示します。 

[![サンプルアプリの実行](table-views-images/intro01.png)](table-views-images/intro01.png#lightbox)

これは tvOS の標準パターンなので、最後の手順を見て、すべてをまとめて、分割ビューの左右左右に相互作用させます。

<a name="The-Detail-View"></a>

### <a name="the-detail-view"></a>詳細ビュー

上に示した旅行アプリの例では、 `AttractionViewController` 詳細ビューとして分割ビューの右側に表示される標準ビューコントローラーに対してカスタムクラス () が定義されています。

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

ここでは、 **Attraction** `AttractionInformation` プロパティとして表示される引力 () を指定し、 `UpdateUI` インターフェイスデザイナーのビューに追加された UI ウィジェットを設定するメソッドを作成しました。

また、 `SplitView` テーブルビュー () に変更を反映するために使用する分割ビューコントローラー () に戻るショートカットも定義されてい `AcctractionTableView` ます。

最後に、カスタムアクション (イベント) が、インターフェイスデザイナーで作成された3つのインスタンスに追加されました。これに `UIButton` より、 _Directions_ユーザーは引力を_お気に入り_としてマークし、引力に移動して、特定の都市に_フライトを Book_ことができます。

<a name="The-Navigation-View-Controller"></a>

### <a name="the-navigation-view-controller"></a>ナビゲーションビューコントローラー

テーブルビューコントローラーは分割ビューの左側にあるナビゲーションビューコントローラーに入れ子になっているため、ナビゲーションビューコントローラーにはインターフェイスデザイナーでカスタムクラス () が割り当てられ、次のように定義されてい `MasterNavigationController` ます。

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

ここでも、このクラスは、分割ビューコントローラーの2つの側での通信を容易にするいくつかのショートカットを定義しています。

- `SplitView` - `MainSpiltViewController` ナビゲーションビューコントローラーが属している分割ビューコントローラー () へのリンクです。
- `TableController` - `AttractionTableViewController` ナビゲーションビューコントローラーの最上位ビューとして表示されるテーブルビューコントローラー () を取得します。

<a name="The-Split-View-Controller"></a>

### <a name="the-split-view-controller"></a>分割ビューコントローラー

分割ビューコントローラーはアプリケーションのベースであるため、インターフェイスデザイナーでカスタムクラス () を作成 `MasterSplitViewController` し、次のように定義しました。

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

まず、分割ビュー ( **Details** `AttractionViewController` ) と**マスター**側 () の詳細な側面へのショートカットを作成し `MasterNavigationController` ます。 この場合も、2つの側の間での通信が容易になります。

次に、分割ビューがメモリに読み込まれるときに、分割ビューの両側に分割ビューコントローラーをアタッチし、テーブルビュー () の引力を強調表示しているユーザーに応答し `AttractionHighlighted` ます。これにより、分割ビューの **詳細** 側に新しい引力が表示されます。

分割ビュー内のテーブルビューの完全な実装については、 [tvTables](/samples/xamarin/ios-samples/tvos-tvtable) サンプルアプリを参照してください。

## <a name="table-views-in-detail"></a>テーブルビューの詳細

TvOS は iOS に基づいているため、テーブルビューとテーブルビューコントローラーは同様の方法で設計および動作します。 Xamarin アプリでテーブルビューを操作する方法の詳細については、 [テーブルとセルの操作](~/ios/user-interface/controls/tables/index.md) に関するドキュメントを参照してください。

<a name="Summary"></a>

## <a name="summary"></a>まとめ

この記事では、tvOS アプリ内でのテーブルビューの設計と操作について説明しました。 とでは、分割ビュー内でテーブルビューを操作する例を示しています。これは、tvOS アプリでのテーブルビューの一般的な使用方法です。

## <a name="related-links"></a>関連リンク

- [tvOS のサンプル](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS ヒューマンインターフェイスガイド](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS のアプリプログラミングガイド](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)