---
title: Visual Basic.NET を使用した Xamarin. フォーム
description: VB.NET を使用して、メインアセンブリの Visual Basic を使用するように Xamarin プロジェクトテンプレートを変更できます。これにより、を使用してクロスプラットフォームのモバイルアプリを構築できます。
ms.prod: xamarin
ms.assetid: da4b4ba9-9205-47dc-8bae-23272ede2c50
author: davidortinau
ms.author: daortin
ms.date: 04/24/2019
ms.openlocfilehash: 8cb26c3d8cec03cfaa12f7acb974c9944119bdfc
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374109"
---
# <a name="xamarinforms-using-visual-basicnet"></a>Visual Basic.NET を使用した Xamarin. フォーム

Xamarin では Visual Basic を直接サポートしていません。このページの指示に従って C# Xamarin. Forms ソリューションを作成し、.NET Standard C# プロジェクトを Visual Basic に置き換えます。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/mobile-samples/visualbasic-xamarinformsvb/)

[![Xamarin. Forms ソリューションを作成し、.NET Standard プロジェクトをに置き換え Visual Basic](xamarin-forms-images/hero-sml.png)](xamarin-forms-images/hero.png#lightbox)

> [!NOTE]
> Visual Basic でプログラムを作成するには、Windows で Visual Studio を使用する必要があります。

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Visual Basic チュートリアルを使用した Xamarin. フォーム

Visual Basic を使用する単純な Xamarin. Forms プロジェクトを作成するには、次の手順に従います。

1. Visual Studio 2019 から **、[新しいプロジェクトの作成** ] を選択します。

2. [ **新しいプロジェクトの作成** ] ウィンドウで、「 **Xamarin. forms** 」と入力して一覧をフィルター処理し、[ **モバイルアプリ (xamarin)** ] を選択して、[ **次へ** ] をクリックします。

    [![Xamarin. Forms アプリのフィルター](xamarin-forms-images/02-sml.png)](xamarin-forms-images/02.png#lightbox)

3. 次の画面で、プロジェクトの名前を入力し、[ **作成** ] をクリックします。

4. **空** のテンプレートを選択し、 **[OK]** をクリックします。

    [![空の Xamarin. Forms テンプレート](xamarin-forms-images/04-sml.png)](xamarin-forms-images/04.png#lightbox)

    これにより、Visual Studio で C# を使用して Xamarin. Forms ソリューションが作成されます。 次の手順では、Visual Basic を使用するようにソリューションを変更します。

5. ソリューションを右クリックし、[ **新しいプロジェクトの追加 >** ] を選択します。

6. **Visual Basic ライブラリ** を入力してプロジェクトオプションをフィルター処理し、[ **クラスライブラリ (.NET Standard)** ] オプションを Visual Basic アイコンで選択します。

    [![Visual Basic ライブラリのフィルター](xamarin-forms-images/06-sml.png)](xamarin-forms-images/06.png#lightbox)

7. 次の画面で、プロジェクトの名前を入力し、[ **作成** ] をクリックします。

8. Visual Basic プロジェクトを右クリックし、[ **プロパティ** ] を選択します。次に、既存の C# プロジェクトに合わせて **既定の名前空間** を変更します。

    [![Visual Basic ルート名前空間が Xamarin. Forms アプリと一致することを確認する](xamarin-forms-images/07a-sml.png)](xamarin-forms-images/07a.png#lightbox)

9. 新しい Visual Basic プロジェクトを右クリックし、[ **NuGet パッケージの管理** ] を選択します。次に、 **Xamarin** をインストールし、[パッケージマネージャー] ウィンドウを閉じます。

    [![パッケージマネージャーウィンドウをフォームに表示して閉じる](xamarin-forms-images/07b-sml.png)](xamarin-forms-images/07b.png#lightbox)

10. 既定の **Class1** ファイルの名前を **app.xaml** に変更します。

    [![既定の Class1 ファイルとクラスの名前を App に変更する](xamarin-forms-images/08.png)](xamarin-forms-images/08.png#lightbox)

11. 次のコードを **app.xaml** ファイルに貼り付けます。これは、Xamarin. Forms アプリの開始点となります。

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

12. Android および iOS プロジェクトを更新して、新しい Visual Basic プロジェクト (テンプレートによって作成された C# プロジェクトではない) を参照するようにします。
Android および iOS プロジェクトの [ **参照設定** ] ノードを右クリックして、 **参照マネージャー** を開きます。 C# ライブラリのティックを解除し、Visual Basic ライブラリをマークします (Android と iOS の両方のプロジェクトについては忘れないでください)。

    [![古いプロジェクト参照の削除、Visual Basic 参照の追加](xamarin-forms-images/10-sml.png)](xamarin-forms-images/10.png#lightbox)

13. C# プロジェクトを削除します。 新しい **.vb** ファイルを追加して、Xamarin. Forms アプリケーションをビルドします。 Visual Basic の新しい s のテンプレートを `ContentPage` 次に示します。

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

- XAML ページを Visual Basic プロジェクトに含めることはできません-分離コードジェネレーターは C# のみをビルドできます。 XAML を別の参照された C# ポータブルクラスライブラリに含め、データバインディングを使用して Visual Basic モデル ( [サンプル](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)に含まれています) を使用して xaml ファイルを設定することができます。

- カスタムレンダラーを Visual Basic に記述することはできません。これらはネイティブプラットフォームプロジェクトの C# で記述する必要があります。

- 依存関係サービスの実装を Visual Basic に記述することはできません。これらはネイティブプラットフォームプロジェクトの C# で記述する必要があります。

## <a name="related-links"></a>関連リンク

- [XamarinFormsVB (サンプル)](/samples/xamarin/mobile-samples/visualbasic-xamarinformsvb/)
- [.NET Framework を使用したプラットフォーム間の開発](/dotnet/standard/cross-platform/)