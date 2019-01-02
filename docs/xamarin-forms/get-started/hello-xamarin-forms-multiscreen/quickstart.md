---
title: Xamarin.Forms マルチスクリーン クイックスタート
description: この記事では、Phoneword アプリケーションの拡張方法を紹介します。アプリケーションの通話履歴を追跡記録するための 2 つ目の画面を追加します。
zone_pivot_groups: platform
ms.prod: quickstart
ms.assetid: 255d93b9-518c-4e5d-a9cd-4dd8a7945a7f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2018
ms.openlocfilehash: 406a7e780136f87dc85b28b970de2bfd02db36ec
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53052372"
---
# <a name="xamarinforms-multiscreen-quickstart"></a>Xamarin.Forms マルチスクリーン クイックスタート

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/PhonewordMultiscreen/)

このクイックスタートでは、Phoneword アプリケーションの拡張方法を紹介します。アプリケーションの通話履歴を追跡記録するための 2 つ目の画面を追加します。 最終的なアプリケーションは、次のとおりです。

[![](quickstart-images/intro-app-examples-sml.png "Phoneword アプリケーション")](quickstart-images/intro-app-examples.png#lightbox "Phoneword アプリケーション")

::: zone pivot="windows"

## <a name="update-the-app-with-visual-studio"></a>Visual Studio でアプリを更新する

1. Visual Studio を起動します。 スタート ページで **[プロジェクトを開く]** をクリックし、**[プロジェクトを開く]** ダイアログで Phoneword プロジェクトのソリューション ファイルを選択します。

    ![](quickstart-images/vs/open-solution.png "プロジェクトを開く")

2. **ソリューション エクスプローラー**で **[Phoneword]** プロジェクトを右クリックし、**[追加]、[新しい項目]** を選択します。

    ![](quickstart-images/vs/add-new-item.png "新しい項目の追加")

3. **[新しい項目の追加]** ダイアログで、**[Visual C# アイテム]、[Xamarin.Forms]、[コンテンツ ページ]** の順に選択し、新しい項目に「**CallHistoryPage**」という名前を付け、**[追加]** ボタンをクリックします。 これによってページに、**CallHistoryPage** という名前のページが追加されます。

    ![](quickstart-images/vs/add-callhistorypage-class.png "Xamarin.Forms プロジェクト テンプレート")

4. **CallHistoryPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードでは、ページのユーザー インターフェイスが宣言によって定義されます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.CallHistoryPage"
                       Title="Call History">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
        </StackLayout>
    </ContentPage>
    ```

    **CTRL + S** を押し、**CallHistoryPage.xaml** への変更内容を保存してから、ファイルを閉じます。

5. **ソリューション エクスプローラー**で、共有 **Phoneword** プロジェクトの **App.xaml.cs** ファイルをダブルクリックして開きます。

    ![](quickstart-images/vs/open-app-class.png "App.xaml.cs を開く")

6. **App.xaml.cs** で、`System.Collections.Generic` 名前空間をインポートし、`PhoneNumbers` プロパティの宣言を追加し、`App` コンストラクターでこのプロパティを初期化し、[`MainPage`](xref:Xamarin.Forms.Application.MainPage) プロパティを初期化して [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) にします。 `PhoneNumbers` コレクションは、アプリケーションに呼び出され、変換された各電話番号の一覧を保存するときに使用されます。

    ```csharp
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
    ```

    **CTRL + S** を押し、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

7. **ソリューション エクスプローラー**で、共有 **Phoneword** プロジェクトの **MainPage.xaml** ファイルをダブルクリックして開きます。

    ![](quickstart-images/vs/open-mainpage-xaml.png "MainPage.xaml を開く")

8. **MainPage.xaml** で、[`StackLayout`](xref:Xamarin.Forms.StackLayout) コントロールの終わりに [`Button`](xref:Xamarin.Forms.Button) コントロールを追加します。 このボタンは通話履歴ページに移動するために使用されます。

    ```xaml
    <StackLayout VerticalOptions="FillAndExpand"
                 HorizontalOptions="FillAndExpand"
                 Orientation="Vertical"
                 Spacing="15">
      ...
      <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
      <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
              Clicked="OnCallHistory" />
    </StackLayout>
    ```

    **CTRL + S** を押し、**MainPage.xaml** への変更内容を保存してから、ファイルを閉じます。

9. **ソリューション エクスプローラー**で、**MainPage.xaml.cs** をダブルクリックして開きます。

    ![](quickstart-images/vs/open-mainpage-codebehind.png "MainPage.xaml.cs を開く")

10. **MainPage.xaml.cs** で、`OnCallHistory` イベント ハンドラー メソッドを追加します。さらに、`dialer` 変数が `null` でなければ、変換された電話番号を `App.PhoneNumbers` コレクションに追加するように `OnCall` イベント ハンドラー メソッドを修正し、`callHistoryButton` を有効にします。

    ```csharp
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
    ```

    **CTRL + S** を押し、**MainPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

11. Visual Studio で、**[ビルド]、[ソリューションのビルド]** メニュー項目を選択します (または **CTRL + SHIFT + B** を押します)。 アプリケーションがビルドされ、Visual Studio のステータス バーに成功のメッセージが表示されます。

    ![](quickstart-images/vs/build-successful.png "ビルドに成功しました")

    エラーがある場合は、アプリケーションが正常にビルドされるまで、前の手順を実行し、誤りを修正します。

12. Visual Studio ツールバーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、アプリケーションを起動します。

    ![](quickstart-images/vs/start.png "Visual Studio ツールバー")
    ![](quickstart-images/vs/phone-result-uwp.png "Phoneword アプリケーション UWP")

13. **ソリューション エクスプローラー**で **Phoneword.Droid** プロジェクトを右クリックし、**[スタートアップ プロジェクトに設定]** を選択します。
14. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、Android エミュレーター内にアプリケーションを起動します。
15. iOS デバイスがあり、Xamarin.Forms での開発のための Mac システム要件が揃っている場合、同様の手法を使用して、iOS デバイスにアプリを展開します。 または、アプリを [iOS リモート シミュレーター](~/tools/ios-simulator/index.md)に展開します。

    > [!NOTE]
    > デバイス エミュレーターでは電話呼び出しはサポートされていません。

::: zone-end
::: zone pivot="macos"

## <a name="update-the-app-with-visual-studio-for-mac"></a>Visual Studio for Mac でアプリを更新する

1. Visual Studio for Mac を起動します。 スタート ページで **[開く]** をクリックし、ダイアログで Phoneword プロジェクトのソリューション ファイルを選択します。

    ![](quickstart-images/xs/open-solution.png "ソリューションを開く")

2. **Solution Pad** で **Phoneword** プロジェクトを選択し、右クリックして、**[追加]、[新しいファイル]** の順に選択します。

    ![](quickstart-images/xs/add-new-file.png "新しいファイルの追加")

3. **[新しいファイル]** ダイアログで、**[フォーム]、[Forms ContentPage Xaml]\(フォーム コンテンツ ページの Xaml\)** の順に選択し、新しいファイルを **CallHistoryPage** と命名し、**[新規]** ボタンをクリックします。 これによってページに、**CallHistoryPage** という名前のページが追加されます。

    ![](quickstart-images/xs/add-callhistorypage-class.png "フォーム ContentPage の追加")

4. **Solution Pad** で、**CallHistoryPage.xaml** をダブルクリックして開きます。

    ![](quickstart-images/xs/open-callhistorypage-xaml.png "CallHistoryPage.xaml を開く")

5. **CallHistoryPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードでは、ページのユーザー インターフェイスが宣言によって定義されます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.CallHistoryPage"
                       Title="Call History">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
        </StackLayout>
    </ContentPage>      
    ```

    **[ファイル]、[保存]** の順に選択し (または **& #8984; + S** キーを押し)、**CallHistoryPage.xaml** への変更内容を保存してから、ファイルを閉じます。

6. **Solution Pad** で **App.xaml.cs** をダブルクリックして開きます。

    ![](quickstart-images/xs/open-app-class.png "App.xaml.cs を開く")

7. **App.xaml.cs** で、`System.Collections.Generic` 名前空間をインポートし、`PhoneNumbers` プロパティの宣言を追加し、`App` コンストラクターでこのプロパティを初期化し、[`MainPage`](xref:Xamarin.Forms.Application.MainPage) プロパティを初期化して [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) にします。 `PhoneNumbers` コレクションは、アプリケーションに呼び出され、変換された各電話番号の一覧を保存するときに使用されます。

    ```csharp
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
    ```

    **[ファイル]、[保存]** の順に選択し (または **& #8984; + S** キーを押し)、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

8. **Solution Pad** で、**MainPage.xaml** をダブルクリックして開きます。

    ![](quickstart-images/xs/open-mainpage-xaml.png "MainPage.xaml を開く")

9. **MainPage.xaml** で、[`StackLayout`](xref:Xamarin.Forms.StackLayout) コントロールの終わりに [`Button`](xref:Xamarin.Forms.Button) コントロールを追加します。 このボタンは通話履歴ページに移動するために使用されます。

    ```xaml
    <StackLayout VerticalOptions="FillAndExpand"
                 HorizontalOptions="FillAndExpand"
                 Orientation="Vertical"
                 Spacing="15">
      ...
      <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
      <Button x:Name="callHistoryButton" Text="Call History" IsEnabled="false"
              Clicked="OnCallHistory" />
    </StackLayout>
    ```

    **[ファイル]、[保存]** の順に選択し (または **& #8984; + S** キーを押し)、**MainPage.xaml** への変更内容を保存してから、ファイルを閉じます。

10. **Solution Pad** で **MainPage.xaml.cs** をダブルクリックして開きます。

    ![](quickstart-images/xs/open-mainpage-codebehind.png "MainPage.xaml.cs を開く")

11. **MainPage.xaml.cs** で、`OnCallHistory` イベント ハンドラー メソッドを追加します。さらに、`dialer` 変数が `null` でなければ、変換された電話番号を `App.PhoneNumbers` コレクションに追加するように `OnCall` イベント ハンドラー メソッドを修正し、`callHistoryButton` を有効にします。

    ```csharp
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
    ```

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**MainPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

12. Visual Studio for Mac で、**[ビルド]、[すべてビルド]** の順にメニュー項目を選択します (または **&#8984; + B** キーを押します)。 アプリケーションがビルドされ、Visual Studio for Mac のツール バーに成功のメッセージが表示されます。

    ![](quickstart-images/xs/build-successful.png "ビルドに成功しました")

    エラーがある場合は、アプリケーションが正常にビルドされるまで、前の手順を実行し、誤りを修正します。

13. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、iOS シミュレーター内にアプリケーションを起動します。

    ![](quickstart-images/xs/start.png "Visual Studio for Ma ツールバー")
    ![](quickstart-images/xs/phone-result-ios.png "iOS シミュレーター")

    注: iOS シミュレーターでは電話呼び出しはサポートされていません。

14. **Solution Pad** で **Phoneword.Droid** プロジェクトを選択し、右クリックして、**[スタートアップ プロジェクトに設定]** を選択します。

    ![](quickstart-images/xs/set-startup-project.png "スタートアップ プロジェクトに設定")

15. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、Android エミュレーター内にアプリケーションを起動します。

    ![](quickstart-images/xs/phone-result-android.png "Android エミュレーター")

    > [!NOTE]
    > デバイス エミュレーターでは電話呼び出しはサポートされていません。

::: zone-end

マルチスクリーン Xamarin.Forms アプリケーションの完成おつかれさまでした。 このガイドの「[次のトピック](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/deepdive.md)」では、Xamarin.Forms 使用したページ ナビゲーションとデータ バインディングに関する理解を深めるために、このチュートリアルで実行した手順を再確認します。

## <a name="related-links"></a>関連リンク

- [Phoneword (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
- [PhonewordMultiscreen (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/PhonewordMultiscreen/)
