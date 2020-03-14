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
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306441"
---
# <a name="listview-performance"></a>ListView のパフォーマンス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)

モバイルアプリケーションの作成は、パフォーマンスが重要です。 ユーザは、スムーズなスクロールと高速ロード時間を期待するようになっています。 ユーザの期待に応え損なうと、ストアでのアプリケーションの評価が下がり、業務アプリの場合であれば、組織の時間と費用も無駄になります。

Xamarin [`ListView`](xref:Xamarin.Forms.ListView)はデータを表示するための強力なビューですが、いくつかの制限があります。 特に、深い入れ子になったビュー階層が含まれる場合や、複雑な測定を必要とする特定のレイアウトを使用する場合は、カスタムセルを使用すると、スクロールパフォーマンスが低下することがあります。 しかし、パフォーマンスの低下を避けるために使用できるテクニックがあります。

## <a name="caching-strategy"></a>キャッシュ方法

ListViews は、画面に収まるよりも多くのデータを表示するためによく使用されます。 たとえば、音楽アプリには何千ものエントリを含む曲のライブラリがある場合があります。 エントリごとに項目を作成すると、貴重なメモリが無駄になり、パフォーマンスが低下します。 行を頻繁に作成して破棄するには、アプリケーションが常にオブジェクトをインスタンス化してクリーンアップする必要があります。この場合も、パフォーマンスが低下します。

メモリを節約するために、各プラットフォームのネイティブ[`ListView`](xref:Xamarin.Forms.ListView)に相当する機能には、行を再利用するための機能が組み込まれています。 画面に表示されるセルだけがメモリに読み込まれ、**コンテンツ**は既存のセルに読み込まれます。 このパターンは、アプリケーションが何千ものオブジェクトをインスタンス化して、時間とメモリを節約するのを防ぎます。

Xamarin では、 [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy)列挙体を使用してセルを再利用[`ListView`](xref:Xamarin.Forms.ListView)ことができます。これには次の値が含まれます。

```csharp
public enum ListViewCachingStrategy
{
    RetainElement,   // the default value
    RecycleElement,
    RecycleElementAndDataTemplate
}
```

> [!NOTE]
> ユニバーサル Windows プラットフォーム (UWP) は、常にキャッシュを使用してパフォーマンスを向上させるため、 [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement)のキャッシュ方法を無視します。 そのため、既定では、 [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)キャッシュ戦略が適用されているかのように動作します。

### <a name="retainelement"></a>RetainElement

[`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement)キャッシュ戦略では、 [`ListView`](xref:Xamarin.Forms.ListView)がリストの各項目に対してセルを生成することを指定します。これは、既定の `ListView` の動作です。 次のような場合に使用する必要があります。

- 各セルには、多数のバインド (20 ~ 30 +) があります。
- セルテンプレートが頻繁に変更されます。
- テストにより、`RecycleElement` のキャッシュ戦略によって実行速度が低下することが明らかになります。

カスタムセルを使用する場合は、 [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement)キャッシュ戦略の結果を認識することが重要です。 どんなセルの初期化コードでも各セルの生成ごとに走らせる必要があり、1秒間に複数回になる場合もあります。 このような状況では、入れ子になった複数の[`StackLayout`](xref:Xamarin.Forms.StackLayout)インスタンスを使用するなど、ページに問題があるレイアウト手法は、ユーザーがスクロールしたときにリアルタイムで設定および破棄されると、パフォーマンスのボトルネックになります。

### <a name="recycleelement"></a>RecycleElement

[`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)キャッシュの方法では、 [`ListView`](xref:Xamarin.Forms.ListView)がリストのセルを再利用することによって、メモリフットプリントと実行速度を最小限に抑えるように指定します。 このモードでは、常にパフォーマンスが向上するわけではありません。また、改善を判断するためにテストを実行する必要があります。 ただし、これは推奨される方法であり、次のような状況で使用する必要があります。

- 各セルには、少ない数のバインドがあります。
- 各セルの[`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)によって、すべてのセルデータが定義されます。
- 各セルは主に類似しており、セルテンプレートは変化しません。

仮想化の間はセルは自身の binding context が更新されるので、アプリがこのモードを使う場合は binding context の更新が適切に処理されることを保証しなければなりません。 セルに関するすべてのデータはその binding context に由来するものでなければなりません。そうしないと整合性エラーが発生する可能性があります。 この問題を回避するには、データバインディングを使用してセルデータを表示します。 また、次のコード例に示すように、セルデータは、カスタムセルのコンストラクターではなく、`OnBindingContextChanged` のオーバーライドで設定する必要があります。

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

詳細については、「[バインドコンテキストの変更](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes)」を参照してください。

iOS や Android でセルに CustomRenderer を使用している場合、プロパティ更新通知が正しく実装されていることを確認してください。 セルが再利用されると、プロパティ値は、`PropertyChanged` イベントが発生している使用可能なセルのバインドコンテキストに更新されると変更されます。 詳細については、「 [ViewCell のカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)」を参照してください。

#### <a name="recycleelement-with-a-datatemplateselector"></a>DataTemplateSelector を使った RecycleElement

[`ListView`](xref:Xamarin.Forms.ListView)が[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)を使用して[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)を選択した場合、 [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)のキャッシュ戦略では `DataTemplate`がキャッシュされません。 代わりに、一覧のデータの各項目に対して `DataTemplate` が選択されます。

> [!NOTE]
> [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)のキャッシュ方法には、Xamarin. Forms 2.4 で導入された前提条件があります。これは、各 `DataTemplate` が同じ[`ViewCell`](xref:Xamarin.Forms.ViewCell)の種類を返す必要がある[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)を選択するように[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)に要求された場合に発生します。 たとえば[`ListView`](xref:Xamarin.Forms.ListView) 、`MyDataTemplateA` を返すことができる `DataTemplateSelector` (`MyDataTemplateA` が `ViewCell` 型の `MyViewCellA`を返す)、または `MyDataTemplateB` (`MyDataTemplateB` が型の `ViewCell` を返す) がある場合、`MyViewCellB`が返されたときには `MyDataTemplateA` を返す必要があり、例外がスローされます。`MyViewCellA`

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

[`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate)キャッシュ戦略は、 [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)のキャッシュ戦略に基づいて構築されています。 [`ListView`](xref:Xamarin.Forms.ListView)が[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)を使用して[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)を選択した場合、`DataTemplate`はリスト内の項目の種類によってキャッシュされます。 したがって、アイテムのインスタンスごとに1回ではなく、アイテムの種類ごとに1回、`DataTemplate`s が選択されます。

> [!NOTE]
> [`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate)のキャッシュ方法には、 [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)によって返される `DataTemplate`が `Type`を受け取る[`DataTemplate`](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type))コンストラクターを使用する必要があることを前提としています。

### <a name="set-the-caching-strategy"></a>キャッシュ方法を設定する

[`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy)列挙値は、次のコード例に示すように、 [`ListView`](xref:Xamarin.Forms.ListView)コンストラクターのオーバーロードを使用して指定します。

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

XAML で、次の XAML に示すように `CachingStrategy` 属性を設定します。

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

サブクラス[`ListView`](xref:Xamarin.Forms.ListView)の XAML から `CachingStrategy` 属性を設定しても、`ListView`に `CachingStrategy` プロパティがないため、目的の動作は生成されません。 さらに、 [XAMLC](~/xamarin-forms/xaml/xamlc.md)が有効になっている場合は、 **"cachingstrategy" のプロパティ、バインド可能なプロパティ、またはイベントが見つからない**というエラーメッセージが生成されます。

この問題を解決するには、サブクラス化された[`ListView`](xref:Xamarin.Forms.ListView)で、 [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy)パラメーターを受け取り、それを基底クラスに渡すコンストラクターを指定します。

```csharp
public class CustomListView : ListView
{
    public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
    {
    }
    ...
}
```

次に、`x:Arguments` 構文を使用して、 [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy)列挙値を XAML から指定できます。

```xaml
<local:CustomListView>
    <x:Arguments>
        <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
    </x:Arguments>
</local:CustomListView>
```

## <a name="listview-performance-suggestions"></a>ListView のパフォーマンスの提案

`ListView`のパフォーマンスを向上させる方法は多数あります。 次の推奨事項により、ListView のパフォーマンスが向上する場合があります。

- `IEnumerable<T>` コレクションはランダムアクセスをサポートしていないため、`ItemsSource` プロパティを `IEnumerable<T>` コレクションではなく `IList<T>` コレクションにバインドします。
- 組み込みのセル (`TextCell` / `SwitchCell` など) は、いつでも `ViewCell` の代わりに使用します。
- より少ない要素を使う。 たとえば、複数のラベルではなく、1つの `FormattedString` ラベルを使用することを検討してください。
- 非同種データ (異なる型のデータ) を表示する場合は、`ListView` を `TableView` に置き換えます。
- [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize)メソッドの使用を制限します。 使いすぎると、パフォーマンスが低下します。
- Android では、パフォーマンスが大幅に低下するため、インスタンス化後に `ListView`の行区切り記号の可視性や色を設定しないようにします。
- [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext)に基づいてセルのレイアウトを変更しないようにしてください。 レイアウトを変更すると、大きな測定と初期化のコストが発生します。
- 深くネストされたレイアウトの階層は避けます。 `AbsoluteLayout` または `Grid` を使用して、入れ子を減らすことができます。
- `Fill` 以外の特定の `LayoutOptions` は避けてください (`Fill` はコンピューティングのコストが最も低い)。
- 次の理由により、`ScrollView` 内に `ListView` を配置しないでください。
  - `ListView` には、独自のスクロールが実装されています。
  - `ListView` は、親 `ScrollView`によって処理されるため、どのようなジェスチャも受け取りません。
  - `ListView` では、リストの要素と共にスクロールするカスタマイズされたヘッダーとフッターを提示できます。これにより、`ScrollView` が使用された機能が提供される可能性があります。 詳細については、「[ヘッダーとフッター](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#headers-and-footers)」を参照してください。
- セルに表示される特定の複雑なデザインが必要な場合は、カスタムレンダラーを検討してください。

`AbsoluteLayout` には、1つのメジャー呼び出しを行わずにレイアウトを実行して、高いパフォーマンスを実現できる可能性があります。 `AbsoluteLayout` 使用できない場合は、 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)を検討してください。 `RelativeLayout`を使用する場合、式 API を使用するよりも制約を直接渡す方がはるかに高速です。 このメソッドは、式 API が JIT を使用し、iOS ではツリーを解釈する必要があるため、処理速度が遅くなります。 Expression API は、初期レイアウトとローテーションでのみ必要なページレイアウトに適していますが、スクロール中に常に実行される `ListView`では、パフォーマンスが低下します。

[`ListView`](xref:Xamarin.Forms.ListView)またはそのセルのカスタムレンダラーを構築する方法の1つは、スクロールパフォーマンスに対するレイアウト計算の効果を下げることです。 詳細については、「 [ListView のカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)」および「 [viewcell のカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [カスタムレンダラービュー (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [カスタムレンダラー ViewCell (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)
- [ListViewCachingStrategy](xref:Xamarin.Forms.ListViewCachingStrategy)
