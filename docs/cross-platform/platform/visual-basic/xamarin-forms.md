---
title: "Xamarin.Forms を使用して Visual Basic.NET"
description: "Xamarin.Forms PCL プロジェクト テンプレートは、実質的に VB.NET を使ったクロスプラット フォーム モバイル アプリをビルドすることができます、メイン アセンブリの Visual Basic を使用して変更できます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: f264c632-8feb-4015-a5e5-cb9c681c787d
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 332882bcef9563ef060c5151c2997ac3b4c8497c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinforms-using-visual-basicnet"></a>Xamarin.Forms を使用して Visual Basic.NET

Xamarin に Visual Basic が直接サポートされていません - c# Xamarin.Forms PCL ソリューションを作成し、Visual Basic を使用して、一般的なコードの PCL プロジェクトを置換するには、このページに、指示に従います。

[ ![](xamarin-forms-images/hero-sml.png "Xamarin.Forms PCL ソリューションを作成し、Visual Basic を使用して、一般的なコードの PCL プロジェクトを置き換えます")](xamarin-forms-images/hero.png)

> ℹ️ **注:** Visual Studio は、Windows で Visual Basic を使用してプログラムを使用する必要があります。

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Visual Basic チュートリアルと Xamarin.Forms

Visual Basic を使用する単純な Xamarin.Forms プロジェクトを作成する手順に従います。

1. 新しい*Xamarin.Forms c#*ポータブル クラス ライブラリ (PCL) を使用するソリューションです。
移動して**ファイル > 新しいプロジェクト**し、、**新しいプロジェクト**にウィンドウが移動**インストールされている > テンプレート > Visual c# > Cross Platform**順に選択**クロス プラットフォーム アプリ (Xamarin.Forms またはネイティブ) > Xamarin.Forms**です。

2. ソリューションを右クリックし、**追加 > 新しいプロジェクト**です。

3. 選択、 **Visual Basic > クラス ライブラリ (ポータブル)**プロジェクトの種類。

   [ ![](xamarin-forms-images/add-vb-2-sml.png "新しいのポータブル クラス ライブラリ プロジェクトを追加します。")](xamarin-forms-images/add-vb-2.png)

4. ように、適切な PCL プロファイルを構成するのには、プラットフォームを選択します (を必ず含めて、Xamarin.iOS および Xamarin.Android)。

   ![](xamarin-forms-images/add-vb-3-sml.png "サポートするプラットフォームを選択します。")

5. Visual Basic プロジェクトを右クリックし、選択**プロパティ**、し、変更、**既定の名前空間**に合わせて既存の c# プロジェクトします。

   ![](xamarin-forms-images/add-vb-4s-sml.png "一致する Xamarin.Forms アプリを Visual Basic のルート名前空間を確認してください。")

6. 新しい Visual Basic プロジェクトを右クリックして選択**Nuget パッケージの管理**、インストールし、 **Xamarin.Forms**パッケージ マネージャー ウィンドウを閉じます。

   [ ![](xamarin-forms-images/add-vb-4-sml.png "フォームとパッケージ マネージャー ウィンドウを閉じる")](xamarin-forms-images/add-vb-4.png)

7. 既定値の名前を変更**Class1**ファイル*と*クラスを`App`:

   [ ![](xamarin-forms-images/add-vb-5-sml.png "アプリを既定の Class1 ファイルとクラスの名前を変更します。")](xamarin-forms-images/add-vb-5.png)

8. 次のコードを貼り付け、 **App.vb** Xamarin.Forms アプリの開始点となるファイル。 必ず含めて`Imports Xamarin.Forms`追加`Inherits Application`クラスに。

    ```vb 
    Imports Xamarin.Forms

    Public Class App
        Inherits Application

        Public Sub New()
            Dim label = New Label With {.HoriztonalTextAlignment = TextAlignment.Center,
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

9. 新しい Visual Basic プロジェクトで、iOS と Android プロジェクトをポイントする必要があります。
右クリックし、**参照**iOS および Android のプロジェクトを開くにはノード、**参照マネージャー**です。 チェック マークの解除、c# ポータブル ライブラリとティック VB ポータブル ライブラリ (しない忘れた、こうと、iOS と Android プロジェクトの両方で)。

   [ ![](xamarin-forms-images/add-vb-8-sml.png "古いプロジェクト参照を削除する、Visual Basic リファレンスの追加")](xamarin-forms-images/add-vb-8.png)

10. C# ポータブル プロジェクトを削除します。 新規追加**.vb**ビルドするファイルは、Xamarin.Forms アプリケーションを出力します。 新しいテンプレート`ContentPage`Visual Basic での s を次に示します。

    ```vb
    Imports Xamarin.Forms

    Public Class Page2
    Inherits ContentPage

        Public Sub New()
            Dim label = New Label With {.HoriztonalTextAlignment = TextAlignment.Center,
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

## <a name="limitations-of-visual-basic-in-xamarinforms"></a>Xamarin.Forms で Visual Basic の制限事項

説明したように、[ポータブル Visual Basic.NET ページ](/guides/cross-platform/application_fundamentals/pcl/portable_visual_basic_net/)Xamarin は、Visual Basic 言語をサポートしていません。 これは、Visual Basic を使用することができますをいくつかの制限があることを意味します。

 - カスタム レンダラーは、Visual Basic で記述することはできません、書き込む必要があります (C#) のネイティブ プラットフォーム プロジェクトでします。

 - 依存関係サービスの実装は、Visual Basic で記述することはできません、書き込む必要があります (C#) のネイティブ プラットフォーム プロジェクトでします。

 - XAML ページは、Visual Basic プロジェクトに含めることができません - のみ、分離コード ジェネレーターを作成できます (C#)。 個別の参照、c# ポータブル クラス ライブラリに XAML を含めるし、XAML ファイルを Visual Basic のモデルを使用して作成するには、データ バインディングを使用することは (この例に含まれる、[サンプル](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB/XamlPages))。

 - Xamarin は、Visual Basic.NET 言語をサポートしていません。

## <a name="related-links"></a>関連リンク

- [XamarinFormsVB (sample)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [.NET Framework (Microsoft) を使用したクロスプラット フォーム開発](http://msdn.microsoft.com/en-us/library/gg597391(v=vs.110).aspx)
