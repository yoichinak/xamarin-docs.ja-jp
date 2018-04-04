---
title: 3 D Touch の概要
description: この記事で説明、new を使用して、アプリで iPhone 6 s と iPhone 6 s Plus 3D タッチ ジェスチャします。
ms.prod: xamarin
ms.assetid: 806D051E-3791-40F7-9776-4E4D3E56F7F3
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: a0d89315b82f4931538cdabe64aade7986b2a42e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-3d-touch"></a>3 D Touch の概要

_この記事で説明、new を使用して、アプリで iPhone 6 s と iPhone 6 s Plus 3D タッチ ジェスチャします。_

[![](3d-touch-images/info01.png "3 D Touch の例には、アプリが有効になっています。")](3d-touch-images/info01.png#lightbox)

この記事とで実行されている新しい iPhone 6 s および iPhone 6 s Xamarin.iOS アプリに不足機密性の高いジェスチャを追加する新しい 3 D Touch Api の使用の概要のデバイスとします。

3D のタッチで iPhone アプリは今すぐをだけでなく、ユーザーが exerting 量不足を検出することができますが、ユーザーはデバイスの画面に触れることを確認し、別の圧迫度に応答できます。

3 D Touch は、アプリには、次の機能を提供します。

- [小文字の区別を圧迫](#Pressure-Sensitivity)- アプリがどの程度厳密測定できるようになりましたかライト、ユーザーはその情報の画面と take 利点に触れることです。
  たとえば、ペイント アプリは太くまたは薄型画面をユーザーがどの程度厳密に触れることに基づいて線になります。
- [ここに表示およびポップ](#Peek-and-Pop)-アプリが、現在のコンテキスト外に移動することがなく、データとやり取りするようになりました。 キーを押して、画面には、(メッセージのプレビュー) と同様の関心のある項目をピークすることができます。 困難を押して、アイテムに表示されることができます。
- [クイック アクション](#Quick-Actions)-と考えるのクイック操作をするポップ アップできる、ユーザーがデスクトップ アプリ内の項目を右クリックすると、コンテキスト メニューのようにします。
  クイック操作を使用して、するショートカットを追加できます関数アプリのホーム画面で、アプリ アイコンから直接です。
- [シミュレーターで、3 D Touch をテスト](#Testing-3D-Touch-in-the-Simulator)-正しい Mac のハードウェアでは、iOS シミュレーターで 3D のタッチを有効になっているアプリをテストできます。

<a name="Pressure-Sensitivity" />

## <a name="pressure-sensitivity"></a>筆圧対応機能

新しいプロパティを使用して前に、述べたよう、 [UITouch](https://developer.xamarin.com/api/type/UIKit.UITouch/)クラス、ユーザーが iOS デバイスの画面に適用する負荷の量を測定して、ユーザー インターフェイスでこの情報を使用します。 たとえば、ブラシ ストロークより透明または不透明なベースで早い負荷の量。

[![](3d-touch-images/pressure01.png "レンダリングより透明または不透明ブラシ ストロークは、負荷の量に基づく")](3d-touch-images/pressure01.png#lightbox)

3 D Touch は、結果として、アプリが iOS 9 (またはそれ以上) で実行されると、iOS デバイスがサポートの 3 D Touch をできるプレッシャ内の変更により、`TouchesMoved`イベントが発生します。

たとえば、監視するときに、`TouchesMoved`のイベント、 [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/)ユーザーが画面に適用する現在の負荷を取得する次のコードを使用することができます。

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

`MaximumPossibleForce`の最大値を返します、`Force`のプロパティ、 [UITouch](https://developer.xamarin.com/api/type/UIKit.UITouch/)でアプリが実行されている iOS デバイスに基づいたです。

> [!IMPORTANT]
> プレッシャ内で変更が発生、`TouchesMoved`イベントが発生する場合でも、X Y 座標値が変更されていない/です。 この動作の変更のために iOS アプリを準備する必要があります、`TouchesMoved`より多くの場合、X に呼び出されるイベント Y 座標、最後と同じであるを/`TouchesMoved`呼び出します。




詳細については、Apple を参照してください[TouchCanvas: UITouch を使用して、効率的かつ効果的に](https://developer.apple.com/library/prerelease/ios/samplecode/TouchCanvas/)サンプル アプリおよび[UITouch クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/)です。

<a name="Peek-and-Pop" />

## <a name="peek-and-pop"></a>ピークおよびポップ

3 D Touch には、現在の場所から移動しなくても、これまでよりも高速のアプリ内の情報と対話するユーザーの新しい方法が用意されています。

たとえば、アプリは、メッセージの表を表示するが、ユーザー キーを押しますアイテムを重ねて表示には、その内容のプレビューでハード (Apple を参照すると、*ピーク*)。

[![](3d-touch-images/peekandpop01.png "コンテンツにピークの例")](3d-touch-images/peekandpop01.png#lightbox)

通常のメッセージ ビューを入力することが困難としたときに、ユーザーが押すと、(これと呼ば*Pop*-ビューに ping)。

### <a name="checking-for-3d-touch-availability"></a>3 D Touch 可用性のチェック

使用する場合、 [UIViewController]()でアプリが実行されている iOS デバイスが 3 D Touch をサポートしているかを表示する次のコードを使用することができます。

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

このメソッドは、前に呼び出すことができます*後または*`ViewDidLoad()`です。 

### <a name="handling-peek-and-pop"></a>処理のピークおよびポップ

インスタンスを使用して、3 D Touch を処理できる iOS デバイスで、`UIViewControllerPreviewingDelegate`の表示を処理するクラス**ピーク**と**Pop**項目の詳細。 などがある発生しました場合テーブル ビュー コント ローラーと呼ばれる`MasterViewController `サポートするために次のコードを使用でした**ピーク**と**Pop**:。

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

`GetViewControllerForPreview`メソッドを使用して、実行、**ピーク**操作します。 テーブルのセルとバックアップ データにアクセスして、ロード、`DetailViewController`現在のストーリー ボードからです。 設定して、`PreferredContentSize`を (0, 0) に質問する、既定の**ピーク**サイズを表示します。 最後に、すべてのセルに示されていますが、ぼかしお`previewingContext.SourceRect = cell.Frame`し、表示するための新しいビューを返します。

`CommitViewController`で作成したビューを再利用、**ピーク**の**Pop**を押すことが困難を表示します。

### <a name="registering-for-peek-and-pop"></a>ピークおよびポップの登録

ユーザーに許可するビュー、コント ローラーから**ピーク**と**Pop**から項目をこのサービスに登録する必要があります。 テーブル ビュー コント ローラーの上記の例 (`MasterViewController`)、次のコードを使用します。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Check to see if 3D Touch is available
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        // Regiser for Peek and Pop
        RegisterForPreviewingWithDelegate(new PreviewingDelegate(this), View);
    }
    ...

}
```

ここで呼び出しには、`RegisterForPreviewingWithDelegate`メソッドのインスタンスを`PreviewingDelegate`上で作成しました。 3 D Touch をサポートする iOS デバイスでキーを押すことでアイテムのピークをハードです。 標準にアイテムが表示される場合も難しいキーを押して、ビューを表示します。

詳細についてを参照してください、 [iOS 9 ApplicationShortcuts サンプル](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/)と Apple の[ViewControllerPreviews: Api のプレビュー UIViewController を使用して](https://developer.apple.com/library/prerelease/ios/samplecode/ViewControllerPreviews/Introduction/Intro.html)サンプル アプリ[UIPreviewAction クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewAction_Class/)、 [UIPreviewActionGroup クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionGroup_Class/)と[UIPreviewActionItem Protocol Reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionItem_Protocol/)です。

<a name="Quick-Actions" />

## <a name="quick-actions"></a>クイック アクション

3D のタッチとクイック操作を使用して、追加できます共通、迅速かつ簡単な関数へのアクセスのショートカットをアプリで、ホーム画面アイコンから iOS デバイスで。

前述のようをするポップ アップできる、ユーザーがデスクトップ アプリ内の項目を右クリックすると、コンテキスト メニューのように、クイック操作の考えることができます。 クイック アクションを使用すると、最も一般的な関数や、アプリの機能へのショートカットを提供するのに必要があります。

[![](3d-touch-images/quickactions01.png "クイック アクション メニューの例")](3d-touch-images/quickactions01.png#lightbox)


### <a name="defining-static-quick-actions"></a>静的なクイック アクションを定義します。

アプリので定義できる場合は 1 つ以上のアプリで必要なクイック アクションは静的、変更する必要はありません`Info.plist`ファイル。 外部エディターでこのファイルを編集し、次のキーを追加します。

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

ここで 2 つの静的なクイック アクション項目の定義に次のキーおは。

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

* `UIApplicationShortcutItemSubtitle` 項目の字幕を定義します。
* `UIApplicationShortcutItemTitle` 項目のタイトルを定義します。
* `UIApplicationShortcutItemType` は、アプリケーション内で項目の識別に使用する文字列値です。 詳細については、以下のセクションを参照してください。

> [!IMPORTANT]
> 設定されている、クイック アクション ショートカット項目、`Info.plist`でファイルにアクセスできない、`Application.ShortcutItems`プロパティです。 内に渡されるだけ、`HandleShortcutItem`イベント ハンドラー。 





### <a name="identifying-quick-action-items"></a>クイック アクション項目を識別します。

上記のとおり、する定義されている場合、クイック アクション項目で、アプリの`Info.plist`、文字列値が割り当てられて、`UIApplicationShortcutItemType`識別するためのキー。

これらの識別子をコードで作業をしやすくすると呼ばれるクラスを追加`ShortcutIdentifier`をアプリのプロジェクトし、次のように表示します。

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

アプリの変更を加える必要がある次に、`AppDelegate.cs`クイック アクション項目のホーム画面で、アプリのアイコンをクリックするユーザーを処理するファイル。

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

最初に、パブリック定義`LaunchedShortcutItem`ユーザーが選択されているクイック アクションの最後の項目を追跡するプロパティです。 次に、オーバーライド、`FinishedLaunching`メソッドを参照してください場合`launchOptions`が渡されましたクイック アクション項目が含まれている場合。 クイック操作の場合は、格納、`LaunchedShortcutItem`プロパティです。

次に、オーバーライド、`OnActivated`メソッドとクイック起動項目を選択したパス、`HandleShortcutItem`に対応するメソッド。 現在メッセージを書き込みのみ、**コンソール**です。 実際のアプリでは、いままでアクションが必要なを処理します。 アクションが実行された後、`LaunchedShortcutItem`プロパティをクリアします。

最後に、アプリが既に実行されている場合、`PerformActionForShortcutItem`をこのメソッドをオーバーライドし、呼び出す必要がありますので、クイック アクション項目を処理するメソッドが呼び出されます、`HandleShortcutItem`メソッドもします。


### <a name="creating-dynamic-quick-action-items"></a>動的なクイック アクション項目を作成します。

静的なクイック アクションを定義するだけでなく、アプリの 項目`Info.plist`ファイル、ダイナミックの場でクイック操作を作成することができます。 2 つの新しい動的なクイック アクションを定義するのには、編集、`AppDelegate.cs`ファイルを再び、および変更、`FinishedLaunching`など、次のメソッドを検索します。

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

これで、かどうかをチェック、`application`既にのセットを含む動的に作成された`ShortcutItems`されていない場合を作成した 2 つの新しい`UIMutableApplicationShortcutItem`に追加して、新しい項目を定義するオブジェクト、`ShortcutItems`配列。

コードは既に追加で、[クイック アクションを処理](#Handling-a-Quick-Action)前のセクションでは、静的なものと同じようにこれらの動的のクイック アクションを処理します。

静的および動的のクイック アクション項目の組み合わせを作成するには、(ここで行っている) と、どちらか一方に限定されないことに注意してください。

詳細については、次のようにしてください、 [iOS 9 ViewControllerPreview サンプル](https://developer.xamarin.com/samples/monotouch/iOS9/ViewControllerPreview/)と Apple の[ApplicationShortcuts: を使用して UIApplicationShortcutItem](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/)サンプル アプリ[。UIApplicationShortcutItem クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/)、 [UIMutableApplicationShortcutItem クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/)と[UIApplicationShortcutIcon クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/)です。

<a name="Testing-3D-Touch-in-the-Simulator" />

## <a name="testing-3d-touch-in-the-simulator"></a>シミュレーターでのテストの 3 D Touch

Force タッチと互換性のある Mac で Xcode および iOS Simulator の最新バージョンを使用するには、トラック パッドが有効に、する場合は、シミュレーターで 3D タッチ機能をテストすることができます。

この機能を有効にするには、3 D Touch をサポートする iPhone のシミュレートされたハードウェアですべてのアプリを実行 (iPhone 6 s およびそれ以降)。 次に、選択、**ハードウェア**、ios シミュレーターと有効化 メニュー、 **3D タッチのトラック パッド強制**メニュー項目。

[![](3d-touch-images/simulator01.png "IOS シミュレーターで、[ハードウェア] メニューを選択し、3 D touch メニュー項目を使用してトラック パッド強制を有効にします。")](3d-touch-images/simulator01.png#lightbox)

この機能を使用するアクティブなをキーが困難になりますを Mac のトラック パッドを実際の iPhone ハードウェア上と同様、3 D Touch を有効にします。

## <a name="summary"></a>まとめ

この記事の内容が新しい 3 D Touch Api iOS 9 iPhone 6 s および iPhone 6 s で利用できるを導入されたとします。 アプリへの追加の筆圧対応機能を説明しましたピークおよびポップを使用してすばやくナビゲーション; せず、現在のコンテキストからのアプリ内の情報を表示するには機能を最もよく使用クイック操作を使用したアプリへのショートカットを提供します。



## <a name="related-links"></a>関連リンク

- [iOS 9 ViewControllerPreview サンプル](https://developer.xamarin.com/samples/monotouch/ios9/ViewControllerPreview/)
- [iOS 9 ApplicationShortcuts サンプル](https://developer.xamarin.com/samples/monotouch/ios9/ApplicationShortcuts/)
- [開発者向けの iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [3D タッチの iPhone アプリを準備します。](https://developer.apple.com/ios/3d-touch/)
