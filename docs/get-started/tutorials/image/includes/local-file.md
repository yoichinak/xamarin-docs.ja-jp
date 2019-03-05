イメージ ファイルをプラットフォーム プロジェクトに追加したり、Xamarin.Forms 共有コードから参照したりすることができます。 このイメージ配布方法は、イメージがプラットフォーム固有の場合に必要になります。例えば、異なるプラットフォームで異なる解像度を使用する場合や、わずかに異なるデザインを使用する場合などです。

この演習では、URI からダウンロードしたイメージではなく、ローカル イメージを表示するように **ImageTutorial** ソリューションを変更します。 ローカル イメージは Xamarin のロゴです。下のボタンをクリックしてダウンロードする必要があります。

> [!div class="nextstepaction"]
> [XamarinLogo.png のダウンロード](https://raw.githubusercontent.com/xamarin/xamarin-forms-samples/master/UserInterface/PlatformSpecifics/Droid/Resources/drawable/XamarinLogo.png)

> [!IMPORTANT]
> すべてのプラットフォームで単一のイメージを使用するには、*すべてのプラットフォームで同じファイル名を使用する必要があります*。また、ファイル名は有効な Android リソース名である必要があります (つまり、小文字、数字、アンダースコア、およびピリオドのみが許可されます)。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. **ソリューション エクスプローラー**の  **ImageTutorial.iOS** プロジェクトで、**[資産カタログ]** を展開し、**[資産]** をダブルクリックして開きます。 次に、**[Assets.xcassets]** タブで **[プラス]** ボタンをクリックし、**[イメージ セットの追加]** を選択します。

    ![Visual Studio で資産カタログに新しいイメージ セットを作成するスクリーンショット](../images/vs/new-image-set.png "新しい資産カタログのイメージ セット")

1. **[Assets.xcassets]** タブで、新しいイメージ セットを選択するとエディターが表示されます。

    ![Visual Studio での資産カタログ内の新しいイメージ セットのスクリーンショット](../images/vs/new-image-set-editor.png "資産カタログのイメージ セット エディター")

1. **XamarinLogo.png** をファイル システムから **[ユニバーサル]** カテゴリの **[1x]** ボックスにドラッグします。

    ![Visual Studio でのイメージを含むイメージ セットのスクリーン ショット](../images/vs/image-set-with-image.png "イメージを含むイメージ セット")

1. **[Assets.xcassets]** タブで、新しいイメージ セットの名前を右クリックし、名前を **XamarinLogo** に変更します。

    ![Visual Studio での名前が変更されたイメージ セットのスクリーンショット](../images/vs/rename-image-set.png "名前が変更されたイメージ セット")

    **[Assets.xcassets]** タブを保存して閉じます。

1. **ソリューション エクスプローラー**の **ImageTutorial.Android** プロジェクトで、**[リソース]** フォルダーを展開します。 次に、**XamarinLogo.png** をファイル システムから、 **ドローアブル** フォルダーにドラッグします。

    ![Visual Studio での Android リソースとしてのイメージ ファイルのスクリーンショット](../images/vs/android-resource.png "Android リソース フォルダー内のローカル イメージ ファイル")

    > [!NOTE]
    > Visual Studio はイメージのビルド アクションを自動的に **AndroidResource** に設定します。

1. **ImageTutorial** プロジェクトの **MainPage.xaml** で、ローカルの **XamarinLogo** ファイルを表示するように [`Image`](xref:Xamarin.Forms.Editor) 宣言を変更します。

    ```xaml
    <Image Source="XamarinLogo"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    このコードは、[`Source`](xref:Xamarin.Forms.Image.Source) プロパティを、表示するローカル ファイルに設定します。 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) プロパティは、iOS では 300 デバイス非依存単位、Android では 250 デバイス非依存単位に設定されます。 また、[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティは、イメージが水平方向の中央に配置されるように指定します。

    > [!NOTE]
    > iOS 上の PNG イメージの場合、[`Source`](xref:Xamarin.Forms.Image.Source) プロパティで指定するファイル名から **.png** 拡張子を省略できます。 その他のイメージ形式の場合は、拡張子が必要です。

1. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android でのローカル イメージを表示するイメージ ビューのスクリーンショット](../images/local-file.png "ローカル イメージを表示するイメージ ビュー")](../images/local-file-large.png#lightbox "ローカル イメージを表示するイメージ ビュー")

    ローカル イメージの詳細については、「[Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md)」ガイドの「[Local images](~/xamarin-forms/user-interface/images.md#local-images)」(ローカル イメージ) を参照してください。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. **Solution Pad** の**ImageTutorial.iOS** プロジェクトで、**[Assets.xcassets]** をダブルクリックして開きます。 次に、**[資産リスト]** で右クリックして **[新しいイメージ セット]** を選択します。

    ![Visual Studio for Mac で資産カタログに新しいイメージ セットを作成するスクリーンショット](../images/vsmac/new-image-set.png "新しい資産カタログのイメージ セット")

1. **[資産リスト]** で、新しいイメージ セットを選択するとエディターが表示されます。

    ![Visual Studio for Mac での資産カタログ内の新しいイメージ セットのスクリーンショット](../images/vsmac/new-image-set-editor.png "資産カタログのイメージ セット エディター")

1. **XamarinLogo.png** をファイル システムから **[ユニバーサル]** カテゴリの **[1x]** ボックスにドラッグします。

    ![Visual Studio for Mac でのイメージを含むイメージ セットのスクリーン ショット](../images/vsmac/image-set-with-image.png "イメージを含むイメージ セット")

1. **[資産リスト]** で、新しいイメージ セットの名前をダブルクリックし、名前を **XamarinLogo** に変更します。

    ![Visual Studio for Mac での名前が変更されたイメージ セットのスクリーンショット](../images/vsmac/rename-image-set.png "名前が変更されたイメージ セット")

1. **Solution Pad** の **ImageTutorial.Android** プロジェクトで、**[リソース]** フォルダーを展開します。 次に、**XamarinLogo.png** をファイル システムから、 **ドローアブル** フォルダーにドラッグします。

1. **[ファイルをフォルダーに追加する]** ダイアログで **[OK]** を選択します。

    ![Visual Studio for Mac での Android リソースとしてのイメージ ファイルのスクリーンショット](../images/vsmac/android-resource.png "Android リソース フォルダー内のローカル イメージ ファイル")

    > [!NOTE]
    > Visual Studio for Mac はイメージのビルド アクションを自動的に **AndroidResource** に設定します。

1. **ImageTutorial** プロジェクトの **MainPage.xaml** で、ローカルの **XamarinLogo** ファイルを表示するように [`Image`](xref:Xamarin.Forms.Editor) 宣言を変更します。

    ```xaml
    <Image Source="XamarinLogo"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    このコードは、[`Source`](xref:Xamarin.Forms.Image.Source) プロパティを、表示するローカル ファイルに設定します。 [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) プロパティは、iOS では 300 デバイス非依存単位、Android では 250 デバイス非依存単位に設定されます。 また、[`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) プロパティは、イメージが水平方向の中央に配置されるように指定します。

    > [!NOTE]
    > iOS 上の PNG イメージの場合、[`Source`](xref:Xamarin.Forms.Image.Source) プロパティで指定するファイル名から **.png** 拡張子を省略できます。 その他のイメージ形式の場合は、拡張子が必要です。

1. Visual Studio for Mac ツール バーで、**[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。

    [![iOS および Android でのローカル イメージを表示するイメージ ビューのスクリーンショット](../images/local-file.png "ローカル イメージを表示するイメージ ビュー")](../images/local-file-large.png#lightbox "ローカル イメージを表示するイメージ ビュー")

    ローカル イメージの詳細については、「[Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md)」ガイドの「[Local images](~/xamarin-forms/user-interface/images.md#local-images)」(ローカル イメージ) を参照してください。
