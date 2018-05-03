---
title: ListView のパフォーマンス
description: "ListView を基本としたアプリで優れたパフォーマンスを確保する"
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: dcd4881e2ad7f1bb4af5455805da1dd2cade3605
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="listview-performance"></a>ListView のパフォーマンス

モバイルアプリケーションの作成は、パフォーマンスが重要です。ユーザは、スムーズなスクロールと高速ロード時間を期待するようになっています。ユーザの期待に応え損なうと、ストアでのアプリケーションの評価が下がり、業務アプリの場合であれば、組織の時間と費用も無駄になります。

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) はデータ表示の強力な view ですが、制限がいくつかあります。深い入れ子になった階層の view を含む場合や、多くの測定値を必要とするレイアウトを使用する場合など、カスタムセルを使う場合は特に、スクロールパフォーマンスに影響が出ます。しかし、パフォーマンスの低下を避けるために使用できるテクニックがあります。

この記事では、次のトピックについて説明します。

- **[Caching Strategy](#cachingstrategy)**
- **[ListView のパフォーマンスの向上](#improving-performance)**

<a name="cachingstrategy" />

## <a name="caching-strategy"></a>Caching Strategy

ListView は画面サイズ以上の大量のデータを表示するためによく使われます。 例えば音楽アプリを考えてみてください。音楽ライブラリは何千もの曲を持つかもしれません。簡単なアプローチでは、曲ごとに1つの行を生成することになり、パフォーマンスを低下させるでしょう。そのアプローチでは貴重なメモリを浪費し、画面のスクロールを遅くする可能性があります。別のアプローチは表示領域にデータがスクロールされた時に行の生成と破棄をすることです。この方法は絶えず表示オブジェクトのインスタンス化とクリーンアップを要求し、その処理はとても重くなる可能性があります。

メモリーを節約するために、各プラットフォームのネイティブの [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) に相当するものには行を再利用するための組み込みの機能があります。画面上に見えているセルだけがメモリーにロードされ、**コンテンツ**がそのセルに読み込まれます。 この機能によってアプリケーションは何千ものオブジェクトのインスタンス化の要求を防ぐことができ、時間とメモリーを節約できます。

Xamarin.Forms [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) 列挙型を使って、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) のセルの再利用を可能にしています。それには以下のような値があります。

```csharp
public enum ListViewCachingStrategy
{
  RetainElement,   // the default value
  RecycleElement,
  RecycleElementAndDataTemplate
}
```

> [!NOTE]
> Universal Windows Platform (UWP) では常にパフォーマンス向上のためにキャッシュ機能を使うため、 Caching Strategy の [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) は無視されます。そのため、デフォルトで、 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) が適用されているような動作をします。

### <a name="retainelement"></a>RetainElement

[ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) の caching strategy は、[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) がリストの各項目ごとに1つのセルを生成することを指定します。これは `ListView` の規定の動作です。一般的には次のような状況での使用が推奨されます。

- 各セルが多数の binding を持つ場合。 (20〜30以上)
- セルの template が頻繁に変更される場合。
- テストによって `RecycleElement` が実行速度を下げる結果になることが判明した場合。

カスタムセルを使って動かす場合は、 [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/) での結果を認識することが大切です。どんなセルの初期化コードでも各セルの生成ごとに走らせる必要があり、そしてそれは1秒間に複数回になるかもしれません。その状況では、ページでの凝った Layout テクニック（複数のネストされた[ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) を使っているような場合）は、それらがスクロールの際にリアルタイムで生成され破棄されるときに、パフォーマンスのボトルネックになります。

<a name="recycleelement" />

### <a name="recycleelement"></a>RecycleElement

[ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) の caching strategy を指定すると、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) はリストのセルを再利用することによって、メモリ使用量と実行速度を最小にすることを試みます。このモードは常にパフォーマンスの向上を提供するわけではなく、どのような改善があるかを判断するためにはテストを実施する必要があります。しかし、これは一般的に好ましい選択であり、次のような状況での使用が推奨されます。

- 各セルの binding の数が小〜中程度の場合。
- 各セルの [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) が全てのセルのデータを定義している場合。
- 各セルがほぼ同じで、セルの template が変化しない場合。

仮想化の間はセルは自身の binding context が更新されるので、アプリがこのモードを使う場合は binding context の更新が適切に処理されることを保証しなければなりません。セルに関するすべてのデータはその binding context に由来するものでなければなりません。そうしないと整合性エラーが発生する可能性があります。この処理は、セルのデータの表示に data binding を使うことによって実現できます。あるいは、以下のコードサンプルで示すように、セルのデータをカスタムセルのコンストラクタの中ではなく、 `OnBindingContextChanged` の override の中でセットするようにしても良いでしょう。

```csharp
public class CustomCell : ViewCell
{
  Image image = null;

  public CustomCell ()
  {
    image = new Image();
    View = image;
  }

  protected override void OnBindingContextChanged ()
  {
    base.OnBindingContextChanged ();

    var item = BindingContext as ImageItem;
    if (item != null) {
      image.Source = item.ImageUrl;
    }
  }
}
```

詳細については、[Binding Context Changes](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes) を参照してください。

iOS と Android で、セルに CustomRenderer を使用している場合は、プロパティ更新通知が正しく実装されていることを保証しなくてはなりません。セルが再利用されるとき、プロパティの値は、 binding context が次の有効なセルのものに更新されるときに `PropertyChanged` イベントの発生をともなって変更されます。より詳しい情報は [Customizing a ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md) を参照してください。

#### <a name="recycleelement-with-a-datatemplateselector"></a>DataTemplateSelector を使った RecycleElement

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) が [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) を選択するために [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) を使用している場合、[ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) の caching strategy は `DataTemplate` をキャッシュしません。その代わり、 `DataTemplate` はリストのデータの項目ごとに選択されます。

> [!NOTE]
> [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) caching strategy には Xamarin.Forms 2.4 で導入された前提条件があります。それは [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) が [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) を選択することを要求されると、それぞれの `DataTemplate` は同じ [ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/) の型を返さなければならないということです。例えば、 `MyDataTemplateA` （ `MyViewCellA` という型の `ViewCell` を返す） または `MyDataTemplateB` （ `MyViewCellB` という型の `ViewCell` を返す） のどちらも返す可能性がある `DataTemplateSelector` を持つ [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) があった場合、 `MyDataTemplateA` が返されたときは `MyViewCellA` が返りますが、それ以外は例外がスローされるようになります。

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

[ `RecycleElementAndDataTemplate` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate/) の caching strategy は [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/) を拡張したもので、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) が [`DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) を使う場合に、 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/) をリスト内の項目の型ごとにキャッシュする機能を加えたものです。これにより `DataTemplate` は、項目のインスタンスごとに1つではなく、項目の型ごとに1つ選択されるようになります。

> [!NOTE]
> [ `RecycleElementAndDataTemplate` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate/) は前提条件があり、 [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/) によって返される `DataTemplate` は `Type` 型 を引数にとる [ `DataTemplate` ](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/) のコンストラクタを使わなければなりません。

### <a name="setting-the-caching-strategy"></a>Caching strategy の設定

[ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) 列挙型の値は、次のコード例で示すように、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) のコンストラクタのオーバーロードで指定します。

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

XAML では、以下のコードで示すように、 `CachingStrategy` 属性をセットします。

```xaml
<ListView CachingStrategy="RecycleElement">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
              ...
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

これは C# でコンストラクタに、caching strategy の引数を設定するのと同じ効果があります。ただし、 `ListView` には `CachingStrategy` というプロパティは存在しないことに注意してください。

#### <a name="setting-the-caching-strategy-in-a-subclassed-listview"></a>サブクラス化された ListView での Caching Strategy の設定

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) のサブクラスでは、 XAML での `CachingStrategy` 属性の設定は、期待した動作は起こりません。 `ListView` には `CachingStrategy` というプロパティは存在しないからです。さらに、 [XAMLC](~/xamarin-forms/xaml/xamlc.md) が有効であれば、次のようなエラーメッセージが表示されるでしょう。
**No property, bindable property, or event found for 'CachingStrategy'**

この問題を解決するには、 [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) パラメータを受け取る [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) のサブクラスのコンストラクタを作成し、それを基本クラスに渡します。

```csharp
public class CustomListView : ListView
{
  public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
  {
  }
  ...
}
```

そして [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) 列挙型の値は、 XAML から `x:Arguments` 構文を使って指定できます。

```xaml
<local:CustomListView>
  <x:Arguments>
    <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
  </x:Arguments>
</local:CustomListView>
```

<a name="improving-performance" />

## <a name="improving-listview-performance"></a>ListView のパフォーマンスの向上

`ListView` のパフォーマンスを向上させるための多くのテクニックがあります。

-  `IEnumerable<T>` コレクションは、ランダムアクセスをサポートしないため、 `ItemsSource` プロパティには `IEnumerable<T>` ではなく `IList<T>` コレクションをバインドする。 
-  可能な限り `ViewCell` ではなく、組み込みのセル ( `TextCell` / `SwitchCell` など) を使用する。
-  より少ない要素を使う。例えば複数の Label の代わりに `FormattedString` を使った1つの Label の使用を検討する。
-  不均一のデータ（異なる型のデータ） を表示する場合は、 `ListView` を `TableView` に置き換える。
-  [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) メソッドの使用を控える。使いすぎると、パフォーマンスが低下します。
-  Android でインスタンス化した後で、 `ListView` の 行セパレータの可視性や色を設定することを避ける。これは大きなパフォーマンスの低下が発生します。
-  [ `BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) に基づいたセルのレイアウトの変更を避ける。これは大きなレイアウトと初期化のコストが発生します。
-  深くネストされたレイアウトの階層は避ける。ネストを減らすために、 `AbsoluteLayout` または `Grid` を使用する。
-  `LayoutOptions` に `Fill` ( Fill は最も計算量が少ない）以外を指定することを避ける。
-  `ScrollView` の内部に `ListView` を配置することは以下の理由で避ける。
  - `ListView` には自身にクロール機能が実装されている。
  - `ListView` は全てのジェスチャを受け取らない。それらは親である `ScrollView` によって処理される。
  - `ListView` リストの要素と同時にスクロールするカスタマイズされたヘッダーとフッターを表示することができ、その機能のために潜在的に `ScrollView` が提供されている。詳細については、次を参照してください。[ヘッダーとフッター](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#Headers_and_Footers)
-  セルの中で非常に特殊な複雑なデザインが必要な場合は Custom Renderer を検討する。

`AbsoluteLayout` は1回も計測処理を呼ぶことなくレイアウトを実行できる可能性があります。これはパフォーマンス上で非常に強力です。もし `AbsoluteLayout` が使えないのであれば、 [ `RelativeLayout`](http://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/) を検討してください。 `RelativeLayout` を使えば、直接制約を渡すことで式木 API を使うよりもかなり速くなるでしょう。なぜなら、式木 API は JIT を使い、 iOS ではその式木が実行時に解釈され、それが非常に低速だからです。式木 API はレイアウトの初期化や回転時にのみ呼ばれるようなページレイアウトに適しています。しかし `ListView` ではその処理がスクロールの間、継続的に実行され、パフォーマンスを損ないます。

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) やセルの custom renderer を作成することは、スクロール実行時のレイアウト計算の処理を減らす1つのアプローチです。詳細については、 [ListView のカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) や [ViewCell のカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md) を参照してください。。


## <a name="related-links"></a>関連リンク

- [Custom Renderer View (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Custom Renderer ViewCell (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
- [ListViewCachingStrategy](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/)
