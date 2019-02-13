---
title: Visual Basic.NET を使用して Xamarin.Forms
description: Xamarin.Forms PCL プロジェクト テンプレートは、効果的に VB.NET を使用してクロス プラットフォーム モバイル アプリを構築することができます、メイン アセンブリの Visual Basic を使用して変更できます。
ms.prod: xamarin
ms.assetid: da4b4ba9-9205-47dc-8bae-23272ede2c50
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: f397cf595a9ae151c5f105341733b2c57023fe99
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109719"
---
# <a name="xamarinforms-using-visual-basicnet"></a>Visual Basic.NET を使用して Xamarin.Forms

Xamarin が Visual Basic を直接サポートされていません - C# Xamarin.Forms PCL ソリューションを作成し、Visual Basic を使用して一般的なコードの PCL プロジェクトを置換するには、このページに、指示に従います。

[![](xamarin-forms-images/hero-sml.png "PCL の Xamarin.Forms ソリューションを作成し、一般的なコードの PCL プロジェクトを Visual Basic で置き換えます")](xamarin-forms-images/hero.png#lightbox)

> [!NOTE]
> Visual Basic でプログラムを Windows で Visual Studio を使用する必要があります。

## <a name="xamarinforms-with-visual-basic-walkthrough"></a>Visual Basic チュートリアルによる Xamarin.Forms

Visual Basic を使用する単純な Xamarin.Forms プロジェクトを作成する次の手順に従います。

1. 新規作成*Xamarin.Forms C#* ポータブル クラス ライブラリ (PCL) を使用するソリューションです。
移動して**ファイル > 新しいプロジェクト**し、**新しいプロジェクト**にウィンドウが移動します**インストール済み > テンプレート > Visual C# > クロス プラットフォーム**選択**クロス プラットフォーム アプリ (Xamarin.Forms またはネイティブ) > Xamarin.Forms**します。

2. ソリューションを右クリックし、**追加 > 新しいプロジェクト**します。

3. 選択、 **Visual Basic > クラス ライブラリ (ポータブル)** プロジェクトの種類。

   [![](xamarin-forms-images/add-vb-2-sml.png "新しいポータブル クラス ライブラリ プロジェクトを追加します。")](xamarin-forms-images/add-vb-2.png#lightbox)

4. ように、適切な PCL プロファイルを構成するのには、プラットフォームを選択します (Xamarin.iOS と Xamarin.Android を含めることを確認する)。

   ![](xamarin-forms-images/add-vb-3-sml.png "サポートするプラットフォームを選択します。")

5. Visual Basic プロジェクトを右クリックし、選択**プロパティ**、変更、**既定の名前空間**に合わせて既存の C# プロジェクトします。

   ![](xamarin-forms-images/add-vb-4s-sml.png "Visual Basic のルート名前空間に一致する Xamarin.Forms アプリを確認します。")

6. 新しい Visual Basic プロジェクトを右クリックし、選択**Nuget パッケージの管理**、インストールし、 **Xamarin.Forms**パッケージ マネージャー ウィンドウを閉じます。

   [![](xamarin-forms-images/add-vb-4-sml.png "フォームと、パッケージ マネージャー ウィンドウを閉じます")](xamarin-forms-images/add-vb-4.png#lightbox)

7. 既定の名前を**Class1**ファイル*と*クラスを`App`:

   [![](xamarin-forms-images/add-vb-5-sml.png "アプリに既定の Class1 ファイルとクラスの名前を変更します。")](xamarin-forms-images/add-vb-5.png#lightbox)

8. 次のコードを貼り付け、 **App.vb**ファイルで、Xamarin.Forms アプリの開始点になります。 必ず含めて`Imports Xamarin.Forms`追加`Inherits Application`クラス。

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
右クリックし、**参照**iOS および Android プロジェクトを開く ノード、**参照マネージャー**します。 チェック マークの解除、C#ポータブル ライブラリと VB のポータブル ライブラリ (しないもご活用ください、これは iOS と Android プロジェクトの両方の操作を行います)。

   [![](xamarin-forms-images/add-vb-8-sml.png "古いプロジェクト参照を削除する、Visual Basic リファレンスの追加")](xamarin-forms-images/add-vb-8.png#lightbox)

10. C# ポータブル プロジェクトを削除します。 新規追加 **.vb**チェック アウト、Xamarin.Forms アプリケーションをビルドするファイル。 新しいテンプレート`ContentPage`Visual Basic では、以下に示します。

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

## <a name="limitations-of-visual-basic-in-xamarinforms"></a>Xamarin.Forms での Visual Basic の制限事項

説明したように、[移植可能な Visual Basic.NET ページ](~/cross-platform/platform/visual-basic/index.md)Xamarin は Visual Basic 言語をサポートしていません。 これは、Visual Basic を使用するいくつかの制限があることを意味します。

 - Visual Basic では、カスタム レンダラーを書き込むことができません、書き込む必要があります (C#)、ネイティブ プラットフォーム プロジェクトにします。

 - Visual Basic では、依存関係サービスの実装を書き込むことができません、書き込む必要があります (C#)、ネイティブ プラットフォーム プロジェクトにします。

 - XAML ページは、Visual Basic プロジェクトに含めることができません - C# 分離コード ジェネレーターの作成できるのみです。 独立した、参照、C# ポータブル クラス ライブラリに XAML を含めるし、データ バインドを使用して Visual Basic のモデルを使用して、XAML ファイルを作成することは (この例が記載されて、[サンプル](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB/XamlPages))。

 - Xamarin は Visual basic.net 言語をサポートしていません。

## <a name="related-links"></a>関連リンク

- [XamarinFormsVB (sample)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [.NET Framework を使用したクロス プラットフォーム開発](https://docs.microsoft.com/dotnet/standard/cross-platform/)
