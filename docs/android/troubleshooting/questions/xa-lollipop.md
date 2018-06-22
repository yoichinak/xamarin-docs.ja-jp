---
title: Xamarin.Android のバージョンでは、ロリポップ サポートを追加しますか。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 065c68a373f67bb352b59dc88ef89daec8b51ef8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764470"
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>Xamarin.Android のバージョンでは、ロリポップ サポートを追加しますか。

**注:** Android L プレビューではこのガイドを最初に記述します。

-   [Xamarin.Android 4.17](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.17/) Android L プレビュー サポートが追加されました。
-   [Xamarin.Android 4.20](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.20/) Android ロリポップ サポートが追加されました。

Xamarin には、Xamarin ツールの現在の安定したリリースのみアクティブにサポートしています。 以下の情報が提供される"としてでは"ツールの古いバージョンのです。 Xamarin のリリースに関する最新の情報を確認してください。[ここ](http://releases.xamarin.com/)です。

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>「が見つかりません」android.jar API レベル 21 の Android L プレビュー

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

次のエラー メッセージ (または同等の) を示します。

```cmd
Error 1 Could not find android.jar for API Level 21.
```

このメッセージは、API レベル 21 用の Android SDK プラットフォームがインストールされていないことを意味します。 Android SDK Manager をインストールするか (ツール > 開く Android SDK Manager を...)、またはインストールされている API バージョンを対象とする Xamarin.Android プロジェクトを変更します。

この問題のいくつかの回避策があります。

1. API 19 を対象に、プロジェクトを変更または削減します。

2. Android L. android 21 から、android 21 フォルダーの名前を変更します。 (これは一時的な修正としてのみ使用する必要がありますおよび機能するわけでも非常にまったく最高でします。)

   **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**

3. Android API レベル 21"L"プレビュー [1] に一時的にダウン グレードします。

    1.  削除、 **%localappdata%\\Android\\android sdk\\プラットフォーム\\android 21** 
    2.  [1] に抽出**c:\\ユーザー\\<username>\\AppData\\ローカル\\Android\\android sdk\\プラットフォーム**を作成します。**android-l**フォルダーです。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

次のエラー メッセージ (または同等の) を示します。

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

これは、API レベル 21 用の Android SDK プラットフォームがインストールされていないことを意味します。Android SDK Manager をインストールするか (ツール > SDK Manager...)、またはインストールされている API バージョンを対象とする Xamarin.Android プロジェクトを変更します。

この問題のいくつかの回避策があります。

1. API 19 を対象に、プロジェクトを変更または削減します。

2. Android L. android 21 から、android 21 フォルダーの名前を変更します。 (これは一時的な修正としてのみ使用する必要がありますおよび機能するわけでも非常にまったく最高でします。)

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. Android API レベル 21"L"プレビュー [1] に一時的にダウン グレードします。

    1.  削除 **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21**
    2.  [1] に抽出 **/Users/username/Library/Developer/Xamarin/android-sdk-macosx**を作成する、 **android-l**フォルダーです。

-----


[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
