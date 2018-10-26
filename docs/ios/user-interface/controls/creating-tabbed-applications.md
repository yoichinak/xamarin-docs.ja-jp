---
title: タブ バーと Xamarin.iOS でのタブ バー コント ローラー
description: このドキュメントでは、iOS タブ バー コント ローラーと Xamarin.iOS でそれらを使用する方法について説明します。 UITabBarController 設定、画像の操作、バッジ値、イベント、操作を設定する方法を示します。
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: e4ab46994ed25daaef95a709e4f9df94f3a21cd0
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114679"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>タブ バーと Xamarin.iOS でのタブ バー コント ローラー

タブ付きアプリケーションは、任意の順序で複数の画面にアクセスできる場所のユーザー インターフェイスをサポートするために iOS で使用されます。 を通じて、`UITabBarController`クラス、アプリケーションは、このようなマルチ スクリーンのシナリオのサポートを含めることができます簡単にします。 `UITabBarController` アプリケーション開発者は各画面の詳細に注目できるように、マルチ スクリーンの管理を行います。

通常、タブ付きアプリケーションをビルドと、`UITabBarController`されている、`RootViewController`のメイン ウィンドウ。 ただし、少し追加のコードのタブ付きアプリケーションこともできるアプリケーションで最初に、タブ付きインターフェイスが後に、ログイン画面が提示するシナリオなど、初期画面に連続しています。

ここでのタブを使用して、単純なアプリケーションのチュートリアルに取り組むことによりについて説明します。 次に、以外でタブを使用する方法を紹介`RootViewController`シナリオ。

## <a name="introducing-uitabbarcontroller"></a>UITabBarController の概要

`UITabBarController`タブ付きアプリケーションの開発を以下がサポートされます。

-  複数のコント ローラーを追加することができます。
-  使用して、タブ付きのユーザー インターフェイスを提供する、`UITabBar`クラスは、コント ローラーとビューを切り替えるユーザーを許可します。 


コント ローラーに追加、`UITabBarController`経由でその`ViewControllers`であるプロパティを`UIViewController`配列。 `UITabBarController`自体の処理の適切なコント ローラーの読み込みと選択されたタブに基づいてそのビューを表示します。

インスタンスは、タブが、`UITabBarItem`クラスに含まれている、`UITabBar`インスタンス。 各`UITabBar`インスタンスを使用してアクセスできますが、`TabBarItem`各タブで、コント ローラーのプロパティ。

使用する方法を理解する、 `UITabBarController`、1 つを使用する単純なアプリケーションを構築してみましょう。

## <a name="tabbed-application-walkthrough"></a>タブ付きアプリケーションのチュートリアル

このチュートリアルでは、次のアプリケーションを作成していきます。

[![](creating-tabbed-applications-images/00-app.png "タブ付きアプリのサンプル")](creating-tabbed-applications-images/00-app.png#lightbox)

この例では、Visual Studio for Mac、使用可能なタブ付きアプリケーション テンプレートに既にがアプリケーションの構築方法の理解を深めるに空のプロジェクトから作業になります。

 <a name="Creating_the_Application" />


### <a name="creating-the-application"></a>アプリケーションの作成

新しいアプリケーションを作成してみましょう。

選択、**ファイル > 新規 > ソリューション**Visual Studio for Mac と選択のメニュー項目を**iOS > アプリ > 空のプロジェクト**名では、プロジェクト テンプレートは、`TabbedApplication`以下に示すように。

[![](creating-tabbed-applications-images/newsolution1.png "空のプロジェクト テンプレートを選択します。")](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![](creating-tabbed-applications-images/newsolution2.png "TabbedApplication プロジェクトを名前します。")](creating-tabbed-applications-images/newsolution2.png#lightbox)



### <a name="adding-the-uitabbarcontroller"></a>UITabBarController を追加します。

次に、空のクラスを選択して追加**ファイル > 新規ファイル**を選択して、**全般: 空のクラス**テンプレート。 ファイルに名前を`TabController`次に示すよう。

[![](creating-tabbed-applications-images/02-newclass.png "TabController クラスを追加します。")](creating-tabbed-applications-images/02-newclass.png#lightbox)

`TabController`クラスの実装を含む、`UITabBarController`の配列を管理する`UIViewControllers`します。 ユーザーが、タブを選択すると、`UITabBarController`の適切なビュー コント ローラーのビューの表示が取得されます。

実装する、`UITabBarController`以下を実行する必要があります。

1.  基本クラスを設定する`TabController`に`UITabBarController`します。 
1.  作成`UIViewController`に追加するインスタンス、`TabController`します。 
1.  追加、`UIViewController`インスタンスに割り当てられた配列を`ViewControllers`のプロパティ、`TabController`します。 


次のコードを追加、`TabController`を次の手順を実現するクラス。

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

それぞれの注意`UIViewController`設定インスタンス、`Title`のプロパティ、`UIViewController`します。 コント ローラーを追加するときに、 `UITabBarController`、`UITabBarController`は読み取り、`Title`各コント ローラーと、次に示すように、関連付けられているタブのラベルに表示。

[![](creating-tabbed-applications-images/00-app.png "サンプル アプリの実行")](creating-tabbed-applications-images/00-app.png#lightbox)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>RootViewController として、TabController を設定します。

タブに、コント ローラーを配置する順序に追加される順序に対応、`ViewControllers`配列。

取得する、`UITabController`を最初の画面として読み込む必要がありますが、ウィンドウの`RootViewController`の次のコードのように、 `AppDelegate`:

```csharp
[Register ("AppDelegate")]
        public partial class AppDelegate : UIApplicationDelegate
        {
                UIWindow window;
                TabController tabController;

                public override bool FinishedLaunching (UIApplication app, NSDictionary options)
                {
                        window = new UIWindow (UIScreen.MainScreen.Bounds);

                        var tabController = new TabController ();
                        window.RootViewController = tabController;

                        window.MakeKeyAndVisible ();
            
                        return true;
                }
        }
```

ここで、アプリケーションを実行した場合、`UITabBarController`既定で選択されている最初のタブが読み込まれます。 関連付けられているコント ローラーの結果ビューの他のタブのいずれかを選択で表示されている、`UITabBarController,`次に、エンドユーザーが 2 番目のタブを選択する場所を示します。

[![](creating-tabbed-applications-images/03-secondtab.png "2 番目のタブに表示")](creating-tabbed-applications-images/03-secondtab.png#lightbox)

 <a name="Modifying_TabBarItems" />


### <a name="modifying-tabbaritems"></a>TabBarItems を変更します。

実行中のアプリケーションのタブで、変更してみましょう取得したので、`TabBarItem`イメージと表示されるテキストを変更するだけでなく、タブのいずれかにバッジを追加します。

 <a name="Setting_a_System_Item" />


#### <a name="setting-a-system-item"></a>システムの項目の設定

最初に、システムの項目を使用する最初のタブを設定してみましょう。 コンス トラクター、 `TabController`、コント ローラーを設定する行を削除`Title`の`tab1`をインスタンス化し、コント ローラーを設定するのには、次のコードに置き換えます`TabBarItem`プロパティ。

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

作成するときに、`UITabBarItem`を使用して、 `UITabBarSystemItem`、イメージ、タイトルは自動的に iOS によって提供される、表示されている以下のスクリーン ショットに示すよう、**お気に入り**アイコンと最初のタブのタイトル。

 ![](creating-tabbed-applications-images/04a-tabimage.png "星のアイコンでは、最初のタブ")

 <a name="Setting_the_Title_and_Image" />


#### <a name="setting-the-title-and-image"></a>タイトルとイメージの設定

システム アイテムをタイトル、およびイメージの使用に加えて、`UITabBarItem`カスタム値に設定することができます。 設定するコードを変更するなど、`TabBarItem`という名前のコント ローラーのプロパティ`tab2`次のようにします。

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

上記のコードには、という名前のイメージが前提としています`second.png`for mac に Visual Studio でプロジェクトのルートが追加されました 3 つのイメージを次に示すように、すべてのデバイスの解像度をカバーする、プロジェクトに実際に追加しました。

 [![](creating-tabbed-applications-images/tabbedimages7new.png "プロジェクトに追加のイメージ")](creating-tabbed-applications-images/tabbedimages7new.png#lightbox)

タブの画像で透明度が通常の解決、60 の高解像度と iPhone 6 用 90 x 90 x 60 の 30 x 30 png をする必要がありますプラスの解像度。 コードでのみ必要がありますをという名前のファイルを読み込む`second.png`iOS は、高解像度 Retina ディスプレイを備えたデバイスで自動的に読み込むとします。 詳細については、これには、[イメージを操作](~/ios/app-fundamentals/images-icons/index.md)ガイド。 既定では、タブ バー項目は、選択されたときに青の色の灰色をされます。

**注:**

上の図にも追加できる、**リソース**ディレクトリで、特別なディレクトリのコンテンツは、アプリケーション バンドルのルートに自動的にコピーされます。

[![](creating-tabbed-applications-images/tabbedapplication8.png "リソースとしてイメージ")](creating-tabbed-applications-images/tabbedapplication8.png#lightbox)

さらに、設定すると、`Title`プロパティで直接、 `TabBarItem`、任意の値の設定が上書きされます`Title`コント ローラー自体にします。

アプリケーションを今すぐ実行する 2 番目のタブは次のように、カスタム タイトルとイメージを示します。

[![](creating-tabbed-applications-images/05-customtab.png "正方形のアイコンを 2 番目のタブ")](creating-tabbed-applications-images/05-customtab.png#lightbox)

 <a name="Setting_the_Badge_Value" />


#### <a name="setting-the-badge-value"></a>バッジ値の設定

タブには、バッジも表示できます。 たとえば、次の 3 番目のタブにバッジを設定するコードの行を追加します。

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

これを実行すると、赤色のラベルを次に示すように、タブの左上隅にある文字列"Hi"が発生します。

[![](creating-tabbed-applications-images/06-badge.png "Hi バッジと 2 番目のタブ")](creating-tabbed-applications-images/06-badge.png#lightbox)

、未読の番号を示す値を表示するバッジが使用される多くの場合、新しい項目。 バッジを削除するには、設定、`BadgeValue`を次に示すように null にします。

```csharp
tab3.TabBarItem.BadgeValue = null;
```

 <a name="Tabs_in_Non-RootViewController_Scenarios" />


## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>RootViewController 以外のシナリオでのタブ

上記の例で使用する方法を紹介しました、`UITabBarController`場合、`RootViewController`ウィンドウ。 使用する方法について、この例では、`UITabBarController`ではない場合、`RootViewController`ストーリー ボードを使用して、これを作成する方法を表示するとします。

 <a name="Initial_Screen_Example" />


### <a name="initial-screen-example"></a>最初の画面の例

このシナリオでは、最初の画面の読み込みがコント ローラーから、`UITabBarController`します。 ボタンをタップして、ユーザーの画面が対話するとき、同じビュー コント ローラーが読み込まれる、`UITabBarController`がユーザーに表示しています。 次のスクリーン ショットは、アプリケーション フローを示しています。

[![](creating-tabbed-applications-images/inital-screen-application.png "このスクリーン ショットは、アプリケーション フローを示しています。")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

この例では新しいアプリケーションを起動してみましょう。 使用して、もう一度、 **iPhone > アプリ > 空のプロジェクト (C#)** テンプレート、今度は、プロジェクトの名前を付け`InitialScreenDemo`します。


この例では、ストーリー ボードのビュー コント ローラーを保持する必要があります。 ストーリー ボードを追加するには。

- プロジェクト名を右クリックし、選択**追加 > 新しいファイル**します。

- 移動し、新しいファイル ダイアログが表示されたら、 **iOS > 空の iPhone ストーリー ボード**します。

この新しいストーリー ボードをましょう**MainStoryboard**以下に示すように。 

[![](creating-tabbed-applications-images/new-file-dialog.png "MainStoryboard ファイルをプロジェクトに追加します。")](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

いくつかの重要な手順に記載されていますが、以前ストーリー ボード ファイルにストーリー ボードを追加するときに注意してください、[ストーリー ボードの概要](~/ios/user-interface/storyboards/index.md)ガイド。 これらの数値は、次のとおりです。

 
1. ストーリー ボード名を追加、**メイン インターフェイス**のセクション、 `Info.plist`:

    [![](creating-tabbed-applications-images/project-options.png "MainStoryboard にメインのインターフェイスを設定します。")](creating-tabbed-applications-images/project-options.png#lightbox)
1. `App Delegate`、次のコードで、ウィンドウ メソッドをオーバーライドします。

    ```csharp
    public override UIWindow Window {
      get;
      set;
    }
    ```

この例で 3 つのビュー コント ローラーを必要になるされます。 という名前の 1 つ`ViewController1`、および最初のタブで、初期のビュー コント ローラーとして使用されます。その他の 2 という`ViewController2`と`ViewController3`、どちらを使用する 2 番目と 3 番目のタブにそれぞれします。

MainStoryboard.storyboard ファイルをダブルクリックしてデザイナーを開き、デザイン画面の 3 つのビュー コント ローラーをドラッグします。 必要、上記の名前に対応する独自のクラスがこれらのビュー コント ローラーの各は、 **Identity > クラス**、次のスクリーン ショットに示すように、名前を入力します。

[![](creating-tabbed-applications-images/class-name.png "クラスを ViewController1 に設定します。")](creating-tabbed-applications-images/class-name.png#lightbox)

クラスとデザイナーのファイルが必要な visual Studio for Mac が自動的に生成、発生することは、Solution Pad で次のようにします。

[![](creating-tabbed-applications-images/solution-pad2.png "プロジェクト内の自動生成されたファイル")](creating-tabbed-applications-images/solution-pad2.png#lightbox)

 <a name="Creating_the_UI" />


#### <a name="creating-the-ui"></a>UI を作成します。

次に、作成しますシンプルなユーザー インターフェイスの各 ViewController のビューでは、Xamarin iOS デザイナーを使用しています。

ドラッグして、`Label`と`Button`から ViewController1 上に、**ツールボックス**右側にします。 次に、名前と、次のコントロールのテキストを編集するのに [プロパティ] タブを使用します。

-  **ラベル**: `Text`  = **いずれか**
-  **ボタン**: `Title`  = **ユーザーがいくつかの初期操作**


ボタンの可視性を制御します、`TouchUpInside`イベント、および私たちは、分離コードで参照する必要があります。 みましょうを識別、**名前**`aButton`プロパティ パッドで、次のスクリーン ショットに示すようにします。

[![](creating-tabbed-applications-images/abutton-properties.png "Properties Pad でボタンに名前を設定します。")](creating-tabbed-applications-images/abutton-properties.png#lightbox)

デザイン画面は次のスクリーン ショットのようになります。

[![](creating-tabbed-applications-images/design-surface1.png "デザイン画面はこのスクリーン ショットのようになります")](creating-tabbed-applications-images/design-surface1.png#lightbox)

もう少し詳しくするを追加してみましょう`ViewController2`と`ViewController3`では、各ラベルを追加して、'2' と '3' に、テキストを変更すると、それぞれします。 ユーザーにハイライト見ているタブ/ビュー。

#### <a name="wiring-up-the-button"></a>接続ボタン

読み込む`ViewController1`アプリケーションを最初に起動します。 ボタンを非表示をロードします、ユーザーがボタンをタップしたとき、`UITabBarController`で、`ViewController1`最初のタブでインスタンス。

ユーザーを解放すると、 `aButton`、TouchUpInside イベントを起動させるします。 ボタンを選択し、**イベント タブ**プロパティ パッドの宣言、イベント ハンドラー: `InitialActionCompleted` – でコードを参照できるようにします。 これは、次のスクリーン ショットに示します。

[![](creating-tabbed-applications-images/event-handler.png "ユーザーが、ボタンを離したときにトリガー TouchUpInside イベント")](creating-tabbed-applications-images/event-handler.png#lightbox)

これで、イベントが発生したときに、ボタンを非表示にするビュー コント ローラーを指示する必要があります`InitialActionCompleted`します。 `ViewController1`、次の部分メソッドを追加します。

```csharp
partial void InitialActionCompleted (UIButton sender)
    {
      aButton.Hidden = true;  
    }
```

ファイルを保存し、アプリケーションを実行します。 私たちは、1 つ表示画面と、ボタンをタッチで非表示になりますが表示されます。

#### <a name="adding-the-tab-bar-controller"></a>タブ バーを追加するコント ローラー

この初期ビューが想定どおりに動作があるようになりました。 次に、追加する、`UITabBarController`ビュー 2 および 3 と共にします。 デザイナーで、ストーリー ボードを開いてみましょう。

**ツールボックス**、検索、**タブ バー コント ローラー** コント ローラーとオブジェクトとデザイン画面上にドラッグします。 タブ バー コント ローラーは、次のスクリーン ショットで示すように、UI のないし、そのためは、既定ではこれで 2 つのビュー コント。

[![](creating-tabbed-applications-images/tabbarcontroller.png "レイアウトにタブ バー コント ローラーの追加")](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

これらの新しいビュー コント ローラーを削除するには、下部にある黒いバーを選択し、del キーを押します。

私たちのストーリー ボードの Segues、TabBarController と、ビュー コント ローラー間の遷移を処理するために使用できます。 初期表示を対話したら、それをユーザーに提示 TabBarController にロードします。 みましょうこれデザイナーで設定します。

**Ctrl-クリック**と**ドラッグ**TabBarController にボタンをクリックします。 マウス時に、コンテキスト メニューが表示されます。 モーダルのセグエを使用します。 
 
各タブを設定する**Ctrl-クリック**から 3、およびリレーションシップを選択する 1 つの順序でこれらのビュー コント ローラーの各 TabBarController から**タブ**下図のように、コンテキスト メニューから。

[![](creating-tabbed-applications-images/context-menu.png "タブのリレーションシップを選択します。")](creating-tabbed-applications-images/context-menu.png#lightbox)

ストーリー ボードは次のスクリーン ショットのようになります。

[![](creating-tabbed-applications-images/segue-layout.png "このスクリーン ショットのようになります、ストーリー ボード")](creating-tabbed-applications-images/segue-layout.png#lightbox)

タブ バー項目のいずれかをクリックし、[プロパティ] パネルを表示する場合は、以下に示すように、各種のオプションの数を確認できます。

[![](creating-tabbed-applications-images/properties-panel.png "プロパティ エクスプ ローラー タブのオプションを設定します。")](creating-tabbed-applications-images/properties-panel.png#lightbox)

これを使用して、バッジ、タイトル、iOS などの特定の属性を編集[識別子](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/UIKitUICatalog/TabBarItem.html)、他のユーザーの間で

保存し、アプリケーションを実行する場合、TabBarController に ViewController1 インスタンスが読み込まれるときがボタンに再び表示されますが分かります。 親ビュー コント ローラーが現在のビューをチェックして、これを修正しましょう。 わかった場合は、TabBarController 中ですし、ボタンを非表示にそのためです。 ViewController1 クラスに次のコードを追加しましょう。

```csharp
public override void ViewDidLoad ()
{
     if (ParentViewController != null){
       aButton.Hidden = true;
     }

}
```

アプリケーションが実行されると、ユーザーが、UITabBarController、最初の画面でボタンをタップするときに読み込まれると、ビューを次に示すように、最初のタブに配置する最初の画面から。

[![](creating-tabbed-applications-images/first-view.png "アプリのサンプル出力")](creating-tabbed-applications-images/first-view.png#lightbox)

<!--Save the files and run the application:

[![](creating-tabbed-applications-images/inital-screen-application.png "Save the files and run the application")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)-->

## <a name="summary"></a>まとめ

この記事では、使用する方法を説明する`UITabBarController`アプリケーションでします。 私たちは、各タブにコント ローラーを読み込む方法と、このようなタイトル、イメージおよびバッジのタブでプロパティを設定する方法を説明しました。 いますしを使用して調べる、ストーリー ボードを読み込む方法、`UITabBarController`できない場合に、実行時に、`RootViewController`ウィンドウの。


## <a name="related-links"></a>関連リンク

- [タブ付きアプリケーション (サンプル) を作成します。](https://developer.xamarin.com/samples/monotouch/CreatingTabbedApplications/)
- [Images.zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [UITabBarController クラスのリファレンス](http://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
