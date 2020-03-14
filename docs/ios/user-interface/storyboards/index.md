---
title: Xamarin のストーリーボードの概要
description: このドキュメントでは、Xamarin のストーリーボードの概要について説明します。 ここでは、ストーリーボードを使用してユーザーインターフェイスを定義する方法、セグエ、および iOS Designer を使用してストーリーボードファイルを編集する方法について説明します。
ms.prod: xamarin
ms.assetid: A3339BD2-9F56-7965-25F5-4B7C991EB775
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 3d9531bbaa38a0a0a9bb42d22c79a63fda75c449
ms.sourcegitcommit: 926f9a44065b41c7d74794a94bee66b61e903bf2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79134004"
---
# <a name="introduction-to-storyboards-in-xamarinios"></a>Xamarin のストーリーボードの概要

このガイドでは、ストーリーボードについて説明し、主要なコンポーネント (セグエなど) の一部を確認します。 ここでは、ストーリーボードの作成方法と使用方法、および開発者にとっての利点について説明します。

IOS アプリケーションの UI を視覚的に表現するために、Apple によってストーリーボードファイル形式が導入される前に、開発者は各ビューコントローラーに対して XIB ファイルを作成し、各ビュー間の移動を手動でプログラミングしました。  ストーリーボードを使用すると、開発者は、ビューコントローラーと、デザインサーフェイス上のビューコントローラー間のナビゲーションの両方を定義し、アプリケーションのユーザーインターフェイスを WYSIWYG で編集できます。

ストーリーボードを作成し、開いて、Xamarin iOS Designer で編集することができます。 このガイドでは、を使用してナビゲーションをプログラミングするときに、 C#デザイナーを使用してストーリーボードを作成する方法についても説明します。

## <a name="requirements"></a>要件

ストーリーボードは、Xcode、Visual Studio for Mac の iOS デザイナー、Xamarin のワークロードがインストールされた Visual Studio 2019 で使用できます。

## <a name="what-is-a-storyboard"></a>ストーリーボードとは

ストーリーボードは、アプリケーションのすべての画面を視覚的に表現したものです。 これには一連のシーンが含まれており、各シーンは*ビューコントローラー*とその*ビュー*を表します。 これらのビューには、ユーザーがアプリケーションと対話できるようにするオブジェクトと[コントロール](~/ios/user-interface/controls/index.md)が含まれている場合があります。 このビューとコントロール *(または*サブビュー) のコレクションは、*コンテンツビュー階層*と呼ばれます。 シーンはセグエオブジェクトによって接続されます。これは、ビューコントローラー間の遷移を表します。 これは通常、初期ビューのオブジェクトと接続ビューの間にセグエを作成することによって実現されます。 次の図は、デザイン画面上のリレーションシップを示しています。

 [![](images/storyboardsview.png "The relationships on the design surface are illustrated in this image")](images/storyboardsview.png#lightbox)

このように、ストーリーボードでは、既にレンダリングされているコンテンツを使用して各シーンをレイアウトし、それらの間の接続を示します。  この時点では、iPhone でシーンについて話すときに、ストーリーボード上の1つの*シーン*がデバイス上のコンテンツの1つの*画面*と同じであると想定するのは安全です。 ただし、iPad では、複数のシーンを一度に表示することができます (たとえば、Segue view controller を使用します)。

特に Xamarin を使用する場合、ストーリーボードを使用してアプリケーションの UI を作成することには多くの利点があります。 まず、UI が視覚的に表現されます。これは、[カスタムコントロール](~/ios/user-interface/designer/ios-designable-controls-overview.md)を含むすべてのオブジェクトがデザイン時にレンダリングされるためです。
これは、アプリケーションをビルドまたは配置する前に、その外観とフローを視覚化できることを意味します。 たとえば、上の図を見てください。 デザインサーフェイスのシーン数、各ビューのレイアウト、およびすべての関連について簡単に確認できます。 これにより、ストーリーボードが強力になります。

イベントは、特に iOS Designer を使用する場合に、ストーリーボードで管理しやすくなります。 ほとんどの UI コントロールには、Properties Pad に考えられるイベントの一覧があります。 イベントハンドラーはここに追加し、ビューコントローラークラスの部分メソッドで完了できます。「」を参照してください。

ストーリーボードのコンテンツは、XML ファイルとして格納されます。 ビルド時には、すべての `.storyboard` ファイルが、"ファイル" と呼ばれるバイナリファイルにコンパイルされます。 実行時に、これらのインスタンスは初期化され、新しいビューを作成するためにインスタンス化されます。

## <a name="segues"></a>セグエ

*セグエ*または*セグエオブジェクト*は、バックグラウンド間の遷移を表すために、iOS 開発で使用されます。 セグエを作成するには、 **Ctrl**キーを押しながら1つのシーンから別のシーンへドラッグします。 マウスをドラッグすると、青いコネクタが表示され、次の図に示すように、セグエの潜在顧客が示されます。

 [![](images/createsegue.png "A blue connector appears, indicating where the segue will lead as demonstrated in this image")](images/createsegue.png#lightbox)

マウスポインターを押すと、セグエの操作を選択できるメニューが表示されます。 次の図のようになります。

**IOS より前の8とサイズのクラス**:

[![](images/segue1.png "The Action Segue dropdown without Size Classes")](images/segue1.png#lightbox)

**サイズクラスとアダプティブセグエを使用する場合**:

[![](images/16new.png "The Action Segue dropdown with Size Classes")](images/16new.png#lightbox)

> [!IMPORTANT]
> Windows 仮想マシンに VMWare を使用している場合、Ctrl キーを押しながらクリックすると、既定で_右クリック_のマウスボタンとしてマップされます。 セグエを作成するには、次に示すように、**キーボード & マウス** > **マウスのショートカット**を > し、**セカンダリボタン**を再マップ**して、** キーボードの基本設定を編集します。
>
> [![](images/image22.png "Keyboard and Mouse preference settings")](images/image22.png#lightbox)
>
> これで、ビューコントローラー間に通常どおりセグエを追加できるようになります。

遷移にはさまざまな種類があり、それぞれが新しいビューコントローラーをユーザーに提示する方法と、ストーリーボード内の他のビューコントローラーとどのように対話するかを制御します。 これらについて以下に説明します。 セグエオブジェクトをサブクラス化して、カスタム遷移を実装することもできます。

- **Show/push** –プッシュセグエは、ビューコントローラーをナビゲーションスタックに追加します。 これは、プッシュを送信するビューコントローラーが、スタックに追加されるビューコントローラーと同じナビゲーションコントローラーの一部であることを前提としています。 これは `pushViewController` と同じです。画面上のデータ間にリレーションシップがある場合に一般的に使用されます。 プッシュセグエを使用すると、ナビゲーションバーにスタック上の各ビューに [戻る] ボタンとタイトルを追加することで、ビュー階層内のドリルダウンナビゲーションが可能になります。
- **[モーダル]** –モーダルセグエは、プロジェクト内の任意の2つのビューコントローラー間のリレーションシップを、アニメーション化された遷移を示すオプションで作成します。 子ビューコントローラーは、表示されたときに親ビューコントローラーを完全に隠すことができます。 プッシュセグエとは異なり、[戻る] ボタンが追加されます。モーダルセグエを使用する場合は、前のビューコントローラーに戻るために `DismissViewController` を使用する必要があります。
- **Custom** –任意のカスタムセグエを `UIStoryboardSegue`のサブクラスとして作成できます。
- **アンワイン**ド–アンワインドセグエを使用して、プッシュまたはモーダルセグエに戻ることができます。たとえば、モーダルで示されたビューコントローラーを終了します。 これに加えて、1つだけではなく、一連のプッシュとモーダルセグエを使用してアンワインドし、1つのアンワインドアクションでナビゲーション階層内の複数のステップを戻ることができます。 IOS でアンワインドセグエを使用する方法については、[アンワインドセグエの作成](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/unwind_segue)に関するレシピをご覧ください。
- **ソースレス**–セグエは、初期ビューコントローラーを含むシーンを示します。したがって、ユーザーが最初に表示されるビューを示します。 次に示すセグエによって表されます。  

    [![](images/sourcelesssegue.png "A sourceless segue")](images/sourcelesssegue.png#lightbox)

### <a name="adaptive-segue-types"></a>Adaptive セグエの種類

 iOS 8 では、使用可能なすべての画面サイズで iOS ストーリーボードファイルを操作できるように[サイズクラス](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes)が導入されました。これにより、開発者はすべての ios デバイス用に1つの UI を作成できます。 既定では、すべての新しい Xamarin iOS アプリケーションでサイズクラスが使用されます。 以前のプロジェクトのサイズクラスを使用するには、統合された[ストーリーボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)に関するガイドを参照してください。

サイズクラスを使用するアプリケーションでは、新しい[*Adaptive セグエ*](~/ios/user-interface/storyboards/unified-storyboards.md)も使用されます。 サイズクラスを使用する場合は、iPhone または iPad を使用している天気を直接指定していないことに注意してください。 つまり、どのくらいの作業が必要であるかに関係なく、常に同じように表示される1つの UI を作成しています。 Adaptive セグエ work は、環境を審査し、コンテンツをどの程度最適に提示するかを決定します。 Adaptive セグエを次に示します。

[![](images/adaptivesegue.png "The Adaptive Segues dropdown")](images/adaptivesegue.png#lightbox)

|セグエ|説明|
|--- |--- |
|表示|これはプッシュセグエとよく似ていますが、画面の内容を考慮しています。|
|［詳細を表示］|アプリがマスタービューと詳細ビューを表示する場合 (iPad の分割ビューコントローラーなど)、詳細ビューがコンテンツに置き換えられます。 アプリがマスターまたは詳細のみを表示する場合、コンテンツはビューコントローラースタックの上部を置き換えます。|
|プレゼンテーション|これはモーダルセグエに似ており、プレゼンテーションと切り替えのスタイルを選択できます。|
|Segue プレゼンテーション|これにより、コンテンツが segue として表示されます。|

### <a name="transferring-data-with-segues"></a>セグエを使用したデータの転送

セグエの利点は遷移で終了しません。 また、ビューコントローラー間のデータ転送を管理するために使用することもできます。 これは、初期ビューコントローラーで `PrepareForSegue` メソッドをオーバーライドし、データを処理することで実現されます。 セグエがトリガーされると (たとえば、ボタンが押された場合)、アプリケーションはこのメソッドを呼び出し、ナビゲーションが発生*する前に*新しいビューコントローラーを準備する機会を提供します。 次のコードは、次の例[では、](https://docs.microsoft.com/samples/xamarin/ios-samples/hello-ios)これを示しています。

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue,
NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryController = segue.DestinationViewController
                                  as CallHistoryController;

    if (callHistoryController != null) {
        callHistoryController.PhoneNumbers = PhoneNumbers;
    }
}
```

この例では、セグエがユーザーによってトリガーされたときに、`PrepareForSegue` メソッドが呼び出されます。 まず、' 受信 ' ビューコントローラーのインスタンスを作成し、これをセグエの宛先ビューコントローラーとして設定する必要があります。 これを行うには、次のコード行を実行します。

```csharp
var callHistoryController = segue.DestinationViewController as CallHistoryController;
```

メソッドに、`DestinationViewController`のプロパティを設定する機能が用意されました。 この例では、`PhoneNumbers` というリストを `CallHistoryController` に渡し、同じ名前のオブジェクトに割り当てることによって、この機能を利用しています。

```csharp
if (callHistoryController != null) {
        callHistoryController.PhoneNumbers = PhoneNumbers;
    }
```

移行が完了すると、ユーザーには、設定された一覧を含む `CallHistoryController` が表示されます。

## <a name="adding-a-storyboard-to-a-non-storyboard-project"></a>ストーリーボード以外のプロジェクトへのストーリーボードの追加

場合によっては、以前のストーリーボード以外のファイルにストーリーボードを追加することが必要になる場合があります。 これを Visual Studio for Mac で実行すると、次の手順に従って合理化できます。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

1. 次に示すように、 **iOS > storyboard > [ファイル > 新しいファイル**] を参照して、新しいストーリーボードファイルを作成します。

    [![](images/new-storyboard-xs.png "The new file dialog")](images/new-storyboard-xs.png#lightbox)

2. 次に示すように、**情報を plist**の**Main インターフェイス**セクションにストーリーボード名を追加します。

    [![](images/infoplist.png "The Info.plist editor")](images/infoplist.png#lightbox)

    これは、アプリデリゲート内の `FinishedLaunching` メソッドで初期ビューコントローラーをインスタンス化するのと同じ結果をもたらします。 このオプションを設定すると、アプリケーションはウィンドウ (下記参照) をインスタンス化し、メインのストーリーボードを読み込み、ストーリーボードの初期ビューコントローラー (ソースレスセグエの横) のインスタンスをウィンドウの [`RootViewController`] プロパティとして割り当て、ウィンドウを画面に表示します。

3. `AppDelegate`で、次のコードを使用して既定の `Window` メソッドをオーバーライドし、ウィンドウプロパティを実装します。

    ```csharp
    public override UIWindow Window {
        get;
        set;
    }
    ```

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. 次に示すように、新しい**ファイル > iOS > 空のストーリーボードに追加**するプロジェクトを右クリックして、新しいストーリーボードファイルを作成 > ます。

    [![](images/new-storyboard-vs.png "The new item dialog")](images/new-storyboard-vs.png#lightbox)

2. 次に示すように、iOS アプリケーションの**メインインターフェイス**セクションにストーリーボード名を追加します。

    [![](images/ios-app.png "The Info.plist editor")](images/ios-app.png#lightbox)

    これは、アプリデリゲート内の `FinishedLaunching` メソッドで初期ビューコントローラーをインスタンス化するのと同じ結果をもたらします。 このオプションを設定すると、アプリケーションはウィンドウ (下記参照) をインスタンス化し、メインのストーリーボードを読み込み、ストーリーボードの初期ビューコントローラー (ソースレスセグエの横) のインスタンスをウィンドウの [`RootViewController`] プロパティとして割り当て、ウィンドウを画面に表示します。

3. `AppDelegate`で、次のコードを使用して既定の `Window` メソッドをオーバーライドし、ウィンドウプロパティを実装します。

    ```csharp
    public override UIWindow Window {
        get;
        set;
    }
    ```

-----

## <a name="creating-a-storyboard-with-xcode"></a>Xcode を使用したストーリーボードの作成

Visual Studio for Mac で開発した iOS アプリで使用するために、Xcode を使用してストーリーボードを作成および変更できます。

ストーリーボードには、プロジェクト内の個々の XIB ファイルが完全に置き換えられます。ただし、ストーリーボード内の個々のビューコントローラーは `Storyboard.InstantiateViewController`を使用してインスタンス化できます。

アプリケーションによっては、デザイナーによって提供される組み込みのストーリーボード遷移で処理できない特殊な要件がある場合があります。 たとえば、アプリケーションの現在の状態に応じて、同じボタンから異なる画面を起動するアプリケーションを作成する場合は、ビューコントローラーを手動でインスタンス化し、自分で移行をプログラムすることをお勧めします。

次のスクリーンショットは、デザイン画面上の2つのビューコントローラーの間にセグエがないことを示しています。 次のセクションでは、コードで遷移を設定する方法について説明します。

1. 空の_IPhone ストーリーボード_を既存のプロジェクトプロジェクトに追加します。

    [![](images/add-storyboard2.png "Adding storyboard")](images/add-storyboard2.png#lightbox)

2. ストーリーボードファイルを右クリックし、 **[> Xcode Interface Builder で開く]** を選択して、Xcode で開きます。

    *既定で Xcode Interface builder を使用する場合は、[プロジェクト] の [Visual Studio for Mac の設定] で [ **iOS] >** 選択できます。*

![](images/set-preferred-designer-tool.png "Selecting the preferred designer tool")

3. Xcode で、ライブラリを開きます (**ビュー > [ライブラリ**の表示] または [ *Shift + Command + L*] を使用)。これにより、ストーリーボードに追加できるオブジェクトの一覧が表示されます。 一覧からストーリーボードにオブジェクトをドラッグして、ストーリーボードに `Navigation Controller` を追加します。 既定では、`Navigation Controller` は2つの画面を提供します。右側の画面は `TableViewController` であり、より単純なビューに置き換えます。そのため、ビューをクリックし、Del キーを押して削除できます。

    [![](images/add-navigation-controller.png "Adding a NavigationController from the Library")](images/add-navigation-controller.png#lightbox)

4. このビューコントローラーには独自のカスタムクラスがあり、独自のストーリーボード ID も必要です。 この新しく追加したビューの上にあるボックスをクリックすると、3つのアイコンが表示されます。左端はビューのビューコントローラーを表します。 このアイコンを選択すると、右側のウィンドウの [id] タブでクラスと ID の値を設定できます。これらの値を `MainViewController` に設定し、`Use Storyboard ID`を確認してください。

    [![](images/identity-panel.png "Setting the MainViewController in the identity panel")](images/identity-panel.png#lightbox)

5. もう一度ライブラリを使用して、ビューコントローラーを画面にドラッグします。 これはルートビューコントローラーとして設定されます。 Ctrl キーを押しながら、左側のナビゲーションコントローラーから右に新しく追加されたビューコントローラーをクリックしてドラッグし、メニューの [*ルートビューコントローラー* ] をクリックします。

    [![](images/add-view-controller.png "Adding a NavigationController from the Library and setting the MainViewController as a Root View Controller")](images/add-view-controller.png#lightbox)

6. このアプリは別のビューに移動します。そのため、前と同様に、ストーリーボードにもう1つビューを追加します。 これを `PinkViewController`と呼びます。これらの値は、`MainViewController`と同じ方法で設定できます。

    [![](images/add-additional-view-controller.png "Adding an additional View Controller")](images/add-additional-view-controller.png#lightbox)

7. ビューコントローラーはピンク色で表示されるため、[属性] パネルで [`Background`] の横のドロップダウンを使用して、そのプロパティを設定できます。

    [![](images/set-pink-background.png "Adding an additional View Controller")](images/set-pink-background.png#lightbox)

8. `MainViewController` が `PinkViewController`に移動するようにするため、前者にはと対話するためのボタンが必要です。 ライブラリを使用して、`MainViewController`にボタンを追加できます。

    [![](images/add-button.png "Adding a Button to the MainViewController")](images/add-button.png#lightbox)

ストーリーボードは完了しましたが、ここでプロジェクトをデプロイすると、空の画面が表示されます。 それは、ストーリーボードを使用するように IDE に指示する必要があり、最初のビューとして機能するようにルートビューコントローラーを設定する必要があるためです。 通常、これは、上記のように、プロジェクトオプションを使用して行うことができます。 ただし、この例では、次のコードを**Appdelegate**に追加することにより、同じ結果が得られます。

```csharp
public partial class AppDelegate : UIApplicationDelegate
{
    UIWindow window;
    public static UIStoryboard Storyboard = UIStoryboard.FromName ("MainStoryboard", null);
    public static UIViewController initialViewController;

    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        window = new UIWindow (UIScreen.MainScreen.Bounds);

        initialViewController = Storyboard.InstantiateInitialViewController () as UIViewController;

        window.RootViewController = initialViewController;
        window.AddSubview(initialViewController.View);
        window.MakeKeyAndVisible ();
        return true;
    }
}
```

これは多くのコードですが、見慣れない数行の行しかありません。 まず、ストーリーボードの名前**mainstoryboard.storyboard ファイル**を渡すことによって、このストーリーボードを**appdelegate**に登録します。 次に、ストーリーボードで `InstantiateInitialViewController` を呼び出してストーリーボードから初期ビューコントローラーをインスタンス化するようにアプリケーションに指示し、そのビューコントローラーをアプリケーションのルートビューコントローラーとして設定します。 このメソッドは、ユーザーに表示される最初の画面を確認し、そのビューコントローラーの新しいインスタンスを作成します。

ソリューションウィンドウでは、IDE によって `MainViewcontroller.cs` クラスが作成されたことがわかります。手順 4. の Properties Pad にクラス名を追加したときは、その `corresponding designer.cs` が表示されます。 このクラスは、基底クラスを含む特殊なコンストラクターを作成したことがわかります。

```csharp
public MainViewController (IntPtr handle) : base (handle)
{
}
```

Xcode を使用してストーリーボードを作成すると、IDE によって `designer.cs` クラスの先頭に[[Register]](xref:Foundation.RegisterAttribute)属性が自動的に追加され、文字列識別子が渡されます。これは、前の手順で指定したストーリーボード ID と同じです。 これにより、 C#がストーリーボードの関連シーンにリンクされます。

```csharp
[Register ("MainViewController")]
public partial class MainViewController : UIViewController
{
    public MainViewController (IntPtr handle) : base (handle)
    {
    }
    //...
}
```

クラスとメソッドの登録の詳細については、[型レジストラー](https://docs.microsoft.com/xamarin/ios/internals/registrar)のドキュメントを参照してください。

このクラスの最後の手順では、ボタンを接続し、ピンクのビューコントローラーに切り替えます。 ストーリーボードから `PinkViewController` をインスタンス化します。次に、以下のコード例に示すように、`PushViewController`を使用してプッシュセグエをプログラミングします。

```csharp
public partial class MainViewController : UIViewController
{
    UIViewController pinkViewController;

    public MainViewController (IntPtr handle) : base (handle)
    {
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        this.Initialize ();
    }

    public void Initialize()
    {
        //Instantiating View Controller with Storyboard ID 'PinkViewController'
        pinkViewController = Storyboard.InstantiateViewController ("PinkViewController") as PinkViewController;
    }

    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        //When we push the button, we will push the pinkViewController onto our current Navigation Stack
        PinkButton.TouchUpInside += (o, e) =&gt;
        {
            this.NavigationController.PushViewController (pinkViewController, true);
        };
    }
}
```

アプリケーションを実行すると、2画面アプリケーションが生成されます。

![](images/finishedstoryboard.png "Sample app run screens")

## <a name="conditional-segues"></a>条件付きセグエ

多くの場合、1つのビューコントローラーから次のビューコントローラーへの移動は、特定の条件に依存します。 たとえば、単純なログイン画面を作成した場合、ユーザー名とパスワードが確認されて*いれば*、次の画面に移動するだけです。

次の例では、上記のサンプルにパスワードフィールドを追加します。 ユーザーは、正しいパスワードを入力すると、 *Pinkviewcontroller*にしかアクセスできません。それ以外の場合は、エラーが表示されます。

開始する前に、上記の手順 1 ~ 8 を実行します。 この手順では、ストーリーボードを作成し、UI の作成を開始して、RootViewController として使用するビューコントローラーをアプリのデリゲートに指示します。

1. ここでは、UI を構築し、次のスクリーンショットに示すように、`MainViewController` に一覧表示されている追加のビューを追加します。

    - UITextField
        - 名前: PasswordTextField
        - プレースホルダー: ' シークレットパスワードを入力してください '
    - UILabel
        - テキスト: ' Error: パスワードが間違っています。 ! ' を渡すことはできません。
        - 色: 赤
        - 配置: 中央
        - 行: 2
        - ' Hidden ' チェックボックスがオンにされました    

    [![](images/passwordvc.png "Center Lines")](images/passwordvc.png#lightbox)

2. [ピンクに移動] ボタンと [ビューコントローラー] の間のセグエを作成するには、[ *Pinkbutton* ] から [ *Pinkviewcontroller*] に移動し、[マウスの**プッシュ**] を選択します。

3. セグエをクリックし、*識別子*`SegueToPink`を指定します。

    [![](images/namesegue.png "Click on the Segue and give it the Identifier SegueToPink")](images/namesegue.png#lightbox)  

4. 最後に、次の ShouldPerformSegue メソッドを `MainViewController` クラスに追加します。

    ```csharp
    public override bool ShouldPerformSegue (string segueIdentifier, NSObject sender)
    {

        if(segueIdentifier == "SegueToPink"){
            if (PasswordTextField.Text == "password") {
                PasswordTextField.ResignFirstResponder ();
                return true;
            }
            else{
                ErrorLabel.Hidden = false;
                return false;
            }
        }
        return base.ShouldPerformSegue (segueIdentifier, sender);
    }
    ```

このコードでは、segueIdentifier を `SegueToPink` セグエに一致させたので、次に条件をテストできます。この場合は、有効なパスワードです。 条件によって `true`が返された場合、セグエはを実行し、`PinkViewController`を提示します。 `false`した場合、新しいビューコントローラーは表示されません。

この方法は、segueIdentifier 引数を ShouldPerformSegue メソッドに対してチェックすることによって、このビューコントローラー上の任意のセグエに適用できます。 この例では、`SegueToPink`のセグエ identifier が1つだけあります。

実際の例については、「ストーリー[ボードの手動サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/manualstoryboard)」の「条件付きソリューション」を参照してください。

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>ストーリーボード参照の使用

ストーリーボード参照を使用すると、大規模で複雑なストーリーボード設計を作成し、元から参照される小さなストーリーボードに分割することができます。これにより、複雑さが解消され、結果として得られる個々のストーリーボードの設計と保守が容易になります。

さらに、ストーリーボード参照は、同じストーリーボード内の別のシーンまたは別のシーンの特定のシーンに_アンカー_を提供できます。

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>外部ストーリーボードの参照

外部ストーリーボードへの参照を追加するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、プロジェクト名を右クリックし、 **[追加]** [ > **新しいファイル**] [ > **iOS** > **ストーリーボード**] の順に選択します。 新しいストーリーボードの**名前**を入力し、 **[新規]** ボタンをクリックします。

    [![](images/ref01.png "The New File Dialog")](images/ref01.png#lightbox)

2. 通常どおりに新しいストーリーボードのシーンのレイアウトをデザインし、変更を保存します。

    [![](images/ref02.png "The layout of the new scene")](images/ref02.png#lightbox)

3. IOS Designer で、参照を追加するストーリーボードを開きます。

4. **ツールボックス**からデザインサーフェイスに**ストーリーボード参照**をドラッグします。

    [![](images/ref03.png "A Storyboard Reference")](images/ref03.png#lightbox)

5. **プロパティエクスプローラー**の **[ウィジェット]** タブで、上で作成した**ストーリーボード**の名前を選択します。

    [![](images/ref04.png "The Widget tab")](images/ref04.png#lightbox)

6. 既存のシーンの UI ウィジェット (ボタンなど) を制御し、先ほど作成した**ストーリーボード参照**に新しいセグエを作成します。

    [![](images/ref05.png "Creating a segue")](images/ref05.png#lightbox)

7. ポップアップメニューから **[表示]** を選択して、セグエを完了します。

    [![](images/ref06.png "Selecting Show to complete the Segue")](images/ref06.png#lightbox)

8. ストーリーボードへの変更を保存します。

アプリを実行し、ユーザーがセグエを作成した UI 要素をクリックすると、ストーリーボードの参照で指定された外部ストーリーボードの初期ビューコントローラーが表示されます。

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>外部ストーリーボードでの特定のシーンの参照

特定のシーンへの参照を (初期ビューコントローラーではなく) 外部ストーリーボードに追加するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、外部ストーリーボードをダブルクリックして編集用に開きます。

2. 次のように、新しいシーンを追加し、そのレイアウトをデザインします。

    [![](images/ref07.png "The new scene layout")](images/ref07.png#lightbox)

3. **プロパティエクスプローラー**の **[ウィジェット]** タブで、新しいシーンのビューコントローラーの**ストーリーボード ID**を入力します。

    [![](images/ref08.png "Enter a Storyboard ID for the new Scenes View Controller")](images/ref08.png#lightbox)

4. IOS Designer で、参照を追加するストーリーボードを開きます。

5. **ツールボックス**からデザインサーフェイスに**ストーリーボード参照**をドラッグします。

    [![](images/ref03.png "A Storyboard Reference")](images/ref03.png#lightbox)

6. **プロパティエクスプローラー**の **[ウィジェット]** タブで、前の手順で作成したシーンの**ストーリーボード**の名前と**参照 ID** (ストーリーボード id) を選択します。

    [![](images/ref09.png "The Widget tab ")](images/ref09.png#lightbox)

7. 既存のシーンの UI ウィジェット (ボタンなど) を制御し、先ほど作成した**ストーリーボード参照**に新しいセグエを作成します。

    [![](images/ref10.png "Creating a segue")](images/ref10.png#lightbox)

8. ポップアップメニューから **[表示]** を選択して、セグエを完了します。

    [![](images/ref06.png "Selecting Show to complete the Segue")](images/ref06.png#lightbox)

9. ストーリーボードへの変更を保存します。

アプリが実行され、ユーザーがセグエを作成した UI 要素をクリックすると、ストーリーボードの参照で指定された外部ストーリーボードから、指定された**ストーリーボード ID**を持つシーンが表示されます。

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>同じストーリーボード内の特定のシーンの参照

同じストーリーボードに特定のシーンへの参照を追加するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、ストーリーボードをダブルクリックして開き、編集します。

2. 次のように、新しいシーンを追加し、そのレイアウトをデザインします。

    [![](images/ref11.png "The new scene layout")](images/ref11.png#lightbox)

3. **プロパティエクスプローラー**の **[ウィジェット]** タブで、新しいシーンのビューコントローラーの**ストーリーボード ID**を入力します。

    [![](images/ref12.png "The Widget tab")](images/ref12.png#lightbox)

4. **ツールボックス**からデザインサーフェイスに**ストーリーボード参照**をドラッグします。

   [![](images/ref03.png "A Storyboard Reference")](images/ref03.png#lightbox)

5. **プロパティエクスプローラー**の **[ウィジェット]** タブで、前の手順で作成したシーンの **[参照 ID]** (ストーリーボード id) を選択します。

    [![](images/ref13.png "The Widget tab")](images/ref13.png#lightbox)

6. 既存のシーンの UI ウィジェット (ボタンなど) を制御し、先ほど作成した**ストーリーボード参照**に新しいセグエを作成します。

    [![](images/ref14.png "Creating a segue")](images/ref14.png#lightbox)

7. ポップアップメニューから **[表示]** を選択して、セグエを完了します。

    [![](images/ref06.png "Selecting Show to complete the Segue")](images/ref06.png#lightbox)

8. ストーリーボードへの変更を保存します。

アプリが実行され、ユーザーがセグエを作成した UI 要素をクリックすると、ストーリーボードの参照で指定された同じストーリーボードに、指定された**ストーリーボード ID**を持つシーンが表示されます。

## <a name="summary"></a>要約

この記事では、ストーリーボードの概念と、iOS アプリケーションの開発においてどのように役立つかについて説明します。 バックグラウンド、ビューコントローラー、ビュー、およびビュー階層について説明し、さまざまな種類のセグエと共にシーンをリンクする方法について説明します。  また、ストーリーボードからビューコントローラーを手動でインスタンス化し、条件付きセグエを作成する方法についても説明します。

## <a name="related-links"></a>関連リンク

- [手動ストーリーボード (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/manualstoryboard/)
- [IOS Designer の概要](~/ios/user-interface/designer/introduction.md)
- [変換 (ストーリーボードに)](https://developer.apple.com/library/ios/#releasenotes/Miscellaneous/RN-AdoptingStoryboards/)
- [UIStoryboard クラスのリファレンス](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIStoryboard_Class/Reference/Reference.html)
