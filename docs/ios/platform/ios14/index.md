---
title: IOS 14 の概要
description: このドキュメントでは、Xamarin が C# バインドを提供する iOS 14 Api の概要について説明します。
ms.prod: xamarin
ms.assetid: 4953216e-472b-4484-9c1e-7263ac537f21
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/17/2020
ms.openlocfilehash: e9793617c76813fb68a57213edd8b48529f19ac7
ms.sourcegitcommit: 0c45e3f810947e3d43223aa01bf3e43a0defca65
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2020
ms.locfileid: "90843607"
---
# <a name="introduction-to-ios-14"></a>IOS 14 の概要

開始するには、こちらの [手順](~/ios/platform/ios14/get-started.md) に従ってください。

## <a name="new-control-uicolorwell"></a>新しいコントロール: UIColorWell

[`UIColorWell`](https://developer.apple.com/documentation/uikit/uicolorwell) は、選択した見本から色を選択したり、スポイトを使用したり、手動で値を入力したりするための新しい UIKit コントロールです。 コントロールには、タップしたときにモーダルフォームを起動する丸い色のボタンが表示されます。

![UIColorWell](ios14-images/colorwell.png)

```xaml
<ios:UIColorWell
    SelectedColor="{x:Static ios:UIColor.Red}"
    ValueChanged="OnColorChanged" />
```

```csharp
private void OnColorChanged(object sender, EventArgs e)
{
    var colorWell = (UIColorWell)sender; 
    Debug.WriteLine(colorWell.SelectedColor);
}
```

## <a name="modified-controls"></a>変更されたコントロール

いくつかのコントロールが更新を受け取りました。特に次のようなものがあります。

- [Uibarbuttonitem](https://developer.apple.com/documentation/uikit/uibarbuttonitem) は、segue として表示される UIMenu を追加できるようになりました。
- [UIDatePicker](https://developer.apple.com/documentation/uikit/uidatepicker) では、自動 (既定)、コンパクト、インライン、およびホイールの複数のスタイルがサポートされるようになりました。
- [Uisplitviewcontroller](https://developer.apple.com/documentation/uikit/uisplitviewcontroller) では、プライマリ、セカンダリ、補助の3つの列がサポートされるようになりました。
 
![プレリリース API](~/media/shared/preview.png)

## <a name="embedded-widgetkit-support"></a>埋め込み WidgetKit のサポート

この SDK のリリースでは、Swift で記述された WidgetKit 拡張機能をメインの Xamarin. iOS アプリケーションに埋め込むためのサポートが追加されました。 これにより、現在、ウィジェットをサポートするアプリを構築できます。

この方法では、"ハイブリッド" アプリケーションを作成し、SwiftUI を使用してウィジェット拡張機能をビルドし、Xamarin. iOS アプリケーションに埋め込むことができます。

WidgetKit サポートを利用するには、プロジェクトファイルを手動で変更する必要があります。

次のようなセクションをプロジェクトに追加します。

```xml
<AdditionalAppExtensions Include="$(MSBuildProjectDirectory)/../../native">
     <Name>NativeTodayExtension</Name>
     <BuildOutput Condition="'$(Platform)' == 'iPhone'">build/Debug-iphoneos</BuildOutput>
     <BuildOutput Condition="'$(Platform)' == 'iPhoneSimulator'">build/Debug-iphonesimulator</BuildOutput>
</AdditionalAppExtensions>
```

Swift UI 拡張機能のビルドディレクトリを指すように、最初のリンクに含まれるパスを変更します。

Xcode プロジェクト内のプロジェクトの相対的な出力場所 ([ファイル] → [プロジェクトの設定]) を有効にすると、次のように簡単に検索できます。

![Xcode の設定](ios14-images/xcode-settings.png)

この [サンプルアプリケーション](https://github.com/chamons/xamarin-ios-swift-extension/blob/master/App/TestApplication/TestApplication.csproj#L143) では、JSON シリアル化を使用して、Xamarin. iOS アプリからのデータを、表示用のサンプルウィジェットに転送します。

WidgetKit に興味をお持ちの方は、 [こちらからフィードバック](https://github.com/xamarin/xamarin-macios/issues/8933)をお送りください。

## <a name="related-links"></a>関連リンク

- [Xamarin iOS 14 リリースノート](/xamarin/ios/release-notes/14/14.0)
- [UIColorWell のドキュメント](https://developer.apple.com/documentation/uikit/uicolorwell)
