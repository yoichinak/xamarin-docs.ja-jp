---
title: Xamarin の iOS 拡張機能
description: このドキュメントでは、通知センター内などの標準コンテキストで iOS によって提供されるウィジェットである拡張機能について説明します。 拡張機能を作成し、親アプリから通信する方法について説明します。
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 0cf44a05f8b40a07dcc099d5789171f4a234a0c2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032569"
---
# <a name="ios-extensions-in-xamarinios"></a>Xamarin の iOS 拡張機能

> [!VIDEO https://youtube.com/embed/Sd0-ch9Udmk]

**IOS ビデオでの拡張機能の作成**

IOS 8 で導入された拡張機能は、 **Notification Center**内などの標準コンテキスト内で ios によって提供される特殊な `UIViewControllers` であり、ユーザーが特殊な入力やその他のコンテキストを実行するために要求するカスタムキーボードの種類として、拡張機能が特殊効果フィルターを提供できる写真を編集する。

すべての拡張機能は、コンテナーアプリと共にインストールされ (両方の要素が64ビット統合 Api を使用して作成されます)、ホストアプリの特定の拡張ポイントからアクティブ化されます。 また、これらは既存のシステム関数の補完として使用されるため、高いパフォーマンス、リーン、堅牢性を備えている必要があります。 

## <a name="extension-points"></a>拡張ポイント

|[種類]|説明|拡張ポイント|ホストアプリ|
|--- |--- |--- |--- |
|操作|特定のメディアの種類の特殊なエディターまたはビューアー|`com.apple.ui-services`|どれでも可|
|ドキュメントプロバイダー|アプリがリモートドキュメントストアを使用できるようにします|`com.apple.fileprovider-ui`|[UIDocumentPickerViewController](xref:UIKit.UIDocumentPickerViewController)を使用するアプリ|
|キーボード|代替キーボード|`com.apple.keyboard-service`|どれでも可|
|写真の編集|写真の操作と編集|`com.apple.photo-editing`|Photos アプリエディター|
|共有|ソーシャルネットワークやメッセージングサービスなどを使用してデータを共有します。|`com.apple.share-services`|どれでも可|
|今日|今日の画面または通知センターに表示される "ウィジェット"|`com.apple.widget-extensions`|今日と通知センター|

IOS 10 で[追加の拡張ポイント](~/ios/platform/introduction-to-ios10/index.md#app-extensions)が追加されました。

## <a name="limitations"></a>制限事項

拡張機能にはいくつかの制限がありますが、その中にはすべての種類に共通するものがあります (たとえば、拡張機能の種類によってカメラやマイクにアクセスできない場合)。他の種類の拡張機能には、使用法に関する制限があります (カスタムキーボードなど)。は、パスワードなどのセキュリティで保護されたデータ入力フィールドには使用できません。 

一般的な制限事項は次のとおりです。

- [Health Kit](~/ios/platform/healthkit.md) および [Event Kit UI](~/ios/platform/eventkit.md)フレームワークは使用できません
- 拡張機能は[拡張バックグラウンドモード](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md)を使用できません
- 拡張機能はデバイスのカメラやマイクにアクセスできません (ただし、既存のメディアファイルにアクセスする可能性があります)。
- 拡張機能は Air Drop データを受信できません (ただし、Air Drop でデータを送信することはできます)
- [UIActionSheet](xref:UIKit.UIActionSheet) と [UIAlertView](xref:UIKit.UIAlertView) は使用できません。拡張機能は [UIAlertController](xref:UIKit.UIAlertController) を使用する必要があります
- [UIApplication](xref:UIKit.UIApplication) のいくつかのメンバーが使用できません: [UIApplication.SharedApplication](xref:UIKit.UIApplication.SharedApplication) 、[UIApplication.OpenUrl](xref:UIKit.UIApplication.OpenUrl(Foundation.NSUrl))、[UIApplication.BeginIgnoringInteractionEvents](xref:UIKit.UIApplication.BeginIgnoringInteractionEvents) と [UIApplication.EndIgnoringInteractionEvents](xref:UIKit.UIApplication.EndIgnoringInteractionEvents)。
- iOS では、現在の拡張機能に 16 MB のメモリ使用量制限が適用されます。
- 既定では、キーボード拡張機能はネットワークにアクセスできません。 これはデバイスのデバッグに影響します (この制限はシミュレーターには適用されません)。そのため、Xamarin.iOS ではデバッグのためにネットワークアクセスが必要になります。 プロジェクトの情報を `Yes` に `Requests Open Access` 設定することによって、ネットワークへのアクセスを要求することができます。 キーボード拡張機能の制限の詳細については、Apple の[カスタムキーボードガイド](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html)を参照してください。

個々の制限事項については、「Apple の[アプリ拡張機能のプログラミングガイド](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/)」を参照してください。

## <a name="distributing-installing-and-running-extensions"></a>拡張機能の配布、インストール、および実行

拡張機能は、コンテナーアプリ内から配布され、アプリストアを介して送信および配布されます。 アプリと共に配布される拡張機能はその時点でインストールされますが、ユーザーは各拡張機能を明示的に有効にする必要があります。 さまざまな種類の拡張機能は、さまざまな方法で有効になります。いくつかの方法で、ユーザーは**設定**アプリに移動し、そこから設定アプリを有効にする必要があります。 他のユーザーは、写真の送信時に共有拡張機能を有効にするなど、使用する時点で有効になります。 

拡張機能を使用する (ユーザーが拡張ポイントを検出する) アプリは、実行時に拡張機能をホストするアプリであるため、**ホストアプリ**と呼ばれます。 拡張機能をインストールするアプリは、インストール時に拡張機能が含まれていたアプリであるため、**コンテナーアプリ**です。  

通常、コンテナーアプリでは拡張機能について説明し、ユーザーに対して有効にするプロセスを説明します。

## <a name="extension-lifecycle"></a>拡張機能のライフサイクル

拡張機能は、UI の複数の画面を表示する1つの[Uiviewcontroller](xref:UIKit.UIViewController)またはより複雑な拡張機能として簡単に使用できます。 ユーザーが_拡張ポイント_(イメージを共有するときなど) を検出すると、その拡張ポイントに登録されている拡張機能から選択できるようになります。 

アプリの拡張機能のいずれかを選択すると、その `UIViewController` がインスタンス化され、通常のビューコントローラーのライフサイクルが開始されます。 ただし、中断されているが、通常はユーザーが操作を終了したときに終了しない通常のアプリとは異なり、拡張機能が読み込まれ、実行された後、繰り返し終了します。

拡張機能は、 [NSExtensionContext](xref:Foundation.NSExtensionContext)オブジェクトを介してホストアプリと通信できます。 一部の拡張機能には、結果を使用して非同期コールバックを受信する操作が含まれています。 これらのコールバックはバックグラウンドスレッドで実行され、拡張機能はこれを考慮に入れる必要があります。たとえば、ユーザーインターフェイスを更新する場合は、 [NSObject](xref:Foundation.NSObject.InvokeOnMainThread*)を使用します。 詳細については、後述の「[ホストアプリとの通信](#communicating-with-the-host-app)」を参照してください。

既定では、拡張機能とそのコンテナーアプリは、一緒にインストールされていても通信できません。 場合によっては、コンテナーアプリが、拡張機能のインストール後に提供される、空の "出荷" コンテナーであることがあります。 ただし、状況によっては、コンテナーアプリと拡張機能が共通領域からリソースを共有する場合があります。 さらに、**今日の拡張機能**では、URL を開くようにコンテナーアプリを要求できます。 この動作は、[イベントカウントダウンウィジェット](https://github.com/xamarin/ios-samples/tree/master/intro-to-extensions)で表示されます。

## <a name="creating-an-extension"></a>拡張機能の作成

拡張機能 (およびそのコンテナーアプリ) は、64ビットのバイナリであり、Xamarin の[統合 api](~/cross-platform/macios/unified/index.md)を使用して構築されている必要があります。 拡張機能を開発する場合、ソリューションには少なくとも2つのプロジェクトが含まれます。コンテナーアプリと、コンテナーが提供する各拡張機能の1つのプロジェクトです。

### <a name="container-app-project-requirements"></a>コンテナーアプリプロジェクトの要件

拡張機能のインストールに使用されるコンテナーアプリには、次の要件があります。

- 拡張機能プロジェクトへの参照を保持する必要があります。   
- 拡張機能をインストールする方法を提供するだけでなく、完全なアプリケーション (正常に起動して実行できる必要があります) である必要があります。 
- このファイルには、拡張プロジェクトのバンドル識別子の基礎となるバンドル識別子が必要です (詳細については、以下のセクションを参照してください)。

### <a name="extension-project-requirements"></a>拡張機能プロジェクトの要件

さらに、拡張機能のプロジェクトには次の要件があります。

- コンテナーアプリのバンドル識別子で始まるバンドル識別子が必要です。 たとえば、コンテナーアプリのに `com.myCompany.ContainerApp` のバンドル識別子が含まれている場合、拡張機能の識別子が `com.myCompany.ContainerApp.MyExtension` 可能性があります。 

  ![](extensions-images/bundleidentifiers.png) 
- `Info.plist` ファイル内の適切な値 (**今日**の Notification Center ウィジェットの `com.apple.widget-extension` など) を使用して、キー `NSExtensionPointIdentifier`を定義する必要があります。
- また、適切な値を使用して、`Info.plist` ファイル内の `NSExtensionMainStoryboard` キーまたは `NSExtensionPrincipalClass` キーの*どちらか*を定義する必要があります。
  - `NSExtensionMainStoryboard` キーを使用して、拡張機能のメイン UI を表示するストーリーボードの名前を指定します (マイナス `.storyboard`)。 たとえば、`Main.storyboard` ファイルの `Main` です。
  - `NSExtensionPrincipalClass` キーを使用して、拡張機能の開始時に初期化されるクラスを指定します。 値は、`UIViewController` の**レジスタ**値と一致している必要があります。 

  ![](extensions-images/registerandprincipalclass.png)

特定の種類の拡張機能には、追加の要件がある場合があります。 たとえば、**今日**または**Notification Center**の拡張機能のプリンシパルクラスでは、 [INCWidgetProviding](xref:NotificationCenter.INCWidgetProviding)を実装する必要があります。

> [!IMPORTANT]
> Visual Studio for Mac によって提供される拡張機能テンプレートの1つを使用してプロジェクトを開始すると、これらの要件がテンプレートによって自動的に提供され、満たされます。

## <a name="walkthrough"></a>チュートリアル 

次のチュートリアルでは、**今日**の日付と残り日数を計算するサンプルのウィジェットを作成します。

[![](extensions-images/carpediemscreenshot-sm.png "An example Today widget that calculates the day and number of days remaining in the year")](extensions-images/carpediemscreenshot.png#lightbox)

### <a name="creating-the-solution"></a>ソリューションの作成

必要なソリューションを作成するには、次の手順を実行します。

1. まず、新しい iOS、**シングルビューアプリ**プロジェクトを作成し、 **[次へ]** ボタンをクリックします。 

    [![](extensions-images/today01.png "First, create a new iOS, Single View App project and click the Next button")](extensions-images/today01.png#lightbox)
2. プロジェクト `TodayContainer` を呼び出し、 **[次へ]** ボタンをクリックします。 

    [![](extensions-images/today02.png "Call the project TodayContainer and click the Next button")](extensions-images/today02.png#lightbox)
3. **プロジェクト名**と**SolutionName**を確認し、 **[作成]** ボタンをクリックしてソリューションを作成します。 

    [![](extensions-images/today03.png "Verify the Project Name and SolutionName and click the Create button to create the solution")](extensions-images/today03.png#lightbox)
4. 次に、**ソリューションエクスプローラー**でソリューションを右クリックし、**今日の拡張**機能テンプレートから新しい**iOS 拡張機能**プロジェクトを追加します。 

    [![](extensions-images/today04.png "Next, in the Solution Explorer, right-click on the Solution and add a new iOS Extension project from the Today Extension template")](extensions-images/today04.png#lightbox)
5. プロジェクト `DaysRemaining` を呼び出し、 **[次へ]** ボタンをクリックします。 

    [![](extensions-images/today05.png "Call the project DaysRemaining and click the Next button")](extensions-images/today05.png#lightbox)
6. プロジェクトを確認し、 **[作成]** ボタンをクリックして作成します。 

    [![](extensions-images/today06.png "Review the project and click the Create button to create it")](extensions-images/today06.png#lightbox)

次に示すように、結果として得られるソリューションには2つのプロジェクトが必要になります。

[![](extensions-images/today07.png "The resulting Solution should now have two projects, as shown here")](extensions-images/today07.png#lightbox)

### <a name="creating-the-extension-user-interface"></a>拡張機能のユーザーインターフェイスの作成

次に、**今日**のウィジェットのインターフェイスを設計する必要があります。 これは、ストーリーボードを使用するか、コードで UI を作成することによって行うことができます。 両方の方法について詳しく説明します。

#### <a name="using-storyboards"></a>ストーリーボードの使用

ストーリーボードを使用して UI を作成するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、拡張機能プロジェクトの `Main.storyboard` ファイルをダブルクリックして、編集用に開きます。 

    [![](extensions-images/today08.png "Double-click the Extension projects Main.storyboard file to open it for editing")](extensions-images/today08.png#lightbox)
2. テンプレートによって UI に自動的に追加されたラベルを選択し、 **[プロパティエクスプローラー]** の **[ウィジェット]** タブに `TodayMessage`**名前**を付けます。 

    [![](extensions-images/today09.png "Select the Label that was automatically added to the UI by template and give it the Name TodayMessage in the Widget tab of the Properties Explorer")](extensions-images/today09.png#lightbox)
3. 変更内容をストーリーボードに保存します。

#### <a name="using-code"></a>コードの使用

コードで UI をビルドするには、次の手順を実行します。 

1. **ソリューションエクスプローラー**で、 **DaysRemaining**プロジェクトを選択し、新しいクラスを追加して、`CodeBasedViewController` を呼び出します。 

    [![](extensions-images/code01.png "Aelect the DaysRemaining project, add a new class and call it CodeBasedViewController")](extensions-images/code01.png#lightbox)
2. ここでも、**ソリューションエクスプローラー**で、拡張機能の `Info.plist` ファイルをダブルクリックして編集用に開きます。 

    [![](extensions-images/code02.png "Double-click Extensions Info.plist file to open it for editing")](extensions-images/code02.png#lightbox)
3. (画面の下部から)**ソースビュー**を選択し、[`NSExtension`] ノードを開きます。 

    [![](extensions-images/code03.png "Select the Source View from the bottom of the screen and open the NSExtension node")](extensions-images/code03.png#lightbox)
4. `NSExtensionMainStoryboard` キーを削除し、`CodeBasedViewController`値を持つ `NSExtensionPrincipalClass` を追加します。 

    [![](extensions-images/code04.png "Remove the NSExtensionMainStoryboard key and add a NSExtensionPrincipalClass with the value CodeBasedViewController")](extensions-images/code04.png#lightbox)
5. 変更内容を保存します。

次に、`CodeBasedViewController.cs` ファイルを編集し、次のように表示します。

```csharp
using System;
using Foundation;
using UIKit;
using NotificationCenter;
using CoreGraphics;

namespace DaysRemaining
{
  [Register("CodeBasedViewController")]
  public class CodeBasedViewController : UIViewController, INCWidgetProviding
  {
    public CodeBasedViewController ()
    {
    }

    public override void ViewDidLoad ()
    {
      base.ViewDidLoad ();

      // Add label to view
      var TodayMessage = new UILabel (new CGRect (0, 0, View.Frame.Width, View.Frame.Height)) {
        TextAlignment = UITextAlignment.Center
      };

      View.AddSubview (TodayMessage);

      // Insert code to power extension here...

    }
  }
}
```

`[Register("CodeBasedViewController")]` が、上記の `NSExtensionPrincipalClass` に指定した値と一致することに注意してください。

### <a name="coding-the-extension"></a>拡張機能のコーディング

ユーザーインターフェイスを作成した状態で、(上のユーザーインターフェイスの作成に使用した方法に基づいた) `TodayViewController.cs` または `CodeBasedViewController.cs` ファイルを開き、 **ViewDidLoad**メソッドを変更し、次のように表示します。

```csharp
public override void ViewDidLoad ()
{
  base.ViewDidLoad ();

  // Calculate the values
  var dayOfYear = DateTime.Now.DayOfYear;
  var leapYearExtra = DateTime.IsLeapYear (DateTime.Now.Year) ? 1 : 0;
  var daysRemaining = 365 + leapYearExtra - dayOfYear;

  // Display the message
  if (daysRemaining == 1) {
    TodayMessage.Text = String.Format ("Today is day {0}. There is one day remaining in the year.", dayOfYear);
  } else {
    TodayMessage.Text = String.Format ("Today is day {0}. There are {1} days remaining in the year.", dayOfYear, daysRemaining);
  }
}
```

コードベースのユーザーインターフェイスメソッドを使用している場合は、`// Insert code to power extension here...` コメントを上記の新しいコードに置き換えます。 基本実装を呼び出した後 (およびコードベースバージョンのラベルを挿入した後)、このコードは単純な計算を実行して、その年の通算日と残りの日数を取得します。 次に、UI デザインで作成したラベル (`TodayMessage`) にメッセージが表示されます。

このプロセスは、アプリの通常の作成プロセスに似ていることに注意してください。 拡張機能の `UIViewController` には、アプリのビューコントローラーと同じライフサイクルがあります。ただし、拡張機能にはバックグラウンドモードがないため、ユーザーが使用を終了したときに中断されることはありません。 代わりに、拡張機能が繰り返し初期化され、必要に応じて割り当てが解除されます。

### <a name="creating-the-container-app-user-interface"></a>コンテナーアプリのユーザーインターフェイスを作成する

このチュートリアルでは、コンテナーアプリは、拡張機能を出荷およびインストールするためのメソッドとしてのみ使用され、独自の機能は提供されません。 TodayContainer の `Main.storyboard` ファイルを編集して、拡張機能の関数とそのインストール方法を定義するテキストを追加します。

[![](extensions-images/today10.png "Edit the TodayContainers Main.storyboard file and add some text defining the Extensions function and how to install it")](extensions-images/today10.png#lightbox)

変更内容をストーリーボードに保存します。

### <a name="testing-the-extension"></a>拡張機能のテスト

IOS シミュレーターで拡張機能をテストするには、 **TodayContainer**アプリを実行します。 コンテナーのメインビューが表示されます。

[![](extensions-images/run01.png "The containers main view will be displayed")](extensions-images/run01.png#lightbox)

次に、シミュレーターの **[ホーム]** ボタンをクリックし、画面の上部から下方向にスワイプして**通知センター**を開き、 **[今日]** タブを選択して **[編集]** ボタンをクリックします。

[![](extensions-images/run02.png "Hit the Home button in the Simulator, swipe down from the top of the screen to open the Notification Center, select the Today tab and click the Edit button")](extensions-images/run02.png#lightbox)

**DaysRemaining**拡張機能を**Today**ビューに追加し、 **[Done]** \ (完了 \) ボタンをクリックします。

[![](extensions-images/run03.png "Add the DaysRemaining Extension to the Today view and click the Done button")](extensions-images/run03.png#lightbox)

新しいウィジェットが**今日**のビューに追加され、結果が表示されます。

[![](extensions-images/run04.png "The new widget will be added to the Today view and the results will be displayed")](extensions-images/run04.png#lightbox)

## <a name="communicating-with-the-host-app"></a>ホストアプリとの通信

上で作成した今日の拡張機能の例は、ホストアプリ (**今日**画面) とは通信しません。 存在する場合は、`TodayViewController` クラスまたは `CodeBasedViewController` クラスの[Extensioncontext](xref:Foundation.NSExtensionContext)プロパティを使用します。 

ホストアプリからデータを受信する拡張機能の場合、データは、拡張機能の `UIViewController` の[Extensioncontext](xref:Foundation.NSExtensionContext)の[InputItems](xref:Foundation.NSExtensionContext.InputItems)プロパティに格納されている[NSExtensionItem](xref:Foundation.NSExtensionItem)オブジェクトの配列の形式になります。

写真編集拡張機能などの他の拡張機能は、ユーザーが使用状況を完了またはキャンセルすることを区別できます。 これは、 [Extensioncontext](xref:Foundation.NSExtensionContext)プロパティの[CompleteRequest](xref:Foundation.NSExtensionContext.CompleteRequest*)および[cancelrequest](xref:Foundation.NSExtensionContext.CancelRequest*)メソッドを介してホストアプリに返されます。

詳細については、「Apple の[アプリ拡張機能のプログラミングガイド](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1)」を参照してください。

## <a name="communicating-with-the-parent-app"></a>親アプリとの通信

アプリ グループを使用すると、さまざまなアプリケーション (またはアプリケーションとその拡張機能) から共有ファイルの保存場所にアクセスできます。 アプリ グループは次のようなデータに使用できます。

- [Apple Watch 設定](~/ios/watchos/app-fundamentals/settings.md)。
- [共有 NSUserDefaults](~/ios/app-fundamentals/user-defaults.md)。
- [共有ファイル](~/ios/watchos/app-fundamentals/parent-app.md#files)。

詳細については、機能の使用**に**関するドキュメントの「[アプリグループ](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)」セクションを参照してください。

## <a name="mobilecoreservices"></a>MobileCoreServices

拡張機能を使用する場合は、Uniform Type Identifier (UTI) を使用して、アプリ、他のアプリ、サービスの間で交換されるデータを作成および操作します。

`MobileCoreServices.UTType` の静的クラスは、Apple の `kUTType...` 定義に関連する次のヘルパープロパティを定義します。

- `kUTTypeAlembic` - `Alembic`
- `kUTTypeAliasFile` - `AliasFile`
- `kUTTypeAliasRecord` - `AliasRecord`
- `kUTTypeAppleICNS` - `AppleICNS`
- `kUTTypeAppleProtectedMPEG4Audio` - `AppleProtectedMPEG4Audio`
- `kUTTypeAppleProtectedMPEG4Video` - `AppleProtectedMPEG4Video`
- `kUTTypeAppleScript` - `AppleScript`
- `kUTTypeApplication` - `Application`
- `kUTTypeApplicationBundle` - `ApplicationBundle`
- `kUTTypeApplicationFile` - `ApplicationFile`
- `kUTTypeArchive` - `Archive`
- `kUTTypeAssemblyLanguageSource` - `AssemblyLanguageSource`
- `kUTTypeAudio` - `Audio`
- `kUTTypeAudioInterchangeFileFormat` - `AudioInterchangeFileFormat`
- `kUTTypeAudiovisualContent` - `AudiovisualContent`
- `kUTTypeAVIMovie` - `AVIMovie`
- `kUTTypeBinaryPropertyList` - `BinaryPropertyList`
- `kUTTypeBMP` - `BMP`
- `kUTTypeBookmark` - `Bookmark`
- `kUTTypeBundle` - `Bundle`
- `kUTTypeBzip2Archive` - `Bzip2Archive`
- `kUTTypeCalendarEvent` - `CalendarEvent`
- `kUTTypeCHeader` - `CHeader`
- `kUTTypeCommaSeparatedText` - `CommaSeparatedText`
- `kUTTypeCompositeContent` - `CompositeContent`
- `kUTTypeConformsToKey` - `ConformsToKey`
- `kUTTypeContact` - `Contact`
- `kUTTypeContent` - `Content`
- `kUTTypeCPlusPlusHeader` - `CPlusPlusHeader`
- `kUTTypeCPlusPlusSource` - `CPlusPlusSource`
- `kUTTypeCSource` - `CSource`
- `kUTTypeData` - `Database`
- `kUTTypeDelimitedText` - `DelimitedText`
- `kUTTypeDescriptionKey` - `DescriptionKey`
- `kUTTypeDirectory` - `Directory`
- `kUTTypeDiskImage` - `DiskImage`
- `kUTTypeElectronicPublication` - `ElectronicPublication`
- `kUTTypeEmailMessage` - `EmailMessage`
- `kUTTypeExecutable` - `Executable`
- `kUTExportedTypeDeclarationsKey` - `ExportedTypeDeclarationsKey`
- `kUTTypeFileURL` - `FileURL`
- `kUTTypeFlatRTFD` - `FlatRTFD`
- `kUTTypeFolder` - `Folder`
- `kUTTypeFont` - `Font`
- `kUTTypeFramework` - `Framework`
- `kUTTypeGIF` - `GIF`
- `kUTTypeGNUZipArchive` - `GNUZipArchive` 
- `kUTTypeHTML` - `HTML`
- `kUTTypeICO` - `ICO`
- `kUTTypeIconFileKey` - `IconFileKey`
- `kUTTypeIdentifierKey` - `IdentifierKey`
- `kUTTypeImage` - `Image`
- `kUTImportedTypeDeclarationsKey` - `ImportedTypeDeclarationsKey`
- `kUTTypeInkText` - `InkText`
- `kUTTypeInternetLocation` - `InternetLocation`
- `kUTTypeItem` - `Item`
- `kUTTypeJavaArchive` - `JavaArchive`
- `kUTTypeJavaClass` - `JavaClass`
- `kUTTypeJavaScript` - `JavaScript`
- `kUTTypeJavaSource` - `JavaSource`
- `kUTTypeJPEG` - `JPEG`
- `kUTTypeJPEG2000` - `JPEG2000`
- `kUTTypeJSON` - `JSON`
- `kUTType3dObject` - `k3dObject`
- `kUTTypeLivePhoto` - `LivePhoto`
- `kUTTypeLog` - `Log` 
- `kUTTypeM3UPlaylist` - `M3UPlaylist`
- `kUTTypeMessage` - `Message`
- `kUTTypeMIDIAudio` - `MIDIAudio`
- `kUTTypeMountPoint` - `MountPoint`
- `kUTTypeMovie` - `Movie`
- `kUTTypeMP3` - `MP3`
- `kUTTypeMPEG` - `MPEG`
- `kUTTypeMPEG2TransportStream` - `MPEG2TransportStream`
- `kUTTypeMPEG2Video` - `MPEG2Video`
- `kUTTypeMPEG4` - `MPEG4`
- `kUTTypeMPEG4Audio` - `MPEG4Audio`
- `kUTTypeObjectiveCPlusPlusSource` - `ObjectiveCPlusPlusSource`
- `kUTTypeObjectiveCSource` - `ObjectiveCSource`
- `kUTTypeOSAScript` - `OSAScript`
- `kUTTypeOSAScriptBundle` - `OSAScriptBundle`
- `kUTTypePackage` - `Package`
- `kUTTypePDF` - `PDF`
- `kUTTypePerlScript` - `PerlScript`
- `kUTTypePHPScript` - `PHPScript`
- `kUTTypePICT` - `PICT`
- `kUTTypePKCS12` - `PKCS12`
- `kUTTypePlainText` - `PlainText`
- `kUTTypePlaylist` - `Playlist`
- `kUTTypePluginBundle` - `PluginBundle`
- `kUTTypePNG` - `PNG`
- `kUTTypePolygon` - `Polygon`
- `kUTTypePresentation` - `Presentation`
- `kUTTypePropertyList` - `PropertyList`
- `kUTTypePythonScript` - `PythonScript`
- `kUTTypeQuickLookGenerator` - `QuickLookGenerator`
- `kUTTypeQuickTimeImage` - `QuickTimeImage`
- `kUTTypeQuickTimeMovie` - `QuickTimeMovie` 
- `kUTTypeRawImage` - `RawImage`
- `kUTTypeReferenceURLKey` - `ReferenceURLKey`
- `kUTTypeResolvable` - `Resolvable`
- `kUTTypeRTF` - `RTF`
- `kUTTypeRTFD` - `RTFD`
- `kUTTypeRubyScript` - `RubyScript`
- `kUTTypeScalableVectorGraphics` - `ScalableVectorGraphics`
- `kUTTypeScript` - `Script`
- `kUTTypeShellScript` - `ShellScript`
- `kUTTypeSourceCode` - `SourceCode`
- `kUTTypeSpotlightImporter` - `SpotlightImporter`
- `kUTTypeSpreadsheet` - `Spreadsheet`
- `kUTTypeStereolithography` - `Stereolithography`
- `kUTTypeSwiftSource` - `SwiftSource`
- `kUTTypeSymLink` - `SymLink`
- `kUTTypeSystemPreferencesPane` - `SystemPreferencesPane`
- `kUTTypeTabSeparatedText` - `TabSeparatedText`
- `kUTTagClassFilenameExtension` - `TagClassFilenameExtension`
- `kUTTagClassMIMEType` - `TagClassMIMEType`
- `kUTTypeTagSpecificationKey` - `TagSpecificationKey`
- `kUTTypeText` - `Text`
- `kUTType3DContent` - `ThreeDContent`
- `kUTTypeTIFF` - `TIFF`
- `kUTTypeToDoItem` - `ToDoItem`
- `kUTTypeTXNTextAndMultimediaData` - `TXNTextAndMultimediaData`
- `kUTTypeUniversalSceneDescription` - `UniversalSceneDescription`
- `kUTTypeUnixExecutable` - `UnixExecutable`
- `kUTTypeURL` - `URL` 
- `kUTTypeURLBookmarkData` - `URLBookmarkData`
- `kUTTypeUTF16ExternalPlainText` - `UTF16ExternalPlainText`
- `kUTTypeUTF16PlainText` - `UTF16PlainText`
- `kUTTypeUTF8PlainText` - `UTF8PlainText`
- `kUTTypeUTF8TabSeparatedText` - `UTF8TabSeparatedText`
- `kUTTypeVCard` - `VCard`
- `kUTTypeVersionKey` - `VersionKey` 
- `kUTTypeVideo` - `Video` 
- `kUTTypeVolume` - `Volume` 
- `kUTTypeWaveformAudio` - `WaveformAudio`
- `kUTTypeWebArchive` - `WebArchive`
- `kUTTypeWindowsExecutable` - `WindowsExecutable`
- `kUTTypeX509Certificate` - `X509Certificate`
- `kUTTypeXML` - `XML`
- `kUTTypeXMLPropertyList` - `XMLPropertyList`
- `kUTTypeXPCService` - `XPCService`
- `kUTTypeZipArchive` - `ZipArchive`

次の例を参照してください。

```csharp
using MobileCoreServices;
...

NSItemProvider itemProvider = new NSItemProvider ();
itemProvider.LoadItem(UTType.PropertyList ,null, (item, err) => {
    if (err == null) {
        NSDictionary results = (NSDictionary )item;
        NSString baseURI =
results.ObjectForKey("NSExtensionJavaScriptPreprocessingResultsKey");
    }
});
```

詳細については、機能の使用**に**関するドキュメントの「[アプリグループ](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)」セクションを参照してください。

## <a name="precautions-and-considerations"></a>予防策と考慮事項

拡張機能は、アプリの場合よりも、使用可能なメモリが大幅に少なくなります。 これらは、ユーザーおよびホストされているアプリへの侵入を最小限に抑えて迅速に実行することが想定されています。 ただし、拡張機能では、ユーザーが属している拡張機能の開発者またはコンテナーアプリを識別できるブランド化された UI を使用するアプリに、独自の便利な機能を提供する必要もあります。

これらの厳しい要件を考慮すると、パフォーマンスとメモリの消費に対して十分にテストされ、最適化された拡張機能のみを展開する必要があります。 

## <a name="summary"></a>まとめ

このドキュメントでは、拡張機能について説明し、拡張ポイントの種類と、iOS によって拡張機能に課せられる既知の制限事項について説明します。 ここでは、拡張機能と拡張機能のライフサイクルの作成、配布、インストール、実行について説明しました。 このチュートリアルでは、ストーリーボードまたはコードを使用してウィジェットの UI を作成する2つの方法を示す、簡単な**Today**ウィジェットの作成について説明しました。 このチュートリアルでは、iOS シミュレーターで拡張機能をテストする方法を示しました。 最後に、ホストアプリとの通信、および拡張機能を開発するときに行う必要があるいくつかの予防措置と考慮事項について簡単に説明しました。 

## <a name="related-links"></a>関連リンク

- [ContainerApp (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/intro-to-extensions)
- [Xamarin で拡張機能を作成する (ビデオ)](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
