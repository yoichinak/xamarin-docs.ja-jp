---
title: ListView データソース
description: この記事では、listview にデータを設定する方法 Xamarin.Forms と、listview でデータバインディングを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1e5f3b6cb84081f5e167d9afe7e7f2f2dffce247
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938113"
---
# <a name="listview-data-sources"></a>ListView データソース

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)

は、 Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) データのリストを表示するために使用されます。 この記事では、にデータを設定する方法 `ListView` と、選択した項目にデータをバインドする方法について説明します。

## <a name="itemssource"></a>ItemsSource

には、を [`ListView`](xref:Xamarin.Forms.ListView) [`ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) 実装する任意のコレクションを受け入れることができるプロパティを使用してデータが設定され `IEnumerable` ます。 にデータを設定する最も簡単な方法は、 `ListView` 文字列の配列を使用することです。

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

これに相当する C# コードを次に示します。

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

![ListView に文字列のリストが表示される](data-and-databinding-images/itemssource-simple.png)

この方法では、に文字列のリストが設定され `ListView` ます。 既定で `ListView` は、はを呼び出し、 `ToString` 各行のに結果を表示し `TextCell` ます。 データの表示方法をカスタマイズするには、「[セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)」を参照してください。

`ItemsSource`は配列に送信されているため、基になるリストまたは配列の変更に応じてコンテンツは更新されません。 基になるリストで項目が追加、削除、および変更されたときに ListView を自動的に更新する場合は、を使用する必要があり `ObservableCollection` ます。 [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1)はで定義されて `System.Collections.ObjectModel` おり `List` 、変更を通知できる点を除いて、と同じです `ListView` 。

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
listView.ItemsSource = employees;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employees.Add(new Employee(){ DisplayName="Mr. Mono"});
```

## <a name="data-binding"></a>データ バインディング

データバインディングは、ユーザーインターフェイスオブジェクトのプロパティを、ビューモデルのクラスなど、一部の CLR オブジェクトのプロパティにバインドする "グルー" です。 データバインディングは、多数の退屈な定型コードを置き換えることでユーザーインターフェイスの開発を簡略化するため、便利です。

データバインディングは、バインドされた値の変更に応じてオブジェクトの同期を維持することによって機能します。 コントロールの値が変更されるたびにイベントハンドラーを作成するのではなく、バインドを確立し、ビューモデルでバインドを有効にします。

データバインディングの詳細については、「 [ Xamarin.Forms XAML の基本」記事シリーズ](~/xamarin-forms/xaml/xaml-basics/index.md)の第4部である「[データバインディングの基本](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)」を参照してください。

### <a name="binding-cells"></a>セルのバインド

セル (およびセルの子) のプロパティは、内のオブジェクトのプロパティにバインドでき `ItemsSource` ます。 たとえば、を使用して `ListView` 従業員の一覧を表示できます。

Employee クラス:

```csharp
public class Employee
{
    public string DisplayName {get; set;}
}
```

が作成され、 `ObservableCollection<Employee>` として設定され、 `ListView` `ItemsSource` リストにデータが設定されます。

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
> は、 `ListView` 基になるの変更に応じて更新されますが、 `ObservableCollection` `ListView` 別の `ObservableCollection` インスタンスが元の参照に割り当てられている場合 `ObservableCollection` (など)、は更新されません `employees = otherObservableCollection;` 。

次のスニペットは、 `ListView` 従業員の一覧にバインドされたを示しています。

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

この XAML の例では `ContentPage` 、を含むを定義 `ListView` します。 `ListView` のデータ ソースは、`ItemsSource` 属性を使用して設定されます。 `ItemsSource` の各行のレイアウトは、`ListView.ItemTemplate` 要素内で定義されます。 この結果、次のスクリーンショットが表示されます。

![データバインディングを使用した ListView](data-and-databinding-images/bound-data.png)

> [!WARNING]
> `ObservableCollection` はスレッド セーフではありません。 を変更する `ObservableCollection` と、変更を行ったのと同じスレッドで UI の更新が発生します。 スレッドがプライマリ UI スレッドでない場合は、例外が発生します。

### <a name="binding-selecteditem"></a>SelectedItem のバインド

多くの場合、 `ListView` 変更に応答するためにイベントハンドラーを使用するのではなく、の選択した項目にバインドします。 XAML でこれを行うには、次のようにプロパティをバインドし `SelectedItem` ます。

```xaml
<ListView x:Name="listView"
          SelectedItem="{Binding Source={x:Reference SomeLabel},
          Path=Text}">
 …
</ListView>
```

が文字列のリストであると仮定すると、 `listView` `ItemsSource` プロパティが `SomeLabel` `Text` にバインドされ `SelectedItem` ます。

## <a name="related-links"></a>関連リンク

- [双方向のバインディング (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
