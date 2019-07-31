---
title: IOS 10 での検索とホーム画面のウィジェットの機能強化
description: このドキュメントでは、Apple が iOS 10 のウィジェットに加えた拡張機能について説明します。これには、検索とホーム画面のウィジェットの更新プログラムが含まれます。
ms.prod: xamarin
ms.assetid: D66FD9E1-9E23-4BB6-825C-ED19B8F72A81
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: f2ac37676dbdfc96c853c9bc679e79c2aae1adb1
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656235"
---
# <a name="search-and-home-screen-widget-enhancements-in-ios-10"></a>IOS 10 での検索とホーム画面のウィジェットの機能強化

_この記事では、iOS 10 のウィジェットシステムに加えられた Apple の機能強化について説明します。_

Apple では、ウィジェットシステムにいくつかの機能強化が導入され、新しい iOS 10 ロック画面に存在する背景でウィジェットが見栄えよく見えるようになりました。 さらに、ウィジェットには[NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode)プロパティが含まれるようになりました。これにより、開発者は、使用可能なコンテンツの量を説明し、ユーザーがコンテンツを展開したり折りたたんだりできるようになります。

ウィジェット (今日の拡張機能とも呼ばれます) は、特殊な種類の iOS 拡張機能であり、ごく一部の有用な情報を表示したり、アプリ固有の機能を適切なタイミングで公開したりします。 たとえば、ニュースアプリには、トップヘッドラインを表示するウィジェットがあり、予定表アプリには2つの異なるウィジェットが用意されています。1つは今日のイベントを表示し、もう1つは、今後のイベントを表示します。

ウィジェットは高度なカスタマイズが可能であり、テキスト、画像、ボタンなどの UI 要素を含めることができます。さらに、開発者はウィジェットのレイアウトをカスタマイズすることもできます。

[![](widgets-images/widgets01.png "ウィジェットの例")](widgets-images/widgets01.png#lightbox)

ユーザーはアプリのウィジェットを表示して操作できる主な場所が2つあります。

- **検索画面**: ユーザーは、検索画面で最も役に立つウィジェットを追加できます。 検索画面にアクセスするには、ホーム画面とロック画面の両方を右にスワイプします。
- **ホーム画面**-ホーム画面から、ユーザーは3d タッチを使用して、アプリのアイコンに圧力を適用してクイックアクションの一覧を開くことができます。 アプリのウィジェットがクイックアクションリストの上に表示されます。 詳細については、「 [3D タッチ](~/ios/platform/3d-touch.md)ドキュメントの概要」を参照してください。

## <a name="widgets-developer-suggestions"></a>ウィジェットの開発者向け候補

開発者は、ユーザーが検索画面に追加するウィジェットを常に試してデザインすることをお勧めします。 そのため、Apple には次のような提案があります。

- ステータスの更新に関する簡単な情報を提供したり、簡単なタスクを迅速に実行したりできる、優れた機能を備えたユーザーのウィジェット**を作成**できます。 これにより、適切な量の情報と対話機能が不可欠になります。 可能な限り、ユーザーは1回のタップで特定のタスクを実行できます。 また、ウィジェットではパンまたはスクロールがサポートされていないため、ウィジェットのデザインでこの点を考慮する必要があります。
- **コンテンツをすばやく表示**できるように設計されているため、ウィジェットが表示されると、ユーザーはコンテンツが読み込まれるまで待機する必要がありません。 ウィジェットでは、コンテンツをローカルにキャッシュする必要があります。これにより、新しいコンテンツがバックグラウンドで読み込まれている間に、最新のコンテンツを表示できます。
- **適切な埋め込みと余白を指定**する-ウィジェットの外観が損なわれないようにするため、ウィジェットのビューの端にコンテンツを拡張しないようにします。 端と内容の間には、常に複数のピクセル幅の余白が必要です。 また、配置ガイドとして、ウィジェットの上部に表示されるアプリのアイコンも使用することをお勧めします。 ウィジェットにグリッドレイアウトが表示されている場合は、グリッド内の項目間に適切な埋め込みがあることを確認し、項目数を最大4つに制限します。
- **適応性のあるレイアウトを使用する**-ウィジェットの幅は、実行されているデバイスとデバイスの向きによって異なります。 ウィジェットの高さは、折りたたまれた状態 (既定値) または展開されている (すべてのウィジェットではサポートされていない) 状態で表示されているかどうかによっても異なります。 折りたたまれたウィジェットの高さは、約2と半分の標準的な iOS テーブル行になります。 開発者は、展開されたウィジェットのサイズを要求できますが、理想的には画面の高さよりも小さくする必要があります。 折りたたまれた状態では、必須のスタンドアロン情報のみがウィジェットに表示されます。 展開すると、ウィジェットには、折りたたまれた状態で表示されるプライマリコンテンツを強化する補足情報が表示されます。 クイックアクションリストに表示されるウィジェットは、折りたたまれた状態のみになります。
- **ウィジェットの背景**ウィジェットをカスタマイズしないで、システムによって提供される背景がぼやけた光が表示されます。 これは、ウィジェット間の一貫性を促進し、コンテンツの読みやすさを向上させるために行われます。 ユーザーのロックおよびホーム画面の壁紙と競合する可能性があるため、ウィジェットの背景としてイメージを使用しないでください。
- **黒または濃い灰色のシステムフォントを使用**する-ウィジェットでテキストを表示する場合は、システムフォントが最適です。 フォントは、明るいウィジェットの背景がぼやけて薄い色になるように、黒または暗い灰色の色にする必要があります。
- **アプリへのアクセスを提供**します。適切なウィジェットが常にアプリとは別に動作する必要がありますが、より深い機能が必要な場合は、ウィジェットがアプリを起動して特定の情報を表示または編集できるようにする必要があります。 [アプリを開く] ボタンを含めないでください。ユーザーがコンテンツ自体をタップするだけで、サードパーティのアプリを開くことはできません。
- **明確で簡潔なウィジェット名を選択し**ます。アプリのアイコンとウィジェットの名前は、常にウィジェットのコンテンツの上に表示されます。 Apple では、主なウィジェットにアプリの名前を使用することと、それが提供するその他の明確で簡潔な名前を使用することをお勧めします。 カスタムウィジェットのタイトルを指定するときは、アプリの名前をプレフィックスとして付ける必要があります (地図の近く、地図レストランなど)。
- **認証時に値が追加**されたことを通知する-追加の機能または情報がユーザーの認証およびサインオン時にのみ使用可能な場合は、これをユーザーに提示します。 たとえば、乗り物共有アプリでは、"book to 乗り物" と表示される場合があります。
- **クイックアクションリストウィジェットを選択**する-アプリに複数のウィジェットが用意されている場合、開発者は、3d タッチを使用してアプリのアイコンに圧力を適用することによって、ユーザーがクイックアクションリストを表示したときに表示するものを選択する必要があります。

ウィジェットの使用方法の詳細については、「[拡張機能の概要](~/ios/platform/extensions.md)」、「 [3d タッチドキュメントの概要](~/ios/platform/3d-touch.md)」、および「Apple の[アプリ拡張機能のプログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)」を参照してください。

## <a name="working-with-vibrancy"></a>Vibrancy の操作

Vibrancy を使うと、ウィジェットの光源に表示されるときにウィジェットのテキストが読みやすくなり、(システムによって提供される) ぼやけた背景が保証されます。 IOS 10 より前の開発者は、ウィジェットの vibrancy に[NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect)を使用します。 例えば:

```csharp
// DEPRECATED: Get Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreateForNotificationCenter ();
```

これは iOS 10 で非推奨とされており、 [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect)または[WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect)のいずれかで置き換える必要があります。 例えば:

```csharp
// Get Primary Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreatePrimaryVibrancyEffectForNotificationCenter ();

// Get Secondary Widget Vibrancy Effect
var vibrancy2 = UIVibrancyEffect.CreateSecondaryVibrancyEffectForNotificationCenter ();
```

## <a name="working-with-collapsed-and-expanded-widgets"></a>折りたたまれた、展開されたウィジェットの操作

IOS 10 の新機能であるウィジェットには、 [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode)プロパティが含まれるようになりました。これにより、開発者は、利用可能なコンテンツの量を説明し、ユーザーがコンテンツを展開したり折りたたんだりできるようになります。

最初にウィジェットが表示されると、折りたたまれた状態になります。 折りたたまれたウィジェットの高さは、約2と半分の標準的な iOS テーブル行になります。 開発者は、展開されたウィジェットのサイズを要求できますが、理想的には画面の高さよりも小さくする必要があります。 

折りたたまれた状態では、必須のスタンドアロン情報のみがウィジェットに表示されます。 展開すると、ウィジェットには、折りたたまれた状態で表示されるプライマリコンテンツを強化する補足情報が表示されます。 たとえば、気象アプリには、折りたたまれたときの現在の気象条件が表示され、展開されると、時間単位の予測が追加されます。

クイックアクションリストに表示されるウィジェットは、折りたたまれた状態のみになります。 アプリに複数のウィジェットが用意されている場合、開発者は、3D タッチを使用してアプリのアイコンに圧力を適用して、ユーザーがクイックアクションリストを表示したときに表示されるものを選択する必要があります。

次の例は、折りたたまれた状態と展開された状態を処理する単純な Today 拡張機能 (ウィジェット) です。

```csharp
using System;
using NotificationCenter;
using Foundation;
using UIKit;
using CoreGraphics;

namespace MonkeyAbout
{
    public partial class TodayViewController : UIViewController, INCWidgetProviding
    {
        protected TodayViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Tell widget it can be expanded
            ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);

            // Get the maximum size
            var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
        }

        [Export ("widgetPerformUpdateWithCompletionHandler:")]
        public void WidgetPerformUpdate (Action<NCUpdateResult> completionHandler)
        {
            // Take action based on the display mode
            switch (ExtensionContext.GetWidgetActiveDisplayMode()) {
            case NCWidgetDisplayMode.Compact:
                Content.Text = "Let's Monkey About!";
                break;
            case NCWidgetDisplayMode.Expanded:
                Content.Text = "Gorilla!!!!";
                break;
            }

            // Report results
            // If an error is encoutered, use NCUpdateResultFailed
            // If there's no update required, use NCUpdateResultNoData
            // If there's an update, use NCUpdateResultNewData
            completionHandler (NCUpdateResult.NewData);
        }

        [Export ("widgetActiveDisplayModeDidChange:withMaximumSize:")]
        public void WidgetActiveDisplayModeDidChange (NCWidgetDisplayMode activeDisplayMode, CGSize maxSize)
        {
            // Take action based on the display mode
            switch (activeDisplayMode) {
            case NCWidgetDisplayMode.Compact:
                PreferredContentSize = maxSize;
                Content.Text = "Let's Monkey About!";
                break;
            case NCWidgetDisplayMode.Expanded:
                PreferredContentSize = new CGSize (0, 200);
                Content.Text = "Gorilla!!!!";
                break;
            }
        }

    }
}
```

ウィジェットの表示モード固有のコードを詳しく見てみましょう。 このウィジェットが拡張された状態をサポートしていることをシステムに通知するには、次のものを使用します。

```csharp
// Tell widget it can be expanded
ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);
```

ウィジェットの現在の表示モードを取得するには、次のように使用します。

```csharp
ExtensionContext.GetWidgetActiveDisplayMode()
```

折りたたまれた状態または展開された状態の最大サイズを取得するには、次のように使用します。

```csharp
// Get the maximum size
var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
```

また、状態 (表示モード) の変更を処理するために、次のものを使用します。

```csharp
[Export ("widgetActiveDisplayModeDidChange:withMaximumSize:")]
public void WidgetActiveDisplayModeDidChange (NCWidgetDisplayMode activeDisplayMode, CGSize maxSize)
{
    // Take action based on the display mode
    switch (activeDisplayMode) {
    case NCWidgetDisplayMode.Compact:
        PreferredContentSize = maxSize;
        Content.Text = "Let's Monkey About!";
        break;
    case NCWidgetDisplayMode.Expanded:
        PreferredContentSize = new CGSize (0, 200);
        Content.Text = "Gorilla!!!!";
        break;
    }
}
```

状態 (折りたたまれているか展開されている) ごとに要求されたサイズを設定するだけでなく、表示されているコンテンツを新しいサイズに合わせて更新することもできます。

## <a name="summary"></a>Summary

この記事では、iOS 10 のウィジェットシステムに加えられた Apple の機能強化について説明し、Xamarin に実装する方法を示しました。



## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [拡張機能の概要](~/ios/platform/extensions.md)
- [3D タッチの概要](~/ios/platform/3d-touch.md)
- [アプリ拡張機能のプログラミングガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)
