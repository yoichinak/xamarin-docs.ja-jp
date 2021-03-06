---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'WPF と Xamarin. Forms: 類似点 & 相違点'
description: このドキュメントでは、WPF と Xamarin. Forms を比較して比較します。 コントロールテンプレート、XAML、バインドインフラストラクチャ、データテンプレート、ItemsControl、UserControl、ナビゲーション、および URL ナビゲーションについて説明します。
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
ms.openlocfilehash: 9c69449f88f9c237b5075967c89ff7ff3b6fb57a
ms.sourcegitcommit: 52fb214c0e0243587d4e9ad9306b75e92a8cc8b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76940795"
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF と Xamarin. Forms: 類似点 & 相違点

## <a name="control-templates"></a>コントロール テンプレート

WPF はコントロール*テンプレート*の概念をサポートしており、コントロール (`Button`、`ListBox`など) の視覚化手順を提供します。 前述のように、Xamarin は、ネイティブプラットフォーム (iOS、Android など) と対話するための具象_レンダリング_クラスを使用して、コントロールを視覚化します。

ただし、Xamarin_のフォームには `ControlTemplate`_ の種類があり、`Page` オブジェクトのテーマに使用されます。 一貫したコンテンツを提供する `Page` の定義が用意されていますが、ページのユーザーは、色やフォントなどを変更できます。また、要素を追加して、アプリケーションに固有のものにすることもできます。

一般的な用途としては、認証ダイアログやプロンプトなどがあり、標準化されたものを提供しますが、アプリ内でカスタマイズできるようになります。 このサポートの一環として、WPF という名前付きコントロールがよく使用されます。

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

ただし、これらの機能が Xamarin. Forms で同じ目的を果たして_いない_ことを理解しておくことが重要です。 この機能の詳細については、[ドキュメントページ](~/xamarin-forms/app-fundamentals/templates/control-template.md)を参照してください。

## <a name="xaml"></a>XAML

XAML は、WPF および Xamarin. Forms の宣言型マークアップ言語として使用されます。 ほとんどの場合、構文は同じです。主な違いは、XAML グラフによって定義または作成されるオブジェクトです。

- Xamarin. Forms は、 [XAML 2009 仕様](/dotnet/framework/xaml-services/xaml-2009-language-features/)をサポートしています。これにより、`string`s や `int`などのデータを定義したり、ジェネリック型を定義したり、コンストラクターに引数を渡したりすることが容易になります。

- 現時点では、WPF が `XamlReader`と同様に、XAML を動的に読み込む方法はありません。 ただし、 [NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/)でも同じ基本的な機能を利用できます。

### <a name="markup-extensions"></a>マークアップ拡張機能

Xamarin は、マークアップ拡張機能を使用した XAML の拡張をサポートしています (WPF など)。 すぐに使える基本的な構成要素は次のとおりです。

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

さらに、XAML 2009 仕様の `{x:Reference}`、および Xamarin でサポートされている特殊なバージョンの `ControlTemplate` に使用される `{TemplateBinding}` マークアップ拡張機能も含まれています。

> [!WARNING]
> 同じ名前を持つ場合でも、`ControlTemplate` のサポートは同じではありません。

Xamarin はカスタムマークアップ拡張機能もサポートしていますが、実装は少し異なります。 WPF では、`MarkupExtension`-抽象基本クラスから派生する必要があります。 Xamarin. Forms では、より柔軟なインターフェイス `IMarkupExtension` または `IMarkupExtension<T>` に置き換えられます。

WPF と同じように、1つの必須メソッドは、マークアップ拡張機能から値を返すための `ProvideValue` メソッドです。

## <a name="binding-infrastructure"></a>バインドインフラストラクチャ

主要な概念の1つは、ビジュアルプロパティを .NET データプロパティに接続するためのデータバインディングインフラストラクチャです。 これにより、MVVM などのアーキテクチャパターンが有効になります。 基本的な設計は同じであり、バインド可能な基底クラスの[Bindableobject](xref:Xamarin.Forms.BindableObject)を持っています。 WPF では、これは[DependencyObject](xref:System.Windows.DependencyObject)クラスです。 この基底クラスは、データバインディングでターゲットとして使用されるすべてのオブジェクトのルート先祖として使用されます。 派生クラスは、プロパティ値のバッキングストレージとして機能する[Bindableproperty](xref:Xamarin.Forms.BindableProperty)オブジェクトを公開します (これらは WPF で[DependencyProperty](xref:System.Windows.DependencyProperty)オブジェクトとして定義されています)。

### <a name="defining-bindable-properties"></a>バインド可能なプロパティを定義します。

Xamarin. Forms のバインド可能なプロパティの定義は、WPF と同じです。

1. オブジェクトは `BindableObject`から派生しなければなりません。
2. プロパティのバッキングストレージキーを定義するために宣言 `BindableProperty` 型のパブリック静的フィールドが必要です。
3. `GetValue` と `SetValue` を使用してプロパティ値を取得および変更するパブリックインスタンスプロパティラッパーが必要です。

完全な例については、「 [Xamarin. Forms でのバインド](~/xamarin-forms/xaml/bindable-properties.md)可能なプロパティ」を参照してください。

### <a name="attached-properties"></a>添付プロパティ

添付プロパティは、バインド可能なプロパティのサブセットであり、WPF と同じように動作します。 主な違いは、この場合はプロパティラッパーが省略され、所有クラスの静的な get/set メソッドのセットに置き換えられることです。 詳細については、「 [Xamarin. Forms の添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)」を参照してください。

### <a name="using-the-binding-engine"></a>バインディングエンジンの使用

バインディングエンジンを使用するためのプロセスは、WPF と同じです。 これは、ソースオブジェクト (任意の .NET 型) に関連付けられた `Binding` オブジェクトを作成し、省略可能なプロパティ値 (省略した場合、WPF と同様に、ソースオブジェクトをプロパティ自体として扱います) によって分離コードで使用できます。 その後、任意の `BindableObject` で `SetBinding` を使用して、バインドを `BindableProperty`に関連付けることができます。

または、`BindingExtension`を使用して、XAML でバインド関係を定義することもできます。 WPF の拡張機能と同じ基本値を持ちます。

バインディングのサポートとエンジンは、WPF よりも Silverlight の実装に似ています。 Xamarin に実装されていないいくつかの機能がありません。

- バインドには、次の機能はサポートされていません。
  - BindingGroupName
  - ビン Dsdirecttosource
  - IsAsync
  - MultiBinding
  - NotifyOnSourceUpdated
  - NotifyOnTargetUpdated
  - NotifyOnValidationError
  - UpdateSourceTrigger
  - アップデート Ourceexceptionfilter
  - System.windows.data.binding.validatesondataerrors
  - ValidatesOnExceptions
  - ValidationRules コレクション
  - XPath
  - XmlNamespaceManager

#### <a name="relativesource"></a>RelativeSource

`RelativeSource` バインドはサポートされていません。 WPF では、XAML で定義されている他のビジュアル要素にバインドできます。 Xamarin. Forms では、この同じ機能を `{x:Reference}` マークアップ拡張機能を使用して実現できます。 たとえば、テキストプロパティを持つ "otherControl" という名前のコントロールがあるとします。このコントロールには、次のようにバインドできます。

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

`{RelativeSource Self}` 機能にも同じ機能を使用できます。 ただし、先祖を型 (`{RelativeSource FindAncestor}`) で検索することはサポートされていません。

#### <a name="binding-context"></a>バインドコンテキスト

WPF では、既定のバインディングソースを再設定する `DataContext` のプロパティ値を定義できます。 バインディングのソースが定義されていない場合は、このプロパティ値が使用されます。 値は、ビジュアルツリーの下位に継承されます。これにより、より高いレベルで定義し、子が使用できるようになります。

全容では、この機能は同じですが、プロパティ名は `BindingContext`です。

#### <a name="value-converters"></a>値コンバーター

値コンバーターは、WPF で完全にサポートされています。 WPF と同様です。 同じインターフェイス図形が使用されますが、Xamarin. Forms には `Xamarin.Forms` 名前空間で定義されたインターフェイスがあります。

### <a name="model-view-viewmodel"></a>モデルビュー-ビューモデル

MVVM は、WPF と Xamarin. Forms の両方で完全にサポートされています。

WPF に組み込まれている `RoutedCommand` は、使用されることがあります。Xamarin. Forms には、`ICommand` インターフェイス定義を超えた組み込みのコマンドのサポートがありません。 MVVM を実装するために必要な基本クラスを追加するために、さまざまな MVVM フレームワークを含めることができます。

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged と INotifyCollectionChanged

どちらのインターフェイスも、Xamarin. フォームバインドで完全にサポートされています。 多くの XAML ベースのフレームワークとは異なり、プロパティの変更通知は、Xamarin. フォーム (WPF と同様) のバックグラウンドスレッドで発生する可能性があり、バインドエンジンは UI スレッドに適切に遷移します。

また、どちらの環境でも `SynchronziationContext` がサポートされ、適切なスレッドマーシャリングを行うために /`await` `async`ます。 WPF には、すべてのビジュアル要素に `Dispatcher` クラスが含まれています。 Xamarin. Forms には、使用できる静的メソッド `Device.BeginInvokeOnMainThread` があります (ただし、クロスプラットフォームコーディングには `SynchronizationContext` が優先されます)。

- Xamarin. フォームには、コレクションの変更通知をサポートする `ObservableCollection<T>` が含まれています。
- `BindingBase.EnableCollectionSynchronization` を使用して、コレクションのスレッド間の更新を有効にすることができます。 API は WPF のバリエーションと若干異なります。[詳細について](xref:Xamarin.Forms.BindingBase.EnableCollectionSynchronization*)は、ドキュメントを確認してください。

## <a name="data-templates"></a>データ テンプレート

データテンプレートは、`ListView` の行 (セル) のレンダリングをカスタマイズするために、Xamarin. Forms でサポートされています。 すべてのコンテンツ指向コントロールで `DataTemplate`s を利用できる WPF とは異なり、現在のところ、Xamarin. フォームでは `ListView`にのみ使用されます。 テンプレート定義は、インラインで定義できます (`ItemTemplate` プロパティに割り当てられます)。また、`ResourceDictionary`内のリソースとして定義することもできます。

また、これらのユーザーは、WPF に対応する柔軟性はあまりありません。

1. `DataTemplate` のルート要素は、_常に_`ViewCell` オブジェクトである必要があります。
2. データトリガーはデータテンプレートで完全にサポートされますが、トリガーが関連付けられているプロパティの型を示す `DataType` プロパティを含める必要があります。
3. `DataTemplateSelector` もサポートされていますが、`DataTemplate` から派生しているため、`ItemTemplate` プロパティに直接割り当てられます (WPF では `ItemTemplateSelector`)。

## <a name="itemscontrol"></a>System.windows.controls.itemscontrol>

Xamarin. Forms の `ItemsControl` に相当するものはありません。ただし、 [Xamarin. フォーム用にカスタムのもの](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs)があります。

## <a name="user-controls"></a>ユーザー コントロール

WPF では、`UserControl`s を使用して、動作が関連付けられている UI のセクションを提供します。 Xamarin. Forms では、同じ目的のために `ContentView` を使用します。 どちらも XAML にバインドとインクルードをサポートしています。

## <a name="navigation"></a>Navigation

WPF には、"ブラウザーのような" ナビゲーション機能を提供するために使用できる、あまり使用されない `NavigationService` が含まれています。 ほとんどのアプリではこれを扱うのではなく、別の `Window` 要素、またはウィンドウの異なるセクションを使用してデータを表示していました。

電話デバイスでは、多くの場合、さまざまな_画面_がソリューションであり、そのため、Xamarin. フォームにはいくつかの形式のナビゲーションがサポートされています。

| ナビゲーションスタイル | ページの種類 |
|--- |--- |
|スタックベース (プッシュ/pop)|NavigationPage|
|マスター/詳細|MasterDetailPage|
|タブ|TabbedPage|
|左または右にスワイプ|CarouselView|

`NavigationPage` は最も一般的な方法です。すべてのページには、ナビゲーションスタックに対してページをプッシュまたはポップするために使用できる `Navigation` プロパティがあります。 これは、WPF の `NavigationService` と最も近いものです。

### <a name="url-navigation"></a>URL のナビゲーション

WPF は、デスクトップ指向のテクノロジであり、スタートアップ動作を直接実行するためのコマンドラインパラメーターを受け入れることができます。 Xamarin では、[ディープ URL リンク](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/)を使用して、スタートアップ時にページにジャンプできます。
