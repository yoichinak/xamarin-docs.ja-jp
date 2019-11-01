---
title: Xamarin. iOS のタブバーとタブバーコントローラー
description: このドキュメントでは、iOS のタブバーコントローラーと、それらを Xamarin で使用する方法について説明します。 ここでは、UITabBarController を設定する方法、イメージを操作する方法、バッジ値を設定する方法、イベントを処理する方法などについて説明します。
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 9f8a5e568946e1aea8541211ec3adc45a25f1897
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022136"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>Xamarin. iOS のタブバーとタブバーコントローラー

タブ付きアプリケーションは、特定の順序で複数の画面にアクセスできるユーザーインターフェイスをサポートするために、iOS で使用されます。 `UITabBarController` クラスを使用すると、アプリケーションには、このようなマルチスクリーンのシナリオのサポートを簡単に含めることができます。 `UITabBarController` はマルチスクリーン管理を行い、アプリケーション開発者は各画面の詳細に集中できます。

通常、タブ付きアプリケーションは、メインウィンドウの `RootViewController` である `UITabBarController` でビルドされます。 ただし、追加のコードを記述することで、タブ付きアプリケーションを他の初期画面に連続して使用することもできます。たとえば、アプリケーションで最初にログイン画面が表示され、次にタブ付きインターフェイスが表示されます。

ここでは、簡単なアプリケーションのチュートリアルを使用して、タブを使用する方法について説明します。 次に、`RootViewController` 以外のシナリオでタブを操作する方法について説明します。

## <a name="introducing-uitabbarcontroller"></a>UITabBarController の概要

`UITabBarController` は、次のようにしてタブ付きアプリケーション開発をサポートしています。

- 複数のコントローラーを追加できるようにします。
- `UITabBar` クラスを使用してタブ付きユーザーインターフェイスを提供し、ユーザーがコントローラーとそのビューを切り替えることができるようにします。 

コントローラーは、`ViewControllers` プロパティ (`UIViewController` 配列) を介して `UITabBarController` に追加されます。 `UITabBarController` 自体は、適切なコントローラーの読み込みと、選択したタブに基づくビューの表示を処理します。

タブは、`UITabBar` インスタンスに含まれる `UITabBarItem` クラスのインスタンスです。 各 `UITabBar` インスタンスには、各タブのコントローラーの `TabBarItem` プロパティを使用してアクセスできます。

`UITabBarController`の操作方法を理解するために、1つを使用する簡単なアプリケーションを構築する方法を説明します。

## <a name="tabbed-application-walkthrough"></a>タブ付きアプリケーションのチュートリアル

このチュートリアルでは、次のアプリケーションを作成します。

[![](creating-tabbed-applications-images/00-app.png "Sample tabbed app")](creating-tabbed-applications-images/00-app.png#lightbox)

Visual Studio for Mac で使用できるタブ付きアプリケーションテンプレートは既に存在しますが、この例では、空のプロジェクトから作業して、アプリケーションの構築方法について理解を深めます。

 <a name="Creating_the_Application" />

### <a name="creating-the-application"></a>アプリケーションの作成

まず、新しいアプリケーションを作成しましょう。

次に示すように、Visual Studio for Mac で **[ファイル > 新しい > ソリューション]** メニュー項目を選択し、 **[空のプロジェクト]** テンプレートを > 選択して、プロジェクトに > という名前を付けます。

[![](creating-tabbed-applications-images/newsolution1.png "Select the Empty Project template")](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![](creating-tabbed-applications-images/newsolution2.png "Name the project TabbedApplication")](creating-tabbed-applications-images/newsolution2.png#lightbox)

### <a name="adding-the-uitabbarcontroller"></a>UITabBarController の追加

次に、 **[ファイル > 新しいファイル]** を選択し、 **[全般: 空のクラス**] テンプレートを選択して、空のクラスを追加します。 次に示すように、ファイルに `TabController` という名前を付けます。

[![](creating-tabbed-applications-images/02-newclass.png "Add the TabController class")](creating-tabbed-applications-images/02-newclass.png#lightbox)

`TabController` クラスには、`UIViewControllers`の配列を管理する `UITabBarController` の実装が含まれます。 ユーザーがタブを選択すると、`UITabBarController` によって適切なビューコントローラーのビューが表示されます。

`UITabBarController` を実装するには、次の手順を実行する必要があります。

1. `TabController` の基本クラスを `UITabBarController` に設定します。 
1. `TabController` に追加する `UIViewController` インスタンスを作成します。 
1. `TabController` の `ViewControllers` プロパティに割り当てられている配列に `UIViewController` インスタンスを追加します。 

次のコードを `TabController` クラスに追加して、これらの手順を実行します。

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

`UIViewController` インスタンスごとに、`UIViewController`の `Title` プロパティを設定します。 コントローラーが `UITabBarController`に追加されると、`UITabBarController` によって各コントローラーの `Title` が読み取られ、次に示すように、関連付けられているタブのラベルに表示されます。

[![](creating-tabbed-applications-images/00-app.png "The sample app run")](creating-tabbed-applications-images/00-app.png#lightbox)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>TabController を RootViewController として設定する

コントローラーがタブに配置される順序は、`ViewControllers` 配列に追加される順序に対応します。

最初の画面として読み込む `UITabController` を取得するには、`AppDelegate`の次のコードに示すように、ウィンドウの `RootViewController`にする必要があります。

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

この時点でアプリケーションを実行すると、既定で選択されている最初のタブで `UITabBarController` が読み込まれます。 他のいずれかのタブを選択すると、次に示すように、関連するコントローラーのビューが `UITabBarController,` によって表示されます。次に、エンドユーザーが2番目のタブを選択します。

[![](creating-tabbed-applications-images/03-secondtab.png "The second tab shown")](creating-tabbed-applications-images/03-secondtab.png#lightbox)

 <a name="Modifying_TabBarItems" />

### <a name="modifying-tabbaritems"></a>タブバー項目の変更

タブアプリケーションを実行したので、`TabBarItem` を変更して、表示されるイメージとテキストを変更したり、バッジをいずれかのタブに追加したりします。

 <a name="Setting_a_System_Item" />

#### <a name="setting-a-system-item"></a>システム項目の設定

まず、システム項目を使用するように最初のタブを設定します。 `TabController`のコンストラクターで、`tab1` インスタンスのコントローラーの `Title` を設定する行を削除し、コントローラーの `TabBarItem` プロパティを設定するために次のコードに置き換えます。

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

`UITabBarSystemItem`を使用して `UITabBarItem` を作成すると、次のスクリーンショットに示すように、タイトルと画像が iOS によって自動的に提供されます。次のスクリーンショットでは、最初のタブに**お気に入り**アイコンとタイトルが示されています。

 ![](creating-tabbed-applications-images/04a-tabimage.png "The first tab with a star icon")

 <a name="Setting_the_Title_and_Image" />

#### <a name="setting-the-title-and-image"></a>タイトルとイメージの設定

システム項目を使用するだけでなく、`UITabBarItem` のタイトルと画像をカスタム値に設定することもできます。 たとえば、次のように `tab2` という名前のコントローラーの `TabBarItem` プロパティを設定するコードを変更します。

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

上のコードでは、`second.png` という名前のイメージが Visual Studio for Mac のプロジェクトのルートに追加されていることを前提としています。 次に示すように、実際には、すべてのデバイスの解像度に対応する3つのイメージをプロジェクトに追加しました。

 [![](creating-tabbed-applications-images/tabbedimages7new.png "The images added to the project")](creating-tabbed-applications-images/tabbedimages7new.png#lightbox)

タブイメージは、通常の解像度の場合は30x30 の png、高解像度の場合はいずれか、iPhone 6 Plus の場合は 90 x 90 にする必要があります。 このコードでは、`second.png` という名前のファイルのみを読み込む必要があります。 iOS は、Retina 表示を使用してデバイス上に高解像度を自動的に読み込みます。 詳細については、「[イメージの操作](~/ios/app-fundamentals/images-icons/index.md)」ガイドを参照してください。 既定では、タブバー項目はグレーで、選択した場合は青色で表示されます。

**注:**

上記のイメージは、**リソース**ディレクトリに追加することもできます。これは、コンテンツがアプリケーションバンドルのルートに自動的にコピーされる特殊なディレクトリです。

[![](creating-tabbed-applications-images/tabbedapplication8.png "The images as Resources")](creating-tabbed-applications-images/tabbedapplication8.png#lightbox)

また、`TabBarItem`の `Title` プロパティを直接設定すると、コントローラー自体で `Title` に設定されている値がオーバーライドされます。

ここでアプリケーションを実行すると、2番目のタブにカスタムのタイトルとイメージが表示されます。次に例を示します。

[![](creating-tabbed-applications-images/05-customtab.png "The second tab with a square icon")](creating-tabbed-applications-images/05-customtab.png#lightbox)

 <a name="Setting_the_Badge_Value" />

#### <a name="setting-the-badge-value"></a>バッジ値の設定

また、1つのタブにバッジを表示することもできます。 たとえば、3番目のタブにバッジを設定するには、次のコード行を追加します。

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

次に示すように、この結果を実行すると、タブの左上隅に "Hi" という文字列の赤いラベルが表示されます。

[![](creating-tabbed-applications-images/06-badge.png "The second tab with a Hi badge")](creating-tabbed-applications-images/06-badge.png#lightbox)

バッジは多くの場合、未読の新しい項目を表示するために使用されます。 バッジを削除するには、次のように `BadgeValue` を null に設定します。

```csharp
tab3.TabBarItem.BadgeValue = null;
```

 <a name="Tabs_in_Non-RootViewController_Scenarios" />

## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>RootViewController 以外のシナリオのタブ

上の例では、`UITabBarController` をウィンドウの `RootViewController` として使用する方法を説明しました。 この例では、`RootViewController` ではなく、ストーリーボードを使用して、`UITabBarController` を使用する方法について説明します。

 <a name="Initial_Screen_Example" />

### <a name="initial-screen-example"></a>最初の画面の例

このシナリオでは、初期画面は `UITabBarController`ではないコントローラーから読み込まれます。 ユーザーがボタンをタップして画面を操作すると、同じビューコントローラーが `UITabBarController`に読み込まれ、ユーザーに表示されます。 次のスクリーンショットは、アプリケーションフローを示しています。

[![](creating-tabbed-applications-images/inital-screen-application.png "This screenshot shows the application flow")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

この例では、新しいアプリケーションを開始してみましょう。 ここでも、 **iPhone > App > 空のプロジェクトC#()** テンプレートを使用します。今度はプロジェクト`InitialScreenDemo`に名前を付けます。

この例では、ビューコントローラーを保持するためのストーリーボードが必要です。 ストーリーボードを追加するには:

- プロジェクト名を右クリックし、 **[新しいファイルの追加 >]** を選択します。

- 新しいファイル ダイアログが表示されたら、 **iOS > 空の IPhone ストーリーボード** に移動します。

次に示すように、この新しいストーリーボード**mainstoryboard.storyboard ファイル**を呼び出しましょう。 

[![](creating-tabbed-applications-images/new-file-dialog.png "Add a MainStoryboard file to the project")](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

前のストーリーボード以外のファイルにストーリーボードを追加する際に注意する必要がある重要な手順がいくつかあります。これについては、「[ストーリーボードの概要](~/ios/user-interface/storyboards/index.md)」ガイドで説明されています。 これらの数値は、次のとおりです。

1. `Info.plist`の**メインインターフェイス**セクションにストーリーボード名を追加します。

    [![](creating-tabbed-applications-images/project-options.png "Set the Main Interface to MainStoryboard")](creating-tabbed-applications-images/project-options.png#lightbox)
1. `App Delegate`で、次のコードを使用して、ウィンドウメソッドをオーバーライドします。

    ```csharp
    public override UIWindow Window {
        get;
        set;
    }
    ```

この例では、3つのビューコントローラーが必要になります。 `ViewController1`という名前の1つは、最初のビューコントローラーとして、および最初のタブで使用されます。2番目と3番目のタブでそれぞれ使用される、`ViewController2` と `ViewController3`という名前の他の2つの。

Mainstoryboard.storyboard ファイルファイルをダブルクリックしてデザイナーを開き、3つのビューコントローラーをデザイン画面にドラッグします。 これらの各ビューコントローラーには上記の名前に対応する独自のクラスが必要です。そのため、次のスクリーンショットに示すように、 **[Identity > クラス]** に名前を入力します。

[![](creating-tabbed-applications-images/class-name.png "Set the Class to ViewController1")](creating-tabbed-applications-images/class-name.png#lightbox)

次の図に示すように、必要なクラスとデザイナーファイルが自動的に生成され、Solution Pad に表示されることが Visual Studio for Mac ます。

[![](creating-tabbed-applications-images/solution-pad2.png "Auto-generated files in the project")](creating-tabbed-applications-images/solution-pad2.png#lightbox)

 <a name="Creating_the_UI" />

#### <a name="creating-the-ui"></a>UI の作成

次に、Xamarin iOS Designer を使用して、ViewController のビューごとに単純なユーザーインターフェイスを作成します。

右側の**ツールボックス**から、`Label` と `Button` を ViewController1 にドラッグします。 次に、Properties Pad を使用して、コントロールの名前とテキストを次のように編集します。

- **ラベル**: **1** = `Text`
- **ボタン**: `Title` = **ユーザーが最初の操作を実行**する

ここでは、`TouchUpInside` イベントでのボタンの表示を制御し、コードビハインドでそのボタンを参照する必要があります。 次のスクリーンショットに示すように、Properties Pad の `aButton`**名前**で識別します。

[![](creating-tabbed-applications-images/abutton-properties.png "Set the Name to aButton in the Properties Pad")](creating-tabbed-applications-images/abutton-properties.png#lightbox)

デザインサーフェイスは、次のスクリーンショットのようになります。

[![](creating-tabbed-applications-images/design-surface1.png "Your Design Surface should now look similar to this screenshot")](creating-tabbed-applications-images/design-surface1.png#lightbox)

ここでは、ラベルをそれぞれに追加し、テキストをそれぞれ ' Two ' と ' 3 ' に変更することにより、`ViewController2` と `ViewController3`についてさらに詳しく説明します。 ここでは、表示しているタブ/ビューをユーザーに強調表示します。

#### <a name="wiring-up-the-button"></a>ボタンの配線

アプリケーションが初めて起動したときに `ViewController1` を読み込みます。 ユーザーがボタンをタップすると、ボタンが非表示になり、最初のタブに `ViewController1` インスタンスを含む `UITabBarController` が読み込まれます。

ユーザーが `aButton`を解放すると、TouchUpInside イベントがトリガーされます。 ボタンを選択し、[プロパティ] パッドの [**イベント] タブ**で、イベントハンドラー (`InitialActionCompleted`) を宣言して、コードで参照できるようにしましょう。 これを次のスクリーンショットに示します。

[![](creating-tabbed-applications-images/event-handler.png "When the user releases the aButton, trigger a TouchUpInside event")](creating-tabbed-applications-images/event-handler.png#lightbox)

次に、イベントが `InitialActionCompleted`発生したときにボタンを非表示にするようにビューコントローラーに指示する必要があります。 `ViewController1`で、次の部分メソッドを追加します。

```csharp
partial void InitialActionCompleted (UIButton sender)
{
    aButton.Hidden = true;  
}
```

ファイルを保存し、アプリケーションを実行します。 画面が表示され、ボタンが再び表示されます。

#### <a name="adding-the-tab-bar-controller"></a>タブバーコントローラーの追加

これで、初期ビューが想定どおりに動作するようになりました。 次に、ビュー2および3と共に `UITabBarController`に追加します。 デザイナーでストーリーボードを開きましょう。

**ツールボックス**で、[コントローラー & オブジェクト] の下にある**タブバーコントローラー**を検索し、それをデザインサーフェイスにドラッグします。 次のスクリーンショットでわかるように、タブバーコントローラーは UI がないため、既定では2つのビューコントローラーが表示されます。

[![](creating-tabbed-applications-images/tabbarcontroller.png "Adding a Tab Bar Controller to the layout")](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

下部にある黒いバーを選択し、del キーを押して、これらの新しいビューコントローラーを削除します。

このストーリーボードでは、セグエを使用して、TabBarController とビューコントローラーの間の遷移を処理できます。 最初のビューと対話した後、ユーザーに提示された TabBarController にそれを読み込みます。 デザイナーでこれを設定してみましょう。

**Ctrl キーを押しながらクリック**して、ボタンから TabBarController に**ドラッグ**します。 マウスポインターを合わせると、コンテキストメニューが表示されます。 ここでは、モーダルセグエを使用します。 

各タブを設定するには、次に示すように、1 ~ 3 の順に表示されている各ビューコントローラーに対して**Ctrl キーを押しながら [** リレーションシップ **] タブ**をクリックします。

[![](creating-tabbed-applications-images/context-menu.png "Select the Tab Relationship")](creating-tabbed-applications-images/context-menu.png#lightbox)

ストーリーボードは次のスクリーンショットのようになります。

[![](creating-tabbed-applications-images/segue-layout.png "The Storyboard should resemble this screenshot")](creating-tabbed-applications-images/segue-layout.png#lightbox)

タブバーの項目のいずれかをクリックして [プロパティ] パネルを調べると、次に示すように、さまざまなオプションが表示されます。

[![](creating-tabbed-applications-images/properties-panel.png "Setting the tab options in the Properties Explorer")](creating-tabbed-applications-images/properties-panel.png#lightbox)

これを使用して、バッジ、タイトル、iOS[識別子](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/UIKitUICatalog/TabBarItem.html)など、特定の属性を編集できます。

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

[![](creating-tabbed-applications-images/first-view.png "The sample app output")](creating-tabbed-applications-images/first-view.png#lightbox)

<!--Save the files and run the application:

[![](creating-tabbed-applications-images/inital-screen-application.png "Save the files and run the application")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)-->

## <a name="summary"></a>まとめ

この記事では、アプリケーションで `UITabBarController` を使用する方法について説明します。 各タブにコントローラーを読み込む方法と、タイトル、画像、バッジなどのタブのプロパティを設定する方法について説明します。 次に、ストーリーボードを使用して、ウィンドウの `RootViewController` ではないときに実行時に `UITabBarController` を読み込む方法について説明します。

## <a name="related-links"></a>関連リンク

- [タブ付きアプリケーションの作成 (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/creatingtabbedapplications)
- [画像 .zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [UITabBarController クラスのリファレンス](https://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
