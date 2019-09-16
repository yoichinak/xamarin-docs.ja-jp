---
title: ListView のパフォーマンス
description: ListView はデータを表示するための強力なビューが、いくつかの制限があります。 この記事では、アプリケーションで Xamarin.Forms ListView で優れたパフォーマンスを確保する方法について説明します。
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
ms.openlocfilehash: ed920db129d2af203046a648c0069580f99a295e
ms.sourcegitcommit: a5ef4497db04dfa016865bc7454b3de6ff088554
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/15/2019
ms.locfileid: "70998180"
---
# <a name="listview-performance"></a>ListView のパフォーマンス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)

モバイルアプリケーションの作成は、パフォーマンスが重要です。 ユーザは、スムーズなスクロールと高速ロード時間を期待するようになっています。 ユーザの期待に応え損なうと、ストアでのアプリケーションの評価が下がり、業務アプリの場合であれば、組織の時間と費用も無駄になります。

Xamarin. Forms [`ListView`](xref:Xamarin.Forms.ListView)はデータを表示するための強力なビューですが、いくつかの制限があります。 特に、深い入れ子になったビュー階層が含まれる場合や、複雑な測定を必要とする特定のレイアウトを使用する場合は、カスタムセルを使用すると、スクロールパフォーマンスが低下することがあります。 しかし、パフォーマンスの低下を避けるために使用できるテクニックがあります。

## <a name="caching-strategy"></a>キャッシュ方法

ListViews は、画面に収まるよりも多くのデータを表示するためによく使用されます。 たとえば、音楽アプリには何千ものエントリを含む曲のライブラリがある場合があります。 エントリごとに項目を作成すると、貴重なメモリが無駄になり、パフォーマンスが低下します。 行を頻繁に作成して破棄するには、アプリケーションが常にオブジェクトをインスタンス化してクリーンアップする必要があります。この場合も、パフォーマンスが低下します。

メモリを節約するために[`ListView`](xref:Xamarin.Forms.ListView) 、各プラットフォームのネイティブに相当する機能には、行を再利用するための機能が組み込まれています。 画面上に見えているセルだけがメモリーにロードされ、**コンテンツ**がそのセルに読み込まれます。 このパターンは、アプリケーションが何千ものオブジェクトをインスタンス化して、時間とメモリを節約するのを防ぎます。

Xamarin では、 [`ListView`](xref:Xamarin.Forms.ListView)次の値を[`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy)持つ列挙体を使用したセルの再利用が許可されます。

```csharp
public enum ListViewCachingStrategy
{
    RetainElement,   // the default value
    RecycleElement,
    RecycleElementAndDataTemplate
}
```

> [!NOTE]
> Universal Windows Platform (UWP) では常にパフォーマンス向上のためにキャッシュ機能を使用するので、Caching Strategy の [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) は無視されます。 そのため、デフォルトで、[`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) が適用されるような動作をします。

### <a name="retainelement"></a>RetainElement

[ `RetainElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement)の caching strategy は、[`ListView`](xref:Xamarin.Forms.ListView) がリストの各項目ごとに1つのセルを生成することを指定します。これは `ListView` の規定の動作です。 次のような場合に使用する必要があります。

- 各セルには、多数のバインド (20 ~ 30 +) があります。
- セルテンプレートが頻繁に変更されます。
- テストでは、 `RecycleElement`キャッシュ戦略によって実行速度が低下することが明らかになりました。

カスタムセルを使って動かす場合は、 [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) での結果を認識することが大切です。 どんなセルの初期化コードでも各セルの生成ごとに走らせる必要があり、1秒間に複数回になる場合もあります。 このような状況では、複数の入れ子になっ[`StackLayout`](xref:Xamarin.Forms.StackLayout)たインスタンスを使用するなど、ページに問題があるレイアウト手法は、ユーザーがスクロールしたときにリアルタイムで設定および破棄された場合に、パフォーマンスのボトルネックになります。

### <a name="recycleelement"></a>RecycleElement

[ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)の caching strategy を指定すると、[ `ListView` ](xref:Xamarin.Forms.ListView)はリストのセルを再利用することによって、メモリ使用量と実行速度を最小にすることを試みます。 このモードでは、常にパフォーマンスが向上するわけではありません。また、改善を判断するためにテストを実行する必要があります。 ただし、これは推奨される方法であり、次のような状況で使用する必要があります。

- 各セルには、少ない数のバインドがあります。
- 各セル[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)には、すべてのセルデータが定義されています。
- 各セルは主に類似しており、セルテンプレートは変化しません。

仮想化の間はセルは自身の binding context が更新されるので、アプリがこのモードを使う場合は binding context の更新が適切に処理されることを保証しなければなりません。 セルに関するすべてのデータはその binding context に由来するものでなければなりません。そうしないと整合性エラーが発生する可能性があります。 この問題を回避するには、データバインディングを使用してセルデータを表示します。 あるいは、以下のコードサンプルで示すように、セルのデータをカスタムセルのコンストラクタの中ではなく、 `OnBindingContextChanged` の override の中でセットするようにしても良いでしょう。

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

iOS や Android でセルに CustomRenderer を使用している場合、プロパティ更新通知が正しく実装されていることを確認してください。 セルが再利用されると、`PropertyChanged` イベントが発生し、それらセルのプロパティの値は、binding context が有効なセルに更新される時を変更します。 より詳しい情報は [Customizing a ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md) を参照してください。

#### <a name="recycleelement-with-a-datatemplateselector"></a>DataTemplateSelector を使った RecycleElement

[`ListView`](xref:Xamarin.Forms.ListView) が [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) を選択するために [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) を使用している場合、[`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) の caching strategy は `DataTemplate` をキャッシュしません。 その代わり、 `DataTemplate` はリストのデータの項目ごとに選択されます。

> [!NOTE]
> [ `RecycleElement` ](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)caching strategy には Xamarin.Forms 2.4 で導入された前提条件があります。それは [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) が [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) を選択することを要求されると、それぞれの `DataTemplate` は同じ [`ViewCell`](xref:Xamarin.Forms.ViewCell) の型を返さなければならないということです 例として、 [ `ListView` ](xref:Xamarin.Forms.ListView)で、`DataTemplateSelector`いずれかを返すことができます`MyDataTemplateA`(ここで`MyDataTemplateA`を返します、`ViewCell`型の`MyViewCellA`)、または`MyDataTemplateB`(ここで`MyDataTemplateB`を返します、`ViewCell`型の`MyViewCellB`) ときに、`MyDataTemplateA`を返す必要があるれる`MyViewCellA`または例外がスローされます。

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

[`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) の caching strategy は [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) を拡張したもので、 [`ListView`](xref:Xamarin.Forms.ListView) が [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) を使う場合に、 [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) をリスト内の項目の型ごとにキャッシュする機能を加えたものです。これにより `DataTemplate` は、項目のインスタンスごとに1つではなく、項目の型ごとに1つ選択されるようになります。 そのため、 `DataTemplate`s は項目インスタンスごとに 1 回ではなく、項目の種類あたり 1 回選択します。

> [!NOTE]
> [`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) は前提条件があり、 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) によって返される`DataTemplate` は `Type` 型 を引数にとる [`DataTemplate`](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) のコンストラクタを使わなければなりません。

### <a name="set-the-caching-strategy"></a>キャッシュ方法を設定する

[ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy)列挙型の値は、次のコード例で示すように、[`ListView`](xref:Xamarin.Forms.ListView) のコンストラクタのオーバーロードで指定します。

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

Xaml で、次の`CachingStrategy` xaml に示すように属性を設定します。

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

このメソッドは、のC#コンストラクターでキャッシュ方法引数を設定するのと同じ効果があります。

#### <a name="set-the-caching-strategy-in-a-subclassed-listview"></a>サブクラス化された ListView でキャッシュ方法を設定する

`CachingStrategy` `ListView`サブクラス[`ListView`](xref:Xamarin.Forms.ListView)の XAML から属性を設定しても、にはプロパティがないため、目的の動作は生成さ`CachingStrategy`れません。 さらに、 [XAMLC](~/xamarin-forms/xaml/xamlc.md)が有効になっている場合は、次のエラーメッセージが生成されます。 **' CachingStrategy ' に対してプロパティ、バインド可能なプロパティ、またはイベントが見つかりませんでした**

この問題を解決するには、 [ `ListView` ](xref:Xamarin.Forms.ListView) パラメータを受け取る [ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) のサブクラスのコンストラクタを作成し、それを基本クラスに渡します。

```csharp
public class CustomListView : ListView
{
    public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
    {
    }
    ...
}
```

そして [ `ListViewCachingStrategy` ](xref:Xamarin.Forms.ListViewCachingStrategy) 列挙型の値は、 XAML から `x:Arguments` 構文を使って指定できます。

```xaml
<local:CustomListView>
    <x:Arguments>
        <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
    </x:Arguments>
</local:CustomListView>
```

## <a name="listview-performance-suggestions"></a>ListView のパフォーマンスの提案

のパフォーマンスを向上させるには、 `ListView`多くの方法があります。 次の推奨事項により、ListView のパフォーマンスが向上する場合があります。

- `ItemsSource` コレクションは、ランダムアクセスをサポートしないため、 `IList<T>` プロパティには `IEnumerable<T>` ではなく `IEnumerable<T>` コレクションをバインドする。
- 可能な場合は、ではなく`TextCell` `ViewCell` 、組み込みのセル (など /  `SwitchCell` ) を使用します。
- より少ない要素を使う。 たとえば、複数のラベルでは`FormattedString`なく、1つのラベルを使用することを検討してください。
- 不均一のデータ（異なる型のデータ） を表示する場合は、 `ListView` を `TableView` 置き換える。
- [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) ソッドの使用を控える。 使いすぎると、パフォーマンスが低下します。
- Android でインスタンス化した後で、 `ListView` の 行セパレータの可視性や色を設定することを避けます。これは大きなパフォーマンスの低下が発生します。
- [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) に基づいたセルのレイアウトの変更を避けます。 レイアウトを変更すると、大きな測定と初期化のコストが発生します。
- 深くネストされたレイアウトの階層は避けます。 ネストを減らすために、 `AbsoluteLayout` または `Grid` を使用します。
- 以外`LayoutOptions` `Fill`は指定しないようにしてください (は、コンピューティングのコストが最も安いものです)。 `Fill`
- `ListView` の内部に `ScrollView` を配置することは以下の理由で避けます。
  - `ListView` には自身にクロール機能が実装されています。
  - `ListView`は全てのジェスチャを受け取らない。それらは親である `ScrollView` によって処理されます。
  - `ListView`リストの要素と同時にスクロールするカスタマイズされたヘッダーとフッターを表示することができ、その機能のために潜在的に `ScrollView` が提供されています。 詳細については、「[ヘッダーとフッター](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#headers-and-footers)」を参照してください。
- セルに表示される特定の複雑なデザインが必要な場合は、カスタムレンダラーを検討してください。

`AbsoluteLayout`1つのメジャー呼び出しを行わずにレイアウトを実行して、高いパフォーマンスを実現できる可能性があります。 場合`AbsoluteLayout`することはできません、使用を検討してください[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)です。 `RelativeLayout` を使えば、直接制約を渡すことで式木 API を使うよりもかなり速くなるでしょう。 このメソッドは、式 API が JIT を使用し、iOS ではツリーを解釈する必要があるため、処理速度が遅くなります。 式木 API はレイアウトの初期化や回転時にのみ呼ばれるようなページレイアウトに適しています。しかし `ListView` ではその処理がスクロールの間、継続的に実行され、パフォーマンスを損ないます。

[ `ListView` ](xref:Xamarin.Forms.ListView) やセルの custom renderer を作成することは、スクロール実行時のレイアウト計算の処理を減らす1つのアプローチです。 詳細については、 [ListView のカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) や [ViewCell のカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md) を参照してください。

## <a name="related-links"></a>関連リンク

- [Custom Renderer View (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [Custom Renderer ViewCell (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)
- [ListViewCachingStrategy](xref:Xamarin.Forms.ListViewCachingStrategy)
