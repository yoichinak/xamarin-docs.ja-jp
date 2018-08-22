---
title: iOS の Xamarin.iOS での拡張機能
description: このドキュメントでは、ウィジェットなどの通知センター内での標準のコンテキストでの iOS によって提示されるは、拡張機能について説明します。 これには、拡張機能を作成し、親のアプリから通信する方法について説明します。
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 831625c88bb3c0f8403d3b330054050488956cb6
ms.sourcegitcommit: b6f3e55d4f3dcdc505abc8dc9241cff0bb5bd154
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/10/2018
ms.locfileid: "40251189"
---
# <a name="ios-extensions-in-xamarinios"></a>Xamarin.iOS での iOS 拡張機能

> [!VIDEO https://youtube.com/embed/Sd0-ch9Udmk]

**IOS の拡張機能の作成[Xamarin University](https://university.xamarin.com/)**

拡張機能、iOS 8 で導入されたは、特化`UIViewControllers`使用される標準コンテキスト内での iOS でなど以内、**通知センター**を実行するユーザーによって要求されたカスタムのキーボードの種類に特化した、入力または他のコンテキストなどの特殊効果のフィルターを拡張機能が提供できる写真を編集します。

すべての拡張機能では、(64 ビットの Unified Api を使用して書き込まれる要素は両方) で、コンテナー アプリと共にインストールされているし、ホスト アプリケーションの特定の拡張機能ポイントからアクティブ化されます。 無駄のないと堅牢性、高性能をいる必要がありますので、既存のシステム関数に追加ファイルとして使用されます。 

## <a name="extension-points"></a>拡張ポイント

|型|説明|拡張ポイント|ホストのアプリ|
|--- |--- |--- |--- |
|アクション|特殊なエディターまたは特定のメディアの種類のビューアー|`com.apple.ui-services`|どれでも可|
|ドキュメント プロバイダー|により、リモート ドキュメント ストアを使用するアプリ|`com.apple.fileprovider-ui`|使用してアプリを[UIDocumentPickerViewController](https://developer.xamarin.com/api/type/UIKit.UIDocumentPickerViewController/)|
|キーボード|代替キーボード|`com.apple.keyboard-service`|どれでも可|
|写真の編集|写真の操作と編集|`com.apple.photo-editing`|Photos.app エディター|
|共有|メッセージング サービスなどのソーシャル ネットワークとデータを共有します。|`com.apple.share-services`|どれでも可|
|今日|現在の画面または通知センターに表示される「ウィジェット」|`com.apple.widget-extensions`|今日、通知センター|

[追加の拡張機能ポイント](~/ios/platform/introduction-to-ios10/index.md#app-extensions)iOS 10 で追加されました。

## <a name="limitations"></a>制限事項

拡張機能があるいくつかの制限、すべての種類をユニバーサル (拡張機能のない種類のインスタンスは、カメラまたはマイクにアクセスできます) のうちいくつかは、その他の種類の拡張機能では、その使用方法 (たとえば、カスタム キーボードに具体的な制限事項があります使用できませんのパスワードなどセキュリティで保護されたデータの入力フィールド)。 

ユニバーサルの制限事項は次のとおりです。

- [ヘルス キット](~/ios/platform/healthkit.md)と[イベント キット UI](~/ios/platform/eventkit.md)フレームワークを利用できません
- 拡張機能を使用できない[バック グラウンド モードを拡張](http://developer.xamarin.com/guides/cross-platform/application_fundamentals/backgrounding/part_3_ios_backgrounding_techniques/registering_applications_to_run_in_background/)
- 拡張機能は、デバイスのカメラまたはマイク (ただし、既存のメディア ファイルにアクセスすることがあります)、アクセスできません。
- 拡張機能は空気を削除するデータを受信できません (ただし、空気のドロップを使用してデータを送信することができます)
- [UIActionSheet](https://developer.xamarin.com/api/type/UIKit.UIActionSheet/)と["uialertview"](https://developer.xamarin.com/api/type/UIKit.UIAlertView/)が使用不可能です拡張機能を使用する必要があります[UIAlertController。](https://developer.xamarin.com/api/type/UIKit.UIAlertController/)
- いくつかのメンバーの[UIApplication](https://developer.xamarin.com/api/type/UIKit.UIApplication/)は使用できません: [UIApplication.SharedApplication](https://developer.xamarin.com/api/property/UIKit.UIApplication.SharedApplication/)、 `UIApplication.OpenURL`、`UIApplication.BeginIgnoringInteractionEvents`と `UIApplication.EndIgnoringInteractionEvents`
- iOS では、今日の拡張機能で 16 MB のメモリ使用制限を適用します。
- 既定では、キーボード拡張機能は、ネットワークへのアクセスを必要はありません。 これは (制限は、シミュレーターでは適用されません)、デバイスでのデバッグに影響 Xamarin.iOS では、デバッグを行うにネットワーク アクセスが必要なためです。 設定してネットワーク アクセスを要求することは、`Requests Open Access`値をプロジェクトの Info.plist で`Yes`します。 Apple を参照してください[カスタム キーボード ガイド](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html)キーボード拡張機能の制限事項の詳細について。

個々 の制限については、Apple を参照してください[アプリ拡張機能のプログラミング ガイド](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/)します。

## <a name="distributing-installing-and-running-extensions"></a>配布、インストール、および拡張機能の実行

拡張機能は、これによりが送信され、App Store を介して配布コンテナー アプリ内から配布されます。 アプリと共に配布する拡張機能がその時点では、インストールされているが、ユーザーが明示的にそれぞれの拡張機能を有効する必要があります。 さまざまな方法でさまざまな種類の拡張機能が有効になっています。移動するユーザーが必要なもの、**設定**アプリし、そこからそれらを有効にします。 中に、写真を送信するときに共有拡張機能を有効にするなど、使用の時点では、他のユーザーが有効になります。 

(ユーザーは、拡張機能ポイントが発生した)、拡張機能を使用するアプリと呼びます、**ホスト アプリ**を実行するときに、拡張機能をホストするアプリであるため、します。 拡張機能をインストールするアプリは、**コンテナー アプリ**拡張機能をインストールしたときに含まれているアプリであるため、します。  

通常、コンテナー アプリは、拡張機能について説明し、ユーザーを有効にする手順について説明します。

## <a name="extension-lifecycle"></a>拡張機能のライフ サイクル

拡張機能は、1 つの簡単な[UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/)または UI の複数の画面を提示するより複雑な拡張機能。 ユーザーが検出した場合、_拡張ポイント_(場合など、イメージの共有)、その拡張機能ポイントに登録されている拡張機能から選択する機会が。 

アプリのいずれかを選択した場合の拡張機能では、その`UIViewController`がインスタンス化され、通常のビュー コント ローラーのライフ サイクルを開始します。 ただし、拡張機能は中断しているが、一般終了とやり取りするユーザーが終了したときに、通常のアプリとは異なり、読み込まれて、実行、および、繰り返しが終了しは。

拡張機能を使用してアプリをホストと通信できる、 [NSExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/)オブジェクト。 一部の拡張機能では、結果を使用した非同期のコールバックを受信する操作があります。 これらのコールバックがバック グラウンド スレッドで実行され、考慮します。 この拡張機能を考慮する必要があります。使用して、たとえば、 [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/member/Foundation.NSObject.InvokeOnMainThread/)ユーザー インターフェイスを更新する場合。 参照してください、[ホスト アプリケーションと通信する](#Communicating-with-the-Host-App)詳細については後述します。

既定では、拡張機能とそのコンテナー アプリ通信可能でない、一緒にインストールされているにもかかわらずです。 場合によっては、コンテナー アプリは基本的に、空"shipping"コンテナーを目的とするが、拡張機能がインストールされると処理されます。 ただし、状況下で、必要である場合コンテナー アプリと拡張機能がリソースを共有一般的な領域から。 さらに、**拡張機能を今すぐ**の URL を開くには、そのコンテナー アプリを要求する場合があります。 この動作を示した、[進化 Countdown Widget](http://github.com/xamarin/monotouch-samples/tree/master/ExtensionsDemo)します。

## <a name="creating-an-extension"></a>拡張機能の作成

拡張機能 (とそのコンテナー アプリ) は、64 ビット バイナリをする必要があり、Xamarin.iOS を使用して構築された[Unified Api](http://developer.xamarin.com/guides/cross-platform/macios/unified)します。 ソリューションが少なくとも 2 つのプロジェクトを含む拡張機能を開発する場合: コンテナー アプリとプロジェクトの各拡張機能のコンテナーを提供します。 

### <a name="container-app-project-requirements"></a>コンテナー アプリのプロジェクトの要件

拡張機能をインストールするために使用するコンテナー アプリでは、次の要件があります。

- 拡張機能プロジェクトへの参照の管理にする必要があります。   
- 完全なアプリ (起動し、正常に実行できる必要があります) する必要がある場合でも、これは、拡張機能をインストールする方法を提供します。 
- バンドルの識別子の拡張機能のバンドル Id の基になる必要があります (詳細については、以下のセクションを参照してください) のプロジェクトします。

### <a name="extension-project-requirements"></a>拡張機能プロジェクトの要件

また、拡張機能のプロジェクトでは、次の要件には。

- そのコンテナー アプリのバンドル識別子で始まるバンドル識別子が必要です。 たとえば、コンテナー アプリがある、バンドル識別子が`com.myCompany.ContainerApp`、拡張機能の識別子に`com.myCompany.ContainerApp.MyExtension`: 

    ![](extensions-images/bundleidentifiers.png) 
- キーを定義する必要があります`NSExtensionPointIdentifier`、適切な値 (など`com.apple.widget-extension`の**今日**通知センターのウィジェット) で、その`Info.plist`ファイル。
- これを定義する必要がありますも*か*、`NSExtensionMainStoryboard`キーまたは`NSExtensionPrincipalClass`でキーその`Info.plist`ファイルを適切な値。
    - 使用して、`NSExtensionMainStoryboard`拡張機能の主な UI を表示するストーリー ボードの名前を指定するキー (マイナス`.storyboard`)。 たとえば、`Main`の`Main.storyboard`ファイル。
    - 使用して、`NSExtensionPrincipalClass`拡張機能の開始時に初期化されるクラスを指定するキー。 値が一致する必要があります、**登録**の値、 `UIViewController`: 

    ![](extensions-images/registerandprincipalclass.png)

特定の種類の拡張機能の追加要件があります。 たとえば、**今日**または**通知センター**拡張機能のプリンシパル クラスを実装する必要があります[INCWidgetProviding](https://developer.xamarin.com/api/type/NotificationCenter.INCWidgetProviding/)します。

> [!IMPORTANT]
> For Mac に Visual Studio によって提供される拡張機能のテンプレートを 1 つを使用してプロジェクトを開始する場合、これらの (全部ではない場合は、ほとんどの要件が提供され、テンプレートによって自動的にするために満たします。

## <a name="walkthrough"></a>チュートリアル 

次のチュートリアルでは、例を作成します**今日**日と年の残り日数を計算するウィジェット。

[![](extensions-images/carpediemscreenshot-sm.png "1 日と年の残り日数を計算する例今日ウィジェット")](extensions-images/carpediemscreenshot.png#lightbox)

### <a name="creating-the-solution"></a>ソリューションを作成します。

必要なソリューションを作成するには、次の操作を行います。

1. 新しい iOS の場合を最初に、作成**単一ビュー アプリ**プロジェクトをクリックして、**次**ボタン。 

    [![](extensions-images/today01.png "最初に、新しい iOS プロジェクトの単一ビュー アプリを作成し、[次へ] ボタンをクリックします。")](extensions-images/today01.png#lightbox)
2. プロジェクトを呼び出す`TodayContainer` をクリックし、**次**ボタン。 

    [![](extensions-images/today02.png "プロジェクト TodayContainer を呼び出すし、[次へ] ボタンをクリックします。")](extensions-images/today02.png#lightbox)
3. 確認、**プロジェクト名**と**SolutionName**  をクリックし、**作成**ソリューションを作成するボタン。 

    [![](extensions-images/today03.png "プロジェクト名と SolutionName を確認し、ソリューションを作成する [作成] ボタンをクリックします")](extensions-images/today03.png#lightbox)
4. 次に、**ソリューション エクスプ ローラー**、ソリューションを右クリックし、新しい追加**iOS 拡張機能**からプロジェクトを**拡張機能を今すぐ**テンプレート。 

    [![](extensions-images/today04.png "次に、ソリューション エクスプ ローラーでソリューションを右クリックし、今日の拡張機能テンプレートから新しい iOS 拡張機能プロジェクトを追加")](extensions-images/today04.png#lightbox)
5. プロジェクトを呼び出す`DaysRemaining` をクリックし、**次**ボタン。 

    [![](extensions-images/today05.png "プロジェクト DaysRemaining を呼び出すし、[次へ] ボタンをクリックします。")](extensions-images/today05.png#lightbox)
6. プロジェクトを確認し、をクリックして、**作成** ボタンを作成します。 

    [![](extensions-images/today06.png "プロジェクトを確認し、作成時に [作成] ボタンをクリックします。")](extensions-images/today06.png#lightbox)

次のように、結果として得られるソリューションを 2 つのプロジェクトを取得できました。

[![](extensions-images/today07.png "結果として得られるソリューションが 2 つのプロジェクトでは、次のように")](extensions-images/today07.png#lightbox)

### <a name="creating-the-extension-user-interface"></a>拡張機能のユーザー インターフェイスを作成します。

次に、インターフェイスを設計する必要がありますには、**今日**ウィジェット。 これは、実行することができますか、ストーリー ボードを使用して、またはコードで UI を作成します。 どちらの方法は、以下で詳しく説明されます。

#### <a name="using-storyboards"></a>ストーリー ボードを使用します。

ストーリー ボード UI をビルドするには、次の操作を行います。

1. **ソリューション エクスプ ローラー**、拡張機能プロジェクトのダブルクリック`Main.storyboard`ファイルを開き、編集します。 

    [![](extensions-images/today08.png "開き、編集するには、拡張機能プロジェクト Main.storyboard ファイルをダブルクリックします。")](extensions-images/today08.png#lightbox)
2. テンプレートによって、UI を自動的に追加されたラベルを選択し、試して、**名前**`TodayMessage`で、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**: 

    [![](extensions-images/today09.png "テンプレートによって、UI を自動的に追加されたラベルを選択し、プロパティ エクスプ ローラーの [ウィジェット] タブで名前 TodayMessage")](extensions-images/today09.png#lightbox)
3. ストーリー ボードに変更を保存します。

#### <a name="using-code"></a>コードを使用します。

コードでは、UI をビルドするには、次の操作を行います。 

1. **ソリューション エクスプ ローラー**を選択、 **DaysRemaining**プロジェクトで新しいクラスを追加し、それを呼び出す`CodeBasedViewController`: 

    [![](extensions-images/code01.png "Aelect DaysRemaining プロジェクトは、新しいクラスを追加し、CodeBasedViewController を付けます")](extensions-images/code01.png#lightbox)
2. もう一度、**ソリューション エクスプ ローラー**、拡張機能のダブルクリック`Info.plist`ファイルを開き、編集します。 

    [![](extensions-images/code02.png "開き、編集の拡張機能の Info.plist ファイルをダブルクリックします。")](extensions-images/code02.png#lightbox)
3. 選択、**ソース ビュー** (画面の下部にある) から開くと、`NSExtension`ノード。 

    [![](extensions-images/code03.png "NSExtension ノードを開き、画面の下部にある、ソース ビューを選択します。")](extensions-images/code03.png#lightbox)
4. 削除、`NSExtensionMainStoryboard`キーを追加、`NSExtensionPrincipalClass`値を持つ`CodeBasedViewController`: 

    [![](extensions-images/code04.png "NSExtensionMainStoryboard キーを削除し、NSExtensionPrincipalClass CodeBasedViewController 値の追加")](extensions-images/code04.png#lightbox)
5. 変更内容を保存します。

次に、編集、`CodeBasedViewController.cs`ファイルを開き、次のようになります。

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

なお、`[Register("CodeBasedViewController")]`に対して指定した値と一致する、`NSExtensionPrincipalClass`上。

### <a name="coding-the-extension"></a>拡張をコーディングします。

ユーザー インターフェイスを作成して、開くか、`TodayViewController.cs`または`CodeBasedViewController.cs`ファイル (上記のユーザー インターフェイスを作成するために使用するメソッドのベース)、変更、 **ViewDidLoad**メソッドと、次のようになります。

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

ユーザー インターフェイスのメソッド ベースのコードを使用して場合、置換、`// Insert code to power extension here...`上から新しいコードをコメントします。 後基本実装を呼び出す (およびコード ベースのバージョンのラベルの挿入)、このコードは、年と、残りの日数の日を取得する単純な計算します。 ラベル内のメッセージを表示し、(`TodayMessage`) UI デザインで作成しました。

このプロセスは、通常のプロセスのアプリケーションを作成する、類似性に注意してください。 拡張機能の`UIViewController`拡張機能は、バック グラウンド モードがないと、ユーザーが終了するとは中断されません点を除いて、アプリケーションでは、ビュー コント ローラーとして同じライフ サイクルがこれらを使用します。 代わりに、拡張機能は繰り返し初期化され、必要に応じて割り当てが解除されます。

### <a name="creating-the-container-app-user-interface"></a>コンテナー アプリのユーザー インターフェイスを作成します。

このチュートリアルでは、コンテナー アプリが付属し、拡張機能をインストールする方法として単に使用し、独自の機能はありません。 編集 TodayContainer の`Main.storyboard`ファイルを開き、拡張機能の関数とそのインストール方法を定義するいくつかのテキストを追加します。

[![](extensions-images/today10.png "TodayContainers Main.storyboard ファイルを編集し、拡張関数とそのインストール方法を定義するいくつかのテキストを追加します。")](extensions-images/today10.png#lightbox)

ストーリー ボードに変更を保存します。

### <a name="testing-the-extension"></a>拡張機能のテスト

IOS シミュレーターで、拡張機能をテストするには、実行、 **TodayContainer**アプリ。 コンテナーのメイン ビューが表示されます。

[![](extensions-images/run01.png "コンテナーの主なビューが表示されます。")](extensions-images/run01.png#lightbox)

次に、ヒット、**ホーム**開く画面の上部から下へスワイプ シミュレーターでのボタン、**通知センター**を選択、**今日** タブで、 をクリックし、**編集**ボタンをクリックします。

[![](extensions-images/run02.png "通知センターを開き、[今日] タブを選択して編集ボタンをクリックする画面の上部から下へスワイプ シミュレーターで [ホーム] ボタンをクリックします。")](extensions-images/run02.png#lightbox)

追加、 **DaysRemaining**拡張機能を**今日**表示し、クリックして、**完了**ボタン。

[![](extensions-images/run03.png "今日ビューに DaysRemaining 拡張機能を追加し、[終了] ボタンをクリックします。")](extensions-images/run03.png#lightbox)

新しいウィジェットに追加されます、**今日**ビューと結果が表示されます。

[![](extensions-images/run04.png "新しいウィジェットが今日ビューに追加され、結果が表示されます。")](extensions-images/run04.png#lightbox)

## <a name="communicating-with-the-host-app"></a>ホスト アプリケーションとの通信

上記で作成した拡張機能今日の例は、そのホストのアプリとは通信しません (、**今日**画面)。 使用した場合、 [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/)のプロパティ、`TodayViewController`または`CodeBasedViewController`クラス。 

そのアプリをホストからデータを受信する拡張機能、データの配列の形式で[NSExtensionItem](https://developer.xamarin.com/api/type/Foundation.NSExtensionItem/)オブジェクトに格納されている、 [InputItems](https://developer.xamarin.com/api/property/Foundation.NSExtensionContext.InputItems/)のプロパティ、 [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/)の拡張機能の`UIViewController`します。

写真編集拡張機能などの他の拡張機能は、完了またはキャンセルの使用状況、ユーザーの間で区別可能性があります。 これを使用してホスト アプリに通知される、 [CompleteRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CompleteRequest/)と[CancelRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CancelRequest/)メソッドの[ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/)プロパティ。

詳細については、Apple を参照してください[アプリ拡張機能のプログラミング ガイド](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1)します。

## <a name="communicating-with-the-parent-app"></a>親アプリとの通信

アプリ グループを使用すると、さまざまなアプリケーション (またはアプリケーションとその拡張機能) から共有ファイルの保存場所にアクセスできます。 アプリ グループは次のようなデータに使用できます。

- [Apple Watch 設定](~/ios/watchos/app-fundamentals/settings.md)します。
- [共有 NSUserDefaults](~/ios/app-fundamentals/user-defaults.md)します。
- [共有ファイル](~/ios/watchos/app-fundamentals/parent-app.md#files)します。

詳細についてを参照してください、[アプリ グループ](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)のセクション、**機能の使用**ドキュメント。

## <a name="mobilecoreservices"></a>MobileCoreServices

拡張機能を使用する場合は、作成し、アプリ、他のアプリやサービスの間で交換されるデータを操作する統一型識別子 (UTI) を使用します。

`MobileCoreServices.UTType`静的クラスには、Apple の関連する次のヘルパー プロパティを定義する`kUTType...`定義。

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

詳細についてを参照してください、[アプリ グループ](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)のセクション、**機能の使用**ドキュメント。

## <a name="precautions-and-considerations"></a>注意事項と考慮事項

拡張機能では、利用することをアプリよりもはるかに少ないメモリがあります。 ホストされているアプリをユーザーに最小限に抑え、迅速に実行する必要があります。 ただし、拡張機能は独特で実用的な関数、拡張機能の開発者を識別するためにユーザーに許可するブランド化された UI を使用したアプリを使うかに属しているコンテナー アプリも用意する必要があります。

これらの厳しい要件を指定するには、のみ、十分にテストしてパフォーマンスとメモリ使用量に対して最適化されている拡張機能をデプロイしてください。 

## <a name="summary"></a>まとめ

このドキュメントは、拡張機能は何か、拡張ポイントと拡張機能による iOS の既知の制限の種類をについて説明します。 作成、配布、インストール、および拡張機能と拡張機能のライフ サイクルを実行していることについて説明します。 作成、簡単なチュートリアルが提供されている**今日**ストーリー ボードまたはコードを使用して、ウィジェットの UI を作成する 2 つの方法を表示するウィジェット。 IOS シミュレーターでの拡張機能をテストする方法を示しました。 最後に、ホスト アプリケーションといくつかの予防措置と拡張機能を開発するときに実行すべき考慮事項と通信することを簡単に説明しました。 

## <a name="related-links"></a>関連リンク

- [ContainerApp (サンプル)](https://developer.xamarin.com/samples/monotouch/intro-to-extensions)
- [Xamarin.iOS での拡張機能を作成する (ビデオ)](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
