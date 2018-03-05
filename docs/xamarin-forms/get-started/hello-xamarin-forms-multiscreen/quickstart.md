---
title: "Xamarin.Forms マルチスクリーン クイックスタート"
ms.topic: article
ms.prod: xamarin
ms.assetid: 255d93b9-518c-4e5d-a9cd-4dd8a7945a7f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/06/2018
ms.openlocfilehash: 6a24ff05ae2a2c2368650c368cc408f0219ce21e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinforms-multiscreen-quickstart"></a>Xamarin.Forms マルチスクリーン クイックスタート

このクイックスタートでは、Phoneword アプリケーションの拡張方法を紹介します。アプリケーションの通話履歴を追跡記録するための 2 つ目の画面を追加します。 最終的なアプリケーションは、次のとおりです。

[![](quickstart-images/intro-app-examples-sml.png "Phoneword アプリケーション")](quickstart-images/intro-app-examples.png "Phoneword アプリケーション")

Phoneword アプリケーションを次のように拡張します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio を起動します。 スタート ページで **[プロジェクトを開く]** をクリックし、**[プロジェクトを開く]** ダイアログで Phoneword プロジェクトのソリューション ファイルを選択します。

  ![](quickstart-images/vs/open-solution.png "プロジェクトを開く")

1. **ソリューション エクスプローラー**で **[Phoneword]** プロジェクトを右クリックし、**[追加]、[新しい項目]** を選択します。

  ![](quickstart-images/vs/add-new-item.png "新しい項目の追加")

1. **[新しい項目の追加]** ダイアログで、**[Visual C# アイテム]、[Xamarin.Forms]、[コンテンツ ページ]** の順に選択し、新しい項目に「**CallHistoryPage**」という名前を付け、**[追加]** ボタンをクリックします。 これによってページに、**CallHistoryPage** という名前のページが追加されます。

  ![](quickstart-images/vs/add-callhistorypage-class.png "Xamarin.Forms プロジェクト テンプレート")

1. **CallHistoryPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードでは、ページのユーザー インターフェイスが宣言によって定義されます。

        <?xml version="1.0" encoding="UTF-8"?>
        <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                           xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                           x:Class="Phoneword.CallHistoryPage"
                           Title="Call History">
            <ContentPage.Padding>
                <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS" Value="20, 40, 20, 20" />
                    <On Platform="Android, WinPhone, Windows" Value="20" />
                </OnPlatform>
            </ContentPage.Padding>
            <StackLayout>
              <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
            </StackLayout>
        </ContentPage>

  **CTRL + S** を押し、**CallHistoryPage.xaml** への変更内容を保存してから、ファイルを閉じます。

1. **ソリューション エクスプローラー**で、**App.xaml.cs** をダブルクリックして開きます。

  ![](quickstart-images/vs/open-app-class.png "App.xaml.cs を開く")

1. **App.xaml.cs** で、`System.Collections.Generic` 名前空間をインポートし、`PhoneNumbers` プロパティの宣言を追加し、`App` コンストラクターでこのプロパティを初期化し、[`MainPage`](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) プロパティを初期化して [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) にします。 `PhoneNumbers` コレクションは、アプリケーションに呼び出され、変換された各電話番号の一覧を保存するときに使用されます。

        using System.Collections.Generic;
        using Xamarin.Forms;
        using Xamarin.Forms.Xaml;

        [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
        namespace Phoneword
        {
            public partial class App : Application
            {
                public static IList<string> PhoneNumbers { get; set; }

                public App()
                {
                    InitializeComponent();
                    PhoneNumbers = new List<string>();
                    MainPage = new NavigationPage(new MainPage());
                }
                ...
            }
        }

  **CTRL + S** を押し、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

1. **ソリューション エクスプローラー**で、**MainPage.xaml** をダブルクリックして開きます。

  ![](quickstart-images/vs/open-mainpage-xaml.png "MainPage.xaml を開く")

1. **MainPage.xaml** で、[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) コントロールの終わりに [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) コントロールを追加します。 このボタンは通話履歴ページに移動するために使用されます。

        <StackLayout VerticalOptions="FillAndExpand"
                     HorizontalOptions="FillAndExpand"
                     Orientation="Vertical"
                     Spacing="15">
          ...
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
          <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
                  Clicked="OnCallHistory" />
        </StackLayout>

  **CTRL + S** を押し、**MainPage.xaml** への変更内容を保存してから、ファイルを閉じます。

1. **ソリューション エクスプローラー**で、**MainPage.xaml.cs** をダブルクリックして開きます。

  ![](quickstart-images/vs/open-mainpage-codebehind.png "MainPage.xaml.cs を開く")

1. **MainPage.xaml.cs** で、`OnCallHistory` イベント ハンドラー メソッドを追加します。さらに、`dialer` 変数が `null` でなければ、変換された電話番号を `App.PhoneNumbers` コレクションに追加するように `OnCall` イベント ハンドラー メソッドを修正し、`callHistoryButton` を有効にします。

        using System;
        using Xamarin.Forms;

        namespace Phoneword
        {
            public partial class MainPage : ContentPage
            {
                ...

                async void OnCall(object sender, EventArgs e)
                {
                    ...
                    if (dialer != null) {
                        App.PhoneNumbers.Add (translatedNumber);
                        callHistoryButton.IsEnabled = true;
                        dialer.Dial (translatedNumber);
                    }
                    ...
                }

                async void OnCallHistory(object sender, EventArgs e)
                {
                    await Navigation.PushAsync (new CallHistoryPage ());
                }
            }
        }

  **CTRL + S** を押し、**MainPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

1. Visual Studio で、**[ビルド]、[ソリューションのビルド]** メニュー項目を選択します (または **CTRL + SHIFT + B** を押します)。 アプリケーションがビルドされ、Visual Studio のステータス バーに成功のメッセージが表示されます。

  ![](quickstart-images/vs/build-successful.png "ビルドに成功しました")

  エラーがある場合は、アプリケーションが正常にビルドされるまで、前の手順を実行し、誤りを修正します。

1. Visual Studio ツールバーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、アプリケーションを起動します。

  ![](quickstart-images/vs/start.png "Visual Studio ツールバー")
  ![](quickstart-images/vs/phone-result-uwp.png "Phoneword アプリケーション UWP")

1. **ソリューション エクスプローラー**で **Phoneword.Droid** プロジェクトを右クリックし、**[スタートアップ プロジェクトに設定]** を選択します。
1. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、Android エミュレーター内にアプリケーションを起動します。
1. iOS デバイスがあり、Xamarin.Forms での開発のための Mac システム要件が揃っている場合、同様の手法を使用して、iOS デバイスにアプリを展開します。 または、アプリを [iOS リモート シミュレーター](~/tools/ios-simulator.md)に展開します。

  注: すべてのシミュレーターで電話呼び出しはサポートされていません。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Visual Studio for Mac を起動します。 スタート ページで **[開く]** をクリックし、ダイアログで Phoneword プロジェクトのソリューション ファイルを選択します。

  ![](quickstart-images/xs/open-solution.png "ソリューションを開く")

1. **Solution Pad** で **Phoneword** プロジェクトを選択し、右クリックして、**[追加]、[新しいファイル]** の順に選択します。

  ![](quickstart-images/xs/add-new-file.png "新しいファイルの追加")

1. **[新しいファイル]** ダイアログで、**[フォーム]、[Forms ContentPage Xaml]\(フォーム コンテンツ ページの Xaml\)** の順に選択し、新しいファイルを **CallHistoryPage** と命名し、**[新規]** ボタンをクリックします。 これによってページに、**CallHistoryPage** という名前のページが追加されます。

  ![](quickstart-images/xs/add-callhistorypage-class.png "フォーム ContentPage の追加")

1. **Solution Pad** で、**CallHistoryPage.xaml** をダブルクリックして開きます。

  ![](quickstart-images/xs/open-callhistorypage-xaml.png "CallHistoryPage.xaml を開く")

1. **CallHistoryPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードでは、ページのユーザー インターフェイスが宣言によって定義されます。

        <?xml version="1.0" encoding="UTF-8"?>
        <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                           xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                           x:Class="Phoneword.CallHistoryPage"
                           Title="Call History">
            <ContentPage.Padding>
                <OnPlatform x:TypeArguments="Thickness">
                    <On Platform="iOS" Value="20, 40, 20, 20" />
                    <On Platform="Android, WinPhone, Windows" Value="20" />
                </OnPlatform>
            </ContentPage.Padding>
            <StackLayout>
              <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
            </StackLayout>
        </ContentPage>      

  **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**CallHistoryPage.xaml** への変更内容を保存してから、ファイルを閉じます。

1. **Solution Pad** で **App.xaml.cs** をダブルクリックして開きます。

  ![](quickstart-images/xs/open-app-class.png "App.xaml.cs を開く")

1. **App.xaml.cs** で、`System.Collections.Generic` 名前空間をインポートし、`PhoneNumbers` プロパティの宣言を追加し、`App` コンストラクターでこのプロパティを初期化し、[`MainPage`](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) プロパティを初期化して [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) にします。 `PhoneNumbers` コレクションは、アプリケーションに呼び出され、変換された各電話番号の一覧を保存するときに使用されます。

        using System.Collections.Generic;
        using Xamarin.Forms;
        using Xamarin.Forms.Xaml;

        [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
        namespace Phoneword
        {
            public partial class App : Application
            {
                public static IList<string> PhoneNumbers { get; set; }

                public App()
                {
                    InitializeComponent();
                    PhoneNumbers = new List<string>();
                    MainPage = new NavigationPage(new MainPage());
                }
                ...
            }
        }

  **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

1. **Solution Pad** で、**MainPage.xaml** をダブルクリックして開きます。

  ![](quickstart-images/xs/open-mainpage-xaml.png "MainPage.xaml を開く")

1. **MainPage.xaml** で、[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) コントロールの終わりに [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) コントロールを追加します。 このボタンは通話履歴ページに移動するために使用されます。

        <StackLayout VerticalOptions="FillAndExpand"
                     HorizontalOptions="FillAndExpand"
                     Orientation="Vertical"
                     Spacing="15">
          ...
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
          <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
                  Clicked="OnCallHistory" />
        </StackLayout>

  **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**MainPage.xaml** への変更内容を保存してから、ファイルを閉じます。

1. **Solution Pad** で **MainPage.xaml.cs** をダブルクリックして開きます。

  ![](quickstart-images/xs/open-mainpage-codebehind.png "MainPage.xaml.cs を開く")

1. **MainPage.xaml.cs** で、`OnCallHistory` イベント ハンドラー メソッドを追加します。さらに、`dialer` 変数が `null` でなければ、変換された電話番号を `App.PhoneNumbers` コレクションに追加するように `OnCall` イベント ハンドラー メソッドを修正し、`callHistoryButton` を有効にします。

        using System;
        using Xamarin.Forms;

        namespace Phoneword
        {
            public partial class MainPage : ContentPage
            {
                ...

                async void OnCall(object sender, EventArgs e)
                {
                    ...
                    if (dialer != null) {
                        App.PhoneNumbers.Add (translatedNumber);
                        callHistoryButton.IsEnabled = true;
                        dialer.Dial (translatedNumber);
                    }
                    ...
                }

                async void OnCallHistory(object sender, EventArgs e)
                {
                    await Navigation.PushAsync (new CallHistoryPage ());
                }
            }
        }

  **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**MainPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

1. Visual Studio for Mac で、**[ビルド]、[すべてビルド]** の順にメニュー項目を選択します (または **&#8984; + B** キーを押します)。 アプリケーションがビルドされ、Visual Studio for Mac のツール バーに成功のメッセージが表示されます。

  ![](quickstart-images/xs/build-successful.png "ビルドに成功しました")

  エラーがある場合は、アプリケーションが正常にビルドされるまで、前の手順を実行し、誤りを修正します。

1. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、iOS シミュレーター内にアプリケーションを起動します。

  ![](quickstart-images/xs/start.png "Visual Studio for Ma ツールバー")
  ![](quickstart-images/xs/phone-result-ios.png "iOS シミュレーター")

  注: iOS シミュレーターでは電話呼び出しはサポートされていません。

1. **Solution Pad** で **Phoneword.Droid** プロジェクトを選択し、右クリックして、**[スタートアップ プロジェクトに設定]** を選択します。

  ![](quickstart-images/xs/set-startup-project.png "スタートアップ プロジェクトに設定")

1. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、Android エミュレーター内にアプリケーションを起動します。

  ![](quickstart-images/xs/phone-result-android.png "Android エミュレーター")

  注: Android シミュレーターでは電話呼び出しはサポートされていません。

-----

マルチスクリーン Xamarin.Forms アプリケーションの完成おつかれさまでした。 このガイドの「[次のトピック](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/deepdive.md)」では、Xamarin.Forms 使用したページ ナビゲーションとデータ バインディングに関する理解を深めるために、このチュートリアルで実行した手順を再確認します。


## <a name="related-links"></a>関連リンク

- [Phoneword (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
- [PhonewordMultiscreen (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/PhonewordMultiscreen/)
