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
ms.date: 04/02/2019
ms.openlocfilehash: 0031cb7fb46cf5ad35872963fd3c3def0a2ae9a6
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58855303"
---
# <a name="build-your-first-xamarinforms-app"></a>最初の Xamarin.Forms アプリのビルド

_このビデオを視聴し、Xamarin.Forms で初めてのモバイル アプリを作成する作業を進めるにします。_

::: zone pivot="windows"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-Android--iOS-App-in-Visual-Studio-2017/player]

## <a name="step-by-step-instructions-for-windows"></a>Windows での手順の詳細

[![Download サンプル](~/media/shared/download.png) サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/FirstApp/)

上記のビデオと共に以下の手順に従います。

1. 選択**ファイル > 新規 > プロジェクト.** かキーを押して、**新しいプロジェクトの作成.** ボタンをクリックします。

    [![C新しいプロジェクトを作成する](images/win-2019/01-sml.png)](images/win-2019/01.png#lightbox)

2. "Xamarin"を検索または選択**Mobile**から、**プロジェクトの種類**メニュー。 選択、**モバイル アプリ (Xamarin.Forms)** プロジェクトの種類。

    [![FXamarin プロジェクトの ilter](images/win-2019/02-sml.png)](images/win-2019/02.png#lightbox)

3. プロジェクト名を選択&ndash;"AwesomeApp"を使用する例を示します。

    [![Cプロジェクト名の選択](images/win-2019/03-sml.png)](images/win-2019/03.png#lightbox)

4. をクリックして、**黒**プロジェクトの種類を確認して**Android**と**iOS**が選択されています。

    [![Android と .NET Standard での iOS の場合](images/win-2019/04-sml.png)](images/win-2019/04.png#lightbox)

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

::: zone-end
::: zone pivot="win-vs2017"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-Android--iOS-App-in-Visual-Studio-2017/player]

## <a name="step-by-step-instructions-for-windows"></a>Windows での手順の詳細

[![Download サンプル](~/media/shared/download.png) サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/FirstApp/)

上記のビデオと共に以下の手順に従います。

1. **[ファイル]、[新規]、[プロジェクト]** の順に選択するか、**[新しいプロジェクトの作成]** ボタンをクリックして、**[Visual C#]、[クロスプラットフォーム]、[モバイル アプリ (Xamarin.Forms)]** の順にクリックします。

    [![Mobile アプリ (Xamarin.Forms)](images/win/01-sml.png)](images/win/01.png#lightbox)

2. **.NET Standard** コード共有と共に、**[Android]** と **[iOS]** が確実に選択されているようにします。

    [![Android と .NET Standard での iOS の場合](images/win/02-sml.png)](images/win/02.png#lightbox)

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

[![Download サンプル](~/media/shared/download.png) サンプルのダウンロード](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/FirstApp/)

上記のビデオと共に以下の手順に従います。

1. **[ファイル]、[新しいソリューション]** の順に選択するか、**[新しいプロジェクト]** ボタンをクリックし、**[マルチプラットフォーム]、[アプリ]、[空白フォームのアプリ]** の順に選択します。

    [![Blank フォーム アプリの場合](images/01-sml.png)](images/01.png#lightbox)

2. **.NET Standard** コード共有と共に、**[Android]** と **[iOS]** が確実に選択されているようにします。

    [![Android と .NET Standard での iOS の場合](images/02-sml.png)](images/02.png#lightbox)

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

    [![Set iOS をスタートアップ プロジェクト](images/08-sml.png)](images/08.png#lightbox)

9. iOS 上のアプリのデバッグ:

    ![iOS アプリ](images/09-sml.png)

::: zone-end

[サンプル ギャラリー](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/FirstApp/)から完成したコードをダウンロードしたり、[GitHub](https://github.com/xamarin/xamarin-forms-samples/tree/master/GetStarted/FirstApp) でそのコードを表示したりすることができます。

## <a name="next-steps"></a>次の手順

- [1 つのページのクイック スタート](~/get-started/quickstarts/single-page.md)&ndash;より機能的なアプリをビルドします。
- [Xamarin.Forms のサンプル](~/xamarin-forms/samples/index.yml) &ndash; コード例とサンプル アプリをダウンロードして実行する。
- [Mobile Apps の電子ブックの作成](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) &ndash; PDF で提供され、数百の追加のサンプルを含む Xamarin.Forms 開発について解説する詳細な章。
