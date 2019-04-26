---
title: XAML プレビューアーでカスタム コントロールをレンダリングします。
description: この記事では、カスタム コントロールを XAML プレビューアーで表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 4D795372-CB8F-48F4-B63D-845E44B261F7
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
ms.openlocfilehash: 977c29312e0be8b92f216c224414f9bd03f8562d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "60875978"
---
# <a name="render-custom-controls-in-the-xaml-previewer"></a>XAML プレビューアーでカスタム コントロールをレンダリングします。

_カスタム コントロールは、XAML プレビューアーで想定どおり使用機能しない場合があります。この記事では、ガイダンスを使用すると、カスタム コントロールのプレビューの制限事項を把握できます。_

## <a name="basic-preview-mode"></a>基本のプレビュー モード

プロジェクトをビルドしていない場合でも、XAML プレビューアーには、ページが表示されます。 構築するまで、その基本 Xamarin.Forms の型が分離コードに依存している任意のコントロールに表示されます。 プロジェクトのビルド時に XAML プレビューアーはデザイン時の表示を有効になっていると、カスタム コントロールを表示しようとします。 レンダリングに失敗した場合は、Xamarin.Forms の基本の型が表示されます。

## <a name="enable-design-time-rendering-for-custom-controls"></a>カスタム コントロールのデザイン時のレンダリングを有効にします。

場合は、独自のカスタム コントロールを作成またはサード パーティ製のライブラリからコントロールを使用して、プレビューアーが正しく表示されないに。 カスタム コントロールがデザイン タイムでレンダリングにコントロールを記述またはライブラリからインポートするかどうかは、プレビューアーで表示されるオプトインする必要があります。 作成した、コントロールを追加、 [ `[DesignTimeVisible(true)]` ](xref:System.ComponentModel.DesignTimeVisibleAttribute)プレビューアーで表示するのには、コントロールのクラスに。

```csharp
namespace MyProject
{
  [DesignTimeVisible(true)]
  public class MyControl : BaseControl
  {
    // Your control's code here
  }

}
```

使用[ImageCirclePlugin James Montemagno 氏の基本クラス](https://github.com/jamesmontemagno/ImageCirclePlugin/blob/master/src/ImageCircle/CircleImage.shared.cs)など。


## <a name="skiasharp-controls"></a>SkiaSharp のコントロール

現時点では、iOS でプレビューしているときに SkiaSharp コントロールのみサポートされます。 Android のプレビューに表示されません。

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="check-your-xamarinforms-version"></a>Xamarin.Forms バージョンを確認してください。
以上であることを確認 Xamarin.Forms 3.6 がインストールされています。 NuGet で Xamarin.Forms バージョンを更新することができます。

### <a name="even-with-designtimevisibletrue-my-custom-control-isnt-rendering-properly"></a>使用しても`[DesignTimeVisible(true)]`、カスタム コントロールが正しく表示されていません。
コード ビハインドまたはバックエンドのデータに大きく依存するカスタム コントロールは、XAML プレビューアーで常に機能しません。 行うことができます。
* コントロールを移動して、場合に初期化しないため[デザイン モードが有効になっています。](index.md#detect-design-mode)
* 設定する[デザイン時データ](design-time-data.md)バックエンドからの仮のデータを表示するには

### <a name="the-xaml-previewer-shows-the-error-custom-controls-arent-rendering-properly"></a>XAML プレビューアーは、「カスタム コントロールは正しくレンダリングされません」というエラーを示します
Try クリーニングし、プロジェクトをリビルドまたは閉じて XAML ファイル。
