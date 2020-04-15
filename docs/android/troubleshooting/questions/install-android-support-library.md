---
title: Xamarin.Android.Support パッケージに必要な Android サポート ライブラリを手動でインストールする方法を教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 99571e0b62592597bb1fffdc8d3ed8336fe050b2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73026924"
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>Xamarin.Android.Support パッケージに必要な Android サポート ライブラリを手動でインストールする方法を教えてください

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Xamarin.Android.Support.v4 の手順の例 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

必要な Xamarin.Android.Support NuGet パッケージをダウンロードします (たとえば、NuGet パッケージ マネージャーによるインストール)。

`ildasm` を使用して、NuGet パッケージに必要な **android_m2repository.zip** のバージョンを確認します。

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```

出力例:

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

**ildasm** から返された URL を使用し、Google から **android\_m2repository.zip** をダウンロードします。 または、現在 Android SDK マネージャーにインストールされている _Android Support Repository_ のバージョンを確認することもできます。

!["Android Support Repository バージョン 32 がインストールされていることを示す Android SDK マネージャー"](install-android-support-library-images/sdk-extras.png)

バージョンが NuGet パッケージに必要なものと一致する場合、新しいものをダウンロードする必要はありません。 代わりに、"_SDK パス_" (Android SDK マネージャー ウィンドウの上部に表示されています) の **extras\\android** にある既存の **m2repository** ディレクトリを再 zip することができます。

**ildasm** から返された URL の MD5 ハッシュを計算します。 結果の文字列の書式を、すべて大文字で、スペースが含まれないように設定します。 たとえば、必要に応じて `$url` 変数を調整した後、PowerShell で次の 2 行を実行します ([Xamarin.Android からの元の C# コード](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)に基づきます)。

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```

出力例:

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

**android\_m2repository.zip** を **%LOCALAPPDATA%\\Xamarin\\zips\\** フォルダーにコピーします。 前の MD5 ハッシュ計算ステップの MD5 ハッシュを使用するように、ファイルの名前を変更します。 次に例を示します。

**%LOCALAPPDATA%\\Xamarin\\zips\\F16A3455987DBAE5783F058F19F7FCDF.zip**

(省略可能) ファイルを **%LOCALAPPDATA%\\Xamarin\\Xamarin.Android.Support.v4\\23.4.0.0\\content\\** に解凍します (**content\\m2repository** サブディレクトリを作成します)。 このステップを省略した場合、ライブラリを使用する最初のビルドは、このステップを完了する必要があるため、少し時間がかかります。
サブディレクトリのバージョン番号 (この例では **23.4.0.0**) は、NuGet パッケージのバージョンとまったく同じではありません。 `ildasm` を使用して、正しいバージョン番号を調べることができます。

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```

出力例:

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

必要な Xamarin.Android.Support NuGet パッケージをダウンロードします (たとえば、NuGet パッケージ マネージャーによるインストール)。

Visual Studio for Mac の Android プロジェクトの _[参照]_ セクションにある _Xamarin.Android.Support.v4_ アセンブリをダブルクリックして、アセンブリ ブラウザーでアセンブリを開きます。 _[言語]_ ドロップダウンが _[C#]_ に設定されていることを確認し、アセンブリ ブラウザーのナビゲーション ツリーから最上位の _Xamarin.Android.Support.v4_ アセンブリを選択します。 `IncludeAndroidResourcesFrom` 属性または `JavaLibraryReference` 属性のいずれかで、`SourceUrl` プロパティを見つけます。

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

**ildasm** から返された `SourceUrl` を使用して、Google から **android\_m2repository.zip** をダウンロードします。 または、現在 Android SDK マネージャーにインストールされている _Android Support Repository_ のバージョンを確認することもできます。

!["Android Support Repository バージョン 32 がインストールされていることを示す Android SDK マネージャー"](install-android-support-library-images/sdk-extras.png)

バージョンが NuGet パッケージに必要なものと一致する場合、新しいものをダウンロードする必要はありません。 代わりに、"_SDK パス_" (Android SDK マネージャー ウィンドウの上部に表示されています) の **extras/android** にある既存の **m2repository** ディレクトリを再 zip することができます。

**ildasm** から返された URL の MD5 ハッシュを計算します。 結果の文字列の書式を、すべて大文字で、スペースが含まれないように設定します。 たとえば、必要に応じて URL 文字列を調整し、**Terminal.app** コマンド プロンプトで次のコマンドを実行します。

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

別の方法としては、`csharp` インタープリターを使用して、[Xamarin.Android 自体が使用するのと同じ C# コード](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)を実行します。
そのためには、必要に応じて `url` 変数を調整し、**Terminal.app** コマンド プロンプトで次のコマンドを実行します。

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```

出力例:

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

**android\_m2repository.zip** を **$HOME/.local/share/Xamarin/zips/** フォルダーにコピーします。 前の MD5 ハッシュ計算ステップの MD5 ハッシュを使用するように、ファイルの名前を変更します。 次に例を示します。

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

(省略可能) ファイルを次の場所に解凍します。 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

(**content/m2repository** サブディレクトリを作成します)。 このステップを省略した場合、ライブラリを使用する最初のビルドは、このステップを完了する必要があるため、少し時間がかかります。

サブディレクトリのバージョン番号 (この例では **23.4.0.0**) は、NuGet パッケージのバージョンとまったく同じではありません。 前の **ildasm.exe** のステップと同様に、Visual Studio for Mac のアセンブリ ブラウザーを使用して、正しいバージョン番号を見つけることができます。 `IncludeAndroidResourcesFrom` 属性または `JavaLibraryReference` 属性のいずれかで、`Version` プロパティを探します。

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----

## <a name="additional-references"></a>その他のリファレンス

- [バグ 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) - 正しくない "ダウンロードに失敗しました。 {0} をダウンロードし、{1} ディレクトリに配置してください。" および "SDK インストーラーで利用可能なパッケージ: '{0}' をインストールしてください" という、Xamarin.Android.Support に関連するエラーメッセージ

### <a name="next-steps"></a>次の手順

このドキュメントでは、2016 年 8 月時点での現在の動作について説明されています。 このドキュメントで説明されている手法は、Xamarin の安定したテスト スイートの一部ではないため、将来は使用できない可能性があります。

詳細については、お問い合わせください。または、上記の情報を利用してもこの問題が解決しない場合は、[Xamarin で利用できるサポート オプション](~/cross-platform/troubleshooting/support-options.md)に関する記事で、連絡オプション、提案、および必要に応じて新しいバグを登録する方法を参照してください。
