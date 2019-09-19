---
title: XAML プレビューアーでカスタムコントロールを表示する
description: この記事では、XAML プレビューアーでカスタムコントロールを表示する方法について説明します。
ms.prod: xamarin
ms.assetid: 4D795372-CB8F-48F4-B63D-845E44B261F7
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
ms.openlocfilehash: 57c0fd540ef42c18462b4f989b21bac5ed05dc04
ms.sourcegitcommit: 6b833f44d5fd8dc7ab7f8546e8b7d383e5a989db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2019
ms.locfileid: "71105994"
---
# <a name="render-custom-controls-in-the-xaml-previewer"></a>XAML プレビューアーでカスタムコントロールを表示する

_カスタムコントロールは、XAML プレビューアーで期待どおりに動作しないことがあります。カスタムコントロールをプレビューする際の制限事項については、この記事のガイダンスを参考にしてください。_

## <a name="basic-preview-mode"></a>基本プレビューモード

プロジェクトをビルドしていない場合でも、XAML プレビューアーによってページが表示されます。 ビルドするまで、分離コードに依存するコントロールには、その基本となる Xamarin. Forms 型が表示されます。 プロジェクトがビルドされると、XAML プレビューアーは、デザイン時の表示が有効になっているカスタムコントロールを表示しようとします。 レンダリングが失敗した場合は、基本の Xamarin. Forms 型が表示されます。

## <a name="enable-design-time-rendering-for-custom-controls"></a>カスタムコントロールのデザイン時の表示を有効にする

独自のカスタムコントロールを作成したり、サードパーティのライブラリのコントロールを使用したりすると、プレビューアーによって正しく表示されない場合があります。 カスタムコントロールは、コントロールを作成したかライブラリからインポートしたかにかかわらず、プレビューに表示されるデザイン時のレンダリングをオプトインする必要があります。 作成したコントロールで、をコントロール[`[DesignTimeVisible(true)]`](xref:System.ComponentModel.DesignTimeVisibleAttribute)のクラスに追加して、プレビューアーに表示します。

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

例として、 [James Montemagno の ImageCirclePlugin's 基底クラス](https://github.com/jamesmontemagno/ImageCirclePlugin/blob/master/src/ImageCircle/CircleImage.shared.cs)を使用します。

## <a name="skiasharp-controls"></a>SkiaSharp コントロール

現時点では、SkiaSharp コントロールは、iOS でプレビューしている場合にのみサポートされます。 Android preview ではレンダリングされません。

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="check-your-xamarinforms-version"></a>Xamarin. Forms バージョンを確認する
少なくとも Xamarin. Forms 3.6 がインストールされていることを確認します。 NuGet で Xamarin. Forms バージョンを更新できます。

### <a name="even-with-designtimevisibletrue-my-custom-control-isnt-rendering-properly"></a>で`[DesignTimeVisible(true)]`も、カスタムコントロールは正しくレンダリングされません。
分離コードやバックエンドデータに大きく依存するカスタムコントロールは、常に XAML プレビューアーで動作するわけではありません。 次の操作を実行できます。

* [デザインモードが有効になっ](index.md#detect-design-mode)ている場合に初期化されないようにコントロールを移動する
* バックエンドからの偽のデータを表示するように[デザイン時データ](design-time-data.md)を設定する

### <a name="the-xaml-previewer-shows-the-error-custom-controls-arent-rendering-properly"></a>XAML プレビューアーに "カスタムコントロールが正しくレンダリングされていません" というエラーが表示される
プロジェクトをクリーニングしてリビルドするか、XAML ファイルを閉じてから開き直してみてください。
