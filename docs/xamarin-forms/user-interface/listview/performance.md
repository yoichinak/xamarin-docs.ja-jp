---
title: ListView のパフォーマンス
description: ListView を基本としたアプリで優れたパフォーマンスを確保する
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

モバイル アプリケーションを作成するには、パフォーマンスが重要です。 スムーズ スクロールと高速読み込み時間を期待するユーザーになっています。 ユーザーの期待を満たすために失敗して、アプリケーション ストア内の評価のコストまたは、基幹業務アプリケーションの場合、組織の時間とコストのコストします。

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)強力なビュー データを表示するには、いくつかの制限があります。 深く入れ子になったビュー階層が含まれて、やを多数の測定値を必要とする特定のレイアウトを使用する場合は特に、カスタムのセルを使用する場合、スクロールのパフォーマンスが低下します。 幸いにも、パフォーマンスの低下を避けるために使用できる手法があります。

この記事では、次のトピックがについて説明します。

- **[キャッシュの戦略](#cachingstrategy)**
- **[ListView のパフォーマンスの向上](#improving-performance)**

<a name="cachingstrategy" />

## <a name="caching-strategy"></a>キャッシュの戦略

Listview がより多くのデータの表示に使用される多くの場合、画面に表示される調整します。 たとえば、音楽のアプリを検討してください。 曲のライブラリは、何千ものエントリがあります。 すべての曲の行を作成することを簡単なアプローチは、パフォーマンスの低下必要があります。 アプローチでは、貴重なメモリを浪費され、スクロール、クロールする速度が低下します。 別の方法を作成し、データがビューにスクロールされる基準と行を破棄します。 これには、定数のインスタンス化と非常に遅いことが可能なビュー オブジェクトのクリーンアップが必要です。

ネイティブ メモリを節約するために[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)対応する各プラットフォームの行を再使用するための組み込みの機能があります。 画面に表示されているセルのみがメモリに読み込まれると、**コンテンツ**は既存のセルに読み込まれます。 これは、何千もの時間とメモリを節約して、オブジェクトをインスタンス化する必要があるからアプリケーションを防止します。

Xamarin.Forms で許可される[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)を通じてセルが再利用して、 [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/)列挙体は、次の値があります。

```csharp
public enum ListViewCachingStrategy
{
  RetainElement,   // the default value
  RecycleElement,
  RecycleElementAndDataTemplate
}
```

> [!NOTE]
> ユニバーサル Windows プラットフォーム (UWP) は無視されます、 [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/)常に使用してキャッシュをパフォーマンスを向上させるための戦略をキャッシュします。 そのため、既定の動作として、 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/)キャッシング戦略を適用します。

### <a name="retainelement"></a>RetainElement

[ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/)をキャッシュ方法を指定します、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)一覧で、各項目のセルを生成し、既定値は、`ListView`動作します。 これは、次の状況で一般的に使用する必要があります。

- 各セルが多数の binding を持つ場合。 (20〜30以上)
- セルの template が頻繁に変更される場合。
- テストによって `RecycleElement` が実行速度を下げる結果になることが判明した場合。

結果を認識することが重要、 [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/)カスタムのセルを使用する場合は、戦略をキャッシュします。 任意のセルの初期化コードが各セルの作成を実行する必要があります 1 秒間に複数回ことが考えられます。 この状況では、ページに問題がレイアウトの手法などの複数の入れ子になった[ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)インスタンス、セットアップがあるときにパフォーマンスのボトルネックとなるし、ユーザーがスクロールをリアルタイムで破棄されます。

<a name="recycleelement" />

### <a name="recycleelement"></a>RecycleElement

[ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/)をキャッシュ方法を指定します、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)を一覧のセルを再利用して、メモリ使用量と実行速度を最小限に抑えることを試みます。 常に、このモードにパフォーマンスの向上が提供されていないと、今後の改善点を確認するテストを実行する必要があります。 ただし、優先の選択肢では、通常、し、次のような状況で使用する必要があります。

- ときに各セルにはバインドの中程度の数を小さいです。
- 各セルの [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) が全てのセルのデータを定義している場合。
- 各セルがほぼ同じで、セルの template が変化しない場合。

セルが更新されると、そのバインディング コンテキストを持つ仮想化中に確認してくださいためアプリケーションは、このモードを使用する場合にする必要がありますバインド コンテキストの更新プログラムが適切に処理されること。 セルに関するすべてのデータは、バインディング コンテキストから取得する必要があります。 または一貫性エラーが発生する可能性があります。 これは、セル データを表示するデータ バインドを使用して行うことができます。 または、セル データはで設定する必要があります、 `OnBindingContextChanged` 、上書きではなく、カスタムのセルのコンス トラクター、次のコード例に示すように。

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

詳細については、次を参照してください。[バインディング コンテキストを変更](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes)です。

IOS および Android では場合セルは、カスタムのレンダラーを使用している必要があります確認プロパティの変更通知が正しく実装されているします。 使用可能なセルのバインド コンテキストが更新されたときに、プロパティ値が変更されますセルが再利用される`PropertyChanged`イベントが発生します。 詳細については、次を参照してください。 [、ViewCell をカスタマイズする](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)です。

#### <a name="recycleelement-with-a-datatemplateselector"></a>DataTemplateSelector RecycleElement

ときに、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)を使用して、 [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)を選択する、 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)、 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/)キャッシュ戦略をキャッシュしない`DataTemplate`s。 代わりに、`DataTemplate`リスト内のデータの各項目が選択されています。

> [!NOTE]
> [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/)前提、キャッシング戦略が Xamarin.Forms 2.4 で導入されたときに、 [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)は選択を求められます、 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)各`DataTemplate`同じを返す必要があります[ `ViewCell` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)型です。 例として、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)で、`DataTemplateSelector`いずれかを返すことができます`MyDataTemplateA`(ここで`MyDataTemplateA`を返します、`ViewCell`型の`MyViewCellA`)、または`MyDataTemplateB`(ここで`MyDataTemplateB`を返します、`ViewCell`型の`MyViewCellB`) ときに、`MyDataTemplateA`を返す必要があるれる`MyViewCellA`または例外がスローされます。

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

[ `RecycleElementAndDataTemplate` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate/)キャッシュ方法に基づいて、 [ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/)キャッシュ方法をさらに確保する場合、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) の使用[`DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)を選択する、 [ `DataTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplate/)、 `DataTemplate`s がリスト内の項目の種類によってキャッシュされます。 したがって、 `DataTemplate`s は項目のインスタンスごとに 1 回ではなく、項目の種類ごと 1 回選択します。

> [!NOTE]
> [ `RecycleElementAndDataTemplate` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate/)前提がキャッシュ方法を`DataTemplate`によって返される、 [ `DataTemplateSelector` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DataTemplateSelector/)を使用する必要があります、 [ `DataTemplate` ](https://developer.xamarin.com/api/constructor/Xamarin.Forms.DataTemplate.DataTemplate/p/System.Type/)受け取るコンス トラクター、`Type`です。

### <a name="setting-the-caching-strategy"></a>キャッシュの方法を設定

[ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/)列挙の値が指定されています、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)コンス トラクター オーバー ロードで、次のコード例に示すようにします。

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

XAML では、設定、`CachingStrategy`属性の次のコードに示すようにします。

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

これは C# でコンストラクタに、caching strategy の引数を設定するのと同じ効果があります。ただし、 `CachingStrategy` には `ListView` というプロパティは存在しないことに注意してください。

#### <a name="setting-the-caching-strategy-in-a-subclassed-listview"></a>サブクラス化された ListView での Caching Strategy の設定

`CachingStrategy` [ `ListView` のサブクラスでは、 XAML での ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) 属性の設定は、期待した動作は起こりません。 `ListView`CachingStrategy`には `ListView` というプロパティは存在しないからです。 さらに、 [XAMLC](~/xamarin-forms/xaml/xamlc.md) が有効であれば、次のようなエラーメッセージが表示されるでしょう。

この問題を解決するには、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) パラメータを受け取る [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/) のサブクラスのコンストラクタを作成し、それを基本クラスに渡します。

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

`ListView` フォーマンスを向上させるための多くのテクニックがあります。

-  `ItemsSource` コレクションは、ランダムアクセスをサポートしないため、 `IList<T>` プロパティには `IEnumerable<T>` ではなく `IEnumerable<T>` コレクションをバインドする。
-  可能な限り `TextCell` ではなく、組み込みのセル (  /  `SwitchCell` `ViewCell` など) を使用する。
-  より少ない要素を使う。 例えば複数の Label の代わりに `FormattedString` を使った1つの Label の使用を検討する。
-  不均一のデータ（異なる型のデータ） を表示する場合は、 `ListView` を `TableView` 置き換える。
-  [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/) ソッドの使用を控える。 使いすぎると、パフォーマンスが低下します。
-  Android でインスタンス化した後で、 `ListView` の 行セパレータの可視性や色を設定することを避けます。これは大きなパフォーマンスの低下が発生します。
-  [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) に基づいたセルのレイアウトの変更を避けます。 これは大きなレイアウトと初期化のコストが発生します。
-  深くネストされたレイアウトの階層は避けます。 ネストを減らすために、 `AbsoluteLayout` または `Grid` を使用します。
-  `LayoutOptions`に `Fill` ( Fill は最も計算量が少ない）以外を指定することを避けます。
-  `ListView` の内部に `ScrollView` を配置することは以下の理由で避けます。
  - `ListView` には自身にクロール機能が実装されています。
  - `ListView`は全てのジェスチャを受け取らない。それらは親である `ScrollView` によって処理されます。

  - `ListView`リストの要素と同時にスクロールするカスタマイズされたヘッダーとフッターを表示することができ、その機能のために潜在的に `ScrollView` が提供されています。 詳細については、次を参照してください。[ヘッダーとフッター](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#Headers_and_Footers)
-  セルの中で非常に特殊な複雑なデザインが必要な場合は Custom Renderer を検討します。

`AbsoluteLayout` は1回も計測処理を呼ぶことなくレイアウトを実行できる可能性があります。 これはパフォーマンス上で非常に強力です。 場合`AbsoluteLayout`することはできません、使用を検討してください[`RelativeLayout`](http://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/)です。 `RelativeLayout` を使えば、直接制約を渡すことで式木 API を使うよりもかなり速くなるでしょう。 なぜなら、式木 API は JIT を使い、 iOS ではその式木が実行時に解釈され、それが非常に低速だからです。 式木 API はレイアウトの初期化や回転時にのみ呼ばれるようなページレイアウトに適しています。しかし `ListView` ではその処理がスクロールの間、継続的に実行され、パフォーマンスを損ないます。

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) やセルの custom renderer を作成することは、スクロール実行時のレイアウト計算の処理を減らす1つのアプローチです。 詳細については、 [ListView のカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) や [ViewCell のカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md) を参照してください。


## <a name="related-links"></a>関連リンク

- [Custom Renderer View (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Custom Renderer ViewCell (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
- [ListViewCachingStrategy](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/)
