---
title: ListView のデータ ソース
description: データを含む、ListView を設定する方法を説明します。
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 6f0553f2887d4acb1015da71395be3f316e9acee
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="listview-data-sources"></a>ListView のデータ ソース

ListView がデータの一覧を表示するために使用します。 データ、および選択した項目にバインドできる方法で ListView を設定することについて説明します。

- **[ItemsSource を設定](#ItemsSource)** &ndash;単純なリストまたは配列を使用します。
- **[データ バインディング](#Data_Binding)** &ndash;モデルと ListView の間にリレーションシップを確立します。 バインディングは、MVVM パターンに最適です。

## <a name="itemssource"></a>ItemsSource
使用してデータのリスト ビューが表示されます、`ItemsSource`プロパティを実装するコレクションを受け入れることができる`IEnumerable`です。 設定する方法が最も簡単な`ListView`文字列の配列を使用してが含まれます。

```csharp
var listView = new ListView();
listView.ItemsSource = new string[]{
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

上記の方法が表示されます、`ListView`文字列のリストを使用します。 既定では、`ListView`が呼び出す`ToString`で結果を表示して、`TextCell`行ごとにします。 データの表示方法をカスタマイズするを参照してください。[セルの外観](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md)です。

`ItemsSource`が送信された、配列に基になるリストまたは配列の変化に応じて、コンテンツは更新されません。 使用する必要があります、ListView 項目が追加、削除、および基になるリストの変更に応じて自動的に更新する場合は、`ObservableCollection`です。 [`ObservableCollection`](https://developer.xamarin.com/api/type/System.Collections.ObjectModel.ObservableCollection%3CT%3E/) 定義された`System.Collections.ObjectModel`とまったく同様に、`List`に通知する点を除いて、`ListView`すべての変更。

```csharp
ObservableCollection<Employees> employeeList = new ObservableCollection<Employess>();
listView.ItemsSource = employeeList;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employeeList.Add(new Employee(){ DisplayName="Mr. Mono"});
```

<a name="Data_Binding" />

## <a name="data-binding"></a>データ バインディング
データ バインディングは、ユーザー インターフェイス オブジェクトのプロパティを ViewModel 内のクラスなど、いくつかの CLR オブジェクトのプロパティにバインドする「グルー」です。 データ バインディングは、多くの退屈な定型コードを置き換えることでユーザー インターフェイスの開発が簡略化されるので便利です。

データ バインディングは、バインドされている値を変更すると、それらのオブジェクトの同期を保つによって機能します。 コントロールの値が変更されるたびにイベント ハンドラーを記述する代わりに、バインドを確立して、ViewModel でバインディングを有効にします。

データ バインディングの詳細については、次を参照してください。[データのバインドの基礎](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)4 つの一部では、 [Xamarin.Forms XAML 基礎シリーズの記事](~/xamarin-forms/xaml/xaml-basics/index.md)です。

### <a name="binding-cells"></a>セルのバインド
内のオブジェクトのプロパティにバインドできるセルとその子セルの) のプロパティ、`ItemsSource`です。 たとえば、ListView を使用して、イメージと従業員の一覧を提供する可能性があります。

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

一覧には、データが表示されます。

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

次のスニペット、`ListView`従業員の一覧にバインドします。

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

XAML の以前のビットを定義、`ContentPage`を格納している、`ListView`です。 データ ソース、`ListView`経由で設定されている、`ItemsSource`属性。 内の各行のレイアウト、`ItemsSource`内で定義された、`ListView.ItemTemplate`要素。

これは、結果です。

![](data-and-databinding-images/bound-data.png "データ バインディングを使用する ListView")

### <a name="binding-selecteditem"></a>SelectedItem のバインド

多くの場合、選択した項目のバインドにしておく、`ListView`ではなく、変更に応答するイベント ハンドラーを使用します。 これを行う XAML で、バインド、`SelectedItem`プロパティ。

```xaml
<ListView x:Name="listView"
 SelectedItem="{Binding Source={x:Reference SomeLabel},
 Path=Text}">
 …
</ListView>
```

仮定すると`listView`の`ItemsSource`、文字列の一覧を示します`SomeLabel`にバインドされている、text プロパティを持つ、`SelectedItem`です。



## <a name="related-links"></a>関連リンク

- [双方向バインディング (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
- [1.4 のリリース ノート](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [1.3 リリース ノート](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
