---
title: Hello, iOS マルチスクリーン – クイック スタート
description: このドキュメントでは、Phoneword サンプル アプリケーションを拡張し、2 つ目の画面を追加する方法について説明しています。モデル ビュー コントローラー、iOS ナビゲーション、他の iOS 開発概念についても同時に取り上げています。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: d72e6230-c9ee-4bee-90ec-877d256821aa
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: 94bb87e10b8cab2a9a37c7701b59d621f6329b8d
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436182"
---
# <a name="hello-ios-multiscreen--quickstart"></a>Hello, iOS マルチスクリーン – クイック スタート

チュートリアルのこの部分では、アプリでかけた電話番号の履歴を表示する 2 つ目の画面を Phoneword アプリケーションに追加します。 最終的なアプリケーションには、以下のスクリーン ショットのように、通話履歴を表示する 2 つ目の画面が表示されます。

[![最終的なアプリケーションには、このスクリーンショットのように、通話履歴を表示する 2 つ目の画面が表示されます](hello-ios-multiscreen-quickstart-images/00.png)](hello-ios-multiscreen-quickstart-images/00.png#lightbox)

[後半の詳細のページ](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md)では、ビルドしたアプリケーションを確認し、アーキテクチャ、ナビゲーション、およびその過程で遭遇したその他の新しい iOS の概念について説明します。

## <a name="requirements"></a>必要条件

このガイドは、「Hello, iOS」ドキュメントを中断した箇所から再開するため、「[Hello, iOS クイック スタート](~/ios/get-started/hello-ios/index.md)」を完了する必要があります。 Phoneword アプリの完成版は、[Hello, iOS のサンプル](/samples/xamarin/ios-samples/hello-ios) ページからダウンロードしてください。

::: zone pivot="macos"

## <a name="walkthrough-on-macos"></a>MacOS でのチュートリアル

このチュートリアルでは、**Phoneword** アプリケーションに通話履歴画面を追加します。

1. Visual Studio for Mac で **Phoneword** アプリケーションを開きます。 必要に応じて、[Hello, iOS チュートリアル](~/ios/get-started/hello-ios/index.md) ガイドで完成した Phoneword アプリケーションを[ここ](/samples/xamarin/ios-samples/hello-ios)からダウンロードすることができます。

2. **Solution Pad** から **Main.storyboard** ファイルを開きます。

    ![iOS Designer の Main.storyboard](hello-ios-multiscreen-quickstart-images/02new.png)

3. **[ツールボックス]** からデザイン サーフェイスに**ナビゲーション コントローラー**をドラッグします (デザイン サーフェイスにこれらすべてが収まるように縮小する必要がある場合があります)。

    ![[ツールボックス] からデザイン サーフェイスにナビゲーション コントローラーをドラッグする](hello-ios-multiscreen-quickstart-images/03new.png)

4. **ソースレス セグエ** (1 つのビュー コントローラーの左側にある灰色の矢印) を**ナビゲーション コントローラー**にドラッグし、アプリケーションの開始点を変更します。

    ![ナビゲーション コントローラーにソースレス セグエをドラッグして、アプリケーションの開始点を変更する](hello-ios-multiscreen-quickstart-images/04new.png)

5. 下部にあるバーをクリックして既存の **[ルート ビュー コントローラー]** を選択し、**Delete** キーを押してデザイン サーフェイスから削除します。
次に、**ナビゲーション コントローラー**の横に **Phoneword** シーンを移動します。

    ![ナビゲーション コントローラーの横に Phoneword シーンを移動する](hello-ios-multiscreen-quickstart-images/05new.png)

6. ナビゲーション コントローラーの**ルート ビュー コントローラー**として **ViewController** を設定します。 **Ctrl** キーを押したまま、**ナビゲーション コントローラー**内をクリックします。 青い線が表示されます。 次に、**Ctrl** キーを押したままの状態で、**ナビゲーション コントローラー**から **Phoneword** シーンまでドラッグして離します。 これを _Ctrl-ドラッグ_といいます。

    ![ナビゲーション コントローラーから Phoneword シーンまでドラッグして離す](hello-ios-multiscreen-quickstart-images/06.png)

7. ポップオーバーから、関係を **[ルート]** に設定します。

    ![関係を [ルート] に設定](hello-ios-multiscreen-quickstart-images/07new.png)

    これで、**ViewController** が**ナビゲーション コントローラーのルート ビュー コントローラーになりました。**

    ![これで、ViewController がナビゲーション コントローラーのルート ビュー コントローラーになりました](hello-ios-multiscreen-quickstart-images/08.png)

8. **Phoneword** 画面の**タイトル** バーをダブルクリックして、**タイトル**を **Phoneword** に変更します。

    ![タイトルを Phoneword に変更する](hello-ios-multiscreen-quickstart-images/09.png)

9. **[ツールボックス]** から**ボタン**をドラッグして、**通話ボタン**の下に配置します。 ハンドルをドラッグして、新しい**ボタン**を**通話ボタン**と同じ幅にします。

    ![新しいボタンを通話ボタンと同じ幅にする](hello-ios-multiscreen-quickstart-images/10new.png)

10. **Properties Pad** で、ボタンの **[名前]** を **CallHistoryButton** に変更し、 **[タイトル]** を**通話履歴**に変更します。

    ![ボタンの [名前] を CallHistoryButton に変更し、[タイトル] を通話履歴に変更する](hello-ios-multiscreen-quickstart-images/11new.png)

11. **通話履歴**画面を作成します。 **[ツールボックス]** から、**テーブル ビュー コントローラー**をデザイン サーフェイスにドラッグします。

    ![テーブル ビュー コントローラーをデザイン サーフェイスにドラッグする](hello-ios-multiscreen-quickstart-images/12new.png)

12. 次に、シーンの下部にある黒いバーをクリックして、**テーブル ビュー コントローラー**を選択します。 **Properties Pad** で、**テーブル ビュー コントローラー**のクラスを `CallHistoryController` に変更し、**Enter** キーを押します。

    ![テーブル ビュー コントローラーのクラスを CallHistoryController に変更する](hello-ios-multiscreen-quickstart-images/13new.png)

    iOS Designer は、`CallHistoryController` というカスタム バッキング クラスを生成して、この画面のコンテンツ ビュー階層を管理します。 **CallHistoryController.cs** ファイルが **Solution Pad** に表示されます。

    ![Solution Pad の CallHistoryController.cs ファイル](hello-ios-multiscreen-quickstart-images/14new.png)

13. **CallHistoryController.cs** ファイルをダブルクリックして開き、その内容を以下のコードに置き換えます。

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.Collections.Generic;

    namespace Phoneword_iOS
    {
        public partial class CallHistoryController : UITableViewController
        {
            public List<string> PhoneNumbers { get; set; }

            static NSString callHistoryCellId = new NSString ("CallHistoryCell");

            public CallHistoryController (IntPtr handle) : base (handle)
            {
                TableView.RegisterClassForCellReuse (typeof(UITableViewCell), callHistoryCellId);
                TableView.Source = new CallHistoryDataSource (this);
                PhoneNumbers = new List<string> ();
            }

            class CallHistoryDataSource : UITableViewSource
            {
                CallHistoryController controller;

                public CallHistoryDataSource (CallHistoryController controller)
                {
                    this.controller = controller;
                }

                public override nint RowsInSection (UITableView tableView, nint section)
                {
                    return controller.PhoneNumbers.Count;
                }

                public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
                {
                    var cell = tableView.DequeueReusableCell (CallHistoryController.callHistoryCellId);

                    int row = indexPath.Row;
                    cell.TextLabel.Text = controller.PhoneNumbers [row];
                    return cell;
                }
            }
        }
    }
    ```

    アプリケーションを保存し (**⌘ + s** キーを押し)、ビルドして (**⌘ + b** キーを押して) エラーがないことを確認します。

14. **Phoneword** シーンと**通話履歴**シーンの間に_セグエ_ (切り替え) を作成します。
  **Phoneword シーン**で、**通話履歴ボタン**を選択し、**ボタン**から**通話履歴**シーンに Ctrl キーを押しながらドラッグします。

    ![ボタンから通話履歴シーンに Ctrl キーを押しながらドラッグする](hello-ios-multiscreen-quickstart-images/15.png)

    **[Action Segue]\(アクション セグエ\)** ポップオーバーから、 **[表示]** を選択します

    iOS Designer は次の 2 つのシーン間にセグエを追加します。

    ![2 つのシーン間のセグエ](hello-ios-multiscreen-quickstart-images/17new.png)

15. シーンの下部にある黒いバーを選択し、**Properties Pad** で **ビュー コントローラーの [タイトル]** を**通話履歴**に変更して、**テーブル ビュー コントローラー**に**タイトル**を追加します。

    ![Properties Pad でビュー コントローラーのタイトルを通話履歴に変更する](hello-ios-multiscreen-quickstart-images/18new.png)

16. アプリケーションが実行されている場合は、**通話履歴ボタン**で**通話履歴**画面が開きますが、電話番号を追跡して表示するコードがないため、テーブル ビューは空になります。

    このアプリでは、電話番号は文字列のリストとして格納されます。

    **ViewController** の上部で `System.Collections.Generic` の `using` ディレクティブを追加します。

    ```csharp
    using System.Collections.Generic;
    ```

17. `ViewController` クラスを次のコードで変更します。

    ```csharp
    using System;
    using System.Collections.Generic;
    using Foundation;
    using UIKit;

    namespace Phoneword_iOS
    {
      public partial class ViewController : UIViewController
      {
        string translatedNumber = "";

        public List<string> PhoneNumbers { get; set; }

        protected ViewController(IntPtr handle) : base(handle)
        {
          //initialize list of phone numbers called for Call History screen
          PhoneNumbers = new List<string>();
        }

        public override void ViewDidLoad()
        {
          base.ViewDidLoad();
          // Perform any additional setup after loading the view, typically from a nib.

          TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
            // Convert the phone number with text to a number
            // using PhoneTranslator.cs
            translatedNumber = PhoneTranslator.ToNumber(
              PhoneNumberText.Text);

            // Dismiss the keyboard if text field was tapped
            PhoneNumberText.ResignFirstResponder();

            if (translatedNumber == "")
            {
              CallButton.SetTitle("Call ", UIControlState.Normal);
              CallButton.Enabled = false;
            }
            else
            {
              CallButton.SetTitle("Call " + translatedNumber,
                UIControlState.Normal);
              CallButton.Enabled = true;
            }
          };

          CallButton.TouchUpInside += (object sender, EventArgs e) => {

            //Store the phone number that we're dialing in PhoneNumbers
            PhoneNumbers.Add(translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl("tel:" + translatedNumber);

            // otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl(url))
            {
              var alert = UIAlertController.Create("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
              alert.AddAction(UIAlertAction.Create("Ok", UIAlertActionStyle.Default, null));
              PresentViewController(alert, true, null);
            }
          };
        }

        public override void PrepareForSegue(UIStoryboardSegue segue, NSObject sender)
        {
          base.PrepareForSegue(segue, sender);

          // set the view controller that’s powering the screen we’re
          // transitioning to

          var callHistoryController = segue.DestinationViewController as CallHistoryController;

          //set the table view controller’s list of phone numbers to the
          // list of dialed phone numbers

          if (callHistoryController != null)
          {
            callHistoryController.PhoneNumbers = PhoneNumbers;
          }
        }
      }
    }
    ```

    ここではいくつかの処理が行われます。

    - 変数 `translatedNumber` が `ViewDidLoad` メソッドから_クラス レベルの変数_に移動されています。
    - `PhoneNumbers.Add(translatedNumber)` を呼び出して電話番号リストにダイヤル番号を追加するように **CallButton** コードが変更されています。
    - `PrepareForSegue` メソッドが追加されています。

    アプリケーションを保存してビルドし、エラーがないことを確認します。

18. **[開始]** ボタンを押して、**iOS シミュレーター**内でアプリケーションを起動します。

    ![[開始] ボタンを押して、iOS シミュレーター内でアプリケーションを起動する](hello-ios-multiscreen-quickstart-images/19.png)

おつかれさまでした。これで最初のマルチスクリーン Xamarin.iOS アプリケーションが完成しました。

::: zone-end
::: zone pivot="windows"

## <a name="walkthrough-on-windows"></a>Windows でのチュートリアル

このチュートリアルでは、**Phoneword** アプリケーションに通話履歴画面を追加します。

1. Visual Studio で **Phoneword** アプリケーションを開きます。 必要に応じて、[Hello, iOS チュートリアル](~/ios/get-started/hello-ios/index.md) ガイドから[完成した Phoneword アプリケーション](/samples/xamarin/ios-samples/hello-ios)をダウンロードします。 iOS Designer、および iOS シミュレーターを使用するには [Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) に接続する必要があることを思い出してください。

2. ユーザー インターフェイスの編集を開始します。 **[表示方法]** が _iPhone 6_ に設定されていることを確認して、**ソリューション エクスプローラー**から **Main.storyboard** ファイルを開きます。

    ![iOS Designer の Main.storyboard](hello-ios-multiscreen-quickstart-images/image1.png)

3. **[ツールボックス]** からデザイン サーフェイスに**ナビゲーション コントローラー**をドラッグします。

    ![[ツールボックス] からデザイン サーフェイスにナビゲーション コントローラーをドラッグする](hello-ios-multiscreen-quickstart-images/image2.png)

4. **Phoneword** シーンから**ナビゲーション コントローラー**に**ソースレス セグエ** (**Phoneword** シーンの左側にある灰色の矢印) をドラッグし、アプリケーションの開始点を変更します。

    ![ナビゲーション コントローラーにソースレス セグエをドラッグして、アプリケーションの開始点を変更する](hello-ios-multiscreen-quickstart-images/image3.png)

5. 黒いバーをクリックして **[ルート ビュー コントローラー]** を選択し、**Delete** キーを押してデザイン サーフェイスから削除します。
  次に、**ナビゲーション コントローラー**の横に **Phoneword** シーンを移動します。

    ![ナビゲーション コントローラーの横に Phoneword シーンを移動する](hello-ios-multiscreen-quickstart-images/image4.png)

6. ナビゲーション コントローラーのルート ビュー コントローラーとして **ViewController** を設定します。 **Ctrl** キーを押し、**ナビゲーション コントローラー**内をクリックします。 青い線が表示されます。 次に、**Ctrl** キーを押したままの状態で、**ナビゲーション コントローラー**から **Phoneword** シーンまでドラッグして離します。 これを _Ctrl-ドラッグ_といいます。

    ![ナビゲーション コントローラーから Phoneword シーンまでドラッグして離す](hello-ios-multiscreen-quickstart-images/image5.png)

7. ポップオーバーから、関係を **[ルート]** に設定します。

    ![関係を [ルート] に設定](hello-ios-multiscreen-quickstart-images/image6.png)

    これで、**ViewController** が**ナビゲーション コントローラーのルート ビュー コントローラー**になりました。

8. **Phoneword** 画面の**タイトル** バーをダブルクリックして、**タイトル**を **Phoneword** に変更します。

    ![タイトルを Phoneword に変更する](hello-ios-multiscreen-quickstart-images/image7.png)

9. **[ツールボックス]** から**ボタン**をドラッグして、**通話ボタン**の下に配置します。 ハンドルをドラッグして、新しい**ボタン**を**通話ボタン**と同じ幅にします。

    ![新しいボタンを通話ボタンと同じ幅にする](hello-ios-multiscreen-quickstart-images/image8.png)

10. **プロパティ エクスプローラー**で、**ボタン**の**名前**を `CallHistoryButton` に変更し、**タイトル**を**通話履歴**に変更します。

    ![ボタンの [名前] を CallHistoryButton に変更し、[タイトル] を通話履歴に変更する](hello-ios-multiscreen-quickstart-images/image9.png)

11. **通話履歴**画面を作成します。 **[ツールボックス]** から、**テーブル ビュー コントローラー**をデザイン サーフェイスにドラッグします。

    ![テーブル ビュー コントローラーをデザイン サーフェイスにドラッグする](hello-ios-multiscreen-quickstart-images/image10.png)

12. シーンの下部にある黒いバーをクリックして、**テーブル ビュー コントローラー**を選択します。 **プロパティ エクスプローラー**で、**テーブル ビュー コントローラー**のクラスを `CallHistoryController` に変更して **Enter** キーを押します。

    ![テーブル ビュー コントローラーのクラスを CallHistoryController に変更する](hello-ios-multiscreen-quickstart-images/image11.png)

    iOS Designer は、`CallHistoryController` というカスタム バッキング クラスを生成して、この画面のコンテンツ ビュー階層を管理します。 **CallHistoryController.cs** ファイルが**ソリューション エクスプローラー**に表示されます。

    ![ソリューション エクスプローラーの CallHistoryController.cs ファイル](hello-ios-multiscreen-quickstart-images/image12.png)

13. **CallHistoryController.cs** ファイルをダブルクリックして開き、その内容を以下のコードに置き換えます。

    ```csharp
    using System;
    using Foundation;
    using UIKit;
    using System.Collections.Generic;

    namespace Phoneword
    {
        public partial class CallHistoryController : UITableViewController
        {
            public List<String> PhoneNumbers { get; set; }

            static NSString callHistoryCellId = new NSString ("CallHistoryCell");

            public CallHistoryController (IntPtr handle) : base (handle)
            {
                TableView.RegisterClassForCellReuse (typeof(UITableViewCell), callHistoryCellId);
                TableView.Source = new CallHistoryDataSource (this);
                PhoneNumbers = new List<string> ();
            }

            class CallHistoryDataSource : UITableViewSource
            {
                CallHistoryController controller;

                public CallHistoryDataSource (CallHistoryController controller)
                {
                    this.controller = controller;
                }

                // Returns the number of rows in each section of the table
                public override nint RowsInSection (UITableView tableView, nint section)
                {
                    return controller.PhoneNumbers.Count;
                }

                public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
                {
                    var cell = tableView.DequeueReusableCell (CallHistoryController.callHistoryCellId);

                    int row = indexPath.Row;
                    cell.TextLabel.Text = controller.PhoneNumbers [row];
                    return cell;
                }
            }
        }
    }
    ```

    アプリケーションを保存し、ビルドしてエラーがないことを確認します。 ここでは、ビルド警告を無視してもかまいません。

14. **Phoneword** シーンと**通話履歴**シーンの間に_セグエ_ (切り替え) を作成します。
  **Phoneword シーン**で、**通話履歴ボタン**を選択し、**ボタン**から**通話履歴**シーンに **Ctrl キーを押しながらドラッグ**します。

    ![ボタンから通話履歴シーンに Ctrl キーを押しながらドラッグする](hello-ios-multiscreen-quickstart-images/image13.png)

    **[Action Segue]\(アクション セグエ\)** ポップオーバーから、 **[表示]** を選択します。

    ![セグエの種類として [表示] を選択する](hello-ios-multiscreen-quickstart-images/image14.png)

    iOS Designer は次の 2 つのシーン間にセグエを追加します。

    ![2 つのシーン間のセグエ](hello-ios-multiscreen-quickstart-images/image15.png)

15. シーンの下部にある黒いバーを選択し、**プロパティ エクスプローラー**で **[ビュー コントローラー] の [タイトル]** を**通話履歴**に変更して、**テーブル ビュー コントローラー**に**タイトル**を追加します。

    ![ビュー コントローラーの [タイトル] を通話履歴に変更する](hello-ios-multiscreen-quickstart-images/image16.png)

16. アプリケーションが実行されている場合は、**通話履歴ボタン**で**通話履歴**画面が開きますが、電話番号を追跡して表示するコードがないため、テーブル ビューは空になります。

    このアプリでは、電話番号は文字列のリストとして格納されます。

    **ViewController** の上部で `System.Collections.Generic` の `using` ディレクティブを追加します。

    ```csharp
    using System.Collections.Generic;
    ```

17. `ViewController` クラスを次のコードで変更します。

    ```csharp
    using System;
    using System.Collections.Generic;
    using Foundation;
    using UIKit;

    namespace Phoneword_iOS
    {
      public partial class ViewController : UIViewController
      {
        string translatedNumber = "";

        public List<string> PhoneNumbers { get; set; }

        protected ViewController(IntPtr handle) : base(handle)
        {
          //initialize list of phone numbers called for Call History screen
          PhoneNumbers = new List<string>();
        }

        public override void ViewDidLoad()
        {
          base.ViewDidLoad();
          // Perform any additional setup after loading the view, typically from a nib.

          TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
            // Convert the phone number with text to a number
            // using PhoneTranslator.cs
            translatedNumber = PhoneTranslator.ToNumber(
              PhoneNumberText.Text);

            // Dismiss the keyboard if text field was tapped
            PhoneNumberText.ResignFirstResponder();

            if (translatedNumber == "")
            {
              CallButton.SetTitle("Call ", UIControlState.Normal);
              CallButton.Enabled = false;
            }
            else
            {
              CallButton.SetTitle("Call " + translatedNumber,
                UIControlState.Normal);
              CallButton.Enabled = true;
            }
          };

          CallButton.TouchUpInside += (object sender, EventArgs e) => {

            //Store the phone number that we're dialing in PhoneNumbers
            PhoneNumbers.Add(translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl("tel:" + translatedNumber);

            // otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl(url))
            {
              var alert = UIAlertController.Create("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
              alert.AddAction(UIAlertAction.Create("Ok", UIAlertActionStyle.Default, null));
              PresentViewController(alert, true, null);
            }
          };
        }

        public override void PrepareForSegue(UIStoryboardSegue segue, NSObject sender)
        {
          base.PrepareForSegue(segue, sender);

          // set the view controller that’s powering the screen we’re
          // transitioning to

          var callHistoryController = segue.DestinationViewController as CallHistoryController;

          //set the table view controller’s list of phone numbers to the
          // list of dialed phone numbers

          if (callHistoryController != null)
          {
            callHistoryController.PhoneNumbers = PhoneNumbers;
          }
        }
      }
    }
    ```

    ここではいくつかの処理が行われます
    - 変数 `translatedNumber` が `ViewDidLoad` メソッドから "_クラス レベルの変数_" に移動されています。
    - `PhoneNumbers.Add(translatedNumber)` を呼び出して電話番号リストにダイヤル番号を追加するように **CallButton** コードが変更されています
    - `PrepareForSegue` メソッドが追加されています

    アプリケーションを保存してビルドし、エラーがないことを確認します。

    アプリケーションを保存してビルドし、エラーがないことを確認します。

18. **[開始]** ボタンを押して、**iOS シミュレーター**内でアプリケーションを起動します。

    ![サンプル アプリの最初の画面](hello-ios-multiscreen-quickstart-images/19.png)

おつかれさまでした。これで最初のマルチスクリーン Xamarin.iOS アプリケーションが完成しました。

::: zone-end

これで、アプリでストーリーボード セグエとコードの両方を使用して、ナビゲーションを処理できるようになりました。 次は、「[Hello, iOS マルチスクリーンの詳細](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md)」で学習したツールとスキルを詳しく分析します。

## <a name="related-links"></a>関連リンク

- [Hello, iOS (サンプル)](/samples/xamarin/ios-samples/hello-ios)
- [iOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS プロビジョニング ポータル](https://developer.apple.com/ios/manage/overview/index.action)