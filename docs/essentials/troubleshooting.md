---
title: 'Xamarin.Essentials: トラブルシューティング'
description: このドキュメントでは、Xamarin.Essentials ライブラリを開発するときに発生した問題をトラブルシューティングする方法について説明します。
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 06/26/2018
ms.openlocfilehash: 3dba315aec2475cb334110ba7555f773f4165aa1
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066735"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: トラブルシューティング

![プレリリース NuGet](~/media/shared/pre-release.png)

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>エラー: バージョンの競合の Xamarin.Android.Support.Compat が検出されました

NuGet パッケージを更新するときに、次のエラーが発生する可能性があります (または新しいパッケージを追加する) で、Xamarin.Forms プロジェクト Xamarin.Essentials を使用します。

```
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 0.7.0.17-preview -> Xamarin.Android.Support.CustomTabs 27.0.2 -> Xamarin.Android.Support.Compat (= 27.0.2) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

この問題は、次の 2 つの NuGets の不一致の依存関係です。 依存関係の特定のバージョンを手動で追加して解決できます (このケースで**Xamarin.Android.Support.Compat**) 両方をサポートすることができます。

これを行うには、手動で競合の原因である NuGet を追加しを使用して、**バージョン**リストを特定のバージョンを選択します。 現在バージョン 27.0.2 の Xamarin.Android.Support.Compat NuGet は、このエラーを解決します。

参照してください[このブログの投稿](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/)の詳細については、ビデオ、問題を解決する方法については、します。

された懸案事項またはバグを報告してくださいで検索を実行している場合、 [Xamarin.Essentials GitHub リポジトリ](http://github.com/xamarin/Essentials)です。
