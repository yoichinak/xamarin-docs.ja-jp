---
title: Xamarin.Formsよく寄せられる質問
ms.topic: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: edd6cfefe18ff3d5cc97ec58f3bce867f11df7c8
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84135877"
---
# <a name="xamarinforms-frequently-asked-questions"></a>Xamarin.Formsよく寄せられる質問

## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[Xamarin.Forms既定のテンプレートを新しい NuGet パッケージに更新することはできますか。](update-forms-template.md)
このガイドでは、 Xamarin.Forms 例として .NET Standard ライブラリテンプレートを使用しますが、共有プロジェクトテンプレートでも同じ一般的な方法を使用でき Xamarin.Forms ます。

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[Xaml ファイルに対して Visual Studio XAML デザイナーが動作しないのはなぜです Xamarin.Forms か。](forms-xaml-designer.md)
Xamarin.Formsでは、XAML ファイルのビジュアルデザイナーは現在サポートされていません。

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedly"></a>[Android のビルドエラー: "LinkAssemblies" タスクが予期せず失敗しました](android-linkassemblies-error.md)
`The "LinkAssemblies" task failed unexpectedly`フォームを使用する Xamarin Android プロジェクトをビルドすると、エラーメッセージが表示される場合があります。 これは、リンカーがアクティブなときに発生します (通常は*リリース*ビルドで、アプリケーションパッケージのサイズを小さくします)。また、Android ターゲットが最新のフレームワークに更新されないために発生します。 

## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>["なぜですか Xamarin.Forms 。Maps Android プロジェクトが COMPILETODALVIK で失敗する: 予期しないトップレベルのエラーが発生した場合は、"](maps-compiletodalvik-error.md)
このエラーは、Visual Studio for Mac のエラーパッドまたは Visual Studio のビルド出力ウィンドウに表示されることがあります。Android プロジェクトでは、を使用 Xamarin.Forms します。マップ. 最も一般的に解決されるのは、Xamarin Android プロジェクトの Java ヒープサイズを増やすことによって解決されます。
