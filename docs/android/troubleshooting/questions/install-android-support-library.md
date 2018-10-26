---
title: どのように手動でインストールできる Xamarin.Android.Support パッケージによって必要な Android サポート ライブラリ
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 84ee33fe174c01656144e55bc3cbba7c773950fd
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120646"
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>どのように手動でインストールできる Xamarin.Android.Support パッケージによって必要な Android サポート ライブラリ

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Xamarin.Android.Support.v4 の手順例 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

(たとえば、NuGet パッケージ マネージャーにインストールする) を目的の Xamarin.Android.Support NuGet パッケージをダウンロードします。

使用`ildasm`のバージョンを確認する**android_m2repository.zip** NuGet パッケージが必要があります。

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```
出力例:

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

ダウンロード**android\_m2repository.zip**から Google から URL を使用して返された**ildasm**します。 またはのバージョンを調べることができます、 _Android Support Repository_ Android SDK Manager でインストール済み。

!["Android SDK Manager が示す Android Support Repository バージョンがインストールされている 32"](install-android-support-library-images/sdk-extras.png)

バージョンが必要な NuGet パッケージの 1 つに一致する場合は、新しいものを何でもダウンロードする必要はありません。 希望する設定が代わりに再既存**m2repository**ディレクトリの下にある**extras\\android**で、 _SDK のパス_(Android の上部に示すようにSDK マネージャー ウィンドウ)。

返された URL の MD5 ハッシュを計算する**ildasm**します。 すべて大文字の文字とスペースを使用する結果の文字列の書式を設定します。 たとえば、調整、`$url`として変数が必要し、次の 2 行を実行 (に基づいて[元C#Xamarin.Android からコード](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)) PowerShell で。

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```
出力例:

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

コピー **android\_m2repository.zip**に、 **%localappdata%\\Xamarin\\zips\\** フォルダー。 前の MD5 ハッシュが計算ステップからの MD5 ハッシュを使用するファイルの名前を変更します。 例えば:

**%LOCALAPPDATA%\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

(省略可能)ファイルを解凍 **%localappdata%\\Xamarin\\Xamarin.Android.Support.v4\\23.4.0.0\\コンテンツ\\** (作成、**コンテンツ\\m2repository**サブディレクトリ)。 この手順をスキップする場合、ライブラリを使用する最初のビルドは少し長くかかるため、この手順を完了する必要があります。
サブディレクトリのバージョン番号 (**23.4.0.0**この例では)、NuGet パッケージのバージョンとまったく同じではありません。 使用することができます`ildasm`適切なバージョン番号を検索します。

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```
出力例:

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

(たとえば、NuGet パッケージ マネージャーにインストールする) を目的の Xamarin.Android.Support NuGet パッケージをダウンロードします。

ダブルクリックして、 _Xamarin.Android.Support.v4_の下のアセンブリ、_参照_Visual Studio for Mac は、アセンブリ ブラウザーでアセンブリを開くことで Android プロジェクトのセクション。 いることを確認、_言語_ドロップダウンに設定されている_C#_ 、最上位レベルの選択と_Xamarin.Android.Support.v4_アセンブリ ブラウザー ナビゲーション ツリーからアセンブリ. 検索、`SourceUrl`プロパティのいずれかで、`IncludeAndroidResourcesFrom`または`JavaLibraryReference`属性。

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

ダウンロード**android\_m2repository.zip** Google を使用してから、`SourceUrl`から返された**ildasm**します。 またはのバージョンを調べることができます、 _Android Support Repository_ Android SDK Manager でインストール済み。

!["Android SDK Manager が示す Android Support Repository バージョンがインストールされている 32"](install-android-support-library-images/sdk-extras.png)

バージョンが必要な NuGet パッケージの 1 つに一致する場合は、新しいものを何でもダウンロードする必要はありません。 代わりに再 zip 圧縮する既存の**m2repository**ディレクトリの下にある**extras/android**で、 _SDK パス_(ように、Android SDK マネージャー ウィンドウの上部).

返された URL の MD5 ハッシュを計算する**ildasm**します。 すべて大文字の文字とスペースを使用する結果の文字列の書式を設定します。 たとえば、必要に応じて、URL 文字列を調整しで、次のコマンドを実行する**Terminal.app**コマンド プロンプト。

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

使用することも、`csharp`インタープリターを実行する[同じC#自体 Xamarin.Android を使用するコード](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)します。
調整、`url`として変数が必要なし、で、次のコマンドを実行する**Terminal.app**コマンド プロンプト。

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```
出力例:

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

コピー **android\_m2repository.zip**を **$HOME/.local/share/Xamarin/zips/** フォルダー。 前の MD5 ハッシュが計算ステップからの MD5 ハッシュを使用するファイルの名前を変更します。 例えば:

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

(省略可能)ファイルを解凍します。 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

(作成、**コンテンツ/m2repository**サブディレクトリ)。 この手順をスキップする場合、ライブラリを使用する最初のビルドは少し長くかかるため、この手順を完了する必要があります。

サブディレクトリのバージョン番号 (**23.4.0.0**この例では)、NuGet パッケージのバージョンとまったく同じではありません。 **Ildasm**前の手順、適切なバージョン番号を検索する Visual Studio for Mac で、アセンブリ ブラウザーを使用することができます。 探して、`Version`プロパティのいずれかで、`IncludeAndroidResourcesFrom`または`JavaLibraryReference`属性。

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----


## <a name="additional-references"></a>その他の参照

- [バグ 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) – Inaccurate"ダウンロードに失敗しました。 ダウンロードしてください{0}言うと、{1}ディレクトリ"。 "パッケージをインストールしてください: '{0}' SDK インストーラーで使用可能な"Xamarin.Android.Support パッケージに関連するエラー メッセージ

### <a name="next-steps"></a>次の手順

このドキュメントでは、2016 年 8 月の時点では、現在の動作について説明します。 このドキュメントで説明した手法は、Xamarin の安定したテスト スイートの一部ではないが、今後中断ため。

問い合わせ、または上記の情報を使用した後でもこの問題が残っている場合を参照してください、詳細については[Xamarin のどのようなサポート オプションを使用しますか?](~/cross-platform/troubleshooting/support-options.md)連絡先オプション、推奨事項は、についてする方法についても必要な場合は、新しいバグをファイルします。

