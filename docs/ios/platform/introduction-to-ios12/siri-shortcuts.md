---
title: Xamarin.iOS で Siri ショートカット
description: このドキュメントでは、iOS 12 で Siri ショートカットを使用する方法について説明します。 NSUserActivities、カスタムのインテント音声ショートカットや関連するショートカットを割り当てることがについて説明します。
ms.prod: xamarin
ms.assetid: 86424F79-3A7D-436E-927D-9A3267DA333B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: e37fd88f0d5fcf02ece0ae2f5e3164a507067e29
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61034786"
---
# <a name="siri-shortcuts-in-xamarinios"></a>Xamarin.iOS で Siri ショートカット

[IOS 10](~/ios/platform/sirikit/index.md)Apple では、SiriKit を導入しました。、、、予約をオーバーライドできるようにビルドのメッセージング、VoIP 通話、支払、ワークアウト、、および写真の Siri と対話するアプリの検索。

[IOS 11](~/ios/platform/introduction-to-ios11/sirikit.md)SiriKit の詳細の種類のアプリのサポートと UI のカスタマイズの柔軟性を獲得しました。

iOS 12 は、Siri ショートカット、Siri に機能を公開するアプリのすべての種類の許可を追加します。 Siri を学習特定アプリ ベースのタスクがユーザーに最も関連し、使用して潜在的なアクションを提案するこのナレッジを使用して、_ショートカット_します。 [ショートカット] をタップまたは音声コマンドを使用して呼び出すことをアプリを開くまたはバック グラウンド タスクを実行します。

ショートカットは、対象のアプリを開くこともなく多くの場合 – 一般的なタスクを実行するユーザーの能力を使用する必要があります。

## <a name="sample-app-soup-chef"></a>サンプル アプリ:Chef の混乱状態

Siri のショートカットをより深く理解するを参照してください、[スープ Chef](https://developer.xamarin.com/samples/monotouch/ios12/SoupChef/)サンプル アプリです。 スープ Chef、虚数部のスープ restaurant から注文を配置、自分の注文履歴を表示および Siri とやり取りして部品を注文するときに使用する語句を定義することができます。

> [!TIP]
> 12 の iOS シミュレーターまたはデバイスでスープ Chef をテストする前に、ショートカットをデバッグするときに便利ですが次の 2 つ設定を有効にします。
>
> - **設定**アプリを有効にする**開発者 > 表示最近ショートカット**。
> - **設定**アプリを有効にする**開発者 > のロック画面に表示寄付**します。
>
> これらの設定のことを簡単に見つけること作成された直後のデバッグ (予測の代わりに)、iOS 上のショートカットは、画面と検索画面をロックします。

サンプル アプリを使用します。

- インストールし、スープ Chef に 12 の iOS シミュレーターでのサンプル アプリを実行または[デバイス](#testing-on-device)します。
- をクリックして、 **+** 新しい注文を作成する右のボタンをクリックします。
- 部品の種類を選択、数量とオプションを指定およびタップ**Place Order**します。
- **注文履歴**画面で、新しく作成された順序の詳細を表示する をタップします。
- 注文の詳細画面の下部には、タップ**Siri に追加**します。
- 順序に関連付ける をタップして音声フレーズを記録**完了**します。
- スープ Chef を最小限に抑える、Siri を起動し、もう一度記録した音声フレーズを使用して、注文を配置します。
- Siri には、注文が完了すると、スープ Chef を再び開くしで新しい注文が表示されていることに注意してください、**注文履歴**画面。

サンプル アプリについて説明する方法。

- [アプリを開く、NSUserActivity ショートカットを使用して](#using-an-nsuseractivity-shortcut-to-open-an-app)します。
- [タスクを実行するカスタム インテント ショートカットを使用して](#using-a-custom-intent-shortcut-to-perform-a-task)します。
- [カスタムの目的のユーザー インターフェイスを提供する](#providing-a-user-interface-for-a-custom-intent)します。
- [音声のショートカットを作成](#creating-a-voice-shortcut)です。

## <a name="infoplist-and-entitlementsplist"></a>Info.plist と Entitlements.plist

ご覧になるスープ Chef コード深く掘り下げ、前にその**Info.plist**と**Entitlements.plist**ファイル。

### <a name="infoplist"></a>Info.plist

**Info.plist**ファイル、 **SoupChef**プロジェクトの定義、**バンドル識別子**として`com.xamarin.SoupChef`します。 このバンドル識別子は Intents および Intents UI のバンドル識別子のプレフィックスとして使用する拡張機能がこのドキュメントの後半で説明します。

**Info.plist**ファイルにも、次が含まれています。

```xml
<key>NSUserActivityTypes</key>
<array>
    <string>OrderSoupIntent</string>
    <string>com.xamarin.SoupChef.viewMenu</string>
</array>
```

これは、`NSUserActivityTypes`キー/値ペアは、スープ Chef を処理する方法を知っていることを示します、 `OrderSoupIntent`、および[ `NSUserActivity` ](xref:Foundation.NSUserActivity)こと、 [ `ActivityType` ](xref:Foundation.NSUserActivity.ActivityType) "com.xamarin.SoupChef.viewMenu"のです。

アクティビティと、その拡張機能ではなく、アプリ自体に渡されるカスタムのインテントの処理、 `AppDelegate` (、 [ `UIApplicationDelegate` ](xref:UIKit.UIApplicationDelegate)によって、 [ `ContinueUserActivity` ](xref:UIKit.UIApplicationDelegate.ContinueUserActivity*)メソッド。

### <a name="entitlementsplist"></a>Entitlements.plist

**Entitlements.plist**ファイル、 **SoupChef**プロジェクトには、次が含まれています。

```xml
<key>com.apple.security.application-groups</key>
<array>
    <string>group.com.xamarin.SoupChef</string>
</array>
<key>com.apple.developer.siri</key>
<true/>
```

この構成では、アプリは"group.com.xamarin.SoupChef"アプリ グループを使用することを示します。 **SoupChefIntents**アプリ拡張機能を共有する 2 つのプロジェクトは、この同じアプリ グループを使用します。 [`NSUserDefaults`](xref:Foundation.NSUserDefaults)
データ。

`com.apple.developer.siri`キーは、アプリが、Siri と対話することを示します。

> [!NOTE]
> **SoupChef**プロジェクトのビルド構成セット**カスタム権利**に**Entitlements.plist**します。

## <a name="using-an-nsuseractivity-shortcut-to-open-an-app"></a>アプリを開く、NSUserActivity ショートカットを使用します。

特定のコンテンツを表示するアプリを開くショートカットを作成するには、作成、`NSUserActivity`を開くショートカットが希望される画面のビュー コント ローラーにアタッチします。

### <a name="setting-up-an-nsuseractivity"></a>設定する、NSUserActivity

メニュー画面で、`SoupMenuViewController`作成、`NSUserActivity`ビュー コント ローラーに割り当てます[ `UserActivity` ](xref:UIKit.UIResponder.UserActivity)プロパティ。

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    UserActivity = NSUserActivityHelper.ViewMenuActivity;
}
```

設定、`UserActivity`プロパティ_団体に寄付_Siri するアクティビティ。 この献血からは、Siri は、このアクティビティがユーザーに関連してより後で、提案するための学習するタイミングと場所に関する情報を取得します。

`NSUserActivityHelper` 含まれるユーティリティ クラス、 **SoupChef**ソリューションで、 **SoupKit**クラス ライブラリ。 作成されます、 `NSUserActivity` Siri と検索に関連するさまざまなプロパティを設定します。

```csharp
public static string ViewMenuActivityType = "com.xamarin.SoupChef.viewMenu";

public static NSUserActivity ViewMenuActivity {
    get
    {
        var userActivity = new NSUserActivity(ViewMenuActivityType)
        {
            Title = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_LUNCH_TITLE", "View menu activity title"),
            EligibleForSearch = true,
            EligibleForPrediction = true
        };

        var attributes = new CSSearchableItemAttributeSet(NSUserActivityHelper.SearchableItemContentType)
        {
            ThumbnailData = UIImage.FromBundle("tomato").AsPNG(),
            Keywords = ViewMenuSearchableKeywords,
            DisplayName = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_LUNCH_TITLE", "View menu activity title"),
            ContentDescription = NSBundleHelper.SoupKitBundle.GetLocalizedString("VIEW_MENU_CONTENT_DESCRIPTION", "View menu content description")
        };
        userActivity.ContentAttributeSet = attributes;

        var phrase = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_LUNCH_SUGGESTED_PHRASE", "Voice shortcut suggested phrase");
        userActivity.SuggestedInvocationPhrase = phrase;
        return userActivity;
    }
}
```

以下に注意具体的には。

- 設定`EligibleForPrediction`に`true`Siri がこのアクティビティを予測し、ショートカットとして画面にことを示します。
- [ `ContentAttributeSet` ](xref:Foundation.NSUserActivity.ContentAttributeSet)配列は、標準[ `CSSearchableItemAttributeSet` ](xref:CoreSpotlight.CSSearchableItemAttributeSet)含めるために使用、 `NSUserActivity` iOS 検索結果にします。
- [`SuggestedInvocationPhrase`](xref:Foundation.NSUserActivity.SuggestedInvocationPhrase) Siri は、潜在的な選択肢としてショートカットに語句を割り当てる場合で、ユーザーに提案されます語句です。

### <a name="handling-an-nsuseractivity-shortcut"></a>NSUserActivity ショートカットの処理

処理するために、`NSUserActivity`ショートカットがユーザーによって呼び出される、iOS アプリケーションをオーバーライドする必要があります、`ContinueUserActivity`のメソッド、`AppDelegate`クラス、応答に基づいて、 `ActivityType` 、渡された内のフィールド`NSUserActivity`オブジェクト。

```csharp
public override bool ContinueUserActivity(UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    // ...
    else if (userActivity.ActivityType == NSUserActivityHelper.ViewMenuActivityType)
    {
        HandleUserActivity();
        return true;
    }
    // ...
}
```

このメソッドを呼び出す`HandleUserActivity`、メニュー画面にセグエの検索し、これを呼び出します。

```csharp
void HandleUserActivity()
{
    var rootViewController = Window?.RootViewController as UINavigationController;
    var orderHistoryViewController = rootViewController?.ViewControllers?.FirstOrDefault() as OrderHistoryTableViewController;
    if (orderHistoryViewController is null)
    {
        Console.WriteLine("Failed to access OrderHistoryTableViewController.");
        return;
    }
    var segue = OrderHistoryTableViewController.SegueIdentifiers.SoupMenu;
    orderHistoryViewController.PerformSegue(segue, null);
}
```

### <a name="assigning-a-phrase-to-an-nsuseractivity"></a>句を NSUserActivity に割り当てる

語句を割り当てる、 `NSUserActivity`、iOS の開く**設定**アプリ選択**検索 (&)、Siri > 自分のショートカット**。 (この例では、「Order ランチ」) では、ショートカットを選択し、フレーズを記録します。

Siri を呼び出すと、このフレーズを使用してに、メニュー画面のスープ Chef が開きます。

## <a name="using-a-custom-intent-shortcut-to-perform-a-task"></a>インテントのカスタム ショートカットを使用してタスクを実行するには

### <a name="defining-a-custom-intent"></a>カスタムの目的を定義します。

ユーザーがアプリに関連する特定のタスクをすばやく完了できるショートカットを提供するには、カスタムのインテントを作成します。 カスタムの目的は、ユーザーがそのタスク、およびタスクの実行から潜在的な応答に関連するパラメーターを完了するようにタスクを表します。 カスタムの目的の定義方法に応じてそれを呼び出すアプリを開くか、バック グラウンド タスクを実行します。

Xcode の 10 を使用すると、カスタムのインテントを作成します。 [SoupChef リポジトリ](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef)で定義されているカスタムのインテント**OrderSoupIntentCodeGen**Objective C のプロジェクトをします。 このプロジェクトを開き、選択、 **Intents.intentdefinition**ファイル、**プロジェクト ナビゲーター**を表示する、 **OrderSoup**インテントです。

次の点に注意してください。

- 目的が、**カテゴリ**の**順序**します。 カスタムの目的に使用できるさまざまな定義済みのカテゴリがあります。カスタムの意図を有効にするタスクに最も近いものを選択します。 これは、アプリ、順序付けスープのため**OrderSoupIntent**使用**順序**します。
- **確認**チェック ボックスを Siri がタスクを実行する前に確認を要求する必要があるかどうかを示します。 **順序スープ**スープ Chef でをインテントに、このオプションが有効になって、ユーザーが購入を行うためです。
- **パラメーター** .intentdefinition ファイルのセクションのショートカットに関連するパラメーターを定義します。 部品を注文には、スープ Chef はスープ、その数量、および関連するオプションの種類を知る必要があります。
各パラメーターには型であります。定義済みの型では表現できないパラメーターに設定されます**カスタム**します。
- **ショートカット型**インターフェイスには、さまざまなパラメーターの組み合わせのショートカットを推奨する際に Siri を使用できますがについて説明します。 関連付けられている**タイトル**と**サブタイトル**セクションでは、ユーザーに推奨されるショートカットを表示する場合は Siri を使用するメッセージを定義することができます。
- **バック グラウンド実行をサポート**ユーザーによる操作用のアプリを開くことがなく実行できる任意のショートカットのチェック ボックスを選択する必要があります。

### <a name="defining-custom-intent-responses"></a>インテントのカスタム応答を定義します。

**応答**項目は、以下の入れ子になった、 **OrderSoup**の目的は、部品注文からの潜在的な応答を表します。

**OrderSoup**目的の応答の定義が、次に注意してください。

- 応答の**プロパティ**ユーザーに通知メッセージをカスタマイズするために使用できます。 **OrderSoup**インテントの応答に**スープ**と**waitTime**プロパティ。
- **応答テンプレート**を目的のタスクが完了した後に状態を示すために使用できるさまざまな成功と失敗メッセージを指定します。
- **成功**応答の成功を示すチェック ボックスを選択する必要があります。
 - **OrderSoupIntent**成功応答を使用して、**スープ**と**waitTime**親しみやすく、便利なときに、部品注文は準備ができますを説明するメッセージを提供するプロパティ。

### <a name="generating-code-for-the-custom-intent"></a>カスタムの目的のコードを生成します。

カスタムのインテントとその回答をプログラムで操作に使用できるコードを生成する Xcode は、このカスタムのインテントの定義を含む、Xcode プロジェクトをビルドします。

これを表示するには、コードが生成されます。

- 開いている**AppDelegate.m**します。
- カスタムの目的のヘッダー ファイルには、インポートを追加します。 `#import "OrderSoupIntent.h"`
- クラス内の任意のメソッド内への参照を追加`OrderSoupIntent`します。
- 右クリックして`OrderSoupIntent`選択**定義にジャンプ**します。
- 新しく開かれたファイルで右クリックして**OrderSoupIntent.h**を選択し、 **Finder で表示する**します。
- 開き、 **Finder**生成コードを含む .h、.m ファイルを含むウィンドウ。

生成されたこのコードは次のとおりです。

- `OrderSoupIntent` -カスタムの意図を表すクラスです。
- `OrderSoupIntentHandling` – 目的が実行されることを確認するために使用するメソッドと実際にこれを実行するメソッドを定義するプロトコル。
- `OrderSoupIntentResponseCode` – さまざまな応答の状態を定義する列挙。
- `OrderSoupIntentResponse` – 目的を実行する応答を表すクラス。

### <a name="creating-a-binding-to-the-custom-intent"></a>カスタムの意図をバインドの作成

Xamarin.iOS アプリの Xcode によって生成されたコードを使用するには、作成、C#をバインドします。

#### <a name="creating-a-static-library-and-c-binding-definitions"></a>スタティック ライブラリを作成し、C#バインディング定義

[SoupChef リポジトリ](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef)、確認、 **OrderSoupIntentStaticLib**フォルダー、およびオープン、 **OrderSoupIntentStaticLib.xcodeproj** Xcode プロジェクト。

これは、 **Cocoa Touch のスタティック ライブラリ**プロジェクトが含まれています、 **OrderSoupIntent.h**と**OrderSoupIntent.m** Xcode によって生成されるファイル。

#### <a name="configuring-the-static-library-project-build-settings"></a>スタティック ライブラリ プロジェクトのビルド設定を構成します。

Xcode で**プロジェクト ナビゲーター**、最上位のプロジェクトを選択します。 **OrderSoupIntentStaticLib**、に移動します**ビルド フェーズ > コンパイル ソース**します。
注意して**OrderSoupIntent.m** (どの imports **OrderSoupIntent.h**) ここに記載されています。 **Link Binary With Libraries**、注意**Intents.framework**と**Foundation.framework**が含まれています。
これら設定した状態で、framework が正しくビルドされます。

#### <a name="building-the-static-library-and-generating-c-bindings-definitions"></a>スタティック ライブラリを構築し、生成するC#バインドの定義

スタティック ライブラリをビルドし、生成C#、バインドの定義に次の手順します。

- [インストール目的油性](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/get-started?context=xamarin/mac#installing-objective-sharpie)、Xcode により作成された .h、.m ファイルからバインド定義を生成するために使用するツール。

- Xcode 10 コマンド ライン ツールを使用してシステムを構成するには。

    > [!WARNING]
    > 選択したコマンド ライン ツールの更新、システム上の Xcode のインストールされているすべてのバージョンに影響を与えます。 完了したらスープ Chef を使用してサンプル アプリ、元の構成設定にこれを元に戻すことを確認します。

    - Xcode で、次のように選択します。 **Xcode > 設定 > 場所**設定と**コマンド ライン ツール**、システムで使用できる最新の Xcode の 10 のインストールにします。

- ターミナルで、`cd`を**OrderSoupIntentStaticLib**ディレクトリ。

- 型`make`、どのビルド。

    - スタティック ライブラリ、 **libOrderSoupIntentStaticLib.a**
    - **Bo**出力ディレクトリをC#バインドの定義。
        - **ApiDefinitions.cs**
        - **StructsAndEnums.cs**

**OrderSoupIntentBindings**このスタティック ライブラリとその関連するバインドの定義に依存するプロジェクトが自動的にこれらの項目をビルドします。
ただし、上記のプロセスを手動で実行するには、期待どおりにビルドされる確認されます。

スタティック ライブラリを作成し、目的の油性を使用して作成する方法についてC#バインドの定義を見て、 [、iOS OBJECTIVE-C ライブラリのバインド](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough?tabs=vsmac#creating-a-static-library)チュートリアル。

#### <a name="creating-a-bindings-library"></a>バインド ライブラリを作成します。

スタティック ライブラリと、C#バインド定義を作成、残りの部分 Xcode で生成されたを使用するために必要な Xamarin.iOS プロジェクトでのインテントに関連するコードはバインド ライブラリ。

[スープの Chef リポジトリ](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef)を開き、 **SoupChef.sln**ファイル。 特に、このソリューションに含まれる**OrderSoupIntentBinding**、上記で生成された静的ライブラリのバインド ライブラリ。

具体的にはこのプロジェクトが含まれることに注意してください。

- **ApiDefinitions.cs** – ファイルが目的油性によって上記生成され、このプロジェクトに追加します。 このファイルの**ビルド アクション**に設定されている**ObjcBindingApiDefinition**します。
- **StructsAndEnums.cs** – 別のファイル上の目標油性によって生成され、このプロジェクトに追加します。 このファイルの**ビルド アクション**に設定されている**ObjcBindingCoreSource**します。
- A**ネイティブ参照**に**libOrderSoupIntentStaticLib.a**、ビルドのスタティック ライブラリ。

> [!NOTE]
> 両方**ApiDefinitions.cs**と**StructsAndEnums.cs**などの属性を含める`[Watch (5,0), iOS (12,0)]`します。 このプロジェクトで必要ないので、目的の油性によって生成されたこれらの属性をアウト コメントされています。

作成の詳細については、C#バインド ライブラリを表示する、 [iOS OBJECTIVE-C ライブラリのバインド](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough?tabs=vsmac#create-a-xamarinios-binding-project)チュートリアル。

注意して、 **SoupChef**への参照がプロジェクトに含まれている**OrderSoupIntentBinding**、今すぐにアクセスできるでつまりC#、クラス、インターフェイス、および列挙型が含まれています。

- `OrderSoupIntent`
- `OrderSoupIntentHandling`
- `OrderSoupIntentResponse`
- `OrderSoupIntenseResponseCode`

### <a name="adding-the-intentdefinition-file-to-your-solution"></a>.Intentdefinition ファイルをソリューションに追加します。

C# **SoupChef**ソリューション、 **SoupKit**プロジェクトには、アプリとその拡張機能の間で共有コードが含まれています。 **Intents.intentdefinition**でファイルが配置されている、 **Base.lproj**ディレクトリ**SoupKit**があり、**ビルド アクション**の**コンテンツ**します。 ビルド プロセスが正常に動作するアプリケーションに必要な部品の Chef アプリ バンドルにこのファイルをコピーします。

### <a name="donating-an-intent"></a>インテントを寄付します。

Siri のショートカットを提案するためには、最初のショートカットが関連する場合を理解するにする必要があります。

この詳細については、スープ Chef Siri を与える_団体に寄付_インテント Siri たびに、ユーザーの混乱状態に注文します。 これが寄付、場所寄付が、ときに、この寄付 – に基づいて、パラメーターが含まれている – Siri 学習、将来、ショートカットを提案するときにします。

**SoupChef**を使用して、`SoupOrderDataManager`寄付を配置するクラス。
で、ユーザーの部品注文とする呼び出されると、`PlaceOrder`メソッドを呼び出します[ `DonateInteraction` ](xref:Intents.INInteraction.DonateInteraction*):

```csharp
void DonateInteraction(Order order)
{
    var interaction = new INInteraction(order.Intent, null);
    interaction.Identifier = order.Identifier.ToString();
    interaction.DonateInteraction((error) =>
    {
        // ...
    });
}
```

インテントをフェッチした後にラップする[ `INInteraction`](xref:Intents.INInteraction)します。
`INInteraction`が与えられます、 [`Identifier`](xref:Intents.INInteraction.Identifier*)
(便利になります後で有効になっているインテント寄付を削除するときに) 注文の一意の ID に一致します。 次に、相互作用は Siri を寄付します。

呼び出し、 `order.Intent` get アクセス操作子のフェッチ、`OrderSoupIntent`を設定して、順序を表すその`Quantity`、 `Soup`、`Options`とイメージ、およびユーザーに関連付ける Siri の語句を記録するときに、修正候補として使用する、呼び出しの語句目的: で

```csharp
public OrderSoupIntent Intent
{
    get
    {
        var orderSoupIntent = new OrderSoupIntent();
        orderSoupIntent.Quantity = new NSNumber(Quantity);
        orderSoupIntent.Soup = new INObject(MenuItem.ItemNameKey, MenuItem.LocalizedString);

        var image = UIImage.FromBundle(MenuItem.IconImageName);
        if (!(image is null))
        {
            var data = image.AsPNG();
            orderSoupIntent.SetImage(INImage.FromData(data), "soup");
        }

        orderSoupIntent.Options = MenuItemOptions
            .ToArray<MenuItemOption>()
            .Select<MenuItemOption, INObject>(arg => new INObject(arg.Value, arg.LocalizedString))
            .ToArray<INObject>();

        var comment = "Suggested phrase for ordering a specific soup";
        var phrase = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_SOUP_SUGGESTED_PHRASE", comment);
        orderSoupIntent.SuggestedInvocationPhrase = String.Format(phrase, MenuItem.LocalizedString);

        return orderSoupIntent;
    }
}
```

#### <a name="removing-invalid-donations"></a>無効な寄付を削除します。

Siri が役に立たない、またはわかりづらいショートカットの提案を行わないように、有効なされなく寄付を削除するのには重要です。

スープ Chef では、**構成メニュー**画面を使用して、メニューの項目利用不可としてマークすることができます。 Siri をする必要がありますを使用できないメニュー品目の注文へのショートカットをお勧めします不要になったため、`RemoveDonation`メソッドの`SoupMenuManager`は使用できなくするメニュー項目の寄付を削除します。 これで。

- ここで使用できないメニュー項目に関連付けられた注文を検索します。
- それらの識別子を取得します。
- 同じ識別子の相互作用を削除しています。

```csharp
void RemoveDonation(MenuItem menuItem)
{
    if (!menuItem.IsAvailable)
    {
        Order[] orderHistory = OrderManager?.OrderHistory.ToArray<Order>();
        if (orderHistory is null)
        {
            return;
        }

        string[] orderIdentifiersToRemove = orderHistory
            .Where<Order>((order) => order.MenuItem.ItemNameKey == menuItem.ItemNameKey)
            .Select<Order, string>((order) => order.Identifier.ToString())
            .ToArray<string>();

        INInteraction.DeleteInteractions(orderIdentifiersToRemove, (error) =>
        {
            if (!(error is null))
            {
                Console.WriteLine($"Failed to delete interactions with error {error.ToString()}");
            }
            else
            {
                Console.WriteLine("Successfully deleted interactions");
            }
        });
    }
}
```

### <a name="creating-an-intents-extension"></a>Intents の拡張機能を作成します。

Siri がインテントを呼び出すときに実行されるコードは、Xamarin.iOS アプリ スープ Chef などと同じソリューションに新しいプロジェクトとして追加できる、Intents の拡張機能で配置されます。 **SoupChef**ソリューションでは、拡張機能と呼びます**SoupChefIntents**します。

#### <a name="soupchefintents-infoplist-and-entitlementsplist"></a>SoupChefIntents – Info.plist と Entitlements.plist

##### <a name="soupchefintents-infoplist"></a>SoupChefIntents – Info.plist

**Info.plist**で、 **SoupChefIntents**プロジェクトの定義、**バンドル識別子**として`com.xamarin.SoupChef.SoupChefIntents`します。

**Info.plist**ファイルにも、次が含まれています。

```xml
<key>NSExtension</key>
<dict>
    <key>NSExtensionAttributes</key>
    <dict>
        <key>IntentsRestrictedWhileLocked</key>
        <array/>
        <key>IntentsSupported</key>
        <array>
            <string>OrderSoupIntent</string>
        </array>
        <key>IntentsRestrictedWhileProtectedDataUnavailable</key>
        <array/>
    </dict>
    <key>NSExtensionPointIdentifier</key>
    <string>com.apple.intents-service</string>
    <key>NSExtensionPrincipalClass</key>
    <string>IntentHandler</string>
</dict>
```

上記の**Info.plist**:

- `IntentsRestrictedWhileLocked` デバイスがロックされている場合にのみ処理インテントを一覧表示します。
- `IntentsSupported` この拡張機能によって処理されるインテントを一覧表示します。
- `NSExtensionPointIdentifier` アプリ拡張機能の種類を指定します (を参照してください[Apple のドキュメント](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212-SW15)詳細については)。
- `NSExtensionPrincipalClass` この拡張機能でサポートされているインテントを処理するために使用する必要がありますクラスを指定します。

##### <a name="soupchefintents-entitlementsplist"></a>SoupChefIntents – Entitlements.plist

**Entitlements.plist**で、 **SoupChefIntents**プロジェクトには、**アプリ グループ**機能します。 この機能の構成と同じアプリ グループを使用する、 **SoupChef**プロジェクト。

```xml
<key>com.apple.security.application-groups</key>
<array>
    <string>group.com.xamarin.SoupChef</string>
</array>
```

スープ Chef を使用してデータを永続化`NSUserDefaults`します。 アプリの同じグループを参照するアプリとアプリの拡張機能の間のデータを共有するために、 **Entitlements.plist**ファイル。

> [!NOTE]
> **SoupChefIntents**プロジェクトのビルド構成セット**カスタム権利**に**Entitlements.plist**します。

#### <a name="handling-an-ordersoupintent-background-task"></a>OrderSoupIntent のバック グラウンド タスクの処理

Intents の拡張機能は、カスタムの意図に基づいたショートカットの必要なバック グラウンド タスクを実行します。

Siri の呼び出し、 [ `GetHandler` ](xref:Intents.INExtension.GetHandler*)のメソッド、`IntentHandler`クラス (で定義されている**Info.plist**として、 `NSExtensionPrincipalClass`) を拡張するクラスのインスタンスを取得する`OrderSoupIntentHandling`、使用できます。処理するために、 `OrderSoupIntent`:

```csharp
[Register("IntentHandler")]
public class IntentHandler : INExtension
{
    public override NSObject GetHandler(INIntent intent)
    {
        if (intent is OrderSoupIntent)
        {
            return new OrderSoupIntentHandler();
        }
        throw new Exception("Unhandled intent type: ${intent}");
    }

    protected IntentHandler(IntPtr handle) : base(handle) { }
}
```

`OrderSoupIntentHandler`、で定義されている、 **SoupKit**共有コード、重要なメソッドの実装の 2 つのプロジェクト。

- `ConfirmOrderSoup` – 目的に関連付けられているタスクを実行する実際にかどうかを確認します
- `HandleOrderSoup` – は、部品注文し、渡された完了ハンドラーを呼び出すことによって、ユーザーに応答

#### <a name="handling-an-ordersoupintent-that-opens-the-app"></a>アプリを開いた、OrderSoupIntent の処理

アプリでは、バック グラウンドで実行されているインテントを正しく処理する必要があります。
これらの値と同じ方法で処理が`NSUserActivity`ショートカットで、`ContinueUserActivity`メソッドの`AppDelegate`:

```csharp
public override bool ContinueUserActivity(UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    var intent = userActivity.GetInteraction()?.Intent as OrderSoupIntent;
    if (!(intent is null))
    {
        HandleIntent(intent);
        return true;
    }
    // ...
}  
```

## <a name="providing-a-user-interface-for-a-custom-intent"></a>カスタムの目的のユーザー インターフェイスを提供します。

Intents UI 拡張機能は、Intents の拡張機能のカスタム ユーザー インターフェイスを提供します。 **SoupChef**ソリューション、 **SoupChefIntentsUI**のインターフェイスを提供する Intents UI 拡張機能は、 **SoupChefIntents**します。

### <a name="soupchefintentsui--infoplist-and-entitlementsplist"></a>SoupChefIntentsUI – Info.plist と Entitlements.plist

#### <a name="soupchefintentsui-infoplist"></a>SoupChefIntentsUI – Info.plist

**Info.plist**で、 **SoupChefIntentsUI**プロジェクトの定義、**バンドル識別子**として`com.xamarin.SoupChef.SoupChefIntentsui`します。

**Info.plist**ファイルにも、次が含まれています。

```xml
<key>NSExtension</key>
<dict>
    <key>NSExtensionAttributes</key>
    <dict>
        <key>IntentsSupported</key>
        <array>
            <string>OrderSoupIntent</string>
        </array>
        <!-- ... -->
    </dict>
    <key>NSExtensionPointIdentifier</key>
    <string>com.apple.intents-ui-service</string>
    <key>NSExtensionMainStoryboard</key>
    <string>MainInterface</string>
</dict>
```

上記の**Info.plist**:

- `IntentsSupported` 示します、`OrderSoupIntent`この Intents UI 拡張機能によって処理されます。
- `NSExtensionPointIdentifier` アプリ拡張機能の種類を指定します (を参照してください[Apple のドキュメント](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212-SW15)詳細については)。
- `NSExtensionMainStoryboard` この拡張機能の主要なインターフェイスを定義するストーリー ボードを指定します

#### <a name="soupchefintentsui-entitlementsplist"></a>SoupChefIntentsUI – Entitlements.plist

**SoupChefIntentsUI**プロジェクトは必要はありません、 **Entitlements.plist**ファイル。

### <a name="creating-the-user-interface"></a>ユーザー インターフェイスの作成

以降、 **Info.plist**の**SoupChefIntentsUI**設定、`NSExtensionMainStoryboard`キー `MainInterface`、 **MainInterace.storyboard**ファイル用のインターフェイスを定義しますIntents UI 拡張機能。

型の 1 つのビュー コント ローラーがある、このストーリー ボードに**IntentViewController**します。 これは、2 つのビューを参照します。

- **invoiceView**型の `InvoiceView`
- **confirmationView**型の `ConfirmOrderView`

> [!NOTE]
> インターフェイスを**invoiceView**と**confirmationView**で定義された**Main.storyboard**セカンダリ ビューとして。 IOS Designer が Visual studio for Mac と Visual Studio 2017 では、表示またはセカンダリ ビューを編集するためのサポートは提供されません。これを行うには、開く**Main.storyboard** Xcode の Interface Builder でします。

`IntentViewController` 実装します [`IINUIHostedViewControlling`](xref:IntentsUI.IINUIHostedViewControlling)
このインターフェイスを使用して、Siri インテントを使用する場合は、カスタム インターフェイスを提供するために使用します。 、 [`ConfigureView`](xref:IntentsUI.INUIHostedViewControlling_Extensions.ConfigureView*)
メソッドを呼び出して、確認または相互作用が確認されているかどうかに応じて、請求書を表示するインターフェイスをカスタマイズする ([`INIntentHandlingStatus.Ready`](xref:Intents.INIntentHandlingStatus)) または正常に実行される ([ `INIntentHandlingStatus.Success`](xref:Intents.INIntentHandlingStatus)):

```csharp
[Export("configureViewForParameters:ofInteraction:interactiveBehavior:context:completion:")]
public void ConfigureView(
    NSSet<INParameter> parameters,
    INInteraction interaction,
    INUIInteractiveBehavior interactiveBehavior,
    INUIHostedViewContext context,
    INUIHostedViewControllingConfigureViewHandler completion)
{
    // ...
    if (interaction.IntentHandlingStatus == INIntentHandlingStatus.Ready)
    {
        desiredSize = DisplayInvoice(order, intent);
    }
    else if(interaction.IntentHandlingStatus == INIntentHandlingStatus.Success)
    {
        var response = interaction.IntentResponse as OrderSoupIntentResponse;
        if (!(response is null))
        {
            desiredSize = DisplayOrderConfirmation(order, intent, response);
        }
    }
    completion(true, parameters, desiredSize);
}
```

> [!TIP]
> 詳細については、`ConfigureView`メソッドでは、Apple の WWDC 2017 でプレゼンテーションを視聴[SiriKit で新](https://developer.apple.com/videos/play/wwdc2017/214/)します。

## <a name="creating-a-voice-shortcut"></a>音声のショートカットの作成

スープ Chef は、Siri と部品を注文すること、各注文に音声ショートカットを割り当てるインターフェイスを提供します。 実際には、記録し、音声のショートカットを割り当てるに使用されるインターフェイスは iOS によって提供され、ほとんどのカスタム コードが必要 。

`OrderDetailViewController`テーブルをタップすると、 **Siri を追加**行、 [ `RowSelected` ](xref:UIKit.UITableViewSource.RowSelected*)メソッドは、追加するか、音声のショートカットを編集するための画面を表示します。

```csharp
public override void RowSelected(UITableView tableView, NSIndexPath indexPath)
{
    // ...
    else if (TableConfiguration.Sections[indexPath.Section].Type == OrderDetailTableConfiguration.SectionType.VoiceShortcut)
    {
        INVoiceShortcut existingShortcut = VoiceShortcutDataManager?.VoiceShortcutForOrder(Order);
        if (!(existingShortcut is null))
        {
            var editVoiceShortcutViewController = new INUIEditVoiceShortcutViewController(existingShortcut);
            editVoiceShortcutViewController.Delegate = this;
            PresentViewController(editVoiceShortcutViewController, true, null);
        }
        else
        {
            // Since the app isn't yet managing a voice shortcut for
            // this order, present the add view controller
            INShortcut newShortcut = new INShortcut(Order.Intent);
            if (!(newShortcut is null))
            {
                var addVoiceShortcutVC = new INUIAddVoiceShortcutViewController(newShortcut);
                addVoiceShortcutVC.Delegate = this;
                PresentViewController(addVoiceShortcutVC, true, null);
            }
        }
    }
}
```

現在表示されているために、既存の音声ショートカットが存在するかどうかに基づいて`RowSelected`型のビュー コント ローラーを表示します。 [ `INUIEditVoiceShortcutViewController` ](xref:IntentsUI.INUIEditVoiceShortcutViewController)または[ `INUIAddVoiceShortcutViewController`](xref:IntentsUI.INUIAddVoiceShortcutViewController)します。
各ケースで`OrderDetailViewController`ビュー コント ローラーとして設定`Delegate`も実装されているためにです [`IINUIAddVoiceShortcutViewControllerDelegate`](xref:IntentsUI.IINUIAddVoiceShortcutViewControllerDelegate)
[ `IINUIEditVoiceShortcutViewControllerDelegate`](xref:IntentsUI.IINUIEditVoiceShortcutViewControllerDelegate)します。

## <a name="testing-on-device"></a>デバイスでのテスト

スープ Chef をデバイスで実行するには、次の手順に従います。 読み取ることも、[自動プロビジョニングに関する注意事項](#automatic-provisioning)します。

### <a name="app-group-app-ids-provisioning-profiles"></a>アプリ グループ、アプリケーション Id、プロビジョニング プロファイル

**証明書、Id、およびプロファイル**のセクション、 [Apple Developer Portal](https://developer.apple.com/)次の操作を行います。

- スープ Chef アプリとその拡張機能の間でデータを共有するアプリ グループを作成します。 例: **group.com.yourcompanyname.SoupChef**

- 次の 3 つのアプリ Id を作成する: アプリ自体の 1 つ、Intents の拡張機能の 1 つおよび Intents UI 拡張機能の 1 つ。 例:

    - App: **com.yourcompanyname.SoupChef**
        - このアプリ ID に割り当てる、SiriKit と**アプリ グループ**機能します。

    - Intents の拡張機能: **com.yourcompanyname.SoupChef.Intents**
        - このアプリ ID に割り当てる、**アプリ グループ**機能します。

    - Intents UI 拡張機能: **com.yourcompanyname.SoupChef.Intentsui**
        - このアプリ ID には、特別な機能は不要です。

- 上記のアプリ Id を作成すると、編集、**アプリ グループ**上記で作成したアプリと、特定のアプリ グループを指定する Intents の拡張機能に割り当てられている機能です。

- 次の 3 つ新しい開発プロビジョニング プロファイル、新しいアプリ Id ごとに 1 つを作成します。

- これらのプロビジョニング プロファイルをダウンロードし、インストールする 1 つずつをダブルクリックします。 Visual Studio for Mac または Visual Studio 2017 が既に実行されている場合は、新しいプロビジョニング プロファイルが登録を確認することを再起動します。

### <a name="editing-infoplist-entitlementsplist-and-source-code"></a>Info.plist、Entitlements.plist、およびソース コードの編集

Visual Studio for Mac または Visual Studio 2017 で、次の操作を行います。

- さまざまな更新**Info.plist**ソリューション内のファイル。 アプリ、Intents の拡張機能、および Intents UI 拡張機能設定**バンドル識別子**上で定義したアプリ id:

    - App: **com.yourcompanyname.SoupChef**
    - Intents の拡張機能: **com.yourcompanyname.SoupChef.Intents**
    - Intents UI 拡張機能: **com.yourcompanyname.SoupChef.Intentsui**

- 更新プログラム、 **Entitlements.plist**ファイルを**SoupChef**プロジェクト。
    - **アプリ グループ**機能、上記で作成した新しいアプリ グループをグループの設定 (が上記の例で**group.com.yourcompanyname.SoupChef**)。
    - 必ず**SiriKit**を有効にします。

- 更新プログラム、 **Entitlements.plist**ファイルを**SoupChefIntents**プロジェクト。
    - **アプリ グループ**機能、上記で作成した新しいアプリ グループをグループの設定 (が上記の例で**group.com.yourcompanyname.SoupChef**)。

- 最後に、開く**NSUserDefaultsHelper.cs**します。 設定、`AppGroup`新しいアプリ グループの値を変数 (などに設定`group.com.yourcompanyname.SoupChef`)。

### <a name="configuring-the-build-settings"></a>ビルド設定を構成します。

Visual Studio for Mac または Visual Studio 2017 では。

- オプション/プロパティを開き、 **SoupChef**プロジェクト。 **IOS バンドル署名**タブで、設定**署名 Id**自動にし、**プロビジョニング プロファイル**新しいアプリに固有のプロビジョニング プロファイル上で作成しました。

- オプション/プロパティを開き、 **SoupChefIntents**プロジェクト。 **IOS バンドル署名**タブで、設定**署名 Id**に自動と**プロビジョニング プロファイル**作成した新しい Intents 拡張機能に固有のプロファイルをプロビジョニングするには上。

- オプション/プロパティを開き、 **SoupChefIntentsUI**プロジェクト。 **IOS バンドル署名**タブで、設定**署名 Id**に自動と**プロビジョニング プロファイル**新しい Intents UI 拡張機能に固有のプロビジョニング プロファイルします。上記で作成しました。

これらの場所の変更は、アプリが iOS デバイスで実行されます。

### <a name="automatic-provisioning"></a>自動プロビジョニング

使用できる注[自動プロビジョニング](https://docs.microsoft.com/xamarin/ios/get-started/installation/device-provisioning/automatic-provisioning)プロビジョニング タスクを IDE で直接これらの多くを実行します。
ただし、アプリ グループを自動プロビジョニングは設定されません。 手動で構成する必要があります、 **Entitlements.plist**を使用するアプリのグループの名前のファイルは、アプリ グループを作成するには、自動で作成されたアプリ ID ごとにそのアプリ グループを割り当てる Apple Developer Portal を参照してください。プロビジョニングするには、(アプリ、Intents の拡張機能、Intents UI 拡張機能)、新しく作成されたアプリ グループを含めると、ダウンロードしてインストールするには、プロビジョニング プロファイルを再生成します。

## <a name="related-links"></a>関連リンク

- [スープ Chef (Xamarin)](https://developer.xamarin.com/samples/monotouch/ios12/SoupChef/)
- [スープ Chef (Swift)](https://developer.apple.com/documentation/sirikit/accelerating_app_interactions_with_shortcuts?language=objc)
- [SiriKit (Apple)](https://developer.apple.com/sirikit/)
- [Siri ショートカット – WWDC 2018 の概要](https://developer.apple.com/videos/play/wwdc2018/211/)
- [Siri ショートカット – WWDC 2018 で音声の構築](https://developer.apple.com/videos/play/wwdc2018/214/)
- [Siri ウォッチの文字盤 – WWDC 2018 上の Siri のショートカット](https://developer.apple.com/videos/play/wwdc2018/217/)
- [SiriKit – WWDC 2017 の新機能新機能](https://developer.apple.com/videos/play/wwdc2017/214/)
- [Intents アプリ拡張機能 (Apple) を作成します。](https://developer.apple.com/documentation/sirikit/creating_an_intents_app_extension?language=objc)
