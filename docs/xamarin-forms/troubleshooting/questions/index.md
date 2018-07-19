---
title: よく寄せられる質問
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 89364175-53BA-4A09-B3E2-44AC67DD971C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: 5a36c6ab14fdc7bfc5916456670be9c8fe4476ff
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
ms.locfileid: "34049884"
---
# <a name="frequently-asked-questions"></a>よく寄せられる質問


## <a name="can-i-update-the-xamarinforms-default-template-to-a-newer-nuget-packageupdate-forms-templatemd"></a>[Xamarin.Forms の既定のテンプレートを新しい NuGet パッケージに更新することはできますか。](update-forms-template.md)
このガイドは、例として、Xamarin.Forms .NET 標準のライブラリ テンプレートを使用しますが、同じ一般的な方法は、Xamarin.Forms 共有プロジェクト テンプレートのでも機能します。 

## <a name="why-doesnt-the-visual-studio-xaml-designer-work-for-xamarinforms-xaml-filesforms-xaml-designermd"></a>[Visual Studio XAML デザイナーが Xamarin.Forms XAML ファイルに有効でない理由を教えてください。](forms-xaml-designer.md)
Xamarin.Forms は XAML ファイルのビジュアルなデザイナーをサポートされていません。

## <a name="android-build-error-the-linkassemblies-task-failed-unexpectedlyandroid-linkassemblies-errormd"></a>[Android のビルド エラー: "LinkAssemblies" タスクが予期せず失敗しました](android-linkassemblies-error.md)
エラー メッセージが表示することがあります`The "LinkAssemblies" task failed unexpectedly`フォームを使用する Xamarin.Android プロジェクトを構築する場合。 これは、エラーは、リンカーは、アクティブな場合 (通常の*リリース*アプリ パッケージのサイズを縮小するビルド); し、Android の対象は、最新のフレームワークに更新されないために発生します。 


## <a name="why-does-my-xamarinformsmaps-android-project-fail-with-compiletodalvik--unexpected-top-level-errormaps-compiletodalvik-errormd"></a>["理由が Xamarin.Forms.Maps Android プロジェクトで異常終了 COMPILETODALVIK: トップレベルの予期しないエラーですか?"](maps-compiletodalvik-error.md)
Mac 用または Visual Studio 以外のビルド出力 ウィンドウで Visual Studio のエラー パッドでこのエラーが発生する可能性があります。で Android プロジェクト Xamarin.Forms.Maps を使用します。 最もよくは Xamarin.Android プロジェクト用 Java のヒープ サイズを増やすことで解決します。

