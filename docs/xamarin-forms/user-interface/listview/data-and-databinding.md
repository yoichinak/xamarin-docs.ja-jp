---
title: ListView のデータ ソース
description: この記事では、データ、Xamarin.Forms の ListView を設定する方法と、ListView でのデータ バインディングを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/30/2018
ms.openlocfilehash: 4f80682c5d8c4f5231fbdd2e081fdcf7962aa969
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103726"
---
# <a name="listview-data-sources"></a>ListView のデータ ソース

A [ `ListView` ](xref:Xamarin.Forms.ListView)データのリストを表示するためです。 データ、私たちが選択した項目にバインドする方法と、ListView の設定について説明します。

- **[設定の ItemsSource](#ItemsSource)**  &ndash;単純なリストまたは配列を使用します。
- **[データ バインディング](#Data_Binding)** &ndash;モデルと、ListView の間の関係を確立します。 バインディングは、MVVM パターンに最適です。

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

同等の c# コードに示します。

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

//monochrome will not appear in the list because it was added
//after the list was populated.
listView.ItemsSource.Add("monochrome");
```

![](data-and-databinding-images/itemssource-simple.png "ListView 文字列の一覧を表示します。")

上記の方法が設定されます、`ListView`文字列のリスト。 既定では、`ListView`が呼び出す`ToString`で結果を表示し、`TextCell`行ごとにします。 データの表示方法をカスタマイズするを参照してください。[セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)します。

`ItemsSource`コンテンツは、基になるリストまたは配列の変更としては更新されません、配列に送信されました。 使用する必要があります、ListView の項目が追加、削除、および基になるリストの変更に応じて自動的に更新する場合は、`ObservableCollection`します。 [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1) 定義されている`System.Collections.ObjectModel`が同様と`List`に通知する点を除いて、`ListView`すべての変更。

```csharp
ObservableCollection<Employees> employeeList = new ObservableCollection<Employess>();
listView.ItemsSource = employeeList;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employeeList.Add(new Employee(){ DisplayName="Mr. Mono"});
```

<a name="Data_Binding" />

## <a name="data-binding"></a>データ バインディング
データ バインディングは、ユーザー インターフェイス オブジェクトのプロパティを ViewModel でクラスなど、いくつかの CLR オブジェクトのプロパティにバインドする「グルー」です。 データ バインディングは、多くの退屈な定型コードを置き換えることで、ユーザー インターフェイスの開発が簡略化されますので便利です。

データ バインディングは、バインドされた値が変更、オブジェクトを同期維持することで動作します。 コントロールの値が変更されるたびにイベント ハンドラーを記述するのではなく、バインドを確立し、ViewModel でバインディングを有効にします。

データ バインディングの詳細については、次を参照してください。[データ バインディングの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)4 つの一部では、[記事シリーズを Xamarin.Forms XAML の基礎](~/xamarin-forms/xaml/xaml-basics/index.md)します。

### <a name="binding-cells"></a>セルのバインド
内のオブジェクトのプロパティにバインドできるプロパティのセル (セルの子)、`ItemsSource`します。 たとえば、ListView を使用して、従業員の一覧を提供する可能性があります。

Employee クラス:

```csharp
public class Employee{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>` 作成され、設定、`ListView`の`ItemsSource`:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public EmployeeListPage()
{
  //defined in XAML to follow
  EmployeeView.ItemsSource = employees;
  ...
}
```

一覧には、データが設定されます。

```csharp
public EmployeeListPage()
{
  ...
  employees.Add(new Employee{ DisplayName="Rob Finnerty"});
  employees.Add(new Employee{ DisplayName="Bill Wrestler"});
  employees.Add(new Employee{ DisplayName="Dr. Geri-Beth Hooper"});
  employees.Add(new Employee{ DisplayName="Dr. Keith Joyce-Purdy"});
  employees.Add(new Employee{ DisplayName="Sheri Spruce"});
  employees.Add(new Employee{ DisplayName="Burt Indybrick"});
}
```

次のスニペットを示しています、`ListView`従業員の一覧にバインドします。

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
Title="Employee List">
  <ListView x:Name="EmployeeView">
    <ListView.ItemTemplate>
      <DataTemplate>
        <TextCell Text="{Binding DisplayName}" />
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

XAML でバインドされている可能性がありますが、バインドされたわかりやすいように、コードでのセットアップに注意してください。

XAML の前のビットを定義、`ContentPage`を格納している、`ListView`します。 データ ソース、`ListView`経由で設定されている、`ItemsSource`属性。 内の各行のレイアウト、`ItemsSource`内で定義されて、`ListView.ItemTemplate`要素。

これは、結果です。

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

想定`listView`の`ItemsSource`文字列のリストは、 `SomeLabel` text プロパティにバインドする必要があります、 `SelectedItem`。

## <a name="related-links"></a>関連リンク

- [2 つの方法 (サンプル) をバインド](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
