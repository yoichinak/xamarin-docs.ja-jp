---
title: Xamarin.Forms のライト テーマ
description: この記事では、アプリでは、Xamarin.Forms 光のテーマを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: D5D16AE3-F51F-4359-B37A-E1087ECE512B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 7f40e375d653acec60f8848627234ab46fcce8de
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60896273"
---
# <a name="xamarinforms-light-theme"></a>Xamarin.Forms のライト テーマ

![](~/media/shared/preview.png "この API は現在プレビュー段階")

> [!NOTE]
> テーマでは、Xamarin.Forms 2.3 のプレビュー リリースが必要です。 チェック、[トラブルシューティングのヒント](~/xamarin-forms/user-interface/themes/index.md)エラーが発生した場合。

ライト テーマを使用するには。

## <a name="1-add-nuget-packages"></a>1.Nuget パッケージを追加します。

* Xamarin.Forms.Theme.Base
* Xamarin.Forms.Theme.Light

## <a name="2-add-to-the-resource-dictionary"></a>2.リソース ディクショナリに追加します。

**App.xaml**ファイルに追加する新しいカスタム`xmlns`テーマのテーマのリソースは、アプリケーションのリソース ディクショナリとマージを確認します。
XAML ファイルの例は、以下に示します。

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="EvolveApp.App"
             xmlns:light="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light">
    <Application.Resources>
        <ResourceDictionary MergedWith="light:LightThemeResources" />
    </Application.Resources>
</Application>
```

## <a name="3-load-theme-classes"></a>3.テーマ クラスを読み込む

この後に[トラブルシューティング手順](~/xamarin-forms/user-interface/themes/index.md)し、iOS と Android アプリケーション プロジェクトで、必要なコードを追加します。

## <a name="4-use-styleclass"></a>4.StyleClass を使用します。

ボタンおよびそれらを生成するマークアップと共に、ライト テーマでのラベルの例を次に示します。

[![](light-images/light-theme-sml.png "ライト テーマでラベルを印刷] ボタンと [")](light-images/light-theme.png#lightbox "ボタンし、ライト テーマでのラベル")

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

[組み込みクラスの完全な一覧](~/xamarin-forms/user-interface/themes/index.md)どのようなスタイルは、いくつかの一般的なコントロールの使用を示しています。
