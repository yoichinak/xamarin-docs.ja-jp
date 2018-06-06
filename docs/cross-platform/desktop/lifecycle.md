---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: WPF とします。Xamarin.Forms アプリのライフ サイクル
description: このドキュメントを比較して、類似点および Xamarin.Forms と WPF アプリケーションのアプリケーションのライフ サイクル間の相違点です。 ビジュアル ツリーをグラフィックス、リソース、およびスタイルも確認します。
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: abb7773873fa181085464b5985cc8233715cc4be
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781583"
---
# <a name="wpf-vs-xamarinforms-app-lifecycle"></a>WPF とします。Xamarin.Forms アプリのライフ サイクル

Xamarin.Forms では、多くの前に、特に WPF 付属している XAML ベースのフレームワークからの設計ガイダンスを受け取ります。 ただし、他の方法で、大幅に下回っている経由で移行しようとしています。 ユーザーの付箋のポイントになります。 このドキュメントは、これらの問題のいくつかを特定し、Xamarin.Forms をブリッジ WPF ナレッジに可能なガイダンスを提供を試みます。

## <a name="app-lifecycle"></a>アプリのライフ サイクル

WPF と Xamarin.Forms の間でアプリケーションのライフ サイクルは似ています。 両方の外部 (プラットフォーム) コードに起動し、メソッド呼び出しによって UI を起動します。 違いは、Xamarin.Forms が常に初期化し、アプリの UI を作成するプラットフォーム固有のアセンブリで起動することです。

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> `Main`メソッドは、既定では、自動生成されると、コードでは表示されません。

**Xamarin.Forms**

- **iOS** &ndash; `Main method > AppDelegate > App > ContentPage`
- **Android** &ndash; `MainActivity > App > ContentPage`
- **UWP** &ndash; `Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### <a name="application-class"></a>アプリケーション クラス

WPF と Xamarin.Forms の両方がある、`Application`クラスを singleton として作成されます。 ほとんどの場合、アプリはこのクラスから派生したカスタム アプリケーションを提供これは必要ありませんが厳密に WPF でします。 両方を公開、`Application.Current`プロパティを作成したシングルトンを検索します。

### <a name="global-properties--persistence"></a>グローバル プロパティ + 永続化

WPF と Xamarin.Forms の両方がある、`Application.Properties`ディクショナリの使用可能なアプリケーションで任意の場所にアクセス可能であるグローバル アプリ レベル オブジェクトを格納できます。 主な違いは、Xamarin.Forms は_保持_と、アプリが中断されているときに再起動が再読み込みは、コレクションに格納されている任意のプリミティブ型。 WPF は自動的にその動作をサポートしていません - 代わりに、ほとんどの開発者は、分離ストレージに依存したり、組み込みの使用率が低い`Settings`をサポートします。

## <a name="defining-pages-and-the-visual-tree"></a>ページとビジュアル ツリーを定義します。

WPF を使用して、`Window`最上位のビジュアル要素のルート要素として。 これは、情報を表示する Windows の世界で、HWND を定義します。 作成し、WPF で好きなように同時に多くの windows を表示できます。

Xamarin.Forms で最上位のビジュアルは常に、iOS でなどのプラットフォームで定義、`UIWindow`です。 Xamarin.Forms のレンダリングを使用してこれらのプラットフォームのネイティブ表記にコンテンツである、`Page`クラスです。 各`Page`Xamarin.Forms で表すアプリケーションでは、固有の「ページ」場所だけで、一度に 1 つが表示されます。

両方 WPFs`Window`と Xamarin.Forms`Page`含める、 `Title` 、表示されるタイトル、および両方に影響するプロパティが、`Icon`ページの特定のアイコンを表示するプロパティ (**注**をタイトルとアイコンが常に表示されない Xamarin.Forms で) です。 さらに、両方の背景色や背景画像などの共通のビジュアル プロパティを変更できます。

別のプラットフォームの 2 つのビューを表示するために、技術的に可能であれば (例: 2 つの定義`UIWindow`オブジェクトおよび外付けディスプレイまたは AirPlay を 2 つ目の 1 つのレンダリングがある)、そのためにはプラットフォーム固有のコードを必要としての直接サポートされている機能ではありません自体 Xamarin.Forms です。

### <a name="views"></a>Views

両方のフレームワークのビジュアルの階層は似ています。 WPF は WYSIWYG ドキュメントのサポートがあるため、少し進むです。

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

### <a name="view-lifcycle"></a>ビュー Lifcycle

Xamarin.Forms は、mobile のシナリオを主に指向です。 アプリケーションは、そのため、_アクティブ_、_中断_、および_再アクティブ化_ように、ユーザーがそれらを操作します。 これは外側をクリックすると同様、 `Window` WPF アプリケーションで、一連のメソッドと対応するイベントをオーバーライドしたり、この動作を監視するにフックすることができます。

| 目的 | WPF メソッド | Xamarin.Forms メソッド |
|--- |--- |--- |
|最初のアクティブ化|ctor + Window.OnLoaded|ctor + Page.OnStart|
|表示されます。|Window.IsVisibleChanged|Page.Appearing|
|非表示|Window.IsVisibleChanged|Page.Disapearing|
|中断/失注フォーカス|Window.OnDeactivated|Page.OnSleep|
|アクティブ化/Got フォーカス|Window.OnActivated|Page.OnResume|
|Closed|Window.OnClosing + Window.OnClosed|N/A|


両方のサポートの非表示/表示子コントロールも、WPF では、3 つの状態プロパティ`IsVisible`(表示、非表示、および折りたたまれている)。 Xamarin.Forms であるだけ表示/非表示を`IsVisible`プロパティです。

### <a name="layout"></a>レイアウト

ページ レイアウトは、同じ 2 パス (メジャー/配置) WPF で発生したで発生します。 ページ レイアウトをフック、Xamarin.Forms では、次のメソッドをオーバーライドすることで`Page`クラス。

| メソッド | 目的 |
|--- |--- |
|OnChildMeasureInvalidated|子の推奨サイズが変更されました。|
|OnSizeAllocated|ページには、幅と高さが割り当てられています。|
|LayoutChanged イベント|ページのレイアウトとサイズが変更されました。|

今日と呼ばれるグローバル レイアウト イベントはありませんもグローバルない`CompositionTarget.Rendering`wpf などのイベントが検出されました。

#### <a name="common-layout-properties"></a>一般的なレイアウト プロパティ

WPF と Xamarin.Forms の両方をサポートして`Margin`コントロールの行間を要素の周囲および`Padding`コントロールの行間に_内_要素。 さらに、Xamarin.Forms レイアウト ビューのほとんどには、(行または列) の間隔を制御するプロパティがあります。

さらに、ほとんどの要素には、親コンテナー内の配置方法に影響を与えるプロパティがあります。

| WPF | Xamarin.Forms | 目的 |
|--- |--- |--- |
|HorizontalAlignment|HorizontalOptions|Left、Center、Right/Stretch オプション|
|VerticalAlignment|VerticalOptions|上部、中央、下部にある/Stretch オプション|

> [!NOTE]
> これらのプロパティの実際の解釈は、親コンテナーに依存します。

#### <a name="layout-views"></a>レイアウト ビュー

WPF と Xamarin.Forms の両方は、子要素を配置するのにレイアウト コントロールを使用します。 ほとんどの場合、これらは非常に相互に近接機能の観点からです。

| WPF | Xamarin.Forms | レイアウト スタイル |
|--- |--- |--- |
|StackPanel|StackLayout|左から右へ、または上から下への無限スタック|
|グリッド|グリッド|表形式 (行と列)|
|DockPanel|N/A|ウィンドウの端にドッキングします。|
|Canvas|AbsoluteLayout|/座標をピクセル単位の配置|
|WrapPanel|N/A|スタックの折り返し|
|N/A|RelativeLayout|相対ルール ベースの配置|

> [!NOTE]
> Xamarin.Forms はサポートしていません、`GridSplitter`です。

両方のプラットフォームを使用して_アタッチされるプロパティ_を子を細かく調整します。

### <a name="rendering"></a>[レンダリング]

WPF と Xamarin.Forms のレンダリングのしくみについては、大幅に異なります。 Wpf では、直接作成するコントロールは、画面上のピクセルにコンテンツをレンダリングします。 WPF は、2 つのオブジェクト グラフを保持 (_ツリー_) - これを表現する、_論理ツリー_コードまたは XAML で定義されているコントロールを表す、_ビジュアル ツリー_を表す、これは、画面上で発生する実際のレンダリングでは、ビジュアル要素によって直接いずれかを実行 (仮想描画メソッドを通じて)、または XAML で定義された`ControlTemplate`置き換えやカスタマイズするをします。 通常、ビジュアル ツリーが含まれているこの暗黙的なコンテンツなどのラベルがコントロールの枠より複雑なです。WPF には、一連 Api にはが含まれています (`LogicalTreeHelper`と`VisualTreeHelper`) これらを調べるには、2 つのオブジェクト グラフ。

Xamarin.Forms でコントロールを定義するには`Page`実際には単純なデータ オブジェクトです。 これらは、論理ツリー表現に似ていますが、単独でコンテンツを提供することはありません。 これらはむしろ、_データ モデル_要素のレンダリングに影響を与えます。 実際のレンダリングを行う、[のセットを分割_visual レンダラー_それぞれのコントロール型にマップされている](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)です。 これらのレンダラーは、プラットフォーム固有の Xamarin.Forms アセンブリによって各プラットフォームに固有のプロジェクトに登録されます。 一覧を表示できます[ここ](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)です。 に加えて、レンダラーの拡張を置き換えたり、Xamarin.Forms ありますサポート[効果](~/xamarin-forms/app-fundamentals/effects/index.md)プラットフォームごとにネイティブのレンダリングに影響を与えるを使用できます。

#### <a name="the-logicalvisual-tree"></a>論理/Visual ツリー

Xamarin.forms の論理ツリーをウォークに公開されている API はありませんが、リフレクションを使用して同じ情報を得ることができます。 たとえば、[論理子を列挙するメソッドを次に示します](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108)リフレクションを使用します。

## <a name="graphics"></a>グラフィックス

Xamarin.Forms では、単純な四角形以外のプリミティブのグラフィックス システムが含まれない (`BoxView`)。 などのサード パーティ製ライブラリを含めることができます[SkiaSharp](~/graphics-games/skiasharp/index.md)クロスプラット フォームの 2D 図面を取得するか、 [UrhoSharp](~/graphics-games/urhosharp/index.md) 3d です。

## <a name="resources"></a>リソース

WPF と Xamarin.Forms 両方リソースおよびリソース ディクショナリの概念があります。 どのオブジェクト型を配置することができます、`ResourceDictionary`キーを持つを検索し、`{StaticResource}`の変更されませんが、操作または`{DynamicResource}`の操作の実行時にディクショナリを変更することができます。 使用法としくみが 1 つの違いと同じ: Xamarin.Forms では、定義する必要があります、`ResourceDictionary`に割り当てる、`Resources`プロパティ WPF 事前 1 つを作成し、それを一方です。

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

スタイルは、Xamarin.Forms ではサポートされても完全に可能であり、Xamarin.Forms を構成する要素の UI のテーマを使用します。 トリガー (プロパティ、イベントおよびデータ) の継承をサポート`BasedOn`、およびリソースの値を検索します。 スタイルは要素に適用されますか明示的に必要で、`Style`プロパティ、または暗黙でないリソース キー - WPF と同じように指定することによってです。

### <a name="device-styles"></a>デバイスのスタイル

WPF は、一連の定義済みプロパティ (など、一連の静的クラスで静的な値として格納されている`SystemColors`) している色、フォントおよびメトリックの値とリソースのキーの形式で指定します。 Xamarin.Forms は似ていますが、一連の定義[デバイス スタイル](~/xamarin-forms/user-interface/styles/device.md)と同じ操作を表します。 これらのスタイルは、frameowrk によって指定され、ランタイム環境 (例: ユーザー補助) に基づく値に設定します。

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
