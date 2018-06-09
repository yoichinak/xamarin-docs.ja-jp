---
title: Xamarin.Forms テーマ
description: この記事では、Xamarin.Forms のテーマは、標準のビューの特定の視覚的外観を定義するが導入されています。
ms.prod: xamarin
ms.assetid: 3DFB7C55-69F6-4980-A501-588719143482
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/01/2017
ms.openlocfilehash: 0f49eeba072d6aeb7ead40d5d56d4af9e9bf5e27
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245733"
---
# <a name="xamarinforms-themes"></a>Xamarin.Forms テーマ

![](~/media/shared/preview.png "この API は現在プレビュー中")

Xamarin.Forms テーマする 2016年で発表されたおよびフィードバックを提供する顧客用のプレビューとして利用できます。

テーマは含めることで Xamarin.Forms アプリケーションに追加、 **Xamarin.Forms.Theme.Base**プラス (の特定のテーマを定義する追加のパッケージの Nuget パッケージ Xamarin.Forms.Theme.Light) またはローカルのテーマを定義して、アプリケーションのそれ以外の場合。

参照してください、[ライト テーマ](light.md)と[ダーク テーマ](dark.md)、アプリに追加するか、チェック アウトする方法についてのページ、[例カスタム テーマ](custom.md)です。

**重要:** する手順を行う必要があります[テーマ アセンブリ (下記) を読み込む](#loadtheme)iOS に定型コードを追加することによって`AppDelegate`および Android`MainActivity`です。 これが、将来のプレビュー リリースでは改善されます。


## <a name="control-appearance"></a>コントロールの外観

[ライト](light.md)と[濃い](dark.md)両方のテーマが標準コントロールの特定の外観を定義します。 アプリケーションのリソース ディクショナリにテーマを追加すると、標準のコントロールの外観が変更されます。

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

これらのスクリーン ショットでは、これらのコントロールで表示します。

* テーマが適用なし
* ライト テーマ (テーマを持たないに微妙な差異のみ)
* ダーク テーマ

![](images/standard-none-sml.png "テーマせずコントロール") ![ ](images/standard-light-sml.png "ライト テーマでのコントロール") ![ ](images/standard-dark-sml.png "ダーク テーマでのコントロール")

<a name="styleclass" />

## <a name="styleclass"></a>StyleClass

`StyleClass`プロパティは、テーマによって提供される定義に応じて変更するビューの外観を使用します。

[ライト](light.md)と[濃い](dark.md)両方のテーマの 3 つの異なる外観を定義する、 `BoxView`: `HorizontalRule`、 `Circle`、および`Rounded`です。 このマークアップを表示する 3 つの異なる`BoxView`s 適用とは異なるスタイル クラスを使用します。

```xaml
<StackLayout Padding="40">
    <BoxView StyleClass="HorizontalRule" />
    <BoxView StyleClass="Circle" />
    <BoxView StyleClass="Rounded" />
</StackLayout>
```

これによって、薄いおよび濃いとおり。

![](images/boxview-light-sml.png "ライト テーマ StyleClass で BoxView") ![ ](images/boxview-dark-sml.png "ダーク テーマ StyleClass で BoxView")

<a name="builtin" />

## <a name="built-in-classes"></a>組み込みクラス

自動的にスタイル設定だけでなく、一般的なは、光を制御し、濃色のテーマが現在の設定を適用できる次のクラスをサポート、`StyleClass`これらのコントロールに。

**BoxView**

* HorizontalRule
* [円]
* 丸められます

**イメージ**

* [円]
* 丸められます
* 縮小表示

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
* 逆の操作


## <a name="troubleshooting"></a>トラブルシューティング

<a name="loadtheme" />

### <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>ファイルまたはアセンブリ 'Xamarin.Forms.Theme.Light' またはその依存関係の 1 つを読み込めませんでした。

プレビュー リリースでテーマの実行時に読み込むこといない場合があります。 このエラーを解決するのには、関連するプロジェクトに、以下に示すコードを追加します。

**iOS**

**<code>appdelegate.cs</code>** 後に次の行を追加 `LoadApplication`

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
