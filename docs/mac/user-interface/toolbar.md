---
title: Xamarin. Mac のツールバー
description: この記事では、Xamarin. Mac アプリケーションでのツールバーの使用について説明します。 Xcode と Interface Builder でのツールバーの作成と保守、コードへの公開、プログラムによる作業について説明します。
ms.prod: xamarin
ms.assetid: C8D228CE-C860-47E1-85FD-69864BF91F20
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: cd2490bfad880d128f5eaeebd4aac58ad3a4d8fa
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "70772723"
---
# <a name="toolbars-in-xamarinmac"></a>Xamarin. Mac のツールバー

_この記事では、Xamarin. Mac アプリケーションでのツールバーの使用について説明します。Xcode と Interface Builder でのツールバーの作成と保守、コードへの公開、プログラムによる作業について説明します。_

Visual Studio for Mac と連携する Xamarin の開発者は、ツールバーコントロールなど、Xcode を操作する macOS 開発者が使用できるものと同じ UI コントロールにアクセスできます。 Xcode は直接統合されているため、Xcode の Interface Builder を使用して、ツールバー項目を作成および管理することができます。 これらのツールバー項目は、でC#も作成できます。

MacOS のツールバーは、ウィンドウの上部のセクションに追加され、その機能に関連するコマンドに簡単にアクセスできるようになります。 ツールバーは、アプリケーションのユーザーが非表示にしたり、表示したり、カスタマイズしたりすることができます。また、さまざまな方法でツールバー項目を表示することもできます。

この記事では、Xamarin. Mac アプリケーションでのツールバーとツールバー項目の操作の基本について説明します。 

続行する前に、 [Hello, Mac](~/mac/get-started/hello-mac.md)に関する記事を参照してください。具体的には、このガイド全体で使用される主要な概念と手法について説明している、「 [Xcode And Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)と[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)の概要」セクションを参照してください。

また、「 [Xamarin. Mac の内部](~/mac/internals/how-it-works.md)ドキュメント」の「[クラス/メソッドを目的の C に公開C#する](~/mac/internals/how-it-works.md)」セクションを参照してください。 ここでは、クラスを目的の C クラスC#に接続するために使用される `Register` および `Export` の属性について説明します。

## <a name="introduction-to-toolbars"></a>ツールバーの概要

MacOS アプリケーションのウィンドウには、ツールバーを含めることができます。

![ツールバーのあるサンプルウィンドウ](toolbar-images/info01.png "ツールバーのあるサンプルウィンドウ")

ツールバーは、アプリケーションのユーザーが重要な機能や一般的に使用される機能にすばやくアクセスするための簡単な方法を提供します。 たとえば、ドキュメント編集アプリケーションでは、テキストの色の設定、フォントの変更、または現在のドキュメントの印刷を行うためのツールバー項目が提供される場合があります。

ツールバーには、次の3つの方法で項目を表示できます。

1. **アイコンとテキスト** 

     ![アイコンとテキストを含むツールバー](toolbar-images/info02.png "アイコンとテキストを含むツールバー")

2. **アイコンのみ** 

     ![アイコンのみのツールバー](toolbar-images/info03.png "アイコンのみのツールバー")

3. **テキストのみ** 

     ![テキストのみのツールバー](toolbar-images/info04.png "テキストのみのツールバー")

これらのモードを切り替えるには、ツールバーを右クリックし、コンテキストメニューから表示モードを選択します。

![ツールバーのコンテキストメニュー](toolbar-images/info05.png "ツールバーのコンテキストメニュー")

同じメニューを使用して、小さいサイズでツールバーを表示します。

![小さいアイコンを含むツールバー](toolbar-images/info06.png "小さいアイコンを含むツールバー")

メニューでは、ツールバーをカスタマイズすることもできます。

![ツールバーのカスタマイズに使用するダイアログ](toolbar-images/info07.png "ツールバーのカスタマイズに使用するダイアログ")

Xcode の Interface Builder でツールバーを設定すると、開発者は既定の構成の一部ではないツールバー項目を追加できます。 アプリケーションのユーザーは、ツールバーをカスタマイズし、必要に応じて事前定義された項目を追加および削除できます。 もちろん、ツールバーは既定の構成にリセットできます。

ツールバーは自動的に **[表示]** メニューに接続し、ユーザーはこのメニューを非表示にしたり、表示したり、カスタマイズしたりできます。

![[表示] メニューのツールバーに関連する項目](toolbar-images/info08.png "[表示] メニューのツールバーに関連する項目")

詳細については、[組み込みのメニュー機能](~/mac/user-interface/menu.md)に関するドキュメントを参照してください。

また、ツールバーが Interface Builder で適切に構成されている場合、アプリケーションの複数の起動にわたって、ツールバーのカスタマイズが自動的に保持されます。

このガイドの次のセクションでは、Xcode の Interface Builder を使用してツールバーを作成および管理する方法と、それらをコードで操作する方法について説明します。

## <a name="setting-a-custom-main-window-controller"></a>カスタムメインウィンドウコントローラーの設定

UI 要素をアウトレットとC#アクションを使用してコードに公開するには、Xamarin. Mac アプリでカスタムウィンドウコントローラーを使用する必要があります。

1. Xcode の Interface Builder でアプリのストーリーボードを開きます。
2. デザイン画面でウィンドウコントローラーを選択します。
3. **Id インスペクター**に切り替え、**クラス名**として「windowcontroller」と入力します。 

    [![ウィンドウコントローラーのカスタムクラス名の設定](toolbar-images/windowcontroller01.png "ウィンドウコントローラーのカスタムクラス名の設定")](toolbar-images/windowcontroller01-large.png#lightbox) 

4. 変更を保存し、Visual Studio for Mac に戻って同期します。
5. Visual Studio for Mac の**Solution Pad**に、 **WindowController.cs**ファイルがプロジェクトに追加されます。 

    ![Solution Pad での WindowController.cs の選択](toolbar-images/windowcontroller02.png "Solution Pad での WindowController.cs の選択")

6. Xcode の Interface Builder でストーリーボードを再度開きます。
7. **Windowcontroller .h**ファイルは、次のように使用できます。 

    [![WindowController .h ファイル](toolbar-images/windowcontroller03.png "WindowController .h ファイル")](toolbar-images/windowcontroller03-large.png#lightbox)

## <a name="creating-and-maintaining-toolbars-in-xcode"></a>Xcode でのツールバーの作成と保守

ツールバーは、Xcode の Interface Builder を使用して作成および管理されます。 アプリケーションにツールバーを追加するには、 **Solution Pad**でダブルクリックして、アプリのプライマリストーリーボード (この場合は**メインストーリーボード**) を編集します。

![Solution Pad でメインストーリーボードを開いています](toolbar-images/edit01.png "Solution Pad でメインストーリーボードを開いています")

**ライブラリインスペクター**で、**検索ボックス**に「tool」と入力して、使用可能なすべてのツールバー項目を表示しやすくします。

![ツールバー項目を表示するためにフィルター処理されたライブラリインスペクター](toolbar-images/edit02.png "ツールバー項目を表示するためにフィルター処理されたライブラリインスペクター")

**インターフェイスエディター**で、ツールバーをウィンドウにドラッグします。 ツールバーを選択した状態で、**属性インスペクター**でプロパティを設定して動作を構成します。

![ツールバーの属性インスペクター](toolbar-images/edit04.png "ツールバーの属性インスペクター")

使用できるプロパティは次のとおりです。

1. **表示**-ツールバーにアイコン、テキスト、またはその両方を表示するかどうかを制御します
2. [**起動時に表示**する]-オンにすると、ツールバーが既定で表示されます。
3. **カスタマイズ可能**-選択されている場合、ユーザーはツールバーを編集およびカスタマイズできます。
4. **Separator** -選択されている場合、細い水平線はウィンドウのコンテンツからツールバーを分離します。
5. **[サイズ]** -ツールバーのサイズを設定します。
6. **[自動保存]** : オンにすると、アプリケーションの起動時にユーザーのツールバーの構成の変更が保持されます。

**[自動保存]** オプションを選択し、その他のプロパティはすべて既定の設定のままにします。 

**インターフェイス階層**でツールバーを開いた後、ツールバー項目を選択してカスタマイズダイアログを表示します。

![ツールバーのカスタマイズ](toolbar-images/edit05.png "ツールバーのカスタマイズ")

このダイアログボックスを使用すると、既にツールバーの一部である項目のプロパティを設定したり、アプリケーションの既定のツールバーをデザインしたり、ツールバーをカスタマイズするときにユーザーが選択する追加のツールバー項目を指定したりできます。 ツールバーに項目を追加するには、**ライブラリインスペクター**から項目をドラッグします。

![ライブラリインスペクター](toolbar-images/edit06.png "ライブラリインスペクター")

次のツールバー項目を追加できます。

- **イメージツールバー項目**-カスタムイメージをアイコンとして持つツールバー項目。
- **柔軟な領域ツールバー項目**-後続のツールバー項目を揃えるために使用される柔軟な領域。 たとえば、1つまたは複数のツールバー項目の後に柔軟な領域のツールバー項目があり、もう1つのツールバー項目によって、最後の項目がツールバーの右側にピン留めされます。
- **スペースツールバー項目**-ツールバーの項目間の固定スペース
- **Separator ツールバー項目**-複数のツールバー項目の間に表示される区切り記号。グループ化する場合
- **ツールバー項目のカスタマイズ**-ユーザーがツールバーをカスタマイズできるようにします。
- [**ツールバー項目の印刷]** -ユーザーが開いているドキュメントを印刷できるようにします。
- [**色の表示] ツールバー項目**-標準のシステムカラーピッカーが表示されます。 

     ![システムカラーピッカー](toolbar-images/edit07.png "システムカラーピッカー")

- **[フォントツールバー項目の表示]** : 標準のシステムフォントダイアログが表示されます。 

     ![フォントセレクター](toolbar-images/edit08.png "フォントセレクター")

> [!IMPORTANT]
> 後で説明するように、ツールバーには、検索フィールド、セグメント化されたコントロール、水平方向のスライダーなど、多くの標準的な Cocoa UI コントロールを追加することもできます。

### <a name="adding-an-item-to-a-toolbar"></a>ツールバーへの項目の追加

ツールバーに項目を追加するには、**インターフェイス階層**でツールバーを選択し、項目のいずれかをクリックすると、カスタマイズダイアログが表示されます。 次に、**ライブラリインスペクター**から、[許可されている**ツールバーアイテム]** 領域に新しいアイテムをドラッグします。

![ツールバーのカスタマイズダイアログで使用できるツールバー項目](toolbar-images/add01.png "ツールバーのカスタマイズダイアログで使用できるツールバー項目")

新しい項目が既定のツールバーの一部であることを確認するには、 **[既定のツールバー項目]** 領域にドラッグします。 

![ドラッグしてツールバー項目を並べ替える](toolbar-images/add02.png "ドラッグしてツールバー項目を並べ替える")

既定のツールバー項目の順序を変更するには、それらを左または右にドラッグします。

次に、**属性インスペクター**を使用して、項目の既定のプロパティを設定します。

![属性インスペクターを使用したツールバーアイテムのカスタマイズ](toolbar-images/add03.png "属性インスペクターを使用したツールバーアイテムのカスタマイズ")

使用できるプロパティは次のとおりです。

- **イメージ名**-項目のアイコンとして使用するイメージ
- **ラベル**-ツールバーの項目に表示するテキスト
- **パレットラベル**-[**許可されたツールバーアイテム]** 領域のアイテムに表示するテキスト
- **Tag** -コード内の項目を識別するために使用できる、省略可能な一意の識別子。
- **識別子**-ツールバー項目の種類を定義します。 カスタム値を使用して、コード内のツールバー項目を選択できます。
- **選択可能**-オンの場合、項目はオン/オフボタンのように動作します。

> [!IMPORTANT]
> [許可されている**ツールバー項目**] 領域に項目を追加します。ただし、既定のツールバーではなく、ユーザーのカスタマイズオプションを提供します。 

### <a name="adding-other-ui-controls-to-a-toolbar"></a>ツールバーへの他の UI コントロールの追加

検索フィールドやセグメント化されたコントロールなど、いくつかの Cocoa UI 要素をツールバーに追加することもできます。

これを試すには、**インターフェイス階層**のツールバーを開き、ツールバー項目を選択して、カスタマイズダイアログを開きます。 **[ライブラリインスペクター]** から [許可されている**ツールバーアイテム]** 領域に**検索フィールド**をドラッグします。

![ツールバーのカスタマイズダイアログの使用](toolbar-images/add05.png "ツールバーのカスタマイズダイアログの使用")

ここでは、Interface Builder を使用して検索フィールドを構成し、アクションまたはアウトレットを通じてコードに公開します。

## <a name="built-in-toolbar-item-support"></a>組み込みのツールバー項目のサポート

いくつかの Cocoa UI 要素は、既定で標準のツールバー項目と対話します。 たとえば、**テキストビュー**をアプリケーションのウィンドウにドラッグして、コンテンツ領域に収まるように配置します。

[![アプリケーションへのテキストビューの追加](toolbar-images/edit09.png "アプリケーションへのテキストビューの追加")](toolbar-images/edit09-large.png#lightbox)

ドキュメントを保存し、Visual Studio for Mac に戻って Xcode と同期し、アプリケーションを実行して、テキストを入力して選択し、 **[色]** ツールバー項目をクリックします。 [テキスト] ビューが自動的にカラーピッカーで動作することに注意してください。

![テキストビューとカラーピッカーを使用した組み込みのツールバー機能](toolbar-images/edit10.png "テキストビューとカラーピッカーを使用した組み込みのツールバー機能")

## <a name="using-images-with-toolbar-items"></a>ツールバー項目を含むイメージの使用

**イメージツールバー項目**を使用すると、 **Resources**フォルダーに追加されたビットマップイメージ (および**バンドルリソース**のビルドアクション) を、ツールバーにアイコンとして表示できます。

1. Visual Studio for Mac の**Solution Pad**で、 **[リソース]** フォルダーを右クリックし、[**追加** > **ファイル**の追加] を選択します。
2. **[ファイルの追加]** ダイアログボックスで目的のイメージに移動し、選択して **[開く]** ボタンをクリックします。 

    [![追加するイメージの選択](toolbar-images/edit11.png "追加するイメージの選択")](toolbar-images/edit11-large.png#lightbox)

3. **[コピー]** を選択し、 **[選択したすべてのファイルに同じアクションを使用する]** をオンにして、[ **OK]** をクリックします。

    ![追加したイメージのコピー操作を選択する](toolbar-images/edit12.png "追加したイメージのコピー操作を選択する")

4. **Solution Pad**で、mainwindow.xaml をダブルクリックして Xcode で開きます **。**

5. **インターフェイス階層**のツールバーを選択し、そのいずれかの項目をクリックして、カスタマイズダイアログを開きます。

6. **ライブラリインスペクター**から、ツールバーの **[使用できるツールバー項目]** 領域に、**イメージツールバー項目**をドラッグします。 

    ![許可されているツールバー項目領域に追加されたイメージツールバー項目](toolbar-images/edit14.png "許可されているツールバー項目領域に追加されたイメージツールバー項目")

7. [**属性] インスペクター**で、Visual Studio for Mac に追加したイメージを選択します。 

    ![ツールバーアイテムのカスタムイメージの設定](toolbar-images/edit15.png "ツールバーアイテムのカスタムイメージの設定")

8. **ラベル**を "ごみ箱" に設定し、[**パレット] ラベル**を "Erase Document" に設定します。 

    ![ツールバー項目のラベルとパレットラベルの設定](toolbar-images/edit16.png "ツールバー項目のラベルとパレットラベルの設定")

9. [**ライブラリ] インスペクター**から、ツールバーの **[許可されたツールバーアイテム]** 領域に、**区切りツールバーアイテム**をドラッグします。 

    [![[許可されたツールバーアイテム] 領域に追加された区切りツールバーアイテム](toolbar-images/edit17.png "[許可されたツールバーアイテム] 領域に追加された区切りツールバーアイテム")](toolbar-images/edit17-large.png#lightbox)

10. 区分項目と "ごみ箱" 項目を**既定のツールバー項目**領域にドラッグし、次のようにツールバー項目の順序を左から右に設定します (色、フォント、区切り文字、ごみ箱、フレキシブルスペース、印刷)。 

    ![既定のツールバー項目](toolbar-images/edit18.png "既定のツールバー項目")

11. Xcode と同期するには、変更を保存して Visual Studio for Mac に戻ります。

アプリケーションを実行して、新しいツールバーが既定で表示されていることを確認します。

![カスタマイズされた既定の項目を含むツールバー](toolbar-images/edit19.png "カスタマイズされた既定の項目を含むツールバー")

## <a name="exposing-toolbar-items-with-outlets-and-actions"></a>アウトレットとアクションを使用したツールバーアイテムの公開

コード内のツールバーまたはツールバーアイテムにアクセスするには、アウトレットまたはアクションにアタッチする必要があります。

1. **Solution Pad**で **、[Xcode** ] をダブルクリックして開きます。
2. カスタムクラス "WindowController" が**Id インスペクター**のメインウィンドウコントローラーに割り当てられていることを確認します。

    [![Id インスペクターを使用してウィンドウコントローラーのカスタムクラスを設定する](toolbar-images/edit20a.png "Id インスペクターを使用してウィンドウコントローラーのカスタムクラスを設定する")](toolbar-images/edit20a-large.png#lightbox)

3. 次に、**インターフェイス階層**内のツールバー項目を選択します。 

    ![インターフェイス階層内のツールバー項目の選択](toolbar-images/edit20.png "インターフェイス階層内のツールバー項目の選択")  

4. **アシスタントビュー**を開き、 **windowcontroller .h**ファイルを選択して、ツールバー項目から**windowcontroller .h**ファイルに制御をドラッグします。
5. **[接続]** の種類 を **[アクション]** に設定し、**名前**として「trashdocument」と入力して、 **[接続]** ボタンをクリックします。 

    [![ツールバーアイテムのアクションを構成する](toolbar-images/edit23.png "ツールバーアイテムのアクションを構成する")](toolbar-images/edit23-large.png#lightbox)

6. **テキストビュー**を**viewcontroller .h**ファイルの "documenteditor" というアウトレットとして公開します。 

    [![テキストビューのアウトレットの構成](toolbar-images/edit24.png "テキストビューのアウトレットの構成")](toolbar-images/edit24-large.png#lightbox)

7. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

Visual Studio for Mac で、 **ViewController.cs**ファイルを編集し、次のコードを追加します。

```csharp
public void EraseDocument() {
    documentEditor.Value = "";
}
```

次に、 **WindowController.cs**ファイルを編集し、`WindowController` クラスの下部に次のコードを追加します。

```csharp
[Export ("trashDocument:")]
void TrashDocument (NSObject sender) {

    var controller = ContentViewController as ViewController;
    controller.EraseDocument ();
}
```

アプリケーションを実行すると、**ごみ箱**のツールバー項目がアクティブになります。

![アクティブなごみ箱項目があるツールバー](toolbar-images/edit25.png "アクティブなごみ箱項目があるツールバー")

**ごみ箱**のツールバー項目を使用して、テキストを削除できることに注意してください。

## <a name="disabling-toolbar-items"></a>ツールバー項目の無効化

ツールバーの項目を無効にするには、カスタム `NSToolbarItem` クラスを作成し、`Validate` メソッドをオーバーライドします。 次に、Interface Builder で、有効または無効にする項目にカスタム型を割り当てます。

カスタム `NSToolbarItem` クラスを作成するには、プロジェクトを右クリックし、 > **新しいファイル**の**追加** を選択します。**全般** >  **空のクラス** を選択し、**名前**として「ActivatableItem」と入力して、**新規** ボタンをクリックします。 

![Visual Studio for Mac に空のクラスを追加する](toolbar-images/custom01.png "Visual Studio for Mac に空のクラスを追加する")

次に、 **ActivatableItem.cs**ファイルを編集して次のように読み取ります。

```csharp
using System;

using Foundation;
using AppKit;

namespace MacToolbar
{
    [Register("ActivatableItem")]
    public class ActivatableItem : NSToolbarItem
    {
        public bool Active { get; set;} = true;

        public ActivatableItem ()
        {
        }

        public ActivatableItem (IntPtr handle) : base (handle)
        {
        }

        public ActivatableItem (NSObjectFlag  t) : base (t)
        {
        }

        public ActivatableItem (string title) : base (title)
        {
        }

        public override void Validate ()
        {
            base.Validate ();
            Enabled = Active;
        }
    }
}
```

Xcode をダブル**クリックし**て開きます。 上で作成した**ごみ箱**ツールバー項目を選択し、 **id インスペクター**でクラスを "ActivatableItem" に変更します。

![ツールバー項目のカスタムクラスの設定](toolbar-images/custom02.png "ツールバー項目のカスタムクラスの設定")

**ごみ箱**のツールバー項目に `trashItem` というアウトレットを作成します。 Xcode と同期するには、変更を保存して Visual Studio for Mac に戻ります。 最後に、 **MainWindow.cs**を開き、`AwakeFromNib` メソッドを更新して、次のように読み取ります。

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Disable trash
    trashItem.Active = false;
}
```

アプリケーションを実行し、**ごみ箱**の項目がツールバーで無効になっていることを確認します。

![非アクティブなごみ箱項目を含むツールバー](toolbar-images/custom03.png "非アクティブなごみ箱項目を含むツールバー")

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリケーションでのツールバーとツールバー項目の使用方法について詳しく説明しました。 ここでは、Xcode の Interface Builder でツールバーを作成して管理する方法、いくつかの UI コントロールがツールバーの項目をC#自動的に操作する方法、ツールバーをコードで操作する方法、およびツールバーの項目を有効または無効にする方法について説明しています。

## <a name="related-links"></a>関連リンク

- [MacToolbar (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/mactoolbar)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [ツールバーのヒューマンインターフェイスガイドライン](https://developer.apple.com/macos/human-interface-guidelines/windows-and-views/toolbars/)
- [ツールバーの概要](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Toolbars/Toolbars.html)
