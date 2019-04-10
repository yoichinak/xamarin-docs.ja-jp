---
title: Xamarin.Forms についてよく寄せられる質問
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 89364175-53BA-4A09-B3E2-44AC67DD971C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 24e2ae456e478585f30aa704917f66bb0bf11da9
ms.sourcegitcommit: 650458de1d362cd7de174cacef7838f0e74426f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2019
ms.locfileid: "57981641"
---
# <a name="xamarinforms-frequently-asked-questions"></a>Xamarin.Forms についてよく寄せられる質問

## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[Xamarin.Forms の既定のテンプレートを新しい NuGet パッケージに更新することはできますか。](update-forms-template.md)
このガイドは、例として、Xamarin.Forms .NET Standard ライブラリ テンプレートを使用しますが、同じ一般的な方法は、Xamarin.Forms 共有プロジェクト テンプレートの動作も。

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[Visual Studio XAML デザイナーが Xamarin.Forms XAML ファイルに有効でない理由を教えてください。](forms-xaml-designer.md)
Xamarin.Forms は XAML ファイルのビジュアル デザイナーをサポートされていません。

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedlyandroid-linkassemblies-errormd"></a>[Android のビルド エラー: "LinkAssemblies" タスクが予期せず失敗しました](android-linkassemblies-error.md)
エラー メッセージが表示することがあります`The "LinkAssemblies" task failed unexpectedly`Xamarin.Android プロジェクトのビルドがフォームを使用している場合。 リンカーは、アクティブな場合 (通常で、*リリース*アプリ パッケージのサイズを小さくビルド); Android ターゲットが最新のフレームワークに更新されないために発生するとします。 

## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>["なぜは Xamarin.Forms.Maps Android プロジェクトで失敗 COMPILETODALVIK:予期しない最上位レベル エラーの場合ですか?"](maps-compiletodalvik-error.md)
このエラーは for Mac または Visual Studio のビルド出力 ウィンドウで、Visual Studio のエラー パッドで発生する可能性があります。Xamarin.Forms.Maps を使用して Android のプロジェクト。 最もよくは、Xamarin.Android プロジェクト用の Java ヒープ サイズを増やすことで解決します。
