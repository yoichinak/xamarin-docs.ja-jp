---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'WPF とします。Xamarin.Forms: 類似点と相違点'
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 21ffca65ee72308d1340a1db43471228b2adbe91
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF とします。Xamarin.Forms: 類似点と相違点

## <a name="control-templates"></a>コントロール テンプレート

WPF の概念をサポートしている*コントロール テンプレート*コントロールの視覚エフェクトの指示を提供する (`Button`、 `ListBox`, などです。)。 Xamarin.Forms は具象型を使用する前述のように、_レンダリング_このコントロールを視覚化するにはネイティブ プラットフォーム (iOS、Android など) とやり取りするクラス。

ただし、Xamarin.Forms_は_が、`ControlTemplate`の種類 - テーマの使用される`Page`オブジェクト。 定義を提供する`Page`を一貫性のある内容を提供しますが、色やフォントなどの変更を追加し、アプリケーションを一意にする要素をページのユーザーに許可します。

この一般的な使用法とは、認証ダイアログ、画面の指示など、アプリ内のカスタマイズ可能な標準化ですが themable ページのルック アンド フィールを提供することです。 このサポートの一環として、多くの使い慣れた WPF という名前のコントロールが使用されます。

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

これらがあることを確認することが重要ですが_いない_Xamarin.Forms で同じ目的のサービスを提供します。 この機能の詳細については、チェック アウト、[ドキュメント ページ](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)です。

## <a name="xaml"></a>XAML

XAML は、WPF と Xamarin.Forms の宣言型マークアップ言語として使用されます。 ほとんどの場合、構文は同じです。-主な違いは、XAML をグラフで定義されている作成されるオブジェクト。

- Xamarin.Forms をサポートしている、 [XAML 2009 の仕様](/dotnet/framework/xaml-services/xaml-2009-language-features/); データを定義するなどが簡単になります。 `string`s、`int`などもとして定義するジェネリック型、およびコンス トラクターに渡す引数。

- 方法はありません現在 dyanmically 読み込み XAML を WPF のように`XamlReader`です。 使用して同じ基本的な機能を取得することができます、 [NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/)もします。

### <a name="markup-extensions"></a>マークアップ拡張機能

Xamarin.Forms は XAML の WPF と同様に、マークアップ拡張機能を通じて拡張をサポートします。 既定で同じ基本的な構成要素があります。

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

また、`{x:Reference}`の XAML 2009 の仕様と`{TemplateBinding}`の特殊化バージョンに使用されるマークアップ拡張機能`ControlTemplate`Xamarin.Forms でサポートされています。

> [!WARNING]
> `ControlTemplate`サポート、同じではありません - がある場合でも、同じ名前です。

Xamarin.Forms がカスタム マークアップ拡張機能だけでなくをサポートしますが、実装がわずかに異なります。 WPF から派生する必要があります`MarkupExtension`の抽象基本クラスです。 Xamarin.Forms ではインターフェイスに置き換えは`IMarkupExtension`または`IMarkupExtension<T>`はより柔軟です。

1 つの必要なメソッドは、WPF と同じように、`ProvideValue`マークアップ拡張機能の値を返します。

## <a name="binding-infrastructure"></a>バインド インフラストラクチャ

引き継が主要な概念の 1 つは、データ バインド インフラストラクチャ visual プロパティ .NET データ プロパティを接続します。 これにより、アーキテクチャ パターン MVVM などです。 基本的な設計と同じ - バインド可能な基本クラスがある[BindableObject](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)、これは、WPF では、 [DependencyObject](https://msdn.microsoft.com/en-us/library/system.windows.dependencyobject(v=vs.110).aspx)クラスです。 この基本クラスは、データ バインディングのターゲットとして参加するすべてのオブジェクトの先祖をルートとして使用されます。 派生クラスを公開し、 [BindableProperty](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)バッキング ストレージのプロパティの値として機能するオブジェクト (として定義される[DependencyProperty](https://msdn.microsoft.com/library/system.windows.dependencyproperty(v=vs.110).aspx) WPF 内のオブジェクト)。

### <a name="defining-bindable-properties"></a>バインド可能なプロパティを定義します。

Xamarin.Forms では、バインド可能なプロパティの定義は、WPF と同じです。
1. オブジェクトから派生しなければなりません`BindableObject`です。
2. 型のパブリックな静的フィールドである必要があります`BindableProperty`プロパティのバッキング ストレージのキーを定義するために宣言します。
3. パブリック インスタンス プロパティのラッパーを使用する必要があります`GetValue`と`SetValue`を取得し、プロパティ値を変更します。

完全な例では、次を参照してください。 [Xamarin.Forms でバインド可能なプロパティ](~/xamarin-forms/xaml/bindable-properties.md)です。

### <a name="attached-properties"></a>添付プロパティ

添付プロパティは、バインド可能なプロパティのサブセットされ WPF では同じようになります。 主な違いでありことプロパティのラッパー省略したをここでは、所有クラスで静的 get と set メソッドのセットに置き換えられます。 参照してください[Xamarin.Forms で接続されているプロパティ](~/xamarin-forms/xaml/attached-properties.md)詳細についてはします。

### <a name="using-the-binding-engine"></a>バインディング エンジンを使用します。

バインディング エンジンを使用するプロセスは、WPF では同じです。 分離コードで作成することで利用できますが、`Binding`オブジェクトは、ソース オブジェクト (すべての .NET 型) と省略可能なプロパティ値に関連付けられています (かどうか省略した、ソース オブジェクトとして処理自体のプロパティと同じように WPF)。 使用してできます`SetBinding`いずれかで`BindableObject`をバインドに関連付ける、`BindableProperty`です。

または、XAML を使用してバインド リレーションシップを定義すれば、`BindingExtension`です。 WPF では、拡張機能と同じ基本的な値があります。

バインディングのサポートとエンジンは、WPF を Silverlight 実装より類似します。 Xamarin.Forms で実装されていなかったをいくつか不足している機能。

- 次の機能のバインディングでサポートされていません。
    - BindingGroupName
    - BindsDirectlyToSource
    - IsAsync
    - MultiBinding
    - NotifyOnSourceUpdated
    - NotifyOnTargetUpdated
    - NotifyOnValidationError
    - UpdateSourceTrigger
    - UpdateSourceExceptionFilter
    - ValidatesOnDataErrors
    - ValidatesOnExceptions
    - ValidationRules コレクション
    - XPath
    - XmlNamespaceManager
- `Binding.Mode` サポートしていません`OneTime`、代わりにだけ使用`OneWay`です。

#### <a name="relativesource"></a>RelativeSource

サポートされていません`RelativeSource`バインドします。 WPF では、XAML で定義されている他のビジュアル要素にバインドする実現します。 Xamarin.Forms でこの機能を実現できますを使用して、`{x:Reference}`マークアップ拡張機能です。 たとえば、テキスト プロパティを持つ名前 "otherControl"を持つコントロールがあると仮定した場合、バインドできる、次のように。

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

同じ機能を使用できる、`{RelativeSource Self}`機能します。 ただしはサポートされていません型での先祖を見つけるため (`{RelativeSource FindAncestor}`)。

#### <a name="binding-context"></a>バインド コンテキスト

WPF では、定義することができます、`DataContext`どの reprents ソースにバインドする既定のプロパティ値します。 バインディングのソースが定義されていない場合は、このプロパティ値が使用されます。 値は、ビジュアル ツリーより高いレベルで定義することができると、子を使用してダウン継承されます。

Xamarin.Forms でこの同じ機能 avaialable が、プロパティ名が`BindingContext`です。

#### <a name="value-converters"></a>値コンバーター

値コンバーターは、WPF と同じように、Xamarin.Forms で完全にサポートされます。 同じ [インターフェイス] 図形を使用し、Xamarin.Forms がで定義されているインターフェイスが、`Xamarin.Forms`名前空間。

### <a name="model-view-viewmodel"></a>モデル View-viewmodel

MVVM は WPF と Xamarin.Forms の両方で完全にサポートします。

WPF 組み込まれておりで`RoutedCommand`使用される場合があります。Xamarin.Forms はを超えて組み込みコマンド実行のサポートを持たず、`ICommand`インターフェイス定義です。 MVVM を実装するために必要な基本クラスを追加する MVVM フレームワークのさまざまなを含めることができます。

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged と INotifyCollectionChanged

両方のインターフェイスは、Xamarin.Forms バインドで完全にサポートされます。 多くの XAML ベースのフレームワークとは異なり Xamarin.Forms (WPF) と同様に、バック グラウンド スレッドでプロパティの変更通知を発生させることができ、バインディング エンジンは、UI スレッドに移行し正しくです。

また、両方の環境をサポート`SynchronziationContext`と`async` / `await`を適切なスレッドにマーシャ リングを行うには。 WPF が含まれています、`Dispatcher`クラス Xamarin.Forms が静的メソッドを持つすべてのビジュアル要素の`Device.BeginInvokeOnMainThread`することもできます (が`SynchronizationContext`はクロスプラット フォームのコーディングの推奨)。

- Xamarin.Forms を含む、`ObservableCollection<T>`コレクションの変更通知をサポートします。
- 使用することができます`BindingBase.EnableCollectionSynchronization`コレクションのスレッド間の更新を有効にします。 API とは少し異なる WPF バリエーション[docs 使用方法の詳細を確認して](https://developer.xamarin.com/api/member/Xamarin.Forms.BindingBase.EnableCollectionSynchronization/)です。

## <a name="data-templates"></a>データ テンプレート

データ テンプレートの表示をカスタマイズする Xamarin.Forms ではサポートされて、`ListView`行 (セル)。 利用できる WPF とは異なり`DataTemplate`任意コンテンツ指向コントロールは、Xamarin.Forms 現在 s のみが使用には、`ListView`です。 テンプレートの定義はインラインで定義することができます (に割り当てられている、`ItemTemplate`プロパティ)、または内のリソースとして、`ResourceDictionary`です。

さらはありません、WPF の対応すると、非常に柔軟です。

1. ルート要素、`DataTemplate`必要があります_常に_する、`ViewCell`オブジェクト。
2. データのトリガーは、データ テンプレートで完全にサポートが、含める必要があります、`DataType`トリガーに関連付けられているプロパティの型を示すプロパティです。
3. `DataTemplateSelector` サポートされていますから派生した`DataTemplate`し、そのために直接割り当てられただけ、`ItemTemplate`プロパティ (と`ItemTemplateSelector`WPF で)。

## <a name="itemscontrol"></a>ItemsControl

組み込みの分にはありません、 `ItemsControl` xamarin.forms; がある、 [Xamarin.Forms の使用可能な次のいずれかをカスタム](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs)です。

## <a name="user-controls"></a>ユーザー コントロール

WPF では、`UserControl`動作が関連付けられている UI のセクションを提供するために使用されます。 Xamarin.Forms では使用して、`ContentView`を同じ目的。 両方により、バインドと包含が XAML でサポートします。

## <a name="navigation"></a>ナビゲーション

WPF が含まれていますが、あまり使用されない`NavigationService`「ブラウザーのような」ナビゲーション機能を提供する使用可能性があります。 ただし、代わりに使用されるこのほとんどのアプリはたいした問題別`Window`要素、またはデータを表示するウィンドウのさまざまなセクションです。

Phone デバイスでさまざまな_画面_ソリューションは、多くの場合と Xamarin.Forms のナビゲーションの形式のいくつかサポートしていますので。

| ナビゲーション スタイル | ページの種類 |
|--- |--- |
|スタックに基づく (プッシュ/ポップ)|NavigationPage|
|マスター/詳細|MasterDetailPage|
|タブ|TabbedPage|
|左/右の方向にスワイプ|CarouselView|

`NavigationPage` 、最も一般的な方法は、すべてのページが、`Navigation`プッシュまたはポップのオンとオフ ナビゲーション スタックのページを使用できるプロパティです。 これは、最も近い分に、 `NavigationService` wpf が見つかりました。

### <a name="url-navigation"></a>URL ナビゲーション

WPF は、デスクトップ指向のテクノロジは、起動時の動作を指示するコマンド ライン パラメーターを受け取ることができます。 Xamarin.Forms を使用できる[URL のディープ リンク](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/)スタートアップ時に、ページにジャンプします。
