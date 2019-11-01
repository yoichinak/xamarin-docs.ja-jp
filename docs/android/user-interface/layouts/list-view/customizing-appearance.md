---
title: ListView の外観のカスタマイズ
ms.prod: xamarin
ms.assetid: B09AD282-2C4F-D71E-6806-9B1EF05C2CD4
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/26/2018
ms.openlocfilehash: 48b23a1dce66f13efd3ad598cd61684e64e2b03c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028899"
---
# <a name="customizing-a-listviews-appearance-with-xamarinandroid"></a>Xamarin Android を使用した ListView の外観のカスタマイズ

ListView の外観は、表示されている行のレイアウトによって決まります。 `ListView`の外観を変更するには、別の行レイアウトを使用します。

## <a name="built-in-row-views"></a>組み込みの行ビュー

**Android. Resource. Layout**を使用して参照できる組み込みビューが12個あります。

- **Testlistitem**は、最小限の書式でテキストの単一行を &ndash; します。

- **SimpleListItem1** &ndash; 1 行のテキストです。

- **SimpleListItem2** &ndash; 2 行のテキストです。

- **Simpleselectablelistitem**は、1つまたは複数の項目の選択 (API レベル11で追加) をサポートする1行のテキスト &ndash; ます。

- **SimpleListItemActivated1** &ndash; SimpleListItem1 に似ていますが、背景色は、行が選択された (API レベル11で追加された) ことを示します。

- **SimpleListItemActivated2** &ndash; SimpleListItem2 に似ていますが、背景色は、行が選択された (API レベル11で追加された) ことを示します。

- **Simplelistitemchecked** &ndash; では、選択を示すチェックマークが表示されます。

- **SimpleListItemMultipleChoice** &ndash; では、複数選択の選択肢を示すチェックボックスが表示されます。

- **SimpleListItemSingleChoice** &ndash; では、相互に排他的な選択を示すオプションボタンが表示されます。

- **Twolinelistitem** &ndash; 2 行のテキスト。

- **Activitylistitem** &ndash; 画像を含む1行のテキストです。

- **Simpleexpandablelistitem** &ndash; カテゴリ別に行をグループ化し、各グループを展開または折りたたむことができます。

組み込みの各行ビューには、組み込みのスタイルが関連付けられています。 これらのスクリーンショットは、各ビューの表示方法を示しています。

[TestListItem、SimpleSelectableListItem、SimpleListitem1、および SimpleListItem2 のスクリーンショットを![します。](customizing-appearance-images/builtinviews.png)](customizing-appearance-images/builtinviews.png#lightbox)

[SimpleListItemActivated1、SimpleListItemActivated2、SimpleListItemChecked、および Simplelistitemchecked のスクリーンショットの![](customizing-appearance-images/builtinviews-2.png)](customizing-appearance-images/builtinviews-2.png#lightbox)

[SimpleListItemSingleChoice、TwoLineListItem、ActivityListItem、および SimpleExpandableListItem の![スクリーンショット](customizing-appearance-images/builtinviews-3.png)](customizing-appearance-images/builtinviews-3.png#lightbox)

**BuiltInViews/HomeScreenAdapter**サンプルファイル ( **BuiltInViews**ソリューション内) には、展開できないリスト項目画面を生成するコードが含まれています。 ビューは、次のように `GetView` メソッドで設定されます。

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
```

その後、ビューのプロパティを設定するには、標準コントロール識別子 `Text1`、`Text2`、および `Android.Resource.Id` の下にある `Icon` を参照します (ビューに含まれていないプロパティを設定しないか、例外がスローされます)。

```csharp
view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = item.Heading;
view.FindViewById<TextView>(Android.Resource.Id.Text2).Text = item.SubHeading;
view.FindViewById<ImageView>(Android.Resource.Id.Icon).SetImageResource(item.ImageResourceId); // only use with ActivityListItem
```

**BuiltInExpandableViews/ExpandableScreenAdapter**サンプルファイル ( **BuiltInViews**ソリューション内) には、simpleexpandablelistitem 画面を生成するためのコードが含まれています。 グループビューは、次のように `GetGroupView` メソッドで設定されます。

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem1, null);
```

子ビューは、次のように `GetChildView` メソッドで設定されます。

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem2, null);
```

グループビューと子ビューのプロパティは、次に示すように、標準の `Text1` と `Text2` 制御識別子を参照することによって設定できます。 SimpleExpandableListItem スクリーンショット (上図参照) は、1行のグループビュー (SimpleExpandableListItem1) と2行の子ビュー (SimpleExpandableListItem2) の例を示しています。 または、2つの行 (SimpleExpandableListItem2) に対してグループビューを構成し、子ビューを1つの行 (SimpleExpandableListItem1) に対して構成することも、グループビューと子ビューの両方に同じ行数を設定することもできます。 

## <a name="accessories"></a>[アクセサリ]

行では、ビューの右側にアクセサリを追加して、選択状態を示すことができます。

- **Simplelistitemchecked** &ndash; は、インジケーターとしてチェックを含む単一選択リストを作成します。

- **SimpleListItemSingleChoice** &ndash; では、選択できるオプションが1つだけである、ラジオボタンの種類の一覧が作成されます。

- **SimpleListItemMultipleChoice** &ndash; では、複数の選択肢が可能な場合にチェックボックスの種類の一覧が作成されます。

前述のアクセサリは、次の画面にそれぞれの順序で示されています。

[SimpleListItemSingleChoice と SimpleListItemMultipleChoice のアクセサリを使用して、SimpleListItemChecked、、およびのスクリーンショットを![します](customizing-appearance-images/accessories.png)](customizing-appearance-images/accessories.png#lightbox)

これらのアクセサリのいずれかを表示するには、必要なレイアウトリソース ID をアダプターに渡してから、必要な行の選択状態を手動で設定します。 次のコード行は、これらのレイアウトのいずれかを使用して `Adapter` を作成して割り当てる方法を示しています。

```csharp
ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItemChecked, items);
```

`ListView` 自体は、表示されているアクセサリに関係なく、さまざまな選択モードをサポートしています。 混乱を避けるために、`SingleChoice` のアクセサリ、`Checked` または `Multiple` モードと `MultipleChoice` スタイルを使用して `Single` 選択モードを使用します。 選択モードは、`ListView`の `ChoiceMode` プロパティによって制御されます。

### <a name="handling-api-level"></a>API レベルの処理

以前のバージョンの Xamarin. Android では、整数プロパティとして列挙が実装されていました。 最新バージョンでは、適切な .NET 列挙型が導入されました。これにより、潜在的なオプションをより簡単に検出できます。

ターゲットとする API レベルに応じて、`ChoiceMode` は整数または列挙値になります。 **AccessoryViews/HomeScreen**サンプルファイルには、ジンジャー API を対象とする場合に、コメントアウトされたブロックがあります。

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

### <a name="selecting-items-programmatically"></a>プログラムによる項目の選択

' 選択済み ' の項目を手動で設定するには、`SetItemChecked` メソッドを使用します (複数選択に対して複数回呼び出すことができます)。

```csharp
// Set the initially checked row ("Fruits")
lv.SetItemChecked(1, true);
```

また、このコードでは、複数の選択とは異なる方法で単一選択を検出する必要があります。 `Single` モードで選択されている行を確認するには、`CheckedItemPosition` 整数のプロパティを使用します。

```csharp
FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPosition
```

`Multiple` モードで選択されている行を確認するには、`CheckedItemPositions` `SparseBooleanArray`をループする必要があります。 スパース配列は、値が変更されたエントリのみを含むディクショナリに似ているため、次のコードスニペットに示すように、配列全体を走査して `true` 値を探し、一覧で選択されている内容を確認する必要があります。:

```csharp
var sparseArray = FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPositions;
for (var i = 0; i < sparseArray.Size(); i++ )
{
   Console.Write(sparseArray.KeyAt(i) + "=" + sparseArray.ValueAt(i) + ",");
}
Console.WriteLine();
```

## <a name="creating-custom-row-layouts"></a>カスタム行レイアウトの作成

4つの組み込みの行ビューは非常に単純です。 より複雑なレイアウト (メールのリスト、ツイート、連絡先情報など) を表示するには、カスタムビューが必要です。 カスタムビューは一般に、 **Resources/Layout**ディレクトリでは axml ファイルとして宣言され、その後、カスタムアダプターによってリソース Id を使用して読み込まれます。 ビューには、任意の数の表示クラス (TextViews、ImageViews、その他のコントロールなど) を、カスタムの色、フォント、およびレイアウトで含めることができます。

この例は、次のいくつかの方法で前の例と異なります。

- は、`ListActivity` ではなく `Activity` から継承されます。 任意の `ListView` に対して行をカスタマイズできますが、他のコントロールを `Activity` レイアウト (見出し、ボタン、その他のユーザーインターフェイス要素など) に含めることもできます。 この例では、`ListView` の上に見出しを追加して説明します。

- 画面には、AXML レイアウトファイルが必要です。前の例では、`ListActivity` はレイアウトファイルを必要としません。 この AXML には、`ListView` コントロールの宣言が含まれています。

- で各行を表示するには、AXML レイアウトファイルが必要です。 この AXML ファイルには、フォントと色のカスタム設定を持つテキストコントロールとイメージコントロールが含まれています。

- オプションのカスタムセレクター XML ファイルを使用して、選択した行の外観を設定します。

- `Adapter` 実装は、`GetView` オーバーライドからカスタムレイアウトを返します。

- `ItemClick` は異なる方法で宣言する必要があります (`ListActivity`でオーバーライドする `OnListItemClick` ではなく `ListView.ItemClick` にイベントハンドラーがアタッチされます)。

これらの変更については、以下で詳しく説明します。アクティビティのビューとカスタムの行ビューを作成してから、アダプターとアクティビティに加えられた変更について説明します。

### <a name="adding-a-listview-to-an-activity-layout"></a>アクティビティレイアウトへの ListView の追加

`HomeScreen` は `ListActivity` から継承されないため、既定のビューがないため、HomeScreen のビューに対してレイアウト AXML ファイルを作成する必要があります。 この例では、ビューに見出し (`TextView`を使用) と、データを表示するための `ListView` があります。 レイアウトは、次に示す**Resources/layout/HomeScreen**ファイルで定義されています。

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

(`ListActivity`ではなく) カスタムレイアウトで `Activity` を使用する利点は、この例の見出し `TextView` など、他のコントロールを画面に追加できることです。

### <a name="creating-a-custom-row-layout"></a>カスタムの行レイアウトの作成

別の AXML レイアウトファイルには、リストビューに表示される各行のカスタムレイアウトが含まれている必要があります。 この例では、行の背景が緑色、茶色のテキスト、右に固定されています。 このレイアウトを宣言するための Android XML マークアップは、 **Resources/layout/CustomView. axml**に記述されています。

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

カスタムの行レイアウトにはさまざまなコントロールを含めることができますが、スクロールのパフォーマンスは複雑なデザインやイメージの使用によって影響を受ける可能性があります (特にネットワーク経由で読み込む必要がある場合)。 スクロールのパフォーマンスの問題に対処する方法の詳細については、Google の記事を参照してください。

### <a name="referencing-a-custom-row-view"></a>カスタム行ビューの参照

カスタムアダプターの実装例は、`HomeScreenAdapter.cs`にあります。 キーメソッドは、リソース ID `Resource.Layout.CustomView`を使用してカスタム AXML を読み込む場所を `GetView` し、ビュー内の各コントロールにプロパティを設定してから返すようにします。 完全なアダプタークラスが表示されます。

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

### <a name="referencing-the-custom-listview-in-the-activity"></a>アクティビティでのカスタム ListView の参照

`HomeScreen` クラスは `Activity`から継承されるようになったため、クラスでは、AXML で宣言されたコントロールへの参照を保持するために `ListView` フィールドが宣言されています。

```csharp
ListView listView;
```

クラスは、`SetContentView` メソッドを使用して、アクティビティのカスタムレイアウト AXML を読み込む必要があります。 次に、レイアウト内の `ListView` コントロールを見つけて、アダプターを作成して割り当て、クリックハンドラーを割り当てます。 OnCreate メソッドのコードを次に示します。

```csharp
SetContentView(Resource.Layout.HomeScreen); // loads the HomeScreen.axml as this activity's view
listView = FindViewById<ListView>(Resource.Id.List); // get reference to the ListView in the layout

// populate the listview with data
listView.Adapter = new HomeScreenAdapter(this, tableItems);
listView.ItemClick += OnListItemClick;  // to be defined
```

最後に、`ItemClick` ハンドラーを定義する必要があります。この場合、`Toast` メッセージが表示されます。

```csharp
void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
   var listView = sender as ListView;
   var t = tableItems[e.Position];
   Android.Widget.Toast.MakeText(this, t.Heading, Android.Widget.ToastLength.Short).Show();
}
```

結果の画面は次のようになります。

[結果の CustomRowView の![スクリーンショット](customizing-appearance-images/customrowview.png)](customizing-appearance-images/customrowview.png#lightbox)

### <a name="customizing-the-row-selector-color"></a>行セレクターの色のカスタマイズ

行に触れると、ユーザーからのフィードバックのために強調表示されます。 カスタムビューで**Customview**として背景色が指定されている場合、これも選択範囲の強調表示をオーバーライドします。 **Customview**のこのコード行では、背景が明るい緑に設定されていますが、これは、行がタッチされても視覚インジケーターがないことを意味します。

```xml
android:background="#FFDAFF7F"
```

強調表示動作を再度有効にしたり、使用する色をカスタマイズしたりするには、代わりに background 属性をカスタムセレクターに設定します。 セレクターは、既定の背景色と強調表示色の両方を宣言します。 File **Resources/アブル/CustomSelector .xml**には、次の宣言が含まれています。

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

カスタムセレクターを参照するには、 **Customview. axml**の background 属性を次のように変更します。

```xml
android:background="@drawable/CustomSelector"
```

選択した行とそれに対応する `Toast` メッセージは次のようになります。

[選択した行をオレンジ色で![します。トーストメッセージには、選択した行の名前が表示されます](customizing-appearance-images/customselectcolor.png)](customizing-appearance-images/customselectcolor.png#lightbox)

### <a name="preventing-flickering-on-custom-layouts"></a>カスタムレイアウトのちらつきを防止する

Android では、レイアウト情報をキャッシュすることによって `ListView` スクロールのパフォーマンスを向上させようとしています。 データのスクロールリストが長い場合は、アクティビティの AXML 定義の `ListView` 宣言の `android:cacheColorHint` プロパティも設定する必要があります (カスタムの行レイアウトの背景と同じ色の値に設定します)。 このヒントを含めないと、ユーザーがカスタムの行背景色でリストをスクロールしたときに "ちらつき" が発生する可能性があります。

## <a name="related-links"></a>関連リンク

- [BuiltInViews (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/builtinviews)
- [AccessoryViews (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/accessoryviews)
- [CustomRowView (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/customrowview)
