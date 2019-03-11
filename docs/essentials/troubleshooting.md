---
title: Xamarin.Essentials:トラブルシューティング
description: このドキュメントでは、Xamarin.Essentials ライブラリを使って開発するときに発生した問題のトラブルシューティング方法について説明します。
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: d13589680161de4c9b5d77eef6d5f823cc884136
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671430"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials:トラブルシューティング

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>エラー :Xamarin.Android.Support.Compat について検出されるバージョンの競合

Xamarin.Essentials を使っている Xamarin.Forms プロジェクトを含む NuGet パッケージを更新 (または新しいパッケージを追加) するときに、次のエラーが発生する場合があります。

```error
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 0.8.0-preview -> Xamarin.Android.Support.CustomTabs 27.0.2.1 -> Xamarin.Android.Support.Compat (= 27.0.2.1) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

問題は、2 つの NuGet の依存関係の不一致にあります。 これは、両方をサポートできる特定のバージョンの依存関係 (この場合は **Xamarin.Android.Support.Compat**) を手動で追加することで解決できます。

これを行うには、競合の原因となっている NuGet を手動で追加し、**バージョン**の一覧を使って特定のバージョンを選びます。 現時点では、バージョン 27.0.2.1 の Xamarin.Android.Support.Compat & Xamarin.Android.Support.Core.Util NuGet によりこのエラーが解決されます。

詳細情報と、問題の解決方法に関するビデオについては、[このブログ記事](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/)を参照してください。

問題が発生したりバグを見つけたりした場合は、[Xamarin.Essentials の GitHub リポジトリ](https://github.com/xamarin/Essentials)上でご報告ください。
