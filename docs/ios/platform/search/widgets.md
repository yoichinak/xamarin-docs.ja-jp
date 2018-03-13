---
title: "検索とホーム画面ウィジェットの機能強化"
description: "この記事では、Apple がシステムに加え、ウィジェット 10、iOS での機能強化について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: D66FD9E1-9E23-4BB6-825C-ED19B8F72A81
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 7ca863b92d8d7af46f4ce18f5d088347b9ca04ee
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="search-and-home-screen-widget-enhancements"></a>検索とホーム画面ウィジェットの機能強化

_この記事では、Apple がシステムに加え、ウィジェット 10、iOS での機能強化について説明します。_


Apple はウィジェットが優れた上にある新しい iOS 10 個のロック画面、色の背景に表示されるように、ウィジェットのシステムには拡張機能を導入しました。 さらに、ウィジェットが含まれるよう、 [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode)プロパティにより、開発者は、コンテンツの量が使用可能な説明を入力しを展開して、コンテンツを折りたたむことができます。

ウィジェット (とも呼ばれる現在の拡張機能) は、少量の有用な情報を表示または適切なタイミングで、アプリ固有の機能を公開する拡張機能の iOS の特別な種類です。 たとえば、ニュース アプリ トップ ヘッドラインを表示するウィジェットがあり、予定表アプリは、2 つの異なるウィジェット: 今日のイベントと今後のイベントを表示するいずれかを表示するいずれか。

ウィジェットは自由にカスタマイズし、テキスト、イメージ、ボタンなどの UI 要素を含めることがあります。さらに、開発者は、ウィジェットのレイアウトをさらにカスタマイズすることができます。

[![](widgets-images/widgets01.png "例のウィジェット")](widgets-images/widgets01.png#lightbox)

ユーザーを表示したり、アプリのウィジェットと対話できる 2 つの主な場所があります。

- **検索画面**-ユーザーは、検索画面に最も役に立つ、ウィジェットを追加できます。 検索画面は、ホームとロック画面の右側をスワイプしてアクセスします。
- **ホーム画面**-から、ホーム画面で、ユーザーに使用できる 3 D Touch アプリのアイコンに負荷を適用することでクイック アクション の一覧を開きます。 クイック アクション一覧の上でアプリのウィジェットが表示されます。 参照してください、 [3 D Touch をはじめ](~/ios/platform/3d-touch.md)詳細についてはドキュメントです。

## <a name="widgets-developer-suggestions"></a>ウィジェットの開発者向けの推奨事項

理想的には、開発者を再試行してください常に、し、ユーザーは、検索画面に追加するウィジェットをデザインする必要があります。 そのために、Apple には、以下の推奨事項。

- **優れた、ドリルイン エクスペリエンスを作成する**-ユーザーのステータスの更新の簡単なドリルインの情報を提供したり、単純なタスクをすばやく実行できるようにするウィジェットをします。 これにより、適切な量の情報と、必要な対話機能を提供します。 可能な限りは、1 回のタップで特定のタスクを実行するユーザーを許可します。 さらに、ウィジェットには、パン、またはスクロールをサポートしない、ためには、ウィジェットの設計で考慮この必要があります。
- **コンテンツをすばやく表示**-ウィジェットが設計にドリルイン、ので、ユーザーは、ウィジェットの表示後にロードするコンテンツの待機する必要があることはありません。 ウィジェットは、最近のコンテンツを表示するには、バック グラウンドで新しいコンテンツの読み込み中に、ローカルで、コンテンツをキャッシュする必要があります。
- **適切なパディングと余白を指定**-ウィジェットの表示、ウィジェットの表示の端にコンテンツを拡張することは避けて必要がありますしない過密状態を検索します。 いくつかピクセル単位のワイド マージン エッジとコンテンツの間で常にあります。 Apple では、アプリのアイコンをガイドとして、ウィジェットの上部に表示を使用しても提案します。 ウィジェットでは、グリッド レイアウトを提示する場合がグリッド内の項目の間の余白が正しいことを確認し、4 つの最大項目数を制限するください。
- **適応性のあるレイアウトを使用して**-A ウィジェットの幅がベースで実行されているデバイスとデバイスの向きでは異なります。 ウィジェットの高さも異なります Collapsed (既定) または拡張 (すべてのウィジェットではサポートされていません) の状態で表示されている場合。 折りたたまれているウィジェットでは、標準的な iOS テーブル行の約 2 分の高さがします。 開発者は、展開のウィジェットのサイズを要求することができますが、画面の高さよりも小さくする必要があることをお勧めします。 折りたたみの状態にウィジェットは基本的なスタンドアロンの情報を表示する必要があります。 ときに展開された、ウィジェット折りたたみ済み状態に示すように、主要なコンテンツを強化するための補足情報が表示されます。 クイック アクションの一覧に示すようにウィジェットはのみ折りたたみ済み状態になります。
- **ウィジェットの背景のカスタマイズをしない**-ウィジェットは、システムによって提供される明るいぼかしたバック グラウンドに表示されます。 これは、ウィジェット間の一貫性を昇格させ、コンテンツの読みやすさを向上させるためです。 ユーザーのロックおよびホーム画面の壁紙と衝突でしたが、ウィジェットの背景として画像を使用しないでください。
- **黒または濃い灰色で、システム フォントを使用して**- ウィジェット、最適のシステム フォントのテキストを表示する場合。 フォントは、背景、軽い、ぼかしたウィジェット目立た黒色または濃グレー色でなければなりません。
- **アプリのアクセス時に適切な提供**-ウィジェットを常に動作するとは別に、アプリから、ただしより深い機能が必要な場合は、ウィジェットにする必要を参照するか、特定の情報を編集するアプリを起動できません。 コンテンツ自体をタップするユーザーを許可するだけで、「アプリを開いて」ボタンを含めることはありませんし、サード パーティのアプリを開くことはありません。
- **クリア、簡潔なウィジェット名 を選択**-アプリのアイコンとウィジェットの名前がウィジェットのコンテンツを常に表示します。 Apple では、そのプライマリ ウィジェットのアプリの名前と、他の明確で正確な名前を使用してを提案します。 カスタム ウィジェットのタイトルを入力すると、(マップの近くにある、マップのレストランなど) など、アプリの名前を付ける必要があります。
- **認証が値を追加するときに通知**- 場合は追加の機能や情報、ユーザーが認証のみを使用で、署名されているユーザーに提示します。 たとえば、共有アプリ say「book 飛行へのサインイン」素敵します。
- **クイック アクション一覧ウィジェットを選択して**-アプリは、1 つ以上のウィジェットを提供する場合、開発者は、3 D Touch を使用して、アプリのアイコンに負荷を適用することにより、ユーザーがクイック アクション リストを表示する場合に 1 つを選択する必要があります。

ウィジェットの扱いの詳細についてを参照してください、 [Introduction to Extensions](~/ios/platform/extensions.md)、 [3 D Touch をはじめ](~/ios/platform/3d-touch.md)ドキュメントと Apple の[アプリ拡張機能プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html).

## <a name="working-with-vibrancy"></a>活気の操作

活気ことウィジェットのテキストがウィジェットのライト、背景 (システムによって提供される) に表示したときに読みのままになることを確認します。 10、iOS の前に、開発者が使用する[NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect)ウィジェットの活気のです。 例:

```csharp
// DEPRECATED: Get Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreateForNotificationCenter ();
```

これは、iOS 10 で廃止されましたが、いずれかで置き換える必要があります、 [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect)または[WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect)です。 例:

```csharp
// Get Primary Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreatePrimaryVibrancyEffectForNotificationCenter ();

// Get Secondary Widget Vibrancy Effect
var vibrancy2 = UIVibrancyEffect.CreateSecondaryVibrancyEffectForNotificationCenter ();
```

## <a name="working-with-collapsed-and-expanded-widgets"></a>折りたたまれたおよび展開されているウィジェットの操作

新しい 10、iOS にウィジェットが含まれるよう、 [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode)プロパティにより、開発者は、コンテンツの量が使用可能な説明を入力しを展開して、コンテンツを折りたたむことができます。

ウィジェットが表示される最初と折りたたみ済み状態になります。 折りたたまれているウィジェットでは、標準的な iOS テーブル行の約 2 分の高さがします。 開発者は、展開のウィジェットのサイズを要求することができますが、画面の高さよりも小さくする必要があることをお勧めします。 

折りたたみの状態にウィジェットは基本的なスタンドアロンの情報を表示する必要があります。 ときに展開された、ウィジェット折りたたみ済み状態に示すように、主要なコンテンツを強化するための補足情報が表示されます。 たとえば、Weather app は折りたたむと、現在の天気の条件を表示し、拡大されたときの予測時間単位を追加します。

クイック アクションの一覧に示すようにウィジェットはのみ折りたたみ済み状態になります。 アプリでは、1 つ以上のウィジェットを提供する場合、開発者には 3 D Touch を使用して、アプリのアイコンに負荷を適用することにより、ユーザーがクイック アクション リストを表示する場合にする必要がありますを選択します。

次の例は、単純な今日拡張機能 (ウィジェット) を折りたたみと展開状態を処理するは。

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

見て、ウィジェットの表示モードの特定のコードの詳細。 このウィジェットが展開された状態をサポートしているシステムに通知をするには、を使用します。

```csharp
// Tell widget it can be expanded
ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);
```

ウィジェットの現在の表示モードを取得するを使用します。

```csharp
ExtensionContext.GetWidgetActiveDisplayMode()
```

に展開または折りたたみの状態の最大サイズを取得するには、を使用します。

```csharp
// Get the maximum size
var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
```

状態 (表示モード) の変更を処理するを使用してください。

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

各状態 (折りたたみまたは展開) の要求のサイズを設定するだけでなく、新しいサイズを一致するように表示されているコンテンツも更新します。

## <a name="summary"></a>まとめ

この記事は、Apple がシステムに加え、ウィジェット 10、iOS での機能強化について説明し、Xamarin.iOS でそれらを実装する方法が示されます。



## <a name="related-links"></a>関連リンク

- [iOS 10 サンプル](https://developer.xamarin.com/samples/ios/iOS10/)
- [拡張機能の概要](~/ios/platform/extensions.md)
- [3 D Touch の概要](~/ios/platform/3d-touch.md)
- [アプリ拡張機能プログラミング ガイド](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)
