---
title: "どのように手動でインストールできます Xamarin.Android.Support パッケージで必要な Android サポート ライブラリか。"
ms.topic: article
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 26dd7e23352bf0911c2a7268518ddebf6626596a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>どのように手動でインストールできます Xamarin.Android.Support パッケージで必要な Android サポート ライブラリか。

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Xamarin.Android.Support.v4 の手順例 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

(たとえば、NuGet package manager にインストールする) を必要な Xamarin.Android.Support NuGet パッケージをダウンロードします。

使用して`ildasm`のバージョンをチェックする**android_m2repository.zip** NuGet パッケージが必要があります。

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```
出力例:

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

ダウンロード**android\_m2repository.zip** Google から URL を使用してから返された**ildasm**です。 またはのバージョンを調べることができます、 _Android サポート リポジトリ_が現在インストールされている Android SDK Manager で。

!["Android SDK Manager の Android サポート リポジトリ 32 がインストールされているバージョンを示す"](install-android-support-library-images/sdk-extras.png)

バージョンには、NuGet パッケージに必要な 1 つが一致すると、新しいものを何でもダウンロードするがありません。 希望する設定が代わりに再既存**m2repository**ディレクトリの下にある**extras\\android**で、 _SDK パス_(ように、Android の上部SDK Manager ウィンドウ)。

返される URL の MD5 ハッシュを計算する**ildasm**です。 すべての大文字とスペースを使用する結果の文字列の書式を設定します。 たとえば、調整、`$url`として変数が必要し、次の 2 行を実行 (に基づいて[Xamarin.Android から元の c# コード](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)) PowerShell で。

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```
出力例:

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

コピー **android\_m2repository.zip**に、 **%localappdata%\\Xamarin\\圧縮\\**フォルダーです。 ステップの計算前の MD5 ハッシュから MD5 ハッシュを使用するファイルの名前を変更します。 例:

**%LOCALAPPDATA%\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

(省略可能)ファイルを解凍**%localappdata%\\Xamarin\\Xamarin.Android.Support.v4\\23.4.0.0\\コンテンツ\\** (作成、**コンテンツ\\m2repository**サブディレクトリ)。 この手順をスキップする場合、ライブラリを使用する最初のビルドは少し長くかかるため、この手順を完了する必要があります。
サブディレクトリのバージョン番号 (**23.4.0.0**この例では)、NuGet パッケージのバージョンとまったく同じではありません。 使用することができます`ildasm`正しいバージョン番号を検索します。

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```
出力例:

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

(たとえば、NuGet package manager にインストールする) を必要な Xamarin.Android.Support NuGet パッケージをダウンロードします。

ダブルクリックして、 _Xamarin.Android.Support.v4_下にあるアセンブリ、_参照_Mac アセンブリ ブラウザーでアセンブリを開くには Visual Studio で Android プロジェクトのセクションです。 いることを確認、_言語_ドロップダウンに設定されている_c#_最上位レベルを選択して_Xamarin.Android.Support.v4_アセンブリ ブラウザー ナビゲーション ツリーからのアセンブリ。 検索、`SourceUrl`プロパティのいずれかで、`IncludeAndroidResourcesFrom`または`JavaLibraryReference`属性。

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

ダウンロード**android\_m2repository.zip** Google を使用してから、`SourceUrl`から返された**ildasm**です。 またはのバージョンを調べることができます、 _Android サポート リポジトリ_が現在インストールされている Android SDK Manager で。

!["Android SDK Manager の Android サポート リポジトリ 32 がインストールされているバージョンを示す"](install-android-support-library-images/sdk-extras.png)

バージョンには、NuGet パッケージに必要な 1 つが一致すると、新しいものを何でもダウンロードするがありません。 希望する設定が代わりに再既存**m2repository**ディレクトリの下にある**extras/android**で、 _SDK パス_(示すように Android SDK Manager ウィンドウの上部).

返される URL の MD5 ハッシュを計算する**ildasm**です。 すべての大文字とスペースを使用する結果の文字列の書式を設定します。 たとえば、必要に応じて、URL 文字列を調整しで、次のコマンドを実行、 **Terminal.app**コマンド プロンプト。

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

別のオプションは、使用する、`csharp`を実行するインタープリター[同じ c# コード自体 Xamarin.Android を使用する](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)です。
実行するには、調整、`url`として変数が必要なし、で、次のコマンドを実行、 **Terminal.app**コマンド プロンプト。

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```
出力例:

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

コピー **android\_m2repository.zip**を**$HOME/.local/share/Xamarin/zips/**フォルダーです。 ステップの計算前の MD5 ハッシュから MD5 ハッシュを使用するファイルの名前を変更します。 例:

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

(省略可能)ファイルを解凍します。 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

(作成、**コンテンツ/m2repository**サブディレクトリ)。 この手順をスキップする場合、ライブラリを使用する最初のビルドは少し長くかかるため、この手順を完了する必要があります。

サブディレクトリのバージョン番号 (**23.4.0.0**この例では)、NuGet パッケージのバージョンとまったく同じではありません。 同様、 **ildasm**前のステップ、正しいバージョン番号を検索する、Visual Studio for Mac でアセンブリ ブラウザーを使用することができます。 探して、`Version`プロパティのいずれかで、`IncludeAndroidResourcesFrom`または`JavaLibraryReference`属性。

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----


## <a name="additional-references"></a>その他のリファレンス

- [バグ 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) – Inaccurate"ダウンロードに失敗しました。 {0} をダウンロードしてください {1} ディレクトリに配置します。" "パッケージをインストールしてください: '{0}' の SDK インストーラーに利用可能な"Xamarin.Android.Support パッケージに関連するエラー メッセージ

### <a name="next-steps"></a>次の手順

このドキュメントでは、2016 年 8 月の時点で現在の動作について説明します。 このドキュメントで説明した手法は、Xamarin の安定したテスト スイートの一部ではないが、今後分割ため。

詳細については、問い合わせ先、または、上記の情報を使用した後もこの問題が残っている場合を参照してください[どのようなサポート オプションは、Xamarin を使用しますか?](~/cross-platform/troubleshooting/support-options.md)推奨事項は、の連絡先のオプションについて方法だけでなく必要な場合は、新しいバグをファイルです。

