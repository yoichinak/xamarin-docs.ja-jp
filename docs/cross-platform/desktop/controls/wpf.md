---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: WPF とXamarin. フォーム:類似点 & 相違点
description: このドキュメントでは、WPF と Xamarin. Forms を比較して比較します。 コントロールテンプレート、XAML、バインドインフラストラクチャ、データテンプレート、ItemsControl、UserControl、ナビゲーション、および URL ナビゲーションについて説明します。
author: conceptdev
ms.author: crdun
ms.date: 04/26/2017
ms.openlocfilehash: d23b449382183b0385eac38c0b9205e48dbe0a34
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290405"
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF とXamarin. フォーム:類似点 & 相違点

## <a name="control-templates"></a>コントロール テンプレート

WPF では、コントロール (`Button`、 `ListBox`など) の視覚化手順を提供する*コントロールテンプレート*の概念をサポートしています。 前述のように、Xamarin は、ネイティブプラットフォーム (iOS、Android など) と対話するための具象_レンダリング_クラスを使用して、コントロールを視覚化します。

ただし、Xamarin 形式`ControlTemplate`に_は型があります_。これは、オブジェクト`Page`のテーマに使用されます。 このメソッドは、 `Page`一貫したコンテンツを提供するの定義を提供しますが、ページのユーザーが色やフォントなどを変更したり、要素を追加してアプリケーションに固有のものにしたりできるようにします。

一般的な用途としては、認証ダイアログやプロンプトなどがあり、標準化されたものを提供しますが、アプリ内でカスタマイズできるようになります。 このサポートの一環として、WPF という名前付きコントロールがよく使用されます。

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

ただし、これらの機能が Xamarin. Forms で同じ目的を果たして_いない_ことを理解しておくことが重要です。 この機能の詳細については、[ドキュメントページ](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)を参照してください。

## <a name="xaml"></a>XAML

XAML は、WPF および Xamarin. Forms の宣言型マークアップ言語として使用されます。 ほとんどの場合、構文は同じです。主な違いは、XAML グラフによって定義または作成されるオブジェクトです。

- Xamarin. Forms は、 [XAML 2009 仕様](/dotnet/framework/xaml-services/xaml-2009-language-features/)をサポートしています。これにより、 `string` `int`などのデータを簡単に定義できるだけでなく、ジェネリック型を定義し、引数をコンストラクターに渡すことができます。

- 現在、WPF と同様の`XamlReader`XAML を動的に読み込む方法はありません。 ただし、 [NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/)でも同じ基本的な機能を利用できます。

### <a name="markup-extensions"></a>マークアップ拡張機能

Xamarin は、マークアップ拡張機能を使用した XAML の拡張をサポートしています (WPF など)。 すぐに使える基本的な構成要素は次のとおりです。

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

さらに、XAML 2009 `{x:Reference}`仕様`{TemplateBinding}`と、Xamarin でサポートされているの`ControlTemplate`特別なバージョンで使用されるマークアップ拡張機能も含まれています。

> [!WARNING]
> 同じ`ControlTemplate`名前を持つ場合でも、サポートは同じではありません。

Xamarin はカスタムマークアップ拡張機能もサポートしていますが、実装は少し異なります。 WPF では、-抽象基本`MarkupExtension`クラスから派生する必要があります。 Xamarin. Forms では、インターフェイス`IMarkupExtension`または`IMarkupExtension<T>`より柔軟性があります。

WPF と同じように、1つの必須`ProvideValue`メソッドは、マークアップ拡張機能から値を返すメソッドです。

## <a name="binding-infrastructure"></a>バインドインフラストラクチャ

主要な概念の1つは、ビジュアルプロパティを .NET データプロパティに接続するためのデータバインディングインフラストラクチャです。 これにより、MVVM などのアーキテクチャパターンが有効になります。 基本的な設計は同じであり、バインド可能な基底クラスの[Bindableobject](xref:Xamarin.Forms.BindableObject)を持っています。 WPF では、これは[DependencyObject](xref:System.Windows.DependencyObject)クラスです。 この基底クラスは、データバインディングでターゲットとして使用されるすべてのオブジェクトのルート先祖として使用されます。 派生クラスは、プロパティ値のバッキングストレージとして機能する[Bindableproperty](xref:Xamarin.Forms.BindableProperty)オブジェクトを公開します (これらは WPF で[DependencyProperty](xref:System.Windows.DependencyProperty)オブジェクトとして定義されています)。

### <a name="defining-bindable-properties"></a>バインド可能なプロパティを定義します。

Xamarin. Forms のバインド可能なプロパティの定義は、WPF と同じです。
1. オブジェクトは、から`BindableObject`派生する必要があります。
2. プロパティのバッキングストレージキーを定義するため`BindableProperty`に、型のパブリック静的フィールドが宣言されている必要があります。
3. `GetValue` と`SetValue`を使用してプロパティ値を取得および変更するパブリックインスタンスプロパティラッパーが必要です。

完全な例については、「 [Xamarin. Forms でのバインド](~/xamarin-forms/xaml/bindable-properties.md)可能なプロパティ」を参照してください。

### <a name="attached-properties"></a>添付プロパティ

添付プロパティは、バインド可能なプロパティのサブセットであり、WPF と同じように動作します。 主な違いは、プロパティラッパーはこの場合はオプション、所有クラスでは静的な get/set メソッドのセットに置き換えることです。 詳細については、「 [Xamarin. Forms の添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)」を参照してください。

### <a name="using-the-binding-engine"></a>バインディングエンジンの使用

バインディングエンジンを使用するためのプロセスは、WPF と同じです。 これは、ソースオブジェクト (任意の .net 型) `Binding`に関連付けられたオブジェクトを作成し、オプションのプロパティ値 (オプションの場合、WPF と同様に、ソースオブジェクトをプロパティ自体として扱います) によって分離コードで利用できます。 その後、任意`SetBinding` `BindableObject`のでを使用して、 `BindableProperty`バインドをに関連付けることができます。

または、 `BindingExtension`を使用して、XAML でバインド関係を定義することもできます。 WPF の拡張機能と同じ基本値を持ちます。

バインディングのサポートとエンジンは、WPF よりも Silverlight の実装に似ています。 Xamarin に実装されていないいくつかの機能がありません。

- バインドには、次の機能はサポートされていません。
  - BindingGroupName
  - ビン Dsdirecttosource
  - IsAsync
  - MultiBinding
  - NotifyOnSourceUpdated
  - NotifyOnTargetUpdated
  - NotifyOnValidationError
  - System.windows.data.binding.updatesourcetrigger
  - アップデート Ourceexceptionfilter
  - System.windows.data.binding.validatesondataerrors
  - ValidatesOnExceptions
  - ValidationRules コレクション
  - XPath
  - XmlNamespaceManager

#### <a name="relativesource"></a>RelativeSource

バインドはサポートされ`RelativeSource`ていません。 WPF では、XAML で定義されている他のビジュアル要素にバインドできます。 Xamarin. Forms では、この同じ機能を`{x:Reference}`マークアップ拡張機能を使用して実現できます。 たとえば、テキストプロパティを持つ "otherControl" という名前のコントロールがあるとします。このコントロールには、次のようにバインドできます。

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

この`{RelativeSource Self}`機能に対しても同じ機能を使用できます。 ただし、先祖を型 (`{RelativeSource FindAncestor}`) で検索することはサポートされていません。

#### <a name="binding-context"></a>バインドコンテキスト

WPF では、既定のバインディング`DataContext`ソースを再設定するプロパティ値を定義できます。 バインディングのソースが定義されていない場合は、このプロパティ値が使用されます。 値は、ビジュアルツリーの下位に継承されます。これにより、より高いレベルで定義し、子が使用できるようになります。

Xamarin. Forms では、これと同じ機能が全容ますが、プロパティ`BindingContext`名はです。

#### <a name="value-converters"></a>値コンバーター

値コンバーターは、WPF で完全にサポートされています。 WPF と同様です。 同じインターフェイス図形が使用されますが、Xamarin. Forms には`Xamarin.Forms`名前空間で定義されたインターフェイスがあります。

### <a name="model-view-viewmodel"></a>モデルビュー-ビューモデル

MVVM は、WPF と Xamarin. Forms の両方で完全にサポートされています。

WPF に`RoutedCommand`は、使用されることがある組み込みのが含まれています。Xamarin. Forms には、インターフェイス定義を超えた組み込み`ICommand`のコマンドがサポートされていません。 MVVM を実装するために必要な基本クラスを追加するために、さまざまな MVVM フレームワークを含めることができます。

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged と INotifyCollectionChanged

どちらのインターフェイスも、Xamarin. フォームバインドで完全にサポートされています。 多くの XAML ベースのフレームワークとは異なり、プロパティの変更通知は、Xamarin. フォーム (WPF と同様) のバックグラウンドスレッドで発生する可能性があり、バインドエンジンは UI スレッドに適切に遷移します。

また、両方の環境で`SynchronziationContext`と`async` / `await`がサポートされ、適切なスレッドマーシャリングが実行されます。 WPF には`Dispatcher` 、すべてのビジュアル要素にクラスが含まれています`Device.BeginInvokeOnMainThread` 。 Xamarin. Forms には、 `SynchronizationContext`使用できる静的メソッドがあります (ただし、はクロスプラットフォームコーディングに適しています)。

- Xamarin. フォームには`ObservableCollection<T>` 、コレクションの変更通知をサポートするが含まれています。
- を使用`BindingBase.EnableCollectionSynchronization`して、コレクションのスレッド間の更新を有効にすることができます。 API は WPF のバリエーションと若干異なります。[詳細について](xref:Xamarin.Forms.BindingBase.EnableCollectionSynchronization*)は、ドキュメントを確認してください。

## <a name="data-templates"></a>データ テンプレート

データテンプレートは、行 (セル) のレンダリングをカスタマイズするため`ListView`に、Xamarin でサポートされています。 コンテンツ指向コントロールで s `DataTemplate`を利用できる WPF とは異なり、現在のところ、Xamarin. フォームは`ListView`にのみ使用されます。 テンプレート定義は、インラインで ( `ItemTemplate`プロパティに割り当てられた)、または`ResourceDictionary`内のリソースとして定義できます。

また、これらのユーザーは、WPF に対応する柔軟性はあまりありません。

1. の`DataTemplate`ルート要素は、 `ViewCell` _常に_オブジェクトである必要があります。
2. データトリガーはデータテンプレートで完全にサポートされています`DataType`が、トリガーが関連付けられているプロパティの型を示すプロパティを含める必要があります。
3. `DataTemplateSelector`もサポートされています`DataTemplate`が、はから派生している`ItemTemplate`ため、プロパティ ( `ItemTemplateSelector` WPF の場合は) に直接割り当てられます。

## <a name="itemscontrol"></a>System.windows.controls.itemscontrol>

Xamarin. forms には、 `ItemsControl`に相当する組み込みの機能はありませんが、 [xamarin 用にカスタムのもの](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs)があります。

## <a name="user-controls"></a>ユーザーコントロール

WPF では`UserControl`、s を使用して、関連する動作を持つ UI のセクションを提供します。 Xamarin. Forms では、 `ContentView`同じ目的でを使用します。 どちらも XAML にバインドとインクルードをサポートしています。

## <a name="navigation"></a>ナビゲーション

WPF には、" `NavigationService`ブラウザーのような" ナビゲーション機能を提供するために使用できる、あまり使用されないものが含まれています。 ほとんどのアプリではこれを扱うのでは`Window`なく、別の要素、またはウィンドウの異なるセクションを使用してデータを表示していました。

電話デバイスでは、多くの場合、さまざまな_画面_がソリューションであり、そのため、Xamarin. フォームにはいくつかの形式のナビゲーションがサポートされています。

| ナビゲーションスタイル | ページの種類 |
|--- |--- |
|スタックベース (プッシュ/pop)|NavigationPage|
|マスター/詳細|MasterDetailPage|
|タブ|TabbedPage|
|左または右にスワイプ|CarouselView|

は`NavigationPage` 、最も一般的な方法です。すべてのページに`Navigation`は、ナビゲーションスタックに対してページのプッシュまたはポップを行うために使用できるプロパティがあります。 これは、 `NavigationService` WPF で見つかったに最も近いです。

### <a name="url-navigation"></a>URL のナビゲーション

WPF は、デスクトップ指向のテクノロジであり、スタートアップ動作を直接実行するためのコマンドラインパラメーターを受け入れることができます。 Xamarin では、[ディープ URL リンク](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/)を使用して、スタートアップ時にページにジャンプできます。
