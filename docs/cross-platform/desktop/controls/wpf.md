---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: WPF とします。Xamarin.Forms:類似点と相違点
description: このドキュメントでは、比較し、Xamarin.Forms の WPF は対照的です。 これは、コントロール テンプレート、XAML、バインド インフラストラクチャ、データ テンプレート、ItemsControl、ユーザー コントロール、ナビゲーション、および URL のナビゲーションについて説明します。
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 990253cbd31ad79bc47f086dc5bd2b99233f2032
ms.sourcegitcommit: 64d6da88bb6ba222ab2decd2fdc8e95d377438a6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/18/2019
ms.locfileid: "58175240"
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF とします。Xamarin.Forms:類似点と相違点

## <a name="control-templates"></a>コントロール テンプレート

WPF の概念をサポートして*コントロール テンプレート*コントロールの視覚エフェクトの指示を提供する (`Button`、`ListBox`など。)。 Xamarin.Forms は具象型を使用して前述のように、_レンダリング_コントロールを視覚化するのには、ネイティブ プラットフォーム (iOS、Android など) と対話するこのクラス。

ただし、Xamarin.Forms_は_が、`ControlTemplate`の種類 - テーマの使用されて`Page`オブジェクト。 定義を提供します、`Page`を一貫性のあるコンテンツは、用意されていますが、色やフォントなどの変更も、アプリケーションを一意にする要素を追加して、ページのユーザーに許可します。

この一般的な使用法とは、アプリ内でカスタマイズ可能な標準化が themable ページのルック アンド フィールを提供して、認証ダイアログ プロンプトなどの点です。 このサポートの一環として、多くの使い慣れたという名前の WPF コントロールが使用されます。

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

これらがあることを知っておく必要が_いない_Xamarin.Forms で同じ目的のサービスを提供します。 この機能の詳細については、チェック アウト、[ドキュメント ページ](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md)します。

## <a name="xaml"></a>XAML

XAML は、WPF と Xamarin.Forms の宣言型マークアップ言語として使用されます。 ほとんどの場合、構文は同じです。-主な違いは、XAML のグラフで定義されている作成されるオブジェクト。

- Xamarin.Forms のサポート、 [XAML 2009 仕様](/dotnet/framework/xaml-services/xaml-2009-language-features/); などのデータを定義するが簡単になります。 `string`s、`int`などもとして定義するジェネリック型、およびコンス トラクターに引数を渡す。

- 現在を WPF が使用できるように、XAML を動的に読み込む方法はありません`XamlReader`します。 使用して同じ基本的な機能を取得できます、 [NuGet パッケージ](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/)場合。

### <a name="markup-extensions"></a>マークアップ拡張機能

Xamarin.Forms は XAML の WPF と同様に、マークアップ拡張機能を通じて拡張をサポートします。 既定で同じ基本的な構成要素があります。

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

また、 `{x:Reference}` XAML 2009 の仕様と`{TemplateBinding}`特殊化されたバージョンに使用されるマークアップ拡張機能`ControlTemplate`Xamarin.Forms でサポートされています。

> [!WARNING]
> `ControlTemplate`サポート、同じではありません - 場合でも、同じ名前にします。

Xamarin.Forms は、カスタム マークアップ拡張機能だけでなくをサポートしますが、実装は少し異なります。 WPF から派生する必要があります`MarkupExtension`の抽象基本クラス。 インターフェイスで交換する Xamarin.Forms で`IMarkupExtension`または`IMarkupExtension<T>`より柔軟であります。

1 つの必要なメソッドには、WPF と同様、`ProvideValue`マークアップ拡張機能の値を返します。

## <a name="binding-infrastructure"></a>バインド インフラストラクチャ

引き継がコア概念の 1 つは、.NET のデータ プロパティにビジュアルのプロパティを接続するデータ バインド インフラストラクチャです。 これにより、アーキテクチャのパターンを MVVM などができます。 基本のデザインは同じです - バインド可能な基底クラスを含んだりする[BindableObject](xref:Xamarin.Forms.BindableObject)、これは、WPF では、 [DependencyObject](xref:System.Windows.DependencyObject)クラス。 この基本クラスは、データ バインディングのターゲットとして参加するすべてのオブジェクトの先祖をルートとして使用されます。 派生クラスを公開し、 [BindableProperty](xref:Xamarin.Forms.BindableProperty)プロパティ値のバックアップ用ストレージとして機能するオブジェクト (として定義される[DependencyProperty](xref:System.Windows.DependencyProperty) WPF 内のオブジェクト)。

### <a name="defining-bindable-properties"></a>バインド可能なプロパティを定義します。

Xamarin.Forms のバインド可能なプロパティの定義では、WPF の場合と同じです。
1. オブジェクトがから派生する必要があります`BindableObject`します。
2. 型のパブリックな静的フィールドがある`BindableProperty`プロパティのバッキング ストレージ キーを定義する宣言します。
3. パブリック インスタンス プロパティのラッパーを使用する必要があります`GetValue`と`SetValue`を取得し、プロパティ値を変更します。

完全な例を参照してください。 [Xamarin.Forms でのバインド可能なプロパティ](~/xamarin-forms/xaml/bindable-properties.md)します。

### <a name="attached-properties"></a>添付プロパティ

添付プロパティは、バインド可能なプロパティのサブセットと WPF と同じように動作します。 主な違いは、プロパティのラッパーを省略したをここでは、所有元クラスの静的な取得/設定メソッドのセットに置き換えられますが。 参照してください[Xamarin.Forms でのプロパティのアタッチ](~/xamarin-forms/xaml/attached-properties.md)詳細についてはします。

### <a name="using-the-binding-engine"></a>バインディング エンジンを使用します。

バインディング エンジンを使用するためのプロセスでは、WPF では同じです。 作成すること、分離コードで使用できる、`Binding`ソース オブジェクト (任意の .NET 型) に省略可能なプロパティの値をオブジェクトが関連付けられている (かどうか省略したソース オブジェクトとして処理自体のプロパティと同じように WPF)。 使用することができます`SetBinding`いずれかで`BindableObject`をバインドに関連付ける、`BindableProperty`します。

また、XAML を使用して、バインドの関係を定義、`BindingExtension`します。 WPF では、拡張機能と同じ基本的な値があります。

バインドのサポートとエンジンは、WPF よりもより Silverlight の実装に似ています。 Xamarin.Forms では実装しませんでしたが、いくつか不足している機能があります。

- バインドでは、次の機能のサポートはありません。
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

#### <a name="relativesource"></a>RelativeSource

サポートされていません`RelativeSource`バインドします。 WPF では、XAML で定義されている他のビジュアル要素にバインドする実現します。 Xamarin.Forms では、この同じ機能を実現するを使用して、`{x:Reference}`マークアップ拡張機能。 たとえば、テキスト プロパティを持つ名前 "otherControl"を持つコントロールがあると仮定すると、ことができますにバインドします、このような。

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

同じ機能に使用できる、`{RelativeSource Self}`機能します。 ただし、種類別の先祖を検索するためのサポートされていません (`{RelativeSource FindAncestor}`)。

#### <a name="binding-context"></a>バインディング コンテキスト

WPF では、定義することができます、`DataContext`プロパティ値のどの reprents、既定のソースをバインドします。 バインディングのソースが定義されていない場合は、このプロパティの値が使用されます。 値より高いレベルで定義することができ、子によって使用は、ビジュアル ツリーの下位に継承されます。

Xamarin.Forms でこの同じ機能を利用可能なが、プロパティ名は`BindingContext`します。

#### <a name="value-converters"></a>値コンバーター

値コンバーターは、WPF と同様に、Xamarin.Forms で完全にサポートします。 同じインターフェイスの図形を使用するが Xamarin.Forms は、インターフェイスで定義されている、`Xamarin.Forms`名前空間。

### <a name="model-view-viewmodel"></a>モデル-ビュー-ビューモデル

MVVM は WPF と Xamarin.Forms の両方で完全にサポートされています。

WPF に組み込まれている`RoutedCommand`使用される場合があります。Xamarin.Forms を超える組み込みのコマンド実行のサポートを持たない、`ICommand`インターフェイス定義です。 さまざまな MVVM を実装するために必要な基本クラスを追加する MVVM フレームワークを含めることができます。

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged と INotifyCollectionChanged

両方のインターフェイスは、Xamarin.Forms のバインドで完全にサポートします。 多くの XAML ベースのフレームワークとは異なりプロパティの変更通知は、Xamarin.Forms (WPF) と同様に、バック グラウンド スレッドで発生することができ、バインディング エンジンから UI スレッドへの移行正しくします。

さらに、両方の環境サポート`SynchronziationContext`と`async` / `await`適切なスレッドにマーシャ リングを行う。 WPF が含まれています、`Dispatcher`クラス Xamarin.Forms が静的メソッドを持つすべてのビジュアル要素に`Device.BeginInvokeOnMainThread`することもできます (が`SynchronizationContext`クロスプラット フォームでのコーディングの勧め)。

- Xamarin.Forms を含む、`ObservableCollection<T>`コレクションの変更通知をサポートしています。
- 使用することができます`BindingBase.EnableCollectionSynchronization`コレクションのスレッド間の更新を有効にします。 API は WPF のバリエーションが若干異なる[は、ドキュメントの使用方法の詳細を確認してください。](xref:Xamarin.Forms.BindingBase.EnableCollectionSynchronization*)します。

## <a name="data-templates"></a>データ テンプレート

データ テンプレートの表示をカスタマイズする Xamarin.Forms ではサポートされて、`ListView`行 (セル)。 WPF を利用できるとは異なり`DataTemplate`コントロール コンテンツ指向、Xamarin.Forms を現在のそれらの使用のみ`ListView`します。 テンプレートの定義はインラインで定義することができます (に割り当てられている、`ItemTemplate`プロパティ)、または内のリソースとして、`ResourceDictionary`します。

さらに、非常に、WPF の対応するほどの柔軟性はありません。

1. ルート要素、`DataTemplate`する必要があります_常に_する、`ViewCell`オブジェクト。
2. データ トリガーは、データ テンプレートで完全にサポートされますが、含める必要があります、`DataType`トリガーが関連付けられているプロパティの型を示すプロパティです。
3. `DataTemplateSelector` サポートされていますから派生した`DataTemplate`し、そのために直接割り当てられただけ、`ItemTemplate`プロパティ (と`ItemTemplateSelector`WPF で)。

## <a name="itemscontrol"></a>ItemsControl

相当する組み込みはありません、 `ItemsControl` ; Xamarin.Forms では、[カスタムの 1 つの Xamarin.Forms こちらで入手できます](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs)。

## <a name="user-controls"></a>ユーザー コントロール

WPF では、`UserControl`動作が関連付けられている UI のセクションを提供するために使用されます。 Xamarin.Forms で使用して、`ContentView`を同じ目的。 XAML でバインドと包含をサポート両方。

## <a name="navigation"></a>ナビゲーション

WPF が含まれていますが、あまり使用されない`NavigationService`「ブラウザーのような」ナビゲーション機能を提供するが使用される可能性があります。 ただし、代わりに使用されるこのほとんどのアプリを割り当てませんでした異なる`Window`要素、またはデータを表示するウィンドウのさまざまなセクションです。

Phone デバイス、異なる_画面_ソリューションでは多くの場合と、Xamarin.Forms にいくつかの形式のナビゲーションのサポートが含まれていますので。

| ナビゲーション スタイル | ページの種類 |
|--- |--- |
|スタック ベース (プッシュ/ポップ)|NavigationPage|
|マスター/詳細|MasterDetailPage|
|タブ|TabbedPage|
|左/右の方向にスワイプ|CarouselView|

`NavigationPage` 、最も一般的なアプローチは、すべてのページが、`Navigation`プロパティ プッシュやポップをページで、ナビゲーション スタックからポップするために使用できます。 これは、最も近い相当する、 `NavigationService` wpf が見つかりました。

### <a name="url-navigation"></a>URL ナビゲーション

WPF は、デスクトップ指向のテクノロジは、起動の動作を指示するコマンド ライン パラメーターを受け入れることができます。 Xamarin.Forms を使用できる[URL のディープ リンク](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/)起動時に、ページにジャンプします。
