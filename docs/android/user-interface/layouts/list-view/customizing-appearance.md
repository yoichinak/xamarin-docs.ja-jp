---
title: ListView の外観のカスタマイズ
ms.prod: xamarin
ms.assetid: B09AD282-2C4F-D71E-6806-9B1EF05C2CD4
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: a2487fd0f7d90b70ec0dc1fb1978ca06a3108822
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522607"
---
# <a name="customizing-a-listviews-appearance-with-xamarinandroid"></a>Xamarin Android を使用した ListView の外観のカスタマイズ

ListView の外観は、表示されている行のレイアウトによって決まります。 の外観を変更するに`ListView`は、別の行レイアウトを使用します。


## <a name="built-in-row-views"></a>組み込みの行ビュー

**Android. Resource. Layout**を使用して参照できる組み込みビューが12個あります。

- **Testlistitem**&ndash;書式設定が最小の1行のテキスト。

- **SimpleListItem1**&ndash; 1 行のテキスト。

- **SimpleListItem2**&ndash; 2 行のテキスト。

- **Simpleselectablelistitem**&ndash;単一または複数の項目の選択をサポートする1行のテキスト (API レベル11で追加)。

- **SimpleListItemActivated1**&ndash; SimpleListItem1 と同様ですが、背景色は、行が選択された (API レベル11で追加された) ことを示します。

- **SimpleListItemActivated2**&ndash; SimpleListItem2 と同様ですが、背景色は、行が選択された (API レベル11で追加された) ことを示します。

- **Simplelistitemchecked**&ndash;選択を示すためにチェックマークを表示します。

- **SimpleListItemMultipleChoice**&ndash;複数選択の選択を示すチェックボックスを表示します。

- **SimpleListItemSingleChoice**&ndash;相互排他的な選択を示すオプションボタンを表示します。

- **Twolinelistitem**&ndash; 2 行のテキスト。

- **Activitylistitem**&ndash;イメージを含む1行のテキスト。

- **Simpleexpandablelistitem**&ndash;カテゴリごとに行をグループ化し、各グループを展開または折りたたむことができます。

組み込みの各行ビューには、組み込みのスタイルが関連付けられています。 これらのスクリーンショットは、各ビューの表示方法を示しています。

[![TestListItem、SimpleSelectableListItem、SimpleListitem1、および SimpleListItem2 のスクリーンショット](customizing-appearance-images/builtinviews.png)](customizing-appearance-images/builtinviews.png#lightbox)

[![SimpleListItemActivated1、SimpleListItemActivated2、SimpleListItemChecked、および Simplelistitemchecked のスクリーンショット](customizing-appearance-images/builtinviews-2.png)](customizing-appearance-images/builtinviews-2.png#lightbox)

[![SimpleListItemSingleChoice、TwoLineListItem、ActivityListItem、および SimpleExpandableListItem のスクリーンショット](customizing-appearance-images/builtinviews-3.png)](customizing-appearance-images/builtinviews-3.png#lightbox)

**BuiltInViews/HomeScreenAdapter**サンプルファイル ( **BuiltInViews**ソリューション内) には、展開できないリスト項目画面を生成するコードが含まれています。 ビューは次のように`GetView`メソッドで設定されます。

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
```

その後、標準`Text1`コントロール識別子`Icon`を`Text2` `Android.Resource.Id`参照することによって、ビューのプロパティを設定できます。また、(ビューに含まれていないプロパティを設定しないと、例外がスローされます)。

```csharp
view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = item.Heading;
view.FindViewById<TextView>(Android.Resource.Id.Text2).Text = item.SubHeading;
view.FindViewById<ImageView>(Android.Resource.Id.Icon).SetImageResource(item.ImageResourceId); // only use with ActivityListItem
```

**BuiltInExpandableViews/ExpandableScreenAdapter**サンプルファイル ( **BuiltInViews**ソリューション内) には、simpleexpandablelistitem 画面を生成するためのコードが含まれています。 グループビューは、次のよう`GetGroupView`にメソッドで設定されます。

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem1, null);
```

子ビューは、次のよう`GetChildView`にメソッドで設定されます。

```csharp
view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleExpandableListItem2, null);
```

グループビューと子ビューのプロパティは、上記のように、標準`Text1` `Text2`識別子とコントロール識別子を参照することによって設定できます。 SimpleExpandableListItem スクリーンショット (上図参照) は、1行のグループビュー (SimpleExpandableListItem1) と2行の子ビュー (SimpleExpandableListItem2) の例を示しています。 または、2つの行 (SimpleExpandableListItem2) に対してグループビューを構成し、子ビューを1つの行 (SimpleExpandableListItem1) に対して構成することも、グループビューと子ビューの両方に同じ行数を設定することもできます。 



## <a name="accessories"></a>Accessories

行では、ビューの右側にアクセサリを追加して、選択状態を示すことができます。

- **Simplelistitemchecked**&ndash;インジケーターとしてチェックを使用して、単一選択リストを作成します。

- **SimpleListItemSingleChoice**&ndash;選択できるオプションが1つだけの場合に、ラジオボタンの種類の一覧を作成します。

- **SimpleListItemMultipleChoice**&ndash;複数の選択肢が可能な場合に、チェックボックスの種類の一覧を作成します。

前述のアクセサリは、次の画面にそれぞれの順序で示されています。

[![アクセサリーがある SimpleListItemChecked、SimpleListItemSingleChoice、および SimpleListItemMultipleChoice のスクリーンショット](customizing-appearance-images/accessories.png)](customizing-appearance-images/accessories.png#lightbox)

これらのアクセサリのいずれかを表示するには、必要なレイアウトリソース ID をアダプターに渡してから、必要な行の選択状態を手動で設定します。 次のコード行は、 `Adapter`これらのレイアウトのいずれかを使用してを作成し、割り当てる方法を示しています。

```csharp
ListAdapter = new ArrayAdapter<String>(this, Android.Resource.Layout.SimpleListItemChecked, items);
```

は`ListView` 、表示されているアクセサリに関係なく、さまざまな選択モードをサポートしています。 混乱を避けるには`Single` 、 `SingleChoice`アクセサリを使用し`Checked`て選択`Multiple`モードを、 `MultipleChoice`スタイルにはまたはモードを使用します。 選択モードは、 `ChoiceMode` `ListView`のプロパティによって制御されます。


### <a name="handling-api-level"></a>API レベルの処理

以前のバージョンの Xamarin. Android では、整数プロパティとして列挙が実装されていました。 最新バージョンでは、適切な .NET 列挙型が導入されました。これにより、潜在的なオプションをより簡単に検出できます。

ターゲットとする API レベルに応じて、 `ChoiceMode`は整数または列挙値のいずれかになります。 **AccessoryViews/HomeScreen**サンプルファイルには、ジンジャー API を対象とする場合に、コメントアウトされたブロックがあります。

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

' 選択済み ' 項目を手動で設定する方法は`SetItemChecked` 、メソッドを使用して実行します (複数選択に対して複数回呼び出すことができます)。

```csharp
// Set the initially checked row ("Fruits")
lv.SetItemChecked(1, true);
```

また、このコードでは、複数の選択とは異なる方法で単一選択を検出する必要があります。 モードで`Single`選択されている行を確認する`CheckedItemPosition`には、整数プロパティを使用します。

```csharp
FindViewById<ListView>(Android.Resource.Id.List).CheckedItemPosition
```

モードで`Multiple`選択されている行を確認するには、 `CheckedItemPositions` `SparseBooleanArray`をループ処理する必要があります。 スパース配列は、値が変更されたエントリのみを含むディクショナリに似ているため、次のコードスニペットに示す`true`ように、リストで選択されている内容を確認するために、配列全体を走査する必要があります。

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

- は、 `Activity`では`ListActivity`なく、から継承されます。 任意`ListView`のの行をカスタマイズできますが、他のコントロールを`Activity`レイアウト (見出し、ボタン、その他のユーザーインターフェイス要素など) に含めることもできます。 この例では、の上`ListView`に見出しを追加して説明します。

- 画面には、AXML レイアウトファイルが必要です。前の例では`ListActivity` 、にレイアウトファイルは必要ありません。 この axml には`ListView` 、コントロール宣言が含まれています。

- で各行を表示するには、AXML レイアウトファイルが必要です。 この AXML ファイルには、フォントと色のカスタム設定を持つテキストコントロールとイメージコントロールが含まれています。

- オプションのカスタムセレクター XML ファイルを使用して、選択した行の外観を設定します。

- 実装`Adapter`は、 `GetView`オーバーライドからカスタムレイアウトを返します。

- `ItemClick`を異なる方法で宣言する必要があります ( `ListView.ItemClick`のオーバーライド`OnListItemClick`で`ListActivity`はなく、にアタッチされるイベントハンドラーです)。


これらの変更については、以下で詳しく説明します。アクティビティのビューとカスタムの行ビューを作成してから、アダプターとアクティビティに加えられた変更について説明します。


### <a name="adding-a-listview-to-an-activity-layout"></a>アクティビティレイアウトへの ListView の追加

`HomeScreen` から`ListActivity`継承されなくなるため、既定のビューは存在しません。そのため、HomeScreen のビューに対してレイアウト axml ファイルを作成する必要があります。 この例では、ビューに見出し (を`TextView`使用) と、 `ListView`データを表示するを設定します。 レイアウトは、次に示す**Resources/layout/HomeScreen**ファイルで定義されています。

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

(では`ListActivity`なく) `Activity`カスタムレイアウトでを使用すると、この例の見出し`TextView`などのコントロールを画面に追加できるようになります。


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

カスタムアダプターの実装例は、「」 `HomeScreenAdapter.cs`にあります。 キーメソッドは`GetView` 、リソース ID `Resource.Layout.CustomView`を使用してカスタムの axml を読み込み、それを返す前にビュー内の各コントロールのプロパティを設定します。 完全なアダプタークラスが表示されます。

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

クラスは`HomeScreen`から`Activity`継承するようになっ`ListView`たため、クラスでは、axml で宣言されたコントロールへの参照を保持するフィールドが宣言されています。

```csharp
ListView listView;
```

クラスは、 `SetContentView`メソッドを使用して、アクティビティのカスタムレイアウト axml を読み込む必要があります。 次に、レイアウト内`ListView`のコントロールを見つけて、アダプターを作成して割り当て、クリックハンドラーを割り当てます。 OnCreate メソッドのコードを次に示します。

```csharp
SetContentView(Resource.Layout.HomeScreen); // loads the HomeScreen.axml as this activity's view
listView = FindViewById<ListView>(Resource.Id.List); // get reference to the ListView in the layout

// populate the listview with data
listView.Adapter = new HomeScreenAdapter(this, tableItems);
listView.ItemClick += OnListItemClick;  // to be defined
```

最後に`ItemClick` 、ハンドラーを定義する必要があります。この場合`Toast` 、メッセージが表示されるだけです。

```csharp
void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
   var listView = sender as ListView;
   var t = tableItems[e.Position];
   Android.Widget.Toast.MakeText(this, t.Heading, Android.Widget.ToastLength.Short).Show();
}
```

結果の画面は次のようになります。

[![結果の CustomRowView のスクリーンショット](customizing-appearance-images/customrowview.png)](customizing-appearance-images/customrowview.png#lightbox)



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

選択した行と対応`Toast`するメッセージは、次のようになります。

[![[選択した行の名前を表示するトーストメッセージを含むオレンジ色で選択された行]](customizing-appearance-images/customselectcolor.png)](customizing-appearance-images/customselectcolor.png#lightbox)



### <a name="preventing-flickering-on-custom-layouts"></a>カスタムレイアウトのちらつきを防止する

Android では、レイアウト情報を`ListView`キャッシュすることで、スクロールのパフォーマンスを向上させようとしています。 データのスクロールリストが長い場合は、アクティビティの axml `android:cacheColorHint`定義の`ListView`宣言のプロパティも設定する必要があります (カスタムの行レイアウトの背景と同じ色の値に設定します)。 このヒントを含めないと、ユーザーがカスタムの行背景色でリストをスクロールしたときに "ちらつき" が発生する可能性があります。



## <a name="related-links"></a>関連リンク

- [BuiltInViews (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/builtinviews)
- [AccessoryViews (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/accessoryviews)
- [CustomRowView (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/customrowview)
