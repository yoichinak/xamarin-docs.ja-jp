---
title: Xamarin.Mac の画像
description: この記事では、イメージとアイコンは、Xamarin.Mac アプリケーションでの操作について説明します。 これは、作成、およびアプリケーションのアイコンを作成するために必要し、c# コードと Xcode の Interface Builder の両方でイメージを使用してイメージを保守について説明します。
ms.prod: xamarin
ms.assetid: C6B539C2-FC6A-4C38-B839-32BFFB9B16A7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 3bc3c731729f92ae3c5ad6126166892a236e2eab
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2018
ms.locfileid: "40251123"
---
# <a name="images-in-xamarinmac"></a>Xamarin.Mac の画像

_この記事では、イメージとアイコンは、Xamarin.Mac アプリケーションでの操作について説明します。これは、作成、およびアプリケーションのアイコンを作成するために必要し、c# コードと Xcode の Interface Builder の両方でイメージを使用してイメージを保守について説明します。_

## <a name="overview"></a>概要

同じイメージへのアクセスがある、Xamarin.Mac アプリケーションで c# と .NET を使用する場合、およびアイコン ツールで作業する開発者*Objective C*と*Xcode*は。

MacOS (旧称 Mac OS X) アプリケーション内の資産が使用されるイメージのいくつかの方法があります。 単に、アイコンを提供するためにツール バーまたはソースのリスト アイテムなどの UI コントロールに割り当てるには、アプリケーションの UI の一部としてイメージを表示するから Xamarin.Mac 簡単に、次の方法で、優れたアートワークを macOS アプリケーションに追加: 

- **UI 要素**-背景として、またはイメージ ビューで、アプリケーションの一部として、イメージを表示できます (`NSImageView`)。
- **ボタン**-ボタンの画像を表示できます (`NSButton`)。
- **セルのイメージ**- ベースのテーブル コントロールの一部として (`NSTableView`または`NSOutlineView`)、イメージのセルにイメージを使用できます (`NSImageCell`)。
- **ツール バー アイテム**-イメージをツールバーに追加することができます (`NSToolbar`) イメージのツールバー項目として (`NSToolbarItem`)。
- **ソースの一覧のアイコン**- ソース リストの一部として (を特別にフォーマットされた`NSOutlineView`)。
- **アプリ アイコン**-に一連のイメージをグループ化できる、`.icns`に設定して、アプリケーションのアイコンとして使用します。 参照してください、[アプリケーション アイコン](~/mac/deploy-test/app-icon.md)詳細についてはドキュメントです。

さらに、macOS では、アプリケーション全体で使用できる定義済みのイメージのセットを提供します。

[![例は、アプリの実行](image-images/intro01.png "例は、アプリの実行")](image-images/intro01-large.png#lightbox)

この記事では、イメージとアイコンを Xamarin.Mac アプリケーションで操作の基本を説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)と[Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions)ほどのセクションでは、主要な概念と、この記事で使用する方法について説明します。


## <a name="adding-images-to-a-xamarinmac-project"></a>Xamarin.Mac プロジェクトにイメージを追加します。

Xamarin.Mac アプリケーションで使用するイメージを追加するときにいくつかの場所と方法は、開発者がプロジェクトのソース イメージ ファイルを含めることができます。

- **メイン プロジェクト ツリーの [非推奨]** -イメージは、プロジェクト ツリーに直接追加することができます。 コードからメイン プロジェクト ツリーに格納されているイメージを呼び出すときにフォルダーの場所を指定しません。 たとえば、`NSImage image = NSImage.ImageNamed("tags.png");` のように指定します。 
- **[非推奨] リソース フォルダー** -特殊**リソース**イメージ (イメージや、開発者のファイル、アイコン、起動画面または [全般] など、アプリケーションの一部となるすべてのファイルがバンドル 's フォルダーは希望を追加する)。 格納されているイメージを呼び出すときに、**リソース**コードからフォルダーをメイン プロジェクト ツリーにイメージと同様に格納されている、フォルダーの場所が指定されていません。 たとえば、`NSImage.ImageNamed("tags.png")` のように指定します。
- **カスタムのフォルダーまたは [非推奨] サブフォルダー** -開発者がプロジェクトのソース ツリーにカスタム フォルダーを追加およびがイメージを格納します。 ファイルを追加する場所は、さらに、プロジェクトを整理するサブフォルダーに入れ子にできます。 開発者が追加された場合など、`Card`フォルダー、プロジェクトとのサブ フォルダーを`Hearts`、そのフォルダーにイメージを格納し、 **Jack.png**で、`Hearts`フォルダー、`NSImage.ImageNamed("Card/Hearts/Jack.png")`でイメージの読み込みはランタイム。
- **[優先] 資産カタログの画像セット**- OS X El Capitan 追加**資産カタログの画像セット**すべてのバージョンまたはさまざまなデバイスをサポートし、スケール ファクターのために必要なイメージの表現が含まれて、アプリケーション。 イメージの資産ファイル名ではなく (**@1x**、 **@2x**)。

<a name="asset-catalogs" />

### <a name="adding-images-to-an-asset-catalog-image-set"></a>資産カタログ イメージへのイメージの追加設定

、前述のよう、**資産カタログの画像セット**すべてのバージョンまたはさまざまなデバイスをサポートし、スケール ファクターは、アプリケーションに必要なイメージの表現が含まれています。 イメージの資産ファイル名ではなく (解像度の独立したイメージとイメージの命名規則が上記を参照)、**画像セット**資産エディターを使用して、どのデバイスと解像度にどのイメージが属しているを指定します。

1. **Solution Pad**、ダブルクリックして、 **Assets.xcassets**ファイルを開き、編集します。 

    ![選択、Assets.xcassets](image-images/imageset01.png "Assets.xcassets を選択します。")
2. 右クリックし、 **Assets リスト**選択**新しいイメージ セット**: 

    [![新しいイメージ セットに追加する](image-images/imageset02.png "新しいイメージ セットの追加")](image-images/imageset02-large.png#lightbox)
3. 新しいイメージ セットを選択し、エディターが表示されます。 

    [![新しいイメージ セットの選択](image-images/imageset03.png "新しいイメージ セットの選択")](image-images/imageset03-large.png#lightbox)
4. ここから、それぞれの異なるデバイスや必要な解像度のイメージでドラッグしたことができます。 
5. 新しいイメージ セットをダブルクリックして**名前**で、 **Assets リスト**編集します。 

    [![名前を設定するイメージの編集](image-images/imageset04.png "名の設定、画像の編集")](image-images/imageset04-large.png#lightbox)
    
特殊な**ベクター**クラスに追加された**画像セット**を含めることができます、 _PDF_形式の代わりに個々 のビットマップ ファイルを含む casset でベクター イメージさまざまな解像度。 1 つのベクトルのファイルを指定するためにこのメソッドを使用して、 **@1x** (ベクターの PDF ファイル形式) の解決と**@2x**と**@3x**ファイルのバージョンがコンパイル時に生成され、アプリケーションのバンドルに含まれます。

[![イメージ エディターのインターフェイスを設定する](image-images/imageset05.png "イメージ エディターのインターフェイスを設定します。")](image-images/imageset05-large.png#lightbox)

例では、含める場合は、 `MonkeyIcon.pdf` 150px x 150px、資産は、コンパイルされたときに、最終的なアプリ バンドルに含めると、次のビットマップの解像度のアセット カタログのベクターとしてファイル。

1. **MonkeyIcon@1x.png** -150px x 150px 解決します。
2. **MonkeyIcon@2x.png** -300 ピクセル x 300px 解決します。
3. **MonkeyIcon@3x.png** -450px x 450px 解決します。

次を考慮する資産カタログを PDF ベクター イメージを使用する場合。

- PDF がコンパイル時と、最終的なアプリケーションに付属してビットマップ ビットマップにラスタライズになるベクトルの完全なサポートはありません。
- アセット カタログに設定されていると、イメージのサイズを調整することはできません。 (またはコードに自動レイアウトとサイズ クラスを使用して) イメージのサイズを変更しようとした場合、イメージは他の任意のビットマップと同じようにゆがんで表示されます。

使用する場合、**セット イメージ**で Xcode の Interface Builder では、単に選択セットの名前で、ドロップダウン リストから、**属性インスペクター**: * *

![Xcode の Interface Builder で設定するイメージを選択する](image-images/imageset06.png "Xcode の Interface Builder で設定するイメージを選択します。")

<a name="Adding-new-Assets-Collections"/>

### <a name="adding-new-assets-collections"></a>新しい資産のコレクションを追加します。

すべての画像を追加する代わりに、新しいコレクションを作成する場合がある可能性があります資産カタログ内のイメージを使用する場合、 **Assets.xcassets**コレクション。 たとえば、次のように、オンデマンド リソースを設計するときです。

プロジェクトに新しいアセット カタログを追加するには

1. プロジェクトを右クリックし、 **Solution Pad**選択**追加** > **新しいファイル.**
2. 選択**Mac** > **資産カタログ**、入力、**名前**コレクションをクリックして、**新規**ボタン。 

    ![新しいアセット カタログを追加する](image-images/asset01.png "新しいアセット カタログの追加")

既定値として同じ方法でここからコレクションを使用して作業できる**Assets.xcassets**プロジェクトで自動的に含まれているコレクション。


### <a name="adding-images-to-resources"></a>リソースへの画像の追加

> [!IMPORTANT]
> Apple での macOS アプリで画像の操作には、このメソッドは廃止されました。 使用する必要があります[資産カタログの画像セット](#asset-catalogs)マネージャーにアプリのイメージの代わりにします。

プロジェクトの対象とする必要がありますが (c# コードで、またはインターフェイス ビルダーからか)、Xamarin.Mac アプリケーションでイメージ ファイルを使用する前に**リソース**とフォルダー、**バンドル リソース**します。 ファイルをプロジェクトに追加するには、次の操作を行います。

1. 右クリックし、**リソース**でプロジェクトのフォルダー、 **Solution Pad**選択**追加** > **ファイルを追加しています.**: 

    ![ファイルを追加する](image-images/add01.png "ファイルを追加します。")
2. **ファイルの追加** ダイアログ ボックスで、選択、プロジェクトに追加するファイルのイメージを選択`BundleResource`の**上書きビルド アクション** をクリックし、**オープン**ボタン。

    [![追加ファイルを選択する](image-images/add02.png "を追加するファイルの選択")](image-images/add02-large.png#lightbox)
3. ファイルがでない場合、**リソース**フォルダーを求められます場合**コピー**、**移動**または**リンク**ファイル。 すべてのスートを選択すると、必要に応じて通常は**コピー**:

    ![アクションの追加 を選択する](image-images/add04.png "アクションの追加 を選択します。")
4. 新しいファイルは、プロジェクトに追加し、使用するための読み取り。 

    ![Solution Pad に追加された新しいイメージ ファイル](image-images/add03.png "ソリューション パッドに追加された新しいイメージ ファイル")
5. 必要なすべてのイメージ ファイルのプロセスを繰り返します。

Xamarin.Mac アプリケーションで、任意の png、jpg、または pdf ファイルをソース イメージとして使用できます。 次のセクションで提供されたイメージの高解像度バージョンの追加について説明し、Retina をサポートするためにアイコン ベースの mac コンピューター。

> [!IMPORTANT]
> イメージを追加する場合、**リソース**フォルダー、おくことができます、**上書きビルド アクション**に設定**既定**します。 既定でこのフォルダーの ビルド アクションは`BundleResource`します。

## <a name="provide-high-resolution-versions-of-all-app-graphics-resources"></a>すべてのアプリのグラフィックス リソースの高解像度バージョンを提供します。

任意のグラフィック資産 (アイコン、カスタム コントロール、カスタム カーソル、カスタムのアートワークなど) は、Xamarin.Mac アプリケーションに追加するには、その標準解像度バージョンだけでなく高解像度バージョンが必要です。 Retina ディスプレイでの実行には、Mac コンピューターが搭載されているときに、アプリケーションが最善の状態が表示されるように、この機能が必要です。


### <a name="adopt-the-2x-naming-convention"></a>採用、@2x名前付け規則

> [!IMPORTANT]
> Apple での macOS アプリで画像の操作には、このメソッドは廃止されました。 使用する必要があります[資産カタログの画像セット](#asset-catalogs)マネージャーにアプリのイメージの代わりにします。

イメージのバージョンの標準と高解像度バージョンを作成するときに、Xamarin.Mac プロジェクトに含めるときにこのイメージのペアの名前付け規則に従います。

- **標準解像度**  - **ImageName.filename 拡張子**(例: **tags.png**)
- **高解像度**  -  **ImageName@2x.filename-extension** (例: **tags@2x.png**)

プロジェクトに追加すると、次のような。

![Solution Pad でイメージ ファイル](image-images/add03.png "Solution Pad でイメージ ファイル")

内のファイルを選択するイメージは、Interface Builder での UI 要素に割り当てられている場合だけですが、 _ImageName_**.**_ファイル名拡張子_形式 (例: **tags.png**)。 イメージを使用して c# コードで同じですを選択する内のファイル、 _ImageName_**.**_ファイル名拡張子_形式。

Mac では、Xamarin.Mac アプリケーションの実行時に、 _ImageName_**.**_ファイル名拡張子_形式のイメージは標準の解像度ディスプレイで使用される、 **ImageName@2x.filename-extension**は、イメージが自動的に取得する Retina ディスプレイは、mac コンピューターを行います。


## <a name="using-images-in-interface-builder"></a>インターフェイス ビルダーでイメージを使用します。

追加した、任意のイメージ リソース、**リソース**フォルダーで、Xamarin.Mac プロジェクト ビルド アクションを設定して**BundleResource**インターフェイス ビルダーで自動的に表示されますおよびできます(これには、イメージが処理されます) 場合、UI 要素の一部として選択されます。

インターフェイス ビルダーでのイメージを使用するには、次の操作を行います。

1. イメージの追加、**リソース**フォルダー、**ビルド アクション**の`BundleResource`: 

     ![Solution Pad でイメージ リソース](image-images/ib00.png "Solution Pad でイメージ リソース")
2. ダブルクリックして、 **Main.storyboard**ファイルを開き、Interface Builder での編集します。 

     [![メインのストーリー ボードを編集](image-images/ib01.png "メインのストーリー ボードの編集")](image-images/ib01-large.png#lightbox)
3. デザイン サーフェイスにイメージを UI 要素をドラッグします (たとえば、**イメージ ツール バー アイテム**)。 

     ![ツール バー アイテムの編集](image-images/ib02.png "ツール バー アイテムの編集")
4. 追加したイメージの選択、**リソース**フォルダーで、**イメージ名**ドロップダウン。 

     [![ツールバー項目のイメージを選択する](image-images/ib03.png "ツールバー項目のイメージを選択します。")](image-images/ib03-large.png#lightbox)
5. デザイン画面で、選択した画像が表示されます。 

     ![ツール バー エディターに表示されるイメージ](image-images/ib04.png "ツール バー エディターに表示されるイメージ")
6. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

上記の手順は、イメージのプロパティで設定することができる任意の UI 要素の動作、**属性インスペクター**します。 ここでも、選択した場合、 **@2x**イメージ ファイルのバージョンは、それが自動的に使用する Retina ディスプレイ ベースの mac コンピューター。

> [!IMPORTANT]
> イメージを使用できない場合、**イメージ名**ドロップダウンで、Xcode で .storyboard プロジェクト閉じてから Visual Studio for mac。 場合は、イメージを引き続き使用できないことを確認します。 その**ビルド アクション**は`BundleResource`イメージに追加されていると、**リソース**フォルダー。

## <a name="using-images-in-c-code"></a>C# コードでイメージを使用します。

イメージを格納するは、Xamarin.Mac アプリケーションで c# コードを使用してメモリにイメージを読み込むときに、`NSImage`オブジェクト。 イメージ ファイルは (リソースに含まれてください)、Xamarin.Mac アプリケーション バンドルに含まれる場合は、イメージの読み込みに次のコードを使用します。

```csharp
NSImage image = NSImage.ImageNamed("tags.png");
```

上記の例では、静的な`ImageNamed("...")`のメソッド、`NSImage`からメモリに指定したイメージを読み込むクラスを**リソース**フォルダーで、イメージが見つからない場合`null`が返されます。 インターフェイス ビルダーでは、割り当てられているイメージを選択した場合など、 **@2x**イメージ ファイルのバージョンは、それが自動的に使用する Retina ディスプレイ ベースの mac コンピューター。

アプリケーションのバンドル (Mac ファイル システム) から外のイメージを読み込むには、次のコードを使用します。

```csharp
NSImage image = new NSImage("/Users/KMullins/Documents/photo.jpg")
```

<a name="Working-with-Template-Images"/>

## <a name="working-with-template-images"></a>テンプレート イメージの操作

MacOS アプリの設計に基づき、ありますアイコンまたはイメージ内の (たとえば、ユーザー設定に基づく) の配色の変更に合わせてユーザー インターフェイスをカスタマイズする必要がある場合。

この効果を実現するために切り替え、_レンダリング モード_イメージ資産の**テンプレート イメージ**:

[![テンプレート イメージを設定する](image-images/templateimage01.png "テンプレート イメージの設定")](image-images/templateimage01-large.png#lightbox)

Xcode の Interface Builder からには、UI コントロールにイメージの資産を割り当てます。

![Xcode の Interface Builder でイメージを選択する](image-images/templateimage02.png "Xcode の Interface Builder でイメージを選択します。")

または、必要に応じてイメージ ソースをコードで設定。

```csharp
MyIcon.Image = NSImage.ImageNamed ("MessageIcon");
```

ビュー コント ローラーには、次のパブリック関数を追加します。

```csharp
public NSImage ImageTintedWithColor (NSImage image, NSColor tint)
{
    var tintedImage = image.Copy () as NSImage;
    var frame = new CGRect (0, 0, image.Size.Width, image.Size.Height);

    // Apply tint
    tintedImage.LockFocus ();
    tint.Set ();
    NSGraphics.RectFill (frame, NSCompositingOperation.SourceAtop);
    tintedImage.UnlockFocus ();
    tintedImage.Template = false;

    // Return tinted image
    return tintedImage;
}
```

最後に、テンプレート イメージを付けるには、色分けして表示するイメージに対してこの関数を呼び出します。

```csharp
MyIcon.Image = ImageTintedWithColor (MyIcon.Image, NSColor.Red);
```

<a name="Using_Images_with_Table_Views" />

## <a name="using-images-with-table-views"></a>テーブル ビューでのイメージの使用

内のセルの一部としてイメージを含める、 `NSTableView`、によって、テーブル ビューのデータを返す方法を変更する必要があります`NSTableViewDelegate's``GetViewForItem`メソッドを使用して、`NSTableCellView`ではなく、一般的な`NSTextField`します。 例えば:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = true;

        view.TextField.EditingEnded += (sender, e) => {

            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.TextField.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tags.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

ここで関心のあるいくつかの行があります。 まずにイメージを追加する列の場合作成、新しい`NSImageView`の必要なサイズと場所、また作成、新しい`NSTextField`し、イメージを使用したかどうかに基づいて既定の位置の配置。

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

次に、親に新しいイメージの表示とテキスト フィールドを含める必要があります`NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

最後に、圧縮およびテーブル ビューのセルに拡張できることに、テキスト フィールドを確認する必要があります。

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

出力例:

[![アプリで画像を表示する例](image-images/tables01.png "アプリで画像を表示する例")](image-images/tables01-large.png#lightbox)

テーブル ビューの使用の詳細についてを参照してください、[テーブル ビュー](~/mac/user-interface/table-view.md)ドキュメント。

<a name="Using_Images_with_Outline_Views" />

## <a name="using-images-with-outline-views"></a>イメージを使用して、アウトライン ビューの使用

内のセルの一部としてイメージを含める、 `NSOutlineView`、アウトライン ビューのデータを返す方法を変更する必要があります`NSTableViewDelegate's``GetView`メソッドを使用して、`NSTableCellView`ではなく、一般的な`NSTextField`します。 例えば:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

ここで関心のあるいくつかの行があります。 まずにイメージを追加する列の場合作成、新しい`NSImageView`の必要なサイズと場所、また作成、新しい`NSTextField`し、イメージを使用したかどうかに基づいて既定の位置の配置。

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

次に、親に新しいイメージの表示とテキスト フィールドを含める必要があります`NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

最後に、圧縮およびテーブル ビューのセルに拡張できることに、テキスト フィールドを確認する必要があります。

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

出力例:

[![アウトライン ビューで表示されるイメージの例](image-images/outline01.png "アウトライン表示で表示されるイメージの例")](image-images/outline01-large.png#lightbox)

アウトライン ビューの使用の詳細についてを参照してください、[アウトライン ビュー](~/mac/user-interface/outline-view.md)ドキュメント。


## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションでイメージとアイコンの使用方法について詳しく説明をしました。 さまざまな種類を説明し、イメージ、Xcode の Interface Builder でのイメージとアイコンを使用する方法、および c# コードでのイメージとアイコンを操作する方法を使用します。



## <a name="related-links"></a>関連リンク

- [MacImages (サンプル)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [テーブル ビュー](~/mac/user-interface/table-view.md)
- [アウトライン ビュー](~/mac/user-interface/outline-view.md)
- [macOS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [OS X 向けの高解像度について](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
