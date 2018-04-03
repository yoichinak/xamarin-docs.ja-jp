---
title: Xamarin.Forms のクイック スタート
ms.topic: article
ms.prod: xamarin
ms.assetid: 3f2f9c2d-d204-43bc-8c8a-a55ce1e6d2c8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/06/2018
ms.openlocfilehash: 7c90b5bcca5a6b8d2a4b52c166ae6884646a61d2
ms.sourcegitcommit: 20ca85ff638dbe3a85e601b5eb09b2f95bda2807
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/28/2018
---
# <a name="xamarinforms-quickstart"></a>Xamarin.Forms のクイック スタート

このチュートリアルでは、英数字の電話番号 (ユーザーが入力) を数字の電話番号に変換してから、その番号に電話をかけるアプリケーションを作成する方法を説明します。 最終的なアプリケーションは、次のとおりです。

[![](quickstart-images/intro-app-examples-sml.png "Phoneword アプリケーション")](quickstart-images/intro-app-examples.png#lightbox "Phoneword アプリケーション")

Phoneword アプリケーションは次のように作成します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **[スタート]** 画面で、Visual Studio を起動します。 スタート ページが開きます。

    ![](quickstart-images/vs/start-page.png "Visual Studio")

2. Visual Studio で、**[新しいプロジェクトの作成]** をクリックして新しいプロジェクトを作成します。

    ![](quickstart-images/vs/new-solution.png "新しいプロジェクト")

3. **[新しいプロジェクト]** ダイアログで、**[クロスプラットフォーム]** をクリックして、**[Mobile App (Xamarin.Forms)]\(モバイル アプリ (Xamarin.Forms)\)** テンプレートを選択し、[名前] と [ソリューション名] を `Phoneword` に設定し、プロジェクトの適切な場所を選択し、**[OK]** ボタンをクリックします。

    ![](quickstart-images/vs/new-project.png "クロスプラットフォームのプロジェクト テンプレート")

4. **[New Cross Platform App]\(新しいクロス プラットフォーム アプリ\)** ダイアログで、**[空のアプリケーション]** をクリックして、UI テクノロジとして **[Xamarin.Forms]** を選択して、コードの共有方法として **[.NET Standard]** を選択し、**[OK]** ボタンをクリックします。

    ![](quickstart-images/vs/new-app.png "新しいクロスプラット フォーム アプリ")

5. **ソリューション エクスプローラー**の **Phoneword** プロジェクトで、**MainPage.xaml** をダブルクリックして開きます。

    ![](quickstart-images/vs/open-mainpage-xaml.png "MainPage.xaml を開く")

6. **MainPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードでは、ページのユーザー インターフェイスが宣言によって定義されます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.MainPage">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, WinPhone, Windows" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
    </ContentPage>
    ```

    **CTRL + S** を押し、**MainPage.xaml** への変更内容を保存してから、ファイルを閉じます。

7. **ソリューション エクスプローラー**で、**[MainPage.xaml]** を展開し、**[MainPage.xaml.cs]** をダブルクリックして開きます。

    ![](quickstart-images/vs/open-mainpage-codebehind.png "MainPage.xaml.cs を開く")

8. **MainPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 ユーザー インターフェイスで、**[翻訳]** と **[呼び出し]** ボタンがクリックされると、それに対して `OnTranslate` メソッドと `OnCall` メソッドがそれぞれ実行されます。

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            string translatedNumber;

            public MainPage ()
            {
                InitializeComponent ();
            }

            void OnTranslate (object sender, EventArgs e)
            {
                translatedNumber = Core.PhonewordTranslator.ToNumber (phoneNumberText.Text);
                if (!string.IsNullOrWhiteSpace (translatedNumber)) {
                    callButton.IsEnabled = true;
                    callButton.Text = "Call " + translatedNumber;
                } else {
                    callButton.IsEnabled = false;
                    callButton.Text = "Call";
                }
            }

            async void OnCall (object sender, EventArgs e)
            {
                if (await this.DisplayAlert (
                        "Dial a Number",
                        "Would you like to call " + translatedNumber + "?",
                        "Yes",
                        "No")) {
                    var dialer = DependencyService.Get<IDialer> ();
                    if (dialer != null)
                        dialer.Dial (translatedNumber);
                }
            }
        }
    }
    ```

    > [!NOTE]
    > この時点でアプリケーションをビルドしようとするとエラーが発生しますが、このエラーは後ほど修正されます。

    **CTRL + S** を押し、**MainPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

9. **ソリューション エクスプローラー**で、**[App.xaml]** を展開し、**[App.xaml.cs]** をダブルクリックして開きます。

    ![](quickstart-images/vs/open-app-class.png "App.xaml.cs を開く")

10. **App.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 `App` コンストラクターにより `MainPage` クラスがアプリケーションの起動時に表示されるページとして設定されます。

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public App()
            {
                InitializeComponent();
                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    **CTRL + S** を押し、**App.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

11. **ソリューション エクスプローラー**で **[Phoneword]** プロジェクトを右クリックし、**[追加]、[新しい項目]** を選択します。

    ![](quickstart-images/vs/add-new-item.png "新しい項目の追加")

12. **[新しい項目の追加]** ダイアログで、**[Visual C#]、[コード]、[クラス]** の順に選択し、新しいファイルに **PhoneTranslator** という名前を付け、**[追加]** ボタンをクリックします。

    ![](quickstart-images/vs/add-translator-class.png "新しいクラスの追加")

13. **PhoneTranslator.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードにより、電話の単語が電話番号に変換されます。

    ```csharp
    using System.Text;

    namespace Core
    {
        public static class PhonewordTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw))
                    return null;

                raw = raw.ToUpperInvariant();

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c))
                        newNumber.Append(c);
                    else
                    {
                        var result = TranslateToNumber(c);
                        if (result != null)
                            newNumber.Append(result);
                        // Bad character?
                        else
                            return null;
                    }
                }
                return newNumber.ToString();
            }

            static bool Contains(this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static readonly string[] digits = {
                "ABC", "DEF", "GHI", "JKL", "MNO", "PQRS", "TUV", "WXYZ"
            };

            static int? TranslateToNumber(char c)
            {
                for (int i = 0; i < digits.Length; i++)
                {
                    if (digits[i].Contains(c))
                        return 2 + i;
                }
                return null;
            }
        }
    }
    ```

    **CTRL + S** を押し、**PhoneTranslator.cs** への変更内容を保存してから、ファイルを閉じます。

14. **ソリューション エクスプローラー**で **[Phoneword]** プロジェクトを右クリックし、**[追加]、[新しい項目]** を選択します。

    ![](quickstart-images/vs/add-new-item.png "新しい項目の追加")

15. **[新しい項目の追加]** ダイアログで、**[Visual C#]、[コード]、[インターフェイス]** の順に選択し、新しいファイルに **IDialer** という名前を付け、**[追加]** ボタンをクリックします。

    ![](quickstart-images/vs/add-idialer-interface.png "新しいインターフェイスの追加")

16. **IDialer.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードでは、変換された電話番号にダイヤルするために、各プラットフォームに実装する必要がある `Dial` メソッドを定義します。

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```

    **CTRL + S** を押し、**IDialer.cs** への変更内容を保存してから、ファイルを閉じます。

    > [!NOTE]
    > これでアプリケーション共通のコードが完成しました。 これでプラットフォーム固有の電話ダイヤル コードが [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) として実装されます。

17. **ソリューション エクスプローラー**で **Phoneword.iOS** プロジェクトを右クリックし、**[追加]、[新しい項目]** を選択します。

    ![](quickstart-images/vs/add-new-item-ios.png "新しい項目の追加")

18. **[新しい項目の追加]** ダイアログで、**[Apple]、[コード]、[クラス]** の順に選択し、新しいファイルに **PhoneDialer** という名前を付け、**[追加]** ボタンをクリックします。

    ![](quickstart-images/vs/new-phone-dialer-ios.png "新しいクラスの追加")

19. **PhoneDialer.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードにより、iOS プラットフォームが、変換された電話番号をダイヤルする <code>Dial</code> メソッドが作成されます。

    ```csharp
    using Foundation;
    using Phoneword.iOS;
    using UIKit;
    using Xamarin.Forms;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.iOS
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                return UIApplication.SharedApplication.OpenUrl (
                    new NSUrl ("tel:" + number));
            }
        }
    }
    ```

    **CTRL + S** を押し、**PhoneDialer.cs** への変更内容を保存してから、ファイルを閉じます。

20. **ソリューション エクスプローラー**で **Phoneword.Android** プロジェクトを右クリックし、**[追加]、[新しい項目]** を選択します。

    ![](quickstart-images/vs/add-new-item-android.png "新しい項目の追加")

21. **[新しい項目の追加]** ダイアログで、**[Visual C#]、[Android]、[クラス]** の順に選択し、新しいファイルに **PhoneDialer** という名前を付け、**[追加]** ボタンをクリックします。

    ![](quickstart-images/vs/new-phone-dialer-android.png "新しいクラスの追加")

22. **PhoneDialer.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードにより、Android プラットフォームが、変換された電話番号をダイヤルする `Dial` メソッドが作成されます。

    ```csharp
    using Android.Content;
    using Android.Telephony;
    using Phoneword.Droid;
    using System.Linq;
    using Xamarin.Forms;
    using Uri = Android.Net.Uri;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.Droid
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                var context = MainActivity.Instance;
                if (context == null)
                    return false;

                var intent = new Intent (Intent.ActionCall);
                intent.SetData (Uri.Parse ("tel:" + number));

                if (IsIntentAvailable (context, intent)) {
                    context.StartActivity (intent);
                    return true;
                }

                return false;
            }

            public static bool IsIntentAvailable(Context context, Intent intent)
            {
                var packageManager = context.PackageManager;

                var list = packageManager.QueryIntentServices (intent, 0)
                    .Union (packageManager.QueryIntentActivities (intent, 0));

                if (list.Any ())
                    return true;

                var manager = TelephonyManager.FromContext (context);
                return manager.PhoneType != PhoneType.None;
            }
        }
    }
    ```

    **CTRL + S** を押し、**PhoneDialer.cs** への変更内容を保存してから、ファイルを閉じます。

23. **ソリューション エクスプローラー**の **Phoneword.Android** プロジェクトで、**MainActivity.cs** をダブルクリックして開き、テンプレート コードをすべて削除して、次のコードに置き換えます。

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
        {
            internal static MainActivity Instance { get; private set; }

            protected override void OnCreate(Bundle bundle)
            {
                TabLayoutResource = Resource.Layout.Tabbar;
                ToolbarResource = Resource.Layout.Toolbar;

                base.OnCreate(bundle);

                Instance = this;
                global::Xamarin.Forms.Forms.Init(this, bundle);
                LoadApplication(new App());
            }
        }
    }  
    ```

    **CTRL + S キー**を押して **MainPage.xaml.cs** の変更内容を保存してから、ファイルを閉じます。

24. **ソリューション エクスプローラー**の **[Phoneword.Android]** プロジェクトで、**[プロパティ]** をダブルクリックし、**[Android マニフェスト]** タブを選択します。

    ![](quickstart-images/vs/android-manifest.png "Android マニフェストを開く")

25. **[必要なアクセス許可]** セクションで、**CALL_PHONE** 権限を有効にします。 これによって、アプリケーションに電話をかけるアクセス許可が与えられます。

    ![](quickstart-images/vs/android-manifest-changed.png "CallPhone アクセス許可を有効にする")

    **CTRL + S** を押し、マニフェストに変更内容を保存してから、ファイルを閉じます。

26. **ソリューション エクスプローラー**で **Phoneword.UWP** プロジェクトを右クリックし、**[追加]、[新しい項目]** を選択します。

    ![](quickstart-images/vs/add-new-item-uwp.png "新しい項目の追加")

27. **[新しい項目の追加]** ダイアログで、**[Visual C#]、[コード]、[クラス]** の順に選択し、新しいファイルに **PhoneDialer** という名前を付け、**[追加]** ボタンをクリックします。

    ![](quickstart-images/vs/new-phone-dialer-uwp.png "新しいクラスの追加")

28. **PhoneDialer.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードにより、ユニバーサル Windows プラットフォームが変換された電話番号をダイヤルする `Dial` メソッドとヘルパー メソッドが作成されます。

    ```csharp
    using Phoneword.UWP;
    using System;
    using System.Threading.Tasks;
    using Windows.ApplicationModel.Calls;
    using Windows.UI.Popups;
    using Xamarin.Forms;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.UWP
    {
        public class PhoneDialer : IDialer
        {
            bool dialled = false;

            public bool Dial(string number)
            {
                DialNumber(number);
                return dialled;
            }

            async Task DialNumber(string number)
            {
                var phoneLine = await GetDefaultPhoneLineAsync();
                if (phoneLine != null)
                {
                    phoneLine.Dial(number, number);
                    dialled = true;
                }
                else
                {
                    var dialog = new MessageDialog("No line found to place the call");
                    await dialog.ShowAsync();
                    dialled = false;
                }
            }

            async Task<PhoneLine> GetDefaultPhoneLineAsync()
            {
                var phoneCallStore = await PhoneCallManager.RequestStoreAsync();
                var lineId = await phoneCallStore.GetDefaultLineAsync();
                return await PhoneLine.FromIdAsync(lineId);
            }
        }
    }
    ```

    **CTRL + S** を押し、**PhoneDialer.cs** への変更内容を保存してから、ファイルを閉じます。

29. **ソリューション エクスプローラー**の **Phoneword.UWP** プロジェクトで、**[参照]** を右クリックし、**[参照の追加]** をクリックします。

    ![](quickstart-images/vs/uwp-add-reference.png "参照の追加")

30. **[参照マネージャー]** ダイアログで、**[Universal Windows]\(ユニバーサル Windows\)、[Extensions]\(拡張機能\)、[Windows Mobile Extensions for UWP]\(UWP 用 Windows モバイル拡張機能\)** をクリックし、**[OK]** ボタンをクリックします。

    ![](quickstart-images/vs/uwp-add-reference-extensions.png "Windows Mobile Extensions for UWP の追加")

31. **ソリューション エクスプローラー**の **Phoneword.UWP** プロジェクトで、**Package.appxmanifest** をダブルクリックします。

    ![](quickstart-images/vs/uwp-manifest.png "UWP マニフェストを開く")

31. **[機能]** ページで、**[電話呼び出し]** 機能を有効にします。 これによって、アプリケーションに電話をかけるアクセス許可が与えられます。

    ![](quickstart-images/vs/uwp-manifest-changed.png "[電話呼び出し] 機能を有効にする")

    **CTRL + S** を押し、マニフェストに変更内容を保存してから、ファイルを閉じます。

32. Visual Studio で、**[ビルド]、[ソリューションのビルド]** メニュー項目を選択します (または **CTRL + SHIFT + B** を押します)。 アプリケーションがビルドされ、Visual Studio のステータス バーに成功のメッセージが表示されます。

    ![](quickstart-images/vs/build-successful.png "ビルドに成功しました")

    エラーがある場合は、アプリケーションが正常にビルドされるまで、前の手順を実行し、誤りを修正します。

33. **ソリューション エクスプローラー**で **Phoneword.UWP** プロジェクトを右クリックし、**[スタートアップ プロジェクトに設定]** を選択します。

    ![](quickstart-images/vs/uwp-set-as-startup-project.png "スタートアップ プロジェクトに設定")

34. Visual Studio ツールバーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、アプリケーションを起動します。

    ![](quickstart-images/vs/start.png "Visual Studio ツールバー")
    ![](quickstart-images/vs/phone-result-uwp.png "Phoneword アプリケーション UWP")

35. **ソリューション エクスプローラー**で **Phoneword.Android** プロジェクトを右クリックし、**[スタートアップ プロジェクトに設定]** を選択します。
36. Visual Studio ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、Android エミュレーター内にアプリケーションを起動します。
37. iOS デバイスがあり、Xamarin.Forms での開発のための Mac システム要件が揃っている場合、同様の手法を使用して、iOS デバイスにアプリを展開します。 または、アプリを [iOS リモート シミュレーター](~/tools/ios-simulator.md)に展開します。

    注: すべてのシミュレーターで電話呼び出しはサポートされていません。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Visual Studio for Mac を起動し、スタート ページで **[新しいプロジェクト]** をクリックして新しいプロジェクトを作成します。

    ![](quickstart-images/xs/new-solution.png "新しいソリューション")

2. **[新しいプロジェクト用のテンプレートを選びます]** ダイアログで、**[マルチプラットフォーム]、[アプリ]** の順にクリックし、**[空白フォームのアプリ]** テンプレートを選択して、**[次へ]** ボタンをクリックします。

    ![](quickstart-images/xs/choose-template.png "テンプレートを選択します")

3. **[Configure your Blank Forms app]\(空白フォームのアプリの構成\)** ダイアログで、新しいアプリに `Phoneword` と名前を付け、**[ポータブル クラス ライブラリの使用]** ラジオ ボタンがオンになっていることを確認し、**[ユーザー インターフェイス ファイルに XAML を使用]** チェック ボックスがオンになっていることを確認し、**[次へ]** ボタンをクリックします。

    ![](quickstart-images/xs/configure-app.png "フォーム アプリケーションの構成")

4. **[Configure your new Blank Forms app]\(新しい空白フォームのアプリの構成\)** ダイアログでは、ソリューションとプロジェクトの名前は `Phoneword` に設定したままにし、プロジェクトに適切な場所を選択し、**[作成]** をクリックしてプロジェクトを作成します。

    ![](quickstart-images/xs/configure-project.png "フォーム プロジェクトの構成")

5. **Solution Pad** で **Phoneword** プロジェクトを選択し、右クリックして、**[追加]、[新しいファイル]** の順に選択します。

    ![](quickstart-images/xs/add-new-file.png "新しいファイルの追加")

6. **[新しいファイル]** ダイアログで、**[フォーム]、[Forms ContentPage Xaml]\(フォーム コンテンツ ページの Xaml\)** の順に選択し、新しいファイルを **MainPage** と命名し、**[新規]** ボタンをクリックします。 これによってページに、**MainPage** という名前のページが追加されます。

    ![](quickstart-images/xs/add-mainpage-class.png "新しい ContentPage の追加")

7. **Solution Pad** で、**MainPage.xaml** をダブルクリックして開きます。

    ![](quickstart-images/xs/open-mainpage-xaml.png "MainPage.xaml を開く")

8. **MainPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードでは、ページのユーザー インターフェイスが宣言によって定義されます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.MainPage">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, WinPhone, Windows" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
    </ContentPage>
    ```

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**MainPage.xaml** への変更内容を保存してから、ファイルを閉じます。

9. **Solution Pad** で **MainPage.xaml.cs** をダブルクリックして開きます。

    ![](quickstart-images/xs/open-mainpage-codebehind.png "MainPage.xaml.cs を開く")

10. **MainPage.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 ユーザー インターフェイスで、**[翻訳]** と **[呼び出し]** ボタンがクリックされると、それに対して `OnTranslate` メソッドと `OnCall` メソッドがそれぞれ実行されます。

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace Phoneword
    {
        public partial class MainPage : ContentPage
        {
            string translatedNumber;

            public MainPage ()
            {
                InitializeComponent ();
            }

            void OnTranslate (object sender, EventArgs e)
            {
                translatedNumber = Core.PhonewordTranslator.ToNumber (phoneNumberText.Text);
                if (!string.IsNullOrWhiteSpace (translatedNumber)) {
                    callButton.IsEnabled = true;
                    callButton.Text = "Call " + translatedNumber;
                } else {
                    callButton.IsEnabled = false;
                    callButton.Text = "Call";
                }
            }

            async void OnCall (object sender, EventArgs e)
            {
                if (await this.DisplayAlert (
                        "Dial a Number",
                        "Would you like to call " + translatedNumber + "?",
                        "Yes",
                        "No")) {
                    var dialer = DependencyService.Get<IDialer> ();
                    if (dialer != null)
                        dialer.Dial (translatedNumber);
                }
            }
        }
    }
    ```

    > [!NOTE]
    > この時点でアプリケーションをビルドしようとするとエラーが発生しますが、このエラーは後ほど修正されます。

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**MainPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

11. **Solution Pad** で **App.xaml.cs** をダブルクリックして開きます。

    ![](quickstart-images/xs/open-app-class.png "App.xaml.cs を開く")

12. **App.xaml.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 `App` コンストラクターにより `MainPage` クラスがアプリケーションの起動時に表示されるページとして設定されます。

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Xaml;

    [assembly: XamlCompilation(XamlCompilationOptions.Compile)]
    namespace Phoneword
    {
        public partial class App : Application
        {
            public App()
            {
                InitializeComponent();
                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**Phoneword.cs** への変更内容を保存してから、ファイルを閉じます。

13. **Solution Pad** で **Phoneword** プロジェクトを選択し、右クリックして、**[追加]、[新しいファイル]** の順に選択します。

    ![](quickstart-images/xs/add-new-translator-file.png "新しいファイルの追加")

14. **[新しいファイル]** ダイアログで、**[全般]、[空のクラス]** の順に選択し、新しいファイルに **PhoneTranslator** という名前を付けて **[新規]** ボタンをクリックします。

    ![](quickstart-images/xs/add-translator-class.png "新しいクラスの追加")

15. **PhoneTranslator.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードにより、電話の単語が電話番号に変換されます。

    ```csharp
    using System.Text;

    namespace Core
    {
        public static class PhonewordTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw))
                    return null;

                raw = raw.ToUpperInvariant();

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c))
                        newNumber.Append(c);
                    else
                    {
                        var result = TranslateToNumber(c);
                        if (result != null)
                            newNumber.Append(result);
                        // Bad character?
                        else
                            return null;
                    }
                }
                return newNumber.ToString();
            }

            static bool Contains(this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static readonly string[] digits = {
                "ABC", "DEF", "GHI", "JKL", "MNO", "PQRS", "TUV", "WXYZ"
            };

            static int? TranslateToNumber(char c)
            {
                for (int i = 0; i < digits.Length; i++)
                {
                    if (digits[i].Contains(c))
                        return 2 + i;
                }
                return null;
            }
        }
    }
    ```

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**PhoneTranslator.cs** への変更内容を保存してから、ファイルを閉じます。

16. **Solution Pad** で **Phoneword** プロジェクトを選択し、右クリックして、**[追加]、[新しいファイル]** の順に選択します。

    ![](quickstart-images/xs/add-new-interface.png "新しいファイルの追加")

17. **[新しいファイル]** ダイアログで、**[全般]、[Empty Interface]\(空のインターフェイス\)** の順に選択し、新しいファイルに **IDialer** という名前を付けて **[新規]** ボタンをクリックします。

    ![](quickstart-images/xs/add-idialer-interface.png "新しいインターフェイスの追加")

18. **IDialer.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードでは、変換された電話番号にダイヤルするために、各プラットフォームに実装する必要がある `Dial` メソッドを定義します。

    ```csharp
    namespace Phoneword
    {
        public interface IDialer
        {
            bool Dial(string number);
        }
    }
    ```
    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**IDialer.cs** への変更内容を保存してから、ファイルを閉じます。

    > [!NOTE]
    > これでアプリケーション共通のコードが完成しました。 これでプラットフォーム固有の電話ダイヤル コードが [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) として実装されます。

19. **Solution Pad** で **Phoneword.iOS** プロジェクトを選択し、右クリックして、**[追加]、[新しいファイル]** の順に選択します。

    ![](quickstart-images/xs/add-new-file-ios.png "新しいファイルの追加")

20. **[新しいファイル]** ダイアログで、**[全般]、[空のクラス]** の順に選択し、新しいファイルに **PhoneDialer** という名前を付けて **[新規]** ボタンをクリックします。

    ![](quickstart-images/xs/new-phonedialer-ios.png "新しいクラスの追加")

21. **PhoneDialer.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードにより、iOS プラットフォームが、変換された電話番号をダイヤルする `Dial` メソッドが作成されます。

    ```csharp
    using Foundation;
    using Phoneword.iOS;
    using UIKit;
    using Xamarin.Forms;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.iOS
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                return UIApplication.SharedApplication.OpenUrl (
                    new NSUrl ("tel:" + number));
            }
        }
    }
    ```

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**PhoneDialer.cs** への変更内容を保存してから、ファイルを閉じます。

22. **Solution Pad** で **Phoneword.Droid** プロジェクトを選択し、右クリックして、**[追加]、[新しいファイル]** の順に選択します。

    ![](quickstart-images/xs/add-new-file-android.png "新しいファイルの追加")

23. **[新しいファイル]** ダイアログで、**[全般]、[空のクラス]** の順に選択し、新しいファイルに **PhoneDialer** という名前を付けて **[新規]** ボタンをクリックします。

    ![](quickstart-images/xs/new-phonedialer-android.png "新しいクラスの追加")

24. **PhoneDialer.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードにより、Android プラットフォームが、変換された電話番号をダイヤルする `Dial` メソッドが作成されます。

    ```csharp
    using Android.Content;
    using Android.Telephony;
    using Phoneword.Droid;
    using System.Linq;
    using Xamarin.Forms;
    using Uri = Android.Net.Uri;

    [assembly: Dependency(typeof(PhoneDialer))]
    namespace Phoneword.Droid
    {
        public class PhoneDialer : IDialer
        {
            public bool Dial(string number)
            {
                var context = MainActivity.Instance;
                if (context == null)
                    return false;

                var intent = new Intent (Intent.ActionCall);
                intent.SetData (Uri.Parse ("tel:" + number));

                if (IsIntentAvailable (context, intent)) {
                    context.StartActivity (intent);
                    return true;
                }

                return false;
            }

            public static bool IsIntentAvailable(Context context, Intent intent)
            {
                var packageManager = context.PackageManager;

                var list = packageManager.QueryIntentServices (intent, 0)
                    .Union (packageManager.QueryIntentActivities (intent, 0));

                if (list.Any ())
                    return true;

                var manager = TelephonyManager.FromContext (context);
                return manager.PhoneType != PhoneType.None;
            }
        }
    }
    ```

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**PhoneDialer.cs** への変更内容を保存してから、ファイルを閉じます。

25. **Solution Pad** の **Phoneword.Droid** プロジェクトで、**MainActivity.cs** をダブルクリックして開き、テンプレート コードをすべて削除して、次のコードに置き換えます。

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
        {
            internal static MainActivity Instance { get; private set; }

            protected override void OnCreate(Bundle bundle)
            {
                TabLayoutResource = Resource.Layout.Tabbar;
                ToolbarResource = Resource.Layout.Toolbar;

                base.OnCreate(bundle);

                Instance = this;
                global::Xamarin.Forms.Forms.Init(this, bundle);
                LoadApplication(new App());
            }
        }
    }        
    ```

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S キー**を押し)、**MainActivity.cs** の変更内容を保存してから、ファイルを閉じます。

26. **Solution Pad** で **Properties** フォルダーを展開し、**AndroidManifest.xml** ファイルをダブルクリックします。

    ![](quickstart-images/xs/android-manifest.png "Android マニフェストを開く")

27. **[必要なアクセス許可]** セクションで、**CallPhone** 権限を有効にします。 これによって、アプリケーションに電話をかけるアクセス許可が与えられます。

    ![](quickstart-images/xs/android-manifest-changed.png "CallPhone アクセス許可を有効にする")

    **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**AndroidManifest.xml** への変更内容を保存してから、ファイルを閉じます。

28. **Solution Pad** で、**PhonewordPage** クラスを **Phoneword** プロジェクトから削除します。 このページは、プロジェクトの作成時に自動的に追加されたもので、もう不要です。
29. Visual Studio for Mac で、**[ビルド]、[すべてビルド]** の順にメニュー項目を選択します (または **&#8984; + B** キーを押します)。 アプリケーションがビルドされ、Visual Studio for Mac のツール バーに成功のメッセージが表示されます。

    ![](quickstart-images/xs/build-successful.png "ビルドに成功しました")

30. エラーがある場合は、アプリケーションが正常にビルドされるまで、前の手順を実行し、誤りを修正します。
31. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、iOS シミュレーター内にアプリケーションを起動します。

    ![](quickstart-images/xs/start.png "Visual Studio for Ma ツールバー")
    ![](quickstart-images/xs/phoneword-result-ios.png "iOS シミュレーター")

    注: iOS シミュレーターでは電話呼び出しはサポートされていません。

32. **Solution Pad** で **Phoneword.Droid** プロジェクトを選択し、右クリックして、**[スタートアップ プロジェクトに設定]** を選択します。

    ![](quickstart-images/xs/set-startup-project.png "スタートアップ プロジェクトに設定")

33. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、Android エミュレーター内にアプリケーションを起動します。

    ![](quickstart-images/xs/phoneword-result-android.png "Android エミュレーター")

    注: Android シミュレーターでは電話呼び出しはサポートされていません。

-----

Xamarin.Forms アプリケーションの完成おつかれさまでした。 このガイドの「[次のトピック](~/xamarin-forms/get-started/hello-xamarin-forms/deepdive.md)」では、Xamarin.Forms 使用したアプリケーション開発の基本を理解するために、このチュートリアルで実行した手順を再確認します。


## <a name="related-links"></a>関連リンク

- [DependencyService 経由でのネイティブ機能へのアクセス](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
- [Phoneword (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
