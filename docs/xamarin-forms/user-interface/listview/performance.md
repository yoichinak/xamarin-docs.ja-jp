---
title: "ListView のパフォーマンス"
description: "ListView ベースのアプリで優れたパフォーマンスを確認します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: 81d4aec3153a4cb7bbb0f3577c5a67acd430f279
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="listview-performance"></a>ListView のパフォーマンス

モバイル アプリケーションを作成するには、パフォーマンスが重要です。 スムーズ スクロールと高速読み込み時間を期待するユーザーになっています。 ユーザーの期待を満たすために失敗して、アプリケーション ストア内の評価のコストまたは、基幹業務アプリケーションの場合、組織の時間とコストのコストします。

[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)強力なビュー データを表示するには、いくつかの制限があります。 深く入れ子になったビュー階層が含まれて、やを多数の測定値を必要とする特定のレイアウトを使用する場合は特に、カスタムのセルを使用する場合、スクロールのパフォーマンスが低下します。 幸いにも、パフォーマンスの低下を避けるために使用できる手法があります。

この記事では、次のトピックがについて説明します。

- **[キャッシュの戦略](#cachingstrategy)**
- **[ListView のパフォーマンスを向上させる](#improving-performance)**

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

- 各セルが多数のバインディングを持つ場合に (20-30 以上)。
- ときにセル テンプレートが頻繁に変更します。
- ときにテストが表示される、`RecycleElement`削減実行速度の戦略の結果をキャッシュします。

結果を認識することが重要、 [ `RetainElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RetainElement/)カスタムのセルを使用する場合は、戦略をキャッシュします。 任意のセルの初期化コードが各セルの作成を実行する必要があります 1 秒間に複数回ことが考えられます。 この状況では、ページに問題がレイアウトの手法などの複数の入れ子になった[ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)インスタンス、セットアップがあるときにパフォーマンスのボトルネックとなるし、ユーザーがスクロールをリアルタイムで破棄されます。

<a name="recycleelement" />

### <a name="recycleelement"></a>RecycleElement

[ `RecycleElement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ListViewCachingStrategy.RecycleElement/)をキャッシュ方法を指定します、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)を一覧のセルを再利用して、メモリ使用量と実行速度を最小限に抑えることを試みます。 常に、このモードにパフォーマンスの向上が提供されていないと、今後の改善点を確認するテストを実行する必要があります。 ただし、優先の選択肢では、通常、し、次のような状況で使用する必要があります。

- ときに各セルにはバインドの中程度の数を小さいです。
- ときに各セルの[ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)のすべてのセル データを定義します。
- ときに各セルは不変セル テンプレートを使用して、ほぼ同じです。

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

これは、C# の場合は、コンス トラクターで、キャッシュ ストラテジ引数を設定すると同じ効果あることに注意してくださいありません`CachingStrategy`プロパティ`ListView`です。

#### <a name="setting-the-caching-strategy-in-a-subclassed-listview"></a>サブクラス化された ListView でキャッシュの方法の設定

設定、 `CachingStrategy` 、サブクラスで XAML から属性[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)があるので、目的の動作を作成できませんがない`CachingStrategy`プロパティ`ListView`です。 さらに場合、 [XAMLC](~/xamarin-forms/xaml/xamlc.md)は有効な場合、次のエラー メッセージが生成されます:**ありませんプロパティ、バインド可能なプロパティ、またはイベント 'CachingStrategy' が見つかりました**

この問題を解決するには、サブクラスにコンス トラクターを指定する[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)を受け入れる、 [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/)パラメーター、基本クラスに渡します。

```csharp
public class CustomListView : ListView
{
  public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
  {
  }
  ...
}
```

続いて、 [ `ListViewCachingStrategy` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/)を使用して、XAML から列挙値を指定することができます、`x:Arguments`構文。

```xaml
<local:CustomListView>
  <x:Arguments>
    <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
  </x:Arguments>
</local:CustomListView>
```

<a name="improving-performance" />

## <a name="improving-listview-performance"></a>ListView のパフォーマンスを向上させる

パフォーマンスを向上させるための多くの方法がある、 `ListView`:

-  バインド、`ItemsSource`プロパティを`IList<T>`コレクションの代わりに、`IEnumerable<T>`コレクションのため`IEnumerable<T>`コレクションは、ランダム アクセスをサポートしません。
-  組み込みのセルを使用 (と同様に`TextCell`  /  `SwitchCell` ) の代わりに`ViewCell`されるたびに実行できます。
-  以下の要素を使用します。 たとえば、1 つの使用を検討`FormattedString`複数のラベルではなくラベル。
-  置換、`ListView`で、`TableView`違うデータ – つまり、さまざまな種類のデータを表示するときにします。
-  使用を制限、 [ `Cell.ForceUpdateSize` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Cell.ForceUpdateSize()/)メソッドです。 過剰、ことと、パフォーマンスが低下します。
-  Android での設定を避けるため、`ListView`の行の区切り記号の可視性または色でインスタンス化された後、大規模なパフォーマンスの低下が発生します。
-  に基づいてセルのレイアウトが変更されないように、 [ `BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/)です。 これにより、大規模なレイアウトおよび初期化のコストが発生します。
-  深く入れ子にされたレイアウト階層は避けてください。 使用して`AbsoluteLayout`または`Grid`入れ子を減らすためにします。
-  特定の回避`LayoutOptions`以外の`Fill`(塗りつぶしでは、最もコンピューティング)。
-  配置が回避、`ListView`内、`ScrollView`次の理由。
  - `ListView`独自のスクロールを実装します。
  - `ListView`親によって処理されますが、すべてのジェスチャは受信しません`ScrollView`です。
  - `ListView`機能を潜在的に提供するリストの要素と、カスタマイズされたヘッダーとフッターのスクロールを表示できます、`ScrollView`使用されました。 詳細については、次を参照してください。[ヘッダーとフッター](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#Headers_and_Footers)です。
-  セルに表示される具体的な複雑なデザインする必要がある場合は、カスタム レンダラーを検討してください。

`AbsoluteLayout` 1 つのメジャーの呼び出しがないレイアウトを実行する可能性があります。 これにより、パフォーマンスを非常に強力です。 場合`AbsoluteLayout`することはできません、使用を検討してください[ `RelativeLayout`](http://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/)です。 使用して場合`RelativeLayout`、式 API を使用するよりも大幅に高速の制約を直接渡すことになります。 式 API は、JIT し、iOS でツリー保有に解釈される低速であるためにです。 式 API はページ レイアウト用に適切な場所にのみ必要なは最初のレイアウトおよび回転で`ListView`、スクロール中に継続的に実行されるとき、パフォーマンスが低下します。

カスタム レンダラーを構築、 [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)またはそのセルがレイアウトの計算でスクロールのパフォーマンスの影響を軽減する方法の 1 つです。 詳細については、次を参照してください。 [ListView をカスタマイズする](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)と[、ViewCell をカスタマイズする](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)です。


## <a name="related-links"></a>関連リンク

- [カスタム レンダラー ビュー (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [カスタム レンダラー ViewCell (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
- [ListViewCachingStrategy](https://developer.xamarin.com/api/type/Xamarin.Forms.ListViewCachingStrategy/)
