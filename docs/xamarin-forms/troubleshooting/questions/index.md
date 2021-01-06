---
title: Xamarin.Forms よく寄せられる質問
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 89364175-53BA-4A09-B3E2-44AC67DD971C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e520d222029e0d29a25f16aacfb6273da52cbf9e
ms.sourcegitcommit: 044e8d7e2e53f366942afe5084316198925f4b03
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2021
ms.locfileid: "97940214"
---
# <a name="no-locxamarinforms-frequently-asked-questions"></a>Xamarin.Forms よく寄せられる質問

## <a name="how-do-i-migrate-my-app-to-no-locxamarinforms-50"></a>[アプリを5.0 に移行操作方法 Xamarin.Forms ますか?](forms5-migration.md)

アプリを5.0 に移行するには Xamarin.Forms 、互換性に影響する変更についての知識が必要です。

## <a name="can-i-update-the-no-locxamarinforms-default-template-to-a-newer-nuget-package"></a>[Xamarin.Forms既定のテンプレートを新しい NuGet パッケージに更新することはできますか。](update-forms-template.md)

このガイドでは、 Xamarin.Forms 例として .NET Standard ライブラリテンプレートを使用しますが、共有プロジェクトテンプレートでも同じ一般的な方法を使用でき Xamarin.Forms ます。

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-no-locxamarinforms-xaml-files"></a>[Xaml ファイルに対して Visual Studio XAML デザイナーが動作しないのはなぜです Xamarin.Forms か。](forms-xaml-designer.md)

Xamarin.Forms では、XAML ファイルのビジュアルデザイナーは現在サポートされていません。

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedly"></a>[Android のビルドエラー: "LinkAssemblies" タスクが予期せず失敗しました](android-linkassemblies-error.md)

`The "LinkAssemblies" task failed unexpectedly`フォームを使用する Xamarin Android プロジェクトをビルドすると、エラーメッセージが表示される場合があります。 これは、リンカーがアクティブなときに発生します (通常は *リリース* ビルドで、アプリケーションパッケージのサイズを小さくします)。また、Android ターゲットが最新のフレームワークに更新されないために発生します。

## <a name="why-does-my-no-locxamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-error"></a>["なぜですか Xamarin.Forms 。Maps Android プロジェクトが COMPILETODALVIK で失敗する: 予期しないトップレベルのエラーが発生した場合は、"](maps-compiletodalvik-error.md)

このエラーは、Visual Studio for Mac のエラーパッドまたは Visual Studio のビルド出力ウィンドウに表示されることがあります。Android プロジェクトでは、を使用 Xamarin.Forms します。マップ. 最も一般的に解決されるのは、Xamarin Android プロジェクトの Java ヒープサイズを増やすことによって解決されます。
