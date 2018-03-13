---
title: "リスト ビューの外観のカスタマイズ"
ms.topic: article
ms.prod: xamarin
ms.assetid: B09AD282-2C4F-D71E-6806-9B1EF05C2CD4
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 1bf481e4999365f4afc52cb9dda83c6e627950e1
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="customizing-a-listviews-appearance"></a>リスト ビューの外観のカスタマイズ


## <a name="overview"></a>概要

ListView の外観は、表示されている行のレイアウトによって決まります。 外観を変更する、 `ListView`、別の行のレイアウトを使用します。


## <a name="built-in-row-views"></a>組み込みの行ビュー

使用して参照できる組み込みの 12 個のビューがある**Android.Resource.Layout**:

- **TestListItem** &ndash;を最小限に抑える書式付きテキストの行を 1 つです。

- **SimpleListItem1** &ndash; 1 つの行のテキスト。

- **SimpleListItem2** &ndash; 2 行のテキスト。

- **SimpleSelectableListItem** &ndash; 1 つの行の 1 つまたは複数の項目の選択 (API レベル 11 で追加) をサポートするテキスト。

- **SimpleListItemActivated1** &ndash; SimpleListItem1 に似ていますが、背景色を示す行を選択すると (API レベル 11 で追加) します。

- **SimpleListItemActivated2** &ndash; SimpleListItem2 に似ていますが、背景色を示す行を選択すると (API レベル 11 で追加) します。

- **SimpleListItemChecked** &ndash;選択範囲を示すためにチェック マークが表示されます。

- **SimpleListItemMultipleChoice** &ndash; Displays check boxes to indicate multiple-choice selection.

- **SimpleListItemSingleChoice** &ndash;表示オプションを相互に排他的な選択を示すボタン。

- **TwoLineListItem** &ndash; 2 行のテキスト。

- **ActivityListItem** &ndash;イメージとテキストの行を 1 つです。

- **SimpleExpandableListItem** &ndash;カテゴリ、および各グループでの行のグループを展開または折りたたむことができます。

各組み込みの行のビューには、関連付けられている組み込みスタイルがあります。 これらのスクリーン ショットは、各ビューの表示方法を示しています。

[![TestListItem、SimpleSelectableListItem、SimpleListitem1、および SimpleListItem2 のスクリーン ショット](customizing-appearance-images/builtinviews.png)](customizing-appearance-images/builtinviews.png#lightbox)

[![SimpleListItemActivated1、SimpleListItemActivated2、SimpleListItemChecked、および SimpleListItemMultipleChecked のスクリーン ショット](customizing-appearance-images/builtinviews-2.png)](customizing-appearance-images/builtinviews-2.png#lightbox)

[![SimpleListItemSingleChoice、TwoLineListItem、ActivityListItem、および SimpleExpandableListItem のスクリーン ショット](customizing-appearance-images/builtinviews-3.png)](customizing-appearance-images/builtinviews-3.png#lightbox)

**BuiltInViews/HomeScreenAdapter.cs**サンプル ファイル (で、 **BuiltInViews**ソリューション) 展開不可能なリスト項目の画面表示を生成するためにコードが含まれています。 ビュー設定されて、`GetView`次のようなメソッド。

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
```

標準コントロールの識別子を参照することによって、ビューのプロパティを設定できます`Text1`、`Text2`と`Icon` `Android.Resource.Id` (ビューがないか、例外がスローされるプロパティを設定しない)。

```csharp
view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = item.Heading;
view.FindViewById<TextView>(Android.Resource.Id.Text2).Text = item.SubHeading;
view.FindViewById<ImageView>(Android.Resource.Id.Icon).SetImageResource(item.ImageResourceId); // only use with ActivityListItem
```

**BuiltInExpandableViews/ExpandableScreenAdapter.cs**サンプル ファイル (で、 **BuiltInViews**ソリューション) SimpleExpandableListItem 画面を生成するためにコードが含まれています。 グループのビューを設定、`GetGroupView`次のようなメソッド。

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem1, null);
```

子ビューを設定、`GetChildView`次のようなメソッド。

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem2, null);
```

標準を参照することで、グループのビューと、子ビューのプロパティを設定することができますし、`Text1`と`Text2`上記のように、識別子を制御します。 (上記) SimpleExpandableListItem スクリーン ショットは、1 行のグループの表示 (SimpleExpandableListItem1) と 2 行の子ビュー (SimpleExpandableListItem2) の例を示します。 代わりに、グループ ビューは、2 つの行 (SimpleExpandableListItem2) 用に構成できますと、子ビューは、1 つの行 (SimpleExpandableListItem1) 用に構成できますまたは両方のグループ化ビューと子ビューは、同じ数の行を持つことができます。 



## <a name="accessories"></a>アクセサリ

行は、選択状態を示すために、ビューの右側に追加するアクセサリを持つことができます。

- **SimpleListItemChecked** &ndash;チェック付きのインジケーターとして単一選択のリストを作成します。

- **SimpleListItemSingleChoice** &ndash;のみ 1 つの選択肢が不可能なラジオ ボタン-型のリストを作成します。

- **SimpleListItemMultipleChoice** &ndash;複数の選択肢も有効である チェック ボックス型リストを作成します。

ここに挙げたアクセサリは、それぞれの順序で、次の画面の例を示します。

[![スクリーン ショットの SimpleListItemChecked、SimpleListItemSingleChoice、およびアクセサリと SimpleListItemMultipleChoice](customizing-appearance-images/accessories.png)](customizing-appearance-images/accessories.png#lightbox)

これらのアクセサリ パスのいずれかを表示するには、アダプターに必要なレイアウトのリソース ID し、手動で設定、必要な行の選択状態。 次のコード行を作成して割り当てる方法を示しています、`Adapter`これらのレイアウトのいずれかを使用します。

```csharp
ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItemChecked, items);
```

`ListView`自体が表示されているアクセサーに関係なく、別の選択モードをサポートしています。 混乱を避けるためには、次のように使用します。`Single`選択モードと`Checked`と`SingleChoice`アクセサリと`Multiple`モードと、`MultipleChoice`スタイル。 選択モードがによって制御される、`ChoiceMode`のプロパティ、`ListView`です。


### <a name="handling-api-level"></a>処理 API レベル

Xamarin.Android の以前のバージョンでは、整数のプロパティとして列挙型を実装します。 最新バージョンは型が導入されました適切な .NET 列挙これは、潜在的なオプションを探索するより簡単です。

API レベルによっては、次の対象に、`ChoiceMode`が整数または列挙体です。 サンプル ファイル**AccessoryViews/HomeScreen.cs**がブロック コメント アウト ジンジャーブレッド API を対象にする場合。

```csharp
// For targeting Gingerbread the ChoiceMode is an int, otherwise it is an
// enumeration.

lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // 1
//lv.ChoiceMode = Android.Widget.ChoiceMode.Multiple; // 2
//lv.ChoiceMode = Android.Widget.ChoiceMode.None; // 0

// Use this block if targeting Gingerbread or lower
/*
lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // Single
//lv.ChoiceMode = 0; // none
//lv.ChoiceMode = 2; // Multiple
//lv.ChoiceMode = 3; // MultipleModal
*/
```


### <a name="selecting-items-programmatically"></a>プログラムで項目を選択します。

実行するアイテムを手動で設定が '選択'、`SetItemChecked`メソッド (呼び出せる複数回の複数選択)。

```csharp
// Set the initially checked row ("Fruits")
lv.SetItemChecked(1, true);
```

コードは、複数の選択肢を異なる単一の選択項目を検出するためにも必要です。 選択されている行を決定する`Single`モードを使用して、`CheckedItemPosition`整数のプロパティ。

```csharp
FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPosition
```

選択した行を決定する`Multiple`モードをループ処理する必要があります、 `CheckedItemPositions` `SparseBooleanArray`です。 スパース配列はのみエントリを含むディクショナリのように、値が変更されているため探して配列全体を走査する必要がありますに`true`値が選択された内容の一覧で次のコード スニペットに示すように次のトピックします。

```csharp
var sparseArray = FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPositions;
for (var i = 0; i < sparseArray.Size(); i++ )
{
   Console.Write(sparseArray.KeyAt(i) + "=" + sparseArray.ValueAt(i) + ",");
}
Console.WriteLine();
```


## <a name="creating-custom-row-layouts"></a>カスタムの行のレイアウトを作成します。

組み込みの行の 4 つのビューでは、非常に単純です。 (電子メール、またはツイート、連絡先情報の一覧) などのより複雑なレイアウトを表示するには、カスタム ビューが必要です。 カスタム ビューが AXML ファイルとして一般に宣言されている、**リソース/レイアウト**ディレクトリおよびから読み込まれるは、リソース Id で、カスタム アダプターを使用します。 ビューは、カスタムの色、フォントおよびレイアウトと表示クラス (するテキスト ビュー、ImageViews 他のコントロールなど) の任意の数を含めることができます。

この例は、さまざまな方法で、前の例とは異なります。

-  継承`Activity`ではなく、`ListActivity`です。 いずれかの行をカスタマイズすることができます`ListView`でその他のコントロールが含めることもできますが、 `Activity` (見出し、ボタンやその他のユーザー インターフェイス要素) などのレイアウトです。 この例は、上記の見出しを追加、`ListView`を示しています。

-  画面の AXML レイアウト ファイルが必要です前の例で、`ListActivity`レイアウト ファイルは不要です。 この AXML が含まれています、`ListView`宣言を制御します。

-  各の行を表示するために AXML レイアウト ファイルが必要です。 この AXML ファイルには、カスタムのフォントおよび色の設定でテキストおよびイメージ コントロールが含まれています。

-  省略可能なカスタム セレクター XML ファイルを使用するを選択すると、行の外観を設定します。

-  `Adapter`実装には、カスタム レイアウトからが返されます、`GetView`をオーバーライドします。

-  `ItemClick` 異なる方法で宣言する必要があります (にイベント ハンドラーがアタッチされて`ListView.ItemClick`オーバーライドするのではなく`OnListItemClick`で`ListActivity`)。


以降で、アクティビティの表示と、行のカスタム ビューを作成して、それらを表示するためには、アダプターとアクティビティへの変更に対応し、以下、これらの変更で説明します。


### <a name="adding-a-listview-to-an-activity-layout"></a>アクティビティのレイアウトに ListView を追加します。

`HomeScreen`から継承しなく`ListActivity`ホーム スクリーンの表示のレイアウト AXML ファイルを作成する必要がありますので、既定のビューがないです。 この例では、ビューは、見出し (を使用して、 `TextView`) および`ListView`データを表示します。 レイアウトがで定義されている、 **Resources/Layout/HomeScreen.axml**ここに表示されるファイル。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="fill_parent"
   android:layout_height="fill_parent">
    <TextView android:id="@+id/Heading"
        android:text="Vegetable Groups"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:background="#00000000"
        android:textSize="30dp"
        android:textColor="#FF267F00"
        android:textStyle="bold"
        android:padding="5dp"
    />
    <ListView android:id="@+id/List"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:cacheColorHint="#FFDAFF7F"
    />
</LinearLayout>
```

使用する利点、`Activity`カスタム レイアウトを持つ (の代わりに、 `ListActivity`) 見出しなどの画面に追加のコントロールを追加できることにあります`TextView`この例ではします。


### <a name="creating-a-custom-row-layout"></a>カスタムの行のレイアウトの作成

リスト ビューに表示される行ごとのカスタム レイアウトを格納するには、別の AXML レイアウト ファイルが必要です。 この例では、行は、緑の背景色、茶色のテキストおよびイメージの右揃えがあります。 このレイアウトを宣言する Android の XML マークアップについては、「 **Resources/Layout/CustomView.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout  xmlns:android="http://schemas.android.com/apk/res/android"
   android:layout_width="fill_parent"
   android:layout_height="wrap_content"
   android:background="#FFDAFF7F"
   android:padding="8dp">
    <LinearLayout android:id="@+id/Text"
       android:orientation="vertical"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:paddingLeft="10dip">
        <TextView
         android:id="@+id/Text1"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textColor="#FF7F3300"
         android:textSize="20dip"
         android:textStyle="italic"
         />
        <TextView
         android:id="@+id/Text2"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:textSize="14dip"
         android:textColor="#FF267F00"
         android:paddingLeft="100dip"
         />
    </LinearLayout>
    <ImageView
        android:id="@+id/Image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:padding="5dp"
        android:src="@drawable/icon"
        android:layout_alignParentRight="true" />
</RelativeLayout >
```

カスタムの行のレイアウトは、多くの異なるコントロールを含めることができます、スクロールのパフォーマンスが影響を受ける複雑なデザインし、(特に、ネットワーク経由で読み込まれる必要がある) 場合にイメージを使用します。 スクロールのパフォーマンスの問題に対処の詳細については、Google の資料を参照してください。


### <a name="referencing-a-custom-row-view"></a>行のカスタム ビューを参照します。

例では、カスタム アダプターの実装は`HomeScreenAdapter.cs`します。 重要なメソッドは`GetView`でリソース ID を使用してカスタム AXML を読み込む`Resource.Layout.CustomView`、し、それぞれの返送前に、ビュー内のコントロールのプロパティを設定します。 完全なアダプター クラスが表示されます。

```csharp
public class HomeScreenAdapter : BaseAdapter<TableItem> {
   List<TableItem> items;
   Activity context;
   public HomeScreenAdapter(Activity context, List<TableItem> items)
       : base()
   {
       this.context = context;
       this.items = items;
   }
   public override long GetItemId(int position)
   {
       return position;
   }
   public override TableItem this[int position]
   {
       get { return items[position]; }
   }
   public override int Count
   {
       get { return items.Count; }
   }
   public override View GetView(int position, View convertView, ViewGroup parent)
   {
       var item = items[position];
       View view = convertView;
       if (view == null) // no view to re-use, create new
           view = context.LayoutInflater.Inflate(Resource.Layout.CustomView, null);
       view.FindViewById<TextView>(Resource.Id.Text1).Text = item.Heading;
       view.FindViewById<TextView>(Resource.Id.Text2).Text = item.SubHeading;
       view.FindViewById<ImageView>(Resource.Id.Image).SetImageResource(item.ImageResourceId);
       return view;
   }
}
```


### <a name="referencing-the-custom-listview-in-the-activity"></a>カスタム アクティビティで ListView を参照します。

`HomeScreen`からクラスを今すぐ継承`Activity`、`ListView`フィールドが、AXML で宣言されているコントロールへの参照を保持するクラスで宣言されています。

```csharp
ListView listView;
```

クラスは、アクティビティのカスタム レイアウト AXML を読み込む必要がありますしを使用して、`SetContentView`メソッドです。 次を検索できる、`ListView`レイアウト内のコントロールを作成し、アダプターに割り当てます、クリック ハンドラーを割り当てます。 OnCreate メソッドのコードを次に示します。

```csharp
SetContentView(Resource.Layout.HomeScreen); // loads the HomeScreen.axml as this activity's view
listView = FindViewById<ListView>(Resource.Id.List); // get reference to the ListView in the layout

// populate the listview with data
listView.Adapter = new HomeScreenAdapter(this, tableItems);
listView.ItemClick += OnListItemClick;  // to be defined
```

最後に、`ItemClick`ハンドラーを定義する必要があります。 ここではそれだけが表示されます、`Toast`メッセージ。

```csharp
void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
   var listView = sender as ListView;
   var t = tableItems[e.Position];
   Android.Widget.Toast.MakeText(this, t.Heading, Android.Widget.ToastLength.Short).Show();
}
```

次のような結果として得られる画面が表示。

[![結果として得られる CustomRowView のスクリーン ショット](customizing-appearance-images/customrowview.png)](customizing-appearance-images/customrowview.png#lightbox)



### <a name="customizing-the-row-selector-color"></a>行セレクターの色をカスタマイズします。

行が影響を受けるときにユーザーからのフィードバックを強調表示されます必要があります。 カスタム ビューと背景色として指定すると**CustomView.axml**もオーバーライド選択の強調表示します。 内のコードには、この行**CustomView.axml**セットもが明るい緑、バック グラウンドでは、行が影響を受けるときに、見た目ではないことを意味します。

```xml
android:background="#FFDAFF7F"
```

再を有効にして、強調表示動作も使用される色をカスタマイズするは、バック グラウンド属性をカスタム セレクターを設定して代わりにします。 セレクターは、既定の背景色と強調表示色の両方を宣言します。 ファイル**Resources/Drawable/CustomSelector.xml**次の宣言が含まれています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
<item android:state_pressed="false"
  android:state_selected="false"
  android:drawable="@color/cellback" />
<item android:state_pressed="true" >
  <shape>
     <gradient
      android:startColor="#E77A26"
        android:endColor="#E77A26"
        android:angle="270" />
  </shape>
</item>
<item android:state_selected="true"
  android:state_pressed="false"
  android:drawable="@color/cellback" />
</selector>
```

カスタム セレクターを参照するには、バック グラウンド属性を変更**CustomView.axml**に。

```xml
android:background="@drawable/CustomSelector"
```

選択した行と、対応する`Toast`今すぐ次のようにメッセージします。

[![選択した行の名前が表示されるトースト メッセージと共に、オレンジ色の選択した行](customizing-appearance-images/customselectcolor.png)](customizing-appearance-images/customselectcolor.png#lightbox)



### <a name="preventing-flickering-on-custom-layouts"></a>カスタム レイアウトのちらつきを防止

Android のパフォーマンスを改善しようとしました。`ListView`レイアウト情報をキャッシュしてスクロールします。 長いデータの一覧をスクロールがある場合も設定し、`android:cacheColorHint`プロパティを`ListView`(ユーザー設定の行のレイアウトの背景と同じ色の値) に、アクティビティの AXML 定義で宣言します。 このヒントが含まれては、カスタム行の背景色の一覧をユーザーがスクロール 'ちらつき'、可能性があります。



## <a name="related-links"></a>関連リンク

- [BuiltInViews (サンプル)](https://developer.xamarin.com/samples/BuiltInViews/)
- [AccessoryViews (サンプル)](https://developer.xamarin.com/samples/AccessoryViews/)
- [CustomRowView (サンプル)](https://developer.xamarin.com/samples/CustomRowView/)
