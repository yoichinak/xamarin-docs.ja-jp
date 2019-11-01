---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: WPF と Xamarin. フォームアプリのライフサイクル
description: このドキュメントでは、Xamarin. Forms アプリケーションと WPF アプリケーションのアプリケーションライフサイクルの類似点と相違点を比較します。 また、ビジュアルツリー、グラフィックス、リソース、およびスタイルも見られます。
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
ms.openlocfilehash: 725af8aebb111ac620a7d1ca5eebf73f31ee4a5e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016473"
---
# <a name="wpf-vs-xamarinforms-app-lifecycle"></a>WPF と Xamarin. フォームアプリのライフサイクル

Xamarin. フォームには、XAML ベースのフレームワーク (特に WPF) の設計ガイダンスが多数あります。 ただし、その他の方法では、移行しようとしている方のための固定ポイントとなることがあります。 このドキュメントでは、これらの問題のいくつかを特定し、WPF のナレッジを Xamarin. Forms にブリッジできるようにするためのガイダンスを提供します。

## <a name="app-lifecycle"></a>アプリのライフサイクル

WPF と Xamarin の間のアプリケーションのライフサイクルは似ています。 どちらも外部 (platform) コードで開始し、メソッド呼び出しを使用して UI を起動します。 違いは、Xamarin は常にプラットフォーム固有のアセンブリで開始し、その後、アプリの UI を初期化して作成することです。

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> `Main` メソッドは、既定では自動生成され、コードには表示されません。

**Xamarin.Forms**

- **iOS** &ndash; `Main method > AppDelegate > App > ContentPage`
- **Android** &ndash; `MainActivity > App > ContentPage`
- **UWP** &ndash; `Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### <a name="application-class"></a>Application クラス

WPF と Xamarin の両方に、シングルトンとして作成された `Application` クラスがあります。 ほとんどの場合、アプリは、このクラスから派生させてカスタムアプリケーションを提供しますが、これは WPF では厳密には必須ではありません。 どちらも、作成されたシングルトンを検索するために `Application.Current` プロパティを公開します。

### <a name="global-properties--persistence"></a>グローバルプロパティ + 永続化

WPF と Xamarin の両方には `Application.Properties` ディクショナリがあり、アプリケーション内のどこからでもアクセスできるグローバルなアプリレベルオブジェクトを格納できます。 主な違いは、Xamarin. Forms は、アプリが中断されたときにコレクションに格納されているプリミティブ型を_保持_し、再起動されると、それらを再読み込みすることです。 WPF はその動作を自動的にサポートしません。代わりに、ほとんどの開発者が分離ストレージに依存しているか、組み込みの `Settings` サポートを利用しています。

## <a name="defining-pages-and-the-visual-tree"></a>ページとビジュアルツリーの定義

WPF では、最上位レベルのビジュアル要素のルート要素として `Window` が使用されます。 これにより、情報を表示するための Windows 環境の HWND が定義されます。 WPF では、必要な数のウィンドウを同時に作成して表示できます。

Xamarin. Forms では、最上位レベルのビジュアルは常にプラットフォームによって定義されます。たとえば、iOS の場合は `UIWindow`です。 Xamarin は、`Page` クラスを使用して、これらのネイティブプラットフォーム表現にコンテンツをレンダリングします。 Xamarin の各 `Page` は、一度に1つだけ表示されるアプリケーションの一意の "ページ" を表します。

WPFs `Window` と Xamarin. Forms `Page` には、表示されるタイトルに影響を与える `Title` プロパティが含まれています。また、両方のプロパティには、ページの特定のアイコンを表示するための `Icon` プロパティがあります (タイトルとアイコンは、Xamarin では常に表示されないことに**注意**してください)。). また、背景色や画像のように、共通のビジュアルプロパティを変更することもできます。

技術的には、2つの異なるプラットフォームビューにレンダリングできます (たとえば、2つの `UIWindow` オブジェクトを定義し、2つ目のオブジェクトを外部ディスプレイまたはエアプレイにレンダリングする)。これを行うには、プラットフォーム固有のコードが必要です。これは、直接サポートされている機能ではありません。Xamarin. フォーム自体。

### <a name="views"></a>Views

両方のフレームワークのビジュアル階層は似ています。 WPF では、WYSIWYG ドキュメントがサポートされているため、もう少し詳しく説明しています。

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

Xamarin. フォームは主にモバイルシナリオを中心にしています。 そのため、ユーザーがアプリケーションと対話するときに、アプリケーションが_アクティブ化_、_中断_、再_アクティブ_化されます。 これは、WPF アプリケーションの `Window` からクリックする場合と似ていますが、この動作を監視するためにオーバーライドまたはフックできる一連のメソッドと対応するイベントがあります。

| 目的 | WPF メソッド | Xamarin. Forms メソッド |
|--- |--- |--- |
|初期アクティブ化|.ctor + Window. OnLoaded|.ctor + ページ. OnStart|
|表|IsVisibleChanged|ページを表示します。|
|非表示|IsVisibleChanged|ページの消失|
|中断/フォーカスの喪失|Window. OnDeactivated アクティブ化|ページ. OnSleep|
|アクティブ化/フォーカスの獲得|ウィンドウ. OnActivated 化済み|ページ. OnResume|
|Closed|Window. OnClosing + Window. Onclosing|N/A|

子コントロールの表示/非表示もサポートされています。 WPF では、これは `IsVisible` (表示、非表示、および折りたたみ) の3つのプロパティです。 Xamarin. Forms では、`IsVisible` プロパティを通じて表示または非表示にするだけです。

### <a name="layout"></a>レイアウト

ページレイアウトは、WPF で同じ2パス (メジャー/配置) に発生します。 Xamarin `Page` クラスの次のメソッドをオーバーライドすることにより、ページレイアウトにフックできます。

| メソッド | 目的 |
|--- |--- |
|OnChildMeasureInvalidated|子の優先サイズが変更されました。|
|OnSizeAllocated 済み|ページに幅/高さが割り当てられています。|
|LayoutChanged イベント|ページのレイアウト/サイズが変更されました。|

現在呼び出されているグローバルレイアウトイベントは存在せず、WPF にあるようなグローバル `CompositionTarget.Rendering` イベントもありません。

#### <a name="common-layout-properties"></a>一般的なレイアウトのプロパティ

WPF と Xamarin では、要素の周囲の間隔を制御 `Padding` したり、要素_内_の間隔を制御したりするために `Margin` をサポートしています。 また、ほとんどの Xamarin のレイアウトビューには、間隔 (行や列など) を制御するプロパティがあります。

また、ほとんどの要素には、親コンテナーへの配置方法に影響を与えるプロパティがあります。

| WPF | Xamarin.Forms | 目的 |
|--- |--- |--- |
|HorizontalAlignment|水平オプション|左/中央/右/伸縮オプション|
|System.windows.frameworkelement.verticalalignment|垂直オプション|上部/中央/下部/伸縮オプション|

> [!NOTE]
> これらのプロパティの実際の解釈は、親コンテナーによって異なります。

#### <a name="layout-views"></a>レイアウトビュー

WPF と Xamarin は両方ともレイアウトコントロールを使用して子要素を配置します。 ほとんどの場合、これらは、機能に関して非常に近接しています。

| WPF | Xamarin.Forms | レイアウトスタイル |
|--- |--- |--- |
|StackPanel|StackLayout|左から右、または上から下の無限スタック|
|グリッド|グリッド|表形式 (行と列)|
|DockPanel|N/A|ウィンドウの端にドッキングする|
|Canvas|AbsoluteLayout|ピクセル/座標の位置|
|WrapPanel|N/A|スタックをラップしています|
|N/A|RelativeLayout|ルールベースの相対的な配置|

> [!NOTE]
> Xamarin. フォームは `GridSplitter`をサポートしていません。

どちらのプラットフォームも、_添付プロパティ_を使用して子を微調整します。

### <a name="rendering"></a>[レンダリング]

WPF と Xamarin のレンダリング機構は根本的に異なります。 WPF では、直接作成したコントロールが画面上のピクセルにコンテンツをレンダリングします。 WPF では、これを表す2つのオブジェクトグラフ (_ツリー_) を保持しています。_論理ツリー_は、コードまたは XAML で定義されているコントロールを表します。_ビジュアルツリー_は、画面上で実行される実際のレンダリングを表します。(仮想描画メソッドを使用して) ビジュアル要素によって直接、または置換またはカスタマイズできる XAML 定義 `ControlTemplate` によって直接実行されます。 通常、ビジュアルツリーの方が複雑になります。これには、コントロールの境界線、暗黙的なコンテンツのラベルなどが含まれます。WPF には、これらの2つのオブジェクトグラフを調べるための一連の Api (`LogicalTreeHelper` と `VisualTreeHelper`) が含まれています。

Xamarin. Forms では、`Page` で定義するコントロールは、実際には単純なデータオブジェクトにすぎません。 これらは論理ツリー表現に似ていますが、独自にコンテンツをレンダリングすることはできません。 代わりに、要素のレンダリングに影響を与える_データモデル_です。 実際のレンダリングは、[各コントロールの種類にマップされる_視覚的レンダラー_の個別のセット](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)によって行われます。 これらのレンダラーは、プラットフォーム固有の Xamarin. Forms アセンブリによってプラットフォーム固有の各プロジェクトに登録されます。 一覧は[こちらで](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)確認できます。 レンダラーの置換や拡張に加えて、Xamarin. Forms は、プラットフォームごとにネイティブレンダリングに影響を与えるために使用できる[効果](~/xamarin-forms/app-fundamentals/effects/index.md)もサポートしています。

#### <a name="the-logicalvisual-tree"></a>論理/ビジュアルツリー

Xamarin. Forms で論理ツリーをウォークするための API は公開されていませんが、リフレクションを使用して同じ情報を取得することができます。 たとえば、次に示すのは、リフレクションを使用して論理上の[子を列挙できるメソッド](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108)です。

## <a name="graphics"></a>グラフィックス

Xamarin. フォームには、単純な四角形 (`BoxView`) を超えるプリミティブのグラフィックスシステムは含まれていません。 [SkiaSharp](~/graphics-games/skiasharp/index.md)などのサードパーティ製のライブラリを使用して、クロスプラットフォームの2d 描画や、3D の[urhosharp](~/graphics-games/urhosharp/index.md)を取得することができます。

## <a name="resources"></a>リソース

WPF と Xamarin。フォームには、リソースとリソースディクショナリの概念があります。 任意のオブジェクトの種類をキーを使用して `ResourceDictionary` に配置してから、変更されない項目 `{DynamicResource}` や、実行時にディクショナリで変更される可能性がある項目については、`{StaticResource}` で検索できます。 使用法としくみは同じですが、Xamarin. Forms では、`Resources` プロパティに割り当てる `ResourceDictionary` を定義する必要がありますが、WPF は事前に作成し、割り当てます。

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

`ResourceDictionary`を定義しないと、ランタイムエラーが生成されます。

## <a name="styles"></a>スタイル

スタイルは、Xamarin. Forms でも完全にサポートされており、UI を構成する Xamarin. Forms 要素のテーマを設定するために使用できます。 トリガー (プロパティ、イベント、データ)、`BasedOn`による継承、および値のリソース参照をサポートしています。 スタイルは、`Style` プロパティを通じて明示的に要素に適用されるか、WPF と同様にリソースキーを指定せずに暗黙的に適用されます。

### <a name="device-styles"></a>デバイスのスタイル

WPF には、定義済みプロパティのセットがあります (`SystemColors`などの静的クラスのセットに静的な値として格納されます)。これにより、システムカラー、フォント、およびメトリックが値とリソースキーの形式で示されます。 Xamarin. Forms は似ていますが、同じものを表す一連の[デバイススタイル](~/xamarin-forms/user-interface/styles/device.md)を定義します。 これらのスタイルは、フレームワークによって提供され、ランタイム環境 (アクセシビリティなど) に基づいて値に設定されます。

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
