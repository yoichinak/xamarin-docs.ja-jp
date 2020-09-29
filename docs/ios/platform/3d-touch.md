---
title: Xamarin での3D タッチの概要
description: この記事では、iPhone 6s と iPhone 6s Plus で導入された3D タッチジェスチャの使用方法について説明します。 これらのジェスチャは、筆圧の区別、ピークとポップ、およびクイックアクションを可能にします。
ms.prod: xamarin
ms.assetid: 806D051E-3791-40F7-9776-4E4D3E56F7F3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 6e76821033276ed3bb756cecd10e801fa615fb1e
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91431843"
---
# <a name="introduction-to-3d-touch-in-xamarinios"></a>Xamarin での3D タッチの概要

_この記事では、新しい iPhone 6s と iPhone 6s に加えて、アプリで3D タッチジェスチャを使用する方法について説明します。_

[![3D タッチ対応アプリの例](3d-touch-images/info01.png)](3d-touch-images/info01.png#lightbox)

この記事では、新しい 3D Touch Api を使用して、Xamarin に負荷の高いジェスチャを追加する方法について説明します。新しい iPhone 6s と iPhone 6s Plus デバイスで実行されている iOS アプリ。

3D タッチを使用すると、iPhone アプリでは、ユーザーがデバイスの画面に接していることを示すだけでなく、ユーザーがどの程度の圧力を exerting、さまざまなプレッシャーレベルに応答するかを判断できます。

3D Touch は、アプリに次の機能を提供します。

- [筆圧の感度](#Pressure-Sensitivity) -アプリは、ユーザーが画面に接しているハードまたはライトを測定し、その情報を活用できるようになりました。
  たとえば、描画アプリを使用すると、ユーザーがどのくらいの画面にタッチしているかに基づいて、行を太くまたは細くすることができます。
- [ピークとポップ](#Peek-and-Pop) -アプリは現在のコンテキストから移動することなく、ユーザーがデータを操作できるようになりました。 画面を画面上で押すと、関心のある項目 (メッセージのプレビューなど) を見ることができます。 より複雑にすると、項目にポップできます。
- [クイックアクション](#Quick-Actions) -ユーザーがデスクトップアプリの項目を右クリックしたときにポップアップできるコンテキストメニューなどのクイックアクションを考えてみましょう。
  クイックアクションを使用して、ホーム画面のアプリアイコンから直接アプリの関数にショートカットを追加できます。
- [シミュレーターでの3D タッチのテスト](#Testing-3D-Touch-in-the-Simulator) -正しい Mac ハードウェアを使用して、iOS シミュレーターで3d タッチ対応アプリをテストすることができます。

<a name="Pressure-Sensitivity"></a>

## <a name="pressure-sensitivity"></a>筆圧の感度

前述のように、 [UITouch](xref:UIKit.UITouch) クラスの新しいプロパティを使用すると、ユーザーが iOS デバイスの画面に適用している圧力の量を測定し、ユーザーインターフェイスでこの情報を使用できます。 たとえば、筆圧の量に基づいて、ブラシのストロークを半透明にしたり不透明にしたりすることができます。

[![筆圧の量に基づいて、半透明または不透明なブラシストロークがレンダリングされます。](3d-touch-images/pressure01.png)](3d-touch-images/pressure01.png#lightbox)

3D タッチの結果として、アプリが iOS 9 (またはそれ以降) で実行されていて、iOS デバイスが3D タッチをサポートできる場合、負荷の変化によって `TouchesMoved` イベントが発生します。

たとえば、 `TouchesMoved` [uiview](xref:UIKit.UIView)のイベントを監視する場合、次のコードを使用して、ユーザーが画面に適用している現在の負荷を取得できます。

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

プロパティは、 `MaximumPossibleForce` `Force` アプリが実行されている iOS デバイスに基づいて、 [UITouch](xref:UIKit.UITouch) のプロパティの最大有効値を返します。

> [!IMPORTANT]
> `TouchesMoved`X/Y 座標が変更されていない場合でも、負荷が変化すると、イベントが発生します。 この動作が変更されたため、イベントが頻繁に呼び出されるように iOS アプリを準備 `TouchesMoved` し、X/Y 座標が最後の呼び出しと同じになるように準備する必要があり `TouchesMoved` ます。

詳細については、「Apple の [TouchCanvas: UITouch を効果的に使用する](https://developer.apple.com/library/prerelease/ios/samplecode/TouchCanvas/) 」と「効率的なサンプルアプリと [UITouch クラスのリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/)」を参照してください。

<a name="Peek-and-Pop"></a>

## <a name="peek-and-pop"></a>ピークとポップ

3D タッチは、ユーザーが現在の場所から移動しなくても、アプリ内の情報をこれまでになくすばやく操作できるようにする新しい方法を提供します。

たとえば、アプリにメッセージのテーブルが表示されている場合、ユーザーは項目をハード押すことで、オーバーレイビューでコンテンツをプレビューすることができます (Apple は *ピーク*と呼ばれます)。

[![コンテンツをピークする例](3d-touch-images/peekandpop01.png)](3d-touch-images/peekandpop01.png#lightbox)

ユーザーが [より難しい] を押すと、通常のメッセージビュー (ビューへの *ポップ*ping と呼ばれます) が入力されます。

### <a name="checking-for-3d-touch-availability"></a>3D タッチの可用性を確認しています

を使用する場合 `UIViewController` は、次のコードを使用して、アプリが実行されている iOS デバイスで3D タッチがサポートされているかどうかを確認できます。

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

このメソッドは、の前 *または後*に呼び出すことができ `ViewDidLoad()` ます。

### <a name="handling-peek-and-pop"></a>ピークとポップの処理

3D タッチを処理できる iOS デバイスでは、クラスのインスタンスを使用して、  `UIViewControllerPreviewingDelegate` **ピーク** と **Pop** 項目の詳細の表示を処理できます。 たとえば、というテーブルビューコントローラーがある場合は、 `MasterViewController` 次のコードを使用して **ピーク** と **ポップ**をサポートできます。

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

この `GetViewControllerForPreview` メソッドは、 **ピーク** 操作を実行するために使用されます。 テーブルセルとバッキングデータへのアクセスを取得し、現在の `DetailViewController` ストーリーボードからを読み込みます。 `PreferredContentSize`を (0, 0) に設定すると、既定の**ピーク**ビューサイズが要求されます。 最後に、表示しているセル以外のすべてをぼかし、 `previewingContext.SourceRect = cell.Frame` 表示する新しいビューを返します。

を `CommitViewController` 使用すると、ユーザーが少し押したときに、**ポップ**ビューの [**プレビュー** ] で作成したビューが再利用されます。

### <a name="registering-for-peek-and-pop"></a>プレビューとポップ用に登録しています

ユーザーが項目を **ピーク** および **ポップ** することを許可するビューコントローラーから、このサービスに登録する必要があります。 テーブルビューコントローラー () の上に示されている例では、 `MasterViewController` 次のコードを使用します。

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

ここでは `RegisterForPreviewingWithDelegate` 、上記で作成したのインスタンスを使用してメソッドを呼び出してい `PreviewingDelegate` ます。 3D タッチがサポートされている iOS デバイスでは、ユーザーは項目をハード押してピークすることができます。 このようにすると、その項目は標準の表示ビューに表示されます。

詳細については、 [iOS 9 ApplicationShortcuts カットサンプル](/samples/xamarin/ios-samples/ios9-viewcontrollerpreview) と Apple の [Viewcontroller のプレビュー](https://developer.apple.com/library/prerelease/ios/samplecode/ViewControllerPreviews/Introduction/Intro.html) に関する記事を参照してください。 Uiviewcontroller preview Api サンプルアプリ、 [uipreview action クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewAction_Class/)、 [Uipreview Actiongroup クラス参照](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionGroup_Class/) 、 [uipreview actionitem プロトコルリファレンス](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionItem_Protocol/)を参照してください。

<a name="Quick-Actions"></a>

## <a name="quick-actions"></a>クイック アクション

3D タッチとクイックアクションを使用すると、iOS デバイスのホーム画面アイコンから、アプリ内の関数に共通の、迅速かつ簡単にアクセスできるショートカットを追加できます。

前述のように、ユーザーがデスクトップアプリの項目を右クリックしたときにポップアップ表示されるコンテキストメニューのようなクイックアクションを考えることができます。 クイックアクションを使用して、アプリの最も一般的な機能へのショートカットを提供する必要があります。

[![クイックアクションメニューの例](3d-touch-images/quickactions01.png)](3d-touch-images/quickactions01.png#lightbox)

### <a name="defining-static-quick-actions"></a>静的クイックアクションの定義

アプリで必要な1つ以上のクイックアクションが静的であり、変更する必要がない場合は、アプリのファイルで定義でき `Info.plist` ます。 外部エディターでこのファイルを編集し、次のキーを追加します。

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

ここでは、次のキーを持つ2つの静的クイックアクション項目を定義します。

- `UIApplicationShortcutItemIconType` -クイックアクション項目によって次のいずれかの値として表示されるアイコンを定義します。
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

  ![UIApplicationShortcutIconType の画像](3d-touch-images/uiapplicationshortcuticontype.png)

- `UIApplicationShortcutItemSubtitle` -項目のサブタイトルを定義します。
- `UIApplicationShortcutItemTitle` -項目のタイトルを定義します。
- `UIApplicationShortcutItemType` -は、アプリ内の項目を識別するために使用する文字列値です。 詳細については、以下のセクションを参照してください。

> [!IMPORTANT]
> ファイルに設定されているクイックアクションのショートカット項目は `Info.plist` 、プロパティを使用してアクセスできません `Application.ShortcutItems` 。 これらは、イベントハンドラーにのみ渡され `HandleShortcutItem` ます。

### <a name="identifying-quick-action-items"></a>クイックアクション項目の識別

前述のように、アプリのクイックアクション項目を定義したときに、その項目を `Info.plist` 識別するためのキーに文字列値が割り当てられて `UIApplicationShortcutItemType` います。

これらの識別子をコード内で簡単に操作できるようにするには、というクラスを `ShortcutIdentifier` アプリのプロジェクトに追加し、次のようにします。

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

<a name="Handling-a-Quick-Action"></a>

### <a name="handling-a-quick-action"></a>クイックアクションの処理

次に、アプリのファイルを変更して、 `AppDelegate.cs` ホーム画面のアプリのアイコンからクイックアクション項目を選択するユーザーを処理する必要があります。

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

まず、 `LaunchedShortcutItem` ユーザーが最後に選択したクイックアクション項目を追跡するパブリックプロパティを定義します。 次に、メソッドをオーバーライドし、 `FinishedLaunching` が渡されたかどうか `launchOptions` と、クイックアクション項目が含まれているかどうかを確認します。 そのような場合は、クイックアクションをプロパティに格納し `LaunchedShortcutItem` ます。

次に、メソッドをオーバーライド `OnActivated` し、選択したクイック起動項目を操作するメソッドに渡し `HandleShortcutItem` ます。 現在、 **コンソール**にメッセージを書き込んでいるだけです。 実際のアプリでは、どのようなアクションが必要であったかを処理します。 アクションが実行されると、 `LaunchedShortcutItem` プロパティはクリアされます。

最後に、アプリが既に実行されている場合は、 `PerformActionForShortcutItem` クイックアクション項目を処理するためにメソッドが呼び出されます。そのため、このメソッドをオーバーライドし、ここでメソッドを呼び出す必要があり `HandleShortcutItem` ます。

### <a name="creating-dynamic-quick-action-items"></a>動的クイックアクションアイテムの作成

アプリのファイルに静的クイックアクション項目を定義するだけでなく `Info.plist` 、動的なクイックアクションを動的に作成することもできます。 2つの新しい動的クイックアクションを定義するには、 `AppDelegate.cs` ファイルをもう一度編集し、 `FinishedLaunching` 次のようにメソッドを変更します。

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

ここで、に動的に作成されたセットが既に含まれているかどうかを確認します `application` `ShortcutItems` 。そうでない場合は、新しい項目を定義して `UIMutableApplicationShortcutItem` 配列に追加する2つの新しいオブジェクトを作成し `ShortcutItems` ます。

前の「 [クイックアクションの処理](#Handling-a-Quick-Action) 」セクションで既に追加したコードは、静的なクイックアクションと同様に、これらの動的クイックアクションを処理します。

ここで説明するように、静的なクイックアクション項目と動的クイックアクション項目の両方を組み合わせて作成できることに注意する必要があります。

詳細については、 [iOS 9 ViewUIApplicationShortcutItem Preview サンプル](/samples/xamarin/ios-samples/ios9-viewcontrollerpreview) を参照してください。 Apple の [applicationshortcuts カット: Using](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/) Sample App、 [UIApplicationShortcutItem class reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/)、 [UIMutableApplicationShortcutItem class](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/) reference、 [UIApplicationShortcutIcon class reference](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/)。

<a name="Testing-3D-Touch-in-the-Simulator"></a>

## <a name="testing-3d-touch-in-the-simulator"></a>シミュレーターでの3D タッチのテスト

Xcode を有効にして、互換性のある Force Touch Mac で最新バージョンのと iOS シミュレーターを使用する場合は、シミュレーターで3D タッチ機能をテストすることができます。

この機能を有効にするには、3D タッチ (iPhone 6s 以降) をサポートするシミュレートされた iPhone ハードウェアで任意のアプリを実行します。 次に、iOS シミュレーターで [ **ハードウェア** ] メニューを選択し、[ **Use トラックパッド Force for 3d touch** ] メニュー項目を有効にします。

[![IOS シミュレーターで [ハードウェア] メニューを選択し、[Use トラックパッド Force for 3D touch] メニュー項目を有効にします。](3d-touch-images/simulator01.png)](3d-touch-images/simulator01.png#lightbox)

この機能がアクティブになっていると、Mac のトラックパッドを使用して、実際の iPhone ハードウェアの場合と同様に3D タッチを有効にすることができます。

## <a name="summary"></a>まとめ

この記事では、iPhone 6s と iPhone 6s Plus の iOS 9 で利用可能な新しい 3D Touch Api を紹介しました。 アプリへの圧力感度の追加について説明します。Peek と Pop を使用して、ナビゲーションなしで現在のコンテキストからアプリ内の情報をすばやく表示する。また、クイックアクションを使用して、アプリの最も一般的に使用される機能へのショートカットを提供します。

## <a name="related-links"></a>関連リンク

- [iOS 9 Viewコントローラープレビューのサンプル](/samples/xamarin/ios-samples/ios9-viewcontrollerpreview)
- [iOS 9 ApplicationShortcuts カットのサンプル](/samples/xamarin/ios-samples/ios9-applicationshortcuts)
- [iOS 9 (開発者向け)](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [IPhone アプリを3D タッチ用に準備する](https://developer.apple.com/ios/3d-touch/)