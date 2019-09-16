---
title: ListView のデータ ソース
description: この記事では、データ、Xamarin.Forms の ListView を設定する方法と、ListView でのデータ バインディングを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/30/2018
ms.openlocfilehash: aa968562470a1e3405bf68be7eb0294273970386
ms.sourcegitcommit: a5ef4497db04dfa016865bc7454b3de6ff088554
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/15/2019
ms.locfileid: "70998025"
---
# <a name="listview-data-sources"></a>ListView のデータ ソース

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)

データの一覧を[`ListView`](xref:Xamarin.Forms.ListView)表示するには、Xamarin. フォームを使用します。 この記事では、 `ListView`にデータを設定する方法と、選択した項目にデータをバインドする方法について説明します。

## <a name="itemssource"></a>ItemsSource

A [ `ListView` ](xref:Xamarin.Forms.ListView)を使用してデータが表示されます、 [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource)プロパティを実装するコレクションを受け入れることができる`IEnumerable`します。 設定する最も簡単な方法を`ListView`文字列の配列を使用する必要があります。

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

同等の C# コードに示します。

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

![](data-and-databinding-images/itemssource-simple.png "ListView 文字列の一覧を表示します。")

この方法では、 `ListView`に文字列のリストが設定されます。 既定では、`ListView`が呼び出す`ToString`で結果を表示し、`TextCell`行ごとにします。 データの表示方法をカスタマイズするを参照してください。[セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)します。

`ItemsSource`コンテンツは、基になるリストまたは配列の変更としては更新されません、配列に送信されました。 使用する必要があります、ListView の項目が追加、削除、および基になるリストの変更に応じて自動的に更新する場合は、`ObservableCollection`します。 [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1) 定義されている`System.Collections.ObjectModel`が同様と`List`に通知する点を除いて、`ListView`すべての変更。

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
listView.ItemsSource = employees;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employees.Add(new Employee(){ DisplayName="Mr. Mono"});
```

## <a name="data-binding"></a>データ バインディング

データバインディングは、ユーザーインターフェイスオブジェクトのプロパティを、ビューモデルのクラスなど、一部の CLR オブジェクトのプロパティにバインドする "グルー" です。 データ バインディングは、多くの退屈な定型コードを置き換えることで、ユーザー インターフェイスの開発が簡略化されますので便利です。

データ バインディングは、バインドされた値が変更、オブジェクトを同期維持することで動作します。 コントロールの値が変更されるたびにイベントハンドラーを作成するのではなく、バインドを確立し、ビューモデルでバインドを有効にします。

データ バインディングの詳細については、[データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)4 つの一部では、[記事シリーズを Xamarin.Forms XAML の基礎](~/xamarin-forms/xaml/xaml-basics/index.md)を参照してください。

### <a name="binding-cells"></a>セルのバインド

内のオブジェクトのプロパティにバインドできるプロパティのセル (セルの子)、`ItemsSource`します。 たとえば、を`ListView`使用して従業員の一覧を表示できます。

Employee クラス:

```csharp
public class Employee
{
    public string DisplayName {get; set;}
}
```

が作成され、 `ListView` `ItemsSource`として設定され、リストにデータが設定されます。 `ObservableCollection<Employee>`

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
> `ObservableCollection`はスレッドセーフではありません。 を変更すると、変更を行ったのと同じスレッドで UI の更新が発生します。 `ObservableCollection` スレッドがプライマリ UI スレッドでない場合は、例外が発生します。

次のスニペットを示しています、`ListView`従業員の一覧にバインドします。

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

この XAML の例で`ContentPage`は、を`ListView`含むを定義します。 データ ソース、`ListView`経由で設定されている、`ItemsSource`属性。 の`ItemsSource`各行のレイアウトは、 `ListView.ItemTemplate`要素内で定義されます。 この結果、次のスクリーンショットが表示されます。

![](data-and-databinding-images/bound-data.png "データ バインディングを使用して ListView")

### <a name="binding-selecteditem"></a>SelectedItem にバインド

選択項目にバインドしたい多くの場合、`ListView`ではなく、変更に応答するイベント ハンドラーを使用します。 これを行うには XAML に、バインド、`SelectedItem`プロパティ。

```xaml
<ListView x:Name="listView"
          SelectedItem="{Binding Source={x:Reference SomeLabel},
          Path=Text}">
 …
</ListView>
```

`listView`が文字列`SomeLabel`のリストであると仮定すると`SelectedItem`、プロパティがにバインドされます。`Text` `ItemsSource`

## <a name="related-links"></a>関連リンク

- [2 つの方法 (サンプル) をバインド](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
