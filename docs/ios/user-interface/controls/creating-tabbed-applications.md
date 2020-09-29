---
title: Xamarin. iOS のタブバーとタブバーコントローラー
description: このドキュメントでは、iOS のタブバーコントローラーと、それらを Xamarin で使用する方法について説明します。 ここでは、UITabBarController を設定する方法、イメージを操作する方法、バッジ値を設定する方法、イベントを処理する方法などについて説明します。
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 9bd048239c404c0eb3309fdc74b26bcb94db4740
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91434123"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>Xamarin. iOS のタブバーとタブバーコントローラー

タブ付きアプリケーションは、特定の順序で複数の画面にアクセスできるユーザーインターフェイスをサポートするために、iOS で使用されます。 クラスを使用すると、アプリケーションには、 `UITabBarController` このようなマルチスクリーンのシナリオのサポートを簡単に含めることができます。 `UITabBarController` 複数画面の管理を行い、アプリケーション開発者が各画面の詳細に集中できるようにします。

通常、タブ付きアプリケーションは、 `UITabBarController` メインウィンドウのを使用してビルドされ `RootViewController` ます。 ただし、追加のコードを記述することで、タブ付きアプリケーションを他の初期画面に連続して使用することもできます。たとえば、アプリケーションで最初にログイン画面が表示され、次にタブ付きインターフェイスが表示されます。

このページでは、両方のシナリオについて説明します。タブがアプリケーションビュー階層のルートにあり、非シナリオでも同様 `RootViewController` です。

## <a name="introducing-uitabbarcontroller"></a>UITabBarController の概要

は、 `UITabBarController` 次のようにタブ付きアプリケーション開発をサポートしています。

- 複数のコントローラーを追加できるようにします。
- `UITabBar`ユーザーがコントローラーとそのビューを切り替えることができるように、クラスを使用してタブ付きのユーザーインターフェイスを提供します。

コントローラーは、その `UITabBarController` プロパティ (配列) を介してに追加され `ViewControllers` `UIViewController` ます。 自体は、 `UITabBarController` 適切なコントローラーの読み込みと、選択したタブに基づくビューの表示を処理します。

タブは、 `UITabBarItem` インスタンスに含まれているクラスのインスタンスです `UITabBar` 。 各 `UITabBar` インスタンスには、 `TabBarItem` 各タブのコントローラーのプロパティを使用してアクセスできます。

の使用方法を理解するために `UITabBarController` 、1つのを使用する単純なアプリケーションを構築する方法について説明します。

## <a name="tabbed-application-walkthrough"></a>タブ付きアプリケーションのチュートリアル

このチュートリアルでは、次のアプリケーションを作成します。

[![タブ付きアプリのサンプル](creating-tabbed-applications-images/00-app.png)](creating-tabbed-applications-images/00-app.png#lightbox)

Visual Studio for Mac で使用できるタブ付きアプリケーションテンプレートは既に存在しますが、この例では、これらの手順は空のプロジェクトから機能し、アプリケーションの構築方法をより深く理解します。

### <a name="creating-the-application"></a>アプリケーションの作成

まず、新しいアプリケーションを作成します。

Visual Studio for Mac で [ **ファイル > 新規 > ソリューション** ] メニュー項目を選択し、[ **iOS > アプリ] > 空のプロジェクト** テンプレートを選択し `TabbedApplication` ます。次に示すように、プロジェクトの名前を指定します。

[![空のプロジェクトテンプレートを選択します](creating-tabbed-applications-images/newsolution1.png)](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![プロジェクトに TabbedApplication という名前を指定する](creating-tabbed-applications-images/newsolution2.png)](creating-tabbed-applications-images/newsolution2.png#lightbox)

### <a name="adding-the-uitabbarcontroller"></a>UITabBarController の追加

次に、[ **ファイル > 新しいファイル** ] を選択し、 **[全般: 空のクラス** ] テンプレートを選択して、空のクラスを追加します。 次のようにファイルに名前を `TabController` 付けます。

[![TabController クラスを追加する](creating-tabbed-applications-images/02-newclass.png)](creating-tabbed-applications-images/02-newclass.png#lightbox)

クラスには `TabController` 、の配列を管理するの実装が含まれ `UITabBarController` `UIViewControllers` ます。 ユーザーがタブを選択すると、によって `UITabBarController` 適切なビューコントローラーのビューが表示されます。

を実装するには、 `UITabBarController` 次の手順を実行する必要があります。

1. の基本クラス  `TabController` をに設定  `UITabBarController` します。
1. `UIViewController`に追加するインスタンスを作成 `TabController` します。
1. インスタンスをの  `UIViewController` プロパティに割り当てられた配列に追加  `ViewControllers`  `TabController` します。

次のコードをクラスに追加して、 `TabController` これらの手順を実行します。

```csharp
using System;
using UIKit;

namespace TabbedApplication {
    public class TabController : UITabBarController {

        UIViewController tab1, tab2, tab3;

        public TabController ()
        {
            tab1 = new UIViewController();
            tab1.Title = "Green";
            tab1.View.BackgroundColor = UIColor.Green;

            tab2 = new UIViewController();
            tab2.Title = "Orange";
            tab2.View.BackgroundColor = UIColor.Orange;

            tab3 = new UIViewController();
            tab3.Title = "Red";
            tab3.View.BackgroundColor = UIColor.Red;

            var tabs = new UIViewController[] {
                tab1, tab2, tab3
            };

            ViewControllers = tabs;
        }
    }
}
```

各インスタンスについて、のプロパティが設定されていることに注意 `UIViewController` `Title` して `UIViewController` ください。 コントローラーがに追加されると、は `UITabBarController` `UITabBarController` 各コントローラーのを読み取り、次のように `Title` 関連付けられたタブのラベルに表示します。

[![サンプルアプリの実行](creating-tabbed-applications-images/00-app.png)](creating-tabbed-applications-images/00-app.png#lightbox)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>TabController を RootViewController として設定する

コントローラーがタブに配置される順序は、配列に追加される順序に対応し `ViewControllers` ます。

を最初の `UITabController` 画面として読み込むには、 `RootViewController` 次のコードに示すように、をウィンドウにする必要があり `AppDelegate` ます。

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    UIWindow window;
    TabController tabController;

    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        window = new UIWindow (UIScreen.MainScreen.Bounds);

        tabController = new TabController ();
        window.RootViewController = tabController;

        window.MakeKeyAndVisible ();

        return true;
    }
}
```

ここでアプリケーションを実行すると、 `UITabBarController` 既定で選択されている最初のタブでが読み込まれます。 他のいずれかのタブを選択すると、次に示すように、関連付けられているコントローラーのビューがに表示され `UITabBarController,` ます。次に、エンドユーザーが2番目のタブを選択しています。

![2番目のタブ](creating-tabbed-applications-images/03-secondtab-sml.png)

### <a name="modifying-tabbaritems"></a>タブバー項目の変更

タブアプリケーションが完成したので、を変更して、 `TabBarItem` 表示されるイメージとテキストを変更し、1つのタブにバッジを追加してみましょう。

#### <a name="setting-a-system-item"></a>システム項目の設定

まず、システム項目を使用するように最初のタブを設定します。 のコンストラクターで、 `TabController` インスタンスのコントローラーを設定する行を削除 `Title` し、 `tab1` コントローラーのプロパティを設定するために次のコードに置き換え `TabBarItem` ます。

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

を使用してを作成すると `UITabBarItem` `UITabBarSystemItem` 、次のスクリーンショットに示すように、タイトルと画像が iOS に**Favorites**よって自動的に提供されます。

 ![星のアイコンが付いた最初のタブ](creating-tabbed-applications-images/04a-tabimage-sml.png)

#### <a name="setting-the-image"></a>画像の設定

システム項目を使用するだけでなく、のタイトルとイメージを `UITabBarItem` カスタム値に設定することもできます。 たとえば、という名前のコントローラーのプロパティを設定するコードを次のように変更し `TabBarItem` `tab2` ます。

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

上のコードでは、という名前のイメージが `second.png` プロジェクトのルート (または **リソース** ディレクトリ) に追加されていることを前提としています。 すべての画面密度をサポートするには、次のように3つのイメージが必要です。

![プロジェクトに追加されたイメージ](creating-tabbed-applications-images/tabbedimages7new.png)

正しいディメンションのガイダンスについては、 [Apple のカスタムアイコンページ](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/custom-icons/)の**タブバーアイコンのサイズ**に関するセクションを参照してください。 推奨されるサイズは、イメージのスタイル (円形、正方形、横、または高さ) によって異なります。

`Image`プロパティは**second.png**ファイル名に設定するだけで十分です。 iOS では、必要に応じて、より高い解像度のファイルが自動的に読み込まれます。 詳細については、「 [イメージの操作](~/ios/app-fundamentals/images-icons/index.md) 」ガイドを参照してください。 既定では、タブバー項目はグレーで、選択した場合は青色で表示されます。

#### <a name="overriding-the-title"></a>タイトルのオーバーライド

`Title`プロパティがに直接設定されている場合は、 `TabBarItem` コントローラー自体でに設定されているすべての値がオーバーライドされ `Title` ます。

このスクリーンショットの2番目の (中央) タブには、カスタムのタイトルと画像が表示されています。

![2番目のタブと四角形のアイコン](creating-tabbed-applications-images/05-customtab-sml.png)

#### <a name="setting-the-badge-value"></a>バッジ値の設定

また、1つのタブにバッジを表示することもできます。 たとえば、3番目のタブにバッジを設定するには、次のコード行を追加します。

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

次に示すように、この結果を実行すると、タブの左上隅に "Hi" という文字列の赤いラベルが表示されます。

![2番目のタブとこんにちは](creating-tabbed-applications-images/06-badge-sml.png)

バッジは多くの場合、未読の新しい項目を表示するために使用されます。 バッジを削除するには、次のようにを `BadgeValue` null に設定します。

```csharp
tab3.TabBarItem.BadgeValue = null;
```

## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>RootViewController 以外のシナリオのタブ

上の例では、がウィンドウのである場合に、を操作する方法を示しまし `UITabBarController` た `RootViewController` 。 この例では、がではなく、 `UITabBarController` ストーリーボードの使用方法を示すために、を使用する方法を確認 `RootViewController` します。

### <a name="initial-screen-example"></a>最初の画面の例

このシナリオでは、初期画面はではないコントローラーから読み込まれ `UITabBarController` ます。 ユーザーがボタンをタップして画面を操作すると、同じビューコントローラーがに読み込まれ、 `UITabBarController` ユーザーに表示されます。 次のスクリーンショットには、アプリケーション フローが示されています。

[![このスクリーンショットは、アプリケーションフローを示しています。](creating-tabbed-applications-images/inital-screen-application.png)](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

この例では、新しいアプリケーションを開始してみましょう。 ここでも、 **iPhone > App > 空のプロジェクト (C#)** テンプレートを使用します。今度はプロジェクトに名前を付け `InitialScreenDemo` ます。

この例では、ストーリーボードを使用してビューコントローラーをレイアウトします。 ストーリーボードを追加するには:

- プロジェクト名を右クリックし、[ **新しいファイルの追加 >**] を選択します。

- [新しいファイル] ダイアログが表示されたら、[ **iOS > 空の IPhone ストーリーボード**] に移動します。

次に示すように、この新しいストーリーボード **mainstoryboard.storyboard ファイル** を呼び出しましょう。

[![Mainstoryboard.storyboard ファイルファイルをプロジェクトに追加する](creating-tabbed-applications-images/new-file-dialog.png)](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

前のストーリーボード以外のファイルにストーリーボードを追加する際に注意する必要がある重要な手順がいくつかあります。これについては、「 [ストーリーボードの概要](~/ios/user-interface/storyboards/index.md) 」ガイドで説明されています。 次のとおりです。

1. の **メインインターフェイス** セクションにストーリーボード名を追加し `Info.plist` ます。

    [![Main インターフェイスを Mainstoryboard.storyboard ファイルに設定します。](creating-tabbed-applications-images/project-options.png)](creating-tabbed-applications-images/project-options.png#lightbox)
1. で、 `App Delegate` 次のコードを使用して、ウィンドウメソッドをオーバーライドします。

    ```csharp
    public override UIWindow Window {
        get;
        set;
    }
    ```

この例では、3つのビューコントローラーが必要になります。 という名前の1つは、最初 `ViewController1` のビューコントローラーとして、および最初のタブで使用されます。 `ViewController2` `ViewController3` 2 番目と3番目のタブでそれぞれ使用されるおよびという名前のその他の2つの。

Mainstoryboard.storyboard ファイルファイルをダブルクリックしてデザイナーを開き、3つのビューコントローラーをデザイン画面にドラッグします。 これらの各ビューコントローラーには上記の名前に対応する独自のクラスが必要です。そのため、次のスクリーンショットに示すように、[ **Identity > クラス**] に名前を入力します。

[![クラスを ViewController1 に設定します。](creating-tabbed-applications-images/class-name.png)](creating-tabbed-applications-images/class-name.png#lightbox)

次の図に示すように、必要なクラスとデザイナーファイルが自動的に生成され、Solution Pad に表示されることが Visual Studio for Mac ます。

[![プロジェクト内の自動生成されたファイル](creating-tabbed-applications-images/solution-pad2.png)](creating-tabbed-applications-images/solution-pad2.png#lightbox)

#### <a name="creating-the-ui"></a>UI の作成

次に、Xamarin iOS Designer を使用して、ViewController のビューごとに単純なユーザーインターフェイスを作成します。

`Label` `Button` 右側の**ツールボックス**から、とを ViewController1 にドラッグします。 次に、Properties Pad を使用して、コントロールの名前とテキストを次のように編集します。

- **ラベル**: `Text`  =  **1**
- **ボタン**: `Title`  =  **ユーザーが最初の操作を実行**する

イベントのボタンの表示を制御し、 `TouchUpInside` コードビハインドでそのボタンを参照する必要があります。 **Name** `aButton` 次のスクリーンショットに示すように、Properties Pad 内の名前で識別します。

[![Properties Pad の名前を「aButton」に設定します。](creating-tabbed-applications-images/abutton-properties.png)](creating-tabbed-applications-images/abutton-properties.png#lightbox)

デザインサーフェイスは、次のスクリーンショットのようになります。

[![デザインサーフェイスは次のスクリーンショットのようになります。](creating-tabbed-applications-images/design-surface1.png)](creating-tabbed-applications-images/design-surface1.png#lightbox)

さらに詳細な情報をとに追加し `ViewController2` `ViewController3` 、ラベルをそれぞれに追加して、テキストをそれぞれ ' 2 ' と ' 3 ' に変更してみましょう。 ここでは、表示しているタブ/ビューをユーザーに強調表示します。

#### <a name="wiring-up-the-button"></a>ボタンの配線

`ViewController1`アプリケーションを初めて起動するときに読み込みます。 ユーザーがボタンをタップすると、ボタンが非表示になり、 `UITabBarController` 最初のタブにインスタンスと共にが読み込ま `ViewController1` れます。

ユーザーがをリリースすると `aButton` 、TouchUpInside イベントがトリガーされます。 ボタンを選択し、[プロパティ] パッドの [ **イベント] タブ** で、イベントハンドラーを宣言し `InitialActionCompleted` ます。これは、コードで参照できるようにするためです。 これを次のスクリーンショットに示します。

[![ユーザーが aButton を解放すると、TouchUpInside イベントがトリガーされます。](creating-tabbed-applications-images/event-handler.png)](creating-tabbed-applications-images/event-handler.png#lightbox)

次に、イベントが発生したときにボタンを非表示にするようにビューコントローラーに指示する必要があり `InitialActionCompleted` ます。 で `ViewController1` 、次の部分メソッドを追加します。

```csharp
partial void InitialActionCompleted (UIButton sender)
{
    aButton.Hidden = true;  
}
```

ファイルを保存し、アプリケーションを実行します。 画面が表示され、ボタンが再び表示されます。

#### <a name="adding-the-tab-bar-controller"></a>タブバーコントローラーの追加

これで、初期ビューが想定どおりに動作するようになりました。 次に、 `UITabBarController` ビュー2および3と共に、それをに追加します。 デザイナーでストーリーボードを開きましょう。

**ツールボックス**で、[コントローラー & オブジェクト] の下にある**タブバーコントローラー**を検索し、それをデザインサーフェイスにドラッグします。 次のスクリーンショットでわかるように、タブバーコントローラーは UI がないため、既定では2つのビューコントローラーが表示されます。

[![レイアウトへのタブバーコントローラーの追加](creating-tabbed-applications-images/tabbarcontroller.png)](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

下部にある黒いバーを選択し、del キーを押して、これらの新しいビューコントローラーを削除します。

このストーリーボードでは、セグエを使用して、TabBarController とビューコントローラーの間の遷移を処理できます。 最初のビューと対話した後、ユーザーに提示された TabBarController にそれを読み込みます。 デザイナーでこれを設定してみましょう。

**Ctrl キーを押しながらクリック** して、ボタンから TabBarController に **ドラッグ** します。 マウスポインターを合わせると、コンテキストメニューが表示されます。 ここでは、モーダルセグエを使用します。

各タブを設定するには、次に示すように、1 ~ 3 の順に表示されている各ビューコントローラーに対して **Ctrl キーを押しながら [** リレーションシップ **] タブ** をクリックします。

[![タブリレーションシップを選択します](creating-tabbed-applications-images/context-menu.png)](creating-tabbed-applications-images/context-menu.png#lightbox)

ストーリーボードは次のスクリーンショットのようになります。

[![ストーリーボードは次のスクリーンショットのようになります。](creating-tabbed-applications-images/segue-layout.png)](creating-tabbed-applications-images/segue-layout.png#lightbox)

タブバーの項目のいずれかをクリックして [プロパティ] パネルを調べると、次に示すように、さまざまなオプションが表示されます。

[![プロパティエクスプローラーでのタブオプションの設定](creating-tabbed-applications-images/properties-panel.png)](creating-tabbed-applications-images/properties-panel.png#lightbox)

これを使用すると、バッジ、タイトル、iOS 識別子など、特定の属性を編集できます。

アプリケーションを保存して実行した場合、ViewController1 インスタンスが TabBarController に読み込まれると、ボタンが再び表示されます。 現在のビューに親ビューコントローラーがあるかどうかを確認して、この問題を解決してみましょう。 表示されている場合は、TabBarController 内にあることがわかっているので、ボタンは非表示にする必要があります。 次のコードを ViewController1 クラスに追加してみましょう。

```csharp
public override void ViewDidLoad ()
{
    if (ParentViewController != null){
        aButton.Hidden = true;
    }
}
```

アプリケーションが実行され、ユーザーが最初の画面のボタンをタップすると、UITabBarController が読み込まれ、最初の画面のビューが次のように表示されます。

[![サンプルアプリの出力](creating-tabbed-applications-images/first-view-sml.png)](creating-tabbed-applications-images/first-view.png#lightbox)

## <a name="summary"></a>まとめ

この記事では、アプリケーションでを使用する方法について説明し `UITabBarController` ます。 各タブにコントローラーを読み込む方法と、タイトル、画像、バッジなどのタブのプロパティを設定する方法について説明します。 次に、ストーリーボードを使用して、 `UITabBarController` ウィンドウのではないときにを実行時に読み込む方法を調べます `RootViewController` 。

## <a name="related-links"></a>関連リンク

- [タブ付きアプリケーションの作成 (サンプル)](/samples/xamarin/ios-samples/creatingtabbedapplications)
- [Images.zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [UITabBarController クラスのリファレンス](https://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)