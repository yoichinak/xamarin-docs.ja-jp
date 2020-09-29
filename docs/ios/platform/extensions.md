---
title: Xamarin.iOS での iOS 拡張機能
description: このドキュメントでは、通知センター内などの標準コンテキストで iOS によって提供されるウィジェットである拡張機能について説明します。 拡張機能を作成し、親アプリから通信する方法について説明します。
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 05/12/2020
ms.openlocfilehash: d305f671ef68e9a1eded61936f3f32cc8769393f
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436293"
---
# <a name="ios-extensions-in-xamarinios"></a>Xamarin の iOS 拡張機能

> [!VIDEO https://youtube.com/embed/Sd0-ch9Udmk]

**IOS ビデオでの拡張機能の作成**

IOS 8 で導入された拡張機能は、 `UIViewControllers` ios によって、 **通知センター**内などの標準コンテキスト内で、特殊な入力を実行するためにユーザーによって要求されたカスタムキーボードの種類、または拡張機能が特殊効果フィルターを提供できる画像の編集などの他のコンテキストに特化したものです。

すべての拡張機能は、コンテナーアプリと共にインストールされ (両方の要素が64ビット統合 Api を使用して作成されます)、ホストアプリの特定の拡張ポイントからアクティブ化されます。 また、これらは既存のシステム関数の補完として使用されるため、高いパフォーマンス、リーン、堅牢性を備えている必要があります。

## <a name="extension-points"></a>拡張ポイント

|種類|説明|拡張ポイント|ホストアプリ|
|--- |--- |--- |--- |
|操作|特定のメディアの種類の特殊なエディターまたはビューアー|`com.apple.ui-services`|Any|
|ドキュメントプロバイダー|アプリがリモートドキュメントストアを使用できるようにします|`com.apple.fileprovider-ui`|[UIDocumentPickerViewController](xref:UIKit.UIDocumentPickerViewController)を使用するアプリ|
|キーボード|代替キーボード|`com.apple.keyboard-service`|Any|
|写真の編集|写真の操作と編集|`com.apple.photo-editing`|Photos アプリエディター|
|共有|ソーシャルネットワークやメッセージングサービスなどを使用してデータを共有します。|`com.apple.share-services`|Any|
|今日|今日の画面または通知センターに表示される "ウィジェット"|`com.apple.widget-extensions`|今日と通知センター|

[Ios 10](~/ios/platform/introduction-to-ios10/index.md#app-extensions)と[ios 12](~/ios/platform/introduction-to-ios12/index.md#notification-improvements)で追加の拡張ポイントが追加されました。 サポートされているすべての種類の完全な表については、「 [IOS アプリ拡張機能のプログラミングガイド](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW2)」を参照してください。

## <a name="limitations"></a>制限事項

拡張機能にはいくつかの制限がありますが、その中にはすべての種類に対して汎用的なものもあります (たとえば、拡張機能の種類によってカメラやマイクにアクセスできない場合)。また、他の種類の拡張機能には、使用法に関する特定の制限があります

一般的な制限事項は次のとおりです。

- [正常性キット](~/ios/platform/healthkit.md)および[イベントキットの UI](~/ios/platform/eventkit.md)フレームワークは使用できません
- 拡張機能は[拡張バックグラウンドモード](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md)を使用できません
- 拡張機能はデバイスのカメラやマイクにアクセスできません (ただし、既存のメディアファイルにアクセスする可能性があります)。
- 拡張機能はエアドロップデータを受信できません (無線でデータを送信することもできます)
- [Uiactionsheet](xref:UIKit.UIActionSheet)と[uiactionsheet](xref:UIKit.UIAlertView)は使用できません。拡張機能は[Uialertcontroller](xref:UIKit.UIAlertController)を使用する必要があります
- [Uiapplication のいくつ](xref:UIKit.UIApplication)かのメンバーは使用できません: [uiapplication](xref:UIKit.UIApplication.SharedApplication)、uiapplication. [OpenUrl](xref:UIKit.UIApplication.OpenUrl(Foundation.NSUrl))、 [Uiapplication、beginignoringinteractionevents](xref:UIKit.UIApplication.BeginIgnoringInteractionEvents) 、および[uiapplication。](xref:UIKit.UIApplication.EndIgnoringInteractionEvents)
- iOS では、現在の拡張機能に 16 MB のメモリ使用量制限が適用されます。
- 既定では、キーボード拡張機能はネットワークにアクセスできません。 これはデバイスのデバッグに影響します (この制限はシミュレーターには適用されません)。そのため、Xamarin. iOS ではデバッグのためにネットワークアクセスが必要になります。 プロジェクトの情報をに設定することにより、ネットワークアクセスを要求することができます `Requests Open Access` 。 `Yes` キーボード拡張機能の制限の詳細については、Apple の [カスタムキーボードガイド](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) を参照してください。

個々の制限事項については、「Apple の [アプリ拡張機能のプログラミングガイド](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/)」を参照してください。

## <a name="distributing-installing-and-running-extensions"></a>拡張機能の配布、インストール、および実行

拡張機能は、コンテナーアプリ内から配布され、アプリストアを介して送信および配布されます。 アプリと共に配布される拡張機能はその時点でインストールされますが、ユーザーは各拡張機能を明示的に有効にする必要があります。 さまざまな種類の拡張機能は、さまざまな方法で有効になります。いくつかの方法で、ユーザーは **設定** アプリに移動し、そこから設定アプリを有効にする必要があります。 他のユーザーは、写真の送信時に共有拡張機能を有効にするなど、使用する時点で有効になります。

拡張機能を使用する (ユーザーが拡張ポイントを検出する) アプリは、実行時に拡張機能をホストするアプリであるため、 **ホストアプリ**と呼ばれます。 拡張機能をインストールするアプリは、インストール時に拡張機能が含まれていたアプリであるため、 **コンテナーアプリ**です。  

通常、コンテナーアプリでは拡張機能について説明し、ユーザーに対して有効にするプロセスを説明します。

## <a name="debug-and-release-versions-of-extensions"></a>拡張機能のデバッグバージョンとリリースバージョン

実行中のアプリ拡張機能のメモリ制限は、フォアグラウンドアプリに適用されるメモリ制限よりも大幅に低くなります。 IOS を実行しているシミュレーターには、拡張機能に適用される制限が少ないため、拡張機能は問題なく実行できます。 ただし、デバイスで同じ拡張機能を実行すると、拡張機能がクラッシュしたり、システムによって積極的に終了されたりするなど、予期しない結果が生じる可能性があります。 そのため、デバイスを出荷する前に、その拡張機能をビルドしてテストしてください。

コンテナープロジェクトとすべての参照される拡張機能には、次の設定が適用されていることを確認する必要があります。

1. **リリース**構成でアプリケーションパッケージをビルドします。
1. **IOS ビルド**プロジェクトの設定で、[**リンカー動作**] オプションを [**フレームワーク sdk のみをリンク**する] または [**すべてリンク**] に設定します。
1. **IOS デバッグ**プロジェクトの設定で、[**デバッグを有効に**し、**プロファイルを有効**にする] オプションをオフにします。

## <a name="extension-lifecycle"></a>拡張機能のライフサイクル

拡張機能は、UI の複数の画面を表示する1つの [Uiviewcontroller](xref:UIKit.UIViewController) またはより複雑な拡張機能として簡単に使用できます。 ユーザーが _拡張ポイント_ (イメージを共有するときなど) を検出すると、その拡張ポイントに登録されている拡張機能から選択できるようになります。

アプリの拡張機能のいずれかを選択 `UIViewController` すると、がインスタンス化され、通常のビューコントローラーのライフサイクルが開始されます。 ただし、中断されているが、通常はユーザーが操作を終了したときに終了しない通常のアプリとは異なり、拡張機能が読み込まれ、実行された後、繰り返し終了します。

拡張機能は、 [NSExtensionContext](xref:Foundation.NSExtensionContext) オブジェクトを介してホストアプリと通信できます。 一部の拡張機能には、結果を使用して非同期コールバックを受信する操作が含まれています。 これらのコールバックはバックグラウンドスレッドで実行され、拡張機能はこれを考慮に入れる必要があります。たとえば、ユーザーインターフェイスを更新する場合は、 [NSObject](xref:Foundation.NSObject.InvokeOnMainThread*) を使用します。 詳細については、後述の「 [ホストアプリとの通信](#communicating-with-the-host-app) 」を参照してください。

既定では、拡張機能とそのコンテナーアプリは、一緒にインストールされていても通信できません。 場合によっては、コンテナーアプリが、拡張機能のインストール後に提供される、空の "出荷" コンテナーであることがあります。 ただし、状況によっては、コンテナーアプリと拡張機能が共通領域からリソースを共有する場合があります。 さらに、 **今日の拡張機能** では、URL を開くようにコンテナーアプリを要求できます。 この動作は、 [イベントカウントダウンウィジェット](https://github.com/xamarin/ios-samples/tree/master/intro-to-extensions)で表示されます。

## <a name="creating-an-extension"></a>拡張機能の作成

拡張機能 (およびそのコンテナーアプリ) は、64ビットのバイナリであり、Xamarin の [統合 api](~/cross-platform/macios/unified/index.md)を使用して構築されている必要があります。 拡張機能を開発する場合、ソリューションには少なくとも2つのプロジェクトが含まれます。コンテナーアプリと、コンテナーが提供する各拡張機能の1つのプロジェクトです。

### <a name="container-app-project-requirements"></a>コンテナーアプリプロジェクトの要件

拡張機能のインストールに使用されるコンテナーアプリには、次の要件があります。

- 拡張機能プロジェクトへの参照を保持する必要があります。   
- 拡張機能をインストールする方法を提供するだけでなく、完全なアプリケーション (正常に起動して実行できる必要があります) である必要があります。
- このファイルには、拡張プロジェクトのバンドル識別子の基礎となるバンドル識別子が必要です (詳細については、以下のセクションを参照してください)。

### <a name="extension-project-requirements"></a>拡張機能プロジェクトの要件

さらに、拡張機能のプロジェクトには次の要件があります。

- コンテナーアプリのバンドル識別子で始まるバンドル識別子が必要です。 たとえば、コンテナーアプリののバンドル識別子がの場合、 `com.myCompany.ContainerApp` 拡張機能の識別子は次のようになり `com.myCompany.ContainerApp.MyExtension` ます。

  ![バンドル識別子](extensions-images/bundleidentifiers.png)
- このファイルには、 `NSExtensionPointIdentifier` 適切な値 ( `com.apple.widget-extension` **今日** の Notification Center ウィジェットなど) を指定して、キーを定義する必要があり `Info.plist` ます。
- また、 *either* `NSExtensionMainStoryboard` `NSExtensionPrincipalClass` `Info.plist` 適切な値を使用して、ファイル内のキーまたはキーのいずれかを定義する必要があります。
  - キーを使用して、 `NSExtensionMainStoryboard` 拡張機能のメイン UI を表示するストーリーボードの名前を指定します (マイナス `.storyboard` )。 たとえば、 `Main` ファイルの場合は `Main.storyboard` です。
  - キーを使用して、 `NSExtensionPrincipalClass` 拡張機能の開始時に初期化されるクラスを指定します。 値は、の **レジスタ** 値と一致している必要があり `UIViewController` ます。

  ![プリンシパルクラスの登録](extensions-images/registerandprincipalclass.png)

特定の種類の拡張機能には、追加の要件がある場合があります。 たとえば、 **今日** または **Notification Center** の拡張機能のプリンシパルクラスでは、 [INCWidgetProviding](xref:NotificationCenter.INCWidgetProviding)を実装する必要があります。

> [!IMPORTANT]
> Visual Studio for Mac によって提供される拡張機能テンプレートの1つを使用してプロジェクトを開始すると、これらの要件がテンプレートによって自動的に提供され、満たされます。

## <a name="walkthrough"></a>チュートリアル

次のチュートリアルでは、 **今日** の日付と残り日数を計算するサンプルのウィジェットを作成します。

[![今日の日と残り日数を計算する今日のウィジェットの例](extensions-images/carpediemscreenshot-sm.png)](extensions-images/carpediemscreenshot.png#lightbox)

### <a name="creating-the-solution"></a>ソリューションの作成

必要なソリューションを作成するには、次の手順を実行します。

1. まず、新しい iOS、 **シングルビューアプリ** プロジェクトを作成し、[ **次へ** ] ボタンをクリックします。

    [![まず、新しい iOS、シングルビューアプリプロジェクトを作成し、[次へ] ボタンをクリックします。](extensions-images/today01.png)](extensions-images/today01.png#lightbox)
2. プロジェクトを呼び出し、 `TodayContainer` [ **次へ** ] ボタンをクリックします。

    [![プロジェクト TodayContainer を呼び出し、[次へ] ボタンをクリックします。](extensions-images/today02.png)](extensions-images/today02.png#lightbox)
3. **プロジェクト名**と**SolutionName**を確認し、[**作成**] ボタンをクリックしてソリューションを作成します。

    [![プロジェクト名と SolutionName を確認し、[作成] ボタンをクリックしてソリューションを作成します。](extensions-images/today03.png)](extensions-images/today03.png#lightbox)
4. 次に、**ソリューションエクスプローラー**でソリューションを右クリックし、**今日の拡張**機能テンプレートから新しい**iOS 拡張機能**プロジェクトを追加します。

    [![次に、ソリューションエクスプローラーの [ソリューション] を右クリックし、[今日の拡張機能] テンプレートから新しい iOS 拡張機能プロジェクトを追加します。](extensions-images/today04.png)](extensions-images/today04.png#lightbox)
5. プロジェクトを呼び出し、 `DaysRemaining` [ **次へ** ] ボタンをクリックします。

    [![プロジェクト DaysRemaining を呼び出し、[次へ] ボタンをクリックします。](extensions-images/today05.png)](extensions-images/today05.png#lightbox)
6. プロジェクトを確認し、[ **作成** ] ボタンをクリックして作成します。

    [![プロジェクトを確認し、[作成] ボタンをクリックして作成します。](extensions-images/today06.png)](extensions-images/today06.png#lightbox)

次に示すように、結果として得られるソリューションには2つのプロジェクトが必要になります。

[![次に示すように、結果として得られるソリューションには2つのプロジェクトが必要になります。](extensions-images/today07.png)](extensions-images/today07.png#lightbox)

### <a name="creating-the-extension-user-interface"></a>拡張機能のユーザーインターフェイスの作成

次に、 **今日** のウィジェットのインターフェイスを設計する必要があります。 これは、ストーリーボードを使用するか、コードで UI を作成することによって行うことができます。 両方の方法について詳しく説明します。

#### <a name="using-storyboards"></a>ストーリーボードの使用

ストーリーボードを使用して UI を作成するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、拡張機能プロジェクトのファイルをダブルクリックし `Main.storyboard` て、編集用に開きます。

    [![拡張機能プロジェクトのメインのストーリーボードファイルをダブルクリックして、編集用に開きます。](extensions-images/today08.png)](extensions-images/today08.png#lightbox)
2. テンプレートによって UI に自動的に追加されたラベルを選択し**Name** 、[ `TodayMessage` **プロパティエクスプローラー**] の [**ウィジェット**] タブで名前を指定します。

    [![テンプレートによって UI に自動的に追加されたラベルを選択し、プロパティエクスプローラーの [ウィジェット] タブに TodayMessage という名前を付けます。](extensions-images/today09.png)](extensions-images/today09.png#lightbox)
3. 変更内容をストーリーボードに保存します。

#### <a name="using-code"></a>コードの使用

コードで UI をビルドするには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、 **DaysRemaining**プロジェクトを選択し、新しいクラスを追加して、次のように呼び出し `CodeBasedViewController` ます。

    [![DaysRemaining プロジェクトを選択し、新しいクラスを追加して、Codeベース Viewcontroller を呼び出します。](extensions-images/code01.png)](extensions-images/code01.png#lightbox)
2. ここでも、 **ソリューションエクスプローラー**で、拡張機能のファイルをダブルクリックし `Info.plist` て開き、編集します。

    [![拡張機能の情報 plist ファイルをダブルクリックして編集用に開きます。](extensions-images/code02.png)](extensions-images/code02.png#lightbox)
3. (画面の下部から) **ソースビュー** を選択し、ノードを開き `NSExtension` ます。

    [![画面の下部にあるソースビューを選択し、NSExtension ノードを開きます。](extensions-images/code03.png)](extensions-images/code03.png#lightbox)
4. キーを削除 `NSExtensionMainStoryboard` し、値を指定してを追加し `NSExtensionPrincipalClass` `CodeBasedViewController` ます。

    [![NSExtensionMainStoryboard キーを削除し、値 CodeNSExtensionPrincipalClass Viewcontroller を使用してを追加します。](extensions-images/code04.png)](extensions-images/code04.png#lightbox)
5. 変更を保存します。

次に、ファイルを編集 `CodeBasedViewController.cs` し、次のように表示します。

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

は、上で指定した値と一致することに注意して `[Register("CodeBasedViewController")]` `NSExtensionPrincipalClass` ください。

### <a name="coding-the-extension"></a>拡張機能のコーディング

ユーザーインターフェイスを作成した状態で、 `TodayViewController.cs` または `CodeBasedViewController.cs` (上のユーザーインターフェイスの作成に使用した方法に基づいて) ファイルを開き、 **ViewDidLoad** メソッドを変更し、次のようにします。

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

コードベースのユーザーインターフェイスメソッドを使用している場合は、 `// Insert code to power extension here...` 上記の新しいコードでコメントを置き換えます。 基本実装を呼び出した後 (およびコードベースバージョンのラベルを挿入した後)、このコードは単純な計算を実行して、その年の通算日と残りの日数を取得します。 次に、UI デザインで作成したラベル () にメッセージが表示され `TodayMessage` ます。

このプロセスは、アプリの通常の作成プロセスに似ていることに注意してください。 拡張機能に `UIViewController` は、アプリ内のビューコントローラーと同じライフサイクルがあります。ただし、拡張機能にはバックグラウンドモードがないため、ユーザーが使用を終了したときに中断されることはありません。 代わりに、拡張機能が繰り返し初期化され、必要に応じて割り当てが解除されます。

### <a name="creating-the-container-app-user-interface"></a>コンテナーアプリのユーザーインターフェイスを作成する

このチュートリアルでは、コンテナーアプリは、拡張機能を出荷およびインストールするためのメソッドとしてのみ使用され、独自の機能は提供されません。 TodayContainer のファイルを編集して、 `Main.storyboard` 拡張機能の関数とそのインストール方法を定義するテキストを追加します。

[![TodayContainers のメインストーリーボードファイルを編集し、Extensions 関数とそのインストール方法を定義するテキストを追加します。](extensions-images/today10.png)](extensions-images/today10.png#lightbox)

変更内容をストーリーボードに保存します。

### <a name="testing-the-extension"></a>拡張機能のテスト

IOS シミュレーターで拡張機能をテストするには、 **TodayContainer** アプリを実行します。 コンテナーのメインビューが表示されます。

[![コンテナーのメインビューが表示されます](extensions-images/run01.png)](extensions-images/run01.png#lightbox)

次に、シミュレーターの [ **ホーム** ] ボタンをクリックし、画面の上部から下方向にスワイプして **通知センター**を開き、[ **今日** ] タブを選択して [ **編集** ] ボタンをクリックします。

[![シミュレーターの [ホーム] ボタンをクリックし、画面の上部から下方向にスワイプして通知センターを開き、[今日] タブを選択して [編集] ボタンをクリックします。](extensions-images/run02.png)](extensions-images/run02.png#lightbox)

**DaysRemaining**拡張機能を**Today**ビューに追加し、[ **Done** ] \ (完了 \) ボタンをクリックします。

[![DaysRemaining 拡張機能を Today ビューに追加し、[完了] ボタンをクリックします。](extensions-images/run03.png)](extensions-images/run03.png#lightbox)

新しいウィジェットが **今日** のビューに追加され、結果が表示されます。

[![新しいウィジェットが今日のビューに追加され、結果が表示されます。](extensions-images/run04.png)](extensions-images/run04.png#lightbox)

## <a name="communicating-with-the-host-app"></a>ホストアプリとの通信

上で作成した今日の拡張機能の例は、ホストアプリ ( **今日** 画面) とは通信しません。 存在する場合は、クラスまたはクラスの [Extensioncontext](xref:Foundation.NSExtensionContext) プロパティを使用し `TodayViewController` `CodeBasedViewController` ます。

ホストアプリからデータを受信する拡張機能の場合、データは、拡張機能の[Extensioncontext](xref:Foundation.NSExtensionContext)の[InputItems](xref:Foundation.NSExtensionContext.InputItems)プロパティに格納されている[NSExtensionItem](xref:Foundation.NSExtensionItem)オブジェクトの配列の形式になり `UIViewController` ます。

写真編集拡張機能などの他の拡張機能は、ユーザーが使用状況を完了またはキャンセルすることを区別できます。 これは、 [Extensioncontext](xref:Foundation.NSExtensionContext)プロパティの[CompleteRequest](xref:Foundation.NSExtensionContext.CompleteRequest*)および[cancelrequest](xref:Foundation.NSExtensionContext.CancelRequest*)メソッドを介してホストアプリに返されます。

詳細については、「Apple の [アプリ拡張機能のプログラミングガイド](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1)」を参照してください。

## <a name="communicating-with-the-parent-app"></a>親アプリとの通信

アプリ グループを使用すると、さまざまなアプリケーション (またはアプリケーションとその拡張機能) から共有ファイルの保存場所にアクセスできます。 アプリ グループは次のようなデータに使用できます。

- [Apple Watch 設定](~/ios/watchos/app-fundamentals/settings.md)。
- [共有 NSUserDefaults](~/ios/app-fundamentals/user-defaults.md)。
- [共有ファイル](~/ios/watchos/app-fundamentals/parent-app.md#files)。

詳細については、機能の使用**に**関するドキュメントの「[アプリグループ](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)」セクションを参照してください。

## <a name="mobilecoreservices"></a>MobileCoreServices

拡張機能を使用する場合は、Uniform Type Identifier (UTI) を使用して、アプリ、他のアプリ、サービスの間で交換されるデータを作成および操作します。

`MobileCoreServices.UTType`静的クラスは、Apple の定義に関連する次のヘルパープロパティを定義し `kUTType...` ます。

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

このドキュメントでは、拡張機能について説明し、拡張ポイントの種類と、iOS によって拡張機能に課せられる既知の制限事項について説明します。 ここでは、拡張機能と拡張機能のライフサイクルの作成、配布、インストール、実行について説明しました。 このチュートリアルでは、ストーリーボードまたはコードを使用してウィジェットの UI を作成する2つの方法を示す、簡単な **Today** ウィジェットの作成について説明しました。 このチュートリアルでは、iOS シミュレーターで拡張機能をテストする方法を示しました。 最後に、ホストアプリとの通信、および拡張機能を開発するときに行う必要があるいくつかの予防措置と考慮事項について簡単に説明しました。

## <a name="related-links"></a>関連リンク

- [ContainerApp (サンプル)](/samples/xamarin/ios-samples/intro-to-extensions)
- [Xamarin で拡張機能を作成する (ビデオ)](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
- [IOS アプリ拡張機能の効率とパフォーマンスを最適化する](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/ExtensionCreation.html#//apple_ref/doc/uid/TP40014214-CH5-SW7)