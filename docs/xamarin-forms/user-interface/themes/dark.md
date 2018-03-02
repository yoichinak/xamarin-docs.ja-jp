---
title: "ダーク テーマ"
ms.topic: article
ms.prod: xamarin
ms.assetid: 43A3798D-6F05-4734-AF5E-97235B46D9B9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 0b3a714e8295ed60f201c202fbe30e45dbdcc205
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2018
---
# <a name="dark-theme"></a>ダーク テーマ

![](~/media/shared/preview.png "この API は現在プレビュー中")

> [!NOTE]
> テーマは、Xamarin.Forms 2.3 のプレビュー リリースが必要です。 チェック、[トラブルシューティングのヒント](~/xamarin-forms/user-interface/themes/index.md)エラーが発生する場合。

ダーク テーマを使用します。

## <a name="1-add-nuget-packages"></a>1.Nuget パッケージを追加します。

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Dark

## <a name="2-add-to-the-resource-dictionary"></a>2.リソース ディクショナリに追加します。

**App.xaml**ファイルを追加する新しいカスタム`xmlns`テーマのテーマのリソースは、アプリケーションのリソース ディクショナリとマージすることを確認します。
XAML ファイルの例は、次に示します。

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="EvolveApp.App"
             xmlns:dark="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Dark">
    <Application.Resources>
        <ResourceDictionary MergedWith="dark:DarkThemeResources" />
    </Application.Resources>
</Application>
```

## <a name="3-load-theme-classes"></a>3.テーマ クラスを読み込む

この後に[ステップのトラブルシューティング](~/xamarin-forms/user-interface/themes/index.md)し、iOS および Android アプリケーション プロジェクトで、必要なコードを追加します。

## <a name="4-use-styleclass"></a>4.StyleClass を使用します。

ボタンおよびマークアップを生成すると共に、ダーク テーマでのラベルの例を次に示します。

[ ![](dark-images/dark-theme-sml.png "ダーク テーマでラベルを印刷] ボタンと [")](dark-images/dark-theme.png "] ボタンし、[ダーク テーマでのラベル")

```xaml
<StackLayout Padding="20">
    <Button Text="Button Default" />
    <Button Text="Button Class Default" StyleClass="Default" />
    <Button Text="Button Class Primary" StyleClass="Primary" />
    <Button Text="Button Class Success" StyleClass="Success" />
    <Button Text="Button Class Info" StyleClass="Info" />
    <Button Text="Button Class Warning" StyleClass="Warning" />
    <Button Text="Button Class Danger" StyleClass="Danger" />
    <Button Text="Button Class Link" StyleClass="Link" />

    <Button Text="Button Class Default Small" StyleClass="Small" />
    <Button Text="Button Class Default Large" StyleClass="Large" />
</StackLayout>
```

[組み込みクラスの完全なリスト](~/xamarin-forms/user-interface/themes/index.md)どのようなスタイルは、いくつかの一般的なコントロールの使用を示しています。

