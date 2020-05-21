---
title: IOS 拡張機能で Xamarin のフォームページを再利用する
description: このドキュメントでは、iOS の拡張機能について、およびその中で Xamarin のページを使用する方法について説明します。 このチュートリアルでは、拡張機能を作成し、Xamarin を統合して、簡単な ContentPage を iOS 拡張プロジェクトにネイティブでレンダリングする方法について説明します。
ms.prod: xamarin
ms.assetid: 50076FFD-3EF6-41CC-BC7E-210DE3958F5B
ms.technology: xamarin-ios
author: alexeystrakh
ms.author: alstrakh
ms.date: 05/13/2020
ms.openlocfilehash: 4006bb3ef82264b5a7a6d764719bee81a8ce78a1
ms.sourcegitcommit: 5bc37b384706bfbf5bf8e5b1288a34ddd0f3303b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/15/2020
ms.locfileid: "83418074"
---
# <a name="reuse-xamarinforms-pages-in-an-ios-extension"></a>IOS 拡張機能で Xamarin のフォームページを再利用する

iOS 拡張機能を使用すると、カスタムコンテキストアクション、パスワードオートコンプリート、着信呼び出しフィルター、通知コンテンツ修飾子など、 [ios および MacOS 拡張ポイントによって事前定義](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW2)された機能を追加することで、既存のシステム動作をカスタマイズできます。 Xamarin. iOS は拡張機能をサポートします。[このガイド](https://docs.microsoft.com/xamarin/ios/platform/extensions)では、xamarin ツールを使用して ios 拡張機能を作成する手順について説明します。

拡張機能は、コンテナーアプリの一部として配布され、ホストアプリの特定の拡張ポイントからアクティブ化されます。 通常、コンテナーアプリは単純な iOS アプリケーションで、ユーザーに拡張機能に関する情報、アクティブ化、および使用方法を提供します。 拡張機能とコンテナーアプリの間でコードを共有するには、主に次の3つの方法があります。

1. 共通の iOS プロジェクト。

    コンテナーと拡張機能の間のすべての共有コードを共有 iOS ライブラリに配置し、両方のプロジェクトからライブラリを参照できます。 通常、共有ライブラリにはネイティブの UIViewControllers が含まれており、これは Xamarin. iOS ライブラリである必要があります。

1. ファイルリンク。

    場合によっては、拡張機能が単一のをレンダリングする必要がある場合でも、コンテナーアプリにはほとんどの機能が用意されて `UIViewController` います。 共有するファイルが少ない場合は、コンテナーアプリに配置されているファイルから拡張アプリにファイルリンクを追加するのが一般的です。

1. 一般的な Xamarin. Forms プロジェクト。

    アプリページが既に Android などの別のプラットフォームと共有されている場合、Xamarin. Forms framework を使用すると、一般的な方法として、必要なページを拡張プロジェクトにネイティブで再実装します。これは、iOS 拡張機能が Xamarin のページではなくネイティブの UIViewControllers で動作するためです。 IOS 拡張機能で Xamarin. Forms を使用するには、追加の手順を実行する必要があります。詳細については、以下で説明します。

## <a name="xamarinforms-in-an-ios-extension-project"></a>IOS 拡張プロジェクトの Xamarin. フォーム

ネイティブプロジェクトで Xamarin. フォームを使用する機能は、[ネイティブ形式](~/xamarin-forms/platform/native-forms.md)で提供されます。 `ContentPage`ネイティブの Xamarin. iOS プロジェクトには、派生ページを直接追加できます。 `CreateViewController`拡張メソッドは、Xamarin. フォームページのインスタンスをネイティブに変換します。これは、 `UIViewController` 通常のコントローラーとして使用または変更できます。 IOS 拡張機能は、ネイティブ iOS プロジェクトの特殊な種類であるため、ここでも**ネイティブ形式**を使用できます。

> [!IMPORTANT]
> IOS 拡張機能には、多くの[既知の制限](https://docs.microsoft.com/xamarin/ios/platform/extensions#limitations)があります。 IOS 拡張機能で Xamarin. Forms を使用することもできますが、十分に注意して、メモリ使用量と起動時間を監視する必要があります。 それ以外の場合は、この拡張機能を適切に処理する方法を使用せずに iOS によって終了される可能性があります。

## <a name="walkthrough"></a>チュートリアル

このチュートリアルでは、xamarin. フォームアプリケーションと Xamarin の拡張子を作成し、拡張プロジェクトで共有コードを再利用します。

1. Visual Studio for Mac を開いて、**空のフォームアプリ**テンプレートを使用して新しい Xamarin. フォームプロジェクトを作成し、フォームを "フォーム \**拡張機能**" という名前を指定します。

    ![Create Project](extensions-xf-images/1.walkthrough-createproject.png)

1. **フォーム \ extension/mainpage.xaml**で、コンテンツを次のレイアウトに置き換えます。

    ```xaml
    <?xml version="1.0" encoding="utf-8" ?>
    <ContentPage
        x:Class="FormsShareExtension.MainPage"
        xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        xmlns:d="http://xamarin.com/schemas/2014/forms/design"
        xmlns:local="clr-namespace:FormsShareExtension"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        x:DataType="local:MainPageViewModel"
        BackgroundColor="Orange"
        mc:Ignorable="d">
        <ContentPage.BindingContext>
            <local:MainPageViewModel Message="Hello from Xamarin.Forms!" />
        </ContentPage.BindingContext>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <Label
                Margin="20"
                Text="{Binding Message}"
                VerticalOptions="CenterAndExpand" />
            <Button Command="{Binding DoCommand}" Text="Do the job!" />
        </StackLayout>
    </ContentPage>
    ```

1. **Mainpageviewmode**という名前の新しいクラスを**フォーム/拡張**プロジェクトに追加し、クラスの内容を次のコードに置き換えます。

    ```csharp
    using System;
    using System.ComponentModel;
    using System.Windows.Input;
    using Xamarin.Forms;

    namespace FormsShareExtension
    {
        public class MainPageViewModel : INotifyPropertyChanged
        {
            public event PropertyChangedEventHandler PropertyChanged;

            private string _message;
            public string Message
            {
                get { return _message; }
                set
                {
                    if (_message != value)
                    {
                        _message = value;
                        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(Message)));
                    }
                }
            }

            private ICommand _doCommand;
            public ICommand DoCommand
            {
                get { return _doCommand; }
                set
                {
                    if(_doCommand != value)
                    {
                        _doCommand = value;
                        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(DoCommand)));
                    }
                }
            }

            public MainPageViewModel()
            {
                DoCommand = new Command(OnDoCommandExecuted);
            }

            private void OnDoCommandExecuted(object state)
            {
                Message = $"Job {Environment.TickCount} has been completed!";
            }
        }
    }
    ```

    コードはすべてのプラットフォームで共有され、iOS 拡張機能でも使用されます。

1. ソリューションパッドで、ソリューションを右クリックし、[追加] を選択して [**新しいプロジェクト] > iOS > 拡張機能 > アクション拡張を >** し、 **myaction**という名前を指定して、[**作成**] を押します。

    ![拡張機能の作成](extensions-xf-images/2.walkthrough-createextension.png)

1. IOS 拡張機能と共有コードで Xamarin. Forms を使用するには、必要な参照を追加する必要があります。

    - IOS 拡張機能を右クリックし、[参照] を選択して **> プロジェクト > [参照] > 追加**し、 **[OK]** をクリックします。

    - IOS 拡張機能を右クリックし、[パッケージ] を選択して [ **NuGet パッケージの管理**] を > し > [**パッケージの追加**] をクリックします。

1. 拡張機能プロジェクトを展開し、エントリポイントを変更して、Xamarin. Forms を初期化し、ページを作成します。 IOS の要件に従って、拡張機能ではまたはとして、**情報 plist**のエントリポイントを定義する必要があり `NSExtensionMainStoryboard` `NSExtensionPrincipalClass` ます。 エントリポイントがアクティブになると (この例では `ActionViewController.ViewDidLoad` メソッド)、Xamarin. フォームページのインスタンスを作成してユーザーに表示することができます。 そのため、エントリポイントを開き、 `ViewDidLoad` メソッドを次の実装に置き換えます。

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        // Initialize Xamarin.Forms framework
        global::Xamarin.Forms.Forms.Init();
        // Create an instance of XF page with associated View Model
        var xfPage = new MainPage();
        var viewModel = (MainPageViewModel)xfPage.BindingContext;
        viewModel.Message = "Welcome to XF Page created from an iOS Extension";
        // Override the behavior to complete the execution of the Extension when a user press the button
        viewModel.DoCommand = new Command(() => DoneClicked(this));
        // Convert XF page to a native UIViewController which can be consumed by the iOS Extension
        var newController = xfPage.CreateViewController();
        // Present new view controller as a regular view controller
        this.PresentModalViewController(newController, false);
    }
    ```

    は、 `MainPage` 標準コンストラクターを使用してインスタンス化され、拡張機能で使用できるようになる前に、 `UIViewController` 拡張メソッドを使用してネイティブのに変換し `CreateViewController` ます。 
    
    アプリケーションをビルドして実行します。

    ![拡張機能の作成](extensions-xf-images/3.walkthrough-runapp.png)

    拡張機能をアクティブにするには、Safari ブラウザーに移動し、任意の web アドレス (たとえば、 [microsoft.com](https://microsoft.com)) を入力して、[移動] をクリックし、ページの下部にある [**共有**] アイコンをクリックして、使用可能なアクション拡張を表示します。 利用可能な拡張機能の一覧から、 **Myaction**拡張機能をタップして選択します。

    ![拡張機能の作成](extensions-xf-images/4.walkthrough-run1.png) ![拡張機能の作成](extensions-xf-images/5.walkthrough-run2.png) ![拡張機能の作成](extensions-xf-images/6.walkthrough-run3.png)

    拡張機能がアクティブ化され、ユーザーに表示されます。 すべてのバインドとコマンドは、コンテナーアプリと同様に動作します。

1. 元のエントリポイントビューコントローラーは、iOS によって作成およびアクティブ化されるため、表示されます。 この問題を解決するには、 `UIModalPresentationStyle.FullScreen` 呼び出しの直前に次のコードを追加して、新しいコントローラーのモーダルプレゼンテーションスタイルをに変更し `PresentModalViewController` ます。

    ```csharp
    newController.ModalPresentationStyle = UIModalPresentationStyle.FullScreen;
    ```

    IOS シミュレーターまたはデバイスでビルドして実行します。

    ![IOS 拡張機能の Xamarin. Forms](extensions-xf-images/7.walkthrough-result.png)

    > [!IMPORTANT]
    > デバイスビルドの場合は、[こちらの説明](~/iOS/platform/extensions.md#debug-and-release-versions-of-extensions)に従って、適切なビルド設定と**リリース**構成を使用してください。

## <a name="related-links"></a>関連リンク

- [Xamarin.iOS での iOS 拡張機能](~/iOS/platform/extensions.md)
- [Xamarin ネイティブプロジェクトでの xamarin フォーム](~/xamarin-forms/platform/native-forms.md)
- [IOS アプリ拡張機能の効率とパフォーマンスを最適化する](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/ExtensionCreation.html#//apple_ref/doc/uid/TP40014214-CH5-SW7)
- [サンプル ソース コード](https://github.com/xamcat/xamarin-forms-ios-extension)
