---
title: Xamarin. Mac のイメージ
description: この記事では、Xamarin. Mac アプリケーションでのイメージとアイコンの使用について説明します。 ここでは、アプリケーションのアイコンを作成し、C# コードと Xcode の Interface Builder の両方で画像を使用するために必要なイメージの作成と保守について説明します。
ms.prod: xamarin
ms.assetid: C6B539C2-FC6A-4C38-B839-32BFFB9B16A7
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/15/2017
ms.openlocfilehash: 53a3b43347e51cfbc99b2b798f0ad5c787f04847
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435552"
---
# <a name="images-in-xamarinmac"></a>Xamarin. Mac のイメージ

_この記事では、Xamarin. Mac アプリケーションでのイメージとアイコンの使用について説明します。ここでは、アプリケーションのアイコンを作成し、C# コードと Xcode の Interface Builder の両方で画像を使用するために必要なイメージの作成と保守について説明します。_

## <a name="overview"></a>概要

Xamarin. Mac アプリケーションで C# と .NET を使用する場合、 *Xcode および*で作業している開発者が行う*のと同じ*イメージおよびアイコンツールにアクセスできます。

MacOS (旧称 Mac OS X) アプリケーション内でイメージアセットを使用するには、いくつかの方法があります。 単にアプリケーションの UI の一部としてイメージを表示して、ツールバーやソースリスト項目などの UI コントロールに割り当てると、アイコンを提供できるようになり、次のようにして、macOS アプリケーションに優れたアートワークを簡単に追加できるようになります。 

- **UI 要素** -イメージは、イメージビュー () で背景またはアプリケーションの一部として表示でき `NSImageView` ます。
- **ボタン** -イメージはボタン () に表示でき `NSButton` ます。
- 画像**セル**-テーブルベースのコントロール (または) の一部として `NSTableView` 、画像 `NSOutlineView` セル () で画像を使用でき `NSImageCell` ます。
- **Toolbar 項目** -イメージ `NSToolbar` ツールバー項目 () として、ツールバー () に画像を追加でき `NSToolbarItem` ます。
- **ソースリストアイコン** -ソースリストの一部として (特別に書式設定さ `NSOutlineView` れた)。
- **アプリアイコン** -一連のイメージをまとめてグループ化し、 `.icns` アプリケーションのアイコンとして使用することができます。 詳細については、 [アプリケーションアイコン](~/mac/deploy-test/app-icon.md) のドキュメントを参照してください。

さらに、macOS には、アプリケーション全体で使用できる定義済みイメージのセットが用意されています。

[![アプリの実行例](image-images/intro01.png "アプリの実行例")](image-images/intro01-large.png#lightbox)

この記事では、Xamarin. Mac アプリケーションでイメージとアイコンを操作するための基本について説明します。 この記事で使用する主要な概念と手法について説明しているように、最初に [Hello, Mac](~/mac/get-started/hello-mac.md) の記事「 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 」と「 [アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions) 」セクションをご覧になることを強くお勧めします。

## <a name="adding-images-to-a-xamarinmac-project"></a>Xamarin. Mac プロジェクトへのイメージの追加

Xamarin. Mac アプリケーションで使用するイメージを追加する場合、開発者がプロジェクトのソースにイメージファイルを含めることができる場所と方法がいくつかあります。

- **メインプロジェクトツリー [非推奨]** -イメージをプロジェクトツリーに直接追加できます。 コードからメインプロジェクトツリーに格納されているイメージを呼び出すときに、フォルダーの場所が指定されていません。 (例: `NSImage image = NSImage.ImageNamed("tags.png");`)。 
- **Resources フォルダー [** 使用されていません]-特別な **リソース** フォルダーは、アプリケーションのバンドルの一部となる任意のファイル (アイコン、起動画面、一般的な画像 (または、開発者が追加しようとしている他のイメージまたはファイル) 用です。 メインプロジェクトツリーに格納されているイメージと同様に、 **Resources** フォルダーに格納されているイメージをコードから呼び出す場合、フォルダーの場所は指定されません。 (例: `NSImage.ImageNamed("tags.png")`)。
- **カスタムフォルダーまたはサブフォルダー [非推奨]** -開発者は、プロジェクトのソースツリーにカスタムフォルダーを追加し、そこにイメージを格納できます。 ファイルを追加する場所は、サブフォルダー内で入れ子にして、プロジェクトの整理に役立てることができます。 たとえば、開発者がプロジェクトにフォルダーを追加し、そのフォルダーにのサブフォルダーを追加し、そのフォルダーにJack.pngイメージを格納した場合、は `Card` `Hearts` **Jack.png** `Hearts` `NSImage.ImageNamed("Card/Hearts/Jack.png")` 実行時にイメージを読み込みます。
- **アセットカタログイメージセット [推奨]** -OS X El Capitan に追加された **資産カタログイメージセット** には、アプリケーションのさまざまなデバイスとスケールファクターをサポートするために必要なイメージのすべてのバージョンまたは表現が含まれています。 イメージ資産のファイル名 ( **@1x** 、) に依存するのではなく、 **@2x**

<a name="asset-catalogs"></a>

### <a name="adding-images-to-an-asset-catalog-image-set"></a>アセットカタログイメージセットへのイメージの追加

既に説明したように、 **資産カタログのイメージセット** には、アプリケーションのさまざまなデバイスやスケールファクターをサポートするために必要なイメージのすべてのバージョンまたは表現が含まれています。 イメージアセットファイル名を使用するのではなく (解像度に依存しない画像と画像の名前を参照してください)、 **イメージセット** は、アセットエディターを使用して、どのイメージがどのデバイスや解像度に属しているかを指定します。

1. **Solution Pad**で、 **Assets. xcassets**ファイルをダブルクリックして開き、編集します。 

    ![アセットの選択 (xcassets)](image-images/imageset01.png "アセットの選択 (xcassets)")
2. [ **アセット] ボックス** を右クリックし、[ **新しいイメージセット**] を選択します。 

    [![新しいイメージセットの追加](image-images/imageset02.png "新しいイメージセットの追加")](image-images/imageset02-large.png#lightbox)
3. 新しいイメージセットを選択すると、エディターが表示されます。 

    [![新しいイメージセットの選択](image-images/imageset03.png "新しいイメージセットの選択")](image-images/imageset03-large.png#lightbox)
4. ここでは、さまざまなデバイスや必要な解像度ごとにイメージをドラッグできます。 
5. [**アセット] ボックスの一覧**で、新しいイメージセットの**名前**をダブルクリックして編集します。 

    [![イメージセット名の編集](image-images/imageset04.png "イメージセット名の編集")](image-images/imageset04-large.png#lightbox)
    
**イメージセット**に追加された特殊な**ベクター**クラスであり、別の解像度で個別のビットマップファイルを含めずに、casset に_PDF_形式のベクターイメージを含めることができます。 このメソッドを使用すると、(ベクター PDF ファイルとして書式設定された) 解像度に対して1つのベクターファイルを指定 **@1x** し、 **@2x** **@3x** ファイルのバージョンとバージョンがコンパイル時に生成され、アプリケーションのバンドルに含まれます。

[![イメージセットエディターのインターフェイス](image-images/imageset05.png "イメージセットエディターのインターフェイス")](image-images/imageset05-large.png#lightbox)

たとえば、 `MonkeyIcon.pdf` 解像度が 150 px x 150 px の資産カタログのベクターとしてファイルを含めると、コンパイル時に次のビットマップ資産が最終的なアプリバンドルに含まれます。

1. **MonkeyIcon@1x.png** -150 px x 150 px 解像度。
2. **MonkeyIcon@2x.png** -300 x 300 resolution。
3. **MonkeyIcon@3x.png** -450px x 450px 解像度。

アセットカタログで PDF ベクターイメージを使用する場合は、次の点を考慮する必要があります。

- これは、コンパイル時に PDF がビットマップにラスタライズされ、最終的なアプリケーションに付属するビットマップになるため、完全なベクターサポートではありません。
- アセットカタログに画像を設定した後は、イメージのサイズを調整することはできません。 イメージのサイズを変更しようとした場合 (コードまたは Auto Layout クラスと Size クラスを使用して)、イメージは他のビットマップと同様に変形されます。

Xcode の Interface Builder で **イメージセット** を使用する場合は、 **属性インスペクター**のドロップダウンリストからセットの名前を選択するだけです。

![Xcode の Interface Builder でのイメージセットの選択](image-images/imageset06.png "Xcode の Interface Builder でのイメージセットの選択")

<a name="Adding-new-Assets-Collections"></a>

### <a name="adding-new-assets-collections"></a>新しいアセットコレクションの追加

Assets カタログ内のイメージを使用する場合、すべてのイメージを Assets に追加するのではなく、新しいコレクションを作成することが必要になる場合があり **ます。 xcassets** コレクション。 たとえば、オンデマンドリソースを設計する場合などです。

新しい Assets カタログをプロジェクトに追加するには、次のようにします。

1. **Solution Pad**でプロジェクトを右クリックし、[ **Add**  >  **新しいファイル**の追加] を選択します。
2. [ **Mac**  >  **資産カタログ**] を選択し、コレクションの**名前**を入力して、[**新規**] ボタンをクリックします。 

    ![新しい資産カタログの追加](image-images/asset01.png "新しい資産カタログの追加")

ここからは、既定のアセットと同じ方法でコレクションを操作でき **ます。 xcassets** コレクションは、プロジェクトに自動的に含まれます。

### <a name="adding-images-to-resources"></a>リソースへのイメージの追加

> [!IMPORTANT]
> MacOS アプリでイメージを操作するこの方法は、Apple によって非推奨とされています。 代わりに、 [アセットカタログのイメージセット](#asset-catalogs) を使用して、アプリのイメージを管理する必要があります。

Xamarin. Mac アプリケーションでイメージファイルを使用するには (C# コードまたは Interface Builder から)、プロジェクトの **Resources** フォルダーに **バンドルリソース**として含める必要があります。 プロジェクトにファイルを追加するには、次の手順を実行します。

1. **Solution Pad**でプロジェクトの**Resources**フォルダーを右クリックし、[追加] [ファイルの追加] の順に選択します。 **Add**  >  **Add Files...** 

    ![ファイルの追加](image-images/add01.png "ファイルの追加")
2. [ **ファイルの追加** ] ダイアログボックスで、プロジェクトに追加するイメージファイルを選択し、[ビルドの `BundleResource` オーバーライド] **アクション** に対してを選択し、[ **開く** ] ボタンをクリックします。

    [![追加するファイルの選択](image-images/add02.png "追加するファイルの選択")](image-images/add02-large.png#lightbox)
3. ファイルが **Resources** フォルダーに存在しない場合は、ファイルを **コピー**、 **移動** 、または **リンク** するかどうかを確認するメッセージが表示されます。 それぞれのニーズに合ったものを選択します。通常は **コピー**されます。

    ![[追加] アクションの選択](image-images/add04.png "[追加] アクションの選択")
4. 新しいファイルはプロジェクトに含まれ、使用するために読み込まれます。 

    ![Solution Pad に追加された新しいイメージファイル](image-images/add03.png "Solution Pad に追加された新しいイメージファイル")
5. 必要なすべてのイメージファイルに対して、この手順を繰り返します。

Xamarin. Mac アプリケーションでは、任意の png、jpg、または pdf ファイルをソースイメージとして使用できます。 次のセクションでは、Retina ベースの Mac をサポートするために、イメージとアイコンの高解像度バージョンを追加する方法について説明します。

> [!IMPORTANT]
> **リソース**フォルダーにイメージを追加する場合は、[**ビルドのオーバーライド] アクション**を [**既定**] に設定したままにすることができます。 このフォルダーの既定のビルドアクションは `BundleResource` です。

## <a name="provide-high-resolution-versions-of-all-app-graphics-resources"></a>すべてのアプリグラフィックスリソースの高解像度バージョンを提供する

Xamarin. Mac アプリケーションに追加するグラフィック資産 (アイコン、カスタムコントロール、カスタムカーソル、カスタムアートワークなど) には、標準解像度のバージョンに加えて、高解像度バージョンが必要です。 これは、Retina に装備されている Mac コンピューターで実行した場合にアプリケーションが最適な状態になるようにするために必要です。

### <a name="adopt-the-2x-naming-convention"></a>@2x名前付け規則を採用する

> [!IMPORTANT]
> MacOS アプリでイメージを操作するこの方法は、Apple によって非推奨とされています。 代わりに、 [アセットカタログのイメージセット](#asset-catalogs) を使用して、アプリのイメージを管理する必要があります。

イメージの標準解像度と高解像度バージョンを作成する場合は、次の名前付け規則に従ってイメージのペアを Xamarin. Mac プロジェクトに含めます。

- **標準-解像度**   - **ImageName. ファイル名拡張子**(例: **tags.png**)
- **高解像度**   -  **ImageName@2x.filename-extension**(例: **tags@2x.png** )

プロジェクトに追加すると、次のように表示されます。

![Solution Pad 内のイメージファイル](image-images/add03.png "Solution Pad 内のイメージファイル")

イメージが Interface Builder の UI 要素に割り当てられている場合は、そのファイルを_ImageName_内で単に選択し**ます。**_ファイル名拡張子の_形式 (例: **tags.png**)。 C# コードでイメージを使用する場合と同じように、 _ImageName_でファイルを選択し**ます。**_ファイル名-拡張子の_形式。

Xamarin アプリケーションが Mac で実行されている場合は、 _ImageName_**。**_ファイル名-拡張子_ の形式の画像は、標準的な解像度の表示で使用され **ImageName@2x.filename-extension** ます。イメージは、Retina ディスプレイベースの mac で自動的に選択されます。

## <a name="using-images-in-interface-builder"></a>Interface Builder でのイメージの使用

Xamarin. Mac プロジェクトの **Resources** フォルダーに追加したイメージリソースが **BundleResource** に設定されている場合は、自動的に Interface Builder に表示され、UI 要素の一部として選択できます (イメージを処理する場合)。

Interface builder でイメージを使用するには、次の手順を実行します。

1. 次の**ビルドアクション**を使用して、**リソース**フォルダーにイメージを追加し `BundleResource` ます。 

     ![Solution Pad 内のイメージリソース](image-images/ib00.png "Solution Pad 内のイメージリソース")
2. メインの storyboard ファイルをダブルクリックして、Interface Builder で編集するために開き**ます。** 

     [![メインストーリーボードの編集](image-images/ib01.png "メインストーリーボードの編集")](image-images/ib01-large.png#lightbox)
3. 画像を撮影する UI 要素をデザイン画面にドラッグします (たとえば、 **イメージツールバー項目**)。 

     ![ツールバー項目の編集](image-images/ib02.png "ツールバー項目の編集")
4. [**イメージ名**] ボックスの一覧で、 **Resources**フォルダーに追加したイメージを選択します。 

     [![ツールバーアイテムの画像の選択](image-images/ib03.png "ツールバーアイテムの画像の選択")](image-images/ib03-large.png#lightbox)
5. 選択したイメージがデザイン画面に表示されます。 

     ![ツールバーエディターに表示されているイメージ](image-images/ib04.png "ツールバーエディターに表示されているイメージ")
6. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

上記の手順は、 **属性インスペクター**で image プロパティを設定できる UI 要素に対して機能します。 ここでも、イメージファイルのバージョンを含めた場合は、 **@2x** Retina ディスプレイベースの mac で自動的に使用されます。

> [!IMPORTANT]
> [ **イメージ名** ] ボックスの一覧にイメージが表示されない場合は、Xcode で storyboard プロジェクトを閉じ、Visual Studio for Mac から再度開きます。 イメージがまだ使用できない場合は、 **ビルドアクション** がであり、 `BundleResource` イメージが **Resources** フォルダーに追加されていることを確認します。

## <a name="using-images-in-c-code"></a>C# コードでのイメージの使用

Xamarin. Mac アプリケーションの C# コードを使用してイメージをメモリに読み込む場合、イメージはオブジェクトに格納され `NSImage` ます。 イメージファイルが Xamarin. Mac アプリケーションバンドル (リソースに含まれています) に含まれている場合は、次のコードを使用してイメージを読み込みます。

```csharp
NSImage image = NSImage.ImageNamed("tags.png");
```

上のコードでは、 `ImageNamed("...")` クラスの静的メソッドを使用して、 `NSImage` 指定されたイメージを **リソース** フォルダーからメモリに読み込みます。イメージが見つからない場合は、 `null` が返されます。 Interface Builder で割り当てられたイメージと同様に、イメージファイルのバージョンを含めた場合は、 **@2x** Retina ディスプレイベースの mac で自動的に使用されます。

(Mac ファイルシステムから) アプリケーションのバンドルの外部にイメージを読み込むには、次のコードを使用します。

```csharp
NSImage image = new NSImage("/Users/KMullins/Documents/photo.jpg")
```

<a name="Working-with-Template-Images"></a>

## <a name="working-with-template-images"></a>テンプレートイメージの操作

MacOS アプリの設計に基づいて、ユーザーインターフェイス内のアイコンまたはイメージをカスタマイズして、配色の変化 (ユーザー設定に基づくなど) と一致させることが必要になる場合があります。

この効果を実現するには、イメージ資産の _レンダリングモード_ を **テンプレートイメージ**に切り替えます。

[![テンプレートイメージの設定](image-images/templateimage01.png "テンプレートイメージの設定")](image-images/templateimage01-large.png#lightbox)

Xcode の Interface Builder から、イメージ資産を UI コントロールに割り当てます。

![Xcode の Interface Builder でのイメージの選択](image-images/templateimage02.png "Xcode の Interface Builder でのイメージの選択")

または、必要に応じて、コードでイメージソースを設定します。

```csharp
MyIcon.Image = NSImage.ImageNamed ("MessageIcon");
```

次のパブリック関数をビューコントローラーに追加します。

```csharp
public NSImage ImageTintedWithColor(NSImage sourceImage, NSColor tintColor)
    => NSImage.ImageWithSize(sourceImage.Size, false, rect => {
        // Draw the original source image
        sourceImage.DrawInRect(rect, CGRect.Empty, NSCompositingOperation.SourceOver, 1f);

        // Apply tint
        tintColor.Set();
        NSGraphics.RectFill(rect, NSCompositingOperation.SourceAtop);

        return true;
    });
```

> [!IMPORTANT]
> 特に macOS Mojave でのダークモードの登場では、 `LockFocus` カスタムレンダリングオブジェクトを reating するときに API を回避することが重要です `NSImage` 。 このようなイメージは静的になり、外観や表示密度の変化に対応するために自動的に更新されることはありません。
>
> 上のハンドラーベースの機構を使用することにより、がでホストされている場合 (など)、動的条件の再レンダリングが自動的に行われ `NSImage` `NSImageView` ます。

最後に、テンプレートイメージの濃淡を付けるには、イメージに対してこの関数を呼び出して、色分けします。

```csharp
MyIcon.Image = ImageTintedWithColor (MyIcon.Image, NSColor.Red);
```

<a name="Using_Images_with_Table_Views"></a>

## <a name="using-images-with-table-views"></a>テーブルビューでのイメージの使用

画像を内のセルの一部として含めるには、 `NSTableView` テーブルビューのメソッドによるデータの取得方法を変更して、 `NSTableViewDelegate's` `GetViewForItem` `NSTableCellView` 通常のの代わりにを使用する必要があり `NSTextField` ます。 次に例を示します。

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

ここにはいくつかの興味深い行があります。 まず、イメージを含める列に対して、 `NSImageView` 必要なサイズと場所の新しいを作成し `NSTextField` ます。また、イメージを使用しているかどうかに基づいて、新しいを作成し、既定の位置を配置します。

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

次に、新しいイメージビューとテキストフィールドを親に含める必要があり `NSTableCellView` ます。

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

最後に、テキストフィールドに、テーブルビューセルを使用して圧縮および拡張できることを通知する必要があります。

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

出力例:

[![アプリでイメージを表示する例](image-images/tables01.png "アプリでイメージを表示する例")](image-images/tables01-large.png#lightbox)

テーブルビューの操作の詳細については、 [テーブルビュー](~/mac/user-interface/table-view.md) のドキュメントを参照してください。

<a name="Using_Images_with_Outline_Views"></a>

## <a name="using-images-with-outline-views"></a>アウトラインビューでのイメージの使用

画像を内のセルの一部として含めるには、 `NSOutlineView` アウトラインビューのメソッドによるデータの取得方法を変更して、 `NSTableViewDelegate's` `GetView` `NSTableCellView` 通常のの代わりにを使用する必要があり `NSTextField` ます。 次に例を示します。

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

ここにはいくつかの興味深い行があります。 まず、イメージを含める列に対して、 `NSImageView` 必要なサイズと場所の新しいを作成し `NSTextField` ます。また、イメージを使用しているかどうかに基づいて、新しいを作成し、既定の位置を配置します。

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

次に、新しいイメージビューとテキストフィールドを親に含める必要があり `NSTableCellView` ます。

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

最後に、テキストフィールドに、テーブルビューセルを使用して圧縮および拡張できることを通知する必要があります。

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

出力例:

[![アウトラインビューに表示されるイメージの例](image-images/outline01.png "アウトラインビューに表示されるイメージの例")](image-images/outline01-large.png#lightbox)

アウトラインビューの操作の詳細については、 [アウトラインビュー](~/mac/user-interface/outline-view.md) のドキュメントを参照してください。

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリケーションでイメージとアイコンを操作する方法について詳しく説明しました。 ここでは、さまざまな種類と画像の使用方法、Xcode の Interface Builder での画像とアイコンの使用方法、C# コードで画像とアイコンを操作する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [MacImages (サンプル)](/samples/xamarin/mac-samples/macimages)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [テーブルビュー](~/mac/user-interface/table-view.md)
- [アウトラインビュー](~/mac/user-interface/outline-view.md)
- [macOS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [OS X 向けの高解像度について](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)