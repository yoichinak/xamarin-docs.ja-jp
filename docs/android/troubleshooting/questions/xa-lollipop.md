---
title: Lollipop のサポートが追加された Xamarin.Android のバージョンを教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 4fe1bd4dda9a54eb3a1692f07d1069adb39345cb
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69523293"
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>Lollipop のサポートが追加された Xamarin.Android のバージョンを教えてください

> [!NOTE]
> このガイドは、当初は Android L preview 用に書かれています。

- [Xamarin android 4.17](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_4/xamarin.android_4.17/index.md) Android L Preview サポートが追加されました。
- [Xamarin android 4.20](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_4/xamarin.android_4.20/index.md) android ロリポップのサポートが追加されました。

Xamarin では、Xamarin ツールの現在の安定したリリースのみがアクティブにサポートされています。 以前のバージョンのツールでは、以下の情報が "その他" として提供されています。 Xamarin のリリースに関する最新情報については、[こちら](http://releases.xamarin.com/)をご覧ください。

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>Android L Preview の "API レベル21の android .jar がありません"

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

次のエラーメッセージ (または同様) が表示される場合があります。

```cmd
Error 1 Could not find android.jar for API Level 21.
```

このメッセージは、API レベル21の Android SDK プラットフォームがインストールされていないことを意味します。 Android SDK Manager (**ツール > Android SDK Manager...** ) にインストールするか、インストールされている API バージョンをターゲットとするように Xamarin Android プロジェクトを変更します。

この問題には、いくつかの回避策があります。

1. API 19 以下を対象とするようにプロジェクトを変更します。

2. Android-21 フォルダーの名前を「android-21」から「android-L」に変更します。 (これは、一時的な修正としてのみ使用する必要があり、まったくうまく機能しない可能性があります)。

   **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**

3. Android API レベル 21 "L" preview [1] に一時的にダウングレードします。

    1. 削除、 **%localappdata%\\Android\\android sdk\\platforms\\android 21** 
    2. [1] を**C\\: Users\\\\\\ユーザー名&gt;AppData AppData ローカルAndroid\\androidsdk\\プラットフォームを作成する\\&lt;** **android-L**フォルダー。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

次のエラーメッセージ (または同様) が表示される場合があります。

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

これは、API レベル21の Android SDK プラットフォームがインストールされていないことを意味します。Android SDK Manager (ツール > SDK Manager...) にインストールするか、インストールされている API バージョンをターゲットとするように Xamarin Android プロジェクトを変更します。

この問題には、いくつかの回避策があります。

1. API 19 以下を対象とするようにプロジェクトを変更します。

2. Android-21 フォルダーの名前を「android-21」から「android-L」に変更します。 (これは、一時的な修正としてのみ使用する必要があり、まったくうまく機能しない可能性があります)。

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. Android API レベル 21 "L" preview [1] に一時的にダウングレードします。

    1. **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21**の削除
    2. [1] を **/Users/username/Library/Developer/Xamarin/android-sdk-macosx**に抽出して、 **android-L**フォルダーを作成します。

-----


[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
