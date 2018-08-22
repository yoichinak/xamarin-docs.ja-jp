---
title: Xamarin.Mac でツールバー
description: この記事では、Xamarin.Mac アプリケーションでツールバーの使用について説明します。 Xcode と Interface Builder をコードに公開することと、それらをプログラムで操作の作成と保守のツールバーが含まれます。
ms.prod: xamarin
ms.assetid: C8D228CE-C860-47E1-85FD-69864BF91F20
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 06faaf16ffd0adc64063bfa5a264c1895b9ca9cb
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2018
ms.locfileid: "40250957"
---
# <a name="toolbars-in-xamarinmac"></a>Xamarin.Mac でツールバー

_この記事では、Xamarin.Mac アプリケーションでツールバーの使用について説明します。Xcode と Interface Builder をコードに公開することと、それらをプログラムで操作の作成と保守のツールバーが含まれます。_

Xamarin.Mac 開発者が Visual Studio for Mac の操作では、ツール バー コントロールを含む、Xcode を使用する macOS 開発者が利用できるのと同じ UI コントロールにアクセスします。 Xamarin.Mac は直接 Xcode と統合、ために、Xcode の Interface Builder を使用して作成し、ツールバーの項目を維持することができます。 これらのツールバー項目は、c# でも作成できます。

MacOS でのツールバーでは、ウィンドウの最上部セクションに追加され、その機能に関連するコマンドに簡単にアクセスを提供します。 ツールバーの非表示、表示、またはアプリケーションのユーザーによってカスタマイズできますが、さまざまな方法でツールバー項目を提示することができます。

この記事では、ツールバーとツールバー項目、Xamarin.Mac アプリケーションで操作の基本について説明します。 

、続行する前に目を通してください、[こんにちは, Mac](~/mac/get-started/hello-mac.md)記事-具体的には、 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)と[Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions)セクション — ほど主要な概念と、このガイド全体で使用される手法について説明します。

見ても、 [c# を公開するクラス/メソッドを OBJECTIVE-C](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)ドキュメント。 説明しています、`Register`と`Export`OBJECTIVE-C クラスを c# クラスを接続するために使用する属性。

## <a name="introduction-to-toolbars"></a>ツールバーの概要

MacOS アプリケーションのいずれかのウィンドウには、ツールバーを含めることができます。

![ツールバーで、ウィンドウの例](toolbar-images/info01.png "をツールバー、ウィンドウの例")

ツールバーにすばやくアクセスできる重要なアプリケーションのユーザーまたは一般的に使用される機能の簡単な方法を提供します。 たとえば、ドキュメントの編集アプリケーションはツールバー項目のテキストの色を設定、フォントを変更する、または現在のドキュメントを印刷を提供する可能性があります。

ツールバーは、3 つの方法で項目を表示できます。

1. **アイコンとテキスト** 

     ![アイコンとテキストを含むツールバー](toolbar-images/info02.png "アイコンとテキストを含むツールバー")

2. **アイコンのみ** 

     ![アイコン専用のツールバー](toolbar-images/info03.png "アイコン専用のツールバー")

3. **テキストのみ** 

     ![テキストのみツールバー](toolbar-images/info04.png "テキストのみのツールバー")

ツールバーを右クリックし、コンテキスト メニューから表示モードを選択して、これらのモードを切り替えます。

![コンテキスト メニュー、ツールバーの](toolbar-images/info05.png "ツールバーのコンテキスト メニュー")

小さなサイズでツールバーを表示するのにには、同じメニューを使用します。

![小さいアイコンを含むツールバー](toolbar-images/info06.png "小さいアイコンを含むツールバー")

ツールバーをカスタマイズするため、メニューもできます。

![ツールバーをカスタマイズするために使用するダイアログ](toolbar-images/info07.png "ツールバーをカスタマイズするために使用する ダイアログ")

開発者は Xcode の Interface Builder でのツールバーを設定する場合、既定の構成の一部ではない余分なツールバー項目を提供することができます。 アプリケーションのユーザーを追加して、必要に応じて、これらの事前定義された項目を削除するツールバーをカスタマイズできます。 もちろん、ツールバーは、既定の構成にリセットできます。

ツールバーが自動的に接続する、**ビュー**メニューには、非表示に、表示、およびカスタマイズすることができます。

![[表示] メニュー内のアイテムのツールバーに関連する](toolbar-images/info08.png "表示 メニューで、ツールバーに関連する項目")

参照してください、[組み込みメニュー機能](~/mac/user-interface/menu.md)詳細についてはドキュメントです。

さらに、ツールバーが Interface Builder で正しく構成されている場合は、アプリケーションにアプリケーションの複数の起動の間でのツールバーのカスタマイズは保持自動的にします。

このガイドの次のセクションでは、作成し、Xcode の Interface Builder を使用してツールバーを維持する方法とコードで操作する方法について説明します。

## <a name="setting-a-custom-main-window-controller"></a>カスタムのメイン ウィンドウのコント ローラーを設定

Outlet とアクション、Xamarin.Mac アプリを c# コードに UI 要素を公開するには、カスタム ウィンドウ コント ローラーを使用する必要があります。

1. Xcode の Interface Builder では、アプリのストーリー ボードを開きます。
2. デザイン画面で、ウィンドウのコント ローラーを選択します。
3. 切り替えて、 **Identity Inspector**として"WindowController"を入力し、**クラス名**: 

    [![ウィンドウ コント ローラーのカスタム クラス名を設定する](toolbar-images/windowcontroller01.png "ウィンドウ コント ローラーのカスタム クラス名を設定します。")](toolbar-images/windowcontroller01-large.png#lightbox) 

4. 変更を保存し、Visual Studio for Mac を同期に戻ります。
5. A **WindowController.cs**ファイルは、プロジェクトに追加する、 **Solution Pad** Visual Studio for Mac で。 

    ![Solution Pad で WindowController.cs を選択する](toolbar-images/windowcontroller02.png "Solution Pad で WindowController.cs を選択します。")

6. Xcode の Interface Builder のストーリー ボードを再度開きます。
7. **WindowController.h**ファイルが使用できるようにします。 

    [![WindowController.h ファイル](toolbar-images/windowcontroller03.png "WindowController.h ファイル")](toolbar-images/windowcontroller03-large.png#lightbox)

## <a name="creating-and-maintaining-toolbars-in-xcode"></a>作成して、Xcode でツールバーを維持

ツールバーが作成され、Xcode の Interface Builder で維持されます。 アプリケーションにツールバーを追加するアプリの主なストーリー ボードを編集します (ここで**Main.storyboard**) でダブルクリックして、 **Solution Pad**:

![Solution Pad で Main.storyboard を開く](toolbar-images/edit01.png "Solution Pad で Main.storyboard を開く")

**ライブラリ インスペクター**、「ツール」を入力、**検索ボックス**を見やすくすべての利用可能なツールバー項目。

![ツールバー項目を表示するフィルター処理、ライブラリ インスペクター](toolbar-images/edit02.png "ツールバー項目を表示するフィルター処理のライブラリ インスペクター")

ウィンドウ上にツールバーをドラッグして、**インターフェイス エディター**します。 選択されているツールバーでプロパティを設定してその動作を構成、 **Attributes Inspector**:

![属性インスペクターのツールバーに](toolbar-images/edit04.png "属性インスペクターのツールバー")

使用できるプロパティは次のとおりです。

1. **表示**のツールバーには、アイコン、テキスト、またはその両方が表示されるかどうかを制御します。
2. **起動時に表示される**-ツールバーが既定で表示される選択した場合。
3. **カスタマイズ可能な**-選択した場合、ユーザーの編集し、ツールバーをカスタマイズします。
4. **区切り記号**-選択した場合、細い線が水平がウィンドウの内容から、ツールバーを分離します。
5. **サイズ**のツールバーのサイズを設定
6. **自動保存**-アプリケーションがユーザーのツールバーの構成の変更を永続化アプリケーションの起動の間で選択されている場合。

選択、 **Autosave**オプションを選択し、その他のすべてのプロパティの既定の設定のままにします。 

ツールバーを開いた後、**インターフェイス階層**、ツールバー項目を選択して、カスタマイズ ダイアログを起動します。

![ツールバーのカスタマイズ](toolbar-images/edit05.png "ツールバーのカスタマイズ")

このダイアログ ボックスを使用して、デザイン、アプリケーションの既定のツールバーに、ツールバーの一部である項目のプロパティを設定して、ユーザーがツールバーをカスタマイズするときに選択するための余分なツールバー項目を指定します。 ツールバーにアイテムを追加するにドラッグしてから、**ライブラリ インスペクター**:

![ライブラリ インスペクター](toolbar-images/edit06.png "ライブラリ インスペクター")

次のツールバー項目を追加できます。

- **ツール バー アイテムを画像**-ツールバー項目アイコンとしてカスタム イメージを使用します。
- **柔軟なツールバー項目**の柔軟な容量が後続のツールバー項目を正当化するために使用します。 たとえば、柔軟な容量のツール バー アイテムの後に 1 つまたは複数のツールバー項目と別のツール バー アイテムは、ツールバーの右側にある最後のアイテムをピン留めします。
- **ツール バー アイテムのスペース**のツールバーの項目間にスペースを修正しました
- **ツールバー項目の区切り記号**-2 つ以上のツールバー項目をグループ化の間の表示されている区切り記号
- **ツールバー項目をカスタマイズ**-ツールバーをカスタマイズすることができます
- **ツール バー アイテムの印刷**-、開いているドキュメントを印刷することができます
- **ツール バー アイテムの色を表示する**-標準のシステム カラー ピッカーが表示されます。 

     ![システム カラー ピッカー](toolbar-images/edit07.png "システム カラー ピッカー")

- **ツールバー項目のフォントを表示する**-標準のシステムの [フォント] ダイアログ ボックスが表示されます。 

     ![フォント セレクター](toolbar-images/edit08.png "フォント セレクター")

> [!IMPORTANT]
> 後で示すようは、検索フィールドやセグメント化されたコントロールは、水平スライダーなどの多くの標準 Cocoa UI コントロールは、ツールバーにも追加できます。

### <a name="adding-an-item-to-a-toolbar"></a>ツールバーに、項目の追加

ツールバーにアイテムを追加するでツールバーを選択する、**インターフェイス階層**カスタマイズ ダイアログを表示するを原因と、そのアイテムのいずれかをクリックします。 次からの新しい項目をドラッグ、**ライブラリ インスペクター**を**ツールバー項目の許可**領域。

![ツールバーの [カスタマイズ] ダイアログ ボックスの許可されているツールバー項目](toolbar-images/add01.png "ツールバーの [カスタマイズ] ダイアログ ボックスの許可されているツールバー項目")

新しい項目が既定のツールバーの一部であることを確認するには、ドラッグして、**既定ツールバー項目**領域。 

![ツールバー項目をドラッグして並べ替え](toolbar-images/add02.png "をドラッグしてツール バー アイテムの並べ替え")

既定のツールバー項目を並べ替えるには、ドラッグ左側または右側します。

次に、使用、 **Attributes Inspector**項目の既定のプロパティを設定します。

![属性インスペクターを使用してツールバー項目をカスタマイズする](toolbar-images/add03.png "属性インスペクターを使用してツールバー項目をカスタマイズします。")

使用できるプロパティは次のとおりです。

- **イメージ名**-項目のアイコンとして使用するイメージ
- **ラベル**のツールバーの項目に表示するテキスト
- **パレット ラベル**-内の項目に表示するテキスト、**ツールバー項目の許可**領域
- **タグ**-コード内の項目を識別するのに役立つ、オプションの一意の識別子。
- **識別子**-定義、ツールバーの項目の種類。 コードでツールバー項目を選択する、カスタム値を使用できます。
- **選択可能な**のオン/オフ ボタンのように、項目が機能する場合はオンになっています。

> [!IMPORTANT]
> 項目を追加、**ツールバー項目の許可**領域がユーザーのカスタマイズ オプションを提供する既定のツールバーされません。 

### <a name="adding-other-ui-controls-to-a-toolbar"></a>その他の UI コントロール、ツールバーを追加します。

などのいくつかの Cocoa UI 要素がフィールドを検索し、セグメント付きコントロールは、ツールバーにも追加できます。

これには、開く、ツールバーで、**インターフェイス階層**カスタマイズ ダイアログを開くツールバー項目を選択します。 ドラッグ、**検索フィールド**から、**ライブラリ インスペクター**を**ツールバー項目の許可**領域。

![ツールバーのカスタマイズ ダイアログを使用して](toolbar-images/add05.png "ツールバーのカスタマイズ ダイアログを使用します。")

ここでは、Interface Builder を使用して、検索フィールドを構成し、アクションまたはコンセントを使用してコードに公開します。

## <a name="built-in-toolbar-item-support"></a>組み込みのツールバー項目のサポート

いくつかの Cocoa UI 要素は、既定では、標準ツールバーの項目と対話します。 たとえば、ドラッグ、**テキスト ビュー**をアプリケーションのウィンドウにコンテンツ領域に挿入する位置と。

[![アプリケーションへのテキスト ビューの追加](toolbar-images/edit09.png "アプリケーションへのテキスト ビューの追加")](toolbar-images/edit09-large.png#lightbox)

Visual Studio for Mac を Xcode と同期、アプリケーションの実行、テキストを入力、選択しをクリックして戻り、文書を保存、**色**ツールバー項目。 カラー ピッカーを使用して、テキスト ビューが自動的に動作することを確認します。

![テキスト ビューとカラー ピッカーを使用して組み込みツールバー機能](toolbar-images/edit10.png "テキスト ビューとカラー ピッカーを使用して組み込みツールバーの機能")

## <a name="using-images-with-toolbar-items"></a>ツールバー項目を含むイメージを使用します。

使用して、**イメージ ツール バー アイテム**、任意のビットマップ イメージに追加して、**リソース**フォルダー (のビルド アクションを指定し、**バンドル リソース**)、ツールバーのアイコンとして表示することができます。

1. Visual Studio for Mac での**Solution Pad**を右クリックし、**リソース**フォルダーと選択**追加** > **ファイルの追加**.
2. **ファイルの追加** ダイアログ ボックス、必要なイメージに移動し、選択して、クリックして、**オープン**ボタン。 

    [![追加するイメージの選択](toolbar-images/edit11.png "を追加するイメージの選択")](toolbar-images/edit11-large.png#lightbox)

3. 選択**コピー**、確認**同じアクションを使用して、選択したすべてのファイルの**、 をクリック**OK**:

    ![追加したイメージのコピー操作を選択](toolbar-images/edit12.png "追加イメージのコピー操作の選択")

4. **Solution Pad**、ダブルクリックして**MainWindow.xib** Xcode で開きます。

5. ツールバーの選択、**インターフェイス階層**カスタマイズ ダイアログを開くには、その項目のいずれかをクリックします。

6. ドラッグ、**イメージ ツール バー アイテム**から、**ライブラリ インスペクター**をツールバーの**ツールバー項目の許可**領域。 

    ![ツールバー項目の許可されている領域にイメージのツール バー アイテムが追加](toolbar-images/edit14.png "An イメージ ツール バー アイテムがツール バー アイテムの許可 領域に追加")

7. **Attributes Inspector**Mac に Visual Studio で追加されたイメージを選択します。 

    ![ツール バー アイテムのカスタム イメージを設定する](toolbar-images/edit15.png "ツール バー アイテムのカスタム イメージを設定します。")

8. 設定、**ラベル**「ごみ箱」に、**パレット ラベル**「ドキュメントの消去」。 

    ![ツールバーの設定項目のラベルとラベルのパレット](toolbar-images/edit16.png "ラベルとパレット ラベル アイテムのツールバーの設定")

9. ドラッグ、**ツールバー項目の区切り記号**から、**ライブラリ インスペクター**をツールバーの**ツールバー項目の許可**領域。 

    [![ツールバー項目の許可されている領域に区切り記号のツール バー アイテムが追加](toolbar-images/edit17.png "A 区切り記号のツール バー アイテムがツール バー アイテムの許可されている領域に追加")](toolbar-images/edit17-large.png#lightbox)

10. 区切り記号アイテムと「ごみ箱」のアイテムをドラッグして、**ツールバー項目の既定の**領域とセットから項目をツールバーの順序が左から右 (色、フォント、区切り記号、ごみ箱、柔軟な空き領域、印刷) を次のようにします。 

    ![既定のツールバー項目](toolbar-images/edit18.png "は既定のツールバー項目")

11. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

既定では、新しいツールバーが表示されることを確認するアプリケーションを実行します。

![ツールバーのカスタマイズした既定の項目を](toolbar-images/edit19.png "カスタマイズした既定の項目を含むツールバー")

## <a name="exposing-toolbar-items-with-outlets-and-actions"></a>Outlet と action とツールバー項目を公開します。

ツールバーまたはコードのツール バー アイテムにアクセスするには、アウトレットまたはアクションをアタッチする必要があります。

1. **Solution Pad**、ダブルクリックして**Main.storyboard** Xcode で開きます。
2. カスタム クラス"WindowController"は、メイン ウィンドウのコント ローラーに割り当てられていることを確認、 **Identity Inspector**:

    [![ウィンドウ コント ローラーのカスタム クラスを設定する、Identity Inspector を使用して](toolbar-images/edit20a.png "Identity Inspector を使用して、ウィンドウのコント ローラーのカスタム クラスを設定するには")](toolbar-images/edit20a-large.png#lightbox)

3. 次に、ツール バー アイテムを選択、**インターフェイス階層**: 

    ![インターフェイス階層内のツール バー アイテムの選択](toolbar-images/edit20.png "インターフェイス階層内のツール バー アイテムの選択")  

4. 開く、**アシスタント ビュー**を選択、 **WindowController.h**ファイル、およびコントロールをドラッグするツールバー項目から、 **WindowController.h**ファイル。
5. 設定、**接続**に入力**アクション**の"trashDocument"を入力、**名前**、 をクリックし、 **Connect**ボタン。 

    [![ツール バー アイテムのアクションを設定する](toolbar-images/edit23.png "ツール バー アイテムのアクションを設定します。")](toolbar-images/edit23-large.png#lightbox)

6. 公開、**テキスト ビュー** "documentEditor"と呼ばれるアウトレットとして、 **ViewController.h**ファイル。 

    [![テキスト ビューのアウトレットを構成する](toolbar-images/edit24.png "テキスト ビューのアウトレットを構成します。")](toolbar-images/edit24-large.png#lightbox)

7. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

Visual studio for Mac では、編集、 **ViewController.cs**ファイルを開き、次のコードを追加します。

```csharp
public void EraseDocument() {
    documentEditor.Value = "";
}
```

次に、編集、 **WindowController.cs**ファイルを開き、下部に次のコードを追加、`WindowController`クラス。

```csharp
[Export ("trashDocument:")]
void TrashDocument (NSObject sender) {

    var controller = ContentViewController as ViewController;
    controller.EraseDocument ();
}
```

アプリケーションの実行時に、**ごみ箱**ツール バー アイテムがアクティブになります。

![ごみ箱のアクティブな項目を含むツールバー](toolbar-images/edit25.png "ごみ箱のアクティブな項目を含むツールバー")

なお、**ごみ箱**ツールバー項目のテキストを削除に使用できます。

## <a name="disabling-toolbar-items"></a>ツールバー項目を無効にします。

ツールバーにある項目を無効にするには、カスタムを作成`NSToolbarItem`クラスし、オーバーライド、`Validate`メソッド。 次に、インターフェイス ビルダーでは、有効または無効にしたい項目にカスタムの型を割り当てます。

カスタムを作成する`NSToolbarItem`クラス、プロジェクトを右クリックし、選択**追加** > **新しいファイル.**.選択**全般** > **空のクラス**の"ActivatableItem"を入力、**名前**、 をクリックし、**新規**ボタン。 

![Visual studio for Mac の空のクラスを追加する](toolbar-images/custom01.png "Visual studio for Mac の空のクラスを追加します。")

次に、編集、 **ActivatableItem.cs**ファイルを次のように読み取ります。

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

ダブルクリック**Main.storyboard** Xcode で開きます。 選択、**ごみ箱**ツールバー項目を選択し、上記で作成したで"ActivatableItem"をそのクラスに変更、 **Identity Inspector**:

![ツール バー アイテムのカスタム クラスを設定](toolbar-images/custom02.png "ツール バー アイテムのカスタム クラスの設定")

呼ばれるアウトレットを作成する`trashItem`の**ごみ箱**ツールバー項目。 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。 最後に、開く**MainWindow.cs**し、更新、`AwakeFromNib`メソッドを次のように。

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Disable trash
    trashItem.Active = false;
}
```

アプリケーションを実行して、**ごみ箱**項目は、ツールバーで無効になっているようになりました。

![ごみ箱の非アクティブな項目を含むツールバー](toolbar-images/custom03.png "ごみ箱の非アクティブな項目を含むツールバー")

## <a name="summary"></a>まとめ

この記事では、ツールバーとツールバー項目、Xamarin.Mac アプリケーションでの使用方法について詳しく説明をしました。 作成し、Xcode の Interface Builder でツールバーを維持する方法、どの一部の UI コントロールが自動的にツールバー項目を含む動作、c# のコードでツールバーを操作する方法および有効にするツールバー項目を無効にする方法を説明します。


## <a name="related-links"></a>関連リンク

- [MacToolbar (サンプル)](https://developer.xamarin.com/samples/mac/MacToolbar/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [ツールバーのヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/windows-and-views/toolbars/)
- [ツールバーの概要](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Toolbars/Toolbars.html)
