---
title: Lollipop のサポートが追加された Xamarin.Android のバージョンを教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 7e31f9ad46a04b648a6a1f24c075426f7d98a663
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61228261"
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>Lollipop のサポートが追加された Xamarin.Android のバージョンを教えてください

**注:** このガイドは、Android L プレビュー用に作成されたでした。

-   [Xamarin.Android 4.17](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.17/) Android L プレビューのサポートが追加されました。
-   [Xamarin.Android 4.20](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.20/) Android Lollipop のサポートが追加されました。

Xamarin には、現在の安定したリリースの Xamarin ツールのみアクティブにサポートしています。 以下の情報が提供される"として-は、"ツールの以前のバージョン。 Xamarin のリリースに関する最新情報については、確認[ここ](http://releases.xamarin.com/)します。

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>「が見つかりません android.jar API レベル 21」Android L プレビューに

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

次のエラー メッセージ (または同等) を示します。

```cmd
Error 1 Could not find android.jar for API Level 21.
```

このメッセージは、API レベル 21 用、Android SDK プラットフォームがインストールされていないことを意味します。 Android SDK Manager をインストールするか (**ツール > Android SDK マネージャーを開く.**)、またはインストールされている API バージョンを対象とする Xamarin.Android プロジェクトを変更します。

この問題のいくつかの回避策があります。

1. API 19 を対象にするために、プロジェクトを変更または削減します。

2. Android-21 フォルダーを android 21 から android L. に名前を変更します。 (せいぜい、これは一時的な対策としてのみ使用する必要があり、機能するわけでも非常にまったく)。

   **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**

3. Android API レベル 21"L"プレビュー [1] に一時的にダウン グレードします。

    1.  削除、 **%localappdata%\\Android\\android sdk\\platforms\\android 21** 
    2.  [1] に抽出**c:\\ユーザー\\&lt;username&gt;\\AppData\\ローカル\\Android\\android sdk\\プラットフォーム**を作成する、 **android-l**フォルダー。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

次のエラー メッセージ (または同等) を示します。

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

これは、API レベル 21 用、Android SDK プラットフォームがインストールされていないことを意味します。Android SDK Manager をインストールするか (ツール > SDK マネージャー...)、またはインストールされている API バージョンを対象とする Xamarin.Android プロジェクトを変更します。

この問題のいくつかの回避策があります。

1. API 19 を対象にするために、プロジェクトを変更または削減します。

2. Android-21 フォルダーを android 21 から android L. に名前を変更します。 (せいぜい、これは一時的な対策としてのみ使用する必要があり、機能するわけでも非常にまったく)。

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. Android API レベル 21"L"プレビュー [1] に一時的にダウン グレードします。

    1.  Delete **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21**
    2.  [1] に抽出 **/Users/username/Library/Developer/Xamarin/android-sdk-macosx**を作成する、 **android-l**フォルダー。

-----


[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
