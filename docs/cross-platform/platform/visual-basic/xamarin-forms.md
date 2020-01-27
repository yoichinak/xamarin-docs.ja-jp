---
title: Visual Basic.NET を使用して Xamarin.Forms
description: VB.NET を使用して、メインアセンブリの Visual Basic を使用するように Xamarin プロジェクトテンプレートを変更できます。これにより、を使用してクロスプラットフォームのモバイルアプリを構築できます。
ms.prod: xamarin
ms.assetid: da4b4ba9-9205-47dc-8bae-23272ede2c50
author: davidortinau
ms.author: daortin
ms.date: 04/24/2019
ms.openlocfilehash: 42be47e74b4b0da60d517a17bb6090c58448b718
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76724870"
---
# <a name="xamarinforms-using-visual-basicnet"></a>Visual Basic.NET を使用して Xamarin.Forms

Xamarin では Visual Basic を直接サポートしていません。このページのC#指示に従って Xamarin. Forms ソリューションをC#作成し、.NET Standard プロジェクトを Visual Basic に置き換えます。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/mobile-samples/visualbasic-xamarinformsvb/)

[![Xamarin. Forms ソリューションを作成し、.NET Standard プロジェクトを Visual Basic に置き換えます。](xamarin-forms-images/hero-sml.png)](xamarin-forms-images/hero.png#lightbox)

> [!NOTE]
> Visual Basic でプログラムを作成するには、Windows で Visual Studio を使用する必要があります。

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Visual Basic チュートリアルを使用した Xamarin. フォーム

Visual Basic を使用する単純な Xamarin. Forms プロジェクトを作成するには、次の手順に従います。

1. Visual Studio 2019 から **、[新しいプロジェクトの作成**] を選択します。

2. **[新しいプロジェクトの作成]** ウィンドウで、「 **Xamarin. forms** 」と入力して一覧をフィルター処理し、 **[モバイルアプリ (xamarin)]** を選択して、 **[次へ]** をクリックします。

    [Xamarin. Forms アプリの ![フィルター](xamarin-forms-images/02-sml.png)](xamarin-forms-images/02.png#lightbox)

3. 次の画面で、プロジェクトの名前を入力し、 **[作成]** をクリックします。

4. **空**のテンプレートを選択し、 **[OK]** をクリックします。

    [空の Xamarin. Forms テンプレートを ![](xamarin-forms-images/04-sml.png)](xamarin-forms-images/04.png#lightbox)

    これにより、を使用してC#Visual Studio で Xamarin. Forms ソリューションが作成されます。 次の手順では、Visual Basic を使用するようにソリューションを変更します。

5. ソリューションを右クリックし、 **[新しいプロジェクトの追加 >]** を選択します。

6. **Visual Basic ライブラリ**を入力してプロジェクトオプションをフィルター処理し、 **[クラスライブラリ (.NET Standard)]** オプションを Visual Basic アイコンで選択します。

    [Visual Basic ライブラリの ![フィルター](xamarin-forms-images/06-sml.png)](xamarin-forms-images/06.png#lightbox)

7. 次の画面で、プロジェクトの名前を入力し、 **[作成]** をクリックします。

8. Visual Basic プロジェクトを右クリックし、選択**プロパティ**、変更、**既定の名前空間**に合わせて既存の C# プロジェクトします。

    [Visual Basic ルート名前空間が Xamarin. Forms アプリと一致することを ![](xamarin-forms-images/07a-sml.png)](xamarin-forms-images/07a.png#lightbox)

9. 新しい Visual Basic プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。次に、 **Xamarin**をインストールし、[パッケージマネージャー] ウィンドウを閉じます。

    [フォームを ![し、[パッケージマネージャー] ウィンドウを閉じる](xamarin-forms-images/07b-sml.png)](xamarin-forms-images/07b.png#lightbox)

10. 既定の**Class1**ファイルの名前を**app.xaml**に変更します。

    [既定の Class1 ファイルとクラスの名前を App に変更 ![には](xamarin-forms-images/08.png)](xamarin-forms-images/08.png#lightbox)

11. 次のコードを**app.xaml**ファイルに貼り付けます。これは、Xamarin. Forms アプリの開始点となります。

    ```vb
    Imports Xamarin.Forms

    Public Class App
        Inherits Application

        Public Sub New()
            Dim label = New Label With {.HorizontalTextAlignment = TextAlignment.Center,
                                        .FontSize = Device.GetNamedSize(NamedSize.Medium, GetType(Label)),
                                        .Text = "Welcome to Xamarin.Forms with Visual Basic.NET"}

            Dim stack = New StackLayout With {
                .VerticalOptions = LayoutOptions.Center
            }
            stack.Children.Add(label)

            Dim page = New ContentPage
            page.Content = stack
            MainPage = page

        End Sub

    End Class
    ```

12. Android および iOS プロジェクトを更新して、新しい Visual Basic プロジェクト (テンプレートによってC#作成されたプロジェクトではなく) を参照するようにします。
Android および iOS プロジェクトの **[参照設定]** ノードを右クリックして、**参照マネージャー**を開きます。 C#ライブラリを非表示にして、Visual Basic ライブラリをマークします (Android と iOS の両方のプロジェクトについて、忘れずに実行してください)。

    [古いプロジェクト参照を削除 ![には Visual Basic 参照を追加してください](xamarin-forms-images/10-sml.png)](xamarin-forms-images/10.png#lightbox)

13. プロジェクトをC#削除します。 新しい **.vb**ファイルを追加して、Xamarin. Forms アプリケーションをビルドします。 Visual Basic の新しい `ContentPage`s のテンプレートを次に示します。

    ```vb
    Imports Xamarin.Forms

    Public Class Page2
    Inherits ContentPage

        Public Sub New()
            Dim label = New Label With {.HorizontalTextAlignment = TextAlignment.Center,
                                        .FontSize = Device.GetNamedSize(NamedSize.Medium, GetType(Label)),
                                        .Text = "Visual Basic ContentPage"}

            Dim stack = New StackLayout With {
                .VerticalOptions = LayoutOptions.Center
            }
            stack.Children.Add(label)

            Content = stack
        End Sub
    End Class
    ```

## <a name="limitations-of-visual-basic-in-xamarinforms"></a>Xamarin. Forms での Visual Basic の制限事項

[ポータブル Visual Basic.NET ページ](~/cross-platform/platform/visual-basic/index.md)に記載されているように、Xamarin は Visual Basic 言語をサポートしていません。 これは、Visual Basic を使用できる場所にいくつかの制限があることを意味します。

- XAML ページは、Visual Basic プロジェクトに含めることができません - C# 分離コード ジェネレーターの作成できるのみです。 独立した、参照、C# ポータブル クラス ライブラリに XAML を含めるし、データ バインドを使用して Visual Basic のモデルを使用して、XAML ファイルを作成することは (この例が記載されて、[サンプル](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB))。

- カスタムレンダラーを Visual Basic に記述することはできません。 C#これらはネイティブプラットフォームプロジェクトで記述する必要があります。

- 依存関係サービスの実装は Visual Basic で書き込むことができませんC# 。これらはネイティブプラットフォームプロジェクトで記述する必要があります。

## <a name="related-links"></a>関連リンク

- [XamarinFormsVB (sample)](https://docs.microsoft.com/samples/xamarin/mobile-samples/visualbasic-xamarinformsvb/)
- [.NET Framework を使用したクロスプラットフォーム開発](https://docs.microsoft.com/dotnet/standard/cross-platform/)
