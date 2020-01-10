---
title: トラブルシューティング
description: 一般的なエラー状態とその解決方法
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63291951-7375-4CBF-BCC3-2E4AD157A2C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
ms.openlocfilehash: b38e33e05b0bb9d40582611857671d6617023b35
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728318"
---
# <a name="troubleshooting"></a>トラブルシューティング

_一般的なエラー状態とその解決方法_

## <a name="error-unable-to-find-a-version-of-xamarinforms-compatible-with"></a>エラー: "Xamarin のバージョンを見つけることができません。" と互換性があります。

Xamarin. Forms ソリューションまたは Xamarin. Forms Android アプリプロジェクトですべての NuGet パッケージを更新すると、**パッケージコンソール**ウィンドウに次のエラーが表示されることがあります。

```csharp
Attempting to resolve dependency 'Xamarin.Android.Support.v7.AppCompat (= 23.3.0.0)'.
Attempting to resolve dependency 'Xamarin.Android.Support.v4 (= 23.3.0.0)'.
Looking for updates for 'Xamarin.Android.Support.v7.MediaRouter'...
Updating 'Xamarin.Android.Support.v7.MediaRouter' from version '23.3.0.0' to '23.3.1.0' in project 'Todo.Droid'.
Updating 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0' to 'Xamarin.Android.Support.v7.MediaRouter 23.3.1.0' failed.
Unable to find a version of 'Xamarin.Forms' that is compatible with 'Xamarin.Android.Support.v7.MediaRouter 23.3.0.0'.
```

### <a name="what-causes-this-error"></a>このエラーの原因

Visual Studio for Mac (または Visual Studio) は、Xamarin. Forms NuGet パッケージ*とそのすべての依存関係*について、更新プログラムが利用可能であることを示している可能性があります。 Xamarin Studio では、ソリューションの **[パッケージ]** ノードは次のようになります (バージョン番号は異なる場合があります)。

![](images/updates-available.png "Android Project Packages Folder")

このエラーは、_すべて_のパッケージを更新しようとした場合に発生する可能性があります。

これは、android プロジェクトが Android 6.0 (API 23) 以降のターゲット/コンパイルバージョンに設定されている場合、Xamarin. Forms は Android サポートパッケージの*特定*のバージョンに対してハードな依存関係を持つためです。 これらのパッケージの更新バージョンが使用可能な場合もありますが、Xamarin. Forms は必ずしも互換性がありません。

この場合は、互換性のあるバージョンで依存関係が維持されるように、 **Xamarin. Forms**パッケージ_のみ_を更新する必要があります。 プロジェクトに追加した他のパッケージは、Android サポートパッケージが更新されない限り、個別に更新することもできます。

> [!NOTE]
> Xamarin 2.3.4 以上を**使用して**いて、android プロジェクトのターゲット/コンパイルバージョンが android 7.0 (API 24) 以上に設定されている場合、上記のハード依存関係は適用されなくなり、Xamarin. Forms パッケージとは別にサポートパッケージを更新できます。

### <a name="fix-remove-all-packages-and-re-add-xamarinforms"></a>修正: すべてのパッケージを削除し、Xamarin. フォームを再度追加します。

互換性の**ない**バージョンに更新された場合、最も簡単な解決策は次のとおりです。

1. Android プロジェクトのすべての NuGet パッケージを手動で削除します。
2. **Xamarin. Forms**パッケージを再度追加します。

これにより、他のパッケージの*正しい*バージョンが自動的にダウンロードされます。

このプロセスのビデオについては、こちらの[フォーラム投稿](https://forums.xamarin.com/discussion/comment/170012/#Comment_170012)を参照してください。
