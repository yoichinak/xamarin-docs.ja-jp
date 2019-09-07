---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: WPF とXamarin.Forms アプリのライフサイクル
description: このドキュメントでは、Xamarin. Forms アプリケーションと WPF アプリケーションのアプリケーションライフサイクルの類似点と相違点を比較します。 また、ビジュアルツリー、グラフィックス、リソース、およびスタイルも見られます。
author: conceptdev
ms.author: crdun
ms.date: 04/26/2017
ms.openlocfilehash: 1d1c207672baf37c91d39fca1d9f628d85971577
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765902"
---
# <a name="wpf-vs-xamarinforms-app-lifecycle"></a>WPF とXamarin.Forms アプリのライフサイクル

Xamarin. フォームには、XAML ベースのフレームワーク (特に WPF) の設計ガイダンスが多数あります。 ただし、その他の方法では、移行しようとしている方のための固定ポイントとなることがあります。 このドキュメントでは、これらの問題のいくつかを特定し、WPF のナレッジを Xamarin. Forms にブリッジできるようにするためのガイダンスを提供します。

## <a name="app-lifecycle"></a>アプリのライフサイクル

WPF と Xamarin の間のアプリケーションのライフサイクルは似ています。 どちらも外部 (platform) コードで開始し、メソッド呼び出しを使用して UI を起動します。 違いは、Xamarin は常にプラットフォーム固有のアセンブリで開始し、その後、アプリの UI を初期化して作成することです。

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> `Main`メソッドは、既定では自動生成され、コードには表示されません。

**Xamarin.Forms**

- **iOS**&ndash;`Main method > AppDelegate > App > ContentPage`
- **Android**&ndash;`MainActivity > App > ContentPage`
- **UWP**&ndash;`Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### <a name="application-class"></a>Application クラス

WPF と Xamarin の両方に、シングルトン`Application`として作成されたクラスがあります。 ほとんどの場合、アプリは、このクラスから派生させてカスタムアプリケーションを提供しますが、これは WPF では厳密には必須ではありません。 どちらも、 `Application.Current`作成されたシングルトンを検索するためのプロパティを公開します。

### <a name="global-properties--persistence"></a>グローバルプロパティ + 永続化

WPF と Xamarin の両方には、 `Application.Properties`アプリケーション内のどこからでもアクセスできるグローバルなアプリレベルオブジェクトを格納できるディクショナリがあります。 主な違いは、Xamarin. Forms は、アプリが中断されたときにコレクションに格納されているプリミティブ型を_保持_し、再起動されると、それらを再読み込みすることです。 WPF はその動作を自動的にサポートしません。代わりに、ほとんどの開発者が分離ストレージに依存`Settings`しているか、組み込みのサポートを利用しています。

## <a name="defining-pages-and-the-visual-tree"></a>ページとビジュアルツリーの定義

WPF は、 `Window`トップレベルのビジュアル要素のルート要素としてを使用します。 これにより、情報を表示するための Windows 環境の HWND が定義されます。 WPF では、必要な数のウィンドウを同時に作成して表示できます。

Xamarin. Forms では、トップレベルのビジュアルは常にプラットフォームによって定義されます`UIWindow`。たとえば、iOS では、です。 Xamarin は`Page` 、クラスを使用して、これらのネイティブプラットフォーム表現にコンテンツをレンダリングします。 Xamarin `Page`の各は、アプリケーション内の一意の "ページ" を表します。この場合、一度に表示されるのは1つだけです。

Wpfs `Window`と Xamarin. フォーム`Page`の両方に`Title`は、表示されるタイトル`Icon`に影響を与えるプロパティが含まれています。また、このプロパティには、ページの特定のアイコンを表示するプロパティがあります (タイトルとアイコンは常にではないことに**注意**してください)。表示されます。 また、背景色や画像のように、共通のビジュアルプロパティを変更することもできます。

技術的には、2つの異なるプラットフォームビューにレンダリングできます (2 `UIWindow`つのオブジェクトを定義し、2つ目のオブジェクトを外部ディスプレイまたはエアプレイにレンダリングするなど)。これを行うには、プラットフォーム固有のコードが必要ですが、直接サポートされている機能ではありません。Xamarin. フォーム自体。

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

Xamarin. フォームは主にモバイルシナリオを中心にしています。 そのため、ユーザーがアプリケーションと対話するときに、アプリケーションが_アクティブ化_、_中断_、再_アクティブ_化されます。 これは、 `Window` WPF アプリケーションでをクリックする場合と似ていますが、この動作を監視するためにオーバーライドまたはフックできる一連のメソッドとそれに対応するイベントがあります。

| 目的 | WPF メソッド | Xamarin. Forms メソッド |
|--- |--- |--- |
|初期アクティブ化|.ctor + Window. OnLoaded|.ctor + ページ. OnStart|
|表|IsVisibleChanged|ページを表示します。|
|非表示|IsVisibleChanged|ページの消失|
|中断/フォーカスの喪失|Window. OnDeactivated アクティブ化|ページ. OnSleep|
|アクティブ化/フォーカスの獲得|ウィンドウ. OnActivated 化済み|ページ. OnResume|
|Closed|Window. OnClosing + Window. Onclosing|N/A|

子コントロールの表示/非表示もサポートされています。 WPF では、 `IsVisible` 3 つの状態のプロパティ (表示、非表示、および折りたたみ) がサポートされています。 Xamarin. Forms では、プロパティを使用して表示また`IsVisible`は非表示にするだけです。

### <a name="layout"></a>レイアウト

ページレイアウトは、WPF で同じ2パス (メジャー/配置) に発生します。 Xamarin. Forms `Page`クラスで次のメソッドをオーバーライドすることにより、ページレイアウトにフックできます。

| メソッド | 目的 |
|--- |--- |
|OnChildMeasureInvalidated|子の優先サイズが変更されました。|
|OnSizeAllocated 済み|ページに幅/高さが割り当てられています。|
|LayoutChanged イベント|ページのレイアウト/サイズが変更されました。|

現在呼び出されているグローバルレイアウトイベントはありません。また、WPF `CompositionTarget.Rendering`のようなグローバルイベントもありません。

#### <a name="common-layout-properties"></a>一般的なレイアウトのプロパティ

WPF と Xamarin。フォームは、 `Margin`要素の前後の間隔の制御、 `Padding`および要素_内_の間隔の制御をサポートしています。 また、ほとんどの Xamarin のレイアウトビューには、間隔 (行や列など) を制御するプロパティがあります。

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
|キャンバス|AbsoluteLayout|ピクセル/座標の位置|
|WrapPanel|N/A|スタックをラップしています|
|N/A|RelativeLayout|ルールベースの相対的な配置|

> [!NOTE]
> Xamarin は、を`GridSplitter`サポートしていません。

どちらのプラットフォームも、_添付プロパティ_を使用して子を微調整します。

### <a name="rendering"></a>表示

WPF と Xamarin のレンダリング機構は根本的に異なります。 WPF では、直接作成したコントロールが画面上のピクセルにコンテンツをレンダリングします。 WPF では、これを表す2つのオブジェクトグラフ (_ツリー_) を保持しています。_論理ツリー_は、コードまたは XAML で定義されているコントロールを表します。_ビジュアルツリー_は、画面上で実行される実際のレンダリングを表します。(仮想描画メソッドを使用して) ビジュアル要素によって直接、または置換`ControlTemplate`またはカスタマイズできる XAML で定義された。 通常、ビジュアルツリーの方が複雑になります。これには、コントロールの境界線、暗黙的なコンテンツのラベルなどが含まれます。WPF には、これらの 2`LogicalTreeHelper`つ`VisualTreeHelper`のオブジェクトグラフを調べるための api のセット (と) が含まれています。

Xamarin. Forms では、で`Page`定義するコントロールは、実際には単純なデータオブジェクトにすぎません。 これらは論理ツリー表現に似ていますが、独自にコンテンツをレンダリングすることはできません。 代わりに、要素のレンダリングに影響を与える_データモデル_です。 実際のレンダリングは、[各コントロールの種類にマップされる_視覚的レンダラー_の個別のセット](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)によって行われます。 これらのレンダラーは、プラットフォーム固有の Xamarin. Forms アセンブリによってプラットフォーム固有の各プロジェクトに登録されます。 一覧は[こちらで](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)確認できます。 レンダラーの置換や拡張に加えて、Xamarin. Forms は、プラットフォームごとにネイティブレンダリングに影響を与えるために使用できる[効果](~/xamarin-forms/app-fundamentals/effects/index.md)もサポートしています。

#### <a name="the-logicalvisual-tree"></a>論理/ビジュアルツリー

Xamarin. Forms で論理ツリーをウォークするための API は公開されていませんが、リフレクションを使用して同じ情報を取得することができます。 たとえば、次に示すのは、リフレクションを使用して論理上の[子を列挙できるメソッド](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108)です。

## <a name="graphics"></a>グラフィックス

Xamarin. フォームには、単純な四角形 (`BoxView`) を超えるプリミティブのグラフィックスシステムは含まれていません。 [SkiaSharp](~/graphics-games/skiasharp/index.md)などのサードパーティ製のライブラリを使用して、クロスプラットフォームの2d 描画や、3D の[urhosharp](~/graphics-games/urhosharp/index.md)を取得することができます。

## <a name="resources"></a>リソース

WPF と Xamarin。フォームには、リソースとリソースディクショナリの概念があります。 任意のオブジェクトの種類を、 `ResourceDictionary`キーを使用してに配置し、変更されない項目や`{DynamicResource}` 、実行時にディクショナリで変更される可能性`{StaticResource}`のあるものを検索することができます。 使用法としくみは同じですが、1つ違いがあります。Xamarin. フォームでは、 `ResourceDictionary` `Resources`プロパティに割り当てるを定義する必要があります。 WPF では、これが事前に作成され、割り当てられます。

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

を定義`ResourceDictionary`しない場合は、ランタイムエラーが生成されます。

## <a name="styles"></a>スタイル

スタイルは、Xamarin. Forms でも完全にサポートされており、UI を構成する Xamarin. Forms 要素のテーマを設定するために使用できます。 トリガー (プロパティ、イベント、データ)、値の継承、 `BasedOn`およびリソース参照をサポートしています。 スタイルは、 `Style`プロパティを使用して明示的に要素に適用されるか、WPF と同様にリソースキーを指定せずに暗黙的に適用されます。

### <a name="device-styles"></a>デバイスのスタイル

WPF には、定義済みプロパティのセットがあります (など`SystemColors`の静的クラスのセットに静的値として格納されます)。これは、システムカラー、フォント、およびメトリックを値とリソースキーの形式で指定します。 Xamarin. Forms は似ていますが、同じものを表す一連の[デバイススタイル](~/xamarin-forms/user-interface/styles/device.md)を定義します。 これらのスタイルは、フレームワークによって提供され、ランタイム環境 (アクセシビリティなど) に基づいて値に設定されます。

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
