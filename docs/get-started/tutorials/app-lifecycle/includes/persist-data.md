---
ms.openlocfilehash: 2c6c71f5ed46cc1cae66c5d1f412898825805cc6
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61187519"
---
[`Application`](xref:Xamarin.Forms.Application) サブクラスには、ライフサイクルの状態の変化を通してデータを格納するために使用できる静的 [`Properties`](xref:Xamarin.Forms.Application.Properties) ディクショナリがあります。 このディクショナリでは、`string` キーが使用され、`object` 値が格納されます。 このディクショナリはデバイスに自動的に保存され、アプリケーションの再起動時にデータが再作成されます。

> [!IMPORTANT]
> [`Properties`](xref:Xamarin.Forms.Application.Properties) ディクショナリでは、ストレージ用のプリミティブ型のみをシリアル化できます。

この演習では、バックグラウンド処理時に [`Entry`](xref:Xamarin.Forms.Entry) のテキストを保持し、アプリケーションの再起動時にテキストを `Entry` に復元するようにアプリケーションを変更します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **ソリューション エクスプローラー**の **[AppLifecycleTutorial]** プロジェクトで、**[App.xaml]** を展開し、**[App.xaml.cs]** をダブルクリックして開きます。 次に、**App.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace AppLifecycleTutorial
    {
        public partial class App : Application
        {
            const string displayText = "displayText";

            public string DisplayText { get; set; }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                Console.WriteLine("OnStart");

                if (Properties.ContainsKey(displayText))
                {
                    DisplayText = (string)Properties[displayText];
                }
            }

            protected override void OnSleep()
            {
                Console.WriteLine("OnSleep");
                Properties[displayText] = DisplayText;
            }

            protected override void OnResume()
            {
                Console.WriteLine("OnResume");
            }
        }
    }
    ```

    このコードは `DisplayText` プロパティと `displayText` 定数を定義します。 アプリケーションがバックグラウンド化されるか終了すると、`OnSleep` メソッドのオーバーライドは `displayText` のキーに対して `DisplayText` プロパティ値を [`Properties`](xref:Xamarin.Forms.Application.Properties) ディクショナリに追加します。 その後、`Properties` ディクショナリに `displayText` キーが含まれていれば、アプリケーションの起動時にそのキーの値が `DisplayText` プロパティに復元されます。

    > [!IMPORTANT]
    > 予期しないエラーを防ぐために、必ず [`Properties`](xref:Xamarin.Forms.Application.Properties) ディクショナリでキーが存在することを確認してからアクセスしてください。

    `OnResume` メソッドのオーバーロードで [`Properties`](xref:Xamarin.Forms.Application.Properties) ディクショナリからデータを復元する必要はありません。 これは、アプリケーションがバックグラウンド化されても、アプリケーションとその状態はまだメモリに保持されているためです。

1. **ソリューション エクスプローラー**の **AppLifecycleTutorial** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、**MainPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="AppLifecycleTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="entry"
                   Placeholder="Enter text here"
                   Completed="OnEntryCompleted" />
        </StackLayout>
    </ContentPage>
    ```

    このコードは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の [`Entry`](xref:Xamarin.Forms.Entry) から構成されるページのユーザー インターフェイスを宣言によって定義します。 [`Entry.Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) プロパティは、`Entry` が最初に表示されたときに表示されるプレースホルダー テキストを指定し、`OnEntryCompleted` という名前のイベント ハンドラーが [`Completed`](xref:Xamarin.Forms.Entry.Completed) イベントに登録されます。 また、`Entry` には `x:Name` 属性で指定された名前があります。 これにより、分離コード ファイルは、割り当てられた名前を使用して `Entry` オブジェクトにアクセスできます。

1. **ソリューション エクスプローラー**の **AppLifecycleTutorial** プロジェクトで **[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、**MainPage.xaml.cs** で、`OnAppearing` メソッドのオーバーライドと `OnEntryCompleted` イベント ハンドラーをクラスに追加します。

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();

        entry.Text = (Application.Current as App).DisplayText;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        (Application.Current as App).DisplayText = entry.Text;
    }
    ```

    `OnAppearing` メソッドは `App.DisplayText` プロパティの値を取得し、それを [`Entry`](xref:Xamarin.Forms.Entry) の [`Text`](xref:Xamarin.Forms.Entry.Text) プロパティ値として設定します。

    > [!NOTE]
    > `OnAppearing` メソッドのオーバーライドは、[`ContentPage`](xref:Xamarin.Forms.ContentPage) が配置された後、それが表示される直前に実行されます。 したがって、これは Xamarin.Forms ビューのコンテンツを設定するのに適した場所です。

    テキストが [`Entry`](xref:Xamarin.Forms.Entry) で終了したら、Return キーで `OnEntryCompleted` メソッドを実行します。`Entry` テキストが `App.DisplayText` プロパティに格納されます。

1. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [`Entry`](xref:Xamarin.Forms.Entry) にテキストを入力してReturn キーを押します。 次に、[ホーム] ボタンをタップして `OnSleep` メソッドを呼び出してアプリケーションをバックグラウンドにします。

    最後に、Visual Studio からアプリケーションをもう一度起動すると、以前に [`Entry`](xref:Xamarin.Forms.Entry) に入力されたテキストが復元されます。

    [![iOS および Android でライフサイクルの状態の変化を通して Text プロパティが保持されるエントリのスクリーンショット](../images/persist-data.png "ライフサイクルの状態の変化を通して Text プロパティが保持されるエントリ")](../images/persist-data-large.png#lightbox "ライフサイクルの状態の変化を通して Text プロパティが保持されるエントリ")

    プロパティ ディクショナリへのデータの永続化の詳細については、「[Xamarin.Forms App Class](~/xamarin-forms/app-fundamentals/application-class.md)」ガイドの「[Properties Dictionary](~/xamarin-forms/app-fundamentals/application-class.md#properties-dictionary)」(プロパティ ディクショナリ) をご覧ください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Solution Pad** の **[AppLifecycleTutorial]** プロジェクトで、**[App.xaml]** を展開し、**[App.xaml.cs]** をダブルクリックして開きます。 次に、**App.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace AppLifecycleTutorial
    {
        public partial class App : Application
        {
            const string displayText = "displayText";

            public string DisplayText { get; set; }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                Console.WriteLine("OnStart");

                if (Properties.ContainsKey(displayText))
                {
                    DisplayText = (string)Properties[displayText];
                }
            }

            protected override void OnSleep()
            {
                Console.WriteLine("OnSleep");
                Properties[displayText] = DisplayText;
            }

            protected override void OnResume()
            {
                Console.WriteLine("OnResume");
            }
        }
    }
    ```

    このコードは `DisplayText` プロパティと `displayText` 定数を定義します。 アプリケーションがバックグラウンド化されるか終了すると、`OnSleep` メソッドのオーバーライドは `displayText` のキーに対して `DisplayText` プロパティ値を [`Properties`](xref:Xamarin.Forms.Application.Properties) ディクショナリに追加します。 その後、`Properties` ディクショナリに `displayText` キーが含まれていれば、アプリケーションの起動時にそのキーの値が `DisplayText` プロパティに復元されます。

    > [!IMPORTANT]
    > 予期しないエラーを防ぐために、必ず [`Properties`](xref:Xamarin.Forms.Application.Properties) ディクショナリでキーが存在することを確認してからアクセスしてください。

    `OnResume` メソッドのオーバーロードで [`Properties`](xref:Xamarin.Forms.Application.Properties) ディクショナリからデータを復元する必要はありません。 これは、アプリケーションがバックグラウンド化されても、アプリケーションとその状態はまだメモリに保持されているためです。

1. **Solution Pad** の **AppLifecycleTutorial** プロジェクトで、**[MainPage.xaml]** をダブルクリックして開きます。 次に、**MainPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="AppLifecycleTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="entry"
                   Placeholder="Enter text here"
                   Completed="OnEntryCompleted" />
        </StackLayout>
    </ContentPage>
    ```

    このコードは、[`StackLayout`](xref:Xamarin.Forms.StackLayout) の中の [`Entry`](xref:Xamarin.Forms.Entry) から構成されるページのユーザー インターフェイスを宣言によって定義します。 [`Entry.Placeholder`](xref:Xamarin.Forms.Entry.Placeholder) プロパティは、`Entry` が最初に表示されたときに表示されるプレースホルダー テキストを指定し、`OnEntryCompleted` という名前のイベント ハンドラーが [`Completed`](xref:Xamarin.Forms.Entry.Completed) イベントに登録されます。 また、`Entry` には `x:Name` 属性で指定された名前があります。 これにより、分離コード ファイルは、割り当てられた名前を使用して `Entry` オブジェクトにアクセスできます。

1. **Solution Pad** の **[AppLifecycleTutorial]** プロジェクトで、**[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。 次に、**MainPage.xaml.cs** で、`OnAppearing` メソッドのオーバーライドと `OnEntryCompleted` イベント ハンドラーをクラスに追加します。

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();

        entry.Text = (Application.Current as App).DisplayText;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        (Application.Current as App).DisplayText = entry.Text;
    }
    ```

    `OnAppearing` メソッドは `App.DisplayText` プロパティの値を取得し、それを [`Entry`](xref:Xamarin.Forms.Entry) の [`Text`](xref:Xamarin.Forms.Entry.Text) プロパティ値として設定します。

    > [!NOTE]
    > `OnAppearing` メソッドのオーバーライドは、[`ContentPage`](xref:Xamarin.Forms.ContentPage) が配置された後、それが表示される直前に実行されます。 したがって、これは Xamarin.Forms ビューのコンテンツを設定するのに適した場所です。

    テキストが [`Entry`](xref:Xamarin.Forms.Entry) で終了したら、Return キーで `OnEntryCompleted` メソッドを実行します。`Entry` テキストが `App.DisplayText` プロパティに格納されます。

1. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [`Entry`](xref:Xamarin.Forms.Entry) にテキストを入力してReturn キーを押します。 次に、[ホーム] ボタンをタップして `OnSleep` メソッドを呼び出してアプリケーションをバックグラウンドにします。

    最後に、Visual Studio for Mac からアプリケーションをもう一度起動すると、以前に [`Entry`](xref:Xamarin.Forms.Entry) に入力されたテキストが復元されます。

    [![iOS および Android でライフサイクルの状態の変化を通して Text プロパティが保持されるエントリのスクリーンショット](../images/persist-data.png "ライフサイクルの状態の変化を通して Text プロパティが保持されるエントリ")](../images/persist-data-large.png#lightbox "ライフサイクルの状態の変化を通して Text プロパティが保持されるエントリ")

    プロパティ ディクショナリへのデータの永続化の詳細については、「[Xamarin.Forms App Class](~/xamarin-forms/app-fundamentals/application-class.md)」ガイドの「[Properties Dictionary](~/xamarin-forms/app-fundamentals/application-class.md#properties-dictionary)」(プロパティ ディクショナリ) をご覧ください。
