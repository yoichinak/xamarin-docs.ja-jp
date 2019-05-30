---
title: 最初の Xamarin.Forms アプリのビルド
description: Visual Studio で最初の Xamarin.Forms アプリケーションをビルドする方法を示すビデオ ガイド。
zone_pivot_groups: platform-dev16
ms.prod: xamarin
ms.assetid: 72B6AF82-4D98-47E5-AB54-0A35B3253468
ms.technology: xamarin-forms
ms.custom: video
author: conceptdev
ms.author: crdun
ms.date: 05/23/2019
ms.openlocfilehash: 3b25be2c929e0f072af2ff563d49ceaf07cae058
ms.sourcegitcommit: 10b4ccbfcf182be940899c00fc0fecae1e199c5b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2019
ms.locfileid: "66252340"
---
# <a name="build-your-first-xamarinforms-app"></a>最初の Xamarin.Forms アプリのビルド

_このビデオを視聴し、作業を進めて、Xamarin.Forms による最初のモバイル アプリを作成します。_

::: zone pivot="windows"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Build-Your-First-Android-App-with-Visual-Studio-2019-and-Xamarin/player]

## <a name="step-by-step-instructions-for-windows"></a>Windows での手順の詳細

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/FirstApp/)

上記のビデオと共に以下の手順に従います。

1. 選択**ファイル > 新規 > プロジェクト.** かキーを押して、**新しいプロジェクトの作成.** ボタンをクリックします。

    [![新しいプロジェクトを作成する](images/win-2019/01-sml.png)](images/win-2019/01.png#lightbox)

2. "Xamarin"を検索または選択**Mobile**から、**プロジェクトの種類**メニュー。 選択、**モバイル アプリ (Xamarin.Forms)** プロジェクトの種類。

    [![Xamarin プロジェクト用のフィルター](images/win-2019/02-sml.png)](images/win-2019/02.png#lightbox)

3. プロジェクト名を選択&ndash;"AwesomeApp"を使用する例を示します。

    [![プロジェクト名を選択します。](images/win-2019/03-sml.png)](images/win-2019/03.png#lightbox)

4. をクリックして、**空白**プロジェクトの種類を確認して**Android**と**iOS**が選択されています。

    [![Android および iOS と .NET Standard](images/win-2019/04-sml.png)](images/win-2019/04.png#lightbox)

5. NuGet パッケージが復元される (ステータス バーに "復元が完了しました" メッセージが表示される) まで待ちます。

6. デバッグ ボタン (または **[デバッグ]、[デバッグ開始]** メニュー項目) をクリックして、Android エミュレーターを起動します。

7. **MainPage.xaml** を編集し、`</StackLayout>` の末尾の前にこの XAML を追加します。

    ```xaml
    <Button Text="Click Me" Clicked="Button_Clicked" />
    ```

8. **MainPage.xaml.cs** を編集し、クラスの末尾にこのコードを追加します。

    ```csharp
    int count = 0;
    void Button_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

9. Android 上のアプリのデバッグ:

    ![Android アプリ](images/win/07-sml.png)

    > [!TIP]
    > ネットワーク接続された Mac コンピューターで Visual Studio から iOS アプリをビルドし、デバッグすることができます。 詳細については、[セットアップ手順](~/ios/get-started/installation/windows/index.md)を参照してください。

## <a name="build-an-ios-app-in-visual-studio-2019"></a>Visual Studio 2019 で iOS アプリをビルドします。

このビデオでは、構築および Windows で Visual Studio 2019 を使用して iOS アプリのテストのプロセスについて説明します。

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Build-Your-First-iOS-App-with-Visual-Studio-2019-and-Xamarin/player]

::: zone-end
::: zone pivot="win-vs2017"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-Android--iOS-App-in-Visual-Studio-2017/player]

## <a name="step-by-step-instructions-for-windows"></a>Windows での手順の詳細

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/FirstApp/)

上記のビデオと共に以下の手順に従います。

1. **[ファイル]、[新規]、[プロジェクト]** の順に選択するか、**[新しいプロジェクトの作成]** ボタンをクリックして、**[Visual C#]、[クロスプラットフォーム]、[モバイル アプリ (Xamarin.Forms)]** の順にクリックします。

    [![モバイル アプリ (Xamarin.Forms)](images/win/01-sml.png)](images/win/01.png#lightbox)

2. **.NET Standard** コード共有と共に、**[Android]** と **[iOS]** が確実に選択されているようにします。

    [![Android および iOS と .NET Standard](images/win/02-sml.png)](images/win/02.png#lightbox)

3. NuGet パッケージが復元される (ステータス バーに "復元が完了しました" メッセージが表示される) まで待ちます。

4. デバッグ ボタン (または **[デバッグ]、[デバッグ開始]** メニュー項目) をクリックして、Android エミュレーターを起動します。

5. **MainPage.xaml** を編集し、`</StackLayout>` の末尾の前にこの XAML を追加します。

    ```xaml
    <Button Text="Click Me" Clicked="Button_Clicked" />
    ```

6. **MainPage.xaml.cs** を編集し、クラスの末尾にこのコードを追加します。

    ```csharp
    int count = 0;
    void Button_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

7. Android 上のアプリのデバッグ:

    ![Android アプリ](images/win/07-sml.png)

    > [!TIP]
    > ネットワーク接続された Mac コンピューターで Visual Studio から iOS アプリをビルドし、デバッグすることができます。 詳細については、[セットアップ手順](~/ios/get-started/installation/windows/index.md)を参照してください。

::: zone-end
::: zone pivot="macos"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-iOS--Android-App-in-Visual-Studio-for-Mac/player]

## <a name="step-by-step-instructions-for-mac"></a>Mac での手順の詳細

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/FirstApp/)

上記のビデオと共に以下の手順に従います。

1. **[ファイル]、[新しいソリューション]** の順に選択するか、**[新しいプロジェクト]** ボタンをクリックし、**[マルチプラットフォーム]、[アプリ]、[空白フォームのアプリ]** の順に選択します。

    [![空白フォームのアプリ](images/01-sml.png)](images/01.png#lightbox)

2. **.NET Standard** コード共有と共に、**[Android]** と **[iOS]** が確実に選択されているようにします。

    [![Android および iOS と .NET Standard](images/02-sml.png)](images/02.png#lightbox)

3. ソリューションを右クリックして、NuGet パッケージを復元します。

    ![Android アプリ](images/03-sml.png)

4. デバッグ ボタン (または **[実行]、[デバッグ開始]**) をクリックして、Android エミュレーターを起動します。

5. **MainPage.xaml** を編集し、`</StackLayout>` の末尾の前にこの XAML を追加します。

    ```xaml
    <Button Text="Click Me" Clicked="Handle_Clicked" />
    ```

6. **MainPage.xaml.cs** を編集し、クラスの末尾にこのコードを追加します。

    ```csharp
    int count = 0;
    void Handle_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

7. Android 上のアプリのデバッグ:

    ![Android アプリ](images/07-sml.png)

8. 右クリックして、iOS を**スタートアップ プロジェクト**に設定します。

    [![スタートアップ プロジェクトを iOS に設定します](images/08-sml.png)](images/08.png#lightbox)

9. iOS 上のアプリのデバッグ:

    ![iOS アプリ](images/09-sml.png)

::: zone-end

[サンプル ギャラリー](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/FirstApp/)から完成したコードをダウンロードしたり、[GitHub](https://github.com/xamarin/xamarin-forms-samples/tree/master/GetStarted/FirstApp) でそのコードを表示したりすることができます。

## <a name="next-steps"></a>次の手順

- [1 つのページのクイック スタート](~/get-started/quickstarts/single-page.md)&ndash;より機能的なアプリをビルドします。
- [Xamarin.Forms のサンプル](~/xamarin-forms/samples/index.yml) &ndash; コード例とサンプル アプリをダウンロードして実行する。
- [Mobile Apps の電子ブックの作成](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) &ndash; PDF で提供され、数百の追加のサンプルを含む Xamarin.Forms 開発について解説する詳細な章。
