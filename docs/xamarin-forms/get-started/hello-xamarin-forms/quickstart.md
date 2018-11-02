---
title: Xamarin.Forms のクイック スタート
description: この記事では、英数字の電話番号 (ユーザーが入力) を数字の電話番号に変換してから、その番号に電話をかけるアプリケーションを作成する方法を説明します。
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 3f2f9c2d-d204-43bc-8c8a-a55ce1e6d2c8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/13/2018
ms.openlocfilehash: fd9b3032166470a960e97f1754d2133a5ef22631
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109368"
---
# <a name="xamarinforms-quickstart"></a>Xamarin.Forms のクイック スタート

このチュートリアルでは、英数字の電話番号 (ユーザーが入力) を数字の電話番号に変換してから、その番号に電話をかけるアプリケーションを作成する方法を説明します。 最終的なアプリケーションは、次のとおりです。

[![](quickstart-images/intro-app-examples-sml.png "Phoneword アプリケーション")](quickstart-images/intro-app-examples.png#lightbox "Phoneword アプリケーション")

::: zone pivot="windows"

## <a name="get-started-with-visual-studio"></a>Visual Studio 入門

1. **[スタート]** 画面で、Visual Studio を起動します。 スタート ページが開きます。

    ![](quickstart-images/vs/start-page.png "Visual Studio")

2. Visual Studio で、**[新しいプロジェクトの作成]** をクリックして新しいプロジェクトを作成します。

    ![](quickstart-images/vs/new-solution.png "新しいプロジェクト")

3. **[新しいプロジェクト]** ダイアログで、**[クロスプラットフォーム]** をクリックして、**[モバイル アプリ (Xamarin.Forms)]** テンプレートを選択し、[名前] を **Phoneword** に設定し、プロジェクトの適切な場所を選んで **[OK]** ボタンをクリックします。

    ![](quickstart-images/vs/new-project.w157.png "クロスプラットフォームのプロジェクト テンプレート")

    > [!NOTE]
    > このクイックスタートの C# および XAML スニペットでは、**Phoneword** という名前のソリューションが必要です。
    > 別のソリューション名を使用すると、これらの手順からプロジェクトにコードをコピーするときに多数のビルド エラーが発生します。

4. **[新しいクロスプラットフォーム アプリ]** ダイアログで、**[空のアプリケーション]** をクリックして、コード共有方法として **[.NET Standard]** を選択し、**[OK]** ボタンをクリックします。

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
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button Text="Translate" Clicked="OnTranslate" />
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
                translatedNumber = PhonewordTranslator.ToNumber (phoneNumberText.Text);
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

9. **ソリューション エクスプローラー**で **[Phoneword]** プロジェクトを右クリックし、**[追加]、[新しい項目]** を選択します。

    ![](quickstart-images/vs/add-new-item.png "新しい項目の追加")

10. **[新しい項目の追加]** ダイアログで、**[Visual C#]、[コード]、[クラス]** の順に選択し、新しいファイルに **PhoneTranslator** という名前を付け、**[追加]** ボタンをクリックします。

    ![](quickstart-images/vs/add-translator-class.w157.png "新しいクラスの追加")

11. **PhoneTranslator.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードにより、電話の単語が電話番号に変換されます。

    ```csharp
    using System.Text;

    namespace Phoneword
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

12. **ソリューション エクスプローラー**で **[Phoneword]** プロジェクトを右クリックし、**[追加]、[新しい項目]** を選択します。

    ![](quickstart-images/vs/add-new-item.png "新しい項目の追加")

13. **[新しい項目の追加]** ダイアログで、**[Visual C#]、[コード]、[インターフェイス]** の順に選択し、新しいファイルに **IDialer** という名前を付け、**[追加]** ボタンをクリックします。

    ![](quickstart-images/vs/add-idialer-interface.w157.png "新しいインターフェイスの追加")

14. **IDialer.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードでは、変換された電話番号にダイヤルするために、各プラットフォームに実装する必要がある `Dial` メソッドを定義します。

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

15. **ソリューション エクスプローラー**で **Phoneword.iOS** プロジェクトを右クリックし、**[追加]、[新しい項目]** を選択します。

    ![](quickstart-images/vs/add-new-item-ios.png "新しい項目の追加")

16. **[新しい項目の追加]** ダイアログで、**[Apple]、[コード]、[クラス]** の順に選択し、新しいファイルに **PhoneDialer** という名前を付け、**[追加]** ボタンをクリックします。

    ![](quickstart-images/vs/new-phone-dialer-ios.w157.png "新しいクラスの追加")

17. **PhoneDialer.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードにより、iOS プラットフォームが、変換された電話番号をダイヤルする <code>Dial</code> メソッドが作成されます。

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

18. **ソリューション エクスプローラー**で **Phoneword.Android** プロジェクトを右クリックし、**[追加]、[新しい項目]** を選択します。

    ![](quickstart-images/vs/add-new-item-android.png "新しい項目の追加")

19. **[新しい項目の追加]** ダイアログで、**[Visual C#]、[Android]、[クラス]** の順に選択し、新しいファイルに **PhoneDialer** という名前を付け、**[追加]** ボタンをクリックします。

    ![](quickstart-images/vs/new-phone-dialer-android.w157.png "新しいクラスの追加")

20. **PhoneDialer.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードにより、Android プラットフォームが、変換された電話番号をダイヤルする `Dial` メソッドが作成されます。

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

                var intent = new Intent (Intent.ActionDial);
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

    このコードでは最新の Android API を使用していることが想定されています。 **CTRL + S** を押し、**PhoneDialer.cs** への変更内容を保存してから、ファイルを閉じます。

21. **ソリューション エクスプローラー**の **Phoneword.Android** プロジェクトで、**MainActivity.cs** をダブルクリックして開き、テンプレート コードをすべて削除して、次のコードに置き換えます。

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true,
                  ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
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

22. **ソリューション エクスプローラー**の **[Phoneword.Android]** プロジェクトで、**[プロパティ]** をダブルクリックし、**[Android マニフェスト]** タブを選択します。

    ![](quickstart-images/vs/android-manifest.png "Android マニフェストを開く")

23. **[必要なアクセス許可]** セクションで、**CALL_PHONE** 権限を有効にします。 これによって、アプリケーションに電話をかけるアクセス許可が与えられます。

    ![](quickstart-images/vs/android-manifest-changed.png "CallPhone アクセス許可を有効にする")

    **CTRL + S** を押し、マニフェストに変更内容を保存してから、ファイルを閉じます。

24. Android アプリケーション プロジェクトを右クリックし、**[スタートアップ プロジェクトに設定]** をクリックします。

25. "緑の矢印" のツールバー ボタンを使用するか、メニューから **[デバッグ]、[デバッグ開始]** の順に選択して、Android アプリを実行します。

    > [!WARNING]
    > 電話呼び出しは機能しないことがあるため、すべてのシミュレーターでサポートされているとは限りません。

26. iOS デバイスがあり、Xamarin.Forms での開発のための Mac システム要件が揃っている場合、同様の手法を使用して、iOS デバイスにアプリを展開します。 または、アプリを [iOS リモート シミュレーター](~/tools/ios-simulator/index.md)に展開します。

::: zone-end
::: zone pivot="macos"

## <a name="get-started-with-visual-studio-for-mac"></a>Visual Studio for Mac の概要

1. Visual Studio for Mac を起動し、スタート ページで **[新しいプロジェクト]** をクリックして新しいプロジェクトを作成します。

    ![](quickstart-images/xs/new-solution.png "新しいソリューション")

2. **[新しいプロジェクト用のテンプレートを選びます]** ダイアログで、**[マルチプラットフォーム]、[アプリ]** の順にクリックし、**[空白フォームのアプリ]** テンプレートを選択して、**[次へ]** ボタンをクリックします。

    ![](quickstart-images/xs/choose-template.png "テンプレートを選択します")

3. **[Configure your Blank Forms app]\(空白フォームのアプリの構成\)** ダイアログで、新しいアプリに **Phoneword** という名前を付け、**[Use .NET Standard]\(.NET Standard を使用する\)** ラジオ ボタンがオンになっていることを確認し、**[次へ]** ボタンをクリックします。

    ![](quickstart-images/xs/configure-app.png "フォーム アプリケーションの構成")

4. **[Configure your new Blank Forms app]\(新しい空白フォームのアプリの構成\)** ダイアログでは、ソリューションとプロジェクトの名前は **Phoneword** に設定したままにし、プロジェクトに適切な場所を選択し、**[作成]** ボタンをクリックしてプロジェクトを作成します。

    ![](quickstart-images/xs/configure-project.png "フォーム プロジェクトの構成")

    > [!NOTE]
    > このクイックスタートの C# および XAML スニペットでは、**Phoneword** という名前のソリューションが必要です。
    > 別のソリューション名を使用すると、これらの手順からプロジェクトにコードをコピーするときに多数のビルド エラーが発生します。

5. **Solution Pad** で、**MainPage.xaml** をダブルクリックして開きます。

    ![](quickstart-images/xs/open-mainpage-xaml.png "MainPage.xaml を開く")

6. **MainPage.xaml** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードでは、ページのユーザー インターフェイスが宣言によって定義されます。

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                       x:Class="Phoneword.MainPage">
        <ContentPage.Padding>
            <OnPlatform x:TypeArguments="Thickness">
                <On Platform="iOS" Value="20, 40, 20, 20" />
                <On Platform="Android, UWP" Value="20" />
            </OnPlatform>
        </ContentPage.Padding>
        <StackLayout>
          <Label Text="Enter a Phoneword:" />
          <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
          <Button Text="Translate" Clicked="OnTranslate" />
          <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
    </ContentPage>
    ```

    **[ファイル]、[保存]** の順に選択し (または **& #8984; + S** キーを押し)、**MainPage.xaml** への変更内容を保存してから、ファイルを閉じます。

7. **Solution Pad** で **MainPage.xaml.cs** をダブルクリックして開きます。

    ![](quickstart-images/xs/open-mainpage-codebehind.png "MainPage.xaml.cs を開く")

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
                translatedNumber = PhonewordTranslator.ToNumber (phoneNumberText.Text);
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

    **[ファイル]、[保存]** の順に選択し (または **& #8984; + S** キーを押し)、**MainPage.xaml.cs** への変更内容を保存してから、ファイルを閉じます。

9. **Solution Pad** で **Phoneword** プロジェクトを選択し、右クリックして、**[追加]、[新しいファイル]** の順に選択します。

    ![](quickstart-images/xs/add-new-translator-file.png "新しいファイルの追加")

10. **[新しいファイル]** ダイアログで、**[全般]、[空のクラス]** の順に選択し、新しいファイルに **PhoneTranslator** という名前を付けて **[新規]** ボタンをクリックします。

    ![](quickstart-images/xs/add-translator-class.png "新しいクラスの追加")

11. **PhoneTranslator.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードにより、電話の単語が電話番号に変換されます。

    ```csharp
    using System.Text;

    namespace Phoneword
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

12. **Solution Pad** で **Phoneword** プロジェクトを選択し、右クリックして、**[追加]、[新しいファイル]** の順に選択します。

    ![](quickstart-images/xs/add-new-interface.png "新しいファイルの追加")

13. **[新しいファイル]** ダイアログで、**[全般]、[Empty Interface]\(空のインターフェイス\)** の順に選択し、新しいファイルに **IDialer** という名前を付けて **[新規]** ボタンをクリックします。

    ![](quickstart-images/xs/add-idialer-interface.png "新しいインターフェイスの追加")

14. **IDialer.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードでは、変換された電話番号にダイヤルするために、各プラットフォームに実装する必要がある `Dial` メソッドを定義します。

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

15. **Solution Pad** で **Phoneword.iOS** プロジェクトを選択し、右クリックして、**[追加]、[新しいファイル]** の順に選択します。

    ![](quickstart-images/xs/add-new-file-ios.png "新しいファイルの追加")

16. **[新しいファイル]** ダイアログで、**[全般]、[空のクラス]** の順に選択し、新しいファイルに **PhoneDialer** という名前を付けて **[新規]** ボタンをクリックします。

    ![](quickstart-images/xs/new-phonedialer-ios.png "新しいクラスの追加")

17. **PhoneDialer.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードにより、iOS プラットフォームが、変換された電話番号をダイヤルする `Dial` メソッドが作成されます。

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

18. **Solution Pad** で **Phoneword.Droid** プロジェクトを選択し、右クリックして、**[追加]、[新しいファイル]** の順に選択します。

    ![](quickstart-images/xs/add-new-file-android.png "新しいファイルの追加")

19. **[新しいファイル]** ダイアログで、**[全般]、[空のクラス]** の順に選択し、新しいファイルに **PhoneDialer** という名前を付けて **[新規]** ボタンをクリックします。

    ![](quickstart-images/xs/new-phonedialer-android.png "新しいクラスの追加")

20. **PhoneDialer.cs** のテンプレート コードをすべて削除し、次のコードに置き換えます。 このコードにより、Android プラットフォームが、変換された電話番号をダイヤルする `Dial` メソッドが作成されます。

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

                var intent = new Intent (Intent.ActionDial);
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

    このコードでは最新の Android API を使用していることが想定されています。 **[ファイル]、[保存]** の順に選択し (または **&#8984; + S** キーを押し)、**PhoneDialer.cs** への変更内容を保存してから、ファイルを閉じます。

21. **Solution Pad** の **Phoneword.Droid** プロジェクトで、**MainActivity.cs** をダブルクリックして開き、テンプレート コードをすべて削除して、次のコードに置き換えます。

    ```csharp
    using Android.App;
    using Android.Content.PM;
    using Android.OS;

    namespace Phoneword.Droid
    {
        [Activity(Label = "Phoneword", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true,
                  ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
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

    **[ファイル]、[保存]** の順に選択し (または **& #8984; + S キー**を押し)、**MainActivity.cs** の変更内容を保存してから、ファイルを閉じます。

22. **Solution Pad** で **Properties** フォルダーを展開し、**AndroidManifest.xml** ファイルをダブルクリックします。

    ![](quickstart-images/xs/android-manifest.png "Android マニフェストを開く")

23. **[必要なアクセス許可]** セクションで、**CallPhone** 権限を有効にします。 これによって、アプリケーションに電話をかけるアクセス許可が与えられます。

    ![](quickstart-images/xs/android-manifest-changed.png "CallPhone アクセス許可を有効にする")

    **[ファイル]、[保存]** の順に選択し (または **& #8984; + S** キーを押し)、**AndroidManifest.xml** への変更内容を保存してから、ファイルを閉じます。

24. Visual Studio for Mac で、**[ビルド]、[すべてビルド]** の順にメニュー項目を選択します (または **&#8984; + B** キーを押します)。 アプリケーションがビルドされ、Visual Studio for Mac のツール バーに成功のメッセージが表示されます。

    ![](quickstart-images/xs/build-successful.png "ビルドに成功しました")

25. エラーがある場合は、アプリケーションが正常にビルドされるまで、前の手順を実行し、誤りを修正します。
26. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、iOS シミュレーター内にアプリケーションを起動します。

    ![](quickstart-images/xs/start.png "Visual Studio for Ma ツールバー")
    ![](quickstart-images/xs/phoneword-result-ios.png "iOS シミュレーター")

    注: iOS シミュレーターでは電話呼び出しはサポートされていません。

27. **Solution Pad** で **Phoneword.Droid** プロジェクトを選択し、右クリックして、**[スタートアップ プロジェクトに設定]** を選択します。

    ![](quickstart-images/xs/set-startup-project.png "スタートアップ プロジェクトに設定")

28. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンのような三角形のボタン) を押し、Android エミュレーター内にアプリケーションを起動します。

    ![](quickstart-images/xs/phoneword-result-android.png "Android エミュレーター")

    > [!WARNING]
    > Android エミュレーターでは電話呼び出しはサポートされていません。

::: zone-end

Xamarin.Forms アプリケーションの完成おつかれさまでした。 このガイドの「[次のトピック](~/xamarin-forms/get-started/hello-xamarin-forms/deepdive.md)」では、Xamarin.Forms 使用したアプリケーション開発の基本を理解するために、このチュートリアルで実行した手順を再確認します。

## <a name="related-links"></a>関連リンク

- [DependencyService 経由でのネイティブ機能へのアクセス](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
- [Phoneword (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/Phoneword/)
