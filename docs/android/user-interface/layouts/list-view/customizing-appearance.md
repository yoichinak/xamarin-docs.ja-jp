---
title: ListView の外観のカスタマイズ
ms.prod: xamarin
ms.assetid: B09AD282-2C4F-D71E-6806-9B1EF05C2CD4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: fef81fb5e5d2de79508b43a5612bf56af68d0772
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108172"
---
# <a name="customizing-a-listviews-appearance"></a>ListView の外観のカスタマイズ


## <a name="overview"></a>概要

ListView の外観は、表示されている行のレイアウトによって決まります。 外観を変更する、 `ListView`、別の行のレイアウトを使用します。


## <a name="built-in-row-views"></a>組み込みの行ビュー

使用して参照できる組み込みの 12 個のビューがある**Android.Resource.Layout**:

- **TestListItem** &ndash;最低限の書式のテキストの行を 1 。

- **SimpleListItem1** &ndash; 1 つの行のテキスト。

- **SimpleListItem2** &ndash; 2 行のテキスト。

- **SimpleSelectableListItem** &ndash; 1 つの行の 1 つまたは複数の項目の選択 (API レベル 11 で追加) をサポートするテキスト。

- **SimpleListItemActivated1** &ndash; SimpleListItem1 と似ていますが、背景色を示す行を選択すると (API レベル 11 で追加します)。

- **SimpleListItemActivated2** &ndash; SimpleListItem2 と似ていますが、背景色を示す行を選択すると (API レベル 11 で追加します)。

- **SimpleListItemChecked** &ndash;選択範囲を示すチェック マークが表示されます。

- **SimpleListItemMultipleChoice** &ndash;選択肢を指定するためのチェック ボックスが表示されます。

- **SimpleListItemSingleChoice** &ndash;表示のオプションを相互に排他的な選択範囲を示すボタン。

- **TwoLineListItem** &ndash; 2 行のテキスト。

- **ActivityListItem** &ndash;イメージのテキストの行を 1 。

- **SimpleExpandableListItem** &ndash;カテゴリ、および各グループでの行のグループを展開または折りたたむことができます。

各組み込み行ビューには、関連付けられている組み込みスタイルがあります。 これらのスクリーン ショットでは、それぞれのビューの表示方法を示します。

[![TestListItem、SimpleSelectableListItem、SimpleListitem1、および SimpleListItem2 のスクリーン ショット](customizing-appearance-images/builtinviews.png)](customizing-appearance-images/builtinviews.png#lightbox)

[![SimpleListItemActivated1、SimpleListItemActivated2、SimpleListItemChecked、および SimpleListItemMultipleChecked のスクリーン ショット](customizing-appearance-images/builtinviews-2.png)](customizing-appearance-images/builtinviews-2.png#lightbox)

[![SimpleListItemSingleChoice、TwoLineListItem、ActivityListItem、および SimpleExpandableListItem のスクリーン ショット](customizing-appearance-images/builtinviews-3.png)](customizing-appearance-images/builtinviews-3.png#lightbox)

**BuiltInViews/HomeScreenAdapter.cs**サンプル ファイル (で、 **BuiltInViews**ソリューション) の展開を可能なリスト項目の画面を生成するためにコードが含まれています。 ビューが設定されている、`GetView`このようなメソッド。

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
```

標準コントロールの識別子を参照することで、ビューのプロパティを設定できます`Text1`、`Text2`と`Icon` `Android.Resource.Id` (ビューは含まれていないか、例外がスローされるプロパティを設定しないでください)。

```csharp
view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = item.Heading;
view.FindViewById<TextView>(Android.Resource.Id.Text2).Text = item.SubHeading;
view.FindViewById<ImageView>(Android.Resource.Id.Icon).SetImageResource(item.ImageResourceId); // only use with ActivityListItem
```

**BuiltInExpandableViews/ExpandableScreenAdapter.cs**サンプル ファイル (で、 **BuiltInViews**ソリューション) SimpleExpandableListItem 画面を生成するためにコードが含まれています。 グループ ビューが 設定されている、`GetGroupView`このようなメソッド。

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem1, null);
```

子ビューが 設定されている、`GetChildView`このようなメソッド。

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem2, null);
```

標準を参照することで、グループ ビューと、子ビューのプロパティを設定しできます`Text1`と`Text2`上記のように、識別子を制御します。 (上記) SimpleExpandableListItem スクリーン ショットでは、1 行のグループ ビュー (SimpleExpandableListItem1) と 2 行の子ビュー (SimpleExpandableListItem2) の例を示します。 または、(SimpleExpandableListItem2) の 2 行のグループ ビューを構成することができます (SimpleExpandableListItem1) が 1 行、子ビューを構成することができますまたは両方のグループ化ビューと子ビューは、同じ数の行を持つことができます。 



## <a name="accessories"></a>Accessories

行には、[アクセサリ] の選択状態を示すため、ビューの右側に追加されたことがあります。

- **SimpleListItemChecked** &ndash;インジケーターとしてチェックで単一選択リストを作成します。

- **SimpleListItemSingleChoice** &ndash; 1 つだけ選択が可能なラジオ ボタン-型のリストを作成します。

- **SimpleListItemMultipleChoice** &ndash;複数の選択肢が考えられる checkbox 型のリストを作成します。

それぞれの順序で、次の画面は、前述の [アクセサリ] を示したです。

[![スクリーン ショットの SimpleListItemChecked、SimpleListItemSingleChoice、およびアクセサリと SimpleListItemMultipleChoice](customizing-appearance-images/accessories.png)](customizing-appearance-images/accessories.png#lightbox)

これらのアクセサリ パスのいずれかを表示するには、アダプターに必要なレイアウトのリソース ID、手動で設定、必要な行の選択状態。 次のコードを作成して割り当てる方法を示しています、`Adapter`これらのレイアウトのいずれかを使用します。

```csharp
ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItemChecked, items);
```

`ListView`自体が表示されている付属品に関係なく、別の選択モードをサポートしています。 混乱を避けるためには、次のように使用します。`Single`選択モードと`SingleChoice`[アクセサリ]、`Checked`または`Multiple`モードと、`MultipleChoice`スタイル。 選択モードがによって制御される、`ChoiceMode`のプロパティ、`ListView`します。


### <a name="handling-api-level"></a>API レベルの処理

以前のバージョンの Xamarin.Android では、整数のプロパティとして列挙型を実装します。 最新バージョンは、適切な .NET 列挙型をこれにより、潜在的なオプションを検出するはるかに簡単導入にされています。

API レベルによっては、ターゲット`ChoiceMode`は整数または列挙体。 サンプル ファイル**AccessoryViews/HomeScreen.cs**がブロックをコメント アウト Gingerbread API を対象にする場合。

```csharp
// For targeting Gingerbread the ChoiceMode is an int, otherwise it is an
// enumeration.

lv.ChoiceMode = Android.Widget.ChoiceMode.Single; // 1
//lv.ChoiceMode = Android.Widget.ChoiceMode.Multiple; // 2
//lv.ChoiceMode = Android.Widget.ChoiceMode.None; // 0

// Use this block if targeting Gingerbread or lower
/*
lv.ChoiceMode = 1; // Single
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

コードは、異なる方法で複数の選択肢から 1 項目の選択を検出するためにも必要です。 行が選択されているかを判断する`Single`モードの使用、`CheckedItemPosition`整数のプロパティ。

```csharp
FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPosition
```

選択した行を決定する`Multiple`をループ処理する必要があるモード、 `CheckedItemPositions` `SparseBooleanArray`します。 スパース配列はエントリのみを含むディクショナリのように、値が変更されて探して配列全体を走査する必要がありますので`true`を知ると、次のコード スニペットに示すように、一覧で選択されている値。

```csharp
var sparseArray = FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPositions;
for (var i = 0; i < sparseArray.Size(); i++ )
{
   Console.Write(sparseArray.KeyAt(i) + "=" + sparseArray.ValueAt(i) + ",");
}
Console.WriteLine();
```


## <a name="creating-custom-row-layouts"></a>カスタムの行のレイアウトを作成します。

4 つの組み込みの行ビューでは、非常に単純です。 (電子メール、または、ツイートの連絡先情報の一覧) などのより複雑なレイアウトを表示するには、カスタム ビューが必要です。 カスタム ビューは通常、AXML ファイルでとして宣言されている、**リソース/レイアウト**ディレクトリにし、リソース Id、カスタム アダプターを使用して、読み込まれました。 ビューは、カスタムの色、フォントおよびレイアウトで表示クラス (するテキスト ビュー、ImageViews 他のコントロールなど) の任意の数を含めることができます。

この例は、さまざまな方法で、前の例とは異なります。

-  継承`Activity`ではなく、`ListActivity`します。 いずれかの行をカスタマイズすることができます`ListView`で他のコントロールが含めることもできますが、 `Activity` (見出し、ボタンやその他のユーザー インターフェイス要素) などのレイアウト。 この例では、上記の見出しを追加します。、`ListView`について説明します。

-  画面の AXML レイアウト ファイルが必要です前の例で、`ListActivity`レイアウト ファイルは必要ありません。 この AXML を含む、`ListView`宣言を制御します。

-  各の行を表示するために、AXML レイアウト ファイルが必要です。 この AXML ファイルには、カスタムのフォントおよび色の設定でテキストとイメージ コントロールが含まれています。

-  省略可能なカスタム セレクターの XML ファイルを使用すると、選択されている行の外観を設定します。

-  `Adapter`実装からのカスタム レイアウトを返します、`GetView`をオーバーライドします。

-  `ItemClick` 異なる方法で宣言する必要があります (にイベント ハンドラーがアタッチされている`ListView.ItemClick`オーバーライドするのではなく`OnListItemClick`で`ListActivity`)。


これらの変更は、詳細な以降では、アクティビティのビューとカスタム行ビューを作成して、それらを表示するために、アダプターとアクティビティへの変更について説明します。


### <a name="adding-a-listview-to-an-activity-layout"></a>アクティビティのレイアウトを ListView を追加します。

`HomeScreen`から継承しなく`ListActivity`ホーム スクリーンの表示レイアウト AXML ファイルを作成する必要がありますので、既定のビューがないです。 この例で、ビュー、見出しがある (を使用して、 `TextView`) と`ListView`データを表示します。 レイアウトが定義されている、 **Resources/Layout/HomeScreen.axml**ファイルを次に示します。

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

使用するメリット、`Activity`カスタム レイアウトを持つ (の代わりに、 `ListActivity`)、見出しなどの画面に追加のコントロールを追加できることにあります`TextView`この例では。


### <a name="creating-a-custom-row-layout"></a>カスタムの行のレイアウトの作成

リスト ビューに表示される各の行のカスタム レイアウトを格納する別の AXML レイアウト ファイルが必要です。 この例では、行は、緑の背景、茶色のテキストおよびイメージを右揃えがあります。 このレイアウトを宣言する Android XML マークアップについては、「 **Resources/Layout/CustomView.axml**:

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

カスタムの行のレイアウトは、多くのさまざまなコントロールを含めることができます、スクロールのパフォーマンスが複雑なデザインが適用され、(特に、ネットワーク経由で読み込まれる必要がある) 場合にイメージを使用します。 スクロールのパフォーマンスの問題に対処の詳細については、Google の記事を参照してください。


### <a name="referencing-a-custom-row-view"></a>行のカスタム ビューを参照します。

例では、カスタム アダプターの実装は`HomeScreenAdapter.cs`します。 重要なメソッドは`GetView`リソース ID を使用してカスタム AXML を読み込み、 `Resource.Layout.CustomView`、および返す前に、ビュー内のコントロールの各プロパティを設定します。 完全なアダプターのクラスが表示されます。

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


### <a name="referencing-the-custom-listview-in-the-activity"></a>アクティビティで、カスタム ListView を参照します。

`HomeScreen`クラスを今すぐ継承`Activity`、`ListView`フィールドは、AXML で宣言されたコントロールへの参照を保持するクラスで宣言されています。

```csharp
ListView listView;
```

クラスは、アクティビティのカスタム レイアウト AXML を読み込む必要がありますしを使用して、`SetContentView`メソッド。 次を検索できる、`ListView`レイアウト内のコントロールと、アダプターに割り当てられ、作成クリック ハンドラーを割り当てます。 OnCreate メソッドのコードを次に示します。

```csharp
SetContentView(Resource.Layout.HomeScreen); // loads the HomeScreen.axml as this activity's view
listView = FindViewById<ListView>(Resource.Id.List); // get reference to the ListView in the layout

// populate the listview with data
listView.Adapter = new HomeScreenAdapter(this, tableItems);
listView.ItemClick += OnListItemClick;  // to be defined
```

最後に、`ItemClick`ハンドラーを定義する必要がありますが、単に表示されますこの場合、`Toast`メッセージ。

```csharp
void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
   var listView = sender as ListView;
   var t = tableItems[e.Position];
   Android.Widget.Toast.MakeText(this, t.Heading, Android.Widget.ToastLength.Short).Show();
}
```

表示された画面のようになります。

[![結果として得られる CustomRowView のスクリーン ショット](customizing-appearance-images/customrowview.png)](customizing-appearance-images/customrowview.png#lightbox)



### <a name="customizing-the-row-selector-color"></a>行セレクターの色をカスタマイズします。

行が処理されたときに、ユーザーからのフィードバックの強調表示する必要があります。 カスタム ビューの背景色としてを指定した場合**CustomView.axml**もオーバーライド選択の強調表示します。 次のコードで**CustomView.axml**セットの行が処理されたときに、視覚的なインジケーターがないが明るい緑は、バック グラウンドも意味します。

```xml
android:background="#FFDAFF7F"
```

強調表示の動作を再度有効にするまた、使用される色をカスタマイズする、代わりにカスタム セレクターに背景属性を設定します。 セレクターは、強調表示色だけでなく、既定の背景色を宣言します。 ファイル**Resources/Drawable/CustomSelector.xml**次の宣言が含まれています。

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

選択した行と、対応する`Toast`ここで次のようにメッセージします。

[![トースト メッセージが選択されている行の名前を表示するのには、オレンジ色で選択した行](customizing-appearance-images/customselectcolor.png)](customizing-appearance-images/customselectcolor.png#lightbox)



### <a name="preventing-flickering-on-custom-layouts"></a>カスタム レイアウトでちらつきの防止

Android のパフォーマンスを改善しようとしました。`ListView`レイアウト情報をキャッシュすることによってスクロールします。 設定する必要がある時間の長いデータのリストをスクロールしていれば、`android:cacheColorHint`プロパティを`ListView`(行のカスタム レイアウトの背景として同じ色の値) に、アクティビティの AXML 定義で宣言します。 このヒントを含めるに失敗すると、カスタム行の背景色の一覧をユーザーがスクロール 'ちらつき' する可能性がありますが、



## <a name="related-links"></a>関連リンク

- [BuiltInViews (サンプル)](https://developer.xamarin.com/samples/BuiltInViews/)
- [AccessoryViews (サンプル)](https://developer.xamarin.com/samples/AccessoryViews/)
- [CustomRowView (サンプル)](https://developer.xamarin.com/samples/CustomRowView/)
