---
title: タブ バーおよびタブ バー コント ローラー
description: タブ ナビゲーション UI を使用して iOS アプリケーションは、UITabBarController クラスを使用して構築されます。 この記事の内容をいくつかのコント ローラーとビューを含むタブ付きアプリケーションを設定する方法を説明します。 できない場合に、ルートのコント ローラーなど、ログイン画面の後に、UITabBarController を読み込む方法について確認し、します。
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: c3c57cceed7271ebbe707172db892a246003426b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="tab-bars-and-tab-bar-controllers"></a>タブ バーおよびタブ バー コント ローラー

_タブ ナビゲーション UI を使用して iOS アプリケーションは、UITabBarController クラスを使用して構築されます。この記事の内容をいくつかのコント ローラーとビューを含むタブ付きアプリケーションを設定する方法を説明します。できない場合に、ルートのコント ローラーなど、ログイン画面の後に、UITabBarController を読み込む方法について確認し、します。_

タブ付きのアプリケーションは、iOS で任意の順序で複数の画面にアクセスできる場所のユーザー インターフェイスをサポートするために使用されます。 を介して、`UITabBarController`クラス、アプリケーションがこのようなマルチ スクリーンのシナリオのサポートを簡単に含めることができます。 `UITabBarController` アプリケーション開発者は、各画面の詳細に重点を置くを許可する、マルチ スクリーンの管理を行います。

通常、タブ付きのアプリケーションをビルドと、`UITabBarController`されている、`RootViewController`のメイン ウィンドウのです。 ただし、ほんの少しのコードを追加、タブ付きアプリケーションこともできるアプリケーションは、ログイン画面で、タブ付きインターフェイスが、続けて最初を提示シナリオなど、初期画面に続けてです。

タブは、ここを使用して単純なアプリケーションのチュートリアルを使用して作業を説明します。 以外でタブを使用する方法を紹介し、`RootViewController`シナリオです。

## <a name="introducing-uitabbarcontroller"></a>UITabBarController の概要

`UITabBarController`タブ付きアプリケーションの開発を次をサポートします。

-  複数のコント ローラーに追加することができます。
-  使用して、タブ付きのユーザー インターフェイスを提供する、`UITabBar`クラス、コント ローラーとそのビューを切り替えるユーザーを許可します。 


コント ローラーに追加されて、`UITabBarController`経由でその`ViewControllers`プロパティである、`UIViewController`配列。 `UITabBarController`自体処理する適切なコント ローラーを読み込みし、選択されたタブに基づいてそのビューを提供します。

インスタンスは、タブを`UITabBarItem`に含まれているクラス、`UITabBar`インスタンス。 各`UITabBar`インスタンスが経由でアクセスできる、`TabBarItem`各タブで、コント ローラーのプロパティです。

使用する方法を理解するために、 `UITabBarController`、1 つを使用する簡単なアプリケーションの構築を見てみましょう。

## <a name="tabbed-application-walkthrough"></a>タブ付きのアプリケーションのチュートリアル

このチュートリアルでは、次のアプリケーションを作成しましょう。

[![](creating-tabbed-applications-images/00-app.png "タブ付きのサンプル アプリ")](creating-tabbed-applications-images/00-app.png#lightbox)

この例で、Mac を Visual Studio で使用可能なタブ付きのアプリケーション テンプレートに既にがしようとして、アプリケーションを構築する方法の理解に空のプロジェクトから作業します。

 <a name="Creating_the_Application" />


### <a name="creating-the-application"></a>アプリケーションの作成

新しいアプリケーションを作成して開始しましょう。

選択、**ファイル > 新規 > ソリューション**Mac と選択の Visual Studio のメニュー項目、 **iOS > アプリ > 空のプロジェクト**名では、プロジェクト テンプレートは、`TabbedApplication`次のように。

[![](creating-tabbed-applications-images/newsolution1.png "空のプロジェクト テンプレートを選択します。")](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![](creating-tabbed-applications-images/newsolution2.png "TabbedApplication プロジェクトを名前します。")](creating-tabbed-applications-images/newsolution2.png#lightbox)



### <a name="adding-the-uitabbarcontroller"></a>UITabBarController を追加します。

次に、空のクラスを追加 を選択して**ファイル > 新しいファイル**を選択して、**全般: 空のクラス**テンプレート。 ファイルの名前を付けます`TabController`次のようにします。

[![](creating-tabbed-applications-images/02-newclass.png "TabController クラスを追加します。")](creating-tabbed-applications-images/02-newclass.png#lightbox)

`TabController`クラスの実装に含まれる、`UITabBarController`の配列を管理する`UIViewControllers`です。 ユーザーは、タブを選択したときに、`UITabBarController`は、適切なビュー コント ローラーのビューを表示するように注意します。

実装する、`UITabBarController`以下を実行する必要があります。

1.  基本クラスを設定する`TabController`に`UITabBarController`です。 
1.  作成`UIViewController`に追加するインスタンス、`TabController`です。 
1.  追加、`UIViewController`インスタンスに割り当てられた配列を`ViewControllers`のプロパティ、`TabController`です。 


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

各ことに注意して`UIViewController`設定は、インスタンス、`Title`のプロパティ、`UIViewController`です。 コント ローラーを追加するときに、 `UITabBarController`、`UITabBarController`は読み取り、`Title`各コント ローラーにし、次のように、関連付けられているタブのラベルに表示。

[![](creating-tabbed-applications-images/00-app.png "実行するサンプル アプリ")](creating-tabbed-applications-images/00-app.png#lightbox)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>RootViewController として、TabController を設定します。

タブに、コント ローラーが配置される順序に追加された順序に対応、`ViewControllers`配列。

取得する、`UITabController`としてに読み込まれる最初の画面、いただくためにするために、ウィンドウの`RootViewController`の次のコードに示すように、 `AppDelegate`:

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

今すぐ、アプリケーションを実行している場合、`UITabBarController`既定で選択されている最初のタブでの読み込みになります。 関連付けられたコント ローラーの結果ビューを選択すると、その他のタブのいずれかで表示されている、`UITabBarController,`エンドユーザーが 2 番目のタブを選択する、次のようにします。

[![](creating-tabbed-applications-images/03-secondtab.png "2 番目のタブが表示されます。")](creating-tabbed-applications-images/03-secondtab.png#lightbox)

 <a name="Modifying_TabBarItems" />


### <a name="modifying-tabbaritems"></a>TabBarItems を変更します。

これでが実行中のアプリケーションのタブで、変更してみましょう、`TabBarItem`イメージと、表示されるテキストを変更するだけでなく、タブのいずれかにバッジを追加します。

 <a name="Setting_a_System_Item" />


#### <a name="setting-a-system-item"></a>システム アイテムを設定

最初に、システム アイテムを使用して最初のタブを設定してみましょう。 コンス トラクターで、 `TabController`、コント ローラーを設定する行を削除する`Title`の`tab1`をインスタンス化し、コント ローラーの設定に次のコードに置き換える`TabBarItem`プロパティ。

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

作成するときに、`UITabBarItem`を使用して、 `UITabBarSystemItem`、タイトルとイメージによって提供される自動的に、iOS を示す次のスクリーン ショットに示すように、**お気に入り**アイコンをクリックして最初のタブ上のタイトル。

 ![](creating-tabbed-applications-images/04a-tabimage.png "星形のアイコンを持つ最初のタブ")

 <a name="Setting_the_Title_and_Image" />


#### <a name="setting-the-title-and-image"></a>タイトルとイメージの設定

システム アイテム、タイトル、およびのイメージを使用するだけでなく、`UITabBarItem`カスタム値に設定することができます。 設定するコードを変更するなど、`TabBarItem`プロパティが指定されたコント ローラーの`tab2`次のようにします。

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

上記のコードには、という名前のイメージが前提としています`second.png`for mac を Visual Studio でプロジェクトのルートが追加されました 3 つのイメージは、次に示すようにすべてのデバイスの解像度をカバーする、プロジェクトには実際に追加されました。

 [![](creating-tabbed-applications-images/tabbedimages7new.png "プロジェクトに追加のイメージ")](creating-tabbed-applications-images/tabbedimages7new.png#lightbox)

透過性は、通常の解決、60 倍高解像度の 60 と 90 x 90 iPhone 6 用に 30 × 30 png をする必要があります タブの画像解像度とします。 このコードでのみ必要がありますをという名前のファイルを読み込む`second.png`iOS は、高解像度いずれかの Retina ディスプレイを持つデバイスに自動的に読み込むとします。 詳細を読み取ることができますにこれは、[イメージを操作](~/ios/app-fundamentals/images-icons/index.md)ガイドです。 既定では、タブ バーの項目は、選択されているときに青の色の灰色をされます。

**注:**

上の図にも追加でした、**リソース**ディレクトリで、特別なディレクトリの内容は、アプリケーション バンドルのルートに自動的にコピーされます。

[![](creating-tabbed-applications-images/tabbedapplication8.png "リソースとしてイメージ")](creating-tabbed-applications-images/tabbedapplication8.png#lightbox)

さらに設定すると、`Title`プロパティに直接、 `TabBarItem`、任意の値の設定をオーバーライドは`Title`コント ローラー自体にします。

今すぐアプリケーションを実行したときに 2 番目のタブ イメージを示しています、カスタムのタイトルの下に示すように。

[![](creating-tabbed-applications-images/05-customtab.png "四角形のアイコンと 2 番目のタブ")](creating-tabbed-applications-images/05-customtab.png#lightbox)

 <a name="Setting_the_Badge_Value" />


#### <a name="setting-the-badge-value"></a>バッジ値の設定

タブには、バッジも表示できます。 たとえば、次の 3 番目のタブにバッジを設定するコード行を追加します。

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

これを実行すると、次のように、タブの左上隅で、文字列"Hi"で赤色のラベルで得られます。

[![](creating-tabbed-applications-images/06-badge.png "Hi バッジと 2 番目のタブ")](creating-tabbed-applications-images/06-badge.png#lightbox)

未読、番号の示す値を表示するバッジが使用される多くの場合、新しい項目。 バッジを削除するには、設定、`BadgeValue`を次に示すように null にします。

```csharp
tab3.TabBarItem.BadgeValue = null;
```

 <a name="Tabs_in_Non-RootViewController_Scenarios" />


## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>RootViewController 以外のシナリオでのタブ

上記の例で使用する方法を紹介しました、`UITabBarController`である場合、`RootViewController`ウィンドウのです。 使用する方法については、この例では、 `UITabBarController` 、されていない、`RootViewController`これを作成する方法を表示するストーリー ボードを使用するとします。

 <a name="Initial_Screen_Example" />


### <a name="initial-screen-example"></a>最初の画面の例

このシナリオでは、最初の画面を読み込みますがコント ローラーから、`UITabBarController`です。 同じビュー コント ローラーに読み込まれる、ユーザーを操作する画面にボタンをタップしたときに、`UITabBarController`ユーザーに提示されています。 次のスクリーン ショットは、アプリケーションのフローを示しています。

[![](creating-tabbed-applications-images/inital-screen-application.png "このスクリーン ショットは、アプリケーションのフローを示しています。")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

この例は、新しいアプリケーションを起動してみましょう。 使用して、もう一度、 **iPhone > アプリ > 空のプロジェクト (c#)**テンプレート今度は、プロジェクトの名前付け`InitialScreenDemo`です。


この例では、コント ローラーの表示を保持するために、ストーリー ボードを必要になります。 ストーリー ボードを追加します。

- プロジェクト名を右クリックし **追加 > 新しいファイル**です。

- 新しいファイル ダイアログが表示されたらに移動**iOS > iPhone ストーリー ボードを空にする**です。

この新しいストーリー ボードをとしましょう**MainStoryboard**下図のように、します。 

[![](creating-tabbed-applications-images/new-file-dialog.png "MainStoryboard ファイルをプロジェクトに追加します。")](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

いくつかの重要な手順で説明する以前ストーリー ボード ファイルに、ストーリー ボードを追加する場合に注意してください、[ストーリー ボードの概要](~/ios/user-interface/storyboards/index.md)ガイドです。 これらの数値は、次のとおりです。

 
1. ストーリー ボード名を追加、 **Main インターフェイス**のセクションで、 `Info.plist`:

    [![](creating-tabbed-applications-images/project-options.png "MainStoryboard にメインのインターフェイスを設定します。")](creating-tabbed-applications-images/project-options.png#lightbox)
1. `App Delegate`、次のコード ウィンドウ メソッドをオーバーライドします。

    ```csharp
    public override UIWindow Window {
      get;
      set;
    }
    ```

この例の 3 つのビュー コント ローラーが必要になっています。 という名前であるため、の`ViewController1`、および最初のタブで、初期ビュー コント ローラーとして使用されます。その他の 2 という`ViewController2`と`ViewController3`、どちらを使用する 2 番目と 3 番目のタブにそれぞれします。

MainStoryboard.storyboard ファイルをダブルクリックしてデザイナーを開き、デザイン サーフェイスにコント ローラーを次の 3 つの表示をドラッグします。 たい各コント ローラーのこれらの表示、上記の名前に対応する、独自のクラスには、下にある**Identity > クラス**、次のスクリーン ショットに示すように、その名前で入力します。

[![](creating-tabbed-applications-images/class-name.png "クラスを ViewController1 に設定します。")](creating-tabbed-applications-images/class-name.png#lightbox)

クラスとデザイナーのファイルが必要な visual Studio for Mac が自動的に生成、発生することはソリューション パッドでは、次のようにします。

[![](creating-tabbed-applications-images/solution-pad2.png "プロジェクト内のファイルの自動生成")](creating-tabbed-applications-images/solution-pad2.png#lightbox)

 <a name="Creating_the_UI" />


#### <a name="creating-the-ui"></a>UI を作成します。

次に、Xamarin iOS デザイナーを使用して各 ViewController のビューのシンプルなユーザー インターフェイスが作成されます。

ドラッグして、`Label`と`Button`から ViewController1 上に、**ツールボックス**右辺でします。 次は、名前と、次のコントロールのテキストを編集するのに、プロパティ パッドを使用します。

-  **ラベル**: `Text`  = **いずれか**
-  **ボタン**: `Title`  = **ユーザーがいくつかの初期操作**


ボタンの表示を制御して、`TouchUpInside`イベント、およびおを分離コード内で参照する必要があります。 みましょう識別するために、**名前**`aButton`パッドでは、プロパティ、次のスクリーン ショットに示されています。

[![](creating-tabbed-applications-images/abutton-properties.png "名前プロパティ パッドでボタンを設定します")](creating-tabbed-applications-images/abutton-properties.png#lightbox)

デザイン画面は次のスクリーン ショットのようになります。

[![](creating-tabbed-applications-images/design-surface1.png "デザイン画面はこのスクリーン ショットのようになります")](creating-tabbed-applications-images/design-surface1.png#lightbox)

もう少し詳しくを追加してみましょう。`ViewController2`と`ViewController3`では、各グループにラベルを追加すると、'2' と '3' にテキストを変更すると、それぞれします。 これを強調表示をユーザーに見ているタブ/ビュー。

#### <a name="wiring-up-the-button"></a>ボタンを配線

読み込みしようとして`ViewController1`アプリケーションを最初に起動します。 ユーザーがボタンをタップしたときにうまく ボタンを非表示にし、ロード、`UITabBarController`で、`ViewController1`最初のタブでインスタンス。

ユーザーを離したときに、 `aButton`、TouchUpInside イベントを起動させるします。 ボタンを選択し、[、**イベント] タブ**プロパティ パッドのイベント ハンドラーを宣言 – `InitialActionCompleted` : コード内を参照するようにします。 これは、次のスクリーン ショットに示します。

[![](creating-tabbed-applications-images/event-handler.png "ユーザーが、ボタンを離したときに、TouchUpInside イベントをトリガーします。")](creating-tabbed-applications-images/event-handler.png#lightbox)

ここで、イベントの発生時に、ボタンを非表示にするビュー コント ローラーを確認する必要があります`InitialActionCompleted`です。 `ViewController1`、次の部分的なメソッドを追加します。

```csharp
partial void InitialActionCompleted (UIButton sender)
    {
      aButton.Hidden = true;  
    }
```

ファイルを保存し、アプリケーションを実行します。 1 つに画面と修正に非表示ボタンおはずです。

#### <a name="adding-the-tab-bar-controller"></a>タブ バーを追加するコント ローラー

初期ビューと想定どおりに動作しているおようになりました。 次に追加する、`UITabBarController`ビュー 2 および 3 と共にします。 デザイナーで、ストーリー ボードを開いてみましょう。

**ツールボックス**、検索、**タブ バーのコント ローラー** コント ローラーとオブジェクトをデザイン画面にドラッグします。 タブ バー コント ローラーは、次のスクリーン ショットに示すように、UI のない 2 コント ローラーの表示で、既定ではそのためとします。

[![](creating-tabbed-applications-images/tabbarcontroller.png "タブ バー コント ローラーをレイアウトに追加します。")](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

下部の黒のバーを選択し、del キーを押してしてこれらの新しいビューのコント ローラーを削除します。

このストーリー ボードで、TabBarController と、ビューのコント ローラーの間の遷移を処理するのに Segues 使用できます。 最初のビューとの対話、後に、ユーザーに提示 TabBarController に読み込むことができます。 設定してこれデザイナーでします。

**Ctrl キーを押し**と**ドラッグ**TabBarController にボタンをクリックします。 マウス時に、コンテキスト メニューが表示されます。 モーダル segue を使用することができます。 
 
それぞれのタブの設定を**Ctrl キーを押し**1 ~ 3、およびリレーションシップを選択する順序でコント ローラーの表示の各 TabBarController から**タブ**以下に示すように、コンテキスト メニューから。

[![](creating-tabbed-applications-images/context-menu.png "タブのリレーションシップを選択します。")](creating-tabbed-applications-images/context-menu.png#lightbox)

ストーリー ボードは次のスクリーン ショットのようになります。

[![](creating-tabbed-applications-images/segue-layout.png "ストーリー ボードこのスクリーン ショットのようになります")](creating-tabbed-applications-images/segue-layout.png#lightbox)

タブ バー項目のいずれかをクリックすると、[プロパティ] パネルの探索は、以下に示すように、各種のオプションの数を確認できます。

[![](creating-tabbed-applications-images/properties-panel.png "プロパティ エクスプ ローラー タブのオプションを設定します。")](creating-tabbed-applications-images/properties-panel.png#lightbox)

これを使用して、バッジ、タイトルや iOS などの特定の属性を編集[識別子](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/UIKitUICatalog/TabBarItem.html)、その他

保存、今すぐアプリケーションを実行すると、ViewController1 インスタンスが、TabBarController に読み込まれるときにボタンが再びことをおあります。 みましょうこれを修正する親ビュー コント ローラーを現在のビューがあるかどうかはどうかを確認します。 場合は、ことが分かって、TabBarController 内にあることと、そのため、ボタンを非表示します。 みましょう ViewController1 クラスに次のコードを追加します。

```csharp
public override void ViewDidLoad ()
{
     if (ParentViewController != null){
       aButton.Hidden = true;
     }

}
```

アプリケーションの実行時とユーザーの最初の画面で、UITabBarController 上のボタンをタップした読み込まれると、ビューを次に示すように、最初のタブに配置する最初の画面から。

[![](creating-tabbed-applications-images/first-view.png "アプリのサンプル出力")](creating-tabbed-applications-images/first-view.png#lightbox)

<!--Save the files and run the application:

[![](creating-tabbed-applications-images/inital-screen-application.png "Save the files and run the application")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)-->

## <a name="summary"></a>まとめ

この記事の内容を使用する方法を説明する、`UITabBarController`アプリケーションでします。 おとおし、各タブにコント ローラーを読み込む方法と、このようなタイトル、イメージおよびバッジのタブでプロパティを設定する方法です。 調査、ストーリー ボードを使用して読み込む方法、`UITabBarController`されていないときに実行時に、`RootViewController`ウィンドウのです。


## <a name="related-links"></a>関連リンク

- [タブ付きアプリケーション (サンプル) を作成します。](https://developer.xamarin.com/samples/monotouch/CreatingTabbedApplications/)
- [Images.zip](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [UITabBarController クラスのリファレンス](http://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
