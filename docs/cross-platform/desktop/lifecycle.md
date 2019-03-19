---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: WPF とします。Xamarin.Forms アプリのライフサイクル
description: このドキュメントの類似点と Xamarin.Forms と WPF アプリケーションのアプリケーションのライフ サイクルの違いを比較します。 ビジュアル ツリー、グラフィックス、リソース、およびスタイルにも見えます。
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 5f157f2bbf36076e542a5f96b912cb1788a99052
ms.sourcegitcommit: 64d6da88bb6ba222ab2decd2fdc8e95d377438a6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/18/2019
ms.locfileid: "58175227"
---
# <a name="wpf-vs-xamarinforms-app-lifecycle"></a>WPF とします。Xamarin.Forms アプリのライフサイクル

Xamarin.Forms は、多くのする前に、特に WPF XAML ベースのフレームワークからの設計ガイダンスを受け取ります。 ただし、他の方法で以上逸脱しているが大幅を移行しようとしています。 ユーザーの注釈のポイントであることができます。 このドキュメントは、これらの問題のいくつかを特定し、Xamarin.Forms の WPF サポート技術情報をブリッジに可能なガイダンスを提供を試みます。

## <a name="app-lifecycle"></a>アプリのライフサイクル

WPF と Xamarin.Forms アプリケーションのライフ サイクルは似ています。 外部 (プラットフォーム) のコードで開始し、メソッド呼び出しによって UI を起動します。 違いは、Xamarin.Forms が常に初期化し、アプリの UI を作成し、プラットフォーム固有のアセンブリで起動することです。

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> `Main`メソッドは、既定では、自動生成されると、コードでは表示されません。

**Xamarin.Forms**

- **iOS** &ndash; `Main method > AppDelegate > App > ContentPage`
- **Android** &ndash; `MainActivity > App > ContentPage`
- **UWP** &ndash; `Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### <a name="application-class"></a>アプリケーション クラス

WPF と Xamarin.Forms の両方が、`Application`クラスをシングルトンとして作成されます。 ほとんどの場合、アプリは WPF では厳密に必要はありませんが、カスタム アプリケーションを提供するには、このクラスから派生します。 両方を公開、`Application.Current`プロパティを作成したシングルトンを検索します。

### <a name="global-properties--persistence"></a>グローバル プロパティ + 永続化

WPF と Xamarin.Forms の両方が、`Application.Properties`ディクショナリの使用可能なアプリケーションで任意の場所からアクセスできるグローバル アプリ レベルのオブジェクトを格納することができます。 主な違いは、Xamarin.Forms が_保持_アプリが中断されているとは再起動され、ときに再読み込み時に、コレクションに格納されている任意のプリミティブ型。 WPF は自動的にその動作をサポートしていません - 代わりに、ほとんどの開発者は、分離ストレージに依存したり、組み込みを利用`Settings`をサポートします。

## <a name="defining-pages-and-the-visual-tree"></a>ページとビジュアル ツリーを定義します。

WPF を使用して、`Window`任意の最上位のビジュアル要素のルート要素として。 これは、情報を表示する Windows の世界では、HWND を定義します。 作成し、WPF で好きなように同時に多くの windows を表示できます。

最上位のビジュアルに常に iOS でのプラットフォームで定義されている Xamarin.Forms では、`UIWindow`します。 Xamarin.Forms のレンダリングを使用してこれらのプラットフォームのネイティブ表現にコンテンツが、`Page`クラス。 各`Page`Xamarin.Forms で表すアプリケーションでは、固有の「ページ」場所だけで、一度に 1 つが表示されます。

両方 WPFs`Window`と Xamarin.Forms`Page`が含まれて、`Title`表示タイトル、および両方に影響するプロパティが、`Icon`ページの特定のアイコンを表示するプロパティを (**注**をタイトルとアイコンは常に表示されません Xamarin.Forms で) です。 さらに、背景色や画像などの両方で共通の visual プロパティを変更できます。

技術的には、2 つの別のプラットフォームのビューに表示することは (例: 2 つ定義`UIWindow`外付けディスプレイまたは AirPlay に 2 つ目の 1 つのレンダリングをいてオブジェクト)、そのためにはプラットフォーム固有のコードを必要し、するの直接サポートされている機能ではありません自体 Xamarin.Forms です。

### <a name="views"></a>Views

どちらのフレームワークの階層をビジュアルに似ています。 WPF では、WYSIWYG ドキュメントのサポートのためもう少し深くです。

**WPF**

```
DependencyObject - base class for all bindable things
   Visual - rendering mechanics
      UIElement - common events + interactions
         FrameworkElement - adds layout
            Shape - 2D graphics
            Control - interactive controls
```

**Xamarin.Forms**

```
BindableObject - base class for all bindable things
   Element - basic parent/child support + resources + effects
      VisualElement - adds visual rendering properties (color, fonts, transforms, etc.)
         View - layout + gesture support
```

### <a name="view-lifecycle"></a>ビュー ライフサイクル

Xamarin.Forms は、モバイルのシナリオを中心指向は主にします。 アプリケーションは、そのため、_アクティブ_、_中断_、および_再アクティブ化_ように、ユーザーがそれらを操作します。 クリックしてに似ています、 `Window` WPF アプリケーションでは、一連のメソッドおよび対応するイベントをオーバーライドしたり、この動作の監視にフックすることができます。

| 目的 | WPF メソッド | Xamarin.Forms メソッド |
|--- |--- |--- |
|最初のアクティブ化|ctor + Window.OnLoaded|ctor + Page.OnStart|
|表示されます。|Window.IsVisibleChanged|Page.Appearing|
|非表示|Window.IsVisibleChanged|Page.Disappearing|
|中断/Lost フォーカス|Window.OnDeactivated|Page.OnSleep|
|アクティブ化/Got フォーカス|Window.OnActivated|Page.OnResume|
|Closed|Window.OnClosing + Window.OnClosed|N/A|


両方サポートは非表示/表示子コントロールも、WPF では、3 つの状態プロパティ`IsVisible`(表示、非表示、および折りたたまれている)。 Xamarin.Forms でのみ表示するかを非表示は、`IsVisible`プロパティ。

### <a name="layout"></a>レイアウト

ページ レイアウトは、同じ 2 パス (メジャー/配置) WPF で発生するのに発生します。 ページ レイアウトをフックするには、Xamarin.Forms では、次のメソッドをオーバーライドすることで`Page`クラス。

| メソッド | 目的 |
|--- |--- |
|OnChildMeasureInvalidated|子の推奨サイズが変更されました。|
|OnSizeAllocated|ページには、幅と高さが割り当てられています。|
|LayoutChanged イベント|ページのレイアウトやサイズが変更されました。|

今日と呼ばれるグローバル レイアウト イベントはありませんもグローバルない`CompositionTarget.Rendering`WPF でのようなイベントを見つけます。

#### <a name="common-layout-properties"></a>一般的なレイアウトのプロパティ

WPF と Xamarin.Forms の両方をサポートして`Margin`コントロールの行間を要素の周囲および`Padding`コントロールの行間に_内_要素。 さらに、Xamarin.Forms のレイアウト ビューのほとんどでは、空白文字 (例: 行または列) を制御するプロパティがあります。

さらに、ほとんどの要素には、親コンテナーに配置する方法に影響を与えるプロパティがあります。

| WPF | Xamarin.Forms | 目的 |
|--- |--- |--- |
|HorizontalAlignment|HorizontalOptions|左、中央、右/Stretch オプション|
|[Verticalalignment]|\-Options|上部、中央、下/Stretch オプション|

> [!NOTE]
> これらのプロパティの実際の解釈は、親コンテナーに依存します。

#### <a name="layout-views"></a>レイアウト ビュー

WPF と Xamarin.Forms は、子要素を配置するのにレイアウト コントロールを使用します。 ほとんどの場合、これらは非常に近くなる機能の観点からです。

| WPF | Xamarin.Forms | レイアウト スタイル |
|--- |--- |--- |
|StackPanel|StackLayout|左から右、または上から下の無限スタック|
|グリッド|グリッド|表形式 (行と列)|
|DockPanel|N/A|ウィンドウの端にドッキングします。|
|Canvas|AbsoluteLayout|座標をピクセル単位/配置|
|WrapPanel|N/A|スタックの折り返し|
|N/A|RelativeLayout|相対的なルール ベースの配置|

> [!NOTE]
> Xamarin.Forms はサポートしていません、`GridSplitter`します。

両方のプラットフォームを使用して、_添付プロパティ_子を微調整します。

### <a name="rendering"></a>[レンダリング]

WPF と Xamarin.Forms のレンダリング メカニズムはまったく異なるです。 WPF を直接作成するコントロールは、画面上のピクセルにコンテンツをレンダリングします。 WPF は、2 つのオブジェクト グラフを維持 (_ツリー_) - これを表現する、_論理ツリー_コードまたは XAML で定義されているコントロールを表す、_ビジュアル ツリー_を表す、(仮想 draw メソッドの場合) を使用して実際に表示される画面上で発生する実行ビジュアル要素を直接または XAML 定義によって`ControlTemplate`は交換またはカスタマイズできます。 通常、ビジュアル ツリーは、暗黙的なコンテンツなどのラベルがコントロールの枠などが含まれている複雑です。WPF には、一連 Api にはが含まれています (`LogicalTreeHelper`と`VisualTreeHelper`) 2 つのオブジェクト グラフとこれらを確認します。

Xamarin.Forms でのコントロールで定義する、`Page`は実際には単純なデータ オブジェクトです。 これらは、論理ツリー形式に似ていますが、独自のコンテンツをレンダリングことはありません。 代わりが、_データ モデル_要素のレンダリングに影響を与えます。 実際のレンダリングを行う、[のセットを分割_visual レンダラー_コントロールの種類ごとに割り当て先となる](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)します。 これらのレンダラーは、Xamarin.Forms のプラットフォーム固有のアセンブリでの各プラットフォーム固有プロジェクトに登録されます。 一覧を確認できます[ここ](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)します。 に加えて、レンダラーの拡張を置き換えたり、Xamarin.Forms もサポートしています[効果](~/xamarin-forms/app-fundamentals/effects/index.md)プラットフォームごとにネイティブのレンダリングに影響を与える使用できます。

#### <a name="the-logicalvisual-tree"></a>論理/ビジュアル ツリー

Xamarin.Forms - 内に、論理ツリーをウォークに公開されている API はありませんが、リフレクションを使用して、同じ情報を取得することができます。 たとえば、[論理的な子を列挙するメソッドを次に示します](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108)リフレクションを使用します。

## <a name="graphics"></a>グラフィックス

Xamarin.Forms では、単純な四角形以外のプリミティブのグラフィックス システムが含まれない (`BoxView`)。 などのサード パーティ製ライブラリを含めることができます[SkiaSharp](~/graphics-games/skiasharp/index.md)クロスプラット フォームの 2D 描画を取得するまたは[UrhoSharp](~/graphics-games/urhosharp/index.md) 3D 用です。

## <a name="resources"></a>リソース

WPF と Xamarin.Forms 両方のリソースとリソース ディクショナリの概念があります。 任意のオブジェクト型を配置することができます、`ResourceDictionary`キーを持つを検索および`{StaticResource}`の変更されませんが、操作または`{DynamicResource}`の操作の実行時にディクショナリを変更できます。 使用状況とメカニズムは、1 つの違いは同じです。Xamarin.Forms では、定義する必要があります、`ResourceDictionary`に割り当てる、`Resources`プロパティが WPF いずれかを事前に作成し、それをします。

たとえば、次の定義を参照してください。

**WPF**

```xaml
<Window.Resources>
   <Color x:Key="redColor">#ff0000</Color>
   ...
</Window.Resources>
```

**Xamarin.Forms**

```xaml
<ContentPage.Resources>
   <ResourceDictionary>
      <Color x:Key="redColor">#ff0000</Color>
      ...
   </ResourceDictionary>
</ContentPage.Resources>
```

定義していない場合、 `ResourceDictionary`、ランタイム エラーが生成されます。

## <a name="styles"></a>スタイル

スタイルは、Xamarin.Forms でも完全にサポートし、は、その UI を起動する Xamarin.Forms の要素をテーマに使用します。 トリガー (プロパティ、イベントおよびデータ) の継承を通じてサポート`BasedOn`と値のリソースの参照。 スタイルがいずれかの要素に適用されてから明示的に、`Style`プロパティ、または WPF と同様のリソース キーを指定していないによる暗黙的な。

### <a name="device-styles"></a>デバイスのスタイル

WPF が一連の定義済みプロパティ (など、一連の静的クラスで静的な値として格納されている`SystemColors`) システム カラー、フォント、およびメトリックの値とリソース キーの形式でを指定します。 Xamarin.Forms は似ていますが、一連の定義[デバイス スタイル](~/xamarin-forms/user-interface/styles/device.md)を同じものを表します。 これらのスタイルは、framework によって提供され、ランタイム環境 (例: ユーザー補助) に基づく値に設定します。

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
