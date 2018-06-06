---
title: Xamarin.Mac 内のイメージ
description: この記事では、イメージやアイコン Xamarin.Mac アプリケーションでの操作について説明します。 作成し、アプリケーションのアイコンを作成するために必要し、c# コードと Xcode のインターフェイスのビルダーの両方でイメージを使用して、イメージの管理について説明します。
ms.prod: xamarin
ms.assetid: C6B539C2-FC6A-4C38-B839-32BFFB9B16A7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: ae0a42051d56eb8bf002c61dfbc60c99924ff301
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792097"
---
# <a name="images-in-xamarinmac"></a>Xamarin.Mac 内のイメージ

_この記事では、イメージやアイコン Xamarin.Mac アプリケーションでの操作について説明します。作成し、アプリケーションのアイコンを作成するために必要し、c# コードと Xcode のインターフェイスのビルダーの両方でイメージを使用して、イメージの管理について説明します。_

## <a name="overview"></a>概要

Xamarin.Mac アプリケーションでは、c# と .NET で作業するとき、同じイメージへのアクセスがあり、アイコンをツールで作業する開発者*OBJECTIVE-C*と*Xcode*はします。

いくつか方法があります (旧称 Mac OS X) macOS アプリケーション内の資産で使用されるイメージです。 単にアイコンを提供することをツール バーまたはソース リスト項目などの UI コントロールに割り当てると、アプリケーションの UI の一部としてイメージが表示されない Xamarin.Mac 簡単に次のように、優れたアートワークを macOS アプリケーションに追加: 

- **UI 要素**-背景として、またはイメージ ビューで、アプリケーションの一部として、画像を表示できます (`NSImageView`)。
- **ボタン**-イメージはボタンに表示されることができます (`NSButton`)。
- **セルのイメージ**ベース テーブルのコントロールの一部として (`NSTableView`または`NSOutlineView`)、画像のセルでは、イメージを使用することができます (`NSImageCell`)。
- **ツールバー項目**-イメージをツールバーに追加することができます (`NSToolbar`) イメージのツールバー項目として (`NSToolbarItem`)。
- **ソース リスト アイコン**- ソース リストの一部として (特別にフォーマットされた`NSOutlineView`)。
- **アプリのアイコン**-に一連のイメージをグループ化できる、`.icns`に設定して、アプリケーションのアイコンとして使用します。 参照してください、[アプリケーション アイコン](~/mac/deploy-test/app-icon.md)詳細についてはドキュメントです。

さらに、macOS は、アプリケーション全体で使用できる定義済みのイメージのセットを提供します。

[![例は、アプリの実行](image-images/intro01.png "例は、アプリの実行")](image-images/intro01-large.png#lightbox)

この記事で Xamarin.Mac アプリケーションでイメージやアイコンの操作の基礎について説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)と[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)セクションでは、これとは、主な概念と、この記事で使用する方法について説明します。


## <a name="adding-images-to-a-xamarinmac-project"></a>Xamarin.Mac プロジェクトにイメージを追加します。

を Xamarin.Mac アプリケーションで使用するイメージを追加する場合があるいくつかの場所と方法は、開発者がプロジェクトのソースにイメージ ファイルを含めることができます。

- **[Deprecated] メイン プロジェクト ツリー** -イメージは、プロジェクト ツリーに直接追加することができます。 メイン プロジェクト ツリーをコードからに格納されているイメージを呼び出すときにフォルダーの場所が指定されていません。 たとえば、`NSImage image = NSImage.ImageNamed("tags.png");` のように指定します。 
- **[Deprecated] のリソース フォルダー** -特殊**リソース**フォルダーは、画像 (またはその他のイメージやファイル、開発者、アイコン、画面の起動または一般的ななど、アプリケーションの一部となるすべてのファイルがバンドル 's のしようとする追加) します。 格納されているイメージを呼び出すときに、**リソース**コードからフォルダーをメイン プロジェクト ツリーに格納されているイメージと同様、フォルダーの場所が指定されていません。 たとえば、`NSImage.ImageNamed("tags.png")` のように指定します。
- **カスタムのフォルダーまたは [deprecated] サブフォルダー** -開発者がプロジェクトのソース ツリーにカスタム フォルダーを追加およびがあるイメージを格納します。 ファイルを追加する場所は、プロジェクトを整理するためのサブフォルダーに入れ子にすることができます。 たとえば、開発者が追加された場合、`Card`フォルダーをプロジェクトとのサブ フォルダーに`Hearts`そのフォルダーにイメージを格納し、 **Jack.png**で、`Hearts`フォルダー、`NSImage.ImageNamed("Card/Hearts/Jack.png")`でイメージの読み込みがランタイム。
- **[優先] アセット カタログのイメージ セット**- 追加の OS X 許可されて、**アセット カタログのイメージ セット**を含むすべてのバージョンをさまざまなデバイスをサポートし、規模の要素のために必要なイメージの表現、アプリケーション。 イメージの資産ファイル名ではなく (**@1x**、 **@2x**)。

<a name="asset-catalogs" />

### <a name="adding-images-to-an-asset-catalog-image-set"></a>設定を資産カタログ イメージにイメージを追加します。

、前述のよう、**アセット カタログのイメージ セット**すべてのバージョンまたはさまざまなデバイスをサポートし、規模のアプリケーションの要素に必要なイメージの表現が含まれています。 イメージの資産ファイル名ではなく (解像度の独立したイメージとイメージの命名規則が上記を参照)、**イメージ セット**資産エディターを使用して、どのデバイスや解像度に属しているどのイメージを指定します。

1. **ソリューション パッド**をダブルクリックして、 **Assets.xcassets**ファイルを開いて編集するファイル。 

    ![選択、Assets.xcassets](image-images/imageset01.png "Assets.xcassets を選択します。")
2. 右クリックし、**資産一覧**選択**イメージは新しい設定**: 

    [![新しいイメージ セットを追加する](image-images/imageset02.png "新しいイメージ セットを追加します。")](image-images/imageset02-large.png#lightbox)
3. 新しいイメージ セットを選択し、エディターが表示されます。 

    [![新しいイメージ セットを選択する](image-images/imageset03.png "新しいイメージ セットを選択します。")](image-images/imageset03-large.png#lightbox)
4. ここでは異なるデバイスや必要な解像度の各イメージでおドラッグできます。 
5. 新しいイメージ セットをダブルクリックして**名前**で、**資産一覧**編集します。 

    [![名前を設定、画像を編集](image-images/imageset04.png "名の設定、画像の編集")](image-images/imageset04-large.png#lightbox)
    
特殊な**ベクター**クラスに追加された**イメージ セット**を含めることができます、 _PDF_形式の代わりに個々 のビットマップ ファイルを含む casset でベクター イメージ別の解像度。 1 つのベクトル ファイルを指定するためにこのメソッドを使用して、 **@1x** (ベクター PDF ファイル形式) の解決と**@2x**と**@3x**のバージョンのファイルがコンパイル時に生成され、アプリケーションのバンドルに含まれます。

[![イメージ エディターのインターフェイスを設定する](image-images/imageset05.png "イメージ エディターのインターフェイスを設定します。")](image-images/imageset05-large.png#lightbox)

含めた場合など、 `MonkeyIcon.pdf` 150px x 150px、コンパイルされたときに、最終的なアプリのバンドルに含まれます資産次ビットマップの解像度を持つアセット カタログのベクトルとしてファイル。

1. **MonkeyIcon@1x.png** -150px x 150px 解決します。
2. **MonkeyIcon@2x.png** -300 ピクセル x 300px 解決します。
3. **MonkeyIcon@3x.png** -450px x 450px 解決します。

次を考慮する資産カタログで PDF ベクター イメージを使用する場合。

- PDF がコンパイル時と最終的なアプリケーションに付属してビットマップのビットマップにラスタライズになる完全ベクターのサポートはありません。
- アセット カタログに設定されると、イメージのサイズを調整することはできません。 (またはのいずれかのコードに自動レイアウトおよびサイズのクラスを使用して) イメージのサイズを変更しようとする場合は、他の任意のビットマップと同じようにイメージに音がひずみます。

使用する場合、**イメージ設定**ビルダーでは Xcode のインターフェイス、クリックするだけ、セットの名前で、ドロップダウン リストから、**属性インスペクター**: * *

![Xcode のインターフェイスのビルダーで設定イメージを選択する](image-images/imageset06.png "Xcode のインターフェイスのビルダーで設定イメージを選択します。")

<a name="Adding-new-Assets-Collections"/>

### <a name="adding-new-assets-collections"></a>新しい資産のコレクションを追加します。

すべての画像を追加する代わりに、新しいコレクションを作成する場合がある可能性がありますアセット カタログ内のイメージを使用する場合、 **Assets.xcassets**コレクション。 たとえば、オンデマンドでリソースを設計するときです。

プロジェクトに新しい資産カタログを追加します。

1. プロジェクトを右クリックし、**ソリューション パッド**選択**追加** > **新しいファイル.**
2. 選択**Mac** > **アセット カタログ**、入力、**名前**をクリックして、コレクションの**新規**ボタン。 

    ![新しい資産カタログを追加する](image-images/asset01.png "新しい資産カタログを追加します。")

既定値と同じ方法でここからコレクションを使用して作業できます**Assets.xcassets**プロジェクトに自動的に含まれているコレクション。


### <a name="adding-images-to-resources"></a>リソースへのイメージの追加

> [!IMPORTANT]
> Apple の macOS アプリで画像の操作には、このメソッドは廃止されました。 使用する必要があります[アセット カタログのイメージ セット](#asset-catalogs)マネージャーにアプリのイメージの代わりにします。

またはのいずれかの c# コードをインターフェイスのビルダーから) Xamarin.Mac アプリケーションでイメージ ファイルを使用する前に、プロジェクトに含まれる必要があります**リソース**とフォルダー、**バンドル リソース**です。 プロジェクトにファイルを追加するには、次の操作を行います。

1. 右クリックし、**リソース**でプロジェクトのフォルダーに、**ソリューション パッド**選択**追加** > **ファイルを追加しています.**: 

    ![ファイルを追加する](image-images/add01.png "ファイルを追加します。")
2. **ファイルの追加** ダイアログ ボックスで、イメージは、プロジェクトに追加するファイルを選択`BundleResource`の**上書きビルド アクション** をクリックし、**開く**ボタン。

    [![追加するファイルを選択すると](image-images/add02.png "を追加するファイルの選択")](image-images/add02-large.png#lightbox)
3. ファイルがでない場合、**リソース**フォルダー、たずねられますたいかどうか**コピー**、**移動**または**リンク**ファイル。 すべてのスイートを選択すると、必要に応じて通常は**コピー**:

    ![アクションの追加 を選択すると](image-images/add04.png "アクションの追加 を選択します。")
4. 新しいファイルは、プロジェクトに含まれ、使用するための読み取り。 

    ![ソリューションのパッドに追加された新しいイメージ ファイル](image-images/add03.png "ソリューション パッドに追加された新しいイメージ ファイル")
5. 必要なすべてのイメージ ファイルをプロセスを繰り返します。

Xamarin.Mac アプリケーションで、ソース イメージとして、png、jpg、または pdf ファイルを使用できます。 次のセクションで、イメージの高解像度バージョンを追加することに紹介し、Retina をサポートするためにアイコンが mac コンピューターをベースです。

> [!IMPORTANT]
> イメージを追加する場合は、**リソース**フォルダー、おくことができます、**上書きビルド アクション**に設定**既定**です。 既定値は、このフォルダーのビルド アクション`BundleResource`です。

## <a name="provide-high-resolution-versions-of-all-app-graphics-resources"></a>すべてのアプリのグラフィックス リソースの高解像度バージョンを提供します。

任意グラフィックをアセット (アイコン、カスタム コントロール、カスタムのカーソル、カスタムのアートワークなど) は、Xamarin.Mac アプリケーションに追加するには、その標準の解像度バージョンだけでなく高解像度バージョンが必要です。 これは、機能は、Retina ディスプレイでの実行には、Mac コンピューターが装備されているときに、アプリケーションは、パフォーマンスを最大になりますように必要です。


### <a name="adopt-the-2x-naming-convention"></a>採用、@2x名前付け規則

> [!IMPORTANT]
> Apple の macOS アプリで画像の操作には、このメソッドは廃止されました。 使用する必要があります[アセット カタログのイメージ セット](#asset-catalogs)マネージャーにアプリのイメージの代わりにします。

イメージのバージョンの標準と高解像度バージョンを作成するときに、このイメージのペアの名前付け規則、Xamarin.Mac プロジェクトに含めるときに従います。

- **標準解像度**  - **ImageName.filename 拡張子**(例: **tags.png**)
- **高解像度**  -  **ImageName@2x.filename-extension** (例: **tags@2x.png**)

プロジェクトに追加すると、次のような。

![ソリューションのパッドにあるイメージ ファイル](image-images/add03.png "ソリューション パッドにあるイメージ ファイル")

内のファイルを選択するイメージがインターフェイスのビルダーでの UI 要素に割り当てられたときにだけが、 _ImageName_**.**_ファイル名拡張子_形式 (例: **tags.png**)。 同じイメージを使用して、c# コードを選択する内のファイル、 _ImageName_**.**_ファイル名拡張子_形式です。

Mac では、する Xamarin.Mac アプリケーションを実行すると、 _ImageName_**.**_ファイル名拡張子_形式の画像は標準の解像度表示に使用される、 **ImageName@2x.filename-extension**は、イメージが自動的に取得する mac コンピューターの基本 Retina ディスプレイです。


## <a name="using-images-in-interface-builder"></a>インターフェイスのビルダーでのイメージの使用

追加した、任意のイメージ リソース、**リソース**フォルダー、Xamarin.Mac にプロジェクトを設定しているビルド アクション**BundleResource**インターフェイスのビルダーに自動的に表示されますでき、(これには、イメージが処理されます) 場合は、UI 要素の一部として選択されます。

インターフェイスのビルダーでのイメージを使用するには、次の操作を行います。

1. イメージの追加、**リソース**フォルダーが、**ビルド アクション**の`BundleResource`: 

     ![ソリューションのパッド内のイメージ リソース](image-images/ib00.png "ソリューション パッド内のイメージ リソース")
2. ダブルクリックして、 **Main.storyboard**編集のためインターフェイス ビルダーで開くファイル。 

     [![メインのストーリー ボードの編集](image-images/ib01.png "メインのストーリー ボードの編集")](image-images/ib01-large.png#lightbox)
3. デザイン サーフェイスにイメージとなる UI 要素をドラッグして (たとえば、**イメージ ツールバー項目**)。 

     ![ツール バー アイテムの編集](image-images/ib02.png "ツールバー項目の編集")
4. 追加したイメージを選択、**リソース**内のフォルダー、**イメージ名**ドロップダウンします。 

     [![ツールバー項目のイメージを選択する](image-images/ib03.png "はツールバー項目のイメージを選択します。")](image-images/ib03-large.png#lightbox)
5. 選択したイメージがデザイン画面に表示されます。 

     ![ツール バー エディターに表示されるイメージ](image-images/ib04.png "ツール バー エディターに表示されるイメージ")
6. 変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

上記の手順は、イメージのプロパティで設定するのには、すべての UI 要素について作業、**属性インスペクター**です。 ここでも、選択した場合、 **@2x**イメージ ファイルのバージョンは、これは自動的に使用する Retina ディスプレイ ベース Mac。

> [!IMPORTANT]
> イメージがで使用できない場合、**イメージ名**ドロップダウン リストで、プロジェクトを閉じて、.storyboard Xcode で、後で再び開くから Visual Studio for mac イメージをまだ使用できない場合いることを確認、**ビルド アクション**は`BundleResource`イメージに追加されたことと、**リソース**フォルダーです。

## <a name="using-images-in-c-code"></a>C# コードでのイメージの使用

イメージが格納される Xamarin.Mac アプリケーションでの c# コードを使用してメモリにイメージを読み込むときに、`NSImage`オブジェクト。 イメージ ファイルが (リソースに含まれてください)、Xamarin.Mac アプリケーション バンドルに含まれている場合は、イメージの読み込みに次のコードを使用します。

```csharp
NSImage image = NSImage.ImageNamed("tags.png");
```

上記の例では、静的な`ImageNamed("...")`のメソッド、`NSImage`からメモリに指定されたイメージの読み込みにクラス、**リソース**フォルダーで、イメージが見つからない場合`null`が返されます。 インターフェイス ビルダーでは、割り当てられているイメージを選択した場合と同様に、 **@2x**イメージ ファイルのバージョンは、これは自動的に使用する Retina ディスプレイ ベース Mac。

アプリケーションのバンドル (Mac ファイル システム) からの外部でイメージを読み込むには、次のコードを使用します。

```csharp
NSImage image = new NSImage("/Users/KMullins/Documents/photo.jpg")
```

<a name="Working-with-Template-Images"/>

## <a name="working-with-template-images"></a>テンプレート イメージの操作

MacOS アプリの設計に基づき、ありますアイコンまたは画面の配色 (たとえば、ユーザー設定に基づいた) の変更を一致するように、ユーザー インターフェイスの内部でイメージをカスタマイズする必要がある場合。

この特殊効果を実現するために切り替え、_レンダリング モード_イメージ資産の**テンプレート イメージ**:

[![テンプレート イメージを設定する](image-images/templateimage01.png "テンプレート イメージを設定します。")](image-images/templateimage01-large.png#lightbox)

Xcode のインターフェイスのビルダーからは、UI コントロールにイメージ アセットを割り当てます。

![Xcode のインターフェイスのビルダーでイメージを選択する](image-images/templateimage02.png "Xcode のインターフェイスのビルダーでイメージを選択します。")

または、必要に応じてイメージ ソースをコードで設定。

```csharp
MyIcon.Image = NSImage.ImageNamed ("MessageIcon");
```

ビュー、コント ローラーには、次のパブリック関数を追加します。

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

最後には、テンプレート イメージを付けるには、色分けして表示するイメージに対してこの関数を呼び出します。

```csharp
MyIcon.Image = ImageTintedWithColor (MyIcon.Image, NSColor.Red);
```

<a name="Using_Images_with_Table_Views" />

## <a name="using-images-with-table-views"></a>テーブル ビューでのイメージの使用

内のセルの一部として画像を含める、 `NSTableView`、データをテーブル ビューのによって返される方法を変更する必要があります`NSTableViewDelegate's``GetViewForItem`メソッドを使用して、`NSTableCellView`ではなく、一般的な`NSTextField`します。 例えば:

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

ここでは、複数の行があります。 最初に、イメージを対象にする列の場合は、作成、新しい`NSImageView`、必要なサイズと場所の私たちもを新規作成`NSTextField`し、イメージを使用したかどうかに基づいて既定の位置を配置します。

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

最後に、ことができる拡大および縮小テーブル ビューのセルにテキスト フィールドを確認する必要があります。

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

出力例:

[![アプリで画像を表示する例](image-images/tables01.png "アプリでのイメージの表示の例")](image-images/tables01-large.png#lightbox)

テーブルのビューの操作の詳細についてを参照してください、[テーブル ビュー](~/mac/user-interface/table-view.md)ドキュメント。

<a name="Using_Images_with_Outline_Views" />

## <a name="using-images-with-outline-views"></a>アウトライン ビューでのイメージの使用

内のセルの一部として画像を含める、 `NSOutlineView`、アウトライン表示のによってデータが返される方法を変更する必要があります`NSTableViewDelegate's``GetView`メソッドを使用して、`NSTableCellView`ではなく、一般的な`NSTextField`します。 例えば:

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

ここでは、複数の行があります。 最初に、イメージを対象にする列の場合は、作成、新しい`NSImageView`、必要なサイズと場所の私たちもを新規作成`NSTextField`し、イメージを使用したかどうかに基づいて既定の位置を配置します。

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

最後に、ことができる拡大および縮小テーブル ビューのセルにテキスト フィールドを確認する必要があります。

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

出力例:

[![アウトライン表示で表示されるイメージの例](image-images/outline01.png "アウトライン ビューで表示されるイメージの例")](image-images/outline01-large.png#lightbox)

アウトライン ビューの使用の詳細についてを参照してください、[アウトライン ビュー](~/mac/user-interface/outline-view.md)ドキュメント。


## <a name="summary"></a>まとめ

この記事では、イメージとアイコンが付いた Xamarin.Mac アプリケーションでの作業についての詳細を取得しました。 お、さまざまな種類の説明しのイメージ、イメージやアイコンを Xcode のインターフェイスのビルダーで使用する方法、およびイメージとアイコンの c# コードで作業する方法を使用します。



## <a name="related-links"></a>関連リンク

- [MacImages (サンプル)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [テーブルのビュー](~/mac/user-interface/table-view.md)
- [アウトライン ビュー](~/mac/user-interface/outline-view.md)
- [macOS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [OS X 向けの高解像度について](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
