---
title: "ListView Performance" の説明: "ListView はデータを表示するための強力なビューですが、いくつかの制限があります。 この記事では、アプリケーションで ListView を使用して優れたパフォーマンスを確保する方法について説明 Xamarin.Forms します。 "
ms. 製品: xamarin ms. assetid: 1B085639-652C-4862-86EB-5D55D32B9395: xamarin-forms author: davidbritch ms. author: dabritch ms. date: 12/11/2017 no loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="listview-performance"></a>ListView のパフォーマンス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)

モバイルアプリケーションを作成するときは、パフォーマンスが重要です。 ユーザーは、スムーズスクロールと高速読み込み時間を期待できます。 ユーザーの期待に合わない場合、アプリケーションストアでの評価にコストがかかります。また、基幹業務アプリケーションの場合は、組織の時間とコストを削減できます。

は、 Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) データを表示するための強力なビューですが、いくつかの制限があります。 特に、深い入れ子になったビュー階層が含まれる場合や、複雑な測定を必要とする特定のレイアウトを使用する場合は、カスタムセルを使用すると、スクロールパフォーマンスが低下することがあります。 幸いにも、パフォーマンスの低下を避けるために使用できる手法があります。

## <a name="caching-strategy"></a>キャッシュ方法

ListViews は、画面に収まるよりも多くのデータを表示するためによく使用されます。 たとえば、音楽アプリには何千ものエントリを含む曲のライブラリがある場合があります。 エントリごとに項目を作成すると、貴重なメモリが無駄になり、パフォーマンスが低下します。 行を頻繁に作成して破棄するには、アプリケーションが常にオブジェクトをインスタンス化してクリーンアップする必要があります。この場合も、パフォーマンスが低下します。

メモリを節約するために、各プラットフォームのネイティブに相当する機能には、行を再 [`ListView`](xref:Xamarin.Forms.ListView) 利用するための機能が組み込まれています。 画面に表示されるセルだけがメモリに読み込まれ、**コンテンツ**は既存のセルに読み込まれます。 このパターンは、アプリケーションが何千ものオブジェクトをインスタンス化して、時間とメモリを節約するのを防ぎます。

Xamarin.Formsでは、 [`ListView`](xref:Xamarin.Forms.ListView) [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) 次の値を持つ列挙体を使用したセルの再利用が許可されます。

```csharp
public enum ListViewCachingStrategy
{
    RetainElement,   // the default value
    RecycleElement,
    RecycleElementAndDataTemplate
}
```

> [!NOTE]
> ユニバーサル Windows プラットフォーム (UWP) は、 [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) 常にキャッシュを使用してパフォーマンスを向上させるため、キャッシュ方法を無視します。 そのため、既定では、 [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) キャッシュ戦略が適用されているかのように動作します。

### <a name="retainelement"></a>RetainElement

キャッシュ戦略では、によって [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) [`ListView`](xref:Xamarin.Forms.ListView) リスト内の項目ごとにセルが生成されることを指定し `ListView` ます。これは既定の動作です。 次のような場合に使用する必要があります。

- 各セルには、多数のバインド (20 ~ 30 +) があります。
- セルテンプレートが頻繁に変更されます。
- テストでは、キャッシュ戦略によって実行速度が低下することが明らかになりました `RecycleElement` 。

[`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement)カスタムセルを使用する場合は、キャッシュ戦略の結果を認識することが重要です。 セルの初期化コードは、各セルの作成に対して実行する必要があります。これは1秒間に複数回行うことができます。 このような状況では、複数の入れ子になったインスタンスを使用するなど、ページに問題があるレイアウト手法は、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) ユーザーがスクロールしたときにリアルタイムで設定および破棄された場合に、パフォーマンスのボトルネックになります。

### <a name="recycleelement"></a>RecycleElement

[`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement)キャッシュ戦略では、で [`ListView`](xref:Xamarin.Forms.ListView) リストのセルを再利用することによって、メモリフットプリントと実行速度を最小限に抑えるように指定します。 このモードでは、常にパフォーマンスが向上するわけではありません。また、改善を判断するためにテストを実行する必要があります。 ただし、これは推奨される方法であり、次のような状況で使用する必要があります。

- 各セルには、少ない数のバインドがあります。
- 各セル [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) には、すべてのセルデータが定義されています。
- 各セルは主に類似しており、セルテンプレートは変化しません。

仮想化中に、セルのバインドコンテキストが更新されます。そのため、アプリケーションでこのモードを使用する場合は、バインドコンテキストの更新が適切に処理されるようにする必要があります。 セルに関するすべてのデータは、バインドコンテキストから取得する必要があります。それ以外の場合、一貫性エラーが発生する可能性があります。 この問題を回避するには、データバインディングを使用してセルデータを表示します。 また、次のコード例に示すように、セルデータは、 `OnBindingContextChanged` カスタムセルのコンストラクターではなく、オーバーライドで設定する必要があります。

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

IOS と Android では、セルでカスタムレンダラーを使用する場合、プロパティの変更通知が正しく実装されていることを確認する必要があります。 セルが再利用されると、プロパティ値は、バインドコンテキストが、使用可能なセルのバインドコンテキストに更新されると変更され、 `PropertyChanged` イベントが発生します。 詳細については、「 [ViewCell のカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)」を参照してください。

#### <a name="recycleelement-with-a-datatemplateselector"></a>RecycleElement と DataTemplateSelector

がを使用してを選択する場合、キャッシュ方法では、は [`ListView`](xref:Xamarin.Forms.ListView) [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) キャッシュされません `DataTemplate` 。 代わりに、 `DataTemplate` リスト内のデータの各項目に対してが選択されます。

> [!NOTE]
> キャッシュ戦略には、 [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) 2.4 で導入された前提条件があり Xamarin.Forms [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) ます。これは、が [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) 同じ型を返す必要があるを選択するように求められた場合に発生し `DataTemplate` [`ViewCell`](xref:Xamarin.Forms.ViewCell) ます。 たとえば、 [`ListView`](xref:Xamarin.Forms.ListView) `DataTemplateSelector` (が `MyDataTemplateA` `MyDataTemplateA` 型のを返す)、または (型のを返す) を返すを持つを指定した `ViewCell` 場合、 `MyViewCellA` `MyDataTemplateB` `MyDataTemplateB` `ViewCell` `MyViewCellB` が返されると、 `MyDataTemplateA` が返されるか、例外が `MyViewCellA` スローされます。

### <a name="recycleelementanddatatemplate"></a>RecycleElementAndDataTemplate

キャッシュ戦略は、がを使用してを選択するときに、 [`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) [`ListView`](xref:Xamarin.Forms.ListView) [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) `DataTemplate` リスト内の項目の型によってキャッシュされることを保証することで、キャッシュ戦略に基づいています。 したがって、 `DataTemplate` は項目のインスタンスごとに1回ではなく、項目の種類ごとに1回だけ選択されます。

> [!NOTE]
> キャッシュ戦略には、に [`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) `DataTemplate` よって返されたが [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) を受け取るコンストラクターを使用する必要があることを前提としてい [`DataTemplate`](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) `Type` ます。

### <a name="set-the-caching-strategy"></a>キャッシュ方法を設定する

[`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy)列挙値は、 [`ListView`](xref:Xamarin.Forms.ListView) 次のコード例に示すように、コンストラクターのオーバーロードを使用して指定します。

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

XAML で、次の `CachingStrategy` xaml に示すように属性を設定します。

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

このメソッドは、C# のコンストラクターでキャッシュ方法引数を設定するのと同じ効果があります。

#### <a name="set-the-caching-strategy-in-a-subclassed-listview"></a>サブクラス化された ListView でキャッシュ方法を設定する

サブクラスの XAML から属性を設定しても、 `CachingStrategy` [`ListView`](xref:Xamarin.Forms.ListView) にはプロパティがないため、目的の動作は生成されません `CachingStrategy` `ListView` 。 さらに、 [XAMLC](~/xamarin-forms/xaml/xamlc.md)が有効になっている場合は、 **"cachingstrategy" のプロパティ、バインド可能なプロパティ、またはイベントが見つからない**というエラーメッセージが生成されます。

この問題を解決するには、 [`ListView`](xref:Xamarin.Forms.ListView) パラメーターを受け取り、 [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) それを基底クラスに渡す、サブクラスのコンストラクターを指定します。

```csharp
public class CustomListView : ListView
{
    public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
    {
    }
    ...
}
```

次に、 [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) 構文を使用して、XAML から列挙値を指定でき `x:Arguments` ます。

```xaml
<local:CustomListView>
    <x:Arguments>
        <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
    </x:Arguments>
</local:CustomListView>
```

## <a name="listview-performance-suggestions"></a>ListView のパフォーマンスの提案

のパフォーマンスを向上させるには、多くの方法があり `ListView` ます。 次の推奨事項により、ListView のパフォーマンスが向上する場合があります。

- コレクションではなくコレクションにプロパティをバインドし `ItemsSource` `IList<T>` `IEnumerable<T>` ます。これは、コレクションが `IEnumerable<T>` ランダムアクセスをサポートしていないためです。
- 可能な場合は、ではなく、組み込みのセル (など) を使用し `TextCell`  /  `SwitchCell` `ViewCell` ます。
- 使用する要素を減らす。 たとえば、複数のラベルではなく、1つのラベルを使用することを検討してください `FormattedString` 。
- `ListView` `TableView` 非同種データ (異なる型のデータ) を表示する場合は、をに置き換えます。
- メソッドの使用を制限し [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) ます。 過剰になると、パフォーマンスが低下します。
- Android では、パフォーマンスが `ListView` 大幅に低下するため、インスタンス化後に、の行区切り記号の可視性や色を設定しないようにしてください。
- に基づいてセルのレイアウトを変更することは避けて [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) ください。 レイアウトを変更すると、大きな測定と初期化のコストが発生します。
- 深く入れ子になったレイアウト階層は避けてください。 `AbsoluteLayout`またはを使用して、 `Grid` 入れ子を減らすことができます。
- 以外は指定しないよう `LayoutOptions` `Fill` `Fill` にしてください (は、コンピューティングのコストが最も安いものです)。
- 次の理由により、にを配置しないで `ListView` `ScrollView` ください。
  - は、 `ListView` 独自のスクロールを実装します。
  - は、 `ListView` 親によって処理されるため、どのようなジェスチャも受け取りません `ScrollView` 。
  - は、 `ListView` リストの要素と共にスクロールする、カスタマイズされたヘッダーとフッターを提示できます。これにより、が使用された機能が提供される可能性があり `ScrollView` ます。 詳細については、「[ヘッダーとフッター](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#headers-and-footers)」を参照してください。
- セルに表示される特定の複雑なデザインが必要な場合は、カスタムレンダラーを検討してください。

`AbsoluteLayout`1つのメジャー呼び出しを行わずにレイアウトを実行して、高いパフォーマンスを実現できる可能性があります。 `AbsoluteLayout`を使用できない場合は、を検討 [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) してください。 を使用する場合 `RelativeLayout` 、制約を直接渡すと、EXPRESSION API を使用するよりもはるかに高速になります。 このメソッドは、式 API が JIT を使用し、iOS ではツリーを解釈する必要があるため、処理速度が遅くなります。 Expression API は、初期レイアウトとローテーションでのみ必要なページレイアウトに適していますが、スクロール中に常に実行されている場合は、 `ListView` パフォーマンスが低下します。

またはそのセルのカスタムレンダラーを構築 [`ListView`](xref:Xamarin.Forms.ListView) する方法の1つは、スクロールパフォーマンスに対するレイアウト計算の効果を下げることです。 詳細については、「 [ListView のカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)」および「 [viewcell のカスタマイズ](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [カスタムレンダラービュー (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [カスタムレンダラー ViewCell (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)
- [ListViewCachingStrategy](xref:Xamarin.Forms.ListViewCachingStrategy)
