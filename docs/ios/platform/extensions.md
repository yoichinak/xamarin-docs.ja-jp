---
title: "iOS の拡張機能"
description: "IOS 8 で導入された、拡張機能は、ウィジェットが提示する、標準のコンテキストで iOS など、通知センター内で、ユーザーがカスタムのキーボードを要求するときや、写真を編集します。 すべての拡張機能では、コンテナー アプリと共にインストールされ、ホスト アプリケーションの特定の拡張機能ポイントからアクティブ化します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
ms.custom: xamu-video
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 6e0eebef2404ce3f117fe897d456f3ef78a8f585
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2018
---
# <a name="ios-extensions"></a>iOS の拡張機能

_IOS 8 で導入された、拡張機能は、ウィジェットが提示する、標準のコンテキストで iOS など、通知センター内で、ユーザーがカスタムのキーボードを要求するときや、写真を編集します。すべての拡張機能では、コンテナー アプリと共にインストールされ、ホスト アプリケーションの特定の拡張機能ポイントからアクティブ化します。_

> [!VIDEO https://youtube.com/embed/Sd0-ch9Udmk]

**IOS での拡張機能の作成[Xamarin 大学](https://university.xamarin.com/)**

拡張機能、8、iOS で導入された、特殊な`UIViewControllers`をから提示された標準のコンテキスト内の iOS など以内、**通知センター**を実行するユーザーによって要求されたカスタム キーボードの種類に特化した、入力やその他のコンテキストなどの特殊効果のフィルターを拡張機能が提供できる、写真を編集します。

すべての拡張機能では、(64 ビット Unified Api を使用して書き込まれる要素は両方) のコンテナー アプリと共にインストールされ、ホスト アプリケーションの特定の拡張機能ポイントからアクティブ化します。 高パフォーマンス、リーン、かつ堅牢なをする必要があるため、これらは、既存のシステム関数に追加ファイルとして使用するが、します。 

ここでは、次のトピックについて説明します。

- [拡張機能ポイント](#Extension-Points)-使用できる拡張ポイントの種類とポイントごとに作成できる拡張機能の種類を一覧表示します。
- [制限事項](#Limitations)-拡張機能があるいくつかの制限、これらの一部はすべての種類にユニバーサル他の種類の拡張機能では、利用状況に具体的な制限事項があります。
- [配布、インストール、および拡張機能を実行している](#Distributing-Installing-and-Running-Extensions)-拡張機能は、順番が送信され、アプリ ストア経由で配布されるコンテナー アプリ内から配布されます。 アプリの配布の拡張機能がその時点では、インストールされているが、ユーザーが明示的に各拡張機能を有効する必要があります。 さまざまな方法では、さまざまな種類の拡張機能が有効にします。
- [拡張機能のライフ サイクル](#Extension-Lifecycle)- 拡張機能の`UIViewController`がインスタンス化され、通常のビュー コント ローラーのライフ サイクルを開始します。 ただし、通常のアプリとは異なり拡張機能が読み込まれて、実行すると、繰り返しは終了します。
- [拡張機能の作成](#Creating-an-Extension)-、ソリューションが少なくとも 2 つのプロジェクトを含む拡張機能を開発する場合: コンテナーのアプリとプロジェクトの各拡張機能のコンテナーを提供します。
- [チュートリアル](#Walkthrough)- 単純なを作成するカバー**今日**ウィジェット ストーリー ボードを使用するか、ユーザー インターフェイスを提供する拡張機能、またはコードでは、拡張機能をインストールして、iOS シミュレーターでのテストします。
- [ホスト アプリケーションと通信する](#Communicating-with-the-Host-App)-拡張機能から、ホストのアプリとの通信について簡単に説明します。
- [親のアプリとの通信](#Communicating-with-the-Parent-App)-拡張機能から親アプリとの通信について簡単に説明します。
- [注意事項と考慮事項](#Precautions-and-Considerations)-リストの予防措置と設計および拡張機能を実装するときに考慮する必要がある考慮事項を理解します。
 

<a name="Extension-Points" />

## <a name="extension-points"></a>拡張ポイント

|型|説明|拡張点|ホスト アプリケーション|
|--- |--- |--- |--- |
|アクション|特殊なエディターまたは特定のメディアの種類のビューアー|`com.apple.ui-services`|どれでも可|
|ドキュメント プロバイダー|リモート ドキュメント ストアを使用するアプリに許可します。|`com.apple.fileprovider-ui`|アプリを使用して、 [UIDocumentPickerViewController](https://developer.xamarin.com/api/type/UIKit.UIDocumentPickerViewController/)|
|キーボード|代替キーボード|`com.apple.keyboard-service`|どれでも可|
|写真の編集|写真の操作と編集|`com.apple.photo-editing`|Photos.app エディター|
|共有|メッセージング サービスなどのソーシャル ネットワークとデータを共有します。|`com.apple.share-services`|どれでも可|
|今日|現在の画面または通知センターに表示される「ウィジェット」|`com.apple.widget-extensions`|今日と通知センター|

[その他の拡張点](~/ios/platform/introduction-to-ios10/index.md#app-extensions)iOS 10 に追加されました。

<a name="Limitations" />

## <a name="limitations"></a>制限事項

拡張機能がいくつかの制限、これらの一部はすべての種類にユニバーサル (拡張機能のない型のインスタンスは、カメラ、マイクにアクセスできます) があるときに、その他の種類の拡張機能では、それぞれの使用 (たとえば、カスタム キーボード具体的な制限事項があります使用できませんのパスワードのように、入力フィールドをセキュリティで保護されたデータの)。 

ユニバーサルの制限事項は次のとおりです。

- [ヘルス キット](~/ios/platform/healthkit.md)と[イベント キット UI](~/ios/platform/eventkit.md)フレームワークは使用できません
- 拡張機能を使用できない[バック グラウンド モードを拡張](http://developer.xamarin.com/guides/cross-platform/application_fundamentals/backgrounding/part_3_ios_backgrounding_techniques/registering_applications_to_run_in_background/)
- 拡張機能は、デバイスのカメラ、マイク (ただし、既存のメディア ファイルにアクセスする可能性があります)、アクセスできません。
- 拡張機能データを受け取れない空気の削除 (ただし、無線ドロップを使用してデータを送信することができます)
- [UIActionSheet](https://developer.xamarin.com/api/type/UIKit.UIActionSheet/)と[UIAlertView](https://developer.xamarin.com/api/type/UIKit.UIAlertView/)が表示されていない; 拡張機能を使用する必要があります[UIAlertController](https://developer.xamarin.com/api/type/UIKit.UIAlertController/)
- いくつかのメンバ[UIApplication](https://developer.xamarin.com/api/type/UIKit.UIApplication/)は使用できません: [UIApplication.SharedApplication](https://developer.xamarin.com/api/property/UIKit.UIApplication.SharedApplication/)、 `UIApplication.OpenURL`、`UIApplication.BeginIgnoringInteractionEvents`と `UIApplication.EndIgnoringInteractionEvents`
- iOS では、今日の拡張機能で 16 MB のメモリ使用制限を強制します。
- キーボード拡張機能は既定では、ネットワークへのアクセスがありません。 これは (制限は、シミュレーターでは適用されません)、デバイスでのデバッグに影響 Xamarin.iOS デバッグを行うにネットワーク アクセスを必要とするためです。 設定してネットワーク アクセス権を要求することは、`Requests Open Access`値に、プロジェクトの Info.plist で`Yes`です。 Apple を参照してください[カスタム キーボード ガイド](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html)キーボード拡張機能の制限についての詳細。

個々 の制限については、Apple を参照してください[アプリ拡張機能プログラミング ガイド](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/)です。

<a name="Distributing-Installing-and-Running-Extensions" />

## <a name="distributing-installing-and-running-extensions"></a>配布、インストール、および拡張機能を実行しています。

拡張機能は、コンテナー アプリであり、さらに送信される、アプリ ストア経由で配布から配布されます。 アプリの配布の拡張機能がその時点では、インストールされているが、ユーザーが明示的に各拡張機能を有効する必要があります。 別の方法でさまざまな種類の拡張機能が有効になっています。移動するユーザーが必要なもの、**設定**アプリし、そこからそれらを有効にします。 写真を送信するときに、共有の拡張機能を有効にするなど、使用するの時点で他のユーザーは有効です。 

拡張機能が使用されている (ユーザーは、拡張ポイントが検出した) アプリと呼びます、**ホスト アプリ**実行時に、拡張機能をホストするアプリケーションであるため、します。 拡張機能をインストールするアプリは、**コンテナー アプリ**されているインストールされている場合は、拡張機能をアプリになっているため、します。  

通常、コンテナー アプリケーションについて説明、拡張機能し、ユーザーがこれを有効にするプロセスを実行します。

<a name="Extension-Lifecycle" />

## <a name="extension-lifecycle"></a>拡張機能のライフ サイクル

拡張機能は、1 つの簡単な[UIViewController](https://developer.xamarin.com/api/type/UIKit.UIViewController/)または UI の複数の画面を表示するより複雑な拡張機能です。 ユーザーが検出した場合、_拡張ポイント_(場合など、イメージを共有)、その拡張機能ポイントの登録された拡張から選択することが必要であります。 

アプリのいずれかを選択した場合の拡張機能では、その`UIViewController`がインスタンス化され、通常のビュー コント ローラーのライフ サイクルを開始します。 ただし、これは中断されますが、では、一般に終了とやり取りするユーザーが終了したときに、通常のアプリとは異なり拡張機能が読み込まれて、実行すると繰り返しが終了し、します。

拡張機能を使用して、ホスト アプリケーションと通信できる、 [NSExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/)オブジェクト。 一部の拡張機能では、結果を使用した非同期のコールバックを受信する操作があります。 これらのコールバックはバック グラウンド スレッドで実行してを考慮します。 この拡張機能を考慮する必要があります。使用して、 [NSObject.InvokeOnMainThread](https://developer.xamarin.com/api/member/Foundation.NSObject.InvokeOnMainThread/)ユーザー インターフェイスを更新する場合。 参照してください、[ホスト アプリケーションと通信する](#Communicating-with-the-Host-App)詳細については、後述の「します。

既定では、拡張機能とそのコンテナー アプリ通信可能でない、一緒にインストールされているにもかかわらずです。 場合によっては、コンテナーのアプリは基本的には、空"shipping"コンテナーを目的とするが、拡張機能がインストールされるが提供です。 ただし、状況において求められている場合、コンテナーのアプリと、拡張機能可能性がありますリソースを共有の共通領域から。 さらに、**今日拡張子**を開くには、URL には、そのコンテナー アプリケーションを要求することができます。 この動作に表示される、[カウント ダウン ウィジェットの進化](http://github.com/xamarin/monotouch-samples/tree/master/ExtensionsDemo)です。

<a name="Creating-an-Extension" />

## <a name="creating-an-extension"></a>拡張機能の作成

拡張機能 (とそのコンテナー アプリ) は、64 ビット バイナリをする必要があり、Xamarin.iOS を使用して構築された[Unified Api](http://developer.xamarin.com/guides/cross-platform/macios/unified)です。 ソリューションが少なくとも 2 つのプロジェクトを含む拡張機能を開発するとき: コンテナーのアプリとプロジェクトの各拡張機能のコンテナーを提供します。 

<a name="Container-App-Project-Requirements" />

### <a name="container-app-project-requirements"></a>コンテナー アプリケーション プロジェクトの要件

拡張機能をインストールするために使用するコンテナー アプリには、次の要件があります。

- 拡張機能プロジェクトへの参照を保持するあります。   
- 完全なアプリ (起動して正常に実行できるようにする必要があります) する必要がある場合でも、それは拡張機能をインストールする方法を提供します。 
- 拡張機能のバンドル Id の基礎となるバンドル Id を持つ必要があります (詳細については、以下のセクションを参照してください) プロジェクトします。

<a name="Extension-Project-Requirements" />

### <a name="extension-project-requirements"></a>拡張機能プロジェクトの要件

さらに、拡張機能のプロジェクトでは、次の要件があります。

- そのコンテナー アプリのバンドル Id で始まるバンドル Id が必要です。 たとえば、コンテナー アプリケーションの識別子が存在するバンドルの`com.myCompany.ContainerApp`、拡張機能の識別子に`com.myCompany.ContainerApp.MyExtension`: 

    ![](extensions-images/bundleidentifiers.png) 
- キーを定義する必要があります`NSExtensionPointIdentifier`、適切な値 (など`com.apple.widget-extension`の**今日**通知センターのウィジェット) で、その`Info.plist`ファイル。
- 定義もあります*か*、`NSExtensionMainStoryboard`キーまたは`NSPrincipalClass`でキーの`Info.plist`適切な値を持つファイル。
    - 使用して、`NSExtensionMainStoryboard`拡張機能の主要な UI を提示するストーリー ボードの名前を指定するキー (マイナス`.storyboard`)。 たとえば、`Main`の`Main.storyboard`ファイル。
    - 使用して、`NSPrincipalClass`拡張機能が開始されたときに初期化されるクラスを指定するキー。 値に一致する必要があります、**登録**の値、 `UIViewController`: 

    ![](extensions-images/registerandprincipalclass.png)

特定の種類の拡張機能の追加要件があります。 インスタンス、**今日**または**通知センター**拡張機能の重要なクラスを実装する必要があります[INCWidgetProviding](https://developer.xamarin.com/api/type/NotificationCenter.INCWidgetProviding/)です。

> [!IMPORTANT]
> **注:**これら (全部ではない場合は、ほとんどの要件が提供され満たすには、テンプレートを自動的にするのいずれかの Mac を Visual Studio によって提供される拡張機能のテンプレートを使用して、プロジェクトを開始する場合。

<a name="Walkthrough" />

## <a name="walkthrough"></a>チュートリアル 

次のチュートリアルでは、例を作成します**今日**日と年の残り日数の数を計算するウィジェット。

[![](extensions-images/carpediemscreenshot-sm.png "1 日と年の残り日数の数を計算する例今日ウィジェット")](extensions-images/carpediemscreenshot.png#lightbox)

<a name="Creating-the-Solution" />

### <a name="creating-the-solution"></a>ソリューションの作成

必要なソリューションを作成するには、次の操作を行います。

1. 新しい iOS の場合を最初に、作成**1 つのアプリの表示**プロジェクトし、をクリックして、**次**ボタン。 

    [![](extensions-images/today01.png "まず、新しい iOS、1 つのアプリの表示のプロジェクトを作成し、[次へ] をクリックしてください")](extensions-images/today01.png#lightbox)
2. このプロジェクト`TodayContainer` をクリックし、**次**ボタン。 

    [![](extensions-images/today02.png "プロジェクト TodayContainer を呼び出すし、[次へ] をクリックしてください")](extensions-images/today02.png#lightbox)
3. 確認してください、**プロジェクト名**と**SolutionName**  をクリックし、**作成**ソリューションを作成するにはボタン。 

    [![](extensions-images/today03.png "SolutionName とプロジェクトの名前を確認し、ソリューションを作成する [作成] をクリックしてください")](extensions-images/today03.png#lightbox)
4. 次に、**ソリューション エクスプ ローラー**、ソリューションを右クリックし、新しい**iOS 拡張子**プロジェクトから、**今日拡張子**テンプレート。 

    [![](extensions-images/today04.png "次に、ソリューション エクスプ ローラーでソリューションを右クリックし、拡張機能では現在のテンプレートから新しい iOS の拡張機能プロジェクトを追加")](extensions-images/today04.png#lightbox)
5. このプロジェクト`DaysRemaining` をクリックし、**次**ボタン。 

    [![](extensions-images/today05.png "プロジェクト DaysRemaining を呼び出すし、[次へ] をクリックしてください")](extensions-images/today05.png#lightbox)
6. プロジェクトを確認し、をクリックして、**作成**それを作成するボタンをクリックします。 

    [![](extensions-images/today06.png "プロジェクトを確認し、それを作成する [作成] をクリックしてください")](extensions-images/today06.png#lightbox)

結果として得られるソリューションが 2 つのプロジェクトでは、次のように。

[![](extensions-images/today07.png "結果として得られるソリューションが 2 つのプロジェクトでは、次のように")](extensions-images/today07.png#lightbox)

<a name="Creating-the-Extension-User-Interface" />

### <a name="creating-the-extension-user-interface"></a>拡張ユーザー インターフェイスの作成

次に、必要がありますのインターフェイスを設計、**今日**ウィジェット。 これを行うか、ストーリー ボードを使用するか、コードで UI を作成します。 どちらの方法については、説明の下の詳細。

<a name="Using-Storyboards" />

#### <a name="using-storyboards"></a>ストーリー ボードを使用します。

ストーリー ボードで UI をビルドするには、次の操作を行います。

1. **ソリューション エクスプ ローラー**、拡張機能プロジェクトのダブルクリック`Main.storyboard`ファイルを開いて編集するファイル。 

    [![](extensions-images/today08.png "ファイルを開いて編集するには、拡張機能プロジェクト Main.storyboard ファイルをダブルクリックします。")](extensions-images/today08.png#lightbox)
2. テンプレートによって、UI に自動的に追加されたラベルを選択し、**名前**`TodayMessage`で、**ウィジェット**のタブ、**プロパティ エクスプ ローラー**: 

    [![](extensions-images/today09.png "テンプレートによって、UI に自動的に追加されたラベルを選択し、プロパティ エクスプ ローラーのウィジェット タブで名前 TodayMessage")](extensions-images/today09.png#lightbox)
3. ストーリー ボードに、変更を保存します。

<a name="Using-Code" />

#### <a name="using-code"></a>コードの使用

コードで UI をビルドするには、次の操作を行います。 

1. **ソリューション エクスプ ローラー**、select、 **DaysRemaining**プロジェクト、新しいクラスを追加して、 `CodeBasedViewController`: 

    [![](extensions-images/code01.png "Aelect DaysRemaining プロジェクトは、新しいクラスを追加および CodeBasedViewController の呼び出し")](extensions-images/code01.png#lightbox)
2. もう一度、**ソリューション エクスプ ローラー**、拡張機能のダブルクリック`Info.plist`ファイルを開いて編集するファイル。 

    [![](extensions-images/code02.png "ファイルを開いて編集する拡張機能の Info.plist ファイルをダブルクリックします。")](extensions-images/code02.png#lightbox)
3. 選択、**ソース ビュー** (画面の下部にある) から開くと、`NSExtension`ノード。 

    [![](extensions-images/code03.png "画面の下部から、ソース ビューを選択し、NSExtension ノードを開きます")](extensions-images/code03.png#lightbox)
4. 削除、`NSExtensionMainStoryboard`キーし、追加、`NSPrincipalClass`値を持つ`CodeBasedViewController`: 

    [![](extensions-images/code04.png "NSExtensionMainStoryboard キーを削除し、値 CodeBasedViewController NSPrincipalClass の追加")](extensions-images/code04.png#lightbox)
5. 変更内容を保存します。

次に、編集、`CodeBasedViewController.cs`ファイルし、次のようになります。

```csharp
using System;
using Foundation;
using UIKit;
using NotificationCenter;
using CoreGraphics;

namespace DaysRemaining
{
    [Register("CodeBasedViewContoller")]
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

なお、`[Register("CodeBasedViewContoller")]`用に指定された値と一致する、`NSPrincipalClass`上。

<a name="Coding-the-Extension" />

### <a name="coding-the-extension"></a>拡張機能をコーディング

作成、ユーザー インターフェイスには、いずれかを開き、`TodayViewController.cs`または`CodeBasedViewController.cs`ファイル (上記のユーザー インターフェイスの作成に使用する方法のベース)、変更、 **ViewDidLoad**メソッドされ次のようになります。

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

ベースの場合、コードを使用してユーザー インターフェイスのメソッド、置換、`// Insert code to power extension here...`上記の新しいコードをコメントします。 後に基本実装を呼び出す (および基になるコードのバージョンについては、ラベルの挿入)、このコードは単純な計算を曜日、年と、残りの日数を取得します。 ラベル内のメッセージを表示し、(`TodayMessage`) UI の設計で作成しました。

このプロセスは通常、アプリの作成のプロセスを類似性に注意してください。 拡張機能の`UIViewController`アプリケーションでは、ビュー コント ローラーと同じライフ サイクルを持つ拡張機能のバック グラウンド モードがないと、ユーザーが完了したときに保留されません点を除いて、これらを使用します。 代わりに、拡張機能は繰り返し初期化され、必要に応じて割り当てが解除されます。

<a name="Creating-the-Container-App-User-Interface" />

### <a name="creating-the-container-app-user-interface"></a>コンテナー アプリケーションのユーザー インターフェイスの作成

このチュートリアルでは、コンテナー アプリは出荷方法と、拡張機能をインストールするメソッドとして使用するだけと、独自の機能は備えていません。 編集 TodayContainer の`Main.storyboard`ファイルし、拡張機能の関数とそのインストール方法を定義するテキストを追加します。

[![](extensions-images/today10.png "TodayContainers Main.storyboard ファイルを編集し、拡張関数とそのインストール方法を定義するテキストを追加")](extensions-images/today10.png#lightbox)

ストーリー ボードに、変更を保存します。

<a name="Testing-the-Extension" />

### <a name="testing-the-extension"></a>拡張機能のテスト

IOS シミュレーターで、拡張機能をテストするには、実行、 **TodayContainer**アプリ。 コンテナーのメイン ビューが表示されます。

[![](extensions-images/run01.png "コンテナーのメイン ビューが表示されます。")](extensions-images/run01.png#lightbox)

次に、ヒット、**ホーム**を開くには、画面の一番上から下へスワイプして、シミュレーターのボタン、**通知センター**、select、**今日** タブで、 をクリックし、**編集**ボタンをクリックします。

[![](extensions-images/run02.png "シミュレーターを通知センターを開き、[今日] タブを選択し、[編集] ボタンをクリックして、画面の一番上から下方向にスワイプのホーム ボタンをクリックします。")](extensions-images/run02.png#lightbox)

追加、 **DaysRemaining**拡張機能を**今日**表示し、クリックして、**実行**ボタン。

[![](extensions-images/run03.png "今日ビューに DaysRemaining 拡張機能を追加し、元に戻す ボタンをクリックしてください")](extensions-images/run03.png#lightbox)

追加する新しいウィジェット、**今日**ビューと結果が表示されます。

[![](extensions-images/run04.png "新しいウィジェットが今日ビューに追加され、結果が表示されます。")](extensions-images/run04.png#lightbox)

<a name="Communicating-with-the-Host-App" />

## <a name="communicating-with-the-host-app"></a>ホスト アプリケーションとの通信

上記で作成した拡張今日の例は、ホスト アプリケーションとは通信しません (、**今日**画面)。 使用する場合は、 [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/)のプロパティ、`TodayViewController`または`CodeBasedViewController`クラスです。 

そのホストのアプリからデータを受信する拡張機能、データの配列の形式で[NSExtensionItem](https://developer.xamarin.com/api/type/Foundation.NSExtensionItem/)オブジェクトに格納されている、 [InputItems](https://developer.xamarin.com/api/property/Foundation.NSExtensionContext.InputItems/)のプロパティ、 [ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/)の拡張機能の`UIViewController`します。

写真編集の拡張機能など、他の拡張機能は、完了または使用状況を取り消すと、ユーザーの区別可能性があります。 これを使用してホスト アプリケーションに通知される、 [CompleteRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CompleteRequest/)と[CancelRequest](https://developer.xamarin.com/api/member/Foundation.NSExtensionContext.CancelRequest/)のメソッド[ExtensionContext](https://developer.xamarin.com/api/type/Foundation.NSExtensionContext/)プロパティです。

詳細については、Apple を参照してください[アプリ拡張機能プログラミング ガイド](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1)です。

<a name="Communicating-with-the-Parent-App" />

## <a name="communicating-with-the-parent-app"></a>親のアプリとの通信

アプリ グループを使用すると、さまざまなアプリケーション (またはアプリケーションとその拡張機能) から共有ファイルの保存場所にアクセスできます。 アプリ グループは次のようなデータに使用できます。

- [Apple Watch 設定](~/ios/watchos/app-fundamentals/settings.md)です。
- [共有 NSUserDefaults](~/ios/app-fundamentals/user-defaults.md)です。
- [共有ファイル](~/ios/watchos/app-fundamentals/parent-app.md#files)です。

詳細についてを参照してください、[アプリ グループ](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)のセクションで、**機能を備えた作業**ドキュメント。

<a name="MobileCoreServices" />

## <a name="mobilecoreservices"></a>MobileCoreServices

拡張機能を使用する場合は、作成し、アプリ、その他のアプリやサービス間で交換されるデータを操作する、統一された型の識別子 (ユーティリ ティー) を使用します。

`MobileCoreServices.UTType`静的クラスには、Apple の関連する次のヘルパー プロパティを定義`kUTType...`定義。

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

詳細についてを参照してください、[アプリ グループ](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)のセクションで、**機能を備えた作業**ドキュメント。


<a name="Precautions-and-Considerations" />

## <a name="precautions-and-considerations"></a>注意事項と考慮事項

拡張機能では、利用できるアプリよりもはるかに少ないメモリがあります。 予想されるを迅速かつでホストされているアプリをユーザーに最小限に抑えを実行します。 ただし、拡張機能では、ブランド化された UI 拡張機能の開発者を識別するアクセス許可を使用してアプリを使用またはに属しているコンテナー アプリに個性的な実用的な関数が用意もする必要があります。

これらの厳しい要件を指定するには、のみを十分にテスト、パフォーマンスおよびメモリ消費の最適化された拡張機能を展開する必要があります。 

<a name="Summary" />

## <a name="summary"></a>まとめ

このドキュメントとは何か、拡張機能ポイントおよび iOS での拡張機能に適用される既知の制限事項の種類の拡張機能について説明しました。 これには、作成、配布、インストールおよび拡張機能と拡張機能のライフ サイクルを実行しているがについて説明します。 ファイルの作成、簡単なチュートリアルを提供して**今日**ウィジェットがウィジェットの UI をストーリー ボードまたはコードを使用して作成する 2 つの方法を示すです。 IOS シミュレーターでの拡張機能をテストする方法を示しました。 最後に、ホスト アプリケーションといくつかの予防措置と拡張機能を開発するときに行う必要がありますの考慮事項と通信することを簡単に説明しました。 


## <a name="related-links"></a>関連リンク

- [ContainerApp (サンプル)](https://developer.xamarin.com/samples/monotouch/intro-to-extensions)
- [Xamarin.iOS での拡張機能を作成する (ビデオ)](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
