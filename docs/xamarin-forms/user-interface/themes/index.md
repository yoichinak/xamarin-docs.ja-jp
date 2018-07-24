---
title: Xamarin.Forms のテーマ
description: この記事では、Xamarin.Forms のテーマは、標準的なビューの特定の視覚的外観の定義について説明します。
ms.prod: xamarin
ms.assetid: 3DFB7C55-69F6-4980-A501-588719143482
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 0f49eeba072d6aeb7ead40d5d56d4af9e9bf5e27
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38814708"
---
# <a name="xamarinforms-themes"></a>Xamarin.Forms のテーマ

![](~/media/shared/preview.png "この API は現在プレビュー段階")

Xamarin.Forms のテーマの進化 2016年で発表されたおよびフィードバックを提供するお客様向けのプレビューとして利用できます。

テーマは含めることで、Xamarin.Forms アプリケーションに追加されます、 **Xamarin.Forms.Theme.Base** (例: 特定のテーマを定義する追加のパッケージと、Nuget パッケージ Xamarin.Forms.Theme.Light) またはそれ以外の場合、アプリケーションのローカルのテーマを定義できます。

参照してください、[ライト テーマ](light.md)と[ダーク テーマ](dark.md)確認したり、アプリに追加する方法についてのページ、[例のカスタム テーマ](custom.md)します。

**重要:** する手順を行う必要があります[テーマ アセンブリ (下記) を読み込む](#loadtheme)iOS に一部の定型コードを追加して`AppDelegate`と Android`MainActivity`します。 これは、将来のプレビュー リリースで改善されます。


## <a name="control-appearance"></a>コントロールの外観

[Light](light.md)と[濃い](dark.md)両方のテーマは、標準のコントロールの特定の外観を定義します。 アプリケーションのリソース ディクショナリにテーマを追加すると、標準のコントロールの外観が変更されます。

次の XAML マークアップは、いくつかの一般的なコントロールを示しています。

```xaml
<StackLayout Padding="40">
    <Label Text="Regular label" />
    <Entry Placeholder="type here" />
    <Button Text="OK" />
    <BoxView Color="Yellow" />
    <Switch />
</StackLayout>
```

これらのスクリーン ショットでは、これらのコントロールに表示します。

* テーマが適用されていません
* ライト テーマ (テーマを持たないに微妙な違いのみ)
* ダーク テーマ

![](images/standard-none-sml.png "テーマせずコントロール") ![](images/standard-light-sml.png "ライト テーマでのコントロール") ![](images/standard-dark-sml.png "ダーク テーマでのコントロール")

<a name="styleclass" />

## <a name="styleclass"></a>StyleClass

`StyleClass`プロパティがビューの外観はテーマによって提供される定義に従って変更することができます。

[光](light.md)と[濃い](dark.md)両方のテーマの 3 つの異なる外観を定義する、 `BoxView`: `HorizontalRule`、 `Circle`、および`Rounded`します。 このマークアップを表示する 3 つの異なる`BoxView`を別のスタイル クラスを適用します。

```xaml
<StackLayout Padding="40">
    <BoxView StyleClass="HorizontalRule" />
    <BoxView StyleClass="Circle" />
    <BoxView StyleClass="Rounded" />
</StackLayout>
```

これは、ように表示されます光と暗い。

![](images/boxview-light-sml.png "ライト テーマ StyleClass で BoxView") ![](images/boxview-dark-sml.png "ダーク テーマ StyleClass で BoxView")

<a name="builtin" />

## <a name="built-in-classes"></a>組み込みクラス

自動的にスタイル設定だけでなく、一般的なは、ライトを制御および濃色のテーマが現在設定して適用できる次のクラスをサポート、`StyleClass`これらのコントロールに。

**BoxView**

* HorizontalRule
* [円]
* 丸められます

**イメージ**

* [円]
* 丸められます
* サムネイル

**Button**

* 既定値
* 1 次式
* 成功
* Info
* 警告
* 危険性
* リンク
* 小さな
* 大規模です

**Label**

* Header
* サブヘッダー
* 本文
* リンク
* 逆関数


## <a name="troubleshooting"></a>トラブルシューティング

<a name="loadtheme" />

### <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>ファイルまたはアセンブリ 'Xamarin.Forms.Theme.Light' またはその依存関係の 1 つを読み込めませんでした。

プレビュー リリースでは、テーマの実行時に読み込むことがない場合があります。 このエラーを修正するのには、関連するプロジェクトに、以下に示すコードを追加します。

**iOS**

**AppDelegate.cs**後に次の行を追加 `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

**MainActivity.cs**後に次の行を追加 `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```


## <a name="related-links"></a>関連リンク

- [ThemesDemo サンプル](https://github.com/xamarin/xamarin-forms-samples/tree/master/Themes/ThemesDemo)
