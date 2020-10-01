---
title: Hello, iOS – クイックスタート
description: このチュートリアルは、"Hello, iOS" という名前の単純な Xamarin.iOS アプリケーションをビルドする方法について段階的に説明しています。 その過程で、Xamarin.iOS アプリケーションをビルドするために理解する必要がある基本的なツールや概念を紹介しています。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: D3868F3A-4EED-BDDF-45AA-665102C39634
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: bde05cfeecaeba389224bbbdb7ae01fcb7bcb56c
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91434115"
---
# <a name="hello-ios--quickstart"></a>Hello, iOS – クイックスタート

このガイドでは、ユーザーが入力した英字の電話番号を数値の電話番号に変換してから、その番号に電話をかけるアプリケーションの作成方法を説明します。 最終的にアプリケーションは次のようになります。

 [![Hello.iOS クイック スタート アプリ](hello-ios-quickstart-images/image1.png)](hello-ios-quickstart-images/image1.png#lightbox)

## <a name="requirements"></a>必要条件

Xamarin を使用した iOS 開発の要件:

- macOS High Sierra (10.13) 以降を実行している Mac。
- [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12) からインストールした最新版の Xcode と iOS SDK。

::: zone pivot="macos"

Xamarin.iOS は次のセットアップで機能します。

- 上記の仕様を満たす最新版の Visual Studio for Mac。

[Xamarin.iOS Mac インストール ガイド](~/ios/get-started/installation/mac.md)で段階的インストール手順をご確認いただけます

::: zone-end
::: zone pivot="windows"

Xamarin.iOS は次のセットアップで機能します。

- 最新版の Visual Studio 2019 または Visual Studio 2017 Community、Professional、または Enterprise。Windows 10。上記の仕様を満たす Mac ビルド ホストとペア。

[Xamarin.iOS Windows インストール ガイド](~/ios/get-started/installation/windows/index.md)でステップ バイ ステップのインストール手順をご確認いただけます。

::: zone-end

始める前に、[Xamarin アプリ アイコン セット](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true)をダウンロードしてください。

::: zone pivot="macos"

## <a name="visual-studio-for-mac-walkthrough"></a>Visual Studio for Mac のチュートリアル

このチュートリアルでは、英数字の電話番号を数字の電話番号に変換する Phoneword という名称のアプリケーションを作成する方法を説明します。

1. **Applications** フォルダーまたは**スポットライト**から Visual Studio for Mac を起動します。

    ![起動画面](hello-ios-quickstart-images/image2new.png)

    起動画面で、 **[新しいプロジェクト...]** をクリックして、新しい Xamarin.iOS ソリューションを作成します。

    ![iOS ソリューション](hello-ios-quickstart-images/image3new.png)

2. **[新しいソリューション]** ダイアログで **[iOS]、[アプリ]、[単一ビュー アプリケーション]** テンプレートの順に選択し、C# が選択されていることを確認します。 **[次へ]** をクリックします。

    ![単一ビュー アプリケーションの選択](hello-ios-quickstart-images/image4new.png)

3. アプリを構成します。 **[名前]** に `Phoneword_iOS` を指定し、他のすべてを既定値のままにします。 **[次へ]** をクリックします。

    ![アプリ名の入力](hello-ios-quickstart-images/image5new.png)

4. プロジェクトとソリューションの名前はそのままにします。 ここでプロジェクトの場所を選択するか、既定のままにします。

    ![プロジェクトの場所の選択](hello-ios-quickstart-images/image6new.png)

5. **[作成]** をクリックして**ソリューション**を作成します。

6. **Solution Pad** で **Main.storyboard** ファイルをダブルクリックして開きます。 これで目視で確認しながら UI を作成できます。

    ![iOS Designer](hello-ios-quickstart-images/image7new.png)

    "_サイズ クラス_" は既定で有効になっています。 詳細については、[Unified Storyboards](~/ios/user-interface/storyboards/unified-storyboards.md) ガイドをご覧ください。

7. **ツールボックス パッド**の検索バーに「label」と入力し、デザイン サーフェイス (中央の領域) に**ラベル**をドラッグします。

    ![デザイン サーフェイス (中央の領域) にラベルをドラッグする](hello-ios-quickstart-images/image8new.png)

    > [!NOTE]
    > **[表示] > [パッド]** の順に選択すれば、**プロパティ** や**ツールボックス**をいつでも表示できます。

8. *ドラッグ コントロール*のハンドル (コントロールを囲む円) をつかみ、ラベルを大きくします。

    ![ラベルの幅を広げる](hello-ios-quickstart-images/image9.png)

9. デザイン サーフェイスで**ラベル**が選択されている状態で、 **[プロパティ]** パッドを利用し、 **[ラベル]** の **[テキスト]** プロパティを "Enter a Phoneword:" に変更します。

    ![ラベルに「Enter a Phoneword」を設定する](hello-ios-quickstart-images/image10.png)

10. ツールボックス内で “テキスト フィールド” を検索し、 **[ツールボックス]** の **[テキスト フィールド]** をデザイン サーフェイスにドラッグし、 **[ラベル]** の下に置きます。 **テキスト フィールド**の幅が**ラベル**の幅と同じになるまで幅を調整します。

    ![テキスト フィールドをラベルと同じ幅にする](hello-ios-quickstart-images/image12new.png)

11. デザイン サーフェイスで**テキスト フィールド**が選択されている状態で、**Properties Pad** の ID セクションの **[テキスト フィールド]** の **[名前]** プロパティを `PhoneNumberText` に変更し、 **[テキスト]** プロパティを "1-855-XAMARIN" に変更します。

    ![[タイトル] プロパティを 1-855-XAMARIN に変更する](hello-ios-quickstart-images/image13new.png)

12. **[ボタン]** を **[ツールボックス]** からデザイン サーフェスにドラッグし、 **[テキスト フィールド]** の下に配置します。 **ボタン**の幅が**テキスト フィールド**や**ラベル**の幅と同じになるように幅を調整します。

    ![ボタンの幅がテキスト フィールドやラベルの幅と同じになるように調整する](hello-ios-quickstart-images/image14new.png)

13. デザイン サーフェイスで**ボタン**が選択されている状態で、**Properties Pad** の **[ID]** セクションの **[名前]** プロパティを `TranslateButton` に変更します。 **[タイトル]** プロパティを "Translate" に変更します。

    ![[タイトル] プロパティを Translate に変更する](hello-ios-quickstart-images/image15new.png)

14. 上記の 2 つの手順を繰り返し、 **[ボタン]** を **[ツールボックス]** からデザイン サーフェスにドラッグし、最初の **[ボタン]** の下に配置します。 **ボタン**の幅が最初の**ボタン**の幅と同じになるように幅を調整します。

    ![ボタンの幅が最初のボタンの幅と同じになるように調整する](hello-ios-quickstart-images/image16new.png)

15. デザイン サーフェイスで 2 つ目の**ボタン**が選択されている状態で、 **[プロパティ]** パッドの **[ID]** セクションの **[名前]** プロパティを `CallButton` に変更します。 **[タイトル]** プロパティを "Call" に変更します。

    ![[タイトル] プロパティを Call に変更する](hello-ios-quickstart-images/image17new.png)

    **[ファイル]、[保存]** の順に選択するか、**⌘ + s** を押し、変更内容を保存します。

16. 電話番号を英数字から数字に変換するには、いくつかのロジックをアプリに追加する必要があります。 **Solution Pad** の **Phoneword_iOS** プロジェクトを右クリックし、 **[追加]、[新しいファイル...]** の順に選択するか、**⌘ + n** を押し、新しいファイルをプロジェクトに追加します。

    ![新しいファイルをプロジェクトに追加する](hello-ios-quickstart-images/image18.png)

17. **[新しいファイル]** ダイアログで **[全般]、[空のクラス]** の順に選択し、新しいファイルに「`PhoneTranslator`」という名前を付けます。

    ![[空のクラス] を選択し、新しいファイル PhoneTranslator という名前を付ける](hello-ios-quickstart-images/image19.png)

18. これで、新しい空の C# クラスが作成されます。 テンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword_iOS
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

    **PhoneTranslator.cs** ファイルを保存して閉じます。

19. ユーザー インターフェイスを接続するコードを追加します。 **Solution Pad** で **ViewController.cs** をダブルクリックして開きます。

    ![ユーザー インターフェイスを接続するコードを追加する](hello-ios-quickstart-images/image20new.png)

20. 最初に `TranslateButton` を接続します。 **ViewController** クラスで、`ViewDidLoad` メソッドを見つけ、`base.ViewDidLoad()` 呼び出しの下に次のコードを追加します。

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(
            PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call ", UIControlState.Normal);
            CallButton.Enabled = false;
        } else {
            CallButton.SetTitle ("Call " + translatedNumber,
                UIControlState.Normal);
            CallButton.Enabled = true;
        }
    };
    ```

    ファイルの名前空間が異なる場合、`using Phoneword_iOS;` を追加します。

21. 2 つ目のボタンである `CallButton` をユーザーが押したときに応答するためのコードを追加します。 `TranslateButton` のコードの下に次のコードを配置し、ファイルの一番上に `using Foundation;` を追加します。

    ```csharp
        CallButton.TouchUpInside += (object sender, EventArgs e) => {
            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl ("tel:" + translatedNumber);

            // ...otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl (url)) {
                var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                PresentViewController (alert, true, null);
            }
        };
    ```

22. 変更内容を保存したら、 **[ビルド]、[すべてビルド]** の順に選択するか、**⌘ + B** を押し、アプリケーションをビルドします。アプリケーションがコンパイルされると、IDE の一番上に成功メッセージが表示されます。

    ![IDE の一番上に成功メッセージが表示される](hello-ios-quickstart-images/image21.png)

    エラーがある場合は、アプリケーションが正常にビルドされるまで、前の手順を実行し、誤りを修正します。

23. 最後に **iOS シミュレーター**でアプリケーションをテストします。 IDE の左上で、最初のドロップダウンから **[デバッグ]** を選択し、2 つ目のドロップダウンから **[iPhone XR iOS 12.0]** (またはその他の使用可能なシミュレーター) を選択し、 **[開始]** (再生ボタンに似た三角形のボタン) を押します。

    ![シミュレーターを選択して [開始] を押す](hello-ios-quickstart-images/image27.png)

    > [!NOTE]
    > 現在のところ、Apple の要件に起因し、場合によっては、デバイスまたはシミュレーターのコードをビルドするため、開発証明書または "*署名 ID*" を用意する必要があります。 [デバイス プロビジョニング ガイド](~/ios/get-started/installation/device-provisioning/manual-provisioning.md)の手順に従ってこれを設定します。

24. これにより、iOS シミュレーター内でアプリケーションが起動します。

    ![iOS シミュレーター内で実行されているアプリケーション](hello-ios-quickstart-images/image28.png)

    iOS シミュレーターでは通話に対応していません。代わりに、電話をかけようとすると警告ダイアログが表示されます。

    ![電話をかけようとしたときの警告ダイアログ](hello-ios-quickstart-images/image29.png)

::: zone-end
::: zone pivot="windows"

## <a name="visual-studio-walkthrough"></a>Visual Studio チュートリアル

このチュートリアルでは、英数字の電話番号を数字の電話番号に変換する Phoneword という名称のアプリケーションを作成する方法を説明します。

> [!NOTE]
> このチュートリアルでは、Windows 10 仮想マシンの Visual Studio Enterprise 2017 を使用します。 上記の要件を満たす限り、実際のセットアップはこれと異なってもかまいません。一部のスクリーンショットが実際とは異なる場合があります。

> [!NOTE]
> このチュートリアルを進める前に、Visual Studio から Mac に接続しておく必要があります。 iOS デザイナーとアプリケーションをビルドし、起動するために Xamarin.iOS は Apple のツールを利用するためです。 「[Mac とペアリング](~/ios/get-started/installation/windows/connecting-to-mac/index.md)」ガイドの手順に従い、設定を始めます。

1. **[スタート]** メニューから Visual Studio を起動します。

    ![スタート画面](hello-ios-quickstart-images/image001-.png)

    **[ファイル] > [新規] > [プロジェクト...] > [Visual C#] > [iPhone と iPad] > [iOS アプリ (Xamarin)]** の順に選択し、新しい Xamarin.iOS ソリューションを作成します。

    ![iOS アプリ (Xamarin) プロジェクト タイプの選択](hello-ios-quickstart-images/image002.w157.png "iOS アプリ (Xamarin) プロジェクト タイプの選択")

    次に表示されたダイアログで **[単一ビュー アプリ]** テンプレートを選択し、 **[OK]** をクリックしてプロジェクトを作成します。

    ![単一ビュー プロジェクト テンプレートの選択](hello-ios-quickstart-images/image002-2.w157.png "単一ビュー プロジェクト テンプレートの選択")

1. ツール バーの [Xamarin Mac エージェント] アイコンが緑であることを確認します。

    ![ツール バーの [Xamarin Mac エージェント] アイコンが緑であることを確認します](hello-ios-quickstart-images/vs-image4.png)

    緑でない場合は、Mac ビルド ホストへの接続がないことを示しているため、[構成ガイド](~/ios/get-started/installation/windows/connecting-to-mac/index.md)の手順に従って接続します。

1. **ソリューション エクスプローラー** で iOS デザイナーの **Main.storyboard** ファイルをダブルクリックして開きます。

    ![iOS Designer](hello-ios-quickstart-images/vs-image7.png)

1. **[ツールボックス]** タブの検索バーに「label」と入力し、デザイン サーフェイス (中央の領域) に**ラベル**をドラッグします。

    ![デザイン サーフェイス (中央の領域) にラベルをドラッグする](hello-ios-quickstart-images/vs-image8.png)

1. 次に、*ドラッグ コントロール*のハンドルをつかみ、ラベルを大きくします。

    ![ラベルの幅を広げる](hello-ios-quickstart-images/vs-image9.png)

1. デザイン サーフェイスで**ラベル**が選択されている状態で、 **[プロパティ]** ウィンドウを利用し、 **[ラベル]** の **[テキスト]** プロパティを "Enter a Phoneword:" に変更します。

    ![ラベルの [テキスト] プロパティに「Enter a Phoneword」を設定する](hello-ios-quickstart-images/vs-image10.png)

    > [!NOTE]
    > **[表示]** メニューに移動すれば、 **[プロパティ]** や **[ツールボックス]** をいつでも表示できます。

1. ツールボックス内で “テキスト フィールド” を検索し、 **[ツールボックス]** の **[テキスト フィールド]** をデザイン サーフェイスにドラッグし、 **[ラベル]** の下に置きます。 **テキスト フィールド**の幅が**ラベル**の幅と同じになるまで幅を調整します。

    ![テキスト フィールドの幅がラベルの幅と同じになるまで調整する](hello-ios-quickstart-images/vs-image12.png)

1. デザイン サーフェイスで**テキスト フィールド**が選択されている状態で、 **[プロパティ]** パッドの ID セクションの **[テキスト フィールド]** の **[名前]** プロパティを `PhoneNumberText` に変更し、 **[テキスト]** プロパティを "1-855-XAMARIN" に変更します。

    ![[テキスト] プロパティを 1-855-XAMARIN に変更する](hello-ios-quickstart-images/vs-image13.png)

1. **[ボタン]** を **[ツールボックス]** からデザイン サーフェスにドラッグし、 **[テキスト フィールド]** の下に配置します。 **ボタン**の幅が**テキスト フィールド**や**ラベル**の幅と同じになるように幅を調整します。

    ![ボタンの幅がテキスト フィールドやラベルの幅と同じになるように調整する](hello-ios-quickstart-images/vs-image14.png)

1. デザイン サーフェイスで**ボタン**が選択されている状態で、 **[プロパティ]** パッドの **[ID]** セクションの **[名前]** プロパティを `TranslateButton` に変更します。 **[タイトル]** プロパティを "Translate" に変更します。

    ![[タイトル] プロパティを Translate に変更する](hello-ios-quickstart-images/vs-image15.png)

1. 前の 2 つの手順を繰り返し、 **[ボタン]** を **[ツールボックス]** からデザイン サーフェスにドラッグし、最初の **[ボタン]** の下に配置します。 **ボタン**の幅が最初の**ボタン**の幅と同じになるように幅を調整します。

    ![ボタンの幅が最初のボタンの幅と同じになるように調整する](hello-ios-quickstart-images/vs-image16.png)

1. デザイン サーフェイスで 2 つ目の**ボタン**が選択されている状態で、 **[プロパティ]** の **[ID]** セクションの **[名前]** プロパティを `CallButton` に変更します。 **[タイトル]** プロパティを "Call" に変更します。

    ![[タイトル] プロパティを Call に変更する](hello-ios-quickstart-images/vs-image17.png)

    **[ファイル]、[すべて保存]** の順に選択するか、**Ctrl + s** を押し、変更内容を保存します。

1. 電話番号を英数字から数字に変換するコードをいくつか追加します。 最初に新しいファイルをプロジェクトに追加します。**ソリューション エクスプローラー**で **Phoneword** プロジェクトを右クリックし、 **[追加]、[新しい項目...]** の順に選択するか、**Ctrl + Shift + A** を押してください。

    ![電話番号を英数字から数字に変換するコードをいくつか追加する](hello-ios-quickstart-images/vs-image18.png)

1. (プロジェクトを右クリックし、[追加]、[新しいアイテム] の順に選択すると表示される) **[新しいアイテムの追加]** ダイアログで **[Apple]、[クラス]** の順に選択し、新しいファイルに「`PhoneTranslator`」という名前を付けます。

    ![PhoneTranslator という名前の新しいクラスを追加する](hello-ios-quickstart-images/vs-image19.w157.png)

    > [!IMPORTANT]
    > アイコンに C# がある 'class' テンプレートを必ず選択してください。 それ以外を選択すると、この新しいクラスを参照できないことがあります。

1. これで、新しい C# クラスが作成されます。 テンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

    **PhoneTranslator.cs** ファイルを保存して閉じます。

1. ハンドルとボタンの相互作用にロジックを追加できるように、**ソリューション エクスプローラー**の **ViewController.cs** をダブルクリックして開きます。

    ![ボタンとの相互作用を処理するために追加されたロジック](hello-ios-quickstart-images/vs-image20.png)

1. 最初に `TranslateButton` を接続します。 **ViewController** クラスで、`ViewDidLoad` メソッドを見つけます。 `ViewDidLoad` 内の `base.ViewDidLoad()` 呼び出しの下に次のボタン コードを追加します。

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {

        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call", UIControlState.Normal);
            CallButton.Enabled = false;
            }
        else {
            CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
            CallButton.Enabled = true;
            }
    };
    ```

    ファイルの名前空間が異なる場合、`using Phoneword;` を追加します。

1. 2 つ目のボタンである `CallButton` をユーザーが押したときに応答するためのコードを追加します。 `TranslateButton` のコードの下に次のコードを配置し、ファイルの一番上に `using Foundation;` を追加します。

    ```csharp
    CallButton.TouchUpInside += (object sender, EventArgs e) => {
        var url = new NSUrl ("tel:" + translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app,
            // otherwise show an alert dialog

        if (!UIApplication.SharedApplication.OpenUrl (url)) {
                        var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                        alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        PresentViewController (alert, true, null);
                    }
    };
    ```

1. 変更内容を保存したら、 **[ビルド]、[ソリューションのビルド]** の順に選択するか、**Ctrl + Shift + B** を押し、アプリケーションをビルドします。アプリケーションがコンパイルされると、IDE の一番下に成功メッセージが表示されます。

    ![IDE の一番下に成功メッセージが表示される](hello-ios-quickstart-images/vs-image21.png)

    エラーがある場合は、アプリケーションが正常にビルドされるまで、前の手順を実行し、誤りを修正します。

1. 最後に**リモートの iOS シミュレーター**でアプリケーションをテストします。 IDE ツール バーで、ドロップダウン メニューから **[デバッグ]** と **[iPhone 8 Plus iOS x.x]** を選択し、 **[開始]** (再生ボタンに似た緑の三角形) を押します。

    ![[開始] を押す](hello-ios-quickstart-images/vs-image27.png)

1. これにより、iOS シミュレーター内でアプリケーションが起動します。

    ![iOS シミュレーター内で実行されているアプリケーション](hello-ios-quickstart-images/vs-image28.png)

    iOS シミュレーターでは通話に対応していません。代わりに、電話をかけようとすると警告ダイアログが表示されます。

    ![電話をかけようとすると警告ダイアログが表示される](hello-ios-quickstart-images/vs-image29.png)

::: zone-end

おつかれさまでした。これで最初の Xamarin.iOS アプリケーションが完成しました。

次は、「[Hello, iOS Deep Dive](~/ios/get-started/hello-ios/hello-ios-deepdive.md)」で、このガイドに登場したツールやスキルを詳しく分析します。

## <a name="related-links"></a>関連リンク

- [Xamarin アプリのアイコンと起動イメージ (サンプル)](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true)
- [Hello, iOS (サンプル)](/samples/xamarin/ios-samples/hello-ios)
- [iOS ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS プロビジョニング ポータル](https://developer.apple.com/ios/manage/overview/index.action)