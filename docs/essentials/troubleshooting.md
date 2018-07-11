---
title: 'Xamarin.Essentials: のトラブルシューティング'
description: このドキュメントでは、Xamarin.Essentials ライブラリを開発するときに発生した問題のトラブルシューティングを行う方法について説明します。
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 06/26/2018
ms.openlocfilehash: 8cb18ab029d2fd161c60fda7e130f319b8f0c3ab
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37947375"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: のトラブルシューティング

![NuGet にプレリリースします。](~/media/shared/pre-release.png)

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>エラー: バージョンの競合の Xamarin.Android.Support.Compat が検出されました

NuGet パッケージを更新するときに、次のエラーが発生する可能性があります (または新しいパッケージを追加する) Xamarin.Essentials を使用して Xamarin.Forms プロジェクトで。

```
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 0.8.0-preview -> Xamarin.Android.Support.CustomTabs 27.0.2.1 -> Xamarin.Android.Support.Compat (= 27.0.2.1) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

問題は、2 つの NuGets の不一致の依存関係です。 これは、依存関係の特定のバージョンを手動で追加することで解決できます (この場合**Xamarin.Android.Support.Compat**) 両方をサポートすることができます。

これを行うには、手動で競合の原因である NuGet を追加し、使用、**バージョン**一覧を特定のバージョンを選択します。 現在 27.0.2.1 Xamarin.Android.Support.Compat & Xamarin.Android.Support.Core.Util NuGet のバージョンは、このエラーを解決します。

参照してください[このブログの投稿](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/)詳細と、問題を解決する方法のビデオをします。

問題やバグを報告してくださいで検索を実行している場合、 [Xamarin.Essentials GitHub リポジトリ](http://github.com/xamarin/Essentials)します。
