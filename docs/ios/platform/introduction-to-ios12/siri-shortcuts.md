---
title: Xamarin の siri ショートカット
description: このドキュメントでは、iOS 12 で Siri ショートカットを使用する方法について説明します。 NSUserActivities、カスタムインテント、音声ショートカットの割り当て、関連するショートカットなどについて説明します。
ms.prod: xamarin
ms.assetid: 86424F79-3A7D-436E-927D-9A3267DA333B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/08/2018
ms.openlocfilehash: 40b7adbed3489d449e583b22fa477287d11bdf42
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031867"
---
# <a name="siri-shortcuts-in-xamarinios"></a>Xamarin の siri ショートカット

[IOS 10](~/ios/platform/sirikit/index.md)では、Apple は SiriKit を導入し、siri と対話するメッセージング、VoIP 通話、支払い、ワークスペース、乗り物予約、写真検索アプリを構築できるようにしました。

[IOS 11](~/ios/platform/introduction-to-ios11/sirikit.md)では、sirikit により、より多くの種類のアプリをサポートし、UI のカスタマイズをより柔軟に行うことができます。

iOS 12 では Siri ショートカットが追加され、すべての種類のアプリが Siri に機能を公開できるようになります。 Siri は、特定のアプリベースのタスクがユーザーに最も関係している場合に学習し、この知識を使用して_ショートカット_を使用して潜在的なアクションを提案します。 ショートカットをタップするか、音声コマンドを使用して呼び出すと、アプリが開かれるかバックグラウンドタスクが実行されます。

ショートカットを使用すると、ユーザーが一般的なタスクを実行する能力を加速させることができます。多くの場合、問題のアプリを開く必要はありません。

## <a name="sample-app-soup-chef"></a>サンプルアプリ: スープ Chef

Siri のショートカットについて理解を深めるには、[スープ Chef](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-soupchef)サンプルアプリを参照してください。 スープ Chef を使用すると、ユーザーは、架空のスープレストランから注文を配置したり、注文履歴を表示したり、Siri と対話することによってスープを並べ替えるときに使用する語句を定義したりできます。

> [!TIP]
> IOS 12 シミュレーターまたはデバイスでスープ Chef をテストする前に、次の2つの設定を有効にします。これは、ショートカットをデバッグするときに便利です。
>
> - **設定**アプリで、**開発者 > の最近のショートカットの表示**を有効にします。
> - **設定**アプリで、**開発者 > ロック画面での寄付の表示**を有効にします。
>
> これらのデバッグ設定を使用すると、iOS のロック画面と検索画面で最近作成された (予測ではなく) ショートカットを簡単に見つけることができます。

サンプルアプリを使用するには:

- スープ Chef サンプルアプリを iOS 12 シミュレーターまたは[デバイス](#testing-on-device)にインストールして実行します。
- 右上にある [ **+** ] ボタンをクリックして、新しい注文を作成します。
- スープの種類を選択し、数量とオプションを指定して、 **[位置の順序]** をタップします。
- **[注文履歴]** 画面で、新しく作成された順序をタップして詳細を表示します。
- [注文の詳細] 画面の下部にある [ **Siri に追加] を**タップします。
- 注文に関連付ける音声語句を記録し、 **[完了]** をタップします。
- スープ Chef を最小化し、Siri を呼び出して、記録したばかりの音声フレーズを使用して順序を再設定します。
- Siri が注文を完了したら、スープ Chef を再度開き、**注文履歴**画面に新しい注文が表示されていることを確認します。

サンプルアプリでは、次の方法を示しています。

- [NSUserActivity ショートカットを使用](#using-an-nsuseractivity-shortcut-to-open-an-app)してアプリを開きます。
- [カスタムのインテントショートカットを使用して、タスクを実行](#using-a-custom-intent-shortcut-to-perform-a-task)します。
- [カスタムインテントのユーザーインターフェイスを提供](#providing-a-user-interface-for-a-custom-intent)します。
- [音声ショートカットを作成](#creating-a-voice-shortcut)します。

## <a name="infoplist-and-entitlementsplist"></a>情報 plist と権利 plist

スープ Chef のコードをより深く掘り下げていく前に、その**情報**を確認**してください**。

### <a name="infoplist"></a>Info.plist

**SoupChef**プロジェクトの**情報の plist**ファイルは、**バンドル識別子**を `com.xamarin.SoupChef`として定義します。 このバンドル識別子は、このドキュメントで後述するインテントおよびインテント UI 拡張機能のバンドル識別子のプレフィックスとして使用されます。

また、このファイルには次の**情報**も含まれています。

```xml
<key>NSUserActivityTypes</key>
<array>
    <string>OrderSoupIntent</string>
    <string>com.xamarin.SoupChef.viewMenu</string>
</array>
```

この `NSUserActivityTypes` のキーと値のペアは、スープ Chef が `OrderSoupIntent`を処理する方法を認識し、"SoupChef" という[`ActivityType`](xref:Foundation.NSUserActivity.ActivityType)を持つ[`NSUserActivity`](xref:Foundation.NSUserActivity)を示します。

拡張機能とは対照的に、アプリ自体に渡されるアクティビティとカスタムインテントは、`AppDelegate` ( [`ContinueUserActivity`](xref:UIKit.UIApplicationDelegate.ContinueUserActivity*)メソッドによって[`UIApplicationDelegate`](xref:UIKit.UIApplicationDelegate)されます。

### <a name="entitlementsplist"></a>Entitlements.plist

**SoupChef**プロジェクトの**権利の plist**ファイルには、次のものが含まれています。

```xml
<key>com.apple.security.application-groups</key>
<array>
    <string>group.com.xamarin.SoupChef</string>
</array>
<key>com.apple.developer.siri</key>
<true/>
```

この構成は、アプリが "SoupChef" アプリグループを使用することを示します。 **SoupChefIntents**アプリ拡張機能では、この同じアプリグループを使用します。これにより、2つのプロジェクトが共有できるようになり[`NSUserDefaults`](xref:Foundation.NSUserDefaults)
データ。

`com.apple.developer.siri` キーは、アプリが Siri と対話することを示します。

> [!NOTE]
> **SoupChef**プロジェクトのビルド構成によって、**カスタム権利**が**plist**に設定されます。

## <a name="using-an-nsuseractivity-shortcut-to-open-an-app"></a>NSUserActivity ショートカットを使用してアプリを開く

特定のコンテンツを表示するアプリを開くショートカットを作成するには、`NSUserActivity` を作成し、ショートカットを開くための画面のビューコントローラーにアタッチします。

### <a name="setting-up-an-nsuseractivity"></a>NSUserActivity の設定

メニュー画面で、`SoupMenuViewController` によって `NSUserActivity` が作成され、ビューコントローラーの[`UserActivity`](xref:UIKit.UIResponder.UserActivity)プロパティに割り当てられます。

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    UserActivity = NSUserActivityHelper.ViewMenuActivity;
}
```

`UserActivity` プロパティを_donates_に設定すると、アクティビティは siri になります。 この寄付から、Siri は、このアクティビティがユーザーに関連するタイミングと場所に関する情報を取得し、将来の提案を改善するために学習します。

`NSUserActivityHelper` は、 **SoupKit**クラスライブラリの**SoupChef**ソリューションに含まれるユーティリティクラスです。 `NSUserActivity` を作成し、Siri と検索に関連するさまざまなプロパティを設定します。

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

特に次の点に注意してください。

- `EligibleForPrediction` を `true` に設定すると、Siri がこのアクティビティを予測してショートカットとして表示できることを示します。
- [`ContentAttributeSet`](xref:Foundation.NSUserActivity.ContentAttributeSet)配列は、iOS の検索結果に `NSUserActivity` を含めるために使用される標準[`CSSearchableItemAttributeSet`](xref:CoreSpotlight.CSSearchableItemAttributeSet)です。
- [`SuggestedInvocationPhrase`](xref:Foundation.NSUserActivity.SuggestedInvocationPhrase)は、ショートカットに語句を割り当てるときに、siri がユーザーに候補として提示する語句です。

### <a name="handling-an-nsuseractivity-shortcut"></a>NSUserActivity ショートカットの処理

ユーザーによって呼び出される `NSUserActivity` ショートカットを処理するには、iOS アプリケーションで `AppDelegate` クラスの `ContinueUserActivity` メソッドをオーバーライドし、渡された `NSUserActivity` オブジェクトの `ActivityType` フィールドに基づいて応答する必要があります。

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

このメソッドは `HandleUserActivity`を呼び出し、メニュー画面にセグエを検索して呼び出します。

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

### <a name="assigning-a-phrase-to-an-nsuseractivity"></a>NSUserActivity に語句を割り当てる

`NSUserActivity`に語句を割り当てるには、iOS**設定**アプリを開き、 **Siri & 検索 > マイショートカット**を選択します。 次に、ショートカット (この場合は "Order ランチ") を選択して、語句を記録します。

Siri を呼び出し、この語句を使用すると、スープ Chef がメニュー画面に表示されます。

## <a name="using-a-custom-intent-shortcut-to-perform-a-task"></a>カスタムインテントショートカットを使用したタスクの実行

### <a name="defining-a-custom-intent"></a>カスタムインテントの定義

ユーザーがアプリに関連する特定のタスクをすばやく完了できるショートカットを提供するには、カスタムインテントを作成します。 カスタムインテントは、ユーザーが実行する必要のあるタスク、そのタスクに関連するパラメーター、およびタスクの実行によって生じる可能性のある応答を表します。 カスタムインテントの定義方法に応じて、アプリを起動するか、バックグラウンドタスクを実行することができます。

Xcode 10 を使用して、カスタムインテントを作成します。 [SoupChef リポジトリ](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef)では、カスタムインテントは**OrderSoupIntentCodeGen**プロジェクトで定義されています。 このプロジェクトを開き、 **OrderSoup**インテントを表示する**プロジェクトナビゲーター**でインテントを選択し**ます**。

次の点に注意してください。

- 目的には**Order**の**カテゴリ**があります。 カスタムインテントに使用できる定義済みのカテゴリがいくつかあります。カスタムインテントによって有効になるタスクに最も近いものを選択します。 これはスープ注文アプリであるため、 **OrderSoupIntent**は**Order**を使用します。
- **[確認]** チェックボックスは、siri がタスクを実行する前に確認を要求する必要があるかどうかを示します。 スープ Chef の**注文スープ**のインテントでは、ユーザーが購入しているため、このオプションは有効になっています。
- . Intentdefinition ファイルの**parameters**セクションでは、ショートカットに関連するパラメーターを定義します。 スープの順序を設定するには、スープ Chef はスープの種類、その数量、および関連するオプションを認識している必要があります。
各パラメーターには型があります。定義済みの型で表すことができないパラメーターは、**カスタム**として設定されます。
- ショートカットの**種類**のインターフェイスでは、ショートカットを提案するときに siri で使用できるさまざまなパラメーターの組み合わせについて説明しています。 関連する**タイトル**と**サブタイトル**セクションでは、ユーザーに推奨されるショートカットを提示するときに siri が使用するメッセージを定義できます。
- [**バックグラウンド実行をサポート**する] チェックボックスをオンにすると、ユーザーの操作を続けるためにアプリを開くことなく実行できるショートカットを選択できます。

### <a name="defining-custom-intent-responses"></a>カスタムインテント応答の定義

**OrderSoup**インテントの下に入れ子になっている**応答**項目は、スープ注文によって発生する可能性のある応答を表します。

**OrderSoup**インテントの応答定義では、次の点に注意してください。

- 応答の**プロパティ**を使用して、ユーザーに返されるメッセージをカスタマイズできます。 **OrderSoup**インテント応答には、**スープ**プロパティと**waitTime**プロパティがあります。
- **応答テンプレート**は、インテントのタスクが完了した後の状態を示すために使用できる、さまざまな成功および失敗メッセージを指定します。
- 成功を示す応答では、 **[成功]** チェックボックスをオンにする必要があります。
- **OrderSoupIntent** success 応答では、**スープ**プロパティと**waitTime**プロパティを使用して、スープの順序の準備がいつ完了するかを説明するわかりやすいメッセージを提供します。

### <a name="generating-code-for-the-custom-intent"></a>カスタムインテントのコードを生成する

このカスタムインテント定義を含む Xcode プロジェクトをビルドすると、Xcode によってコードが生成され、カスタムインテントとその応答をプログラムで操作するために使用できます。

生成されたコードを表示するには:

- **Appdelegate. m**を開きます。
- カスタムインテントのヘッダーファイルにインポートを追加する: `#import "OrderSoupIntent.h"`
- クラスの任意のメソッドで、`OrderSoupIntent`への参照を追加します。
- `OrderSoupIntent` を右クリックし、 **[定義へ移動]** を選択します。
- 新しく開かれたファイル**OrderSoupIntent**を右クリックし、 **[Finder で表示]** を選択します。
- これにより、生成されたコードを含む .h ファイルと m ファイルを含む**ファインダー**ウィンドウが開きます。

生成されるコードは次のとおりです。

- `OrderSoupIntent`: カスタムインテントを表すクラス。
- `OrderSoupIntentHandling` –インテントを実行する必要があるかどうかを確認するために使用されるメソッドと、その意図を実際に実行するメソッドを定義するプロトコル。
- `OrderSoupIntentResponseCode` –さまざまな応答の状態を定義する列挙型です。
- `OrderSoupIntentResponse`: インテントの実行に対する応答を表すクラス。

### <a name="creating-a-binding-to-the-custom-intent"></a>カスタムインテントへのバインドの作成

Xcode によって生成されたコードを Xamarin iOS アプリで使用するC#には、そのバインドを作成します。

#### <a name="creating-a-static-library-and-c-binding-definitions"></a>スタティックライブラリとC#バインディング定義の作成

[SoupChef リポジトリ](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef)で、 **OrderSoupIntentStaticLib**フォルダーを参照して、 **OrderSoupIntentStaticLib**プロジェクトを開きます。これを実行します。

この**Cocoa タッチスタティックライブラリ**プロジェクトには、Xcode によって生成された**OrderSoupIntent**ファイルと**OrderSoupIntent**ファイルが含まれています。

#### <a name="configuring-the-static-library-project-build-settings"></a>スタティックライブラリプロジェクトのビルド設定の構成

Xcode**プロジェクトナビゲーター**で、最上位レベルのプロジェクト**OrderSoupIntentStaticLib**を選択し、**ビルドフェーズ** に移動して > のコンパイル をクリックします。
**OrderSoupIntent** ( **OrderSoupIntent**をインポートする) がここに表示されていることに注意してください。 「**リンクバイナリとライブラリ**」で、**インテント**と**基盤**が含まれていることに注意してください。
これらの設定を適用すると、フレームワークは正しくビルドされます。

#### <a name="building-the-static-library-and-generating-c-bindings-definitions"></a>スタティックライブラリの構築とバインディングC#定義の生成

スタティックライブラリをビルドし、バインドC#の定義を生成するには、次の手順を実行します。

- [マジックペンをインストール](https://docs.microsoft.com/xamarin/cross-platform/macios/binding/objective-sharpie/get-started?context=xamarin/mac#installing-objective-sharpie)します。このツールは、Xcode によって作成された .h ファイルと m ファイルからバインド定義を生成するために使用されます。

- Xcode 10 コマンドラインツールを使用するようにシステムを構成します。

  > [!WARNING]
  > 選択したコマンドラインツールを更新すると、システムにインストールされている Xcode のすべてのバージョンに影響します。 スープ Chef サンプルアプリの使用が完了したら、この設定を元の構成に戻すようにしてください。

  - Xcode で、[ **Xcode] > [> 設定**] の順に選択し、 **[コマンドラインツール]** を、システムで利用可能な最新の Xcode 10 インストールに設定します。

- ターミナルで、 **OrderSoupIntentStaticLib**ディレクトリに `cd` します。

- 次のようにビルドする `make`を入力します。

  - スタティックライブラリ、 **libOrderSoupIntentStaticLib**
  - **Bo**出力ディレクトリで、バインドC#の定義を次に示します。
    - **ApiDefinitions.cs**
    - **StructsAndEnums.cs**

このスタティックライブラリとそれに関連付けられているバインディング定義に依存する**OrderSoupIntentBindings**プロジェクトは、これらの項目を自動的にビルドします。
ただし、上記のプロセスを通じて手動で実行すると、予期したとおりにビルドされます。

スタティックライブラリの作成と、目的のマジックペンを使用したバインディングC#定義の作成の詳細については、「 [IOS の目的 C ライブラリのバインド](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough?tabs=vsmac#creating-a-static-library)」のチュートリアルを参照してください。

#### <a name="creating-a-bindings-library"></a>バインドライブラリの作成

スタティックライブラリとC#バインドの定義を作成した場合、Xamarin の Xcode によって生成されるインテント関連のコードを使用するために必要な残りの部分は、バインドライブラリです。

[スープ Chef リポジトリ](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef)で、 **SoupChef**ファイルを開きます。 特に、このソリューションには**OrderSoupIntentBinding**が含まれています。これは、上で生成されたスタティックライブラリのバインドライブラリです。

特に、このプロジェクトには次のものが含まれることに注意してください。

- **ApiDefinitions.cs** –目標マジックペンによって上に生成され、このプロジェクトに追加されるファイルです。 このファイルの**ビルドアクション**は**Objcbindingapidefinition**に設定されています。
- **StructsAndEnums.cs** –前の手順で生成された別のファイルで、このプロジェクトに追加されます。 このファイルの**ビルドアクション**は**Objcbindingcoresource**に設定されています。
- 上に構築されたスタティックライブラリ**libOrderSoupIntentStaticLib**への**ネイティブ参照**。

> [!NOTE]
> **ApiDefinitions.cs**と**StructsAndEnums.cs**の両方に、`[Watch (5,0), iOS (12,0)]`などの属性が含まれています。 これらの属性は、目標マジックペンによって生成されたものであり、このプロジェクトに必要ではないため、コメントアウトされています。

バインドライブラリの作成の詳細については、「iOS の[目的 C ライブラリのバインド](https://docs.microsoft.com/xamarin/ios/platform/binding-objective-c/walkthrough?tabs=vsmac#create-a-xamarinios-binding-project)」のチュートリアルを参照してください。 C#

**SoupChef**プロジェクトには**OrderSoupIntentBinding**への参照が含まれていることに注意してください。 C#これは、それに含まれるクラス、インターフェイス、列挙型にアクセスできるようになったことを意味します。

- `OrderSoupIntent`
- `OrderSoupIntentHandling`
- `OrderSoupIntentResponse`
- `OrderSoupIntenseResponseCode`

### <a name="adding-the-intentdefinition-file-to-your-solution"></a>ソリューションへの intentdefinition ファイルの追加

C# **SoupChef**ソリューションでは、 **SoupKit**プロジェクトに、アプリとその拡張機能間で共有されるコードが含まれています。 **インテントの intentdefinition**ファイルが**SoupKit**の**基本**ディレクトリに配置されており、**コンテンツ**の**ビルドアクション**があります。 ビルドプロセスは、このファイルをスープ Chef アプリケーションバンドルにコピーします。このバンドルは、アプリが正常に機能するために必要です。

### <a name="donating-an-intent"></a>インテントの寄付

Siri がショートカットを提案するには、まずショートカットが関連するタイミングを理解している必要があります。

Siri にこの理解を与えるために、スープ Chef _donates_は、ユーザーがスープ注文を行うたびに siri を意図しています。 この寄付に基づき、寄付された場合は、寄付された場合は、それに含まれるパラメーターは、将来ショートカットを提案するタイミングを学習します。

**SoupChef**は、`SoupOrderDataManager` クラスを使用して寄付を配置します。
ユーザーのスープ order を配置するために呼び出されると、`PlaceOrder` メソッドは[`DonateInteraction`](xref:Intents.INInteraction.DonateInteraction*)を呼び出します。

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

インテントをフェッチした後は、 [`INInteraction`](xref:Intents.INInteraction)にラップされます。
`INInteraction` には、 [`Identifier`](xref:Intents.INInteraction.Identifier*)が指定されています。
注文の一意の ID に一致します (これは、後で無効になっているインテント寄付を削除するときに役立ちます)。 その後、対話は Siri に寄付されます。

`order.Intent` getter への呼び出しは、その `Quantity`、`Soup`、`Options`、およびイメージを設定することによって、注文を表す `OrderSoupIntent` をフェッチします。また、ユーザーが Siri がインテントと関連付けられる語句を記録するときに、候補として使用する呼び出しフレーズを取得します。:

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

#### <a name="removing-invalid-donations"></a>無効な寄付の削除

Siri が立たないや紛らわしいショートカットの候補を作成しないように、有効ではなくなった寄付を削除することが重要です。

スープ Chef では、[**構成] メニュー**画面を使用して、メニュー項目を使用不可としてマークできます。 Siri では、利用できないメニュー項目を並べ替えるためのショートカットが提案されなくなりました。そのため、使用できなくなったメニュー項目の寄付は、`SoupMenuManager` の `RemoveDonation` 方法によって削除されます。 これを行うには、次のようにします。

- 現在使用できないメニュー項目に関連付けられている注文を検索しています。
- 識別子を取得しています。
- 同じ識別子を持つ相互作用を削除しています。

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

### <a name="creating-an-intents-extension"></a>インテント拡張の作成

Siri がインテントを呼び出すと実行されるコードは、インテント拡張に配置されます。これは、スープ Chef などの既存の Xamarin iOS アプリと同じソリューションに新しいプロジェクトとして追加できます。 **SoupChef**ソリューションでは、拡張機能は**SoupChefIntents**と呼ばれています。

#### <a name="soupchefintents-infoplist-and-entitlementsplist"></a>SoupChefIntents – plist と権利の plist

##### <a name="soupchefintents-infoplist"></a>SoupChefIntents – Info. plist

**SoupChefIntents**プロジェクトの**情報 Plist**は、**バンドル識別子**を `com.xamarin.SoupChef.SoupChefIntents`として定義します。

また、このファイルには次の**情報**も含まれています。

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

上記の情報を次に示します **。**

- `IntentsRestrictedWhileLocked` には、デバイスのロックが解除されている場合にのみ処理する必要があるインテントが一覧表示されます。
- `IntentsSupported` は、この拡張機能によって処理されるインテントの一覧を表示します。
- `NSExtensionPointIdentifier` では、アプリ拡張機能の種類を指定します (詳細については、 [Apple のドキュメント](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212-SW15)を参照してください)。
- `NSExtensionPrincipalClass` は、この拡張機能でサポートされているインテントを処理するために使用する必要があるクラスを指定します。

##### <a name="soupchefintents-entitlementsplist"></a>SoupChefIntents –権利

**SoupChefIntents**プロジェクトの**権利 plist**には、**アプリグループ**の機能があります。 この機能は、 **SoupChef**プロジェクトと同じアプリグループを使用するように構成されています。

```xml
<key>com.apple.security.application-groups</key>
<array>
    <string>group.com.xamarin.SoupChef</string>
</array>
```

スープ Chef は `NSUserDefaults`でデータを永続化します。 アプリとアプリ拡張機能の間でデータを共有するために、それらのアプリケーションは、その**権利の plist**ファイルで同じアプリグループを参照します。

> [!NOTE]
> **SoupChefIntents**プロジェクトのビルド構成によって、**カスタム権利**が**plist**に設定されます。

#### <a name="handling-an-ordersoupintent-background-task"></a>OrderSoupIntent バックグラウンドタスクの処理

インテント拡張は、カスタムインテントに基づいて、ショートカットに必要なバックグラウンドタスクを実行します。

Siri は、`IntentHandler` クラスの[`GetHandler`](xref:Intents.INExtension.GetHandler*)メソッド (`NSExtensionPrincipalClass`として定義されている) を**呼び出して、** `OrderSoupIntentHandling`を拡張するクラスのインスタンスを取得します。これは、`OrderSoupIntent`を処理するために使用できます。

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

共有コードの**SoupKit**プロジェクトで定義されている `OrderSoupIntentHandler`は、次の2つの重要なメソッドを実装します。

- `ConfirmOrderSoup` –インテントに関連付けられているタスクを実際に実行する必要があるかどうかを確認します。
- `HandleOrderSoup` –スープの順序を配置し、渡された完了ハンドラーを呼び出してユーザーに応答します。

#### <a name="handling-an-ordersoupintent-that-opens-the-app"></a>アプリを開く OrderSoupIntent の処理

アプリでは、バックグラウンドで実行されないインテントを正しく処理する必要があります。
これらは、`AppDelegate`の `ContinueUserActivity` メソッドで `NSUserActivity` ショートカットと同じように処理されます。

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

## <a name="providing-a-user-interface-for-a-custom-intent"></a>カスタムインテントのユーザーインターフェイスの提供

インテント UI 拡張は、インテント拡張機能のカスタムユーザーインターフェイスを提供します。 **SoupChef**ソリューションでは、 **SoupChefIntentsUI**は**SoupChefIntents**のインターフェイスを提供するインテント UI 拡張機能です。

### <a name="soupchefintentsui--infoplist-and-entitlementsplist"></a>SoupChefIntentsUI – plist と権利の plist

#### <a name="soupchefintentsui-infoplist"></a>SoupChefIntentsUI – Info. plist

**SoupChefIntentsUI**プロジェクトの**情報 Plist**は、**バンドル識別子**を `com.xamarin.SoupChef.SoupChefIntentsui`として定義します。

また、このファイルには次の**情報**も含まれています。

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

上記の情報を次に示します **。**

- `IntentsSupported` は、`OrderSoupIntent` がこのインテント UI 拡張機能によって処理されることを示します。
- `NSExtensionPointIdentifier` では、アプリ拡張機能の種類を指定します (詳細については、 [Apple のドキュメント](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212-SW15)を参照してください)。
- `NSExtensionMainStoryboard` この拡張機能のプライマリインターフェイスを定義するストーリーボードを指定します

#### <a name="soupchefintentsui-entitlementsplist"></a>SoupChefIntentsUI –権利

**SoupChefIntentsUI**プロジェクトには、**権利の plist**ファイルは必要ありません。

### <a name="creating-the-user-interface"></a>ユーザー インターフェイスの作成

**SoupChefIntentsUI**の**情報**は、`NSExtensionMainStoryboard` キーを `MainInterface`に設定するため、 **mainui**拡張機能のインターフェイスを定義します。

このストーリーボードには、 **Intentviewcontroller**型の単一のビューコントローラーがあります。 2つのビューを参照します。

- **invoiceView**(型 `InvoiceView`)
- **confirmationView**(型 `ConfirmOrderView`)

> [!NOTE]
> **InvoiceView**と**confirmationView**のインターフェイスは、**メインストーリーボード**でセカンダリビューとして定義されています。 Visual Studio for Mac と Visual Studio 2017 の iOS デザイナーでは、セカンダリビューの表示や編集はサポートされていません。これを行うには、Xcode の Interface Builder で**メインのストーリーボード**を開きます。

`IntentViewController` は[`IINUIHostedViewControlling`](xref:IntentsUI.IINUIHostedViewControlling)を実装します
Siri インテントを操作するときにカスタムインターフェイスを提供するために使用されるインターフェイス。 [`ConfigureView`](xref:IntentsUI.INUIHostedViewControlling_Extensions.ConfigureView*)
メソッドは、相互作用が確認されている ([`INIntentHandlingStatus.Ready`](xref:Intents.INIntentHandlingStatus)) か正常に実行されたか ([`INIntentHandlingStatus.Success`](xref:Intents.INIntentHandlingStatus)) に応じて、確認または請求書を表示するインターフェイスをカスタマイズするために呼び出されます。

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
> `ConfigureView` 方法の詳細については、Apple の WWDC 2017 プレゼンテーションを視聴してください。 [SiriKit の新機能](https://developer.apple.com/videos/play/wwdc2017/214/)については、こちらをご覧ください。

## <a name="creating-a-voice-shortcut"></a>音声ショートカットの作成

スープ Chef には、各注文に音声ショートカットを割り当てるインターフェイスが用意されており、Siri でスープを注文できます。 実際、音声ショートカットの記録と割り当てに使用されるインターフェイスは iOS によって提供され、カスタムコードはほとんど必要ありません。

`OrderDetailViewController`では、ユーザーがテーブルの **[Siri に追加]** 行をタップすると、 [`RowSelected`](xref:UIKit.UITableViewSource.RowSelected*)メソッドによって、音声ショートカットを追加または編集するための画面が表示されます。

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

現在表示されている順序に対して既存の音声ショートカットが存在するかどうかに基づいて、`RowSelected` は[`INUIEditVoiceShortcutViewController`](xref:IntentsUI.INUIEditVoiceShortcutViewController)または[`INUIAddVoiceShortcutViewController`](xref:IntentsUI.INUIAddVoiceShortcutViewController)型のビューコントローラーを提示します。
どちらの場合も、`OrderDetailViewController` 自身をビューコントローラーの `Delegate`として設定します。これが実装されるのはそのためです[`IINUIAddVoiceShortcutViewControllerDelegate`](xref:IntentsUI.IINUIAddVoiceShortcutViewControllerDelegate)
および[`IINUIEditVoiceShortcutViewControllerDelegate`](xref:IntentsUI.IINUIEditVoiceShortcutViewControllerDelegate)ます。

## <a name="testing-on-device"></a>デバイスでのテスト

デバイスでスープ Chef を実行するには、次の手順に従います。 [自動プロビジョニングについてもメモ](#automatic-provisioning)をお読みください。

### <a name="app-group-app-ids-provisioning-profiles"></a>アプリグループ、アプリ Id、プロビジョニングプロファイル

[Apple Developer Portal](https://developer.apple.com/)の **[Certificates, IDs & Profiles]** セクションで、次の手順を実行します。

- スープ Chef アプリとその拡張機能との間でデータを共有するためのアプリグループを作成します。 たとえば、 **SoupChef**のようになります。

- アプリケーション Id を3つ作成します。1つはアプリ自体用で、もう1つはインテント拡張用、もう1つはインテント UI 拡張機能用です。 (例:

  - アプリ: **.Com SoupChef**
    - このアプリ ID には、SiriKit と**アプリグループ**の機能を割り当てます。

  - インテントの拡張子: **SoupChef. インテント**
    - このアプリ ID に、**アプリグループ**の機能を割り当てます。

  - インテント UI 拡張機能: **SoupChef. Intentsui**
    - このアプリ ID には特別な機能は必要ありません。

- 上記のアプリ Id を作成したら、アプリに割り当てられている**アプリグループ**機能とインテント拡張機能を編集して、上で作成した特定のアプリグループを指定します。

- 新しいアプリ Id ごとに1つずつ、3つの新しい開発プロビジョニングプロファイルを作成します。

- これらのプロビジョニングプロファイルをダウンロードし、それぞれをダブルクリックしてインストールします。 Visual Studio for Mac または Visual Studio 2017 が既に実行されている場合は、再起動して、新しいプロビジョニングプロファイルが登録されていることを確認します。

### <a name="editing-infoplist-entitlementsplist-and-source-code"></a>情報 plist、権利の plist、およびソースコードの編集

Visual Studio for Mac または Visual Studio 2017 で、次の操作を行います。

- ソリューション内のさまざまな**情報の plist**ファイルを更新します。 アプリ、インテント拡張、インテント UI 拡張**バンドル識別子**を、上記で定義したアプリ id に設定します。

  - アプリ: **.Com SoupChef**
  - インテントの拡張子: **SoupChef. インテント**
  - インテント UI 拡張機能: **SoupChef. Intentsui**

- **SoupChef**プロジェクトの**権利の plist**ファイルを更新します。
  - **アプリグループ**の機能については、上で作成した新しいアプリグループにグループを設定します (上記の例では、 **SoupChef**)。
  - **Sirikit**が有効になっていることを確認します。

- **SoupChefIntents**プロジェクトの**権利の plist**ファイルを更新します。
  - **アプリグループ**の機能については、上で作成した新しいアプリグループにグループを設定します (上記の例では、 **SoupChef**)。

- 最後に、 **NSUserDefaultsHelper.cs**を開きます。 `AppGroup` 変数を新しいアプリグループの値に設定します (たとえば、`group.com.yourcompanyname.SoupChef`に設定します)。

### <a name="configuring-the-build-settings"></a>ビルド設定の構成

Visual Studio for Mac または Visual Studio 2017 の場合:

- **SoupChef**プロジェクトのオプションとプロパティを開きます。 **IOS バンドル署名** タブで、**署名 id** を 自動 に設定し、前の手順で作成した新しいアプリ固有のプロビジョニングプロファイルに **プロビジョニングプロファイル** を設定します。

- **SoupChefIntents**プロジェクトのオプションとプロパティを開きます。 **IOS バンドル署名** タブで、**署名 id** を 自動 に設定し、**プロビジョニングプロファイル** を、上記で作成した新しいインテント拡張機能固有のプロビジョニングプロファイルに設定します。

- **SoupChefIntentsUI**プロジェクトのオプションとプロパティを開きます。 **IOS バンドル署名** タブで、**署名 id** を 自動 に設定し、**プロビジョニングプロファイル** を、上記で作成した新しいインテント UI 拡張機能固有のプロビジョニングプロファイルに設定します。

これらの変更を適用すると、アプリは iOS デバイスで実行されます。

### <a name="automatic-provisioning"></a>自動プロビジョニング

[自動プロビジョニング](https://docs.microsoft.com/xamarin/ios/get-started/installation/device-provisioning/automatic-provisioning)を使用すると、これらのプロビジョニングタスクの多くを IDE で直接行うことができます。
ただし、自動プロビジョニングでは、アプリグループは設定されません。 使用するアプリグループの名前で、権利の plist ファイルを手動で構成する必要があり**ます。** Apple Developer Portal にアクセスしてアプリグループを作成し、そのアプリグループを自動プロビジョニングによって作成された各アプリ ID に割り当て、再生成します。新しく作成されたアプリグループを含むプロビジョニングプロファイル (アプリ、インテント拡張、インテント UI 拡張)。これらをダウンロードしてインストールします。

## <a name="related-links"></a>関連リンク

- [スープ Chef (Xamarin)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-soupchef)
- [スープ Chef (Swift)](https://developer.apple.com/documentation/sirikit/accelerating_app_interactions_with_shortcuts?language=objc)
- [SiriKit (Apple)](https://developer.apple.com/sirikit/)
- [Siri ショートカットの概要– WWDC 2018](https://developer.apple.com/videos/play/wwdc2018/211/)
- [Siri ショートカットを使用した音声用のビルド– WWDC 2018](https://developer.apple.com/videos/play/wwdc2018/214/)
- [Siri ウォッチ顔の siri ショートカット– WWDC 2018](https://developer.apple.com/videos/play/wwdc2018/217/)
- [SiriKit の新機能– WWDC 2017](https://developer.apple.com/videos/play/wwdc2017/214/)
- [インテントアプリ拡張機能 (Apple) を作成する](https://developer.apple.com/documentation/sirikit/creating_an_intents_app_extension?language=objc)
