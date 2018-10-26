---
title: 検索し、iOS 10 のホーム画面ウィジェットの強化
description: このドキュメントでは、Apple は iOS 10 では、検索とホーム画面ウィジェットの更新などのウィジェットに加えられた機能強化について説明します。
ms.prod: xamarin
ms.assetid: D66FD9E1-9E23-4BB6-825C-ED19B8F72A81
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: f693b480fff141c177ed135ced60afd65abd77de
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50102792"
---
# <a name="search-and-home-screen-widget-enhancements-in-ios-10"></a>検索し、iOS 10 のホーム画面ウィジェットの強化

_この記事では、Apple は iOS 10 でウィジェットのシステムに加えられた機能強化について説明します。_

Apple には、ウィジェット システムをウィジェットに美しく表示で新しい iOS 10 個のロック画面に存在する任意の背景にいくつかの機能強化が導入されています。 さらに、ウィジェットが含まれています、 [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode)プロパティにより、開発者は、コンテンツの量は、使用可能な説明を展開したり、コンテンツを折りたたんだりできます。

ウィジェット (とも呼ばれる現在の拡張機能) は、少量の有用な情報を表示するか、適切なタイミングでアプリ固有の機能を公開する iOS 拡張機能の特別な種類です。 たとえば、ニュース アプリがトップ ヘッドラインを表示するウィジェットと予定表アプリには、2 つの異なるウィジェットが用意されており: 今日のイベントと今後のイベントを表示する 1 つを表示する 1 つ。

ウィジェットは、高度にカスタマイズ可能な UI 要素 (テキスト、イメージ、ボタンなど) を含めることができます。さらに、開発者は、ウィジェットのレイアウトをさらにカスタマイズすることができます。

[![](widgets-images/widgets01.png "例のウィジェット")](widgets-images/widgets01.png#lightbox)

ユーザーを表示して、アプリのウィジェットと対話する 2 つの主な場所。

- **検索画面**-ユーザーが検索画面に最も役に立つ、ウィジェットを追加できます。 検索画面は、ホームおよび ロック画面で右方向のスワイプ操作によってアクセスされます。
- **ホーム画面**-から、ホーム画面で、ユーザーを使用して 3D Touch アプリのアイコンに負荷を適用することで、クイック アクションの一覧を開きます。 アプリのウィジェットは、クイック アクションの一覧の上に表示されます。 参照してください、 [3D Touch 概要](~/ios/platform/3d-touch.md)詳細についてはドキュメントです。

## <a name="widgets-developer-suggestions"></a>ウィジェットの開発者向けの推奨事項

理想的には、開発者がお試しください常に、し、ユーザーは各自の検索画面を追加するウィジェットを設計する必要があります。 そのために、Apple は次の推奨事項があります。

- **優れた、Glanceable エクスペリエンスを作成**-ユーザーの状態の更新プログラムの簡単な glanceable の情報を提供したり、単純なタスクをすばやく実行できるようにするウィジェットをします。 これにより、適切な量の情報と、必要な対話機能を提供します。 可能であれば、1 回のタップ、特定のタスクを実行するユーザーを許可します。 さらに、ウィジェットで、パン、またはスクロールをサポートされていないため、ウィジェットのデザインで考慮この必要があります。
- **コンテンツをすばやく表示**-ユーザーはウィジェットが表示されたらをロードするコンテンツを待機する必要があるありませんように glanceable、するウィジェットが設計されています。 ウィジェットは、最近のコンテンツを表示するには、バック グラウンドで最新のコンテンツの読み込み中にローカルにコンテンツをキャッシュする必要があります。
- **適切な埋め込みと余白の指定**-ウィジェットは、いっぱいになります、ウィジェットのビューの端にコンテンツを拡張しないようにする必要があることはありません。 ピクセル幅端とコンテンツの余白をいくつか必ず必要があります。 Apple では、アプリのアイコンをガイドとして、ウィジェットの上部に表示を使用してもお勧めします。 ウィジェットは、グリッド レイアウトを提示する場合はグリッド内の項目の間の余白が正しいことを確認およびに最大 4 つの項目の数を制限してください。
- **適応性のあるレイアウトを使用して、** -A ウィジェットの幅がで実行されているデバイスとデバイスの向きに基づいて異なります。 折りたたまれた (既定値) または拡張された (すべてのウィジェットではサポートされていません) の状態で表示されている場合にも、ウィジェットの高さがに基づいてによっても異なります。 折りたたまれたウィジェットは、約 2 ~ 2.5 標準 iOS テーブル行の高さ。 開発者は、展開のウィジェットのサイズを要求することができますが、画面の高さよりも小さいする必要がありますが理想的です。 折りたたまれた状態で、ウィジェットは、唯一の必須のスタンドアロンの情報を表示する必要があります。 ときに展開される、ウィジェット、折りたたまれた状態に示すように、主要なコンテンツを強化するための補足情報が表示されます。 クイック アクションの一覧に表示されるウィジェットは、折りたたまれた状態でのみになります。
- **ウィジェットの背景のカスタマイズをしない**-ウィジェットは、システムによって提供される、ライト、ぼかしのバック グラウンドに表示されます。 これは、ウィジェット間の一貫性を昇格させ、コンテンツの読みやすさを向上させるためです。 ユーザーのロックとホーム画面の壁紙との調和ことでしたので、ウィジェットの背景としてイメージを使用しないでください。
- **黒または濃い灰色で、システム フォントを使用して、** - ウィジェット、最適のシステム フォントでテキストを表示するときにします。 フォントは、ライト、ぼかしウィジェットの背景で目立つようにする黒色または濃グレー色でなければなりません。
- **アプリへのアクセス時に適切な提供**-ウィジェットがそのアプリから個別に動作する必要があります常に、ただし、詳細な機能が必要な場合、ウィジェットする必要がありますできる表示または特定の情報を編集するアプリを起動します。 コンテンツ自体をタップするユーザーを許可する単に「アプリを開く」のボタンの場合を含めることはありませんし、サード パーティのアプリを開くことはありません。
- **明確で簡潔なウィジェットの名前を選択します。** -アプリのアイコンとウィジェットの名前が、ウィジェットのコンテンツを常に表示します。 Apple では、そのプライマリ ウィジェット用のアプリの名前と、他の明確で正確な名前を使用してお勧めします。 カスタム ウィジェットのタイトルを指定するときに、(マップの近くにある、マップのレストランなど) など、アプリの名前を付ける必要があります。
- **認証が値を追加するときに通知**- は、ユーザーが認証のみで使用可能な追加の機能や情報をユーザーにこれを示し、署名された場合。 たとえば、共有アプリ乗り物は、「書籍乗り物にサインインする」と答えてください。
- **クイック アクションの一覧のウィジェットを選択します。** -アプリは、複数のウィジェットを提供する場合、開発者は、1 つを作成するときに 3D タッチを使用して、アプリのアイコンに負荷を適用することで、クイック アクションの一覧を、ユーザーのメンタルを選択する必要があります。

ウィジェットの操作方法の詳細については、「、[拡張機能の概要](~/ios/platform/extensions.md)、 [3D Touch の概要](~/ios/platform/3d-touch.md)ドキュメントと Apple の[アプリ拡張機能プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html).

## <a name="working-with-vibrancy"></a>活気の操作

活気により、ウィジェットのテキストが、ウィジェットのライト、ぼかしの背景 (システムによって提供される) で表示したときに読みのままになることです。 IOS 10 では、前に、開発者は使用して、 [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect)ウィジェットの活気にします。 例えば:

```csharp
// DEPRECATED: Get Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreateForNotificationCenter ();
```

これは iOS 10 で非推奨とされ、いずれかで置き換える必要があります、 [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect)または[WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect)します。 例えば:

```csharp
// Get Primary Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreatePrimaryVibrancyEffectForNotificationCenter ();

// Get Secondary Widget Vibrancy Effect
var vibrancy2 = UIVibrancyEffect.CreateSecondaryVibrancyEffectForNotificationCenter ();
```

## <a name="working-with-collapsed-and-expanded-widgets"></a>折りたたまれ、展開されたウィジェットの使用

新しい ios 10 では、ウィジェットが含まれています、 [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode)プロパティにより、開発者は、コンテンツの量は、使用可能な説明を展開したり、コンテンツを折りたたんだりできます。

ウィジェットが表示される初期状態と折りたたまれた状態になります。 折りたたまれたウィジェットは、約 2 ~ 2.5 標準 iOS テーブル行の高さ。 開発者は、展開のウィジェットのサイズを要求することができますが、画面の高さよりも小さいする必要がありますが理想的です。 

折りたたまれた状態で、ウィジェットは、唯一の必須のスタンドアロンの情報を表示する必要があります。 ときに展開される、ウィジェット、折りたたまれた状態に示すように、主要なコンテンツを強化するための補足情報が表示されます。 たとえば、お天気アプリでは、折りたたむと、現在の気象条件を示していて、展開されたときの予測時間単位を追加します。

クイック アクションの一覧に表示されるウィジェットは、折りたたまれた状態でのみになります。 アプリでは、複数のウィジェットを提供する場合、開発者は、1 つを作成するときに 3D タッチを使用して、アプリのアイコンに負荷を適用することで、クイック アクションの一覧をユーザーのメンタルを選択する必要があります。

次の例は、単純な今日拡張機能 (ウィジェット) 折りたたまれたと展開の状態を処理するは。

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

詳細で、ウィジェットの表示モードの特定のコードを見てに設定します。 このウィジェットが展開済みの状態をサポートしているシステムに通知をするには、を使用します。

```csharp
// Tell widget it can be expanded
ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);
```

ウィジェットの現在の表示モードを取得するには、を使用します。

```csharp
ExtensionContext.GetWidgetActiveDisplayMode()
```

に展開または折りたたまれた状態の最大サイズを取得するには、を使用します。

```csharp
// Get the maximum size
var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
```

状態 (表示モード) の変更を処理するためを使用してください。

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

各状態 (折りたたみまたは展開される) に要求されたサイズを設定するだけでなく、新しいサイズに一致するように表示されるコンテンツも更新されます。

## <a name="summary"></a>まとめ

この記事では、Apple は iOS 10 でウィジェットのシステムに加えられた機能強化について説明し、Xamarin.iOS でそれらを実装する方法が示されますが。



## <a name="related-links"></a>関連リンク

- [iOS 10 のサンプル](https://developer.xamarin.com/samples/ios/iOS10/)
- [拡張機能の概要](~/ios/platform/extensions.md)
- [3D Touch の概要](~/ios/platform/3d-touch.md)
- [アプリ拡張機能のプログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)
