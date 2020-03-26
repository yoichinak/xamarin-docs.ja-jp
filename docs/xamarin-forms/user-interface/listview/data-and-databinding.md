---
title: ListView のデータ ソース
description: この記事では、データ、Xamarin.Forms の ListView を設定する方法と、ListView でのデータ バインディングを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2020
ms.openlocfilehash: e51f0bd011750b030c0a11b9b89a2c2473f2a9ed
ms.sourcegitcommit: d83c6af42ed26947aa7c0ecfce00b9ef60f33319
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247588"
---
# <a name="listview-data-sources"></a>ListView のデータ ソース

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)

データの一覧を表示するには、Xamarin. Forms [`ListView`](xref:Xamarin.Forms.ListView)を使用します。 この記事では、`ListView` にデータを設定する方法と、選択した項目にデータをバインドする方法について説明します。

## <a name="itemssource"></a>ItemsSource

[`ListView`](xref:Xamarin.Forms.ListView)には、 [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource)プロパティを使用してデータが設定されます。このプロパティは、`IEnumerable`を実装する任意のコレクションを受け入れることができます。 `ListView` を設定する最も簡単な方法は、文字列の配列を使用することです。

```xaml
<ListView>
      <ListView.ItemsSource>
          <x:Array Type="{x:Type x:String}">
            <x:String>mono</x:String>
            <x:String>monodroid</x:String>
            <x:String>monotouch</x:String>
            <x:String>monorail</x:String>
            <x:String>monodevelop</x:String>
            <x:String>monotone</x:String>
            <x:String>monopoly</x:String>
            <x:String>monomodal</x:String>
            <x:String>mononucleosis</x:String>
          </x:Array>
      </ListView.ItemsSource>
</ListView>
```

同等の C# コードを次に示します。

```csharp
var listView = new ListView();
listView.ItemsSource = new string[]
{
  "mono",
  "monodroid",
  "monotouch",
  "monorail",
  "monodevelop",
  "monotone",
  "monopoly",
  "monomodal",
  "mononucleosis"
};
```

![](data-and-databinding-images/itemssource-simple.png "ListView Displaying List of Strings")

この方法では、`ListView` に文字列の一覧が設定されます。 既定では、`ListView` は `ToString` を呼び出し、各行の `TextCell` に結果を表示します。 データの表示方法をカスタマイズするには、「[セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)」を参照してください。

`ItemsSource` が配列に送信されているため、基になるリストまたは配列の変更に応じてコンテンツは更新されません。 基になるリストで項目が追加、削除、および変更されたときに ListView を自動的に更新する場合は、`ObservableCollection`を使用する必要があります。 [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1)は `System.Collections.ObjectModel` で定義されており、`List`の場合と同じようになります。ただし、すべての変更を `ListView` に通知できます。

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
listView.ItemsSource = employees;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employees.Add(new Employee(){ DisplayName="Mr. Mono"});
```

## <a name="data-binding"></a>データ バインディング

データバインディングは、ユーザーインターフェイスオブジェクトのプロパティを、ビューモデルのクラスなど、一部の CLR オブジェクトのプロパティにバインドする "グルー" です。 データ バインディングは、多くの退屈な定型コードを置き換えることで、ユーザー インターフェイスの開発が簡略化されますので便利です。

データ バインディングは、バインドされた値が変更、オブジェクトを同期維持することで動作します。 コントロールの値が変更されるたびにイベントハンドラーを作成するのではなく、バインドを確立し、ビューモデルでバインドを有効にします。

データバインディングの詳細については、「[データバインディングの基本](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)」を参照してください。これは、 [Xamarin の XAML の基本記事シリーズ](~/xamarin-forms/xaml/xaml-basics/index.md)の第4部です。

### <a name="binding-cells"></a>セルのバインド

セル (およびセルの子) のプロパティは、`ItemsSource`内のオブジェクトのプロパティにバインドできます。 たとえば、`ListView` を使用すると、従業員の一覧を表示できます。

Employee クラス:

```csharp
public class Employee
{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>` が作成され、`ListView` `ItemsSource`として設定され、一覧にデータが設定されます。

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public ObservableCollection<Employee> Employees { get { return employees; }}

public EmployeeListPage()
{
    EmployeeView.ItemsSource = employees;

    // ObservableCollection allows items to be added after ItemsSource
    // is set and the UI will react to changes
    employees.Add(new Employee{ DisplayName="Rob Finnerty"});
    employees.Add(new Employee{ DisplayName="Bill Wrestler"});
    employees.Add(new Employee{ DisplayName="Dr. Geri-Beth Hooper"});
    employees.Add(new Employee{ DisplayName="Dr. Keith Joyce-Purdy"});
    employees.Add(new Employee{ DisplayName="Sheri Spruce"});
    employees.Add(new Employee{ DisplayName="Burt Indybrick"});
}
```

> [!WARNING]
> `ListView` は、基になる `ObservableCollection`の変更に応じて更新されますが、別の `ObservableCollection` インスタンスが元の `ObservableCollection` 参照 (`employees = otherObservableCollection;`など) に割り当てられている場合、`ListView` は更新されません。

次のスニペットは、従業員の一覧にバインドされている `ListView` を示しています。

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
             x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
             Title="Employee List">
  <ListView x:Name="EmployeeView"
            ItemsSource="{Binding Employees}">
    <ListView.ItemTemplate>
      <DataTemplate>
        <TextCell Text="{Binding DisplayName}" />
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

この XAML の例では、`ListView`を含む `ContentPage` を定義します。 `ListView` のデータソースは、`ItemsSource` 属性を使用して設定されます。 `ItemsSource` 内の各行のレイアウトは、`ListView.ItemTemplate` 要素内で定義されます。 この結果、次のスクリーンショットが表示されます。

![](data-and-databinding-images/bound-data.png "ListView using Data Binding")

> [!WARNING]
> `ObservableCollection` はスレッドセーフではありません。 `ObservableCollection` を変更すると、変更を行ったのと同じスレッドで UI の更新が発生します。 スレッドがプライマリ UI スレッドでない場合は、例外が発生します。

### <a name="binding-selecteditem"></a>SelectedItem にバインド

多くの場合、変更に応答するためにイベントハンドラーを使用するのではなく、`ListView`の選択した項目にバインドします。 XAML でこれを行うには、`SelectedItem` プロパティをバインドします。

```xaml
<ListView x:Name="listView"
          SelectedItem="{Binding Source={x:Reference SomeLabel},
          Path=Text}">
 …
</ListView>
```

`listView`の `ItemsSource` が文字列のリストであると仮定すると、`SomeLabel` の `Text` プロパティが `SelectedItem`にバインドされます。

## <a name="related-links"></a>関連リンク

- [双方向のバインディング (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
