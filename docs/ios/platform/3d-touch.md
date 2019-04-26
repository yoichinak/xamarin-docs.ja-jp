---
title: Xamarin.iOS で 3D Touch の概要
description: この記事は、iPhone 6 s と iPhone 6 s で導入された 3D のタッチ ジェスチャを使用する方法を説明しますとします。 筆圧対応機能、ピークと pop、これらのジェスチャを有効にしてクイック アクション。
ms.prod: xamarin
ms.assetid: 806D051E-3791-40F7-9776-4E4D3E56F7F3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 6a12d157b3de7c3841f5d69d209c01fbc612f79b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61279178"
---
# <a name="introduction-to-3d-touch-in-xamarinios"></a>Xamarin.iOS で 3D Touch の概要

_この記事では、新しいを使用してでは、アプリで iPhone 6 s と iPhone 6 s Plus 3D Touch のジェスチャします。_

[![](3d-touch-images/info01.png "3D Touch の例には、アプリが有効になっています。")](3d-touch-images/info01.png#lightbox)

この記事では説明し、6 s と iPhone 6 s 新しい iPhone で実行されている Xamarin.iOS アプリに負荷の機密性の高いジェスチャを追加する新しい 3D タッチ Api の使用の概要についてさらにデバイス。

3D Touch は、iPhone アプリはだけでなく、ユーザーが exerting 筆圧を理解することができますが、ユーザーのデバイスの画面では触れることを確認し、異なる圧力レベルに対応することができますようになりました。

3D Touch は、アプリには、次の機能を提供します。

- [感度への負荷が](#Pressure-Sensitivity)- アプリがどの程度難しい測定できるようになりましたか光、ユーザーがその情報の画面を利用触れることができます。
  たとえば、描画アプリでは、太くや薄型にユーザーがどの程度難しいが画面に触れるに基づいて行を作成できます。
- [ここに表示し、ポップ](#Peek-and-Pop)-アプリが、ユーザーが、現在のコンテキストから離れることがなく、データと対話できますようになりました。 キーを押して、画面には、(メッセージのプレビュー) のように対象の項目に、ピークできます。 困難を押して、アイテムをポップアップ表示ことができます。
- [クイック アクション](#Quick-Actions)-と考えるのクイック アクションをするポップ アップできる、ユーザーがデスクトップ アプリ内の項目を右クリックすると、コンテキスト メニューのようにします。
  クイック アクションを使用することができますショートカットを追加する関数に、アプリのホーム画面でアプリのアイコンから直接。
- [シミュレーターで 3D タッチをテスト](#Testing-3D-Touch-in-the-Simulator)-正しい Mac ハードウェアでは、iOS シミュレーターで 3D タッチを有効になっているアプリをテストすることができます。

<a name="Pressure-Sensitivity" />

## <a name="pressure-sensitivity"></a>筆圧対応機能

新しいプロパティを使用して、前述のよう、 [UITouch](xref:UIKit.UITouch)クラス、ユーザーが iOS デバイスの画面に適用する圧力の量を測定し、ユーザー インターフェイスでこの情報を使用します。 たとえば、ブラシのストロークより透明または不透明ベースで早い圧力の量。

[![](3d-touch-images/pressure01.png "圧力の量に基づいて、ブラシのストロークがより透明または不透明にレンダリングされます。")](3d-touch-images/pressure01.png#lightbox)

3D Touch は、結果として、アプリが iOS 9 (またはそれ以上) で実行されていると、iOS デバイスがな 3D タッチをサポートできる場合の負荷の変更により、`TouchesMoved`イベントが発生します。

たとえば、監視するとき、`TouchesMoved`のイベントを[UIView](xref:UIKit.UIView)、次のコードを使用するには、ユーザーが画面に適用する現在の負荷を取得します。

```csharp
public override void TouchesMoved (NSSet touches, UIEvent evt)
{
    base.TouchesMoved (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        // Get the pressure
        var force = touch.Force;
        var maxForce = touch.MaximumPossibleForce;

        // Do something with the touch and the pressure
        ...
    }
}
```

`MaximumPossibleForce`プロパティの最大値を返します、`Force`のプロパティ、 [UITouch](xref:UIKit.UITouch)でアプリが実行されている iOS デバイスに基づきます。

> [!IMPORTANT]
> 負荷の変更が発生、`TouchesMoved`イベントが発生する場合でも、X Y 座標が変更されていない/。 この動作の変更のために iOS アプリを準備する必要があります、`TouchesMoved`より多くの場合と、x に呼び出されるイベントを最後のと同じである Y を調整/`TouchesMoved`呼び出し。




詳細については、Apple を参照してください[TouchCanvas:UITouch を使用して、効率的かつ効果的に](https://developer.apple.com/library/prerelease/ios/samplecode/TouchCanvas/)サンプル アプリと[UITouch クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/)します。

<a name="Peek-and-Pop" />

## <a name="peek-and-pop"></a>Peek と Pop

3D Touch には、ユーザーが、現在の場所から移動することがなく、これまでよりも高速アプリ内の情報にアクセスする新しい方法が用意されています。

など、アプリがメッセージのテーブルを表示している場合、ユーザーが押すオーバーレイのビューでは、そのコンテンツをプレビューする項目をハード (Apple として参照する、*ピーク*)。

[![](3d-touch-images/peekandpop01.png "コンテンツには、ピークの例")](3d-touch-images/peekandpop01.png#lightbox)

困難としたときに、ユーザーが押すと、通常のメッセージ ビューは入力します (これと呼ば*ポップ*-をビューに ping)。

### <a name="checking-for-3d-touch-availability"></a>3D Touch 可用性のチェック

使用する場合、`UIViewController`でアプリが実行されている iOS デバイスに 3D タッチでサポートされているかどうかに、次のコードを使用することができます。

```csharp
public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
{
    //Important: call the base function
    base.TraitCollectionDidChange(previousTraitCollection);

    //See if the new TraitCollection value includes force touch
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        //Do something with 3D touch, for instance...
        RegisterForPreviewingWithDelegate (this, View);
        ...
```

前に、このメソッドを呼び出すことができます*後または*`ViewDidLoad()`します。

### <a name="handling-peek-and-pop"></a>処理のピークと Pop

3D タッチを処理できる iOS デバイスでは、インスタンスを使用できます、`UIViewControllerPreviewingDelegate`クラスの表示を処理する**ピーク**と**ポップ**項目の詳細。 などがある場合、テーブル ビュー コント ローラーが呼び出されます`MasterViewController `をサポートする次のコードを使用してでした**ピーク**と**ポップ**:。

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace DTouch
{
    public class PreviewingDelegate : UIViewControllerPreviewingDelegate
    {
        #region Computed Properties
        public MasterViewController MasterController { get; set; }
        #endregion

        #region Constructors
        public PreviewingDelegate (MasterViewController masterController)
        {
            // Initialize
            this.MasterController = masterController;
        }

        public PreviewingDelegate (NSObjectFlag t) : base(t)
        {
        }

        public PreviewingDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        /// Present the view controller for the "Pop" action.
        public override void CommitViewController (IUIViewControllerPreviewing previewingContext, UIViewController viewControllerToCommit)
        {
            // Reuse Peek view controller for details presentation
            MasterController.ShowViewController(viewControllerToCommit,this);
        }

        /// Create a previewing view controller to be shown at "Peek".
        public override UIViewController GetViewControllerForPreview (IUIViewControllerPreviewing previewingContext, CGPoint location)
        {
            // Grab the item to preview
            var indexPath = MasterController.TableView.IndexPathForRowAtPoint (location);
            var cell = MasterController.TableView.CellAt (indexPath);
            var item = MasterController.dataSource.Objects [indexPath.Row];

            // Grab a controller and set it to the default sizes
            var detailViewController = MasterController.Storyboard.InstantiateViewController ("DetailViewController") as DetailViewController;
            detailViewController.PreferredContentSize = new CGSize (0, 0);

            // Set the data for the display
            detailViewController.SetDetailItem (item);
            detailViewController.NavigationItem.LeftBarButtonItem = MasterController.SplitViewController.DisplayModeButtonItem;
            detailViewController.NavigationItem.LeftItemsSupplementBackButton = true;

            // Set the source rect to the cell frame, so everything else is blurred.
            previewingContext.SourceRect = cell.Frame;

            return detailViewController;
        }
        #endregion
    }
}
```

`GetViewControllerForPreview`メソッドを使用して、実行、**ピーク**操作。 テーブルのセル、バックアップ データにアクセスし、ロード、`DetailViewController`現在のストーリー ボードから。 設定して、 `PreferredContentSize` (0, 0) を確認していること、既定の**ピーク**サイズを表示します。 最後に、すべてを表示するにはセルがぼかし`previewingContext.SourceRect = cell.Frame`し、表示するための新しいビューが返されます。

`CommitViewController`で作成したビューを再利用、**ピーク**の**ポップ**表示、ユーザーが難しくなります。

### <a name="registering-for-peek-and-pop"></a>Peek と Pop の登録

ユーザーに許可する必要があるビュー コント ローラーから**ピーク**と**ポップ**項目から、このサービスに登録する必要があります。 テーブル ビュー コント ローラーの上記の例 (`MasterViewController`)、次のコードを使用します。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Check to see if 3D Touch is available
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        // Register for Peek and Pop
        RegisterForPreviewingWithDelegate(new PreviewingDelegate(this), View);
    }
    ...

}
```

ここで呼び出している、`RegisterForPreviewingWithDelegate`メソッドのインスタンスを`PreviewingDelegate`上で作成しました。 3D タッチをサポートする iOS デバイスでは、ユーザーがそれには、ピークに項目をハードが押します。 場合も難しいキーを押して、項目が表示されますに標準的なビューを表示します。

詳細についてを参照してください、 [iOS 9 ApplicationShortcuts サンプル](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/)と Apple の[ViewControllerPreviews:Api のプレビューの UIViewController を使用して](https://developer.apple.com/library/prerelease/ios/samplecode/ViewControllerPreviews/Introduction/Intro.html)サンプル アプリで[UIPreviewAction クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewAction_Class/)、 [UIPreviewActionGroup クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionGroup_Class/)と[UIPreviewActionItemプロトコルのリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionItem_Protocol/)します。

<a name="Quick-Actions" />

## <a name="quick-actions"></a>クイック アクション

3D タッチとクイック アクションを使用して追加できます一般的な迅速かつ簡単には、関数へのアクセスのショートカット、アプリで、iOS デバイスのホーム画面アイコンから。

前述のようがポップされます。 アップできる、ユーザーがデスクトップ アプリ内の項目を右クリックすると、コンテキスト メニューのように、クイック アクションを考えることができます。 クイック アクションを使用すると、最も一般的な関数やアプリの機能へのショートカットを提供するのに必要があります。

[![](3d-touch-images/quickactions01.png "クイック アクション メニューの例")](3d-touch-images/quickactions01.png#lightbox)


### <a name="defining-static-quick-actions"></a>静的なクイック アクションを定義します。

アプリの定義できる場合は 1 つまたは複数のアプリで必要なクイック アクションは静的および変更する必要はありません、`Info.plist`ファイル。 外部エディターでこのファイルを編集し、次のキーを追加します。

```xml
<key>UIApplicationShortcutItems</key>
<array>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeSearch</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will search for an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Search</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.000</string>
    </dict>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeShare</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will share an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Share</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.001</string>
    </dict>
</array>
```

ここで 2 つの静的なクイック アクション項目を定義すると、次のキーでは。

- `UIApplicationShortcutItemIconType` -クイック アクション項目で、次の値の 1 つとして表示されるアイコンを定義します。
  - `UIApplicationShortcutIconTypeAdd`
  - `UIApplicationShortcutIconTypeAlarm`
  - `UIApplicationShortcutIconTypeAudio`
  - `UIApplicationShortcutIconTypeBookmark`
  - `UIApplicationShortcutIconTypeCapturePhoto`
  - `UIApplicationShortcutIconTypeCaptureVideo`
  - `UIApplicationShortcutIconTypeCloud`
  - `UIApplicationShortcutIconTypeCompose`
  - `UIApplicationShortcutIconTypeConfirmation`
  - `UIApplicationShortcutIconTypeContact`
  - `UIApplicationShortcutIconTypeDate`
  - `UIApplicationShortcutIconTypeFavorite`
  - `UIApplicationShortcutIconTypeHome`
  - `UIApplicationShortcutIconTypeInvitation`
  - `UIApplicationShortcutIconTypeLocation`
  - `UIApplicationShortcutIconTypeLove`
  - `UIApplicationShortcutIconTypeMail`
  - `UIApplicationShortcutIconTypeMarkLocation`
  - `UIApplicationShortcutIconTypeMessage`
  - `UIApplicationShortcutIconTypePause`
  - `UIApplicationShortcutIconTypePlay`
  - `UIApplicationShortcutIconTypeProhibit`
  - `UIApplicationShortcutIconTypeSearch`
  - `UIApplicationShortcutIconTypeShare`
  - `UIApplicationShortcutIconTypeShuffle`
  - `UIApplicationShortcutIconTypeTask`
  - `UIApplicationShortcutIconTypeTaskCompleted`
  - `UIApplicationShortcutIconTypeTime`
  - `UIApplicationShortcutIconTypeUpdate`

        ![](3d-touch-images/uiapplicationshortcuticontype.png "UIApplicationShortcutIconType imagery")

* `UIApplicationShortcutItemSubtitle` -項目の字幕を定義します。
* `UIApplicationShortcutItemTitle` -項目のタイトルを定義します。
* `UIApplicationShortcutItemType` -このアプリ内の項目を識別するために使用する文字列値です。 詳細については、以下のセクションを参照してください。

> [!IMPORTANT]
> 設定されている、クイック アクション ショートカット項目、`Info.plist`でファイルにアクセスできない、`Application.ShortcutItems`プロパティ。 のみにで渡される、`HandleShortcutItem`イベント ハンドラー。





### <a name="identifying-quick-action-items"></a>クイック アクション項目を識別します。

上記のとおり、定義したとき、クイック アクション項目で、アプリの`Info.plist`、文字列値が割り当てられて、`UIApplicationShortcutItemType`それらを識別するキー。

これらの識別子をコードで処理しやすくするには、クラスを追加すると呼ばれる`ShortcutIdentifier`アプリのプロジェクトし、次のようになります。

```csharp
using System;

namespace AppSearch
{
    public static class ShortcutIdentifier
    {
        public const string First = "com.company.appname.000";
        public const string Second = "com.company.appname.001";
        public const string Third = "com.company.appname.002";
        public const string Fourth = "com.company.appname.003";
    }
}
```

<a name="Handling-a-Quick-Action" />

### <a name="handling-a-quick-action"></a>クイック アクションの処理

次に、アプリを変更する必要があります`AppDelegate.cs`ホーム画面で、アプリのアイコンから、クイック アクション項目を選択すると、ユーザーを処理するファイル。

次の編集を行います。

```csharp
using System;
...

public UIApplicationShortcutItem LaunchedShortcutItem { get; set; }

public bool HandleShortcutItem(UIApplicationShortcutItem shortcutItem) {
    var handled = false;

    // Anything to process?
    if (shortcutItem == null) return false;

    // Take action based on the shortcut type
    switch (shortcutItem.Type) {
    case ShortcutIdentifier.First:
        Console.WriteLine ("First shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Second:
        Console.WriteLine ("Second shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Third:
        Console.WriteLine ("Third shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Fourth:
        Console.WriteLine ("Forth shortcut selected");
        handled = true;
        break;
    }

    // Return results
    return handled;
}

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    return shouldPerformAdditionalDelegateHandling;
}

public override void OnActivated (UIApplication application)
{
    // Handle any shortcut item being selected
    HandleShortcutItem(LaunchedShortcutItem);

    // Clear shortcut after it's been handled
    LaunchedShortcutItem = null;
}

public override void PerformActionForShortcutItem (UIApplication application, UIApplicationShortcutItem shortcutItem, UIOperationHandler completionHandler)
{
    // Perform action
    completionHandler(HandleShortcutItem(shortcutItem));
}
```

まず、パブリックを定義します`LaunchedShortcutItem`ユーザーが選択されている最後のクイック アクション項目を追跡するプロパティ。 無効にし、`FinishedLaunching`メソッドを参照してください場合`launchOptions`が渡されたとクイック アクション項目が含まれている場合。 クイック アクションの場合は、格納、`LaunchedShortcutItem`プロパティ。

次に、私たちのオーバーライド、`OnActivated`メソッドとパスのいずれかを選択する項目のクイック起動、`HandleShortcutItem`で処理するメソッド。 現在メッセージを書き込みのみ、**コンソール**します。 実際のアプリでのなあらゆるアクションが必要な処理とします。 この操作が実行された後、`LaunchedShortcutItem`プロパティがオフになっています。

最後に、アプリが既に実行されている場合、`PerformActionForShortcutItem`をオーバーライドして呼び出す必要があります、クイック アクション項目を処理するメソッドが呼び出されます、`HandleShortcutItem`メソッドも。


### <a name="creating-dynamic-quick-action-items"></a>動的なクイック アクション項目を作成します。

アプリの項目を静的なクイック アクションを定義するだけでなく`Info.plist`ファイル、動的にその場でクイック アクションを作成することができます。 2 つの新しい動的なクイック アクションを定義するには、編集、`AppDelegate.cs`ファイルを再びおよび変更、`FinishedLaunching`メソッドを次のようにします。

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    // Add dynamic shortcut items
    if (application.ShortcutItems.Length == 0) {
        var shortcut3 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Third, "Play") {
            LocalizedSubtitle = "Will play an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Play)
        };

        var shortcut4 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Fourth, "Pause") {
            LocalizedSubtitle = "Will pause an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Pause)
        };

        // Update the application providing the initial 'dynamic' shortcut items.
        application.ShortcutItems = new UIApplicationShortcutItem[]{shortcut3, shortcut4};
    }

    return shouldPerformAdditionalDelegateHandling;
}
```

かどうかを確認していますので、`application`既にのセットが含まれていますが動的に作成`ShortcutItems`開かない場合は、新しい作成 2`UIMutableApplicationShortcutItem`に追加して、新しい項目を定義するオブジェクト、`ShortcutItems`配列。

コードは、追加済みで、[処理クイック アクション](#Handling-a-Quick-Action)前のセクションは、静的なものと同様の動的これらクイック アクションを処理します。

静的および動的のクイック アクション項目の組み合わせを作成するには、(ここで行っている) よう、どちらか一方に限定されないことに注意する必要があります。

詳細については、次のようにしてくださいこの[iOS 9 ViewControllerPreview サンプル](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/)して Apple の[ApplicationShortcuts:。UIApplicationShortcutItem を使用して](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/)サンプル アプリで[UIApplicationShortcutItem クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/)、 [UIMutableApplicationShortcutItem クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/)と[UIApplicationShortcutIcon クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/)します。

<a name="Testing-3D-Touch-in-the-Simulator" />

## <a name="testing-3d-touch-in-the-simulator"></a>シミュレーターでの 3D タッチのテスト

Force Touch で互換性のある Mac で Xcode と iOS シミュレーターの最新バージョンを使用して、トラック パッドが有効にする、シミュレーターで 3D タッチ機能をテストできます。

この機能を有効にするには、3D タッチをサポートしている iPhone のシミュレートされたハードウェアですべてのアプリを実行 (iPhone 6 s 以降)。 次に、選択、**ハードウェア**iOS シミュレーターと有効にする メニュー、 **3D タッチを使用してトラック パッド Force**メニュー項目。

[![](3d-touch-images/simulator01.png "IOS シミュレーターで、[ハードウェア] メニューを選択し、3D touch メニュー項目を使用してトラック パッド強制を有効にします。")](3d-touch-images/simulator01.png#lightbox)

アクティブなこの機能をキー困難を Mac のトラック パッドすると、実際の iPhone ハードウェア上と同じように 3D タッチを有効にします。

## <a name="summary"></a>まとめ

この記事では加えられて、新しい 3D タッチ Api iOS 9 iPhone 6 s と iPhone 6 s で利用できるとします。 アプリに追加の筆圧対応機能を説明しましたPeek と Pop を使用して、ナビゲーション; せず、現在のコンテキストからのアプリ内の情報をすばやく表示するには機能を最もよく使用クイック アクションを使用して、アプリへのショートカットを提供します。



## <a name="related-links"></a>関連リンク

- [iOS 9 ViewControllerPreview サンプル](https://developer.xamarin.com/samples/monotouch/ios9/ViewControllerPreview/)
- [iOS 9 ApplicationShortcuts サンプル](https://developer.xamarin.com/samples/monotouch/ios9/ApplicationShortcuts/)
- [iOS 9 開発者向け](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [3D Touch 用 iPhone アプリを準備します。](https://developer.apple.com/ios/3d-touch/)
