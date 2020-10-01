---
title: Lollipop のサポートが追加された Xamarin.Android のバージョンを教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 27631d19ea157c1af08666b55a067e729b806434
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453637"
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>Lollipop のサポートが追加された Xamarin.Android のバージョンを教えてください

> [!NOTE]
> このガイドは、もともと Android L Preview 用に書かれたものです。

- [Xamarin.Android 4.17](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_4/xamarin.android_4.17/index.md) で Android L Preview のサポートが追加されました。
- [Xamarin.Android 4.20](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_4/xamarin.android_4.20/index.md) で Android Lollipop のサポートが追加されました。

Xamarin では、Xamarin ツールの現在の安定したリリースのみがアクティブにサポートされています。 以下の情報は、以前のバージョンのツールについて "そのまま" 提供されています。 Xamarin リリースの最新情報については、[リリース ノート](/xamarin/android/release-notes/)を確認してください。

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>Android L Preview には "API レベル 21 用の android.jar がない"

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

次のエラー メッセージ (または似たもの) が表示される場合があります。

```cmd
Error 1 Could not find android.jar for API Level 21.
```

このメッセージは、API レベル 21 用の Android SDK プラットフォームがインストールされていないことを意味します。 Android SDK マネージャーでインストールするか ( **[ツール] > [Android SDK マネージャーを開く...]** )、Xamarin.Android プロジェクトのターゲットをインストールされている API バージョンに変更します。

この問題には、いくつかの回避策があります。

1. API 19 以下を対象とするようにプロジェクトを変更します。

2. android-21 フォルダーの名前を、"android-21" から "android-L" に変更します。 (これは一時的な修正としてのみ使用する必要があり、まったくうまく機能しない可能性があります)。

   **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**

3. Android API レベル 21 "L" Preview に一時的にダウングレードします。[1]

    1. **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21** を削除しますｊ 
    2. [1] を **C:\\Users\\&lt;username&gt;\\AppData\\Local\\Android\\android-sdk\\platforms** に抽出して、**android-L** フォルダーを作成します。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

次のエラー メッセージ (または似たもの) が表示される場合があります。

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

これは、API レベル 21 用の Android SDK プラットフォームがインストールされていないことを意味します。Android SDK マネージャーでインストールするか ([ツール] > [Android SDK...])、Xamarin.Android プロジェクトのターゲットをインストールされている API バージョンに変更します。

この問題には、いくつかの回避策があります。

1. API 19 以下を対象とするようにプロジェクトを変更します。

2. android-21 フォルダーの名前を、"android-21" から "android-L" に変更します。 (これは一時的な修正としてのみ使用する必要があり、まったくうまく機能しない可能性があります)。

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. Android API レベル 21 "L" Preview に一時的にダウングレードします。[1]

    1. **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21** を削除します
    2. [1] を **/Users/username/Library/Developer/Xamarin/android-sdk-macosx** に抽出して、**android-L** フォルダーを作成します。

-----

[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)